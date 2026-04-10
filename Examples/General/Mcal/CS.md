---
title: "Mcal"
description: "This example demonstrates the usage of the Calibration module."
ms.language: "C#"
---

# Mcal

> This example demonstrates the usage of the Calibration module.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `McalAlloc`, `McalAssociate`, `McalControl`, `McalDraw`, `McalFree`, `McalGetCoordinateSystem`, `McalGrid`, `McalInquire`, `McalSetCoordinateSystem`, `McalTransformImage`, `McalUniform`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraControl`, `MgraFree`, `MgraText`, `MmeasAllocMarker`, `MmeasDraw`, `MmeasFindMarker`, `MmeasFree`, `MmeasGetResult`, `MmeasSetMarker`, `MmeasSetScore`, `MmetAddFeature`, `MmetAlloc`, `MmetAllocResult`, `MmetCalculate`, `MmetDraw`, `MmetFree`, `MmetGetResult`, `MmetSetRegion`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Machining, Applications, Measuring, Modules, Buffer, Display, Graphics, Measurement, Metrology, Calibration, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MCal.cs
//
// Description: This program uses the Calibration module to:
//              - Remove distortion and then take measurements in world units using a 2D 
//                calibration.
//              - Perform a 3D calibration to take measurements at several known elevations.
//              - Calibrate a scene using a partial calibration grid that has a 2D code 
//                fiducial.
//
//              Printable calibration grids in PDF format can be found in your
//              "Aurora Imaging Library/##/Images/" directory.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace Mcal
{
    class Program
    {
        // Example selection.
        private const int RUN_LINEAR_CALIBRATION_EXAMPLE = AIL.M_YES;
        private const int RUN_TSAI_CALIBRATION_EXAMPLE = AIL.M_YES;
        private const int RUN_PARTIAL_GRID_CALIBRATION_EXAMPLE = AIL.M_YES;

        // Grid offset specifications.
        private const int GRID_OFFSET_X = 0;
        private const int GRID_OFFSET_Y = 0;
        private const int GRID_OFFSET_Z = 0;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL; // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;      // System Identifier.
            AIL_ID AilDisplay = AIL.M_NULL;     // Display identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Print module name.
            Console.WriteLine("CALIBRATION MODULE:");
            Console.WriteLine("-------------------");
            Console.WriteLine();

            if (RUN_LINEAR_CALIBRATION_EXAMPLE == AIL.M_YES)
            {
                LinearInterpolationCalibration(AilSystem, AilDisplay);
            }

            if (RUN_TSAI_CALIBRATION_EXAMPLE == AIL.M_YES)
            {
                TsaiCalibration(AilSystem, AilDisplay);
            }

            if (RUN_PARTIAL_GRID_CALIBRATION_EXAMPLE == AIL.M_YES)
            {
                PartialGridCalibration(AilSystem, AilDisplay);
            }

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        //****************************************************************************
        // Linear interpolation example. 
        //****************************************************************************

        // Source image files specification.
        private const string GRID_IMAGE_FILE = AIL.M_IMAGE_PATH + "CalGrid.mim";
        private const string BOARD_IMAGE_FILE = AIL.M_IMAGE_PATH + "CalBoard.mim";

        // World description of the calibration grid.
        private const int GRID_ROW_SPACING = 1;
        private const int GRID_COLUMN_SPACING = 1;
        private const int GRID_ROW_NUMBER = 18;
        private const int GRID_COLUMN_NUMBER = 25;

        // Measurement boxes specification.
        private const int MEAS_BOX_POS_X1 = 55;
        private const int MEAS_BOX_POS_Y1 = 24;
        private const int MEAS_BOX_WIDTH1 = 7;
        private const int MEAS_BOX_HEIGHT1 = 425;

        private const int MEAS_BOX_POS_X2 = 225;
        private const int MEAS_BOX_POS_Y2 = 11;
        private const int MEAS_BOX_WIDTH2 = 7;
        private const int MEAS_BOX_HEIGHT2 = 450;

        // Specification of the stripes' constraints.
        private const int WIDTH_APPROXIMATION = 410;
        private const int WIDTH_VARIATION = 25;
        private const int MIN_EDGE_VALUE = 5;

        static void LinearInterpolationCalibration(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;          // Image buffer identifier.
            AIL_ID AilOverlayImage = AIL.M_NULL;   // Overlay image.
            AIL_ID AilCalibration = AIL.M_NULL;    // Calibration identifier.
            AIL_ID MeasMarker1 = AIL.M_NULL;       // Measurement marker identifier.
            AIL_ID MeasMarker2 = AIL.M_NULL;       // Measurement marker identifier.
            double WorldDistance1 = 0.0;
            double WorldDistance2 = 0.0;
            double PixelDistance1 = 0.0;
            double PixelDistance2 = 0.0;
            double PosX1 = 0.0;
            double PosY1 = 0.0;
            double PosX2 = 0.0;
            double PosY2 = 0.0;
            double PosX3 = 0.0;
            double PosY3 = 0.0;
            double PosX4 = 0.0;
            double PosY4 = 0.0;
            AIL_INT CalibrationStatus = 0;

            // Clear the display.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            // Restore source image into an automatically allocated image buffer.
            AIL.MbufRestore(GRID_IMAGE_FILE, AilSystem, ref AilImage);

            // Display the image buffer.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Prepare for overlay annotation.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            // Pause to show the original image.
            Console.WriteLine();
            Console.WriteLine("LINEAR INTERPOLATION CALIBRATION:");
            Console.WriteLine("---------------------------------");
            Console.WriteLine();
            Console.WriteLine("The displayed grid has been grabbed with a high distortion");
            Console.WriteLine("camera and will be used to calibrate the camera.");
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Allocate a camera calibration context.
            AIL.McalAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilCalibration);

            // Calibrate the camera with the image of the grid and its world description.
            AIL.McalGrid(AilCalibration, AilImage, GRID_OFFSET_X, GRID_OFFSET_Y, GRID_OFFSET_Z, GRID_ROW_NUMBER, GRID_COLUMN_NUMBER, GRID_ROW_SPACING, GRID_COLUMN_SPACING, AIL.M_DEFAULT, AIL.M_DEFAULT);

            AIL.McalInquire(AilCalibration, AIL.M_CALIBRATION_STATUS + AIL.M_TYPE_AIL_INT, ref CalibrationStatus);
            if (CalibrationStatus == AIL.M_CALIBRATED)
            {
                // Perform a first image transformation with the calibration grid.
                AIL.McalTransformImage(AilImage, AilImage, AilCalibration, AIL.M_BILINEAR | AIL.M_OVERSCAN_CLEAR, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Pause to show the corrected image of the grid.
                Console.WriteLine("The camera has been calibrated and the image of the grid");
                Console.WriteLine("has been transformed to remove its distortions.");
                Console.WriteLine("Press any key to continue.");
                Console.WriteLine();
                Console.ReadKey(true);

                // Read the image of the board and associate the calibration to the image.
                AIL.MbufLoad(BOARD_IMAGE_FILE, AilImage);
                AIL.McalAssociate(AilCalibration, AilImage, AIL.M_DEFAULT);

                // Allocate the measurement markers.
                AIL.MmeasAllocMarker(AilSystem, AIL.M_STRIPE, AIL.M_DEFAULT, ref MeasMarker1);
                AIL.MmeasAllocMarker(AilSystem, AIL.M_STRIPE, AIL.M_DEFAULT, ref MeasMarker2);

                // Set the markers' measurement search region.
                AIL.MmeasSetMarker(MeasMarker1, AIL.M_BOX_ORIGIN, MEAS_BOX_POS_X1, MEAS_BOX_POS_Y1);
                AIL.MmeasSetMarker(MeasMarker1, AIL.M_BOX_SIZE, MEAS_BOX_WIDTH1, MEAS_BOX_HEIGHT1);
                AIL.MmeasSetMarker(MeasMarker2, AIL.M_BOX_ORIGIN, MEAS_BOX_POS_X2, MEAS_BOX_POS_Y2);
                AIL.MmeasSetMarker(MeasMarker2, AIL.M_BOX_SIZE, MEAS_BOX_WIDTH2, MEAS_BOX_HEIGHT2);

                // Set markers' orientation.
                AIL.MmeasSetMarker(MeasMarker1, AIL.M_ORIENTATION, AIL.M_HORIZONTAL, AIL.M_NULL);
                AIL.MmeasSetMarker(MeasMarker2, AIL.M_ORIENTATION, AIL.M_HORIZONTAL, AIL.M_NULL);

                // Set markers' settings to locate the largest stripe within the range
                // [WIDTH_APPROXIMATION - WIDTH_VARIATION,
                //  WIDTH_APPROXIMATION + WIDTH_VARIATION],
                // and with an edge strength over MIN_EDGE_VALUE.
                AIL.MmeasSetMarker(MeasMarker1, AIL.M_EDGEVALUE_MIN, MIN_EDGE_VALUE, AIL.M_NULL);

                // Remove the default strength characteristic score mapping.
                AIL.MmeasSetScore(MeasMarker1, AIL.M_STRENGTH_SCORE,
                                           0.0,
                                           0.0,
                                           AIL.M_MAX_POSSIBLE_VALUE,
                                           AIL.M_MAX_POSSIBLE_VALUE,
                                           AIL.M_DEFAULT,
                                           AIL.M_DEFAULT,
                                           AIL.M_DEFAULT);

                // Add a width characteristic score mapping (increasing ramp)
                // to find the largest stripe within a given range.
                //
                // Width score mapping to find the largest stripe within a given
                // width range ]Wmin, Wmax]:
                //
                //    Score
                //       ^
                //       |         /|
                //       |       /  |
                //       |     /    |
                //       +---------------> Width
                //           Wmin  Wmax
                //
                AIL.MmeasSetScore(MeasMarker1, AIL.M_STRIPE_WIDTH_SCORE,
                                           WIDTH_APPROXIMATION - WIDTH_VARIATION,
                                           WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                           WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                           WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                           AIL.M_DEFAULT,
                                           AIL.M_PIXEL,
                                           AIL.M_DEFAULT);

                // Set the same settings for the second marker.
                AIL.MmeasSetMarker(MeasMarker2, AIL.M_EDGEVALUE_MIN, MIN_EDGE_VALUE, AIL.M_NULL);

                AIL.MmeasSetScore(MeasMarker2, AIL.M_STRENGTH_SCORE,
                                           0.0,
                                           0.0,
                                           AIL.M_MAX_POSSIBLE_VALUE,
                                           AIL.M_MAX_POSSIBLE_VALUE,
                                           AIL.M_DEFAULT,
                                           AIL.M_DEFAULT,
                                           AIL.M_DEFAULT);

                AIL.MmeasSetScore(MeasMarker2, AIL.M_STRIPE_WIDTH_SCORE,
                                           WIDTH_APPROXIMATION - WIDTH_VARIATION,
                                           WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                           WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                           WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                           AIL.M_DEFAULT,
                                           AIL.M_PIXEL,
                                           AIL.M_DEFAULT);

                // Find and measure the position and width of the board.
                AIL.MmeasFindMarker(AIL.M_DEFAULT, AilImage, MeasMarker1, AIL.M_STRIPE_WIDTH + AIL.M_POSITION);
                AIL.MmeasFindMarker(AIL.M_DEFAULT, AilImage, MeasMarker2, AIL.M_STRIPE_WIDTH + AIL.M_POSITION);

                // Get the world width of the two markers.
                AIL.MmeasGetResult(MeasMarker1, AIL.M_STRIPE_WIDTH, ref WorldDistance1);
                AIL.MmeasGetResult(MeasMarker2, AIL.M_STRIPE_WIDTH, ref WorldDistance2);

                // Get the pixel width of the two markers.
                AIL.MmeasSetMarker(MeasMarker1, AIL.M_RESULT_OUTPUT_UNITS, AIL.M_PIXEL, AIL.M_NULL);
                AIL.MmeasSetMarker(MeasMarker2, AIL.M_RESULT_OUTPUT_UNITS, AIL.M_PIXEL, AIL.M_NULL);
                AIL.MmeasGetResult(MeasMarker1, AIL.M_STRIPE_WIDTH, ref PixelDistance1);
                AIL.MmeasGetResult(MeasMarker2, AIL.M_STRIPE_WIDTH, ref PixelDistance2);

                // Get the edges position in pixel to draw the annotations.
                AIL.MmeasGetResult(MeasMarker1, AIL.M_POSITION + AIL.M_EDGE_FIRST, ref PosX1, ref PosY1);
                AIL.MmeasGetResult(MeasMarker1, AIL.M_POSITION + AIL.M_EDGE_SECOND, ref PosX2, ref PosY2);
                AIL.MmeasGetResult(MeasMarker2, AIL.M_POSITION + AIL.M_EDGE_FIRST, ref PosX3, ref PosY3);
                AIL.MmeasGetResult(MeasMarker2, AIL.M_POSITION + AIL.M_EDGE_SECOND, ref PosX4, ref PosY4);

                // Draw the measurement indicators on the image. 
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_YELLOW);
                AIL.MmeasDraw(AIL.M_DEFAULT, MeasMarker1, AilOverlayImage, AIL.M_DRAW_WIDTH, AIL.M_DEFAULT, AIL.M_RESULT);
                AIL.MmeasDraw(AIL.M_DEFAULT, MeasMarker2, AilOverlayImage, AIL.M_DRAW_WIDTH, AIL.M_DEFAULT, AIL.M_RESULT);

                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKCOLOR, AIL.M_COLOR_BLACK);
                AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, (int)(PosX1 + 0.5 - 40), (int)((PosY1 + 0.5) + ((PosY2 - PosY1) / 2.0)), " Distance 1 ");
                AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, (int)(PosX3 + 0.5 - 40), (int)((PosY3 + 0.5) + ((PosY4 - PosY3) / 2.0)), " Distance 2 ");

                // Pause to show the original image and the measurement results.
                Console.WriteLine("A distorted image grabbed with the same camera was loaded and");
                Console.WriteLine("calibrated measurements were done to evaluate the board dimensions.");
                Console.WriteLine();
                Console.WriteLine("========================================================");
                Console.WriteLine("                      Distance 1          Distance 2 ");
                Console.WriteLine("--------------------------------------------------------");
                Console.WriteLine(" Calibrated unit:   {0,8:0.00} cm           {1,6:0.00} cm    ", WorldDistance1, WorldDistance2);
                Console.WriteLine(" Uncalibrated unit: {0,8:0.00} pixels       {1,6:0.00} pixels", PixelDistance1, PixelDistance2);
                Console.WriteLine("========================================================");
                Console.WriteLine();
                Console.WriteLine("Press any key to continue.");
                Console.WriteLine();
                Console.ReadKey(true);

                // Clear the display overlay.
                AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

                // Read the image of the PCB.
                AIL.MbufLoad(BOARD_IMAGE_FILE, AilImage);

                // Transform the image of the board.
                AIL.McalTransformImage(AilImage, AilImage, AilCalibration, AIL.M_BILINEAR + AIL.M_OVERSCAN_CLEAR, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // show the transformed image of the board.
                Console.WriteLine("The image was corrected to remove its distortions.");

                // Free measurement markers.
                AIL.MmeasFree(MeasMarker1);
                AIL.MmeasFree(MeasMarker2);
            }
            else
            {
                Console.WriteLine("Calibration generated an exception.");
                Console.WriteLine("See User Guide to resolve the situation.");
                Console.WriteLine();
            }

            // Wait for a key to be pressed.
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Free all allocations.
            AIL.McalFree(AilCalibration);
            AIL.MbufFree(AilImage);
        }

        //****************************************************************************
        // Tsai example. 
        //****************************************************************************
        // Source image files specification.
        private const string GRID_ORIGINAL_IMAGE_FILE = AIL.M_IMAGE_PATH + "CalGridOriginal.mim";
        private const string OBJECT_ORIGINAL_IMAGE_FILE = AIL.M_IMAGE_PATH + "CalObjOriginal.mim";
        private const string OBJECT_MOVED_IMAGE_FILE = AIL.M_IMAGE_PATH + "CalObjMoved.mim";

        // World description of the calibration grid.
        private const double GRID_ORG_ROW_SPACING = 1.5;
        private const double GRID_ORG_COLUMN_SPACING = 1.5;
        private const int GRID_ORG_ROW_NUMBER = 12;
        private const int GRID_ORG_COLUMN_NUMBER = 13;
        private const int GRID_ORG_OFFSET_X = 0;
        private const int GRID_ORG_OFFSET_Y = 0;
        private const int GRID_ORG_OFFSET_Z = 0;

        // Region parameters for metrology
        private const int MEASURED_CIRCLE_LABEL = 1;
        private const double RING1_POS1_X = 2.3;
        private const double RING1_POS1_Y = 3.9;
        private const double RING2_POS1_X = 10.7;
        private const double RING2_POS1_Y = 11.1;

        private const double RING_START_RADIUS = 1.25;
        private const double RING_END_RADIUS = 2.3;

        // measured plane position
        private const double RING_THICKNESS = 0.175;
        private const double STEP_THICKNESS = 4.0;

        // Color definitions
        static readonly int ABSOLUTE_COLOR = AIL.M_RGB888(255, 0, 0);
        static readonly int RELATIVE_COLOR = AIL.M_RGB888(0, 255, 0);
        static readonly int REGION_COLOR = AIL.M_RGB888(0, 100, 255);
        static readonly int FEATURE_COLOR = AIL.M_RGB888(255, 0, 255);

        static void TsaiCalibration(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;          // Image buffer identifier.
            AIL_ID AilOverlayImage = AIL.M_NULL;   // Overlay image buffer identifier.
            AIL_ID AilCalibration = AIL.M_NULL;    // Calibration identifier.

            AIL_INT CalibrationStatus = 0;

            // Restore source image into an automatically allocated image buffer.
            AIL.MbufRestore(GRID_ORIGINAL_IMAGE_FILE, AilSystem, ref AilImage);

            // Display the image buffer.
            AIL.MdispSelect(AilDisplay, AilImage);
            // Prepare for overlay annotation.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            // Pause to show the original image.
            Console.WriteLine();
            Console.WriteLine("TSAI BASED CALIBRATION:");
            Console.WriteLine("-----------------------");
            Console.WriteLine();
            Console.WriteLine("The displayed grid has been grabbed with a high perspective");
            Console.WriteLine("camera and will be used to calibrate the camera.");
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Allocate a camera calibration context.
            AIL.McalAlloc(AilSystem, AIL.M_TSAI_BASED, AIL.M_DEFAULT, ref AilCalibration);

            // Calibrate the camera with the image of the grid and its world description.
            AIL.McalGrid(AilCalibration, AilImage, GRID_ORG_OFFSET_X, GRID_ORG_OFFSET_Y, GRID_ORG_OFFSET_Z, GRID_ORG_ROW_NUMBER, GRID_ORG_COLUMN_NUMBER, GRID_ORG_ROW_SPACING, GRID_ORG_COLUMN_SPACING, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Verify if the camera calibration was successful
            AIL.McalInquire(AilCalibration, AIL.M_CALIBRATION_STATUS + AIL.M_TYPE_AIL_INT, ref CalibrationStatus);
            if (CalibrationStatus == AIL.M_CALIBRATED)
            {
                // Display the world absolute coordinate system
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, ABSOLUTE_COLOR);
                AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilOverlayImage, AIL.M_DRAW_ABSOLUTE_COORDINATE_SYSTEM + AIL.M_DRAW_AXES, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Print camera information
                Console.WriteLine("The camera has been calibrated.");
                Console.WriteLine("The world absolute coordinate system is shown in red.");
                Console.WriteLine();
                ShowCameraInformation(AilCalibration);

                // Load source image into an image buffer.
                AIL.MbufLoad(OBJECT_ORIGINAL_IMAGE_FILE, AilImage);

                // Associate calibration context to the image
                AIL.McalAssociate(AilCalibration, AilImage, AIL.M_DEFAULT);

                // Set the offset of the camera calibration plane.
                // This moves the relative origin to the top of the first metallic part 
                AIL.McalSetCoordinateSystem(AilImage, AIL.M_RELATIVE_COORDINATE_SYSTEM, AIL.M_ABSOLUTE_COORDINATE_SYSTEM, AIL.M_TRANSLATION + AIL.M_ASSIGN, AIL.M_NULL, 0, 0, -RING_THICKNESS, AIL.M_DEFAULT);

                // Display the world relative coordinate system
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, RELATIVE_COLOR);
                AIL.McalDraw(AIL.M_DEFAULT, AilImage, AilOverlayImage, AIL.M_DRAW_RELATIVE_COORDINATE_SYSTEM + AIL.M_DRAW_FRAME, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Measure the first circle.
                Console.WriteLine("The relative coordinate system (shown in green) was translated by {0:f3} cm", -RING_THICKNESS);
                Console.WriteLine("in z to align it with the top of the first metallic part.");
                MeasureRing(AilSystem, AilOverlayImage, AilImage, RING1_POS1_X, RING1_POS1_Y);
                Console.WriteLine("Press any key to continue.");
                Console.WriteLine();
                Console.ReadKey(true);

                // Modify the offset of the camera calibration plane.
                // This moves the relative origin to the top of the second metallic part 
                AIL.McalSetCoordinateSystem(AilImage, AIL.M_RELATIVE_COORDINATE_SYSTEM, AIL.M_ABSOLUTE_COORDINATE_SYSTEM, AIL.M_TRANSLATION + AIL.M_COMPOSE_WITH_CURRENT, AIL.M_NULL, 0, 0, -STEP_THICKNESS, AIL.M_DEFAULT);

                // Clear the overlay
                AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);
                /* Display the world absolute coordinate system */
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, ABSOLUTE_COLOR);
                AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilOverlayImage, AIL.M_DRAW_ABSOLUTE_COORDINATE_SYSTEM + AIL.M_DRAW_AXES, AIL.M_DEFAULT, AIL.M_DEFAULT);
                // Display the world relative coordinate system
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, RELATIVE_COLOR);
                AIL.McalDraw(AIL.M_DEFAULT, AilImage, AilOverlayImage, AIL.M_DRAW_RELATIVE_COORDINATE_SYSTEM + AIL.M_DRAW_FRAME, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Measure the second circle.
                Console.WriteLine("The relative coordinate system was translated by another {0:f3} cm", -STEP_THICKNESS);
                Console.WriteLine("in z to align it with the top of the second metallic part.");
                MeasureRing(AilSystem, AilOverlayImage, AilImage, RING2_POS1_X, RING2_POS1_Y);
                Console.WriteLine("Press any key to continue.");
                Console.WriteLine();
                Console.ReadKey(true);
            }
            else
            {
                Console.WriteLine("Calibration generated an exception.");
                Console.WriteLine("See User Guide to resolve the situation.");
                Console.WriteLine();
            }

            // Free all allocations.
            AIL.McalFree(AilCalibration);
            AIL.MbufFree(AilImage);
        }

        // Measuring function with AilMetrology module
        static void MeasureRing(AIL_ID AilSystem, AIL_ID AilOverlayImage, AIL_ID AilImage, double MeasureRingX, double MeasureRingY)
        {
            AIL_ID AilMetrolContext = AIL.M_NULL;  // Metrology Context.
            AIL_ID AilMetrolResult = AIL.M_NULL;   // Metrology Result.

            double Value = 0.0;

            // Allocate metrology context and result.
            AIL.MmetAlloc(AilSystem, AIL.M_DEFAULT, ref AilMetrolContext);
            AIL.MmetAllocResult(AilSystem, AIL.M_DEFAULT, ref AilMetrolResult);

            // Add a first measured segment feature to context and set its search region.
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_CIRCLE, MEASURED_CIRCLE_LABEL, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT);

            AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), AIL.M_DEFAULT, AIL.M_RING, MeasureRingX, MeasureRingY, RING_START_RADIUS, RING_END_RADIUS, AIL.M_NULL, AIL.M_NULL);

            // Calculate.
            AIL.MmetCalculate(AilMetrolContext, AilImage, AilMetrolResult, AIL.M_DEFAULT);

            // Draw region.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, REGION_COLOR);
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, AilOverlayImage, AIL.M_DRAW_REGION, AIL.M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), AIL.M_DEFAULT);

            // Draw features.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FEATURE_COLOR);
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, AilOverlayImage, AIL.M_DRAW_FEATURE, AIL.M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), AIL.M_DEFAULT);

            AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), AIL.M_RADIUS, ref Value);
            Console.WriteLine("The large circle's radius was measured: {0:0.000} cm.", Value);

            // Free all allocations.
            AIL.MmetFree(AilMetrolResult);
            AIL.MmetFree(AilMetrolContext);
        }

        // Print the current camera position and orientation 
        static void ShowCameraInformation(AIL_ID AilCalibration)
        {
            double CameraPosX = 0.0;
            double CameraPosY = 0.0;
            double CameraPosZ = 0.0;
            double CameraYaw = 0.0;
            double CameraPitch = 0.0;
            double CameraRoll = 0.0;

            AIL.McalGetCoordinateSystem(AilCalibration, AIL.M_CAMERA_COORDINATE_SYSTEM, AIL.M_ABSOLUTE_COORDINATE_SYSTEM, AIL.M_TRANSLATION, AIL.M_NULL, ref CameraPosX, ref CameraPosY, ref CameraPosZ, AIL.M_NULL);
            AIL.McalGetCoordinateSystem(AilCalibration, AIL.M_CAMERA_COORDINATE_SYSTEM, AIL.M_ABSOLUTE_COORDINATE_SYSTEM, AIL.M_ROTATION_YXZ, AIL.M_NULL, ref CameraYaw, ref CameraPitch, ref CameraRoll, AIL.M_NULL);

            // Pause to show the corrected image of the grid.
            Console.WriteLine("Camera position in cm:          (x, y, z)           ({0:0.00}, {1:0.00}, {2:0.00})", CameraPosX, CameraPosY, CameraPosZ);
            Console.WriteLine("Camera orientation in degrees:  (yaw, pitch, roll)  ({0:0.00}, {1:0.00}, {2:0.00})", CameraYaw, CameraPitch, CameraRoll);
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);
        }


        //****************************************************************************
        // Partial grid example. 
        // ***************************************************************************
        // Source image files specification.
        private const string PARTIAL_GRID_IMAGE_FILE = AIL.M_IMAGE_PATH + "PartialGrid.mim";

        // Definition of the region to correct.
        private const double CORRECTED_SIZE_X = 60.0;
        private const double CORRECTED_SIZE_Y = 50.0;
        private const double CORRECTED_OFFSET_X = -35.0;
        private const double CORRECTED_OFFSET_Y = -5.0;
        private const int CORRECTED_IMAGE_SIZE_X = 512;

        static void PartialGridCalibration(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;               // Image buffer identifier.
            AIL_ID AilCorrectedImage = AIL.M_NULL;      // Corrected image identifier.
            AIL_ID AilGraList = AIL.M_NULL;             // Graphic list identifier.
            AIL_ID AilCalibration = AIL.M_NULL;         // Calibration identifier.

            AIL_INT CalibrationStatus = 0;
            AIL_INT ImageType = 0;
            AIL_INT CorrectedImageSizeY = 0;
            double RowSpacing = 0.0;
            double ColumnSpacing = 0.0;
            double CorrectedPixelSize = 0.0;
            StringBuilder UnitName = new StringBuilder();


            // Clear the display
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            // Allocate a graphic list and associate it to the display.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref AilGraList);
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraList);

            // Restore source image into an automatically allocated image buffer.
            AIL.MbufRestore(PARTIAL_GRID_IMAGE_FILE, AilSystem, ref AilImage);
            AIL.MbufInquire(AilImage, AIL.M_TYPE, ref ImageType);

            // Display the image buffer.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Pause to show the partial grid image.
            Console.WriteLine();
            Console.WriteLine("PARTIAL GRID CALIBRATION:");
            Console.WriteLine("-------------------------");
            Console.WriteLine();
            Console.WriteLine("A camera will be calibrated using a rectangular grid that");
            Console.WriteLine("is only partially visible in the camera's field of view.");
            Console.WriteLine("The 2D code in the center is used as a fiducial to retrieve");
            Console.WriteLine("the characteristics of the calibration grid.");
            Console.WriteLine("Press any key to continue.");
            Console.ReadKey(true);

            // Allocate the calibration object.
            AIL.McalAlloc(AilSystem, AIL.M_TSAI_BASED, AIL.M_DEFAULT, ref AilCalibration);

            // Set the calibration to calibrate a partial grid with fiducial.
            AIL.McalControl(AilCalibration, AIL.M_GRID_PARTIAL, AIL.M_ENABLE);
            AIL.McalControl(AilCalibration, AIL.M_GRID_FIDUCIAL, AIL.M_DATAMATRIX);

            // Calibrate the camera with the partial grid with fiducial.
            AIL.McalGrid(AilCalibration, AilImage,
                     GRID_OFFSET_X, GRID_OFFSET_Y, GRID_OFFSET_Z,
                     AIL.M_UNKNOWN, AIL.M_UNKNOWN, AIL.M_FROM_FIDUCIAL, AIL.M_FROM_FIDUCIAL,
                     AIL.M_DEFAULT, AIL.M_CHESSBOARD_GRID);

            AIL.McalInquire(AilCalibration, AIL.M_CALIBRATION_STATUS + AIL.M_TYPE_AIL_INT, ref CalibrationStatus);
            if (CalibrationStatus == AIL.M_CALIBRATED)
            {
                // Draw the absolute coordinate system.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);
                AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilGraList, AIL.M_DRAW_ABSOLUTE_COORDINATE_SYSTEM,
                    AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Draw a box around the fiducial.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_CYAN);
                AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilGraList, AIL.M_DRAW_FIDUCIAL_BOX,
                   AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Get the information of the grid read from the fiducial.
                AIL.McalInquire(AilCalibration, AIL.M_ROW_SPACING, ref RowSpacing);
                AIL.McalInquire(AilCalibration, AIL.M_COLUMN_SPACING, ref ColumnSpacing);
                AIL.McalInquire(AilCalibration, AIL.M_GRID_UNIT_SHORT_NAME, UnitName);

                // Draw the information of the grid read from the fiducial.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_INPUT_UNITS, AIL.M_DISPLAY);
                DrawGridInfo(AilGraList, "Row spacing", RowSpacing, 0, UnitName.ToString());
                DrawGridInfo(AilGraList, "Col spacing", ColumnSpacing, 1, UnitName.ToString());

                // Pause to show the calibration result.
                Console.WriteLine();
                Console.WriteLine("The camera has been calibrated.");
                Console.WriteLine();
                Console.WriteLine("The grid information read is displayed.");
                Console.WriteLine("Press any key to continue.");
                Console.WriteLine();
                Console.ReadKey(true);

                // Calculate the pixel size and size Y of the corrected image.
                CorrectedPixelSize = CORRECTED_SIZE_X / CORRECTED_IMAGE_SIZE_X;
                CorrectedImageSizeY = (AIL_INT)(CORRECTED_SIZE_Y / CorrectedPixelSize);

                // Allocate the corrected image.
                AIL.MbufAlloc2d(AilSystem, CORRECTED_IMAGE_SIZE_X, CorrectedImageSizeY, ImageType,
                   AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilCorrectedImage);

                // Calibrate the corrected image.
                AIL.McalUniform(AilCorrectedImage, CORRECTED_OFFSET_X, CORRECTED_OFFSET_Y,
                   CorrectedPixelSize, CorrectedPixelSize, 0.0, AIL.M_DEFAULT);

                // Correct the calibrated image.
                AIL.McalTransformImage(AilImage, AilCorrectedImage, AilCalibration,
                   AIL.M_BILINEAR + AIL.M_OVERSCAN_CLEAR, AIL.M_DEFAULT,
                   AIL.M_WARP_IMAGE + AIL.M_USE_DESTINATION_CALIBRATION);

                // Select the corrected image on the display.
                AIL.MgraClear(AIL.M_DEFAULT, AilGraList);
                AIL.MdispSelect(AilDisplay, AilCorrectedImage);

                // Pause to show the corrected image.
                Console.WriteLine("A sub-region of the grid was selected and transformed");
                Console.WriteLine("to remove the distortions.");
                Console.WriteLine("The sub-region dimensions and position are:");
                Console.WriteLine("   Size X  : {0,3:g3} {1}", CORRECTED_SIZE_X, UnitName);
                Console.WriteLine("   Size Y  : {0,3:g3} {1}", CORRECTED_SIZE_Y, UnitName);
                Console.WriteLine("   Offset X: {0,3:g3} {1}", CORRECTED_OFFSET_X, UnitName);
                Console.WriteLine("   Offset Y: {0,3:g3} {1}", CORRECTED_OFFSET_Y, UnitName);

                // Wait for a key to be pressed.
                Console.WriteLine("Press any key to quit.");
                Console.WriteLine();
                Console.ReadKey(true);

                AIL.MbufFree(AilCorrectedImage);
            }
            else
            {
                Console.WriteLine("Calibration generated an exception.");
                Console.WriteLine("See User Guide to resolve the situation.");
                Console.WriteLine();
                Console.WriteLine("Press any key to quit.");
                Console.WriteLine();
                Console.ReadKey(true);
            }

            // Free all allocations.
            AIL.McalFree(AilCalibration);
            AIL.MbufFree(AilImage);
            AIL.MgraFree(AilGraList);
        }

        // Definition of the parameters for the drawing of the grid info
        private const int LINE_HEIGHT = 16;

        // Draw an information of the grid.
        static void DrawGridInfo(AIL_ID AilGraList, string InfoTag, double Value, AIL_INT LineOffsetY, string Units)
        {
            string Info = string.Format("{0}: {1:g3} {2}", InfoTag, Value, Units);
            AIL.MgraText(AIL.M_DEFAULT, AilGraList, 0, LineOffsetY * LINE_HEIGHT, Info);
        }
    }
}

```
