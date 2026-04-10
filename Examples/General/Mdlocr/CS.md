---
title: "Mdlocr"
description: "This program uses the Deep Learning Ocr module to read a product expiry date. First all strings are read, then a model is defined based on the first read to filter out the target string using its size and dimensions."
ms.language: "C#"
---

# Mdlocr

> This program uses the Deep Learning Ocr module to read a product expiry date. First all strings are read, then a model is defined based on the first read to filter out the target string using its size and dimensions.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MdlocrAlloc`, `MdlocrAllocResult`, `MdlocrControl`, `MdlocrDefineModelFromResult`, `MdlocrDraw`, `MdlocrFree`, `MdlocrGetResult`, `MdlocrPreprocess`, `MdlocrRead`, `MgraControl`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Packaging, Applications, Reading, Modules, Buffer, Display, Deep Learning OCR, Graphics, What's New, AIL 10.0 SP7

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mdlocr.cs
//
// Description: Synopsis: This program uses the Deep Learning Ocr module to read
//              a product expiry date. 
//              First all strings are read, then a model is defined based on the first
//              read to filter out the target string using its size and dimensions.
// 
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Text;
using Zebra.AuroraImagingLibrary;

namespace Mdlocr
    {
    class Program
        {
        // Image file specifications.
        const string IMAGE_FILE_TO_READ = AIL.M_IMAGE_PATH + "ExpiryDateAndLot2.mim";
        // Max string sizes.
        static readonly int STRING_MAX_SIZE = 256;
        // Invalid string index
        static readonly int INVALID_TARGET_READ_INDEX = -1;
        // The target string
        static readonly string TARGET_STRING = "2012JL03ERB1A2217";

        static AIL_INT Read(
            AIL_ID AilDisplay, AIL_ID AilDlocrContext, AIL_ID AilImage,
            AIL_ID AilOverlayImage, AIL_ID AilDlocrResult, AIL_INT StringMatchingMode)
            {
            AIL_INT ReadStatus = AIL.M_READ_NOT_PERFORMED;                      // Status of the read operation
            AIL_INT NumberOfStringRead = 0;                                     // Total number of strings to read.
            StringBuilder StringResult = new StringBuilder(STRING_MAX_SIZE+1);  // String of characters read.

            // Perform the read operation on the specified target image. 
            AIL.MdlocrRead(AilDlocrContext, AilImage, AilDlocrResult, StringMatchingMode);

            // Get the status of the operation
            AIL.MdlocrGetResult(AilDlocrResult, AIL.M_GENERAL, AIL.M_DEFAULT, AIL.M_STATUS + AIL.M_TYPE_AIL_INT, ref ReadStatus);
            if (ReadStatus != AIL.M_COMPLETE)
                {
                Console.Write("Read operation failed.\n");
                return INVALID_TARGET_READ_INDEX;
                }

            // Get number of strings read and show the result. 
            AIL.MdlocrGetResult(
               AilDlocrResult, AIL.M_GENERAL, AIL.M_GENERAL, AIL.M_STRING_NUMBER + AIL.M_TYPE_AIL_INT, ref NumberOfStringRead);

            // Clear the display overlay. 
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            // Find the target string index.
            AIL_INT TargetStringIndex = INVALID_TARGET_READ_INDEX;

            if (NumberOfStringRead >= 1)
                {
                Console.Write("The image was read successfully.\n\n");

                // Draw read result. 
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_CYAN);
                AIL.MdlocrDraw(
                    AIL.M_DEFAULT, AilDlocrResult, AilOverlayImage, AIL.M_DRAW_STRING,
                    AIL.M_ALL, AIL.M_DEFAULT, AIL.M_DEFAULT);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN);
                AIL.MdlocrDraw(AIL.M_DEFAULT, AilDlocrResult, AilOverlayImage, AIL.M_DRAW_STRING_BOX,
                   AIL.M_ALL, AIL.M_DEFAULT, AIL.M_DEFAULT);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);
                AIL.MdlocrDraw(AIL.M_DEFAULT, AilDlocrResult, AilOverlayImage, AIL.M_DRAW_STRING_INDEX,
                   AIL.M_ALL, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Print the read result. 
                Console.Write(" String\n");
                Console.Write(" -----------------------------------\n");
                for (AIL_INT i = 0; i < NumberOfStringRead; ++i)
                    {
                    AIL.MdlocrGetResult(AilDlocrResult, i, AIL.M_GENERAL, AIL.M_STRING, StringResult);
                    Console.Write(" {0}\n", StringResult);
                    // Check if the string read is the one being looked for.
                    if (StringResult.Equals(TARGET_STRING))
                        {
                        TargetStringIndex = i;
                        }
                    }
                Console.Write("\n");
                }

            if (TargetStringIndex == INVALID_TARGET_READ_INDEX)
                {
                Console.Write("Target string '{0}' was not found.\n", TARGET_STRING);
                }
            return TargetStringIndex;
            }

        static void Main(string[] args)
            {
            AIL_ID AilApplication = AIL.M_NULL;                   // Application identifier.          
            AIL_ID AilSystem = AIL.M_NULL;                        // System identifier.               
            AIL_ID AilDisplay = AIL.M_NULL;                       // Display identifier.              
            AIL_ID AilImage = AIL.M_NULL;                         // Image buffer identifier.         
            AIL_ID AilOverlayImage = AIL.M_NULL;                  // Overlay image.                   
            AIL_ID AilDlocrContext = AIL.M_NULL;                  // Dlocr context identifier.        
            AIL_ID AilDlocrResult = AIL.M_NULL;                   // Dlocr result buffer identifier.  

            AIL_INT MaxSizeX = 0;
            AIL_INT MaxSizeY = 0;

            // Print the example synopsis. 
            Console.Write("[EXAMPLE NAME]\n");
            Console.Write("Mdlocr\n\n");
            Console.Write("[SYNOPSIS]\n");
            Console.Write("This program uses the Deep Learning Ocr module to read a product\n");
            Console.Write("expiry date.\n");
            Console.Write("First all strings are read, then a model is defined based on the first\n");
            Console.Write("read to filter out the target string using its size and dimensions.\n\n");
            Console.Write("[MODULES USED]\n");
            Console.Write("Deep Learning OCR, Buffer, Display, Graphics.\n\n");

            // Allocate defaults. 
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore the font definition image. 
            AIL.MbufRestore(IMAGE_FILE_TO_READ, AilSystem, ref AilImage);

            // Display the image and prepare for overlay annotations. 
            AIL.MdispSelect(AilDisplay, AilImage);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            // Allocate a new empty dlocr context. 
            AIL.MdlocrAlloc(AilSystem, AIL.M_OCR_NET1_BALANCED_V1, AIL.M_DEFAULT, ref AilDlocrContext);

            // Allocate a new empty dlocr result buffer. 
            AIL.MdlocrAllocResult(AilSystem, AIL.M_DLOCR_READ_RESULT, AIL.M_DEFAULT, ref AilDlocrResult);

            // Set the context's target image max size X and Y
            // Note that these dimension can be superior to encompass several images.
            AIL.MbufInquire(AilImage, AIL.M_SIZE_X, ref MaxSizeX);
            AIL.MbufInquire(AilImage, AIL.M_SIZE_Y, ref MaxSizeY);
            AIL.MdlocrControl(AilDlocrContext, AIL.M_TARGET_MAX_SIZE_X, MaxSizeX);
            AIL.MdlocrControl(AilDlocrContext, AIL.M_TARGET_MAX_SIZE_Y, MaxSizeY);

            // Preprocess the dlocr context. 
            AIL.MdlocrPreprocess(AilDlocrContext, AIL.M_DEFAULT);

            // First read. 
            AIL_INT TargetStringIndex = Read(AilDisplay, AilDlocrContext, AilImage, AilOverlayImage, AilDlocrResult, AIL.M_READ_ALL);

            if (TargetStringIndex != INVALID_TARGET_READ_INDEX)
                {
                // Pause to show results. 
                Console.Write(
                    "First read without string model, the best before date was detected among\n");
                Console.Write(
                    "others. We note the associated index {0} (in red). It will be used in the next step\n", TargetStringIndex);
                Console.Write(
                   "to filter out the targeted string.\n");

                Console.Write("Press any key to continue.\n\n");
                Console.ReadKey(true);

                // Define model from result. 
                AIL.MdlocrDefineModelFromResult(AilDlocrContext, AIL.M_DEFAULT, AilDlocrResult,
                                            TargetStringIndex, AIL.M_DEFAULT);

                // This modification necessitates a new Preprocess. 
                AIL.MdlocrPreprocess(AilDlocrContext, AIL.M_DEFAULT);

                // Second read. 
                Read(AilDisplay, AilDlocrContext, AilImage, AilOverlayImage, AilDlocrResult, AIL.M_MODEL_BASED);

                // Pause to show results. 
                Console.Write(
                    "We defined a string model from the result to filter out based on string size\n");
                Console.Write(
                    "and character dimensions.\n");
                Console.Write(
                    "Second read with the string model. Targeted string is detected.\n");
                }

            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations. 
            AIL.MdlocrFree(AilDlocrContext);
            AIL.MdlocrFree(AilDlocrResult);
            AIL.MbufFree(AilImage);

            // Free defaults. 
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
            }
        }
    }




```
