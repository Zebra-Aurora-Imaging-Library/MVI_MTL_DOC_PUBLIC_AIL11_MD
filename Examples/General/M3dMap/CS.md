---
title: "M3dMap"
description: "This program inspects a wood surface using laser profiling to find any depth defects and computes the volume of a cookie. Note: Printable calibration grids in PDF format can be found in your \"Zebra Imaging/Images/\" directory. Note: When considering a laser-based 3D reconstruction system, the file \"3D Setup Helper.xls\" can be used to accelerate prototyping by choosing an adequate hardware configuration (angle, distance, lens, camera, ...). The file is located in your \"Zebra Imaging/Tools/\" directory."
ms.language: "C#"
---

# M3dMap

> This program inspects a wood surface using laser profiling to find any depth defects and computes the volume of a cookie. Note: Printable calibration grids in PDF format can be found in your "Zebra Imaging/Images/" directory. Note: When considering a laser-based 3D reconstruction system, the file "3D Setup Helper.xls" can be used to accelerate prototyping by choosing an adequate hardware configuration (angle, distance, lens, camera, ...). The file is located in your "Zebra Imaging/Tools/" directory.

**Language:** C#

**Functions used:** `M3dmapAddScan`, `M3dmapAlloc`, `M3dmapAllocResult`, `M3dmapCalibrate`, `M3dmapControl`, `M3dmapCopyResult`, `M3dmapFree`, `M3dmapInquire`, `M3ddispControl`, `MappAlloc`, `MappFree`, `MappTimer`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobGetResult`, `MbufAlloc1d`, `MbufAlloc2d`, `MbufAllocColor`, `MbufAllocContainer`, `MbufClear`, `MbufCopy`, `MbufCopyColor2d`, `MbufDiskInquire`, `MbufFree`, `MbufControlContainer`, `MbufImportSequence`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `McalAlloc`, `McalDraw`, `McalFree`, `McalGrid`, `McalInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispLut`, `MdispSelect`, `MgenLutRamp`, `MgraControl`, `MgraText`, `MimBinarize`, `MimControl`, `MimConvert`, `MimOpen`, `MsysAlloc`, `MsysFree`, `M3ddispAlloc`, `M3ddispFree`, `M3ddispSelect`, `M3ddispSetView`, `M3dgeoAlloc`, `M3dgeoFree`, `M3dgeoPlane`, `M3dimAlloc`, `M3dimCalibrateDepthMap`, `M3dimControl`, `M3dimCrop`, `M3dimFillGaps`, `M3dimFree`, `M3dimProject`, `M3dmetVolume`, `MappControl`

**Categories:** Overview, General, Industries, Machining, Food and Beverage, Applications, Inspecting/Proofing/Verifying, 3D profiling, Modules, Buffer, Display, Graphics, Image Processing, Calibration, Blob Analysis, 3D Map, 3D Display, 3D Graphics, 3D Image Processing, 3D Geometry, 3D Metrology, What's New, AIL 10.0 SP4, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    m3dmap.cs
//
// Description: This program inspects a wood surface using 
//              sheet-of-light profiling (laser) to find any depth defects.
//
//              Printable calibration grids in PDF format can be found in your
//              "Aurora Imaging Library/##/Images/" directory.
//              
//              When considering a laser-based 3D reconstruction system, the file "3D Setup Helper.xls"
//              can be used to accelerate prototyping by choosing an adequate hardware configuration
//              (angle, distance, lens, camera, ...). The file is located in your
//              "Aurora Imaging Library/##/Tools/" directory.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Text;
using Zebra.AuroraImagingLibrary;

namespace M3dmap
{
    class Program
    {
        // *****************************************************************************
        // Main.
        // *****************************************************************************
        static int Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Run the depth correction example.
            DepthCorrectionExample(AilSystem, AilDisplay);

            // Run the calibrated camera example.
            CalibratedCameraExample(AilSystem, AilDisplay);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);

            return 0;
        }

        // *****************************************************************************
        // Depth correction example.
        // *****************************************************************************

        // Input sequence specifications.
        private const string REFERENCE_PLANES_SEQUENCE_FILE = AIL.M_IMAGE_PATH + "ReferencePlanes.avi";
        private const string OBJECT_SEQUENCE_FILE = AIL.M_IMAGE_PATH + "ScannedObject.avi";

        // Peak detection parameters.
        private const int PEAK_WIDTH_NOMINAL = 10;
        private const int PEAK_WIDTH_DELTA = 8;
        private const int MIN_CONTRAST = 140;

        // Calibration heights in mm.
        private static readonly double[] CORRECTED_DEPTHS = { 1.25, 2.50, 3.75, 5.00 };

        private const double SCALE_FACTOR = 10000.0; // (depth in world units) * SCALE_FACTOR gives gray levels

        // Annotation position.
        private const int CALIB_TEXT_POS_X = 400;
        private const int CALIB_TEXT_POS_Y = 15;

        private static void DepthCorrectionExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilOverlayImage = AIL.M_NULL;    // Overlay image buffer identifier.
            AIL_ID AilImage = AIL.M_NULL;           // Image buffer identifier (for processing).
            AIL_ID AilDepthMap = AIL.M_NULL;        // Image buffer identifier (for results).
            AIL_ID AilLaser = AIL.M_NULL;           // 3dmap laser profiling context identifier.
            AIL_ID AilCalibScan = AIL.M_NULL;       // 3dmap result buffer identifier for laser
                                                    // line calibration.
            AIL_ID AilScan = AIL.M_NULL;            // 3dmap result buffer identifier.

            AIL_INT SizeX = 0;                      // Width of grabbed images.
            AIL_INT SizeY = 0;                      // Height of grabbed images.
            AIL_INT NbReferencePlanes = 0;          // Number of reference planes of known heights.
            AIL_INT NbObjectImages = 0;             // Number of frames for scanned objects.
            int n = 0;                              // Counter.
            double FrameRate = 0.0;                 // Number of grabbed frames per second (in AVI).
            double StartTime = 0.0;                 // Time at the beginning of each iteration.
            double EndTime = 0.0;                   // Time after processing for each iteration.
            double WaitTime = 0.0;                  // Time to wait for next frame.

            // Inquire characteristics of the input sequences.
            AIL.MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_SIZE_X, ref SizeX);
            AIL.MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_SIZE_Y, ref SizeY);
            AIL.MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_NUMBER_OF_IMAGES, ref NbReferencePlanes);
            AIL.MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_FRAME_RATE, ref FrameRate);
            AIL.MbufDiskInquire(OBJECT_SEQUENCE_FILE, AIL.M_NUMBER_OF_IMAGES, ref NbObjectImages);

            // Allocate buffer to hold images.
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC, ref AilImage);
            AIL.MbufClear(AilImage, 0.0);

            Console.WriteLine();
            Console.WriteLine("DEPTH ANALYSIS:");
            Console.WriteLine("---------------");
            Console.WriteLine();
            Console.WriteLine("This program performs a surface inspection to detect depth defects ");
            Console.WriteLine("on a wood surface using a laser (sheet-of-light) profiling system.");
            Console.WriteLine();
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Select display.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Prepare for overlay annotations.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKGROUND_MODE, AIL.M_TRANSPARENT);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_WHITE);

            // Allocate 3dmap objects.
            AIL.M3dmapAlloc(AilSystem, AIL.M_LASER, AIL.M_DEPTH_CORRECTION, ref AilLaser);
            AIL.M3dmapAllocResult(AilSystem, AIL.M_LASER_CALIBRATION_DATA, AIL.M_DEFAULT, ref AilCalibScan);

            // Set laser line extraction options.
            AIL_ID AilPeakLocator = AIL.M_NULL;
            AIL.M3dmapInquire(AilLaser, AIL.M_DEFAULT, AIL.M_LOCATE_PEAK_1D_CONTEXT_ID + AIL.M_TYPE_AIL_ID, ref AilPeakLocator);
            AIL.MimControl(AilPeakLocator, AIL.M_PEAK_WIDTH_NOMINAL, PEAK_WIDTH_NOMINAL);
            AIL.MimControl(AilPeakLocator, AIL.M_PEAK_WIDTH_DELTA, PEAK_WIDTH_DELTA);
            AIL.MimControl(AilPeakLocator, AIL.M_MINIMUM_CONTRAST, MIN_CONTRAST);

            // Open the calibration sequence file for reading.
            AIL.MbufImportSequence(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                            AIL.M_NULL, AIL.M_NULL, AIL.M_OPEN);

            // Read and process all images in the input sequence.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref StartTime);

            for (n = 0; n < NbReferencePlanes; n++)
            {
                string CalibString;

                // Read image from sequence.
                AIL.MbufImportSequence(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_LOAD, AIL.M_NULL,
                                                                    ref AilImage, AIL.M_DEFAULT, 1, AIL.M_READ);

                // Annotate the image with the calibration height.
                AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);
                CalibString = string.Format("Reference plane {0}: {1,0:f2} mm", (int)(n + 1), CORRECTED_DEPTHS[n]);
                AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, CALIB_TEXT_POS_X, CALIB_TEXT_POS_Y, CalibString);

                // Set desired corrected depth of next reference plane.
                AIL.M3dmapControl(AilLaser, AIL.M_DEFAULT, AIL.M_CORRECTED_DEPTH,
                                                                   CORRECTED_DEPTHS[n] * SCALE_FACTOR);

                // Analyze the image to extract laser line.
                AIL.M3dmapAddScan(AilLaser, AilCalibScan, AilImage, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Wait to have a proper frame rate, if necessary.
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref EndTime);
                WaitTime = (1.0 / FrameRate) - (EndTime - StartTime);
                if (WaitTime > 0)
                {
                    AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_WAIT, ref WaitTime);
                }

                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref StartTime);
            }

            // Close the calibration sequence file.
            AIL.MbufImportSequence(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                           AIL.M_NULL, AIL.M_NULL, AIL.M_CLOSE);

            // Calibrate the laser profiling context using reference planes of known heights.
            AIL.M3dmapCalibrate(AilLaser, AilCalibScan, AIL.M_NULL, AIL.M_DEFAULT);

            Console.WriteLine("The laser profiling system has been calibrated using 4 reference");
            Console.WriteLine("planes of known heights.");
            Console.WriteLine();
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            Console.WriteLine("The wood surface is being scanned.");
            Console.WriteLine();

            // Free the result buffer used for calibration because it will not be used anymore.
            AIL.M3dmapFree(AilCalibScan);
            AilCalibScan = AIL.M_NULL;

            // Allocate the result buffer for the scanned depth corrected data.
            AIL.M3dmapAllocResult(AilSystem, AIL.M_DEPTH_CORRECTED_DATA, AIL.M_DEFAULT, ref AilScan);

            // Open the object sequence file for reading.
            AIL.MbufDiskInquire(OBJECT_SEQUENCE_FILE, AIL.M_FRAME_RATE, ref FrameRate);
            AIL.MbufImportSequence(OBJECT_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                                    AIL.M_NULL, AIL.M_OPEN);

            // Read and process all images in the input sequence.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref StartTime);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            for (n = 0; n < NbObjectImages; n++)
            {
                // Read image from sequence.
                AIL.MbufImportSequence(OBJECT_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_LOAD, AIL.M_NULL, ref AilImage,
                                                                               AIL.M_DEFAULT, 1, AIL.M_READ);

                // Analyze the image to extract laser line and correct its depth.
                AIL.M3dmapAddScan(AilLaser, AilScan, AilImage, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Wait to have a proper frame rate, if necessary.
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref EndTime);
                WaitTime = (1.0 / FrameRate) - (EndTime - StartTime);
                if (WaitTime > 0)
                {
                    AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_WAIT, ref WaitTime);
                }

                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref StartTime);
            }

            // Close the object sequence file.
            AIL.MbufImportSequence(OBJECT_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                                   AIL.M_NULL, AIL.M_CLOSE);

            // Allocate the image for a partially corrected depth map.
            AIL.MbufAlloc2d(AilSystem, SizeX, NbObjectImages, 16 + AIL.M_UNSIGNED,
                                                                AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilDepthMap);

            // Get partially corrected depth map from accumulated information in the result buffer.
            AIL.M3dmapCopyResult(AilScan, AIL.M_DEFAULT, AilDepthMap, AIL.M_PARTIALLY_CORRECTED_DEPTH_MAP, AIL.M_DEFAULT);

            // Disable display updates.
            AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_DISABLE);

            // Show partially corrected depth map and find defects.
            SetupColorDisplay(AilSystem, AilDisplay, AIL.MbufInquire(AilDepthMap, AIL.M_SIZE_BIT, AIL.M_NULL));

            // Display partially corrected depth map.
            AIL.MdispSelect(AilDisplay, AilDepthMap);
            AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            Console.WriteLine("The pseudo-color depth map of the surface is displayed.");
            Console.WriteLine();
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            PerformBlobAnalysis(AilSystem, AilDisplay, AilOverlayImage, AilDepthMap);

            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Disassociates display LUT and clear overlay.
            AIL.MdispSelect(AilDisplay, AIL.M_NULL);
            AIL.MdispLut(AilDisplay, AIL.M_DEFAULT);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            // Free all allocations.
            AIL.M3dmapFree(AilScan);
            AIL.M3dmapFree(AilLaser);
            AIL.MbufFree(AilDepthMap);
            AIL.MbufFree(AilImage);
        }

        // Values used for binarization.
        private const double EXPECTED_HEIGHT = 3.4;    // Inspected surface should be at this height (in mm)
        private const double DEFECT_THRESHOLD = 0.2;   // Max acceptable deviation from expected height (mm)
        private const double SATURATED_DEFECT = 1.0;   // Deviation at which defect will appear red (in mm)

        // Radius of the smallest particles to keep.
        private const int MIN_BLOB_RADIUS = 3;

        // Pixel offset for drawing text.
        private const int TEXT_H_OFFSET_1 = -50;
        private const int TEXT_V_OFFSET_1 = -6;
        private const int TEXT_H_OFFSET_2 = -30;
        private const int TEXT_V_OFFSET_2 = 6;

        // Find defects in corrected depth map, compute max deviation and draw contours.
        private static void PerformBlobAnalysis(AIL_ID AilSystem,
                                 AIL_ID AilDisplay,
                                 AIL_ID AilOverlayImage,
                                 AIL_ID AilDepthMap)
        {
            AIL_ID AilBinImage = AIL.M_NULL;        // Binary image buffer identifier.
            AIL_ID AilBlobContext = AIL.M_NULL;     // Blob context identifier.
            AIL_ID AilBlobResult = AIL.M_NULL;      // Blob result buffer identifier.
            AIL_INT SizeX = 0;                      // Width of depth map.
            AIL_INT SizeY = 0;                      // Height of depth map.
            AIL_INT TotalBlobs = 0;                 // Total number of blobs.
            AIL_INT n = 0;                          // Counter.
            AIL_INT[] MinPixels;                    // Maximum height of defects.
            double DefectThreshold = 0.0;           // A gray level below it is a defect.
            double[] CogX;                          // X coordinate of center of gravity.
            double[] CogY;                          // Y coordinate of center of gravity.

            // Get size of depth map.
            AIL.MbufInquire(AilDepthMap, AIL.M_SIZE_X, ref SizeX);
            AIL.MbufInquire(AilDepthMap, AIL.M_SIZE_Y, ref SizeY);

            // Allocate a binary image buffer for fast processing.
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 1 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref AilBinImage);

            // Binarize image.
            DefectThreshold = (EXPECTED_HEIGHT - DEFECT_THRESHOLD) * SCALE_FACTOR;
            AIL.MimBinarize(AilDepthMap, AilBinImage, AIL.M_FIXED + AIL.M_LESS_OR_EQUAL, DefectThreshold, AIL.M_NULL);

            // Remove small particles.
            AIL.MimOpen(AilBinImage, AilBinImage, MIN_BLOB_RADIUS, AIL.M_BINARY);

            // Allocate a blob context.
            AIL.MblobAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilBlobContext);

            // Enable the Center Of Gravity and Min Pixel features calculation.
            AIL.MblobControl(AilBlobContext, AIL.M_CENTER_OF_GRAVITY + AIL.M_GRAYSCALE, AIL.M_ENABLE);
            AIL.MblobControl(AilBlobContext, AIL.M_MIN_PIXEL, AIL.M_ENABLE);

            // Allocate a blob result buffer.
            AIL.MblobAllocResult(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilBlobResult);

            // Calculate selected features for each blob.
            AIL.MblobCalculate(AilBlobContext, AilBinImage, AilDepthMap, AilBlobResult);

            // Get the total number of selected blobs.
            AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref TotalBlobs);
            Console.WriteLine("Number of defects: {0}", TotalBlobs);

            // Read and print the blob characteristics.
            CogX = new double[TotalBlobs];
            CogY = new double[TotalBlobs];
            MinPixels = new AIL_INT[TotalBlobs];
            if (CogX != null && CogY != null && MinPixels != null)
            {
                // Get the results.
                AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_CENTER_OF_GRAVITY_X + AIL.M_GRAYSCALE, CogX);
                AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_CENTER_OF_GRAVITY_Y + AIL.M_GRAYSCALE, CogY);
                AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_MIN_PIXEL + AIL.M_TYPE_AIL_INT, MinPixels);

                // Draw the defects.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);
                AIL.MblobDraw(AIL.M_DEFAULT, AilBlobResult, AilOverlayImage,
                          AIL.M_DRAW_BLOBS, AIL.M_INCLUDED_BLOBS, AIL.M_DEFAULT);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_WHITE);

                // Print the depth of each blob.
                for (n = 0; n < TotalBlobs; n++)
                {
                    double DepthOfDefect;
                    string DepthString;

                    // Write the depth of the defect in the overlay.
                    DepthOfDefect = EXPECTED_HEIGHT - (MinPixels[n] / SCALE_FACTOR);
                    DepthString = string.Format("{0,0:f2} mm", DepthOfDefect);

                    Console.WriteLine("Defect #{0}: depth ={1,5:f2} mm", n, DepthOfDefect);
                    Console.WriteLine();
                    AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, CogX[n] + TEXT_H_OFFSET_1,
                                                   CogY[n] + TEXT_V_OFFSET_1, "Defect depth");
                    AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, CogX[n] + TEXT_H_OFFSET_2,
                                                                CogY[n] + TEXT_V_OFFSET_2, DepthString);
                }

            }
            else
            {
                Console.WriteLine("Error: Not enough memory.");
                Console.WriteLine();
            }

            // Free all allocations.
            AIL.MblobFree(AilBlobResult);
            AIL.MblobFree(AilBlobContext);
            AIL.MbufFree(AilBinImage);
        }

        // Color constants for display LUT.
        private const double BLUE_HUE = 171.0;      // Expected depths will be blue.
        private const double RED_HUE = 0.0;         // Worst defects will be red.
        private const int FULL_SATURATION = 255;    // All colors are fully saturated.
        private const int HALF_LUMINANCE = 128;     // All colors have half luminance.

        // Creates a color display LUT to show defects in red.
        private static void SetupColorDisplay(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_INT SizeBit)
        {
            AIL_ID AilRampLut1Band = AIL.M_NULL;    // LUT containing hue values.
            AIL_ID AilRampLut3Band = AIL.M_NULL;    // RGB LUT used by display.
            AIL_ID AilColorImage = AIL.M_NULL;      // Image used for HSL to RGB conversion.
            AIL_INT DefectGrayLevel = 0;            // Gray level under which all is red.
            AIL_INT ExpectedGrayLevel = 0;          // Gray level over which all is blue.
            AIL_INT NbGrayLevels = 0;

            // Number of possible gray levels in corrected depth map.
            NbGrayLevels = (AIL_INT)(1 << (int)SizeBit);

            // Allocate 1-band LUT that will contain hue values.
            AIL.MbufAlloc1d(AilSystem, NbGrayLevels, 8 + AIL.M_UNSIGNED, AIL.M_LUT, ref AilRampLut1Band);

            // Compute limit gray values.
            DefectGrayLevel = (AIL_INT)((EXPECTED_HEIGHT - SATURATED_DEFECT) * SCALE_FACTOR);
            ExpectedGrayLevel = (AIL_INT)(EXPECTED_HEIGHT * SCALE_FACTOR);

            // Create hue values for each possible gray level.
            AIL.MgenLutRamp(AilRampLut1Band, 0, RED_HUE, DefectGrayLevel, RED_HUE);
            AIL.MgenLutRamp(AilRampLut1Band, DefectGrayLevel, RED_HUE, ExpectedGrayLevel, BLUE_HUE);
            AIL.MgenLutRamp(AilRampLut1Band, ExpectedGrayLevel, BLUE_HUE, NbGrayLevels - 1, BLUE_HUE);

            // Create a HSL image buffer.
            AIL.MbufAllocColor(AilSystem, 3, NbGrayLevels, 1, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE, ref AilColorImage);
            AIL.MbufClear(AilColorImage, AIL.M_RGB888(0, FULL_SATURATION, HALF_LUMINANCE));

            // Set its H band (hue) to the LUT contents and convert the image to RGB.
            AIL.MbufCopyColor2d(AilRampLut1Band, AilColorImage, 0, 0, 0, 0, 0, 0, NbGrayLevels, 1);
            AIL.MimConvert(AilColorImage, AilColorImage, AIL.M_HSL_TO_RGB);

            // Create RGB LUT to give to display and copy image contents.
            AIL.MbufAllocColor(AilSystem, 3, NbGrayLevels, 1, 8 + AIL.M_UNSIGNED, AIL.M_LUT, ref AilRampLut3Band);
            AIL.MbufCopy(AilColorImage, AilRampLut3Band);

            // Associates LUT to display.
            AIL.MdispLut(AilDisplay, AilRampLut3Band);

            // Free all allocations.
            AIL.MbufFree(AilRampLut1Band);
            AIL.MbufFree(AilRampLut3Band);
            AIL.MbufFree(AilColorImage);
        }

        // *****************************************************************************
        // Calibrated camera example.
        // *****************************************************************************

        // Input sequence specifications.
        private const string GRID_FILENAME = AIL.M_IMAGE_PATH + "GridForLaser.mim";
        private const string LASERLINE_FILENAME = AIL.M_IMAGE_PATH + "LaserLine.mim";
        private const string OBJECT2_SEQUENCE_FILE = AIL.M_IMAGE_PATH + "Cookie.avi";

        // Camera calibration grid parameters.
        private const int GRID_NB_ROWS = 13;
        private const int GRID_NB_COLS = 12;
        private const double GRID_ROW_SPACING = 5.0;        // in mm
        private const double GRID_COL_SPACING = 5.0;        // in mm

        // Laser device setup parameters.
        private const double CONVEYOR_SPEED = -0.2;         // in mm/frame

        // Fully corrected depth map generation parameters.
        private const int DEPTH_MAP_SIZE_X = 480;           // in pixels
        private const int DEPTH_MAP_SIZE_Y = 480;           // in pixels
        private const double GAP_DEPTH = 1.5;               // in mm

        // D3D display parameters
        private const int D3D_DISPLAY_SIZE_X = 640;
        private const int D3D_DISPLAY_SIZE_Y = 480;

        // Peak detection parameters.
        private const int PEAK_WIDTH_NOMINAL_2 = 9;
        private const int PEAK_WIDTH_DELTA_2 = 7;
        private const int MIN_CONTRAST_2 = 75;

        // Everything below this is considered as noise.
        private const double MIN_HEIGHT_THRESHOLD = 1.0;    // in mm

        private static void CalibratedCameraExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilOverlayImage = AIL.M_NULL;    // Overlay image buffer identifier.
            AIL_ID AilImage = AIL.M_NULL;           // Image buffer identifier (for processing).
            AIL_ID AilCalibration = AIL.M_NULL;     // Calibration context.
            AIL_ID AilDepthMap = AIL.M_NULL;        // Image buffer identifier (for results).
            AIL_ID AilLaser = AIL.M_NULL;           // 3dmap laser profiling context identifier.
            AIL_ID AilCalibScan = AIL.M_NULL;       // 3dmap result buffer identifier for laser
                                                    // line calibration.
            AIL_ID AilScan = AIL.M_NULL;            // 3map result buffer identifier.
            AIL_ID AilContainerId = AIL.M_NULL;     // Point cloud container identifier.
            AIL_ID FillGapsContext = AIL.M_NULL;    // Fill gaps context identifier.
            AIL_INT CalibrationStatus = 0;          // Used to ensure if AIL.McalGrid() worked.
            AIL_INT SizeX = 0;                      // Width of grabbed images.
            AIL_INT SizeY = 0;                      // Height of grabbed images.
            AIL_INT NumberOfImages = 0;             // Number of frames for scanned objects.
            AIL_INT n = 0;                          // Counter.
            double FrameRate = 0.0;                 // Number of grabbed frames per second (in AVI).
            double StartTime = 0.0;                 // Time at the beginning of each iteration.
            double EndTime = 0.0;                   // Time after processing for each iteration.
            double WaitTime = 0.0;                  // Time to wait for next frame.
            double Volume = 0.0;                    // Volume of scanned object.


            Console.WriteLine();
            Console.WriteLine("3D PROFILING AND VOLUME ANALYSIS:");
            Console.WriteLine("---------------------------------");
            Console.WriteLine();
            Console.WriteLine("This program generates fully corrected 3D data of a");
            Console.WriteLine("scanned cookie and computes its volume.");
            Console.WriteLine("The laser (sheet-of-light) profiling system uses a");
            Console.WriteLine("3d-calibrated camera.");
            Console.WriteLine();

            // Load grid image for camera calibration.
            AIL.MbufRestore(GRID_FILENAME, AilSystem, ref AilImage);

            // Select display.
            AIL.MdispSelect(AilDisplay, AilImage);

            Console.WriteLine("Calibrating the camera...");
            Console.WriteLine();

            AIL.MbufInquire(AilImage, AIL.M_SIZE_X, ref SizeX);
            AIL.MbufInquire(AilImage, AIL.M_SIZE_Y, ref SizeY);

            // Allocate calibration context in 3D mode.
            AIL.McalAlloc(AilSystem, AIL.M_TSAI_BASED, AIL.M_DEFAULT, ref AilCalibration);

            // Calibrate the camera.
            AIL.McalGrid(AilCalibration, AilImage, 0.0, 0.0, 0.0, GRID_NB_ROWS, GRID_NB_COLS,
                     GRID_ROW_SPACING, GRID_COL_SPACING, AIL.M_DEFAULT, AIL.M_CHESSBOARD_GRID);

            AIL.McalInquire(AilCalibration, AIL.M_CALIBRATION_STATUS + AIL.M_TYPE_AIL_INT, ref CalibrationStatus);
            if (CalibrationStatus != AIL.M_CALIBRATED)
            {
                AIL.McalFree(AilCalibration);
                AIL.MbufFree(AilImage);
                Console.WriteLine("Camera calibration failed.");
                Console.WriteLine("Press any key to end.");
                Console.WriteLine();
                Console.ReadKey(true);
                return;
            }

            // Prepare for overlay annotations.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN);

            // Draw camera calibration points.
            AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilOverlayImage, AIL.M_DRAW_IMAGE_POINTS,
                                                                              AIL.M_DEFAULT, AIL.M_DEFAULT);

            Console.WriteLine("The camera was calibrated using a chessboard grid.");
            Console.WriteLine();
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Disable overlay.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_DISABLE);

            // Load laser line image.
            AIL.MbufLoad(LASERLINE_FILENAME, AilImage);

            // Allocate 3dmap objects.
            AIL.M3dmapAlloc(AilSystem, AIL.M_LASER, AIL.M_CALIBRATED_CAMERA_LINEAR_MOTION, ref AilLaser);
            AIL.M3dmapAllocResult(AilSystem, AIL.M_LASER_CALIBRATION_DATA, AIL.M_DEFAULT, ref AilCalibScan);

            // Set laser line extraction options.
            AIL_ID AilPeakLocator = AIL.M_NULL;
            AIL.M3dmapInquire(AilLaser, AIL.M_DEFAULT, AIL.M_LOCATE_PEAK_1D_CONTEXT_ID + AIL.M_TYPE_AIL_ID, ref AilPeakLocator);
            AIL.MimControl(AilPeakLocator, AIL.M_PEAK_WIDTH_NOMINAL, PEAK_WIDTH_NOMINAL_2);
            AIL.MimControl(AilPeakLocator, AIL.M_PEAK_WIDTH_DELTA, PEAK_WIDTH_DELTA_2);
            AIL.MimControl(AilPeakLocator, AIL.M_MINIMUM_CONTRAST, MIN_CONTRAST_2);

            // Calibrate laser profiling context.
            AIL.M3dmapAddScan(AilLaser, AilCalibScan, AilImage, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT);
            AIL.M3dmapCalibrate(AilLaser, AilCalibScan, AilCalibration, AIL.M_DEFAULT);

            Console.WriteLine("The laser profiling system has been calibrated using the image");
            Console.WriteLine("of one laser line.");
            Console.WriteLine();
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Free the result buffer use for calibration as it will not be used anymore.
            AIL.M3dmapFree(AilCalibScan);
            AilCalibScan = AIL.M_NULL;

            // Allocate the result buffer to hold the scanned 3D points.
            AIL.M3dmapAllocResult(AilSystem, AIL.M_POINT_CLOUD_RESULT, AIL.M_DEFAULT, ref AilScan);

            // Set speed of scanned object (speed in mm/frame is constant).
            AIL.M3dmapControl(AilLaser, AIL.M_DEFAULT, AIL.M_SCAN_SPEED, CONVEYOR_SPEED);

            // Inquire characteristics of the input sequence.
            AIL.MbufDiskInquire(OBJECT2_SEQUENCE_FILE, AIL.M_NUMBER_OF_IMAGES, ref NumberOfImages);
            AIL.MbufDiskInquire(OBJECT2_SEQUENCE_FILE, AIL.M_FRAME_RATE, ref FrameRate);

            // Open the object sequence file for reading.
            AIL.MbufImportSequence(OBJECT2_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                                    AIL.M_NULL, AIL.M_OPEN);

            Console.WriteLine("The cookie is being scanned to generate 3D data.");
            Console.WriteLine();

            // Read and process all images in the input sequence.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref StartTime);

            for (n = 0; n < NumberOfImages; n++)
            {
                // Read image from sequence.
                AIL.MbufImportSequence(OBJECT2_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_LOAD, AIL.M_NULL, ref AilImage,
                                                                               AIL.M_DEFAULT, 1, AIL.M_READ);

                // Analyze the image to extract laser line and correct its depth.
                AIL.M3dmapAddScan(AilLaser, AilScan, AilImage, AIL.M_NULL, AIL.M_NULL, AIL.M_POINT_CLOUD_LABEL(1), AIL.M_DEFAULT);

                // Wait to have a proper frame rate, if necessary.
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref EndTime);
                WaitTime = (1.0 / FrameRate) - (EndTime - StartTime);
                if (WaitTime > 0)
                {
                    AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_WAIT, ref WaitTime);
                }

                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref StartTime);
            }

            // Close the object sequence file.
            AIL.MbufImportSequence(OBJECT2_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                                   AIL.M_NULL, AIL.M_CLOSE);



            // Convert to AIL.M_CONTAINER for 3D processing.
            AIL.MbufAllocContainer(AilSystem, AIL.M_PROC | AIL.M_DISP, AIL.M_DEFAULT, ref AilContainerId);
            AIL.M3dmapCopyResult(AilScan, AIL.M_ALL, AilContainerId, AIL.M_POINT_CLOUD_UNORGANIZED, AIL.M_DEFAULT);

            // The container's reflectance is 16bits, but only uses the bottom 8.Set the maximum value to display it properly.
            AIL.MbufControlContainer(AilContainerId, AIL.M_COMPONENT_REFLECTANCE, AIL.M_MAX, 255);

            // Allocate image for the fully corrected depth map.
            AIL.MbufAlloc2d(AilSystem, DEPTH_MAP_SIZE_X, DEPTH_MAP_SIZE_Y, 16 + AIL.M_UNSIGNED,
                        AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilDepthMap);

            // Include all points during depth map generation.
            AIL.M3dimCalibrateDepthMap(AilContainerId, AilDepthMap, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_NEGATIVE, AIL.M_DEFAULT);

            // Remove noise in the container close to the Z = 0.
            AIL_ID AilPlane = AIL.M3dgeoAlloc(AilSystem, AIL.M_GEOMETRY, AIL.M_DEFAULT, AIL.M_NULL);
            AIL.M3dgeoPlane(AilPlane, AIL.M_COEFFICIENTS, 0.0, 0.0, 1.0, MIN_HEIGHT_THRESHOLD, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // AIL.M_INVERSE remove what is above the plane.
            AIL.M3dimCrop(AilContainerId, AilContainerId, AilPlane, AIL.M_NULL, AIL.M_SAME, AIL.M_INVERSE);
            AIL.M3dgeoFree(AilPlane);

            Console.WriteLine("Fully corrected 3D data of the cookie is displayed.");
            Console.WriteLine();

            AIL_ID M3dDisplay = Alloc3dDisplayId(AilSystem);
            if (M3dDisplay != AIL.M_NULL)
            {
                Console.WriteLine("Press <R> on the display window to stop/start the rotation.");
                Console.WriteLine();
                AIL.M3ddispSelect(M3dDisplay, AilContainerId, AIL.M_SELECT, AIL.M_DEFAULT);
                AIL.M3ddispSetView(M3dDisplay, AIL.M_AUTO, AIL.M_BOTTOM_TILTED, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
                AIL.M3ddispControl(M3dDisplay, AIL.M_AUTO_ROTATE, AIL.M_ENABLE);
            }

            // Get fully corrected depth map from accumulated information in the result buffer.
            AIL.M3dimProject(AilContainerId, AilDepthMap, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_MIN_Z, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Set fill gaps parameters.
            AIL.M3dimAlloc(AilSystem, AIL.M_FILL_GAPS_CONTEXT, AIL.M_DEFAULT, ref FillGapsContext);
            AIL.M3dimControl(FillGapsContext, AIL.M_FILL_MODE, AIL.M_X_THEN_Y);
            AIL.M3dimControl(FillGapsContext, AIL.M_FILL_SHARP_ELEVATION, AIL.M_MIN);
            AIL.M3dimControl(FillGapsContext, AIL.M_FILL_SHARP_ELEVATION_DEPTH, GAP_DEPTH);
            AIL.M3dimControl(FillGapsContext, AIL.M_FILL_BORDER, AIL.M_DISABLE);

            AIL.M3dimFillGaps(FillGapsContext, AilDepthMap, AIL.M_NULL, AIL.M_DEFAULT);

            // Compute the volume of the depth map.
            AIL.M3dmetVolume(AilDepthMap, AIL.M_XY_PLANE, AIL.M_TOTAL, AIL.M_DEFAULT, ref Volume, AIL.M_NULL);

            Console.WriteLine("Volume of the cookie is {0,4:f1} cm^3.", Volume / 1000.0);
            Console.WriteLine();
            Console.WriteLine("Press any key to end.");
            Console.WriteLine();
            Console.ReadKey(true);

            if (M3dDisplay != AIL.M_NULL)
            {
                AIL.M3ddispFree(M3dDisplay);
            }

            // Free all allocations.
            AIL.M3dimFree(FillGapsContext);
            AIL.MbufFree(AilContainerId);
            AIL.M3dmapFree(AilScan);
            AIL.M3dmapFree(AilLaser);
            AIL.McalFree(AilCalibration);
            AIL.MbufFree(AilDepthMap);
            AIL.MbufFree(AilImage);
        }

        // *****************************************************************************
        // Allocates a 3D display and returns its identifier.
        // *****************************************************************************
        private static AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
        {
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);
            AIL_ID AilDisplay3D = AIL.M3ddispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, AIL.M_NULL);
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE);

            if (AilDisplay3D == AIL.M_NULL)
            {
                Console.WriteLine();
                Console.WriteLine("The current system does not support the 3D display.");
                Console.WriteLine();
            }
            return AilDisplay3D;
        }
    }
}



```
