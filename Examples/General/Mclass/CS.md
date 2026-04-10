---
title: "Mclass"
description: "This example identifies the type of product using a pre-trained classification module."
ms.language: "C#"
---

# Mclass

> This example identifies the type of product using a pre-trained classification module.

**Language:** C#

**Functions used:** `MclassPredict`, `MclassPreprocess`, `MclassRestore`, `MclassGetResult`, `MclassInquire`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraControl`, `MgraFont`, `MgraRect`, `MgraText`, `MimResize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Identifying, Inspecting/Proofing/Verifying, Modules, Classification, Buffer, Display, Graphics, Image Processing, What's New, Older, AIL 10.0 U94

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mclass.cs
//
// Description: This example identifies the type of pastas using a 
//              pre-trained classification module. 
//
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Runtime.InteropServices;
using Zebra.AuroraImagingLibrary;

namespace Mclass
{
    // Function declarations.
    class ClassStruct
    {
        public AIL_INT NbCategories;
        public AIL_INT NbOfFrames;
        public AIL_INT SourceSizeX;
        public AIL_INT SourceSizeY;

        public AIL_ID ClassCtx;
        public AIL_ID ClassRes;
        public AIL_ID AilDisplay;
        public AIL_ID AilDispChild;
        public AIL_ID AilOverlayImage;
    }

    public class Program
    {
        // Path definitions.
        private const string EXAMPLE_IMAGE_DIR_PATH = AIL.M_IMAGE_PATH + "/Classification/Pasta/";
        private const string EXAMPLE_CLASS_CTX_PATH = EXAMPLE_IMAGE_DIR_PATH + "PastaEx.mclass";
        private const string TARGET_IMAGE_DIR_PATH = EXAMPLE_IMAGE_DIR_PATH + "Products";

        private const string DIG_IMAGE_FOLDER = TARGET_IMAGE_DIR_PATH;
        private const string DIG_REMOTE_IMAGE_FOLDER = "remote:///" + TARGET_IMAGE_DIR_PATH;

        // Util constant.
        private const int BUFFERING_SIZE_MAX = 10;

        ///****************************************************************************
        //    Main.
        ///****************************************************************************
        static int Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;    // Application identifier
            AIL_ID AilSystem = AIL.M_NULL;         // System identifier
            AIL_ID AilDisplay = AIL.M_NULL;        // Display identifier
            AIL_ID AilOverlay = AIL.M_NULL;        // Overlay identifier
            AIL_ID AilDigitizer = AIL.M_NULL;      // Digitizer identifier
            AIL_ID AilDispImage = AIL.M_NULL;      // Image identifier
            AIL_ID AilDispChild = AIL.M_NULL;      // Image identifier
            AIL_ID ClassCtx = AIL.M_NULL;          // Classification Context
            AIL_ID ClassRes = AIL.M_NULL;          // Classification Result

            AIL_ID[] AilGrabBufferList = new AIL_ID[BUFFERING_SIZE_MAX];   // Image identifier
            AIL_ID[] AilChildBufferList = new AIL_ID[BUFFERING_SIZE_MAX];  // Child identifier

            AIL_INT NumberOfCategories = 0;
            AIL_INT BufIndex = 0;
            AIL_INT SourceSizeX = 0;
            AIL_INT SourceSizeY = 0;
            AIL_INT InputSizeX = 0;
            AIL_INT InputSizeY = 0;

            // Allocate objects.
            AIL.MappAlloc(AIL.M_NULL, AIL.M_DEFAULT, ref AilApplication);
            AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilSystem);
            AIL_INT SystemType = 0;
            AIL.MsysInquire(AilSystem, AIL.M_SYSTEM_TYPE, ref SystemType);
            if (SystemType != AIL.M_SYSTEM_HOST_TYPE)
                {
                AIL.MsysFree(AilSystem);
                AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_HOST, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilSystem);
                }

            AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "AIL.M_DEFAULT", AIL.M_DEFAULT, ref AilDisplay);

            AIL_INT AilSystemLocation = AIL.MsysInquire(AilSystem, AIL.M_LOCATION, AIL.M_NULL);
            string DigImageFolder = (AilSystemLocation == AIL.M_REMOTE) ? DIG_REMOTE_IMAGE_FOLDER : DIG_IMAGE_FOLDER;
            AIL.MdigAlloc(AilSystem, AIL.M_DEFAULT, DigImageFolder, AIL.M_DEFAULT, ref AilDigitizer);

            // Print the example synopsis.
            Console.WriteLine("[EXAMPLE NAME]");
            Console.WriteLine("Mclass");
            Console.WriteLine();
            Console.WriteLine("[SYNOPSIS]");
            Console.WriteLine("This programs shows the use of a pre-trained classification");
            Console.WriteLine("tool to recognize product categories.");
            Console.WriteLine();
            Console.WriteLine("[MODULES USED]");
            Console.WriteLine("Classification, Buffer, Display, Graphics, Image Processing.");
            Console.WriteLine();

            // Wait for user.
            Console.WriteLine("Press any key to continue.");
            Console.ReadKey(true);

            Console.Write("Restoring the classification context from file..");
            AIL.MclassRestore(EXAMPLE_CLASS_CTX_PATH, AilSystem, AIL.M_DEFAULT, ref ClassCtx);
            Console.Write(".");

            // Preprocess the context.
            AIL.MclassPreprocess(ClassCtx, AIL.M_DEFAULT);
            Console.WriteLine(".ready.");

            AIL.MclassInquire(ClassCtx, AIL.M_CONTEXT, AIL.M_NUMBER_OF_CLASSES + AIL.M_TYPE_AIL_INT, ref NumberOfCategories);
            AIL.MclassInquire(ClassCtx, AIL.M_DEFAULT_SOURCE_LAYER, AIL.M_SIZE_X + AIL.M_TYPE_AIL_INT, ref InputSizeX);
            AIL.MclassInquire(ClassCtx, AIL.M_DEFAULT_SOURCE_LAYER, AIL.M_SIZE_Y + AIL.M_TYPE_AIL_INT, ref InputSizeY);

            if (NumberOfCategories > 0)
            {
                // Inquire and print source layer information.
                Console.WriteLine(" - The classifier was trained to recognize {0} categories", NumberOfCategories);
                Console.WriteLine(" - The classifier was trained for {0}x{1} source images", InputSizeX, InputSizeY);
                Console.WriteLine();

                // Allocate a classification result buffer.
                AIL.MclassAllocResult(AilSystem, AIL.M_PREDICT_CNN_RESULT, AIL.M_DEFAULT, ref ClassRes);

                // Inquire the size of the source image.
                AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X, ref SourceSizeX);
                AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y, ref SourceSizeY);

                // Setup the example display.
                SetupDisplay(AilSystem,
                             AilDisplay,
                             SourceSizeX,
                             SourceSizeY,
                             ClassCtx,
                             out AilDispImage,
                             out AilDispChild,
                             out AilOverlay,
                             NumberOfCategories);

                // Retrieve the number of frame in the source directory.
                AIL_INT NumberOfFrames = 0;
                AIL.MdigInquire(AilDigitizer, AIL.M_SOURCE_NUMBER_OF_FRAMES, ref NumberOfFrames);

                // Prepare data for Hook Function.
                ClassStruct ClassificationData = new ClassStruct();
                ClassificationData.ClassCtx = ClassCtx;
                ClassificationData.ClassRes = ClassRes;
                ClassificationData.AilDisplay = AilDisplay;
                ClassificationData.AilDispChild = AilDispChild;
                ClassificationData.NbCategories = NumberOfCategories;
                ClassificationData.AilOverlayImage = AilOverlay;
                ClassificationData.SourceSizeX = SourceSizeX;
                ClassificationData.SourceSizeY = SourceSizeY;
                ClassificationData.NbOfFrames = NumberOfFrames;

                // Allocate the grab buffers.
                for (BufIndex = 0; BufIndex < BUFFERING_SIZE_MAX; BufIndex++)
                    {
                    AIL.MbufAlloc2d(AilSystem, SourceSizeX, SourceSizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC, ref AilGrabBufferList[BufIndex]);
                    AIL.MbufChild2d(AilGrabBufferList[BufIndex], (SourceSizeX - InputSizeX) / 2, (SourceSizeY - InputSizeY) / 2, InputSizeX, InputSizeY, ref AilChildBufferList[BufIndex]);
                    AIL.MobjControl(AilGrabBufferList[BufIndex], AIL.M_OBJECT_USER_DATA_PTR, AilChildBufferList[BufIndex]);
                    }


                // Start the grab.
                AIL_DIG_HOOK_FUNCTION_PTR ClassificationFuncHook = new AIL_DIG_HOOK_FUNCTION_PTR(ClassificationFunc);
                GCHandle ClassificationDataHandle = GCHandle.Alloc(ClassificationData);
                if (NumberOfFrames != AIL.M_INFINITE)
                    AIL.MdigProcess(AilDigitizer, AilGrabBufferList, BUFFERING_SIZE_MAX, AIL.M_SEQUENCE + AIL.M_COUNT(NumberOfFrames), AIL.M_SYNCHRONOUS, ClassificationFuncHook, GCHandle.ToIntPtr(ClassificationDataHandle));
                else
                    AIL.MdigProcess(AilDigitizer, AilGrabBufferList, BUFFERING_SIZE_MAX, AIL.M_START, AIL.M_DEFAULT, ClassificationFuncHook, GCHandle.ToIntPtr(ClassificationDataHandle));

                // Ready to exit.
                Console.WriteLine();
                Console.WriteLine("Press any key to exit.");
                Console.ReadKey(true);

                // Stop the digitizer.
                AIL.MdigProcess(AilDigitizer, AilGrabBufferList, BUFFERING_SIZE_MAX, AIL.M_STOP, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL);

                ClassificationDataHandle.Free();
                GC.KeepAlive(ClassificationFuncHook);

                // Free the allocated resources.
                AIL.MbufFree(AilDispChild);
                AIL.MbufFree(AilDispImage);

                for (BufIndex = 0; BufIndex < BUFFERING_SIZE_MAX; BufIndex++)
                    {
                    AIL.MbufFree(AilChildBufferList[BufIndex]);
                    AIL.MbufFree(AilGrabBufferList[BufIndex]);
                    }

                AIL.MclassFree(ClassRes);
                AIL.MclassFree(ClassCtx);
            }

            // Free the allocated resources.
            AIL.MdigFree(AilDigitizer);

            AIL.MdispFree(AilDisplay);
            AIL.MsysFree(AilSystem);
            AIL.MappFree(AilApplication);

            return 0;
        }

        private static void SetupDisplay(AIL_ID AilSystem,
                  AIL_ID AilDisplay,
                  AIL_INT SourceSizeX,
                  AIL_INT SourceSizeY,
                  AIL_ID ClassCtx,
                  out AIL_ID AilDispImage,
                  out AIL_ID AilDispChild,
                  out AIL_ID AilOverlay,
                  AIL_INT NbCategories
                  )
        {
            AIL_ID AilImageLoader = AIL.M_NULL;  // Image identifier       
            AIL_ID AilChildSample = AIL.M_NULL;  // Child image identifier

            // Allocate a color buffer.
            AIL_INT IconSize = SourceSizeY / NbCategories;
            AilDispImage = AIL.MbufAllocColor(AilSystem, 3, SourceSizeX + IconSize, SourceSizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, AIL.M_NULL);
            AIL.MbufClear(AilDispImage, AIL.M_COLOR_BLACK);
            AilDispChild = AIL.MbufChild2d(AilDispImage, 0, 0, SourceSizeX, SourceSizeY, AIL.M_NULL);

            // Set annotation color.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);

            // Setup the display.
            for (int iter = 0; iter < NbCategories; iter++)
            {
                // Allocate a child buffer per product categorie.   
                AIL.MbufChild2d(AilDispImage, SourceSizeX, iter * IconSize, IconSize, IconSize, ref AilChildSample);

                // Load the sample image.
                AIL.MclassInquire(ClassCtx, AIL.M_CLASS_INDEX(iter), AIL.M_CLASS_ICON_ID + AIL.M_TYPE_AIL_ID, ref AilImageLoader);

                if (AilImageLoader != AIL.M_NULL)
                { AIL.MimResize(AilImageLoader, AilChildSample, AIL.M_FILL_DESTINATION, AIL.M_FILL_DESTINATION, AIL.M_BICUBIC + AIL.M_OVERSCAN_FAST); }

                // Draw an initial red rectangle around the buffer.
                AIL.MgraRect(AIL.M_DEFAULT, AilChildSample, 0, 1, IconSize - 1, IconSize - 2);

                // Free the allocated buffers.
                AIL.MbufFree(AilChildSample);
            }

            // Display the window with black color.
            AIL.MdispSelect(AilDisplay, AilDispImage);

            // Prepare for overlay annotations.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AilOverlay = (AIL_ID)AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, AIL.M_NULL);
        }

        private static AIL_INT ClassificationFunc(AIL_INT HookType, AIL_ID EventId, IntPtr DataPtr)
        {
            AIL_ID AilImage = AIL.M_NULL;
            AIL_ID pAilInputImage = AIL.M_NULL;

            AIL.MdigGetHookInfo(EventId, AIL.M_MODIFIED_BUFFER + AIL.M_BUFFER_ID, ref AilImage);

            ClassStruct data = (ClassStruct)GCHandle.FromIntPtr(DataPtr).Target;
            AIL.MdispControl(data.AilDisplay, AIL.M_UPDATE, AIL.M_DISABLE);
            pAilInputImage = (AIL_ID)AIL.MobjInquire(AilImage, AIL.M_OBJECT_USER_DATA_PTR, AIL.M_NULL);

            // Display the new target image.
            AIL.MbufCopy(AilImage, data.AilDispChild);

            // Perform product recognition using the classification module.
            AIL.MclassPredict(data.ClassCtx, pAilInputImage, data.ClassRes, AIL.M_DEFAULT);

            // Retrieve best classification score and class index.
            double BestScore = 0;
            AIL.MclassGetResult(data.ClassRes, AIL.M_GENERAL, AIL.M_BEST_CLASS_SCORE + AIL.M_TYPE_AIL_DOUBLE, ref BestScore);

            AIL_INT BestIndex = 0;
            AIL.MclassGetResult(data.ClassRes, AIL.M_GENERAL, AIL.M_BEST_CLASS_INDEX + AIL.M_TYPE_AIL_INT, ref BestIndex);

            // Clear the overlay buffer.
            AIL.MdispControl(data.AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_TRANSPARENT_COLOR);

            // Draw a green rectangle around the winning sample.
            AIL_INT IconSize = data.SourceSizeY / data.NbCategories;
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN);
            AIL.MgraRect(AIL.M_DEFAULT, data.AilOverlayImage, data.SourceSizeX, (BestIndex * IconSize) + 1, data.SourceSizeX + IconSize - 1, (BestIndex + 1) * IconSize - 2);

            // Print the classification accuracy in the sample buffer.
            string Accuracy_text = string.Format("{0:N1}% score", BestScore);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKGROUND_MODE, AIL.M_TRANSPARENT);
            AIL.MgraFont(AIL.M_DEFAULT, AIL.M_FONT_DEFAULT_SMALL);
            AIL.MgraText(AIL.M_DEFAULT, data.AilOverlayImage, data.SourceSizeX + 2, BestIndex * IconSize + 4, Accuracy_text);

            // Update the display.
            AIL.MdispControl(data.AilDisplay, AIL.M_UPDATE, AIL.M_ENABLE);

            // Wait for the user.
            if (data.NbOfFrames != AIL.M_INFINITE)
            {
                Console.Write("Press any key to continue.\r");
                Console.ReadKey(true);
            }

            return 0;
        }

    }
}
```
