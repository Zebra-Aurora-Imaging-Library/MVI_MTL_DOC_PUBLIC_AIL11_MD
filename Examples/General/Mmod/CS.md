---
title: "Mmod"
description: "This example demonstrates the usage of the Geometric Model Finder module."
ms.language: "C#"
---

# Mmod

> This example demonstrates the usage of the Geometric Model Finder module.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufFree`, `MbufLoad`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MmodAlloc`, `MmodAllocResult`, `MmodControl`, `MmodDefine`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Finding/Locating, Modules, Buffer, Display, Graphics, Geometric Model Finder, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MMod.cs
//
// Description: This program uses the Geometric Model Finder module to define geometric 
//              models and searches for these models in a target image. A simple single 
//              model example (1 model, 1 occurrence, good search conditions) is  
//              presented first, followed by a more complex example (multiple models, 
//              multiple occurrences in a complex scene with bad search conditions).
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MMod
{
    class Program
    {
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Run single model example.
            SingleModelExample(AilSystem, AilDisplay);

            // Run multiple model example.
            MultipleModelExample(AilSystem, AilDisplay);

            // Free defaults
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        //*****************************************************************************
        // Single model example.
        //
        // Source image file specifications.
        private const string SINGLE_MODEL_IMAGE = AIL.M_IMAGE_PATH + "SingleModel.mim";

        // Target image file specifications.
        private const string SINGLE_MODEL_TARGET_IMAGE = AIL.M_IMAGE_PATH + "SingleTarget.mim";

        //Search speed: M_VERY_HIGH for faster search, M_MEDIUM for precision and robustness.
        private const int SINGLE_MODEL_SEARCH_SPEED = AIL.M_VERY_HIGH;

        // Model specifications.
        private const int MODEL_OFFSETX = 176;
        private const int MODEL_OFFSETY = 136;
        private const int MODEL_SIZEX = 128;
        private const int MODEL_SIZEY = 128;
        private const int MODEL_MAX_OCCURRENCES = 16;

        static void SingleModelExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;                               // Image buffer identifier.
            AIL_ID GraphicList = AIL.M_NULL;                            // Graphic list indentifier.
            AIL_ID AilSearchContext = AIL.M_NULL;                       // Search context.
            AIL_ID AilResult = AIL.M_NULL;                              // Result identifier.
            double ModelDrawColor = AIL.M_COLOR_RED;                    // Model draw color.
            AIL_INT[] Model = new AIL_INT[MODEL_MAX_OCCURRENCES];               // Model index.
            AIL_INT NumResults = 0;                                         // Number of results found.

            double[] Score = new double[MODEL_MAX_OCCURRENCES];         // Model correlation score.
            double[] XPosition = new double[MODEL_MAX_OCCURRENCES];     // Model X position.
            double[] YPosition = new double[MODEL_MAX_OCCURRENCES];     // Model Y position. 
            double[] Angle = new double[MODEL_MAX_OCCURRENCES];         // Model occurrence angle.
            double[] Scale = new double[MODEL_MAX_OCCURRENCES];         // Model occurrence scale.
            double Time = 0.0;                                          // Bench variable.
            int i = 0;                                                  // Loop variable.

            // Restore the model image and display it.
            AIL.MbufRestore(SINGLE_MODEL_IMAGE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref GraphicList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

            // Allocate a Geometric Model Finder context.
            AIL.MmodAlloc(AilSystem, AIL.M_GEOMETRIC, AIL.M_DEFAULT, ref AilSearchContext);

            // Allocate a result buffer.
            AIL.MmodAllocResult(AilSystem, AIL.M_DEFAULT, ref AilResult);

            // Define the model.
            AIL.MmodDefine(AilSearchContext, AIL.M_IMAGE, AilImage, MODEL_OFFSETX, MODEL_OFFSETY, MODEL_SIZEX, MODEL_SIZEY);

            // Set the search speed.
            AIL.MmodControl(AilSearchContext, AIL.M_CONTEXT, AIL.M_SPEED, SINGLE_MODEL_SEARCH_SPEED);

            // Preprocess the search context.
            AIL.MmodPreprocess(AilSearchContext, AIL.M_DEFAULT);

            // Draw box and position it in the source image to show the model.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, ModelDrawColor);
            AIL.MmodDraw(AIL.M_DEFAULT, AilSearchContext, GraphicList,
                     AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, 0, AIL.M_ORIGINAL);

            // Pause to show the model.
            Console.Write("\nGEOMETRIC MODEL FINDER:\n");
            Console.Write("-----------------------\n\n");
            Console.Write("A model context was defined with the model in the displayed image.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Clear annotations.
            AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

            // Load the single model target image.
            AIL.MbufLoad(SINGLE_MODEL_TARGET_IMAGE, AilImage);

            // Dummy first call for bench measure purpose only (bench stabilization, cache effect, etc...). This first call is NOT required by the application.
            AIL.MmodFind(AilSearchContext, AilImage, AilResult);

            // Reset the timer.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            // Find the model.
            AIL.MmodFind(AilSearchContext, AilImage, AilResult);

            // Read the find time.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref Time);

            // Get the number of models found.
            AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NumResults);

            // If a model was found above the acceptance threshold.
            if ((NumResults >= 1) && (NumResults <= MODEL_MAX_OCCURRENCES))
            {
                // Get the results of the single model.
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_INDEX + AIL.M_TYPE_AIL_INT, Model);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_POSITION_X, XPosition);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_POSITION_Y, YPosition);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_ANGLE, Angle);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_SCALE, Scale);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_SCORE, Score);

                // Print the results for each model found.
                Console.Write("The model was found in the target image:\n\n");
                Console.Write("Result   Model   X Position   Y Position   Angle   Scale   Score\n\n");
                for (i = 0; i < NumResults; i++)
                {
                    Console.Write("{0,-9}{1,-8}{2,-13:0.00}{3,-13:0.00}{4,-8:0.00}{5,-8:0.00}{6,-5:0.00}%\n", i, Model[i], XPosition[i], YPosition[i], Angle[i], Scale[i], Score[i]);
                }
                Console.Write("\nThe search time is {0:0.0} ms\n\n", Time * 1000.0);

                // Draw edges, position and box over the occurrences that were found.
                for (i = 0; i < NumResults; i++)
                {
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, ModelDrawColor);
                    AIL.MmodDraw(AIL.M_DEFAULT, AilResult, GraphicList, AIL.M_DRAW_EDGES + AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, i, AIL.M_DEFAULT);
                }
            }
            else
            {
                Console.Write("The model was not found or the number of models found is greater than\n");
                Console.Write("the specified maximum number of occurrence !\n\n");
            }

            // Wait for a key to be pressed.
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Free objects.
            AIL.MgraFree(GraphicList);
            AIL.MbufFree(AilImage);
            AIL.MmodFree(AilSearchContext);
            AIL.MmodFree(AilResult);

        }

        //****************************************************************************
        // Multiple models example.
        // Source image file specifications.
        private const string MULTI_MODELS_IMAGE = AIL.M_IMAGE_PATH + "MultipleModel.mim";

        // Target image file specifications.
        private const string MULTI_MODELS_TARGET_IMAGE = AIL.M_IMAGE_PATH + "MultipleTarget.mim";

        // Search speed:  M_VERY_HIGH for faster search, M_MEDIUM for precision and robustness.
        private const int MULTI_MODELS_SEARCH_SPEED = AIL.M_VERY_HIGH;

        // Number of models.
        private const int NUMBER_OF_MODELS = 3;
        private const int MODELS_MAX_OCCURRENCES = 16;

        // Model 1 specifications.
        private const int MODEL0_OFFSETX = 34;
        private const int MODEL0_OFFSETY = 93;
        private const int MODEL0_SIZEX = 214;
        private const int MODEL0_SIZEY = 76;
        private static readonly int MODEL0_DRAW_COLOR = AIL.M_COLOR_RED;

        // Model 2 specifications.
        private const int MODEL1_OFFSETX = 73;
        private const int MODEL1_OFFSETY = 232;
        private const int MODEL1_SIZEX = 150;
        private const int MODEL1_SIZEY = 154;
        private const int MODEL1_REFERENCEX = 23;
        private const int MODEL1_REFERENCEY = 127;
        private static readonly int MODEL1_DRAW_COLOR = AIL.M_COLOR_GREEN;

        // Model 3 specifications.
        private const int MODEL2_OFFSETX = 308;
        private const int MODEL2_OFFSETY = 39;
        private const int MODEL2_SIZEX = 175;
        private const int MODEL2_SIZEY = 357;
        private const int MODEL2_REFERENCEX = 62;
        private const int MODEL2_REFERENCEY = 150;
        private static readonly int MODEL2_DRAW_COLOR = AIL.M_COLOR_BLUE;

        // Models array size.
        private const int MODELS_ARRAY_SIZE = 3;

        static void MultipleModelExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;                               // Image buffer identifier.
            AIL_ID GraphicList = AIL.M_NULL;                            // Graphic list identifier.
            AIL_ID AilSearchContext = AIL.M_NULL;                       // Search context.
            AIL_ID AilResult = AIL.M_NULL;                              // Result identifier.
            double[] ModelsDrawColor = new double[MODELS_ARRAY_SIZE];   // Model drawing colors.
            AIL_INT NumResults = 0;                                     // Number of results found.
            AIL_INT[] Models = new AIL_INT[MODELS_MAX_OCCURRENCES];     // Model indices.
            AIL_INT[] ModelsOffsetX = new AIL_INT[MODELS_ARRAY_SIZE];   // Model X offsets array.
            AIL_INT[] ModelsOffsetY = new AIL_INT[MODELS_ARRAY_SIZE];   // Model Y offsets array.
            AIL_INT[] ModelsSizeX = new AIL_INT[MODELS_ARRAY_SIZE];     // Model X sizes array.
            AIL_INT[] ModelsSizeY = new AIL_INT[MODELS_ARRAY_SIZE];     // Model Y sizes array.
            double[] Score = new double[MODELS_MAX_OCCURRENCES];        // Model correlation scores.
            double[] XPosition = new double[MODELS_MAX_OCCURRENCES];    // Model X positions.
            double[] YPosition = new double[MODELS_MAX_OCCURRENCES];    // Model Y positions.
            double[] Angle = new double[MODELS_MAX_OCCURRENCES];        // Model occurrence angles.
            double[] Scale = new double[MODELS_MAX_OCCURRENCES];        // Model occurrence scales.
            double Time = 0.0;                                          // Time variable.
            int i;                                                      // Loop variable.

            // Models array specifications.
            ModelsOffsetX[0] = MODEL0_OFFSETX;
            ModelsOffsetX[1] = MODEL1_OFFSETX;
            ModelsOffsetX[2] = MODEL2_OFFSETX;

            ModelsOffsetY[0] = MODEL0_OFFSETY;
            ModelsOffsetY[1] = MODEL1_OFFSETY;
            ModelsOffsetY[2] = MODEL2_OFFSETY;

            ModelsSizeX[0] = MODEL0_SIZEX;
            ModelsSizeX[1] = MODEL1_SIZEX;
            ModelsSizeX[2] = MODEL2_SIZEX;

            ModelsSizeY[0] = MODEL0_SIZEY;
            ModelsSizeY[1] = MODEL1_SIZEY;
            ModelsSizeY[2] = MODEL2_SIZEY;

            ModelsDrawColor[0] = MODEL0_DRAW_COLOR;
            ModelsDrawColor[1] = MODEL1_DRAW_COLOR;
            ModelsDrawColor[2] = MODEL2_DRAW_COLOR;

            // Restore the model image and display it.
            AIL.MbufRestore(MULTI_MODELS_IMAGE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref GraphicList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

            // Allocate a geometric model finder.
            AIL.MmodAlloc(AilSystem, AIL.M_GEOMETRIC, AIL.M_DEFAULT, ref AilSearchContext);

            // Allocate a result buffer.
            AIL.MmodAllocResult(AilSystem, AIL.M_DEFAULT, ref AilResult);

            // Define the models.
            for (i = 0; i < NUMBER_OF_MODELS; i++)
            {
                AIL.MmodDefine(AilSearchContext, AIL.M_IMAGE, AilImage, (double)ModelsOffsetX[i], (double)ModelsOffsetY[i], (double)ModelsSizeX[i], (double)ModelsSizeY[i]);
            }

            // Set the desired search speed.
            AIL.MmodControl(AilSearchContext, AIL.M_CONTEXT, AIL.M_SPEED, MULTI_MODELS_SEARCH_SPEED);

            // Increase the smoothness for the edge extraction in the search context.
            AIL.MmodControl(AilSearchContext, AIL.M_CONTEXT, AIL.M_SMOOTHNESS, 75);

            // Modify the acceptance and the certainty for all the models that were defined.
            AIL.MmodControl(AilSearchContext, AIL.M_DEFAULT, AIL.M_ACCEPTANCE, 40);
            AIL.MmodControl(AilSearchContext, AIL.M_DEFAULT, AIL.M_CERTAINTY, 60);

            // Set the number of occurrences to 2 for all the models that were defined.
            AIL.MmodControl(AilSearchContext, AIL.M_DEFAULT, AIL.M_NUMBER, 2);

            if (NUMBER_OF_MODELS > 1)
            {
                // Change the reference point of the second model.
                AIL.MmodControl(AilSearchContext, 1, AIL.M_REFERENCE_X, MODEL1_REFERENCEX);
                AIL.MmodControl(AilSearchContext, 1, AIL.M_REFERENCE_Y, MODEL1_REFERENCEY);
            }

            if (NUMBER_OF_MODELS > 2)
            {
                // Change the reference point of the third model.
                AIL.MmodControl(AilSearchContext, 2, AIL.M_REFERENCE_X, MODEL2_REFERENCEX);
                AIL.MmodControl(AilSearchContext, 2, AIL.M_REFERENCE_Y, MODEL2_REFERENCEY);
            }
            // Preprocess the search context.
            AIL.MmodPreprocess(AilSearchContext, AIL.M_DEFAULT);

            // Draw boxes and positions in the source image to identify the models.
            for (i = 0; i < NUMBER_OF_MODELS; i++)
            {
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, ModelsDrawColor[i]);
                AIL.MmodDraw(AIL.M_DEFAULT, AilSearchContext, GraphicList, AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, i, AIL.M_ORIGINAL);
            }

            // Pause to show the models.
            Console.Write("A model context was defined with the models in the displayed image.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Clear annotations.
            AIL.MgraClear(AIL.M_DEFAULT, GraphicList);

            // Load the complex target image.
            AIL.MbufLoad(MULTI_MODELS_TARGET_IMAGE, AilImage);

            // Dummy first call for bench measure purpose only (bench stabilization, cache effect, etc...). This first call is NOT required by the application.
            AIL.MmodFind(AilSearchContext, AilImage, AilResult);

            // Reset the timer.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            // Find the models.
            AIL.MmodFind(AilSearchContext, AilImage, AilResult);

            // Read the find time.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref Time);

            // Get the number of models found.
            AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NumResults);

            // If the models were found above the acceptance threshold.
            if ((NumResults >= 1) && (NumResults <= MODELS_MAX_OCCURRENCES))
            {
                // Get the results for each model.
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_INDEX + AIL.M_TYPE_AIL_INT, Models);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_POSITION_X, XPosition);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_POSITION_Y, YPosition);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_ANGLE, Angle);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_SCALE, Scale);
                AIL.MmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_SCORE, Score);

                // Print information about the target image.
                Console.Write("The models were found in the target image although there is:\n");
                Console.Write("   Full rotation\n   Small scale change\n   Contrast variation\n");
                Console.Write("   Specular reflection\n   Occlusion\n   Multiple models\n");
                Console.Write("   Multiple occurrences\n\n");

                // Print the results for the found models.
                Console.Write("Result   Model   X Position   Y Position   Angle   Scale   Score\n\n");
                for (i = 0; i < NumResults; i++)
                {
                    Console.Write("{0,-9}{1,-8}{2,-13:0.00}{3,-13:#.00}{4,-8:0.00}{5,-8:0.00}{6,-5:0.00}%\n", i, Models[i], XPosition[i], YPosition[i], Angle[i], Scale[i], Score[i]);
                }
                Console.Write("\nThe search time is {0:0.0} ms\n\n", Time * 1000.0);

                // Draw edges and positions over the occurrences that were found.
                for (i = 0; i < NumResults; i++)
                {
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, ModelsDrawColor[Models[i]]);
                    AIL.MmodDraw(AIL.M_DEFAULT, AilResult, GraphicList, AIL.M_DRAW_EDGES + AIL.M_DRAW_POSITION, i, AIL.M_DEFAULT);
                }
            }
            else
            {
                Console.Write("The models were not found or the number of models found is greater than\n");
                Console.Write("the defined value of maximum occurrences !\n\n");
            }

            // Wait for a key to be pressed.
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free objects.
            AIL.MgraFree(GraphicList);
            AIL.MbufFree(AilImage);
            AIL.MmodFree(AilSearchContext);
            AIL.MmodFree(AilResult);
        }
    }
}

```
