---
title: "Mmet"
description: "This program uses the metrology module to measure geometric feature and tolerances on parts."
ms.language: "C#"
---

# Mmet

> This program uses the metrology module to measure geometric feature and tolerances on parts.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `McalAssociate`, `McalFree`, `McalFixture`, `McalRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MmetAddFeature`, `MmetAddTolerance`, `MmetAlloc`, `MmetAllocResult`, `MmetCalculate`, `MmetControl`, `MmetDraw`, `MmetFree`, `MmetGetResult`, `MmetSetRegion`, `MmodAllocResult`, `MmodControl`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodPreprocess`, `MmodRestore`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Measuring, Modules, Buffer, Display, Graphics, Calibration, Geometric Model Finder, Metrology, What's New, Older, AIL 10.0 U116

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mmet.cs
//
// Description: This program uses the Metrology module to measure geometric 
//              features and to validate tolerance relationships between features.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MMet
{
    class Program
    {
        // Example selection.
        private const int RUN_SIMPLE_IMAGE_EXAMPLE = AIL.M_YES;
        private const int RUN_COMPLETE_IMAGE_EXAMPLE = AIL.M_YES;

        //*****************************************************************************
        // Main.
        //*****************************************************************************
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System Identifier.     
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.    

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Print module name.
            Console.Write("\nMETROLOGY MODULE:\n");
            Console.Write("-------------------\n\n");

            if (RUN_SIMPLE_IMAGE_EXAMPLE == AIL.M_YES)
            {
                SimpleImageExample(AilSystem, AilDisplay);
            }

            if (RUN_COMPLETE_IMAGE_EXAMPLE == AIL.M_YES)
            {
                CompleteImageExample(AilSystem, AilDisplay);
            }

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        //*****************************************************************************
        // Simple example.
        //*****************************************************************************
        // Source image file specification.
        private const string METROL_SIMPLE_IMAGE_FILE = AIL.M_IMAGE_PATH + "SingleModel.mim";

        // Region parameters
        private const int TOP_RING_POSITION_X = 240;
        private const int TOP_RING_POSITION_Y = 155;
        private const int TOP_RING_START_RADIUS = 2;
        private const int TOP_RING_END_RADIUS = 15;

        private const int MIDDLE_RING_POSITION_X = 240;
        private const int MIDDLE_RING_POSITION_Y = 190;
        private const int MIDDLE_RING_START_RADIUS = 2;
        private const int MIDDLE_RING_END_RADIUS = 15;

        private const int BOTTOM_RECT_POSITION_X = 320;
        private const int BOTTOM_RECT_POSITION_Y = 265;
        private const int BOTTOM_RECT_WIDTH = 170;
        private const int BOTTOM_RECT_HEIGHT = 20;
        private const int BOTTOM_RECT_ANGLE = 180;

        // Tolerance parameters
        private const double PERPENDICULARITY_MIN = 0.5;
        private const double PERPENDICULARITY_MAX = 0.5;

        // Color definitions
        static readonly double FAIL_COLOR = AIL.M_RGB888(255, 0, 0);
        static readonly double PASS_COLOR = AIL.M_RGB888(0, 255, 0);
        static readonly double REGION_COLOR = AIL.M_RGB888(0, 100, 255);
        static readonly double FEATURE_COLOR = AIL.M_RGB888(255, 0, 255);

        static void SimpleImageExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;                    // Image buffer identifier.
            AIL_ID GraphicList = AIL.M_NULL;                 // Graphic list identifier.
            AIL_ID AilMetrolContext = AIL.M_NULL;            // Metrology Context
            AIL_ID AilMetrolResult = AIL.M_NULL;             // Metrology Result

            double Status = 0.0;
            double Value = 0.0;
            AIL_INT[] FeatureIndexForTopConstructedPoint = new AIL_INT[1];
            FeatureIndexForTopConstructedPoint[0] = AIL.M_FEATURE_INDEX(1);
            AIL_INT[] FeatureIndexForMiddleConstructedPoint = new AIL_INT[1];
            FeatureIndexForMiddleConstructedPoint [0] = AIL.M_FEATURE_INDEX(2);
            AIL_INT[] FeatureIndexForConstructedSegment = new AIL_INT[2];
            AIL_INT[] FeatureIndexForTolerance = new AIL_INT[2];
            FeatureIndexForConstructedSegment[0] = AIL.M_FEATURE_INDEX(3);
            FeatureIndexForConstructedSegment[1] = AIL.M_FEATURE_INDEX(4);
            FeatureIndexForTolerance[0] = AIL.M_FEATURE_INDEX(5);
            FeatureIndexForTolerance[1] = AIL.M_FEATURE_INDEX(6);

            // Restore and display the source image.
            AIL.MbufRestore(METROL_SIMPLE_IMAGE_FILE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref GraphicList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

            // Allocate metrology context and result.
            AIL.MmetAlloc(AilSystem, AIL.M_DEFAULT, ref AilMetrolContext);
            AIL.MmetAllocResult(AilSystem, AIL.M_DEFAULT, ref AilMetrolResult);

            // Add a first measured circle feature to context and set its search region
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_CIRCLE, AIL.M_DEFAULT,
                           AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT);

            AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_INDEX(1), AIL.M_DEFAULT, AIL.M_RING,
                          TOP_RING_POSITION_X, TOP_RING_POSITION_Y, TOP_RING_START_RADIUS,
                          TOP_RING_END_RADIUS, AIL.M_NULL, AIL.M_NULL);

            // Add a second measured circle feature to context and set its search region
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_CIRCLE, AIL.M_DEFAULT,
                           AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT);

            AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_INDEX(2), AIL.M_DEFAULT, AIL.M_RING,
                          MIDDLE_RING_POSITION_X, MIDDLE_RING_POSITION_Y, MIDDLE_RING_START_RADIUS,
                          MIDDLE_RING_END_RADIUS, AIL.M_NULL, AIL.M_NULL);

            // Add a first constructed point feature to context
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_CONSTRUCTED, AIL.M_POINT, AIL.M_DEFAULT,
                           AIL.M_CENTER, FeatureIndexForTopConstructedPoint, AIL.M_NULL, 1, AIL.M_DEFAULT);

            // Add a second constructed point feature to context
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_CONSTRUCTED, AIL.M_POINT, AIL.M_DEFAULT,
                           AIL.M_CENTER, FeatureIndexForMiddleConstructedPoint, AIL.M_NULL, 1, AIL.M_DEFAULT);

            // Add a constructed segment feature to context passing through the two points
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_CONSTRUCTED, AIL.M_SEGMENT, AIL.M_DEFAULT,
                           AIL.M_CONSTRUCTION, FeatureIndexForConstructedSegment, AIL.M_NULL, 2, AIL.M_DEFAULT);

            // Add a first segment feature to context and set its search region
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_SEGMENT, AIL.M_DEFAULT,
                           AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT);

            AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_INDEX(6), AIL.M_DEFAULT, AIL.M_RECTANGLE,
                          BOTTOM_RECT_POSITION_X, BOTTOM_RECT_POSITION_Y, BOTTOM_RECT_WIDTH,
                          BOTTOM_RECT_HEIGHT, BOTTOM_RECT_ANGLE, AIL.M_NULL);

            // Add perpendicularity tolerance
            AIL.MmetAddTolerance(AilMetrolContext, AIL.M_PERPENDICULARITY, AIL.M_DEFAULT, PERPENDICULARITY_MIN,
                             PERPENDICULARITY_MAX, FeatureIndexForTolerance, AIL.M_NULL, 2, AIL.M_DEFAULT);

            // Calculate
            AIL.MmetCalculate(AilMetrolContext, AilImage, AilMetrolResult, AIL.M_DEFAULT);

            // Draw region
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, REGION_COLOR);
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_REGION, AIL.M_DEFAULT, AIL.M_DEFAULT);
            Console.Write("Regions used to calculate measured features:\n");
            Console.Write("- two measured circles\n");
            Console.Write("- one measured segment\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Clear annotations.
            AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FEATURE_COLOR);
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_FEATURE, AIL.M_DEFAULT, AIL.M_DEFAULT);
            Console.Write("Calculated features:\n");

            AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_INDEX(1), AIL.M_RADIUS, ref Value);
            Console.Write("- first measured circle:  radius={0:0.00}\n", Value);

            AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_INDEX(2), AIL.M_RADIUS, ref Value);
            Console.Write("- second measured circle: radius={0:0.00}\n", Value);

            AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_INDEX(5), AIL.M_LENGTH, ref Value);
            Console.Write("- constructed segment between the two circle centers: length={0:0.00}\n", Value);

            AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_INDEX(6), AIL.M_LENGTH, ref Value);
            Console.Write("- measured segment: length={0:0.00}\n", Value);

            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Get angularity tolerance status and value
            AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_INDEX(0), AIL.M_STATUS, ref Status);
            AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_INDEX(0), AIL.M_TOLERANCE_VALUE, ref Value);

            if (Status == AIL.M_PASS)
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_COLOR);
                Console.Write("Perpendicularity between the two segments: {0:0.00} degrees.\n", Value);
            }
            else
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_COLOR);
                Console.Write("Perpendicularity between the two segments - Fail.\n");
            }
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_TOLERANCE, AIL.M_TOLERANCE_INDEX(0), AIL.M_DEFAULT);

            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MgraFree(GraphicList);
            AIL.MmetFree(AilMetrolResult);
            AIL.MmetFree(AilMetrolContext);
            AIL.MbufFree(AilImage);
        }

        //****************************************************************************
        // Complete example. 
        //****************************************************************************
        // Source image, calibration and model finder context file specification.
        private const string METROL_CALIBRATION_FILE = AIL.M_IMAGE_PATH + "Hook.mca";
        private const string METROL_COMPLETE_IMAGE_FILE = AIL.M_IMAGE_PATH + "Hook.tif";
        private const string METROL_MODEL_FINDER_FILE = AIL.M_IMAGE_PATH + "Hook.mmf";

        // Region parameters
        private const int CIRCLE1_LABEL = 1;
        private const double RING1_POSITION_X = 1.10;
        private const double RING1_POSITION_Y = 0.80;
        private const double RING1_START_RADIUS = 0.20;
        private const double RING1_END_RADIUS = 0.50;

        private const int CIRCLE2_LABEL = 2;
        private const double RING2_POSITION_X = 1.10;
        private const double RING2_POSITION_Y = 3.00;
        private const double RING2_START_RADIUS = 0.10;
        private const double RING2_END_RADIUS = 0.40;

        private const int SEGMENT1_LABEL = 3;
        private const double RECT1_POSITION_X = 0.10;
        private const double RECT1_POSITION_Y = 2.40;
        private const double RECT1_WIDTH = 1.40;
        private const double RECT1_HEIGHT = 0.30;
        private const double RECT1_ANGLE = 90.0;

        private const int SEGMENT2_LABEL = 4;
        private const double RECT2_POSITION_X = 0.90;
        private const double RECT2_POSITION_Y = 2.80;
        private const double RECT2_WIDTH = 0.40;
        private const double RECT2_HEIGHT = 0.20;
        private const double RECT2_ANGLE = 165.0;

        private const int POINT1_LABEL = 5;
        private const double SEG1_START_X = 1.60;
        private const double SEG1_START_Y = 1.50;
        private const double SEG1_END_X = 1.60;
        private const double SEG1_END_Y = 2.40;

        // Tolerance parameters
        private const int MIN_DISTANCE_LABEL = 1;
        private const int ANGULARITY_LABEL = 2;
        private const int MAX_DISTANCE_LABEL = 3;

        private const double MIN_DISTANCE_VALUE_MIN = 1.40;
        private const double MIN_DISTANCE_VALUE_MAX = 1.60;
        private const double MAX_DISTANCE_VALUE_MIN = 0.40;
        private const double MAX_DISTANCE_VALUE_MAX = 0.60;
        private const double ANGULARITY_VALUE_MIN = 65.0;
        private const double ANGULARITY_VALUE_MAX = 75.0;

        static void CompleteImageExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;                        // Image buffer identifier.
            AIL_ID GraphicList = AIL.M_NULL;                     // Graphic list identifier.
            AIL_ID AilCalibration = AIL.M_NULL;                  // Calibration context
            AIL_ID AilMetrolContext = AIL.M_NULL;                // Metrology Context
            AIL_ID AilMetrolResult = AIL.M_NULL;                 // Metrology Result
            AIL_ID AilModelFinderContext = AIL.M_NULL;           // Model Finder Context
            AIL_ID AilModelFinderResult = AIL.M_NULL;            // Model Finder Result
            AIL_ID FixtureOffset = AIL.M_NULL;                   // Fixture offset

            double Status = 0.0;
            double Value = 0.0;

            AIL_INT[] MinDistanceFeatureLabels = new AIL_INT[2];
            AIL_INT[] AngularityFeatureLabels = new AIL_INT[2];
            AIL_INT[] MaxDistanceFeatureLabels = new AIL_INT[2];
            AIL_INT[] MaxDistanceFeatureIndices = new AIL_INT[2];

            MinDistanceFeatureLabels[0] = CIRCLE1_LABEL;
            MinDistanceFeatureLabels[1] = CIRCLE2_LABEL;

            AngularityFeatureLabels[0] = SEGMENT1_LABEL;
            AngularityFeatureLabels[1] = SEGMENT2_LABEL;

            MaxDistanceFeatureLabels[0] = POINT1_LABEL;
            MaxDistanceFeatureLabels[1] = POINT1_LABEL;

            MaxDistanceFeatureIndices[0] = 0;
            MaxDistanceFeatureIndices[1] = 1;

            // Restore and display the source image.
            AIL.MbufRestore(METROL_COMPLETE_IMAGE_FILE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Restore and associate calibration context to source image
            AIL.McalRestore(METROL_CALIBRATION_FILE, AilSystem, AIL.M_DEFAULT, ref AilCalibration);
            AIL.McalAssociate(AilCalibration, AilImage, AIL.M_DEFAULT);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref GraphicList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

            // Allocate metrology context and result.
            AIL.MmetAlloc(AilSystem, AIL.M_DEFAULT, ref AilMetrolContext);
            AIL.MmetAllocResult(AilSystem, AIL.M_DEFAULT, ref AilMetrolResult);

            // Add a first measured circle feature to context and set its search region
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_CIRCLE, CIRCLE1_LABEL,
                           AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT);

            AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_LABEL(CIRCLE1_LABEL), AIL.M_DEFAULT, AIL.M_RING,
                          RING1_POSITION_X, RING1_POSITION_Y, RING1_START_RADIUS, RING1_END_RADIUS,
                          AIL.M_NULL, AIL.M_NULL);

            // Add a second measured circle feature to context and set its search region
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_CIRCLE, CIRCLE2_LABEL,
                           AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT);

            AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_LABEL(CIRCLE2_LABEL), AIL.M_DEFAULT, AIL.M_RING,
                          RING2_POSITION_X, RING2_POSITION_Y, RING2_START_RADIUS, RING2_END_RADIUS,
                          AIL.M_NULL, AIL.M_NULL);

            // Add a first measured segment feature to context and set its search region
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_SEGMENT, SEGMENT1_LABEL,
                           AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT);

            AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_LABEL(SEGMENT1_LABEL), AIL.M_DEFAULT, AIL.M_RECTANGLE,
                          RECT1_POSITION_X, RECT1_POSITION_Y, RECT1_WIDTH, RECT1_HEIGHT,
                          RECT1_ANGLE, AIL.M_NULL);

            AIL.MmetControl(AilMetrolContext, AIL.M_FEATURE_LABEL(SEGMENT1_LABEL), AIL.M_EDGEL_ANGLE_RANGE, 10);

            // Add a second measured segment feature to context and set its search region
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_SEGMENT, SEGMENT2_LABEL,
                           AIL.M_INNER_FIT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT);

            AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_LABEL(SEGMENT2_LABEL), AIL.M_DEFAULT, AIL.M_RECTANGLE,
                          RECT2_POSITION_X, RECT2_POSITION_Y, RECT2_WIDTH, RECT2_HEIGHT,
                          RECT2_ANGLE, AIL.M_NULL);

            // Add a measured point feature to context and set its search region
            AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_POINT, POINT1_LABEL,
                           AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT);

            AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_LABEL(POINT1_LABEL), AIL.M_DEFAULT, AIL.M_SEGMENT,
                          SEG1_START_X, SEG1_START_Y, SEG1_END_X, SEG1_END_Y,
                          AIL.M_NULL, AIL.M_NULL);

            // Set the polarity and the maximum number of points to detect along the segment region
            AIL.MmetControl(AilMetrolContext, AIL.M_FEATURE_LABEL(POINT1_LABEL), AIL.M_EDGEL_RELATIVE_ANGLE, AIL.M_SAME_OR_REVERSE);
            AIL.MmetControl(AilMetrolContext, AIL.M_FEATURE_LABEL(POINT1_LABEL), AIL.M_NUMBER_MAX, 2);

            // Add minimum distance tolerance
            AIL.MmetAddTolerance(AilMetrolContext, AIL.M_DISTANCE_MIN, MIN_DISTANCE_LABEL,
                             MIN_DISTANCE_VALUE_MIN, MIN_DISTANCE_VALUE_MAX, MinDistanceFeatureLabels,
                             AIL.M_NULL, 2, AIL.M_DEFAULT);

            // Add angularity tolerance
            AIL.MmetAddTolerance(AilMetrolContext, AIL.M_ANGULARITY, ANGULARITY_LABEL,
                             ANGULARITY_VALUE_MIN, ANGULARITY_VALUE_MAX, AngularityFeatureLabels,
                             AIL.M_NULL, 2, AIL.M_DEFAULT);


            // Add maximum distance tolerance
            AIL.MmetAddTolerance(AilMetrolContext, AIL.M_DISTANCE_MAX, MAX_DISTANCE_LABEL,
                             MAX_DISTANCE_VALUE_MIN, MAX_DISTANCE_VALUE_MAX, MaxDistanceFeatureLabels,
                             MaxDistanceFeatureIndices, 2, AIL.M_DEFAULT);

            // Calculate
            AIL.MmetCalculate(AilMetrolContext, AilImage, AilMetrolResult, AIL.M_DEFAULT);

            // Draw features
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, REGION_COLOR);
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_REGION, AIL.M_DEFAULT, AIL.M_DEFAULT);
            Console.Write("Regions used to calculate measured features:\n");
            Console.Write("- two measured circle features\n");
            Console.Write("- two measured segment features\n");
            Console.Write("- one measured points feature\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Clear annotations.
            AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FEATURE_COLOR);
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_FEATURE, AIL.M_DEFAULT, AIL.M_DEFAULT);
            Console.Write("Calculated features:\n");

            AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(CIRCLE1_LABEL), AIL.M_RADIUS, ref Value);
            Console.Write("- first measured circle:   radius={0:0.00}mm\n", Value);

            AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(CIRCLE2_LABEL), AIL.M_RADIUS, ref Value);
            Console.Write("- second measured circle:  radius={0:0.00}mm\n", Value);

            AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(SEGMENT1_LABEL), AIL.M_LENGTH, ref Value);
            Console.Write("- first measured segment:  length={0:0.00}mm\n", Value);

            AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(SEGMENT2_LABEL), AIL.M_LENGTH, ref Value);
            Console.Write("- second measured segment: length={0:0.00}mm\n", Value);

            Console.Write("- two measured points\n");

            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Get angularity tolerance status and value
            AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(ANGULARITY_LABEL), AIL.M_STATUS, ref Status);
            AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(ANGULARITY_LABEL), AIL.M_TOLERANCE_VALUE, ref Value);

            if (Status == AIL.M_PASS)
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_COLOR);
                Console.Write("Angularity value: {0:0.00} degrees.\n", Value);
            }
            else
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_COLOR);
                Console.Write("Angularity value - Fail.\n");
            }
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_TOLERANCE, AIL.M_TOLERANCE_LABEL(ANGULARITY_LABEL), AIL.M_DEFAULT);

            // Get min distance tolerance status and value
            AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), AIL.M_STATUS, ref Status);
            AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), AIL.M_TOLERANCE_VALUE, ref Value);

            if (Status == AIL.M_PASS)
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_COLOR);
                Console.Write("Min distance tolerance value: {0:0.00} mm.\n", Value);
            }
            else
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_COLOR);
                Console.Write("Min distance tolerance value - Fail.\n");
            }
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_TOLERANCE, AIL.M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), AIL.M_DEFAULT);

            // Get max distance tolerance status and value
            AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), AIL.M_STATUS, ref Status);
            AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), AIL.M_TOLERANCE_VALUE, ref Value);

            if (Status == AIL.M_PASS)
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_COLOR);
                Console.Write("Max distance tolerance value: {0:0.00} mm.\n", Value);
            }
            else
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_COLOR);
                Console.Write("Max distance tolerance value - Fail.\n");
            }
            AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_TOLERANCE, AIL.M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), AIL.M_DEFAULT);

            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Clear annotations.
            AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

            // Restore the model finder context and calibrate it
            AIL.MmodRestore(METROL_MODEL_FINDER_FILE, AilSystem, AIL.M_DEFAULT, ref AilModelFinderContext);
            AIL.MmodControl(AilModelFinderContext, 0, AIL.M_ASSOCIATED_CALIBRATION, AilCalibration);

            // Allocate a result buffer
            AIL.MmodAllocResult(AilSystem, AIL.M_DEFAULT, ref AilModelFinderResult);

            // Find object occurrence
            AIL.MmodPreprocess(AilModelFinderContext, AIL.M_DEFAULT);
            AIL.MmodFind(AilModelFinderContext, AilImage, AilModelFinderResult);

            // Get number of found occurrences
            AIL.MmodGetResult(AilModelFinderResult, AIL.M_GENERAL, AIL.M_NUMBER, ref Value);

            if (Value == 1)
            {
                AIL.MmodDraw(AIL.M_DEFAULT, AilModelFinderResult, GraphicList, AIL.M_DRAW_POSITION + AIL.M_DRAW_BOX, AIL.M_DEFAULT, AIL.M_DEFAULT);
                Console.Write("Found occurrence using Model Finder.\n");
                Console.Write("Press any key to continue.\n\n");
                Console.ReadKey(true);

                // Clear annotations.
                AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

                // Set the new context position
                FixtureOffset = AIL.McalAlloc(AilSystem, AIL.M_FIXTURING_OFFSET, AIL.M_DEFAULT, AIL.M_NULL);
                AIL.McalFixture(AilImage, FixtureOffset, AIL.M_MOVE_RELATIVE, AIL.M_RESULT_MOD, AilModelFinderResult, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Calculate
                AIL.MmetCalculate(AilMetrolContext, AilImage, AilMetrolResult, AIL.M_DEFAULT);

                // Draw features
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, REGION_COLOR);
                AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_REGION, AIL.M_DEFAULT, AIL.M_DEFAULT);
                Console.Write("Regions used to calculate measured features at the new location.\n");
                Console.Write("Press any key to continue.\n\n");
                Console.ReadKey(true);

                // Clear annotations.
                AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FEATURE_COLOR);
                AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_FEATURE, AIL.M_DEFAULT, AIL.M_DEFAULT);
                Console.Write("Calculated features.\n");

                AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(CIRCLE1_LABEL), AIL.M_RADIUS, ref Value);
                Console.Write("- first measured circle:   radius={0:0.00}mm\n", Value);

                AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(CIRCLE2_LABEL), AIL.M_RADIUS, ref Value);
                Console.Write("- second measured circle:  radius={0:0.00}mm\n", Value);

                AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(SEGMENT1_LABEL), AIL.M_LENGTH, ref Value);
                Console.Write("- first measured segment:  length={0:0.00}mm\n", Value);

                AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(SEGMENT2_LABEL), AIL.M_LENGTH, ref Value);
                Console.Write("- second measured segment: length={0:0.00}mm\n", Value);

                Console.Write("- two measured points\n");

                Console.Write("Press any key to continue.\n\n");
                Console.ReadKey(true);

                // Get angularity tolerance status and value
                AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(ANGULARITY_LABEL), AIL.M_STATUS, ref Status);
                AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(ANGULARITY_LABEL), AIL.M_TOLERANCE_VALUE, ref Value);

                if (Status == AIL.M_PASS)
                {
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_COLOR);
                    Console.Write("Angularity value: {0:0.00} degrees.\n", Value);
                }
                else
                {
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_COLOR);
                    Console.Write("Angularity value - Fail.\n");
                }
                AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_TOLERANCE, AIL.M_TOLERANCE_LABEL(ANGULARITY_LABEL), AIL.M_DEFAULT);

                // Get min distance tolerance status and value
                AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), AIL.M_STATUS, ref Status);
                AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), AIL.M_TOLERANCE_VALUE, ref Value);

                if (Status == AIL.M_PASS)
                {
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_COLOR);
                    Console.Write("Min distance tolerance value: {0:0.00} mm.\n", Value);
                }
                else
                {
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_COLOR);
                    Console.Write("Min distance tolerance value - Fail.\n");
                }
                AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_TOLERANCE, AIL.M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), AIL.M_DEFAULT);

                // Get max distance tolerance status and value
                AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), AIL.M_STATUS, ref Status);
                AIL.MmetGetResult(AilMetrolResult, AIL.M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), AIL.M_TOLERANCE_VALUE, ref Value);

                if (Status == AIL.M_PASS)
                {
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_COLOR);
                    Console.Write("Max distance tolerance value: {0:0.00} mm.\n", Value);
                }
                else
                {
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_COLOR);
                    Console.Write("Max distance tolerance value - Fail.\n");
                }
                AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, GraphicList, AIL.M_DRAW_TOLERANCE, AIL.M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), AIL.M_DEFAULT);

                Console.Write("Press any key to quit.\n\n");
                Console.ReadKey(true);

                // Free the calfixture context
                AIL.McalFree(FixtureOffset);
            }
            else
            {
                Console.Write("Occurrence not found.\n");
                Console.Write("Press any key to quit.\n\n");
                Console.ReadKey(true);
            }

            // Free all allocations.
            AIL.MgraFree(GraphicList);
            AIL.MmodFree(AilModelFinderContext);
            AIL.MmodFree(AilModelFinderResult);
            AIL.MmetFree(AilMetrolResult);
            AIL.MmetFree(AilMetrolContext);
            AIL.McalFree(AilCalibration);
            AIL.MbufFree(AilImage);
        }
    }
}

```
