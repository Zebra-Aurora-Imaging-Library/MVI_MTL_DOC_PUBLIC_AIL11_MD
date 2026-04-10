---
title: "Mcol"
description: "This program uses the color module to perform color segmentation, color identification, color separation and color to grayscale conversion."
ms.language: "C#"
---

# Mcol

> This program uses the color module to perform color segmentation, color identification, color separation and color to grayscale conversion.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufChild2d`, `MbufChildColor2d`, `MbufClear`, `MbufCopy`, `MbufDiskInquire`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufPut2d`, `McolAlloc`, `McolAllocResult`, `McolControl`, `McolDefine`, `McolDraw`, `McolFree`, `McolGetResult`, `McolInquire`, `McolMatch`, `McolPreprocess`, `McolProject`, `McolSetMethod`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraArcFill`, `MgraFont`, `MgraRect`, `MgraRectFill`, `MgraText`, `MimConvert`, `MmodAllocResult`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodPreprocess`, `MmodRestore`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Food and Beverage, Electronics, Packaging, Applications, Inspecting/Proofing/Verifying, Identifying, Finding/Locating, Modules, Buffer, Display, Graphics, Image Processing, Geometric Model Finder, Color Analysis, What's New, AIL 10.0 SP4, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MCol.cs
//
// Description: This program contains 4 examples using the Color module.
//
//              The first example performs color segmentation of an image
//              by classifying each pixel with one out of 6 color samples.
//              The ratio of each color in the image is then calculated.
//         
//              The second example performs color matching of circular regions
//              in objects located with model finder.
//         
//              The third example performs color separation in order to
//              separate 2 types of ink on a piece of paper.
//         
//              The fourth example performs a color to grayscale conversion
//              using the principal component projection functionality.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;
using Zebra.AuroraImagingLibrary;

namespace MCol
{
    class Program
    {
        // Display image margin
        const int DISPLAY_CENTER_MARGIN_X = 5;
        const int DISPLAY_CENTER_MARGIN_Y = 5;

        // Color patch sizes
        const int COLOR_PATCH_SIZEX = 30;
        const int COLOR_PATCH_SIZEY = 40;

        static void Main(string[] args)
        {
            //****************************************************************************
            // Main.
            //****************************************************************************

            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Run the color segmentation example.
            ColorSegmentationExample(AilSystem, AilDisplay);

            // Run the color matching example.
            ColorMatchingExample(AilSystem, AilDisplay);

            // Run the color projection example.
            ColorSeparationExample(AilSystem, AilDisplay);

            // Run the RGB to grayscale conversion example.
            RGBtoGrayscaleExample(AilSystem, AilDisplay);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        //***************************************************************************
        // Color Segmentation using color samples. 
        //***************************************************************************

        // Image filenames
        const string CANDY_SAMPLE_IMAGE_FILE = AIL.M_IMAGE_PATH + "CandySamples.mim";
        const string CANDY_TARGET_IMAGE_FILE = AIL.M_IMAGE_PATH + "Candy.mim";

        // Number of samples
        const int NUM_SAMPLES = 6;

        // Draw spacing and offset
        const int CANDY_SAMPLES_XSPACING = 35;
        const int CANDY_SAMPLES_YOFFSET  = 145;

        // Match parameters
        const int MATCH_MODE                = AIL.M_MIN_DIST_VOTE;  // Minimal distance vote mode.
        const int DISTANCE_TYPE             = AIL.M_MAHALANOBIS;    // Mahalanobis distance.
        const int TOLERANCE_MODE            = AIL.M_SAMPLE_STDDEV;  // Standard deviation tolerance mode.
        const double TOLERANCE_VALUE        = 6.0;                  // Mahalanobis tolerance value.
        const double RED_TOLERANCE_VALUE    = 6.0;
        const double YELLOW_TOLERANCE_VALUE = 12.0;
        const double PINK_TOLERANCE_VALUE   = 5.0;

        static void ColorSegmentationExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID SourceChild     = AIL.M_NULL;           // Source image buffer identifier.
            AIL_ID DestChild       = AIL.M_NULL;           // Dest image buffer identifier.
            AIL_ID MatchContext    = AIL.M_NULL;           // Color matching context identifier.
            AIL_ID MatchResult     = AIL.M_NULL;           // Color matching result identifier.
            AIL_ID DisplayImage    = AIL.M_NULL;           // Display image buffer identifier.

            AIL_INT SourceSizeX    = 0;
            AIL_INT SourceSizeY    = 0;                    // Source image sizes
            AIL_INT SampleIndex    = 0;
            AIL_INT SpacesIndex    = 0;                    // Indices

            double MatchScore      = 0.0;                  // Color matching score.

            // Blank spaces to align the samples names evenly.
            string[] Spaces = { "", " ", "  ", "   " };

            // Color samples names.
            string[] SampleNames = { "Green", "Red", "Yellow", "Purple", "Blue", "Pink" };

            // Color samples position: {OffsetX, OffsetY} 
            double[,] SamplesROI = new double[,] { { 58, 143 }, { 136, 148 }, { 217, 144 }, { 295, 142 }, { 367, 143 }, { 442, 147 } };

            // Color samples size.
            const double SampleSizeX = 36, SampleSizeY = 32;

            // Array for match sample colors.
            AIL_INT[,] SampleMatchColor = new AIL_INT[NUM_SAMPLES, 3];

            Console.Write("\nCOLOR SEGMENTATION:\n");
            Console.Write("-------------------\n");

            // Allocate the parent display image.
            AIL.MbufDiskInquire(CANDY_SAMPLE_IMAGE_FILE, AIL.M_SIZE_X, ref SourceSizeX);
            AIL.MbufDiskInquire(CANDY_SAMPLE_IMAGE_FILE, AIL.M_SIZE_Y, ref SourceSizeY);
            AIL.MbufAllocColor(AilSystem, 3, 2 * SourceSizeX + DISPLAY_CENTER_MARGIN_X, SourceSizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC, ref DisplayImage);
            AIL.MbufClear(DisplayImage, AIL.M_COLOR_BLACK);

            // Create a source and dest child in the display image.
            AIL.MbufChild2d(DisplayImage, 0, 0, SourceSizeX, SourceSizeY, ref SourceChild);
            AIL.MbufChild2d(DisplayImage, SourceSizeX + DISPLAY_CENTER_MARGIN_X, 0, SourceSizeX, SourceSizeY, ref DestChild);

            // Load the source image into the source child.
            AIL.MbufLoad(CANDY_SAMPLE_IMAGE_FILE, SourceChild);

            // Allocate a color matching context.
            AIL.McolAlloc(AilSystem, AIL.M_COLOR_MATCHING, AIL.M_RGB, AIL.M_DEFAULT, AIL.M_DEFAULT, ref MatchContext);

            // Define each color sample in the context.
            for (int i = 0; i < NUM_SAMPLES; i++)
            {
                AIL.McolDefine(MatchContext, SourceChild, AIL.M_SAMPLE_LABEL(i + 1), AIL.M_IMAGE, SamplesROI[i, 0], SamplesROI[i, 1], SampleSizeX, SampleSizeY);
            }

            // Set the color matching parameters.
            AIL.McolSetMethod(MatchContext, MATCH_MODE, DISTANCE_TYPE, AIL.M_DEFAULT, AIL.M_DEFAULT);
            AIL.McolControl(MatchContext, AIL.M_CONTEXT, AIL.M_DISTANCE_TOLERANCE_MODE, TOLERANCE_MODE);
            AIL.McolControl(MatchContext, AIL.M_ALL, AIL.M_DISTANCE_TOLERANCE, TOLERANCE_VALUE);

            // Adjust tolerances for the red, yellow and pink samples.
            AIL.McolControl(MatchContext, AIL.M_SAMPLE_INDEX(1), AIL.M_DISTANCE_TOLERANCE, RED_TOLERANCE_VALUE);
            AIL.McolControl(MatchContext, AIL.M_SAMPLE_INDEX(2), AIL.M_DISTANCE_TOLERANCE, YELLOW_TOLERANCE_VALUE);
            AIL.McolControl(MatchContext, AIL.M_SAMPLE_INDEX(5), AIL.M_DISTANCE_TOLERANCE, PINK_TOLERANCE_VALUE);

            // Preprocess the context.
            AIL.McolPreprocess(MatchContext, AIL.M_DEFAULT);

            // Fill the samples colors array.
            for (int i = 0; i < NUM_SAMPLES; i++)
            {
                AIL.McolInquire(MatchContext, AIL.M_SAMPLE_LABEL(i + 1), AIL.M_MATCH_SAMPLE_COLOR_BAND_0 + AIL.M_TYPE_AIL_INT, ref SampleMatchColor[i, 0]);
                AIL.McolInquire(MatchContext, AIL.M_SAMPLE_LABEL(i + 1), AIL.M_MATCH_SAMPLE_COLOR_BAND_1 + AIL.M_TYPE_AIL_INT, ref SampleMatchColor[i, 1]);
                AIL.McolInquire(MatchContext, AIL.M_SAMPLE_LABEL(i + 1), AIL.M_MATCH_SAMPLE_COLOR_BAND_2 + AIL.M_TYPE_AIL_INT, ref SampleMatchColor[i, 2]);
            }

            // Draw the samples.
            DrawSampleColors(DestChild, SampleMatchColor, SampleNames, NUM_SAMPLES, CANDY_SAMPLES_XSPACING, CANDY_SAMPLES_YOFFSET);

            // Select the image buffer for display.
            AIL.MdispSelect(AilDisplay, DisplayImage);

            // Pause to show the original image.
            Console.Write("Color samples are defined for each possible candy color.\n");
            Console.Write("Press any key to do color matching.\n\n");
            Console.ReadKey(true);

            // Load the target image.*/
            AIL.MbufClear(DisplayImage, AIL.M_COLOR_BLACK);
            AIL.MbufLoad(CANDY_TARGET_IMAGE_FILE, SourceChild);

            // Allocate a color matching result buffer.
            AIL.McolAllocResult(AilSystem, AIL.M_COLOR_MATCHING_RESULT, AIL.M_DEFAULT, ref MatchResult);

            // Enable controls to draw the labeled color image.
            AIL.McolControl(MatchContext, AIL.M_CONTEXT, AIL.M_GENERATE_PIXEL_MATCH, AIL.M_ENABLE);
            AIL.McolControl(MatchContext, AIL.M_CONTEXT, AIL.M_GENERATE_SAMPLE_COLOR_LUT, AIL.M_ENABLE);

            // Match with target image.
            AIL.McolMatch(MatchContext, SourceChild, AIL.M_DEFAULT, AIL.M_NULL, MatchResult, AIL.M_DEFAULT);

            // Retrieve and display results.
            Console.Write("Each pixel of the mixture is matched " +
                      "with one of the color samples.\n");
            Console.Write("\nColor segmentation results:\n");
            Console.Write("---------------------------\n");

            for (SampleIndex = 0; SampleIndex < NUM_SAMPLES; SampleIndex++)
            {
                AIL.McolGetResult(MatchResult, AIL.M_DEFAULT, AIL.M_SAMPLE_INDEX((int)SampleIndex), AIL.M_SCORE, ref MatchScore);
                SpacesIndex = 6 - SampleNames[SampleIndex].Length;
                Console.Write("Ratio of {0}{1} sample = {2,5:0.00}%\n", SampleNames[SampleIndex], Spaces[SpacesIndex], MatchScore);
            }
            Console.Write("\nResults reveal the low proportion of Blue candy.\n");

            // Draw the colored label image in the destination child.
            AIL.McolDraw(AIL.M_DEFAULT, MatchResult, DestChild, AIL.M_DRAW_PIXEL_MATCH_USING_COLOR, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            // Pause to show the result image.
            Console.Write("\nPress any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MbufFree(DestChild);
            AIL.MbufFree(SourceChild);
            AIL.MbufFree(DisplayImage);
            AIL.McolFree(MatchResult);
            AIL.McolFree(MatchContext);
        }

        //****************************************************************************
        // Color matching in labeled regions.
        //****************************************************************************
        // Image filenames
        const string FUSE_SAMPLES_IMAGE = AIL.M_IMAGE_PATH + "FuseSamples.mim";
        const string FUSE_TARGET_IMAGE  = AIL.M_IMAGE_PATH + "Fuse.mim";

        // Model Finder context filename
        const string FINDER_CONTEXT = AIL.M_IMAGE_PATH + "FuseModel.mmf";

        // Number of fuse sample objects
        const int NUM_FUSES = 4;

        // Draw spacing and offset
        const int FUSE_SAMPLES_XSPACING = 40;
        const int FUSE_SAMPLES_YOFFSET  = 145;

        static void ColorMatchingExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID DisplayImage       = AIL.M_NULL;    // Display image buffer identifier.
            AIL_ID SourceChild        = AIL.M_NULL;    // Source image buffer identifier.
            AIL_ID DestChild          = AIL.M_NULL;    // Dest image buffer identifier.
            AIL_ID ColMatchContext    = AIL.M_NULL;    // Color matching context identifier.
            AIL_ID ColMatchResult     = AIL.M_NULL;    // Color matching result identifier.
            AIL_ID ModelImage         = AIL.M_NULL;    // Model image buffer identifier.
            AIL_ID AreaImage          = AIL.M_NULL;    // Area  image buffer identifier.
            AIL_ID OverlayID          = AIL.M_NULL;    // Overlay image buffer identifier.
            AIL_ID OverlaySourceChild = AIL.M_NULL;    // Overlay source child identifier.
            AIL_ID OverlayDestChild   = AIL.M_NULL;    // Overlay dest child identifier.
            AIL_ID FuseFinderCtx      = AIL.M_NULL;    // Model finder context identifier.
            AIL_ID FuseFinderRes      = AIL.M_NULL;    // Model finder result identifier.

            // Image sizes
            AIL_INT SizeX = 0;
            AIL_INT SizeY = 0;

            // Color sample names
            string[] SampleNames    =    { "Green", " Blue", " Red", "Yellow" };

            // Sample ROIs coordinates: OffsetX, OffsetY, SizeX, SizeY
            AIL_INT[,] SampleROIs   = new AIL_INT[,]{{ 54, 139, 28, 14},
                                               {172, 137, 30, 23},
                                               {296, 135, 31, 23},
                                               {417, 134, 27, 22}};

            // Array of match sample colors.
            AIL_INT[,] SampleMatchColor = new AIL_INT[NUM_FUSES, 3];

            Console.Write("\nCOLOR IDENTIFICATION:\n");
            Console.Write("---------------------\n");

            // Allocate the parent display image.
            AIL.MbufDiskInquire(FUSE_TARGET_IMAGE, AIL.M_SIZE_X, ref SizeX);
            AIL.MbufDiskInquire(FUSE_TARGET_IMAGE, AIL.M_SIZE_Y, ref SizeY);
            AIL.MbufAllocColor(AilSystem, 3, 2 * SizeX + DISPLAY_CENTER_MARGIN_X, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC, ref DisplayImage);
            AIL.MbufClear(DisplayImage, AIL.M_COLOR_BLACK);

            // Allocate the model, area and label images.
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref ModelImage);
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AreaImage);

            // Create a source and destination child in the display image.
            AIL.MbufChild2d(DisplayImage, 0, 0, SizeX, SizeY, ref SourceChild);
            AIL.MbufChild2d(DisplayImage, SizeX + DISPLAY_CENTER_MARGIN_X, 0, SizeX, SizeY, ref DestChild);

            // Load the sample source image.
            AIL.MbufLoad(FUSE_SAMPLES_IMAGE, SourceChild);

            // Display the image buffer.
            AIL.MdispSelect(AilDisplay, DisplayImage);

            // Prepare the overlay.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref OverlayID);
            AIL.MbufChild2d(OverlayID, 0, 0, SizeX, SizeY, ref OverlaySourceChild);
            AIL.MbufChild2d(OverlayID, SizeX + DISPLAY_CENTER_MARGIN_X, 0, SizeX, SizeY, ref OverlayDestChild);

            // Prepare the model finder context and result.
            AIL.MmodRestore(FINDER_CONTEXT, AilSystem, AIL.M_DEFAULT, ref FuseFinderCtx);
            AIL.MmodPreprocess(FuseFinderCtx, AIL.M_DEFAULT);
            AIL.MmodAllocResult(AilSystem, AIL.M_DEFAULT, ref FuseFinderRes);

            // Allocate a color match context and result.
            AIL.McolAlloc(AilSystem, AIL.M_COLOR_MATCHING, AIL.M_RGB, AIL.M_DEFAULT, AIL.M_DEFAULT, ref ColMatchContext);
            AIL.McolAllocResult(AilSystem, AIL.M_COLOR_MATCHING_RESULT, AIL.M_DEFAULT, ref ColMatchResult);

            // Define the color samples in the context.
            for (int i = 0; i < NUM_FUSES; i++)
            {
                AIL.McolDefine(ColMatchContext, SourceChild, AIL.M_SAMPLE_LABEL(i + 1), AIL.M_IMAGE, (double)SampleROIs[i, 0], (double)SampleROIs[i, 1], (double)SampleROIs[i, 2], (double)SampleROIs[i, 3]);
            }

            // Preprocess the context.
            AIL.McolPreprocess(ColMatchContext, AIL.M_DEFAULT);

            // Fill the samples colors array.
            for (int i = 0; i < NUM_FUSES; i++)
            {
                AIL.McolInquire(ColMatchContext, AIL.M_SAMPLE_LABEL(i + 1), AIL.M_MATCH_SAMPLE_COLOR_BAND_0 + AIL.M_TYPE_AIL_INT, ref SampleMatchColor[i, 0]);
                AIL.McolInquire(ColMatchContext, AIL.M_SAMPLE_LABEL(i + 1), AIL.M_MATCH_SAMPLE_COLOR_BAND_1 + AIL.M_TYPE_AIL_INT, ref SampleMatchColor[i, 1]);
                AIL.McolInquire(ColMatchContext, AIL.M_SAMPLE_LABEL(i + 1), AIL.M_MATCH_SAMPLE_COLOR_BAND_2 + AIL.M_TYPE_AIL_INT, ref SampleMatchColor[i, 2]);
            }

            // Draw the color samples.
            DrawSampleColors(DestChild, SampleMatchColor, SampleNames, NUM_FUSES, FUSE_SAMPLES_XSPACING, FUSE_SAMPLES_YOFFSET);

            // Draw the sample ROIs in the source image overlay.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);
            for (AIL_INT SampleIndex = 0; SampleIndex < NUM_FUSES; SampleIndex++)
            {
                AIL_INT XEnd = SampleROIs[SampleIndex, 0] + SampleROIs[SampleIndex, 2] - 1;
                AIL_INT YEnd = SampleROIs[SampleIndex, 1] + SampleROIs[SampleIndex, 3] - 1;
                AIL.MgraRect(AIL.M_DEFAULT, OverlaySourceChild, SampleROIs[SampleIndex, 0], SampleROIs[SampleIndex, 1], XEnd, YEnd);
            }

            // Pause to show the source image.
            Console.Write("Colors are defined using one color sample region per fuse.\n");
            Console.Write("Press any key to process the target image.\n");
            Console.ReadKey(true);

            // Clear the overlay.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            // Disable the display update.
            AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_DISABLE);

            // Load the target image into the source child.
            AIL.MbufLoad(FUSE_TARGET_IMAGE, SourceChild);

            // Get the grayscale model image and copy it into the display dest child.
            AIL.MimConvert(SourceChild, ModelImage, AIL.M_RGB_TO_L);
            AIL.MbufCopy(ModelImage, DestChild);

            // Find the Model.
            AIL.MmodFind(FuseFinderCtx, ModelImage, FuseFinderRes);

            // Draw the blob image: labeled circular areas centered at each found fuse occurrence.
            AIL_INT Number = 0;
            AIL.MmodGetResult(FuseFinderRes, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref Number);
            AIL.MbufClear(AreaImage, 0);
            for (AIL_INT ii = 0; ii < Number; ii++)
            {
                double X = 0.0;
                double Y = 0.0;

                // Get the position
                AIL.MmodGetResult(FuseFinderRes, ii, AIL.M_POSITION_X, ref X);
                AIL.MmodGetResult(FuseFinderRes, ii, AIL.M_POSITION_Y, ref Y);

                // Set the label color
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, (double)ii + 1);

                // Draw the filled circle
                AIL.MgraArcFill(AIL.M_DEFAULT, AreaImage, X, Y, 20, 20, 0, 360);
            }

            // Enable controls to draw the labeled color image.
            AIL.McolControl(ColMatchContext, AIL.M_CONTEXT, AIL.M_SAVE_AREA_IMAGE, AIL.M_ENABLE);
            AIL.McolControl(ColMatchContext, AIL.M_CONTEXT, AIL.M_GENERATE_SAMPLE_COLOR_LUT, AIL.M_ENABLE);

            // Perform the color matching.
            AIL.McolMatch(ColMatchContext, SourceChild, AIL.M_DEFAULT, AreaImage, ColMatchResult, AIL.M_DEFAULT);

            // Draw the label image into the overlay child.
            AIL.McolDraw(AIL.M_DEFAULT, ColMatchResult, OverlayDestChild, AIL.M_DRAW_AREA_MATCH_USING_COLOR, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            // Draw the model position over the colored areas.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_BLUE);
            AIL.MmodDraw(AIL.M_DEFAULT, FuseFinderRes, OverlayDestChild, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, AIL.M_ALL, AIL.M_DEFAULT);

            // Enable the display update.
            AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_ENABLE);

            // Pause to show the resulting image.
            Console.Write("\nFuses are located using the Model Finder tool.\n");
            Console.Write("The color of each target area is identified.\n");
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MmodFree(FuseFinderRes);
            AIL.MmodFree(FuseFinderCtx);
            AIL.MbufFree(AreaImage);
            AIL.MbufFree(ModelImage);
            AIL.MbufFree(SourceChild);
            AIL.MbufFree(DestChild);
            AIL.MbufFree(OverlaySourceChild);
            AIL.MbufFree(OverlayDestChild);
            AIL.MbufFree(DisplayImage);
            AIL.McolFree(ColMatchContext);
            AIL.McolFree(ColMatchResult);
        }

        //****************************************************************************
        // Perform color separation of colored inks on a piece of paper.
        //****************************************************************************
        // Source image
        const string WRITING_IMAGE_FILE = AIL.M_IMAGE_PATH + "stamp.mim";

        // Color triplets
        static readonly AIL_INT[] BACKGROUND_COLOR ={ 245, 234, 206 };
        static readonly AIL_INT[] WRITING_COLOR    ={ 141, 174, 174 };
        static readonly AIL_INT[] STAMP_COLOR      = { 226, 150, 118 };

        // Drawing spacing
        const int PATCHES_XSPACING = 70;

        static void ColorSeparationExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID DisplayImage     = AIL.M_NULL;    // Display image buffer identifier.
            AIL_ID SourceChild      = AIL.M_NULL;    // Source image buffer identifier.
            AIL_ID DestChild        = AIL.M_NULL;    // Destination image buffer identifier.
            AIL_ID Child            = AIL.M_NULL;    // Child buffer identifier.
            AIL_ID ColorsArray      = AIL.M_NULL;    // Array buffer identifier.

            // Source image sizes.
            AIL_INT SourceSizeX = 0;
            AIL_INT SourceSizeY = 0;

            // Color samples' names
            string[] ColorNames = { "BACKGROUND", "WRITING", "STAMP" };

            // Array with color patches to draw.
            AIL_INT[,] Colors = new AIL_INT[,] { { 245, 234, 206 }, { 141, 174, 174 }, { 226, 150, 118 } };

            // Samples' color coordinates
            byte[] BackgroundColor = new byte[3] { 245, 234, 206 };
            byte[] SelectedColor   = new byte[3] { 141, 174, 174 };
            byte[] RejectedColor   = new byte[3] { 226, 150, 118 };

            Console.Write("\nCOLOR SEPARATION:\n");
            Console.Write("-----------------\n");

            // Allocate an array buffer and fill it with the color coordinates.
            AIL.MbufAlloc2d(AilSystem, 3, 3, 8 + AIL.M_UNSIGNED, AIL.M_ARRAY, ref ColorsArray);
            AIL.MbufPut2d(ColorsArray, 0, 0, 3, 1, BackgroundColor);
            AIL.MbufPut2d(ColorsArray, 0, 1, 3, 1, SelectedColor);
            AIL.MbufPut2d(ColorsArray, 0, 2, 3, 1, RejectedColor);

            // Allocate the parent display image.
            AIL.MbufDiskInquire(WRITING_IMAGE_FILE, AIL.M_SIZE_X, ref SourceSizeX);
            AIL.MbufDiskInquire(WRITING_IMAGE_FILE, AIL.M_SIZE_Y, ref SourceSizeY);
            AIL.MbufAllocColor(AilSystem, 3, 2 * SourceSizeX + DISPLAY_CENTER_MARGIN_X, SourceSizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC, ref DisplayImage);
            AIL.MbufClear(DisplayImage, AIL.M_COLOR_BLACK);

            // Clear the overlay.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            // Create a source and dest child in the display image
            AIL.MbufChild2d(DisplayImage, 0, 0, SourceSizeX, SourceSizeY, ref SourceChild);
            AIL.MbufChild2d(DisplayImage, SourceSizeX + DISPLAY_CENTER_MARGIN_X, 0, SourceSizeX, SourceSizeY, ref DestChild);

            // Load the source image into the display image source child.
            AIL.MbufLoad(WRITING_IMAGE_FILE, SourceChild);

            // Draw the color patches.
            DrawSampleColors(DestChild, Colors, ColorNames, 3, PATCHES_XSPACING, -1);

            // Display the image.
            AIL.MdispSelect(AilDisplay, DisplayImage);

            // Pause to show the source image and color patches.
            Console.Write("The writing will be separated from the stamp using the following triplets:\n");
            Console.Write("the background color: beige [{0}, {1}, {2}],\n", BackgroundColor[0], BackgroundColor[1], BackgroundColor[2]);
            Console.Write("the writing color   : green [{0}, {1}, {2}],\n", SelectedColor[0], SelectedColor[1], SelectedColor[2]);
            Console.Write("the stamp color     : red   [{0}, {1}, {2}].\n\n", RejectedColor[0], RejectedColor[1], RejectedColor[2]);
            Console.Write("Press any key to extract the writing.\n\n");
            Console.ReadKey(true);

            // Perform the color projection.
            AIL.McolProject(SourceChild, ColorsArray, DestChild, AIL.M_NULL, AIL.M_COLOR_SEPARATION, AIL.M_DEFAULT, AIL.M_NULL);

            // Wait for a key.
            Console.Write("Press any key to extract the stamp.\n\n");
            Console.ReadKey(true);

            // Switch the order of the selected vs rejected colors in the color array.
            AIL.MbufPut2d(ColorsArray, 0, 2, 3, 1, SelectedColor);
            AIL.MbufPut2d(ColorsArray, 0, 1, 3, 1, RejectedColor);

            // Perform the color projection.
            AIL.McolProject(SourceChild, ColorsArray, DestChild, AIL.M_NULL, AIL.M_COLOR_SEPARATION, AIL.M_DEFAULT, AIL.M_NULL);

            // Wait for a key.
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MbufFree(ColorsArray);
            AIL.MbufFree(SourceChild);
            AIL.MbufFree(DestChild);
            AIL.MbufFree(DisplayImage);
        }

        //*****************************************************************************
        // Perform RGB to grayscale conversion
        //*****************************************************************************/
        // Source image
        const string RGB_IMAGE_FILE = AIL.M_IMAGE_PATH + "BinderClip.mim";

        static void RGBtoGrayscaleExample(AIL_ID AilSystem, AIL_ID AilDisplay)
            {
            Console.Write("\nCONVERSION FROM RGB TO GRAYSCALE:\n");
            Console.Write("---------------------------------\n");
            Console.Write("The example compares principal component projection to ");
            Console.Write("luminance based\nconversion.\n\n");

            AIL_ID DisplayImage     = AIL.M_NULL,     // Display image buffer identifier.
                   SourceChild      = AIL.M_NULL,     // Source image child buffer identifier.
                   LuminanceChild   = AIL.M_NULL,     // Luminance image child buffer identifier.
                   PCPchild         = AIL.M_NULL,     // PCP image child buffer identifier.      
                   LuminanceResult  = AIL.M_NULL,     // Luminance result identifier.
                   PCPresult        = AIL.M_NULL,     // PCP result identifier.            
                   Mask             = AIL.M_NULL,     // Mask identifier to perform PCP. 
                   ChildMask        = AIL.M_NULL;     // Child mask identifier to perform PCP. 

            // Inquire size and type of the source image.
            AIL_INT SourceSizeX = AIL.MbufDiskInquire(RGB_IMAGE_FILE, AIL.M_SIZE_X, AIL.M_NULL);
            AIL_INT SourceSizeY = AIL.MbufDiskInquire(RGB_IMAGE_FILE, AIL.M_SIZE_Y, AIL.M_NULL);
            AIL_INT Type        = AIL.MbufDiskInquire(RGB_IMAGE_FILE, AIL.M_TYPE, AIL.M_NULL);

            // Allocate buffer to display the input image and the results.
            AIL.MbufAllocColor(AilSystem, 3, SourceSizeX,
                               3 * SourceSizeY + 2 * DISPLAY_CENTER_MARGIN_Y, Type,
                               AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref DisplayImage);
            AIL.MbufClear(DisplayImage, 0);

            // Create a source child in the display image.
            AIL.MbufChildColor2d(DisplayImage, AIL.M_ALL_BANDS, 0, 0, SourceSizeX, SourceSizeY,
                                 ref SourceChild);

            // Create a destination child in the display image for luminance based conversion.
            AIL.MbufChildColor2d(DisplayImage, AIL.M_ALL_BANDS, 0,
                                 SourceSizeY + DISPLAY_CENTER_MARGIN_Y, SourceSizeX,
                                 SourceSizeY, ref LuminanceChild);

            // Create a destination child in the display image for principal component projection.
            AIL.MbufChildColor2d(DisplayImage, AIL.M_ALL_BANDS, 0,
                                 2 * SourceSizeY + 2 * DISPLAY_CENTER_MARGIN_Y, SourceSizeX,
                                 SourceSizeY, ref PCPchild);

            // Load the source image into the display image source child.
            AIL.MbufLoad(RGB_IMAGE_FILE, SourceChild);
            AIL.MdispSelect(AilDisplay, DisplayImage);

            // Extract luminance channel from source image and copy the result in the display image.
            AIL.MbufAlloc2d(AilSystem, SourceSizeX, SourceSizeY, Type, AIL.M_IMAGE + AIL.M_PROC, ref LuminanceResult);
            AIL.MimConvert(SourceChild, LuminanceResult, AIL.M_RGB_TO_L);
            AIL.MbufCopy(LuminanceResult, LuminanceChild);

            Console.Write("Color image converted to grayscale using luminance based ");
            Console.Write("conversion\ntechnique.\n\n");
            Console.Write("Press any key to convert the image using principal ");
            Console.Write("component projection\ntechnique. \n\n");
            Console.ReadKey(true);

            // Create a mask to identify important colors in the source image.
            AIL.MbufAlloc2d(AIL.M_DEFAULT_HOST, SourceSizeX, SourceSizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC,
                            ref Mask);
            AIL.MbufClear(Mask, 0);

            // Define regions of interest (pixel colors with which to perform the color projection).
            AIL_INT OffsetX   = 105,
                    OffsetY   = 45,
                    MaskSizeX = 60,
                    MaskSizeY = 20;
            AIL.MbufChild2d(Mask, OffsetX, OffsetY, MaskSizeX, MaskSizeY, ref ChildMask);
            AIL.MbufClear(ChildMask, AIL.M_SOURCE_LABEL);
            AIL.MbufFree(ChildMask);

            OffsetX = 220;
            AIL.MbufChild2d(Mask, OffsetX, OffsetY, MaskSizeX, MaskSizeY, ref ChildMask);
            AIL.MbufClear(ChildMask, AIL.M_SOURCE_LABEL);
            AIL.MbufFree(ChildMask);

            OffsetX = 335;
            AIL.MbufChild2d(Mask, OffsetX, OffsetY, MaskSizeX, MaskSizeY, ref ChildMask);
            AIL.MbufClear(ChildMask, AIL.M_SOURCE_LABEL);

            // Perform principal component projection and copy the result in the display image.
            AIL.MbufAlloc2d(AilSystem, SourceSizeX, SourceSizeY, Type, AIL.M_IMAGE + AIL.M_PROC, ref PCPresult);
            AIL.McolProject(SourceChild, Mask, PCPresult, AIL.M_NULL,
                            AIL.M_PRINCIPAL_COMPONENT_PROJECTION, AIL.M_DEFAULT, AIL.M_NULL);
            AIL.MbufCopy(PCPresult, PCPchild);

            Console.Write("Color image converted to grayscale using principal ");
            Console.Write("component projection\ntechnique.\n");
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MbufFree(ChildMask);
            AIL.MbufFree(Mask);
            AIL.MbufFree(PCPresult);
            AIL.MbufFree(LuminanceResult);
            AIL.MbufFree(PCPchild);
            AIL.MbufFree(LuminanceChild);
            AIL.MbufFree(SourceChild);
            AIL.MbufFree(DisplayImage);
            }

        //****************************************************************************
        // Draw the samples as color patches.
        static void DrawSampleColors(AIL_ID DestImage, AIL_INT[,] pSamplesColors, string[] pSampleNames, AIL_INT NumSamples, AIL_INT XSpacing, AIL_INT YOffset)
        {
            AIL_INT DestSizeX = AIL.MbufInquire(DestImage, AIL.M_SIZE_X, AIL.M_NULL);
            AIL_INT DestSizeY = AIL.MbufInquire(DestImage, AIL.M_SIZE_Y, AIL.M_NULL);
            double OffsetX = (DestSizeX - (NumSamples * COLOR_PATCH_SIZEX) - ((NumSamples - 1) * XSpacing)) / 2.0;
            double OffsetY = YOffset > 0 ? YOffset : (DestSizeY - COLOR_PATCH_SIZEY) / 2.0;
            double TextOffsetX;
            AIL.MgraFont(AIL.M_DEFAULT, AIL.M_FONT_DEFAULT_SMALL);

            for (AIL_INT SampleIndex = 0; SampleIndex < NumSamples; SampleIndex++)
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_RGB888((int)pSamplesColors[SampleIndex, 0], (int)pSamplesColors[SampleIndex, 1], (int)pSamplesColors[SampleIndex, 2]));
                AIL.MgraRectFill(AIL.M_DEFAULT, DestImage, OffsetX, OffsetY, OffsetX + COLOR_PATCH_SIZEX, OffsetY + COLOR_PATCH_SIZEY);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_YELLOW);
                TextOffsetX = OffsetX + COLOR_PATCH_SIZEX / 2.0 - 4.0 * pSampleNames[SampleIndex].Length + 0.5;
                AIL.MgraText(AIL.M_DEFAULT, DestImage, TextOffsetX, OffsetY - 20, pSampleNames[SampleIndex]);
                OffsetX += (COLOR_PATCH_SIZEX + XSpacing);
            }
        }

    }
}

```
