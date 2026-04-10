---
title: "MmodelTracking"
description: "This program shows how to track a unique object using pattern recognition. It allocates a model in the field of view of the camera and finds it in a loop. It also prints the coordinates of the found model and draws a box around it. It searches using 2 methods, the normalized grayscale correlation (Mpat), which is very fast and with the Geometric Model Finder (Mmod), which is independent of the model rotation and scale but slower. Note: Display update and annotations drawing can require significant CPU usage."
ms.language: "C#"
---

# MmodelTracking

> This program shows how to track a unique object using pattern recognition. It allocates a model in the field of view of the camera and finds it in a loop. It also prints the coordinates of the found model and draws a box around it. It searches using 2 methods, the normalized grayscale correlation (Mpat), which is very fast and with the Geometric Model Finder (Mmod), which is independent of the model rotation and scale but slower. Note: Display update and annotations drawing can require significant CPU usage.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufAlloc2d`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MdigAlloc`, `MdigControl`, `MdigFree`, `MdigGrab`, `MdigGrabContinuous`, `MdigGrabWait`, `MdigHalt`, `MdigInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraRect`, `MmodAlloc`, `MmodAllocResult`, `MmodControl`, `MmodDefine`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodInquire`, `MmodPreprocess`, `MpatAlloc`, `MpatAllocResult`, `MpatControl`, `MpatDefine`, `MpatDraw`, `MpatFind`, `MpatFree`, `MpatGetResult`, `MpatInquire`, `MpatPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Finding/Locating, Modules, Buffer, Display, Graphics, Digitizer, Pattern Matching, Geometric Model Finder, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MmodelTracking.cs
//
// Description: This program shows how to track a unique object using 
//              pattern recognition. It allocates a model in the field of 
//              view of the camera and finds it in a loop. It also prints 
//              the coordinates of the found model and draws a box around it.
//              It searches using 2 methods, the normalized grayscale 
//              correlation (AIL.Mpat), which is very fast and with the Geometric 
//              Model Finder (AIL.Mmod), which is independent of the model rotation 
//              and scale but slower.
//
// Note:        Display update and annotations drawing can require
//              significant CPU usage.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MModelTracking
{
    class Program
    {
        // Model specification.
        private const int MODEL_WIDTH = 128;
        private const int MODEL_HEIGHT = 128;

        static AIL_INT MODEL_POS_X_INIT(AIL_ID TargetImage) { return (AIL.MbufInquire(TargetImage, AIL.M_SIZE_X, AIL.M_NULL) / 2); }
        static AIL_INT MODEL_POS_Y_INIT(AIL_ID TargetImage) { return (AIL.MbufInquire(TargetImage, AIL.M_SIZE_Y, AIL.M_NULL) / 2); }

        // Minimum score to consider the object found (in percent).
        private const double MODEL_MIN_MATCH_SCORE = 50.0;

        // Drawing color
        private const int DRAW_COLOR = 0xFF; // White

        // Example selection.
        private const int RUN_PAT_TRACKING_EXAMPLE = AIL.M_YES;
        private const int RUN_MOD_TRACKING_EXAMPLE = AIL.M_YES;

        //****************************************************************************
        // Main.
        //****************************************************************************
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;   // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;        // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;       // Display identifier.
            AIL_ID AilDigitizer = AIL.M_NULL;     // Digitizer identifier.
            AIL_ID AilDisplayImage = AIL.M_NULL;  // Display image identifier.
            AIL_ID AilModelImage = AIL.M_NULL;    // Model image identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, ref AilDigitizer, ref AilDisplayImage);

            // Allocate a model image buffer.
            AIL.MbufAlloc2d(AilSystem, AIL.MbufInquire(AilDisplayImage, AIL.M_SIZE_X, AIL.M_NULL), AIL.MbufInquire(AilDisplayImage, AIL.M_SIZE_Y, AIL.M_NULL), 8, AIL.M_IMAGE + AIL.M_PROC, ref AilModelImage);

            Console.Write("\nMODEL TRACKING:\n");
            Console.Write("---------------\n\n");

            // Get the model image.
            GetModelImage(AilSystem, AilDisplay, AilDigitizer, AilDisplayImage, AilModelImage);

            if (RUN_PAT_TRACKING_EXAMPLE == AIL.M_YES)
            {
                // Finds the model using pattern matching.
                MpatTrackingExample(AilSystem, AilDisplay, AilDigitizer, AilDisplayImage, AilModelImage);
            }

            if (RUN_MOD_TRACKING_EXAMPLE == AIL.M_YES)
            {
                // Finds the model using geometric model finder.
                MmodTrackingExample(AilSystem, AilDisplay, AilDigitizer, AilDisplayImage, AilModelImage);
            }

            // Free allocated buffers.
            AIL.MbufFree(AilModelImage);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, AilDisplayImage);
        }

        //****************************************************************************
        // Get Model Image Function.
        //****************************************************************************
        static void GetModelImage(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilDigitizer, AIL_ID AilDisplayImage, AIL_ID AilModelImage)
        {
            AIL_ID AilOverlayImage = AIL.M_NULL;        // Overlay image.
            double DrawColor = DRAW_COLOR;              // Drawing color.

            // Prepare for overlay annotations.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            // Draw the position of the model to define in the overlay.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, DrawColor);
            AIL.MgraRect(AIL.M_DEFAULT, 
                         AilOverlayImage, 
                         MODEL_POS_X_INIT(AilOverlayImage) - (MODEL_WIDTH / 2), 
                         MODEL_POS_Y_INIT(AilOverlayImage) - (MODEL_HEIGHT / 2), 
                         MODEL_POS_X_INIT(AilOverlayImage) + (MODEL_WIDTH / 2), 
                         MODEL_POS_Y_INIT(AilOverlayImage) + (MODEL_HEIGHT / 2));

            // Grab continuously.
            Console.Write("Model definition:\n\n");
            Console.Write("Place a unique model to find in the marked rectangle.\n");
            Console.Write("Press any key to continue.\n\n");

            // Grab a reference model image.
            AIL.MdigGrabContinuous(AilDigitizer, AilDisplayImage);
            Console.ReadKey(true);
            AIL.MdigHalt(AilDigitizer);

            // Copy the grabbed image to the Model image to keep it.
            AIL.MbufCopy(AilDisplayImage, AilModelImage);

            // Clear and disable the overlay.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_DISABLE);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);
        }

        //****************************************************************************
        // Tracking object with pattern matching module.
        //****************************************************************************
        static void MpatTrackingExample(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilDigitizer, AIL_ID AilDisplayImage, AIL_ID AilModelImage)
        {
            AIL_ID[] AilImage = new AIL_ID[2] { AIL.M_NULL, AIL.M_NULL };   // Processing image buffer identifiers.
            AIL_ID ContextId = AIL.M_NULL;                                  // Model identifier.
            AIL_ID Result = AIL.M_NULL;                                     // Result identifier.
            double DrawColor = DRAW_COLOR;                                  // Model drawing color.
            AIL_INT Found = 0;                                              // Number of found models.
            int NbFindDone = 0;                                             // Number of loops to find model done.
            double OrgX = 0.0;                                              // Original center of model.
            double OrgY = 0.0;
            double x = 0.0;                                                 // Result variables.
            double y = 0.0;
            double Score = 0.0;
            double Time = 0.0;                                              // Timer.

            // Print a start message.
            Console.Write("\nGRAYSCALE PATTERN MATCHING:\n");
            Console.Write("---------------------------\n\n");

            // Display the model image.
            AIL.MbufCopy(AilModelImage, AilDisplayImage);

            // Allocate normalized grayscale type model.
            AIL.MpatAlloc(AilSystem, AIL.M_NORMALIZED, AIL.M_DEFAULT, ref ContextId);
            AIL.MpatDefine(ContextId, AIL.M_REGULAR_MODEL, AilModelImage,
                       MODEL_POS_X_INIT(AilModelImage) - (MODEL_WIDTH / 2),
                       MODEL_POS_Y_INIT(AilModelImage) - (MODEL_HEIGHT / 2),
                       MODEL_WIDTH, MODEL_HEIGHT, AIL.M_DEFAULT);


            // Allocate result.
            AIL.MpatAllocResult(AilSystem, AIL.M_DEFAULT, ref Result);

            // Draw box around the model.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, DrawColor);
            AIL.MpatDraw(AIL.M_DEFAULT, ContextId, AilDisplayImage, AIL.M_DRAW_BOX, AIL.M_DEFAULT, AIL.M_ORIGINAL);

            // Set minimum acceptance for search.
            AIL.MpatControl(ContextId, 0, AIL.M_ACCEPTANCE, MODEL_MIN_MATCH_SCORE);

            // Set speed.
            AIL.MpatControl(ContextId, 0, AIL.M_SPEED, AIL.M_HIGH);

            // Set accuracy.
            AIL.MpatControl(ContextId, 0, AIL.M_ACCURACY, AIL.M_LOW);

            // Preprocess model.
            AIL.MpatPreprocess(ContextId, AIL.M_DEFAULT, AilModelImage);

            // Inquire about center of model.
            AIL.MpatInquire(ContextId, 0, AIL.M_ORIGINAL_X, ref OrgX);
            AIL.MpatInquire(ContextId, 0, AIL.M_ORIGINAL_Y, ref OrgY);

            // Print the original position.
            Console.Write("A Grayscale Model was defined.\n");
            Console.Write("Model dimensions:  {0} x {1}.\n", MODEL_WIDTH, MODEL_HEIGHT);
            Console.Write("Model center:      X={0:0.00}, Y={1:0.00}.\n", OrgX, OrgY);
            Console.Write("Model is scale and rotation dependant.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Allocate 2 grab buffers.
            AIL.MbufAlloc2d(AilSystem, AIL.MbufInquire(AilModelImage, AIL.M_SIZE_X, AIL.M_NULL), AIL.MbufInquire(AilModelImage, AIL.M_SIZE_Y, AIL.M_NULL), 8, AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC, ref AilImage[0]);
            AIL.MbufAlloc2d(AilSystem, AIL.MbufInquire(AilModelImage, AIL.M_SIZE_X, AIL.M_NULL), AIL.MbufInquire(AilModelImage, AIL.M_SIZE_Y, AIL.M_NULL), 8, AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC, ref AilImage[1]);

            // Grab continuously and perform the find operation using double buffering.
            Console.Write("\nContinuously finding the Grayscale model.\n");
            Console.Write("Press any key to stop.\n\n");

            // Grab a first target image into first buffer (done twice for timer reset accuracy).
            AIL.MdigControl(AilDigitizer, AIL.M_GRAB_MODE, AIL.M_ASYNCHRONOUS);
            AIL.MdigGrab(AilDigitizer, AilImage[0]);
            AIL.MdigGrab(AilDigitizer, AilImage[1]);
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET, ref Time);

            // Loop, processing one buffer while grabbing the other.
            do
            {
                // Grab a target image into the other buffer.
                AIL.MdigGrab(AilDigitizer, AilImage[NbFindDone % 2]);

                // Read the time.
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, ref Time);

                // Find model.
                AIL.MpatFind(ContextId, AilImage[(NbFindDone + 1) % 2], Result);

                // Get results.
                AIL.MpatGetResult(Result, AIL.M_GENERAL, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref Found);
                AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_POSITION_X, ref x);
                AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_POSITION_Y, ref y);
                AIL.MpatGetResult(Result, AIL.M_DEFAULT, AIL.M_SCORE, ref Score);

                // Print a message based upon the score.
                if (Found > 0)
                {
                    // Draw a box around the model and print the results.
                    AIL.MpatDraw(AIL.M_DEFAULT, Result, AilImage[(NbFindDone + 1) % 2], AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_DEFAULT);
                    Console.Write("Found: X={0,7:0.00}, Y={1,7:0.00}, Score={2,5:0.0}% ({3:0.0} fps).    \r", x, y, Score, (NbFindDone + 1) / Time);
                }
                else
                {
                    // Print the "not found" message.
                    Console.Write("Not found ! (score<{0,5:0.0}%)                ({1:0.0} fps).     \r", MODEL_MIN_MATCH_SCORE, (NbFindDone + 1) / Time);
                }

                // Copy target image to the display.
                AIL.MbufCopy(AilImage[(NbFindDone + 1) % 2], AilDisplayImage);

                // Increment find count
                NbFindDone++;
            }
            while (!Console.KeyAvailable);

            Console.ReadKey(true);
            Console.Write("\n\n");

            // Wait for end of last grab.
            AIL.MdigGrabWait(AilDigitizer, AIL.M_GRAB_END);

            // Free all allocated objects.
            AIL.MpatFree(Result);
            AIL.MpatFree(ContextId);
            AIL.MbufFree(AilImage[1]);
            AIL.MbufFree(AilImage[0]);
        }


        /*****************************************************************************
        Tracking object with Geometric Model Finder module
        *****************************************************************************/
        private const int MODEL_MAX_OCCURRENCES = 16;

        static void MmodTrackingExample(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilDigitizer, AIL_ID AilDisplayImage, AIL_ID AilModelImage)
        {
            AIL_ID[] AilImage = new AIL_ID[2] { AIL.M_NULL, AIL.M_NULL };       // Processing image buffer identifiers.
            AIL_ID SearchContext = AIL.M_NULL;                                  // Search context identifier.          
            AIL_ID Result = AIL.M_NULL;                                         // Result identifier.                  
            double DrawColor = DRAW_COLOR;                                      // Model drawing color.                
            AIL_INT Found = 0;                                                  // Number of models found.             
            int NbFindDone = 0;                                                 // Number of loops to find model done. 
            double OrgX = 0.0;                                                  // Original center of model.
            double OrgY = 0.0;
            double[] Score = new double[MODEL_MAX_OCCURRENCES];                 // Model correlation score.
            double[] x = new double[MODEL_MAX_OCCURRENCES];                     // Model X position.
            double[] y = new double[MODEL_MAX_OCCURRENCES];                     // Model Y position.
            double[] Angle = new double[MODEL_MAX_OCCURRENCES];                 // Model occurrence angle.
            double[] Scale = new double[MODEL_MAX_OCCURRENCES];                 // Model occurrence scale.
            double Time = 0.0;                                                  // Timer.

            // Print a start message.
            Console.Write("\nGEOMETRIC MODEL FINDER (scale and rotation independent):\n");
            Console.Write("--------------------------------------------------------\n\n");

            // Display model image.
            AIL.MbufCopy(AilModelImage, AilDisplayImage);

            // Allocate a context and define a geometric model.
            AIL.MmodAlloc(AilSystem, AIL.M_GEOMETRIC, AIL.M_DEFAULT, ref SearchContext);
            AIL.MmodDefine(SearchContext, AIL.M_IMAGE, AilModelImage, (double)MODEL_POS_X_INIT(AilModelImage) - (MODEL_WIDTH / 2), (double)MODEL_POS_Y_INIT(AilModelImage) - (MODEL_HEIGHT / 2), MODEL_WIDTH, MODEL_HEIGHT);

            // Allocate result.
            AIL.MmodAllocResult(AilSystem, AIL.M_DEFAULT, ref Result);

            // Draw a box around the model.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, DrawColor);
            AIL.MmodDraw(AIL.M_DEFAULT, SearchContext, AilDisplayImage, AIL.M_DRAW_BOX, AIL.M_DEFAULT, AIL.M_ORIGINAL);

            // Set speed to VERY HIGH for fast but less precise search.
            AIL.MmodControl(SearchContext, AIL.M_CONTEXT, AIL.M_SPEED, AIL.M_VERY_HIGH);

            // Set minimum acceptance for the search.
            AIL.MmodControl(SearchContext, AIL.M_DEFAULT, AIL.M_ACCEPTANCE, MODEL_MIN_MATCH_SCORE);

            // Preprocess model.
            AIL.MmodPreprocess(SearchContext, AIL.M_DEFAULT);

            // Inquire about center of model.
            AIL.MmodInquire(SearchContext, AIL.M_DEFAULT, AIL.M_ORIGINAL_X, ref OrgX);
            AIL.MmodInquire(SearchContext, AIL.M_DEFAULT, AIL.M_ORIGINAL_Y, ref OrgY);

            // Print the original position.
            Console.Write("The Geometric target model was defined.\n");
            Console.Write("Model dimensions: {0} x {1}.\n", MODEL_WIDTH, MODEL_HEIGHT);
            Console.Write("Model center:     X={0:0.00}, Y={1:0.00}.\n", OrgX, OrgY);
            Console.Write("Model is scale and rotation independent.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Allocate 2 grab buffers.
            AIL.MbufAlloc2d(AilSystem, AIL.MbufInquire(AilModelImage, AIL.M_SIZE_X, AIL.M_NULL), AIL.MbufInquire(AilModelImage, AIL.M_SIZE_Y, AIL.M_NULL), 8, AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC, ref AilImage[0]);
            AIL.MbufAlloc2d(AilSystem, AIL.MbufInquire(AilModelImage, AIL.M_SIZE_X, AIL.M_NULL), AIL.MbufInquire(AilModelImage, AIL.M_SIZE_Y, AIL.M_NULL), 8, AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC, ref AilImage[1]);

            // Grab continuously grab and perform the find operation using double buffering.
            Console.Write("\nContinuously finding the Geometric Model.\n");
            Console.Write("Press any key to stop.\n\n");

            // Grab a first target image into first buffer (done twice for timer reset accuracy).
            AIL.MdigControl(AilDigitizer, AIL.M_GRAB_MODE, AIL.M_ASYNCHRONOUS);
            AIL.MdigGrab(AilDigitizer, AilImage[0]);
            AIL.MdigGrab(AilDigitizer, AilImage[1]);
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET, ref Time);

            // Loop, processing one buffer while grabbing the other.
            do
            {
                // Grab a target image into the other buffer.
                AIL.MdigGrab(AilDigitizer, AilImage[NbFindDone % 2]);

                // Read the time.
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, ref Time);

                // Find model.
                AIL.MmodFind(SearchContext, AilImage[(NbFindDone + 1) % 2], Result);

                // Get the number of occurrences found.
                AIL.MmodGetResult(Result, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref Found);

                // Print a message based on the score.
                if ((Found >= 1) && (Found < MODEL_MAX_OCCURRENCES))
                {
                    // Get results.
                    AIL.MmodGetResult(Result, AIL.M_DEFAULT, AIL.M_POSITION_X, x);
                    AIL.MmodGetResult(Result, AIL.M_DEFAULT, AIL.M_POSITION_Y, y);
                    AIL.MmodGetResult(Result, AIL.M_DEFAULT, AIL.M_SCALE, Scale);
                    AIL.MmodGetResult(Result, AIL.M_DEFAULT, AIL.M_ANGLE, Angle);
                    AIL.MmodGetResult(Result, AIL.M_DEFAULT, AIL.M_SCORE, Score);

                    // Draw a box and a cross where the model was found and print the results.
                    AIL.MmodDraw(AIL.M_DEFAULT, Result, AilImage[(NbFindDone + 1) % 2], AIL.M_DRAW_BOX + AIL.M_DRAW_POSITION + AIL.M_DRAW_EDGES, AIL.M_DEFAULT, AIL.M_DEFAULT);
                    Console.Write("Found: X={0,6:0.0}, Y={1,6:0.0}, Angle={2,6:0.0}, Scale={3,5:0.00}, Score={4,5:0.0}% ({5,5:0.0} fps).\r", x[0], y[0], Angle[0], Scale[0], Score[0], (NbFindDone + 1) / Time);
                }
                else
                {
                    // Print the "not found" message.
                    Console.Write("Not found! (score<{0,5:0.0}%)                                          ({1,5:0.0} fps).\r", MODEL_MIN_MATCH_SCORE, (NbFindDone + 1) / Time);
                }

                // Copy target image to the display.
                AIL.MbufCopy(AilImage[(NbFindDone + 1) % 2], AilDisplayImage);

                // Increment the counter.
                NbFindDone++;
            }
            while (!Console.KeyAvailable);

            Console.ReadKey(true);
            Console.Write("\n\n");

            // Wait for the end of last grab.
            AIL.MdigGrabWait(AilDigitizer, AIL.M_GRAB_END);

            // Free all allocations.
            AIL.MmodFree(Result);
            AIL.MmodFree(SearchContext);
            AIL.MbufFree(AilImage[1]);
            AIL.MbufFree(AilImage[0]);
        }
    }
}

```
