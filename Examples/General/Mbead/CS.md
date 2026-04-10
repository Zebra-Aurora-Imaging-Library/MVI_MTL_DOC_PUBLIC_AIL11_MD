---
title: "Mbead"
description: "This program performs several bead inspections on mechanical parts."
ms.language: "C#"
---

# Mbead

> This program performs several bead inspections on mechanical parts.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbeadAlloc`, `MbeadAllocResult`, `MbeadControl`, `MbeadDraw`, `MbeadFree`, `MbeadGetResult`, `MbeadInquire`, `MbeadTemplate`, `MbeadTrain`, `MbeadVerify`, `MbufFree`, `MbufRestore`, `McalAlloc`, `McalDraw`, `McalFixture`, `McalFree`, `McalUniform`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MmodAlloc`, `MmodAllocResult`, `MmodDefine`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Measuring, Modules, Buffer, Display, Graphics, Calibration, Geometric Model Finder, Bead Inspection, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mbead.cs
//
// Description: This program uses the Bead module to train a bead template
//              and then to inspect a defective bead in a target image.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MBead
{
    class Program
    {
        // Utility definitions.
        private static readonly AIL_INT USER_POSITION_COLOR = AIL.M_COLOR_RED;
        private static readonly AIL_INT USER_TEMPLATE_COLOR = AIL.M_COLOR_CYAN;
        private static readonly AIL_INT TRAINED_BEAD_WIDTH_COLOR = AIL.M_RGB888(255, 128, 0);
        private static readonly AIL_INT MODEL_FINDER_COLOR = AIL.M_COLOR_GREEN;
        private static readonly AIL_INT COORDINATE_SYSTEM_COLOR = AIL.M_RGB888(164, 164, 0);
        private static readonly AIL_INT RESULT_SEARCH_BOX_COLOR = AIL.M_COLOR_CYAN;
        private static readonly AIL_INT PASS_BEAD_WIDTH_COLOR = AIL.M_COLOR_GREEN;
        private static readonly AIL_INT PASS_BEAD_POSITION_COLOR = AIL.M_COLOR_GREEN;
        private static readonly AIL_INT FAIL_NOT_FOUND_COLOR = AIL.M_COLOR_RED;
        private static readonly AIL_INT FAIL_SMALL_WIDTH_COLOR = AIL.M_RGB888(255, 128, 0);
        private static readonly AIL_INT FAIL_EDGE_OFFSET_COLOR = AIL.M_COLOR_RED;

        //*****************************************************************************
        // Main. 
        //*****************************************************************************
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;         // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;              // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;             // Display identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Run fixtured bead example.
            FixturedBeadExample(AilSystem, AilDisplay);

            // Run predefined bead example.
            PredefinedBeadExample(AilSystem, AilDisplay);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        //****************************************************************************
        // Fixtured 'stripe' bead example.

        // Target image specifications.
        private static readonly string IMAGE_FILE_TRAINING = AIL.M_IMAGE_PATH + "BeadTraining.mim";
        private static readonly string IMAGE_FILE_TARGET = AIL.M_IMAGE_PATH + "BeadTarget.mim";

        // Bead stripe training data definition.
        private const int NUMBER_OF_TRAINING_POINTS = 13;

        private static readonly double[] TrainingPointsX = new double[NUMBER_OF_TRAINING_POINTS] { 180, 280, 400, 430, 455, 415, 370, 275, 185, 125, 105, 130, 180 };
        private static readonly double[] TrainingPointsY = new double[NUMBER_OF_TRAINING_POINTS] { 190, 215, 190, 200, 260, 330, 345, 310, 340, 305, 265, 200, 190 };

        // Max angle correction.
        private const double MAX_ANGLE_CORRECTION_VALUE = 20.0;

        // Max offset deviation.
        private const double MAX_DEVIATION_OFFSET_VALUE = 25.0;

        // Maximum negative width variation.
        private const double WIDTH_DELTA_NEG_VALUE = 2.0;

        // Model region  definition.
        private const int MODEL_ORIGIN_X = 145;
        private const int MODEL_ORIGIN_Y = 115;
        private const int MODEL_SIZE_X = 275;
        private const int MODEL_SIZE_Y = 60;

        private static void FixturedBeadExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilGraList = AIL.M_NULL;             // Graphic list identifier.
            AIL_ID AilImageTraining = AIL.M_NULL;       // Image buffer identifier.
            AIL_ID AilImageTarget = AIL.M_NULL;         // Image buffer identifier.
            AIL_ID AilBeadContext = AIL.M_NULL;         // Bead context identifier.
            AIL_ID AilBeadResult = AIL.M_NULL;          // Bead result identifier.
            AIL_ID AilModelFinderContext = AIL.M_NULL;  // Model finder context identifier.
            AIL_ID AilModelFinderResult = AIL.M_NULL;   // Model finder result identifier.
            AIL_ID AilFixturingOffset = AIL.M_NULL;     // Fixturing offset identifier.

            double NominalWidth = 0.0;                  // Nominal width result value.
            double AvWidth = 0.0;                       // Average width result value.
            double GapCov = 0.0;                        // Gap coverage result value.
            double MaxGap = 0.0;                        // Maximum gap result value.
            double Score = 0.0;                         // Bead score result value.

            // Restore source images into an automatically allocated image buffers.
            AIL.MbufRestore(IMAGE_FILE_TRAINING, AilSystem, ref AilImageTraining);
            AIL.MbufRestore(IMAGE_FILE_TARGET, AilSystem, ref AilImageTarget);

            // Display the training image buffer.
            AIL.MdispSelect(AilDisplay, AilImageTraining);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref AilGraList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraList);

            // Original template image.
            Console.WriteLine();
            Console.WriteLine("FIXTURED BEAD INSPECTION:");
            Console.WriteLine("-------------------------");
            Console.WriteLine();
            Console.WriteLine("This program performs a bead inspection on a mechanical part.");
            Console.WriteLine("In the first step, a bead template context is trained using an image.");
            Console.WriteLine("In the second step, a mechanical part, at an arbitrary angle and with");
            Console.WriteLine("a defective bead, is inspected.");
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Allocate a bead context.
            AIL.MbeadAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilBeadContext);

            // Allocate a bead result.
            AIL.MbeadAllocResult(AilSystem, AIL.M_DEFAULT, ref AilBeadResult);

            // Add bead templates
            AIL.MbeadTemplate(AilBeadContext, AIL.M_ADD, AIL.M_DEFAULT, AIL.M_TEMPLATE_LABEL(1),
               NUMBER_OF_TRAINING_POINTS, TrainingPointsX, TrainingPointsY, AIL.M_NULL, AIL.M_DEFAULT);

            // Set template input units to world units
            AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TEMPLATE_INPUT_UNITS, AIL.M_WORLD);

            // Set the bead 'edge type' search properties
            AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_ANGLE_ACCURACY_MAX_DEVIATION, MAX_ANGLE_CORRECTION_VALUE);

            // Set the maximum valid bead deformation.
            AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_OFFSET_MAX, MAX_DEVIATION_OFFSET_VALUE);

            // Set the valid bead stripe minimum width criterion.
            AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_WIDTH_DELTA_NEG, WIDTH_DELTA_NEG_VALUE);

            // Display the bead polyline.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, USER_TEMPLATE_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilGraList, AIL.M_DRAW_POSITION_POLYLINE,
               AIL.M_USER, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            // Display the bead training points.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, USER_POSITION_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilGraList, AIL.M_DRAW_POSITION,
               AIL.M_USER, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            // Pause to show the template image and user points.
            Console.WriteLine("The initial points specified by the user (in red) are");
            Console.WriteLine("used to train the bead information from an image.");
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Set a 1:1 calibration to the training image.
            AIL.McalUniform(AilImageTraining, 0, 0, 1, 1, 0, AIL.M_DEFAULT);

            // Train the bead context.
            AIL.MbeadTrain(AilBeadContext, AilImageTraining, AIL.M_DEFAULT);

            // Display the trained bead.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, TRAINED_BEAD_WIDTH_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilGraList, AIL.M_DRAW_WIDTH,
               AIL.M_TRAINED, AIL.M_TEMPLATE_LABEL(1), AIL.M_ALL, AIL.M_DEFAULT);

            // Retrieve the trained nominal width.
            AIL.MbeadInquire(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TRAINED_WIDTH_NOMINAL, ref NominalWidth);

            Console.WriteLine("The template has been trained and is displayed in orange.");
            Console.WriteLine("Its nominal trained width is {0:#.##} pixels.", NominalWidth);
            Console.WriteLine();

            // Define model to further fixture the bead template.
            AIL.MmodAlloc(AilSystem, AIL.M_GEOMETRIC, AIL.M_DEFAULT, ref AilModelFinderContext);

            AIL.MmodAllocResult(AilSystem, AIL.M_DEFAULT, ref AilModelFinderResult);

            AIL.MmodDefine(AilModelFinderContext, AIL.M_IMAGE, AilImageTraining,
                MODEL_ORIGIN_X, MODEL_ORIGIN_Y, MODEL_SIZE_X, MODEL_SIZE_Y);

            // Preprocess the model.
            AIL.MmodPreprocess(AilModelFinderContext, AIL.M_DEFAULT);

            // Allocate a fixture object.
            AIL.McalAlloc(AilSystem, AIL.M_FIXTURING_OFFSET, AIL.M_DEFAULT, ref AilFixturingOffset);

            // Learn the relative offset of the model.
            AIL.McalFixture(AIL.M_NULL, AilFixturingOffset, AIL.M_LEARN_OFFSET, AIL.M_MODEL_MOD,
               AilModelFinderContext, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Display the model.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, MODEL_FINDER_COLOR);
            AIL.MmodDraw(AIL.M_DEFAULT, AilModelFinderContext, AilGraList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION,
               AIL.M_DEFAULT, AIL.M_ORIGINAL);

            Console.WriteLine("A Model Finder model (in green) is also defined to");
            Console.WriteLine("further fixture the bead verification operation.");
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Clear the overlay annotation.
            AIL.MgraClear(AIL.M_DEFAULT, AilGraList);

            // Display the target image buffer.
            AIL.MdispSelect(AilDisplay, AilImageTarget);

            // Find the location of the fixture using Model Finder.
            AIL.MmodFind(AilModelFinderContext, AilImageTarget, AilModelFinderResult);

            // Display the found model occurrence.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, MODEL_FINDER_COLOR);
            AIL.MmodDraw(AIL.M_DEFAULT, AilModelFinderResult, AilGraList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION,
               AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Apply fixture offset to the target image.
            AIL.McalFixture(AilImageTarget, AilFixturingOffset, AIL.M_MOVE_RELATIVE, AIL.M_RESULT_MOD,
               AilModelFinderResult, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Display the relative coordinate system.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, COORDINATE_SYSTEM_COLOR);
            AIL.McalDraw(AIL.M_DEFAULT, AIL.M_NULL, AilGraList, AIL.M_DRAW_RELATIVE_COORDINATE_SYSTEM,
               AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Perform the inspection of the bead in the fixtured target image.
            AIL.MbeadVerify(AilBeadContext, AilImageTarget, AilBeadResult, AIL.M_DEFAULT);

            // Display the result search boxes.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, RESULT_SEARCH_BOX_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilGraList, AIL.M_DRAW_SEARCH_BOX, AIL.M_ALL,
               AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            Console.WriteLine("The mechanical part's position and angle (in green) were located");
            Console.WriteLine("using Model Finder, and the bead's search boxes (in cyan) were");
            Console.WriteLine("positioned accordingly.");
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Clear the overlay annotation.
            AIL.MgraClear(AIL.M_DEFAULT, AilGraList);

            // Display the moved relative coordinate system.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, COORDINATE_SYSTEM_COLOR);
            AIL.McalDraw(AIL.M_DEFAULT, AIL.M_NULL, AilGraList, AIL.M_DRAW_RELATIVE_COORDINATE_SYSTEM,
               AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Display the pass bead sections.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_BEAD_WIDTH_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilGraList, AIL.M_DRAW_WIDTH, AIL.M_PASS,
               AIL.M_TEMPLATE_LABEL(1), AIL.M_ALL, AIL.M_DEFAULT);

            // Display the missing bead sections.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_NOT_FOUND_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilGraList, AIL.M_DRAW_SEARCH_BOX,
               AIL.M_FAIL_NOT_FOUND, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            // Display bead sections which do not meet the minimum width criteria.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_SMALL_WIDTH_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilGraList, AIL.M_DRAW_SEARCH_BOX,
               AIL.M_FAIL_WIDTH_MIN, AIL.M_TEMPLATE_LABEL(1), AIL.M_ALL, AIL.M_DEFAULT);

            // Retrieve and display general bead results.
            AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_SCORE, ref Score);
            AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_GAP_COVERAGE, ref GapCov);
            AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_WIDTH_AVERAGE, ref AvWidth);
            AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_GAP_MAX_LENGTH, ref MaxGap);

            Console.WriteLine("The bead has been inspected:");
            Console.WriteLine(" -Passing bead sections (green) cover {0:0.00}% of the bead", Score);
            Console.WriteLine(" -Missing bead sections (red) cover {0:0.00}% of the bead", GapCov);
            Console.WriteLine(" -Sections outside the specified tolerances are drawn in orange");
            Console.WriteLine(" -The bead's average width is {0:0.00} pixels", AvWidth);
            Console.WriteLine(" -The bead's longest gap section is {0:0.00} pixels", MaxGap);

            // Pause to show the result.
            Console.WriteLine("Press any key to continue.");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MmodFree(AilModelFinderContext);
            AIL.MmodFree(AilModelFinderResult);
            AIL.MbeadFree(AilBeadContext);
            AIL.MbeadFree(AilBeadResult);
            AIL.McalFree(AilFixturingOffset);
            AIL.MbufFree(AilImageTraining);
            AIL.MbufFree(AilImageTarget);
            AIL.MgraFree(AilGraList);
        }

        //*****************************************************************************
        // Predefined 'edge' bead example.

        // Target image specifications.
        private static readonly string CAP_FILE_TARGET = AIL.M_IMAGE_PATH + "Cap.mim";

        // Template attributes definition.
        private const double CIRCLE_CENTER_X = 330.0;
        private const double CIRCLE_CENTER_Y = 230.0;
        private const double CIRCLE_RADIUS = 120.0;

        // Edge threshold value.
        private const double EDGE_THRESHOLD_VALUE = 25.0;

        // Max offset found and deviation tolerance.
        private const double MAX_CONTOUR_DEVIATION_OFFSET = 5.0;
        private const double MAX_CONTOUR_FOUND_OFFSET = 25.0;

        private static void PredefinedBeadExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilOverlayImage = AIL.M_NULL;       // Overlay buffer identifier.
            AIL_ID AilImageTarget = AIL.M_NULL;        // Image buffer identifier.
            AIL_ID AilBeadContext = AIL.M_NULL;        // Bead context identifier.
            AIL_ID AilBeadResult = AIL.M_NULL;         // Bead result identifier.

            double MaximumOffset = 0.0;     // Maximum offset result value.

            // Restore target image into an automatically allocated image buffers.
            AIL.MbufRestore(CAP_FILE_TARGET, AilSystem, ref AilImageTarget);

            // Display the training image buffer.
            AIL.MdispSelect(AilDisplay, AilImageTarget);

            // Prepare the overlay for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_TRANSPARENT_COLOR);

            // Original template image.
            Console.WriteLine();
            Console.WriteLine("PREDEFINED BEAD INSPECTION:");
            Console.WriteLine("---------------------------");
            Console.WriteLine();
            Console.WriteLine("This program performs a bead inspection of a bottle");
            Console.WriteLine("cap's contour using a predefined circular bead.");
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Allocate a bead context.
            AIL.MbeadAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilBeadContext);

            // Allocate a bead result.
            AIL.MbeadAllocResult(AilSystem, AIL.M_DEFAULT, ref AilBeadResult);

            // Add the bead templates.
            AIL.MbeadTemplate(AilBeadContext, AIL.M_ADD, AIL.M_BEAD_EDGE, AIL.M_TEMPLATE_LABEL(1),
               0, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT);

            // Set the bead shape properties.
            AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TRAINING_PATH, AIL.M_CIRCLE);

            AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TEMPLATE_CIRCLE_CENTER_X, CIRCLE_CENTER_X);
            AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TEMPLATE_CIRCLE_CENTER_Y, CIRCLE_CENTER_Y);
            AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TEMPLATE_CIRCLE_RADIUS, CIRCLE_RADIUS);

            // Set the edge threshold value to extract the object shape.
            AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_THRESHOLD_VALUE, EDGE_THRESHOLD_VALUE);

            // Using the default fixed user defined nominal edge width.
            AIL.MbeadControl(AilBeadContext, AIL.M_ALL, AIL.M_WIDTH_NOMINAL_MODE, AIL.M_USER_DEFINED);

            // Set the maximal expected contour deformation.
            AIL.MbeadControl(AilBeadContext, AIL.M_ALL, AIL.M_FAIL_WARNING_OFFSET, MAX_CONTOUR_FOUND_OFFSET);

            // Set the maximum valid bead deformation.
            AIL.MbeadControl(AilBeadContext, AIL.M_ALL, AIL.M_OFFSET_MAX, MAX_CONTOUR_DEVIATION_OFFSET);

            // Display the bead in the overlay image.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, USER_TEMPLATE_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilOverlayImage, AIL.M_DRAW_POSITION,
               AIL.M_USER, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            // The bead template is entirely defined and is trained without sample image.
            AIL.MbeadTrain(AilBeadContext, AIL.M_NULL, AIL.M_DEFAULT);

            // Display the trained bead.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, TRAINED_BEAD_WIDTH_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilOverlayImage, AIL.M_DRAW_SEARCH_BOX,
               AIL.M_TRAINED, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            // Pause to show the template image and user points.
            Console.WriteLine("A circular template that was parametrically defined by the");
            Console.WriteLine("user is displayed (in cyan). The template has been trained");
            Console.WriteLine("and the resulting search is displayed (in orange).");
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Perform the inspection of the bead in the fixtured target image.
            AIL.MbeadVerify(AilBeadContext, AilImageTarget, AilBeadResult, AIL.M_DEFAULT);

            // Clear the overlay annotation.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_TRANSPARENT_COLOR);

            // Display the pass bead sections.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_BEAD_POSITION_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilOverlayImage, AIL.M_DRAW_POSITION, AIL.M_PASS,
               AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            // Display the offset bead sections.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_EDGE_OFFSET_COLOR);
            AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilOverlayImage, AIL.M_DRAW_POSITION, AIL.M_FAIL_OFFSET,
               AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT);

            // Retrieve and display general bead results.
            AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_OFFSET_MAX, ref MaximumOffset);

            Console.WriteLine("The bottle cap shape has been inspected:");
            Console.WriteLine(" -Sections outside the specified offset tolerance are drawn in red");
            Console.WriteLine(" -The maximum offset value is {0:0.00} pixels.", MaximumOffset);
            Console.WriteLine();

            // Pause to show the result.
            Console.WriteLine("Press any key to terminate.");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MbeadFree(AilBeadContext);
            AIL.MbeadFree(AilBeadResult);
            AIL.MbufFree(AilImageTarget);
        }
    }
}

```
