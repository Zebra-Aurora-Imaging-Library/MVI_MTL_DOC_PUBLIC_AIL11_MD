---
title: "Mmeas"
description: "This example uses the Measurement module to calculate the position, width and angle of objects in an image."
ms.language: "C#"
---

# Mmeas

> This example uses the Measurement module to calculate the position, width and angle of objects in an image.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufRestore`, `McalAlloc`, `McalFixture`, `McalFree`, `McalUniform`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraControl`, `MgraFree`, `MgraText`, `MimConvert`, `MmeasAllocMarker`, `MmeasDraw`, `MmeasFindMarker`, `MmeasFree`, `MmeasGetResult`, `MmeasSetMarker`, `MmeasSetScore`, `MmodAlloc`, `MmodAllocResult`, `MmodControl`, `MmodDefine`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Semiconductor, Electronics, Applications, Measuring, Counting, Modules, Buffer, Display, Graphics, Image Processing, Calibration, Geometric Model Finder, Measurement, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mmeas.cs
//
// Description: This program consists of 3 examples that use the Measurement module 
//              to calculate the position, width and angle of objects in an image. 
//              The first one measures the position, width and angle of a stripe
//              in an image, and marks its center and edges. The second one measures
//              the average position, width and angle of a row of pins on a chip.
//              Finally the third example uses the fixturing capability to measure
//              the gap width of objects relative to the object's positions.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MMeas
{
    class Program
    {
        // Example selection.
        private const int RUN_SINGLE_MEASUREMENT_EXAMPLE = AIL.M_YES;
        private const int RUN_MULTIPLE_MEASUREMENT_EXAMPLE = AIL.M_YES;
        private const int RUN_FIXTURED_MEASUREMENT_EXAMPLE = AIL.M_YES;

        static int Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System Identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Print module name.
            Console.Write("\nMEASUREMENT MODULE:\n");
            Console.Write("-------------------\n\n");

            if (RUN_SINGLE_MEASUREMENT_EXAMPLE == AIL.M_YES)
            {
                SingleMeasurementExample(AilSystem, AilDisplay);
            }

            if (RUN_MULTIPLE_MEASUREMENT_EXAMPLE == AIL.M_YES)
            {
                MultipleMeasurementExample(AilSystem, AilDisplay);
            }

            if (RUN_MULTIPLE_MEASUREMENT_EXAMPLE == AIL.M_YES)
            {
                FixturedMeasurementExample(AilSystem, AilDisplay);
            }

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);

            return 0;
        }

        //****************************************************************************
        //Single measurement example. 
        //****************************************************************************

        // Source image file specification.
        private const string MEAS_IMAGE_FILE = AIL.M_IMAGE_PATH + "lead.mim";

        // Processing region specification.
        private const int MEAS_BOX_WIDTH = 128;
        private const int MEAS_BOX_HEIGHT = 100;
        private const int MEAS_BOX_POS_X = 166;
        private const int MEAS_BOX_POS_Y = 130;

        // Target stripe typical specifications.
        private const int STRIPE_POLARITY_LEFT = AIL.M_POSITIVE;
        private const int STRIPE_POLARITY_RIGHT = AIL.M_NEGATIVE;
        private const int STRIPE_WIDTH = 45;
        private const int STRIPE_WIDTH_VARIATION = 10;

        static void SingleMeasurementExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;                   // Image buffer identifier.
            AIL_ID AilGraphicList = AIL.M_NULL;             // Graphic list identifier.
            AIL_ID StripeMarker = AIL.M_NULL;               // Stripe marker identifier.
            double StripeCenterX = 0.0;                     // Stripe X center position.
            double StripeCenterY = 0.0;                     // Stripe Y center position.
            double StripeWidth = 0.0;                       // Stripe width.
            double StripeAngle = 0.0;                       // Stripe angle.
            double CrossColor = AIL.M_COLOR_YELLOW;         // Cross drawing color.
            double BoxColor = AIL.M_COLOR_RED;              // Box drawing color.

            // Restore and display the source image.
            AIL.MbufRestore(MEAS_IMAGE_FILE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref AilGraphicList);

            // Associate the graphic list to the display.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

            // Allocate a stripe marker.
            AIL.MmeasAllocMarker(AilSystem, AIL.M_STRIPE, AIL.M_DEFAULT, ref StripeMarker);

            // Specify the stripe characteristics.
            AIL.MmeasSetMarker(StripeMarker, AIL.M_POLARITY, STRIPE_POLARITY_LEFT, STRIPE_POLARITY_RIGHT);

            // Set score function to find the expected width.
            AIL.MmeasSetScore(StripeMarker,
                AIL.M_STRIPE_WIDTH_SCORE,
                STRIPE_WIDTH - STRIPE_WIDTH_VARIATION,
                STRIPE_WIDTH - STRIPE_WIDTH_VARIATION,
                STRIPE_WIDTH + STRIPE_WIDTH_VARIATION,
                STRIPE_WIDTH + STRIPE_WIDTH_VARIATION,
                AIL.M_DEFAULT,
                AIL.M_DEFAULT,
                AIL.M_DEFAULT);

            AIL.MmeasSetMarker(StripeMarker, AIL.M_BOX_ANGLE_MODE, AIL.M_ENABLE, AIL.M_NULL);

            // Specify the search region size and position.
            AIL.MmeasSetMarker(StripeMarker, AIL.M_BOX_ORIGIN, MEAS_BOX_POS_X, MEAS_BOX_POS_Y);
            AIL.MmeasSetMarker(StripeMarker, AIL.M_BOX_SIZE, MEAS_BOX_WIDTH, MEAS_BOX_HEIGHT);

            // Draw the contour of the measurement region.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, BoxColor);
            AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_SEARCH_REGION, AIL.M_DEFAULT, AIL.M_MARKER);

            // Pause to show the original image.
            Console.Write("Position, width and angle of the stripe in the highlighted box\n");
            Console.Write("will be calculated and the center and edges will be marked.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Clear the annotations.
            AIL.MgraClear(AIL.M_DEFAULT, AilGraphicList);

            // Find the stripe and measure its width and angle.
            AIL.MmeasFindMarker(AIL.M_DEFAULT, AilImage, StripeMarker, AIL.M_DEFAULT);

            // Get the stripe position, width and angle.
            AIL.MmeasGetResult(StripeMarker, AIL.M_POSITION, ref StripeCenterX, ref StripeCenterY);
            AIL.MmeasGetResult(StripeMarker, AIL.M_STRIPE_WIDTH, ref StripeWidth);
            AIL.MmeasGetResult(StripeMarker, AIL.M_ANGLE, ref StripeAngle);

            // Draw the contour of the found measurement box.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, BoxColor);
            AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_BOX, AIL.M_DEFAULT, AIL.M_RESULT);

            // Draw a cross on the center, left edge and right edge of the found stripe.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, CrossColor);
            AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_RESULT);
            AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_POSITION + AIL.M_EDGE_FIRST + AIL.M_EDGE_SECOND, AIL.M_DEFAULT, AIL.M_RESULT);

            // Print the result.
            Console.Write("The stripe in the image is at position {0:0.00},{1:0.00} and\n", StripeCenterX, StripeCenterY);
            Console.Write("is {0:0.00} pixels wide with an angle of {1:0.00} degrees.\n", StripeWidth, StripeAngle);
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Remove the graphic list association to the display.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AIL.M_NULL);

            // Free all allocations.
            AIL.MgraFree(AilGraphicList);
            AIL.MmeasFree(StripeMarker);
            AIL.MbufFree(AilImage);
        }

        //****************************************************************************
        // Multiple measurement example. 
        //****************************************************************************

        // Source image file specification.
        private const string MULT_MEAS_IMAGE_FILE = AIL.M_IMAGE_PATH + "chip.mim";

        // Processing region specification.
        private const int MULT_MEAS_BOX_WIDTH = 230;
        private const int MULT_MEAS_BOX_HEIGHT = 7;
        private const int MULT_MEAS_BOX_POS_X = 220;
        private const int MULT_MEAS_BOX_POS_Y = 171;

        // Target stripe specifications.
        private const int MULT_STRIPES_ORIENTATION = AIL.M_VERTICAL;
        private const int MULT_STRIPES_POLARITY_LEFT = AIL.M_POSITIVE;
        private const int MULT_STRIPES_POLARITY_RIGHT = AIL.M_NEGATIVE;
        private const int MULT_STRIPES_NUMBER = 12;

        static void MultipleMeasurementExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;                   // Image buffer identifier. 
            AIL_ID AilGraphicList = AIL.M_NULL;             // Graphic list identifier.
            AIL_ID StripeMarker = AIL.M_NULL;               // Stripe marker identifier.
            double MeanAngle = 0.0;                         // Stripe mean angle.
            double MeanWidth = 0.0;                         // Stripe mean width.
            double MeanSpacing = 0.0;                       // Stripe mean spacing.
            double CrossColor = AIL.M_COLOR_YELLOW;         // Cross drawing color.
            double BoxColor = AIL.M_COLOR_RED;              // Box drawing color.

            // Restore and display the source image.
            AIL.MbufRestore(MULT_MEAS_IMAGE_FILE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref AilGraphicList);

            // Associate the graphic list to the display.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

            // Allocate a stripe marker.
            AIL.MmeasAllocMarker(AilSystem, AIL.M_STRIPE, AIL.M_DEFAULT, ref StripeMarker);

            // Specify the stripe characteristics.
            AIL.MmeasSetMarker(StripeMarker, AIL.M_NUMBER, MULT_STRIPES_NUMBER, AIL.M_NULL);
            AIL.MmeasSetMarker(StripeMarker, AIL.M_POLARITY, MULT_STRIPES_POLARITY_LEFT, MULT_STRIPES_POLARITY_RIGHT);
            AIL.MmeasSetMarker(StripeMarker, AIL.M_ORIENTATION, MULT_STRIPES_ORIENTATION, AIL.M_NULL);

            // Specify the measurement box size and position.
            AIL.MmeasSetMarker(StripeMarker, AIL.M_BOX_ORIGIN, MULT_MEAS_BOX_POS_X, MULT_MEAS_BOX_POS_Y);
            AIL.MmeasSetMarker(StripeMarker, AIL.M_BOX_SIZE, MULT_MEAS_BOX_WIDTH, MULT_MEAS_BOX_HEIGHT);

            // Draw the contour of the measurement region.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, BoxColor);
            AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_SEARCH_REGION, AIL.M_DEFAULT, AIL.M_MARKER);

            // Pause to show the original image.
            Console.Write("The position and angle of a row of pins on a chip will be calculated.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Find the stripe and measure its width and angle.
            AIL.MmeasFindMarker(AIL.M_DEFAULT, AilImage, StripeMarker, AIL.M_POSITION + AIL.M_ANGLE + AIL.M_STRIPE_WIDTH);

            // Draw the contour of the measurement region.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, BoxColor);
            AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_SEARCH_REGION, AIL.M_DEFAULT, AIL.M_RESULT);

            // Draw a cross at the center of each stripe found.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, CrossColor);
            AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_POSITION, AIL.M_RESULT_ALL_OCCURRENCES, AIL.M_RESULT);

            // Get the stripe's width, angle and spacing.
            AIL.MmeasGetResult(StripeMarker, AIL.M_ANGLE + AIL.M_MEAN, ref MeanAngle);
            AIL.MmeasGetResult(StripeMarker, AIL.M_STRIPE_WIDTH + AIL.M_MEAN, ref MeanWidth);
            AIL.MmeasGetResult(StripeMarker, AIL.M_SPACING + AIL.M_MEAN, ref MeanSpacing);

            // Print the results.
            Console.Write("The center and angle of each pin have been marked.\n\n");
            Console.Write("The statistics for the pins are:\n");
            Console.Write("Average angle   : {0,5:0.00}\n", MeanAngle);
            Console.Write("Average width   : {0,5:0.00}\n", MeanWidth);
            Console.Write("Average spacing : {0,5:0.00}\n", MeanSpacing);
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Remove the graphic list association to the display.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AIL.M_NULL);

            // Free all allocations.
            AIL.MgraFree(AilGraphicList);
            AIL.MmeasFree(StripeMarker);
            AIL.MbufFree(AilImage);
        }

        //*****************************************************************************
        // Fixtured measurement example. 
        //*****************************************************************************

        // Source image file specification.
        private const string FIXTURED_MEAS_IMAGE_FILE = AIL.M_IMAGE_PATH + "Fuse.mim";

        // Processing region specification.
        private const int FIXTURED_MEAS_BOX_OFFSET_X = 400;
        private const int FIXTURED_MEAS_BOX_OFFSET_Y = 290;
        private const int FIXTURED_MEAS_BOX_WIDTH = 100;
        private const int FIXTURED_MEAS_BOX_HEIGHT = 15;

        // Model region specification.
        private const int FIXTURED_MODEL_OFFSET_X = 395;
        private const int FIXTURED_MODEL_OFFSET_Y = 200;
        private const int FIXTURED_MODEL_SIZE_X = 110;
        private const int FIXTURED_MODEL_SIZE_Y = 120;

        private const int FIXTURED_IMAGE_SIZE_X = 512;
        private const int FIXTURED_IMAGE_SIZE_Y = 384;

        // Target stripe typical specifications.
        private const int FIXTURED_STRIPE_POLARITY_LEFT = AIL.M_POSITIVE;
        private const int FIXTURED_STRIPE_POLARITY_RIGHT = AIL.M_OPPOSITE;

        private static void FixturedMeasurementExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilSourceImage = AIL.M_NULL;             // Source image buffer identifier.
            AIL_ID AilImage = AIL.M_NULL;                   // Image buffer identifier.
            AIL_ID AilModContext = AIL.M_NULL;              // Model finder context identifier.
            AIL_ID AilModResult = AIL.M_NULL;               // Model finder result identifier.
            AIL_ID AilFixturingOffset = AIL.M_NULL;         // Fixturing object identifier.
            AIL_ID StripeMarker = AIL.M_NULL;               // Stripe marker identifier.
            AIL_ID AilGraphicList = AIL.M_NULL;             // Graphic list identifier.

            double StripeWidth = 0.0;                       // Stripe width.
            double PositionX = 0.0;                         // Occurrence position X.
            double PositionY = 0.0;                         // Occurrence position Y.

            AIL_INT SizeX = 0;                              // Source image size X.
            AIL_INT SizeY = 0;                              // Source image size Y.
            AIL_INT NbOccurrences = 0;                      // Number of found occurrences.
            AIL_INT NbStripes = 0;                          // Number of found stripes.
            AIL_INT Index = 0;                              // Occurrence index.

            // Restore the source image.
            AIL.MbufRestore(FIXTURED_MEAS_IMAGE_FILE, AilSystem, ref AilSourceImage);
            AIL.MbufInquire(AilSourceImage, AIL.M_SIZE_X, ref SizeX);
            AIL.MbufInquire(AilSourceImage, AIL.M_SIZE_Y, ref SizeY);

            // Allocate, then compute the source image luminance.
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilImage);
            AIL.MimConvert(AilSourceImage, AilImage, AIL.M_RGB_TO_L);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref AilGraphicList);

            // Select the image to the display.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Associate the graphic list to the display.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

            // Allocate a stripe marker.
            AIL.MmeasAllocMarker(AilSystem, AIL.M_STRIPE, AIL.M_DEFAULT, ref StripeMarker);

            // Set inputs units to world in order to fixture the region.
            AIL.MmeasSetMarker(StripeMarker, AIL.M_SEARCH_REGION_INPUT_UNITS, AIL.M_WORLD, AIL.M_NULL);

            // Calibrate the destination image to receive the world units annotations.
            AIL.McalUniform(AilImage, 0.0, 0.0, 1.0, 1.0, 0.0, AIL.M_DEFAULT);

            // Specify the stripe characteristics.
            AIL.MmeasSetMarker(StripeMarker, AIL.M_BOX_ORIGIN, FIXTURED_MEAS_BOX_OFFSET_X, FIXTURED_MEAS_BOX_OFFSET_Y);
            AIL.MmeasSetMarker(StripeMarker, AIL.M_POLARITY, FIXTURED_STRIPE_POLARITY_LEFT, FIXTURED_STRIPE_POLARITY_RIGHT);
            AIL.MmeasSetMarker(StripeMarker, AIL.M_SEARCH_REGION_CLIPPING, AIL.M_ENABLE, AIL.M_NULL);

            // Specify the search region size and position.
            AIL.MmeasSetMarker(StripeMarker, AIL.M_BOX_SIZE, FIXTURED_MEAS_BOX_WIDTH, FIXTURED_MEAS_BOX_HEIGHT);

            // Draw the contour of the measurement region.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_BLUE);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_INPUT_UNITS, AIL.M_WORLD);
            AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_SEARCH_REGION, AIL.M_DEFAULT, AIL.M_MARKER);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_INPUT_UNITS, AIL.M_PIXEL);

            Console.Write("A measurement stripe marker (in blue) is defined.\n");

            // Define model to further fixture the measurement marker.
            AIL.MmodAlloc(AilSystem, AIL.M_GEOMETRIC, AIL.M_DEFAULT, ref AilModContext);

            AIL.MmodAllocResult(AilSystem, AIL.M_DEFAULT, ref AilModResult);

            AIL.MmodDefine(AilModContext, AIL.M_IMAGE, AilImage,
               FIXTURED_MODEL_OFFSET_X, FIXTURED_MODEL_OFFSET_Y,
               FIXTURED_MODEL_SIZE_X, FIXTURED_MODEL_SIZE_Y);

            AIL.MmodControl(AilModContext, AIL.M_DEFAULT, AIL.M_NUMBER, AIL.M_ALL);
            AIL.MmodControl(AilModContext, AIL.M_CONTEXT, AIL.M_SPEED, AIL.M_VERY_HIGH);

            // Preprocess the model.
            AIL.MmodPreprocess(AilModContext, AIL.M_DEFAULT);

            // Display the model.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN);
            AIL.MmodDraw(AIL.M_DEFAULT, AilModContext, AilGraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_ORIGINAL);
            Console.Write("A Model Finder model (in green) is defined to\n");
            Console.Write("further fixture the measurement operation.\n\n");

            Console.Write("The stripe marker determines the gap between\n");
            Console.Write("the fuse connectors. Model Finder tracks the\n");
            Console.Write("fuses while the attached fixturing automatically\n");
            Console.Write("relocates the marker relative to the found fuses.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Allocate a fixture object.
            AIL.McalAlloc(AilSystem, AIL.M_FIXTURING_OFFSET, AIL.M_DEFAULT, ref AilFixturingOffset);

            // Learn the relative offset of the model.
            AIL.McalFixture(AIL.M_NULL, AilFixturingOffset, AIL.M_LEARN_OFFSET, AIL.M_MODEL_MOD,
               AilModContext, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Find the location of the fixtures using Model Finder.
            AIL.MmodFind(AilModContext, AilImage, AilModResult);

            // Display and retrieve the number of occurrences found.
            AIL.MgraClear(AIL.M_DEFAULT, AilGraphicList);
            AIL.MmodDraw(AIL.M_DEFAULT, AilModResult, AilGraphicList, AIL.M_DRAW_POSITION + AIL.M_DRAW_BOX, AIL.M_DEFAULT, AIL.M_DEFAULT);

            AIL.MmodGetResult(AilModResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NbOccurrences);

            Console.Write("Locating the parts: {0} occurrences are found.\n", NbOccurrences);
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            Console.Write("The measurement tool is moved relative to each piece.\n");
            Console.Write("A graphic list is used to display the results with\n");
            Console.Write("subpixel annotations.\n\n");

            // Clear the annotations.
            AIL.MgraClear(AIL.M_DEFAULT, AilGraphicList);

            for (Index = 0; Index < NbOccurrences; Index++)
            {
                // Display the found model occurrence position.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN);
                AIL.MmodDraw(AIL.M_DEFAULT, AilModResult, AilGraphicList, AIL.M_DRAW_POSITION, Index, AIL.M_DEFAULT);

                AIL.MmodGetResult(AilModResult, Index, AIL.M_POSITION_X + AIL.M_TYPE_AIL_DOUBLE, ref PositionX);
                AIL.MmodGetResult(AilModResult, Index, AIL.M_POSITION_Y + AIL.M_TYPE_AIL_DOUBLE, ref PositionY);

                AIL.MgraText(AIL.M_DEFAULT, AilGraphicList, PositionX - 20, PositionY, Index.ToString());

                // Apply a fixture offset to the implicit 1:1 calibration of the target image.
                AIL.McalFixture(AilImage, AilFixturingOffset, AIL.M_MOVE_RELATIVE, AIL.M_RESULT_MOD,
                   AilModResult, Index, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Find the stripe and measure its width and angle.
                AIL.MmeasFindMarker(AIL.M_DEFAULT, AilImage, StripeMarker, AIL.M_POSITION + AIL.M_STRIPE_WIDTH);

                // Get the number of found results.
                AIL.MmeasGetResult(StripeMarker, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NbStripes, AIL.M_NULL);

                if (NbStripes == 1)
                {
                    // Get the stripe width.
                    AIL.MmeasGetResult(StripeMarker, AIL.M_STRIPE_WIDTH, ref StripeWidth, AIL.M_NULL);

                    // Draw the contour of the found measurement region.
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_BLUE);
                    AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_SEARCH_REGION, AIL.M_DEFAULT, AIL.M_RESULT);

                    // Draw a cross on the center, left edge and right edge of the found stripe.
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);
                    AIL.MmeasDraw(AIL.M_DEFAULT, StripeMarker, AilGraphicList, AIL.M_DRAW_WIDTH, AIL.M_DEFAULT, AIL.M_RESULT);

                    // Print the result.
                    Console.Write("The gap (in red) of occurrence {0} is {1:0.00} pixels wide.\n", Index, StripeWidth);
                }
                else
                {
                    Console.Write("The gap of occurrence {0} could not be measured.\n", Index);
                }
            }

            Console.Write("Press any key to end.\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MgraFree(AilGraphicList);
            AIL.MmeasFree(StripeMarker);
            AIL.MmodFree(AilModContext);
            AIL.MmodFree(AilModResult);
            AIL.McalFree(AilFixturingOffset);
            AIL.MbufFree(AilImage);
            AIL.MbufFree(AilSourceImage);
        }

    }
}

```
