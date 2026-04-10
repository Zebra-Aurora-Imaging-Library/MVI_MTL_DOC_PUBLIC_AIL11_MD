---
title: "Mpat"
description: "This example demonstrates the usage of the Pattern Matching module."
ms.language: "C#"
---

# Mpat

> This example demonstrates the usage of the Pattern Matching module.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MappGetError`, `MappTimer`, `MbufAlloc2d`, `MbufChild2d`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraControl`, `MgraFree`, `MimRotate`, `MimTranslate`, `MpatAlloc`, `MpatAllocResult`, `MpatControl`, `MpatDefine`, `MpatDraw`, `MpatFind`, `MpatFree`, `MpatGetResult`, `MpatInquire`, `MpatPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Semiconductor, Applications, Finding/Locating, Modules, Buffer, Display, Graphics, Image Processing, Pattern Matching, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MPat.cs
//
// Description: This program contains 4 examples of the pattern matching module:
//
//              The first example defines a model and then searches for it in a shifted
//              version of the image (without rotation).
//           
//              The second example defines a regular model and then searches for it in a
//              rotated version of the image using search angle range.
//           
//              The third example defines a rotated model at certain angle and then
//              searches for it in a rotated version of the image.
//           
//              The fourth example automatically allocates a model in a wafer image and 
//              finds its horizontal and vertical displacement.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MPat
{
    class Program
    {
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.

            Console.Write("\nGRAYSCALE PATTERN MATCHING:\n");
            Console.Write("---------------------------\n\n");

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Run the search at 0 degree example.
            SearchModelExample(AilSystem, AilDisplay);

            // Run the search over 360 degrees example.
            SearchModelAngleRangeExample(AilSystem, AilDisplay);

            // Run the search rotated model example.
            SearchModelAtAngleExample(AilSystem, AilDisplay);

            // Run the automatic model allocation example.
            AutoAllocationModelExample(AilSystem, AilDisplay);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        //****************************************************************************
        // Find model in shifted version of the image example.

        // Source image file name.
        private const string FIND_IMAGE_FILE = AIL.M_IMAGE_PATH + "CircuitsBoard.mim";

        // Model position and size.
        private const int FIND_MODEL_X_POS = 153;
        private const int FIND_MODEL_Y_POS = 132;
        private const int FIND_MODEL_WIDTH = 128;
        private const int FIND_MODEL_HEIGHT = 128;
        private const double FIND_MODEL_X_CENTER = (FIND_MODEL_X_POS + (FIND_MODEL_WIDTH - 1) / 2.0);
        private const double FIND_MODEL_Y_CENTER = (FIND_MODEL_Y_POS + (FIND_MODEL_HEIGHT - 1) / 2.0);

        // Target image shifting values.
        private const double FIND_SHIFT_X = 4.5;
        private const double FIND_SHIFT_Y = 7.5;

        // Minimum match score to determine acceptability of model (default).
        private const double FIND_MODEL_MIN_MATCH_SCORE = 70.0;

        // Minimum accuracy for the search.
        private const double FIND_MODEL_MIN_ACCURACY = 0.1;

        static void SearchModelExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;               // Image buffer identifier.
            AIL_ID GraphicList = AIL.M_NULL;            // Graphic list identifier.
            AIL_ID ContextId = AIL.M_NULL;              // ContextId identifier.
            AIL_ID Result = AIL.M_NULL;                 // Result identifier.
            AIL_INT NumResults = 0;                     // Number of results found.
            double XOrg = 0.0;                          // Original model position.
            double YOrg = 0.0;
            double x = 0.0;                             // Model position.
            double y = 0.0;
            double ErrX = 0.0;                          // Model error position.
            double ErrY = 0.0;
            double Score = 0.0;                         // Model correlation score.
            double Time = 0.0;                          // Model search time.
            double AnnotationColor = AIL.M_COLOR_GREEN; // Drawing color.

            // Restore source image into an automatically allocated image buffer.
            AIL.MbufRestore(FIND_IMAGE_FILE, AilSystem, ref AilImage);

            // Display the image buffer.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref GraphicList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

            // Allocate a normalized pattern matching context.
            AIL.MpatAlloc(AilSystem, AIL.M_NORMALIZED, AIL.M_DEFAULT, ref ContextId);

            // Define a regular model.
            AIL.MpatDefine(ContextId, AIL.M_REGULAR_MODEL, AilImage, FIND_MODEL_X_POS,
                       FIND_MODEL_Y_POS, FIND_MODEL_WIDTH, FIND_MODEL_HEIGHT, AIL.M_DEFAULT);

            // Set the search accuracy to high.
            AIL.MpatControl(ContextId, AIL.M_DEFAULT, AIL.M_ACCURACY, AIL.M_HIGH);

            // Set the search model speed to high.
            AIL.MpatControl(ContextId, AIL.M_DEFAULT, AIL.M_SPEED, AIL.M_HIGH);

            // Preprocess the model.
            AIL.MpatPreprocess(ContextId, AIL.M_DEFAULT, AilImage);

            // Draw a box around the model in the model image.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor);
            AIL.MpatDraw(AIL.M_DEFAULT, ContextId, GraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION,
                                                                           AIL.M_DEFAULT, AIL.M_ORIGINAL);

            // Pause to show the original image and model position.
            Console.Write("\nA {0}x{1} model was defined in the source image.\n", FIND_MODEL_WIDTH, FIND_MODEL_HEIGHT);
            Console.Write("It will be found in an image shifted by {0:0.00} in X and {1:0.00} in Y.\n", FIND_SHIFT_X, FIND_SHIFT_Y);
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Clear annotations.
            AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

            // Translate the image on a subpixel level.
            AIL.MimTranslate(AilImage, AilImage, FIND_SHIFT_X, FIND_SHIFT_Y, AIL.M_DEFAULT);

            // Allocate result buffer.
            AIL.MpatAllocResult(AilSystem, AIL.M_DEFAULT, ref Result);

            // Dummy first call for bench measure purpose only (bench stabilization, cache effect, etc...). This first call is NOT required by the application.
            AIL.MpatFind(ContextId, AilImage, Result);
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            // Find the model in the target buffer.
            AIL.MpatFind(ContextId, AilImage, Result);

            // Read the time spent in MpatFindModel.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref Time);

            // If one model was found above the acceptance threshold.
            AIL.MpatGetResult(Result, AIL.M_GENERAL, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NumResults);
            if (NumResults == 1)
            {
                // Read results and draw a box around the model occurrence.
                AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_POSITION_X, ref x);
                AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_POSITION_Y, ref y);
                AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_SCORE, ref Score);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor);
                AIL.MpatDraw(AIL.M_DEFAULT, Result, GraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Calculate the position errors in X and Y and inquire original model position.
                ErrX = Math.Abs((FIND_MODEL_X_CENTER + FIND_SHIFT_X) - x);
                ErrY = Math.Abs((FIND_MODEL_Y_CENTER + FIND_SHIFT_Y) - y);
                AIL.MpatInquire(ContextId, AIL.M_DEFAULT, AIL.M_ORIGINAL_X, ref XOrg);
                AIL.MpatInquire(ContextId, AIL.M_DEFAULT, AIL.M_ORIGINAL_Y, ref YOrg);

                // Print out the search result of the model in the original image.
                Console.Write("Search results:\n");
                Console.Write("---------------------------------------------------\n");
                Console.Write("The model is found to be shifted by \tX:{0:0.00}, Y:{1:0.00}.\n", x - XOrg, y - YOrg);
                Console.Write("The model position error is \t\tX:{0:0.00}, Y:{1:0.00}\n", ErrX, ErrY);
                Console.Write("The model match score is \t\t{0:0.0}\n", Score);
                Console.Write("The search time is \t\t\t{0:0.000} ms\n\n", Time * 1000.0);

                // Verify the results.
                if ((Math.Abs((x - XOrg) - FIND_SHIFT_X) > FIND_MODEL_MIN_ACCURACY) ||
                    (Math.Abs((y - YOrg) - FIND_SHIFT_Y) > FIND_MODEL_MIN_ACCURACY) ||
                    (Score < FIND_MODEL_MIN_MATCH_SCORE))
                {
                    Console.Write("Results verification error !\n");
                }
            }
            else
            {
                Console.Write("Model not found !\n");
            }

            // Wait for a key to be pressed.
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Clear annotations.
            AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

            // Free all allocations.
            AIL.MgraFree(GraphicList);
            AIL.MpatFree(Result);
            AIL.MpatFree(ContextId);
            AIL.MbufFree(AilImage);
        }

        //****************************************************************************
        // Find rotated model example.

        // Source image file name. 
        private const string ROTATED_FIND_IMAGE_FILE = AIL.M_IMAGE_PATH + "CircuitsBoard.mim";

        // Image rotation values.
        private const int ROTATED_FIND_ROTATION_DELTA_ANGLE = 10;
        private const int ROTATED_FIND_ROTATION_ANGLE_STEP = 1;
        private const double ROTATED_FIND_RAD_PER_DEG = 0.01745329251;

        // Model position and size.
        private const int ROTATED_FIND_MODEL_X_POS = 153;
        private const int ROTATED_FIND_MODEL_Y_POS = 132;
        private const int ROTATED_FIND_MODEL_WIDTH = 128;
        private const int ROTATED_FIND_MODEL_HEIGHT = 128;

        private const double ROTATED_FIND_MODEL_X_CENTER = ROTATED_FIND_MODEL_X_POS + (ROTATED_FIND_MODEL_WIDTH - 1) / 2.0;
        private const double ROTATED_FIND_MODEL_Y_CENTER = ROTATED_FIND_MODEL_Y_POS + (ROTATED_FIND_MODEL_HEIGHT - 1) / 2.0;

        // Minimum accuracy for the search position.
        private const double ROTATED_FIND_MIN_POSITION_ACCURACY = 0.10;

        // Minimum accuracy for the search angle.
        private const double ROTATED_FIND_MIN_ANGLE_ACCURACY = 0.25;

        // Angle range to search.
        private const int ROTATED_FIND_ANGLE_DELTA_POS = ROTATED_FIND_ROTATION_DELTA_ANGLE;
        private const int ROTATED_FIND_ANGLE_DELTA_NEG = ROTATED_FIND_ROTATION_DELTA_ANGLE;

        static void SearchModelAngleRangeExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilSourceImage = AIL.M_NULL;         // Model  image buffer identifier.
            AIL_ID AilTargetImage = AIL.M_NULL;         // Target image buffer identifier.
            AIL_ID AilDisplayImage = AIL.M_NULL;        // Target image buffer identifier.
            AIL_ID GraphicList = AIL.M_NULL;            // Graphic list.
            AIL_ID AilContextId = AIL.M_NULL;           // Model identifier.
            AIL_ID AilResult = AIL.M_NULL;              // Result identifier.
            double RealX = 0.0;                         // Model real position in x.
            double RealY = 0.0;                         // Model real position in y.
            double RealAngle = 0.0;                     // Model real angle.
            double X = 0.0;                             // Model position in x found.
            double Y = 0.0;                             // Model position in y found.
            double Angle = 0.0;                         // Model angle found.
            double Score = 0.0;                         // Model correlation score.
            double Time = 0.0;                          // Model search time.
            double ErrX = 0.0;                          // Model error position in x.
            double ErrY = 0.0;                          // Model error position in y.
            double ErrAngle = 0.0;                      // Model error angle.
            double SumErrX = 0.0;                       // Model total error position in x.
            double SumErrY = 0.0;                       // Model total error position in y.
            double SumErrAngle = 0.0;                   // Model total error angle.
            double SumTime = 0.0;                       // Model total search time.
            AIL_INT NumResults = 0;                     // Number of results found.
            AIL_INT NbFound = 0;                        // Number of models found.
            double AnnotationColor = AIL.M_COLOR_GREEN; // Drawing color.

            // Load target image into image buffers and display it.
            AIL.MbufRestore(ROTATED_FIND_IMAGE_FILE, AilSystem, ref AilSourceImage);
            AIL.MbufRestore(ROTATED_FIND_IMAGE_FILE, AilSystem, ref AilTargetImage);
            AIL.MbufRestore(ROTATED_FIND_IMAGE_FILE, AilSystem, ref AilDisplayImage);
            AIL.MdispSelect(AilDisplay, AilDisplayImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref GraphicList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

            // Allocate a normalized pattern matching context.
            AIL.MpatAlloc(AilSystem, AIL.M_NORMALIZED, AIL.M_DEFAULT, ref AilContextId);

            // Define a regular model.
            AIL.MpatDefine(AilContextId, AIL.M_REGULAR_MODEL + AIL.M_CIRCULAR_OVERSCAN, AilSourceImage,
                       ROTATED_FIND_MODEL_X_POS, ROTATED_FIND_MODEL_Y_POS,
                       ROTATED_FIND_MODEL_WIDTH, ROTATED_FIND_MODEL_HEIGHT, AIL.M_DEFAULT);

            // Set the search model speed.
            AIL.MpatControl(AilContextId, AIL.M_DEFAULT, AIL.M_SPEED, AIL.M_MEDIUM);

            // Set the position search accuracy.
            AIL.MpatControl(AilContextId, AIL.M_DEFAULT, AIL.M_ACCURACY, AIL.M_HIGH);

            // Activate the search model angle mode.
            AIL.MpatControl(AilContextId, AIL.M_DEFAULT, AIL.M_SEARCH_ANGLE_MODE, AIL.M_ENABLE);

            // Set the search model range angle.
            AIL.MpatControl(AilContextId, AIL.M_DEFAULT, AIL.M_SEARCH_ANGLE_DELTA_NEG, ROTATED_FIND_ANGLE_DELTA_NEG);
            AIL.MpatControl(AilContextId, AIL.M_DEFAULT, AIL.M_SEARCH_ANGLE_DELTA_POS, ROTATED_FIND_ANGLE_DELTA_POS);

            // Set the search model angle accuracy.
            AIL.MpatControl(AilContextId, AIL.M_DEFAULT, AIL.M_SEARCH_ANGLE_ACCURACY, ROTATED_FIND_MIN_ANGLE_ACCURACY);

            // Set the search model angle interpolation mode to bilinear.
            AIL.MpatControl(AilContextId, AIL.M_DEFAULT, AIL.M_SEARCH_ANGLE_INTERPOLATION_MODE, AIL.M_BILINEAR);

            // Preprocess the model.
            AIL.MpatPreprocess(AilContextId, AIL.M_DEFAULT, AilSourceImage);

            // Allocate a result buffer.
            AIL.MpatAllocResult(AilSystem, AIL.M_DEFAULT, ref AilResult);

            // Draw the original model position.
            AIL.MpatDraw(AIL.M_DEFAULT, AilContextId, GraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION,
                                                                   AIL.M_DEFAULT, AIL.M_ORIGINAL);

            // Pause to show the original image and model position.
            Console.Write("\nA {0}x{1} model was defined in the source image.\n", ROTATED_FIND_MODEL_WIDTH, ROTATED_FIND_MODEL_HEIGHT);
            Console.Write("It will be searched in images rotated from {0} degree to {1} degree.\n", -ROTATED_FIND_ROTATION_DELTA_ANGLE, ROTATED_FIND_ROTATION_DELTA_ANGLE);
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Dummy first call for bench measure purpose only (bench stabilization, 
            // cache effect, etc...). This first call is NOT required by the application.
            AIL.MpatFind(AilContextId, AilSourceImage, AilResult);

            // If the model was found above the acceptance threshold.
            AIL.MpatGetResult(AilResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NumResults);
            if (NumResults == 1)
            {
                // Search for the model in images at different angles.
                RealAngle = ROTATED_FIND_ROTATION_DELTA_ANGLE;
                while (RealAngle >= -ROTATED_FIND_ROTATION_DELTA_ANGLE)
                {
                    // Rotate the image from the model image to target image.
                    AIL.MimRotate(AilSourceImage, AilTargetImage, RealAngle, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_BILINEAR + AIL.M_OVERSCAN_CLEAR);

                    // Reset the timer.
                    AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

                    // Find the model in the target image.
                    AIL.MpatFind(AilContextId, AilTargetImage, AilResult);

                    // Read the time spent in MpatFindModel().
                    AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref Time);

                    // Clear the annotations.
                    AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

                    // If one model was found above the acceptance threshold.
                    AIL.MpatGetResult(AilResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NumResults);
                    if (NumResults == 1)
                    {
                        // Read results and draw a box around model occurrence.
                        AIL.MpatGetResult(AilResult, AIL.M_DEFAULT, AIL.M_POSITION_X, ref X);
                        AIL.MpatGetResult(AilResult, AIL.M_DEFAULT, AIL.M_POSITION_Y, ref Y);
                        AIL.MpatGetResult(AilResult, AIL.M_DEFAULT, AIL.M_ANGLE, ref Angle);
                        AIL.MpatGetResult(AilResult, AIL.M_DEFAULT, AIL.M_SCORE, ref Score);

                        AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor);
                        AIL.MpatDraw(AIL.M_DEFAULT, AilResult, GraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_DEFAULT);

                        AIL.MbufCopy(AilTargetImage, AilDisplayImage);

                        // Calculate the angle error and the position errors for statistics.
                        ErrAngle = CalculateAngleDist(Angle, RealAngle);

                        RotateModelCenter(AilSourceImage, ref RealX, ref RealY, RealAngle);
                        ErrX = Math.Abs(X - RealX);
                        ErrY = Math.Abs(Y - RealY);

                        SumErrAngle += ErrAngle;
                        SumErrX += ErrX;
                        SumErrY += ErrY;
                        SumTime += Time;
                        NbFound++;

                        // Verify the precision for the position and the angle.
                        if ((ErrX > ROTATED_FIND_MIN_POSITION_ACCURACY) || (ErrY > ROTATED_FIND_MIN_POSITION_ACCURACY) || (ErrAngle > ROTATED_FIND_MIN_ANGLE_ACCURACY))
                        {
                            Console.Write("Model accuracy error at angle {0:0.0} !\n\n", RealAngle);
                            Console.Write("Errors are X:{0:0.000}, Y:{1:0.000} and Angle:{2:0.00}\n\n", ErrX, ErrY, ErrAngle);
                            Console.Write("Press any key to continue.\n\n");
                            Console.ReadKey(true);
                        }
                    }
                    else
                    {
                        Console.Write("Model was not found at angle {0:0.0} !\n\n", RealAngle);
                        Console.Write("Press any key to continue.\n\n");
                        Console.ReadKey(true);
                    }

                    RealAngle -= ROTATED_FIND_ROTATION_ANGLE_STEP;
                }

                // Print out the search result statistics
                // of the models found in rotated images.
                Console.Write("\nSearch statistics for the model found in the rotated images.\n");
                Console.Write("------------------------------------------------------------\n");
                Console.Write("The average position error is \t\tX:{0:0.000}, Y:{1:0.000}\n", SumErrX / NbFound, SumErrY / NbFound);
                Console.Write("The average angle error is \t\t{0:0.000}\n", SumErrAngle / NbFound);
                Console.Write("The average search time is \t\t{0:0.000} ms\n\n", SumTime * 1000.0 / NbFound);
            }
            else
            {
                Console.Write("Model was not found!\n\n");
            }

            // Wait for a key to be pressed. 
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MgraFree(GraphicList);
            AIL.MpatFree(AilResult);
            AIL.MpatFree(AilContextId);
            AIL.MbufFree(AilTargetImage);
            AIL.MbufFree(AilSourceImage);
            AIL.MbufFree(AilDisplayImage);
        }


        // Calculate the rotated center of the model to compare the accuracy with
        // the center of the occurrence found during pattern matching.
        static void RotateModelCenter(AIL_ID Buffer, ref double X, ref double Y, double Angle)
        {
            AIL_INT BufSizeX = AIL.MbufInquire(Buffer, AIL.M_SIZE_X, AIL.M_NULL);
            AIL_INT BufSizeY = AIL.MbufInquire(Buffer, AIL.M_SIZE_Y, AIL.M_NULL);

            double RadAngle = Angle * ROTATED_FIND_RAD_PER_DEG;
            double CosAngle = Math.Cos(RadAngle);
            double SinAngle = Math.Sin(RadAngle);

            double OffSetX = (BufSizeX - 1) / 2.0;
            double OffSetY = (BufSizeY - 1) / 2.0;

            X = (ROTATED_FIND_MODEL_X_CENTER - OffSetX) * CosAngle + (ROTATED_FIND_MODEL_Y_CENTER - OffSetY) * SinAngle + OffSetX;
            Y = (ROTATED_FIND_MODEL_Y_CENTER - OffSetY) * CosAngle - (ROTATED_FIND_MODEL_X_CENTER - OffSetX) * SinAngle + OffSetY;
        }


        // Calculate the absolute difference between the real angle 
        // and the angle found.
        static double CalculateAngleDist(double Angle1, double Angle2)
        {
            double dist = Math.Abs(Angle1 - Angle2);

            while (dist >= 360.0)
            {
                dist -= 360.0;
            }

            if (dist > 180.0)
            {
                dist = 360.0 - dist;
            }

            return dist;
        }

        //****************************************************
        // Find the rotated model in a rotated image example. 
        //****************************************************
        static void SearchModelAtAngleExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilSourceImage = AIL.M_NULL;         // Model image buffer identifier.
            AIL_ID AilTargetImage = AIL.M_NULL;         // Target image buffer identifier.
            AIL_ID AilOverlayImage = AIL.M_NULL;        // Overlay image buffer identifier.
            AIL_ID ContextId = AIL.M_NULL;              // Context identifier.
            AIL_ID GraphicList = AIL.M_NULL;            // Graphic list.
            AIL_ID AilResult = AIL.M_NULL;              // Result identifier.
            double Time = 0.0;                          // Model search time.
            AIL_INT NbFound = 0;                        // Number of models found.
            double AnnotationColor = AIL.M_COLOR_GREEN; // Drawing color.

            // Load the source image and display it.
            AIL.MbufRestore(ROTATED_FIND_IMAGE_FILE, AilSystem, ref AilSourceImage);
            AIL.MdispSelect(AilDisplay, AilSourceImage);

            // Allocate the target image.
            AIL.MbufAlloc2d(AilSystem, AIL.MbufInquire(AilSourceImage, AIL.M_SIZE_X, AIL.M_NULL),
                AIL.MbufInquire(AilSourceImage, AIL.M_SIZE_Y, AIL.M_NULL),
                8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilTargetImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref GraphicList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

            // Allocate a normalized grayscale model.
            AIL.MpatAlloc(AilSystem, AIL.M_NORMALIZED, AIL.M_DEFAULT, ref ContextId);

            // Define a regular model.
            AIL.MpatDefine(ContextId, AIL.M_REGULAR_MODEL, AilSourceImage,
                ROTATED_FIND_MODEL_X_POS, ROTATED_FIND_MODEL_Y_POS,
                ROTATED_FIND_MODEL_WIDTH, ROTATED_FIND_MODEL_HEIGHT, AIL.M_DEFAULT);

            // Activate the search model angle mode.
            AIL.MpatControl(ContextId, AIL.M_DEFAULT, AIL.M_SEARCH_ANGLE_MODE, AIL.M_ENABLE);

            // Disable the search model range angle.
            AIL.MpatControl(ContextId, AIL.M_DEFAULT, AIL.M_SEARCH_ANGLE_DELTA_NEG, 0);
            AIL.MpatControl(ContextId, AIL.M_DEFAULT, AIL.M_SEARCH_ANGLE_DELTA_POS, 0);

            // Set a specific angle.
            AIL.MpatControl(ContextId, 0, AIL.M_SEARCH_ANGLE, ROTATED_FIND_ROTATION_DELTA_ANGLE);

            // Preprocess the model.
            AIL.MpatPreprocess(ContextId, AIL.M_DEFAULT, AilSourceImage);

            // Allocate a result buffer.
            AIL.MpatAllocResult(AilSystem, AIL.M_DEFAULT, ref AilResult);

            // Draw the original model position.
            AIL.MpatDraw(AIL.M_DEFAULT, ContextId, GraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION,
                     AIL.M_DEFAULT, AIL.M_ORIGINAL);

            // Pause to show the original image and model position.
            Console.Write("\nA {0}x{1} model was defined in the source image.\n",
                      ROTATED_FIND_MODEL_WIDTH, ROTATED_FIND_MODEL_HEIGHT);
            Console.Write("It will be searched in an image rotated at {0} degrees.\n",
                      -ROTATED_FIND_ROTATION_DELTA_ANGLE);

            Console.Write("Press any key to continue.\n");
            Console.ReadKey(true);

            // Rotate the source image -10 degrees.
            AIL.MimRotate(AilSourceImage, AilTargetImage, ROTATED_FIND_ROTATION_DELTA_ANGLE, AIL.M_DEFAULT,
                        AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_BILINEAR + AIL.M_OVERSCAN_CLEAR);

            AIL.MdispSelect(AilDisplay, AilTargetImage);

            // Dummy first call for bench measure purpose only (bench stabilization, 
            // cache effect, etc...). This first call is NOT required by the application.
            AIL.MpatFind(ContextId, AilTargetImage, AilResult);

            // Reset the timer.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            AIL.MpatFind(ContextId, AilTargetImage, AilResult);

            // Read the time spent in MpatFind().
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref Time);

            // Clear the annotations.
            AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

            AIL.MpatGetResult(AilResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NbFound);
            if (NbFound == 1)
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor);
                AIL.MpatDraw(AIL.M_DEFAULT, AilResult, GraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_DEFAULT);
                Console.Write("A search model at a specific angle has been found in the rotated image.\n");
                Console.Write("The search time is {0:F3} ms.\n\n", Time * 1000.0);
            }
            else
            {
                Console.Write("Model was not found!\n\n");
            }

            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Disable the overlay display.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_SHOW, AIL.M_DISABLE);

            // Free all allocations.
            AIL.MpatFree(AilResult);
            AIL.MpatFree(ContextId);
            AIL.MgraFree(GraphicList);
            AIL.MbufFree(AilTargetImage);
            AIL.MbufFree(AilSourceImage);
        }

        //*****************************************************************************
        // Automatic model allocation example.

        // Source and target images file specifications.
        private const string AUTO_MODEL_IMAGE_FILE = AIL.M_IMAGE_PATH + "Wafer.mim";
        private const string AUTO_MODEL_TARGET_IMAGE_FILE = AIL.M_IMAGE_PATH + "WaferShifted.mim";

        // Model width and height
        private const int AUTO_MODEL_WIDTH = 64;
        private const int AUTO_MODEL_HEIGHT = 64;

        static void AutoAllocationModelExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;                       // Image buffer identifier.
            AIL_ID AilSubImage = AIL.M_NULL;                    // Sub-image buffer identifier.
            AIL_ID GraphicList = AIL.M_NULL;                    // Graphic list.
            AIL_ID ContextId = AIL.M_NULL;                      // Model identifier.
            AIL_ID Result = AIL.M_NULL;                         // Result buffer identifier.
            AIL_INT AllocError = 0;                             // Allocation error variable.
            AIL_INT NumResults = 0;                             // Number of results found.
            AIL_INT ImageWidth = 0;                             // Target image dimensions 
            AIL_INT ImageHeight = 0;
            double OrgX = 0.0;                                  // Original center of model.
            double OrgY = 0.0;
            double x = 0.0;                                     // Result variables.
            double y = 0.0;
            double Score = 0.0;
            double AnnotationColor = AIL.M_COLOR_GREEN;         // Drawing color.

            // Load model image into an image buffer.
            AIL.MbufRestore(AUTO_MODEL_IMAGE_FILE, AilSystem, ref AilImage);

            // Display the image.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref GraphicList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

            // Restrict the region to be processed to the bottom right corner of the image.
            AIL.MbufInquire(AilImage, AIL.M_SIZE_X, ref ImageWidth);
            AIL.MbufInquire(AilImage, AIL.M_SIZE_Y, ref ImageHeight);
            AIL.MbufChild2d(AilImage, ImageWidth / 2, ImageHeight / 2, ImageWidth / 2, ImageHeight / 2, ref AilSubImage);

            // Add an offset to the drawings so they are aligned with the processed child image.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_DRAW_OFFSET_X, (double)-(ImageWidth / 2));
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_DRAW_OFFSET_Y, (double)-(ImageHeight / 2));

            // Automatically allocate a normalized grayscale type pattern matching context.
            AIL.MpatAlloc(AilSystem, AIL.M_NORMALIZED, AIL.M_DEFAULT, ref ContextId);

            // Define a unique model
            AIL.MpatDefine(ContextId, AIL.M_AUTO_MODEL, AilSubImage, AIL.M_DEFAULT, AIL.M_DEFAULT,
                       AUTO_MODEL_WIDTH, AUTO_MODEL_HEIGHT, AIL.M_DEFAULT);

            // Set the search accuracy to high.
            AIL.MpatControl(ContextId, AIL.M_DEFAULT, AIL.M_ACCURACY, AIL.M_HIGH);

            // Check for that model allocation was successful.
            AIL.MappGetError(AIL.M_DEFAULT, AIL.M_CURRENT, ref AllocError);
            if (AllocError == 0)
            {
                // Draw a box around the model.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor);
                AIL.MpatDraw(AIL.M_DEFAULT, ContextId, GraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_ORIGINAL);
                Console.Write("A model was automatically defined in the image.\n");
                Console.Write("Press any key to continue.\n\n");
                Console.ReadKey(true);

                // Clear the annotations.
                AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

                // Load target image into an image buffer.
                AIL.MbufLoad(AUTO_MODEL_TARGET_IMAGE_FILE, AilImage);

                // Allocate result.
                AIL.MpatAllocResult(AilSystem, AIL.M_DEFAULT, ref Result);

                // Preprocess the model.
                AIL.MpatPreprocess(ContextId, AIL.M_DEFAULT, AilSubImage);

                // Find model.
                AIL.MpatFind(ContextId, AilSubImage, Result);

                // If one model was found above the acceptance threshold set.
                AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NumResults);
                if (NumResults == 1)
                {
                    // Get results.
                    AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_POSITION_X, ref x);
                    AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_POSITION_Y, ref y);
                    AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_SCORE, ref Score);

                    // Draw a box around the occurrence.
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor);
                    AIL.MpatDraw(AIL.M_DEFAULT, Result, GraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_DEFAULT);

                    // Analyze and print results.
                    AIL.MpatInquire(ContextId, AIL.M_DEFAULT, AIL.M_ORIGINAL_X, ref OrgX);
                    AIL.MpatInquire(ContextId, AIL.M_DEFAULT, AIL.M_ORIGINAL_Y, ref OrgY);
                    Console.Write("An image misaligned by 50 pixels in X and 20 pixels in Y was loaded.\n\n");
                    Console.Write("The image is found to be shifted by {0:0.00} in X, and {1:0.00} in Y.\n", x - OrgX, y - OrgY);
                    Console.Write("Model match score is {0:0.0} percent.\n", Score);
                    Console.Write("Press any key to end.\n\n");
                    Console.ReadKey(true);
                }
                else
                {
                    Console.Write("Error: Pattern not found properly.\n");
                    Console.Write("Press any key to end.\n\n");
                    Console.ReadKey(true);
                }

                // Free result buffer and model.
                AIL.MpatFree(Result);
                AIL.MpatFree(ContextId);
            }
            else
            {
                Console.Write("Error: Automatic model definition failed.\n");
                Console.Write("Press any key to end.\n\n");
                Console.ReadKey(true);
            }

            // Remove the drawing offset.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_DRAW_OFFSET_X, 0.0);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_DRAW_OFFSET_Y, 0.0);

            // Free the graphic list.
            AIL.MgraFree(GraphicList);

            // Free child buffer and defaults.
            AIL.MbufFree(AilSubImage);
            AIL.MbufFree(AilImage);
        }
    }
}

```
