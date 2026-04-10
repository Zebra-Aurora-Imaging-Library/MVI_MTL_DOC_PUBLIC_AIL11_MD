---
title: "Mblob"
description: "This example uses the Blob module to analyze an image of some nuts and bolts."
ms.language: "C#"
---

# Mblob

> This example uses the Blob module to analyze an image of some nuts and bolts.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobGetResult`, `MblobSelect`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraFree`, `MimBinarize`, `MimClose`, `MimOpen`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Graphics, Image Processing, Blob Analysis, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MBlob.cs
//
// Description: This program loads an image of some nuts, bolts and washers, 
//              determines the number of each of these, finds and marks
//              their center of gravity using the Blob analysis module.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MBlob
{
    class Program
    {
        // Target image file specifications.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "BoltsNutsWashers.mim";
        private const int IMAGE_THRESHOLD_VALUE = 26;

        // Minimum and maximum area of blobs.
        private const int MIN_BLOB_AREA = 50;
        private const int MAX_BLOB_AREA = 50000;

        // Radius of the smallest particles to keep.
        private const int MIN_BLOB_RADIUS = 3;

        // Minimum hole compactness corresponding to a washer.
        private const double MIN_COMPACTNESS = 1.5;
        static void Main(string[] args)
        {

            AIL_ID AilApplication = AIL.M_NULL;                 // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                      // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                     // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;                       // Image buffer identifier.
            AIL_ID AilGraphicList = AIL.M_NULL;                 // Graphic list identifier.
            AIL_ID AilBinImage = AIL.M_NULL;                    // Binary image buffer identifier.
            AIL_ID AilBlobResult = AIL.M_NULL;                  // Blob result buffer identifier.
            AIL_ID AilBlobContext =  AIL.M_NULL;                // Blob Context identifier.
            AIL_INT TotalBlobs = 0;                             // Total number of blobs.
            AIL_INT BlobsWithHoles = 0;                         // Number of blobs with holes.
            AIL_INT BlobsWithRoughHoles = 0;                    // Number of blobs with rough holes.
            AIL_INT n = 0;                                      // Counter.
            AIL_INT SizeX = 0;                                  // Size X of the source buffer
            AIL_INT SizeY = 0;                                  // Size Y of the source buffer
            double[] CogX = null;                               // X coordinate of center of gravity.
            double[] CogY = null;                               // Y coordinate of center of gravity.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore source image into image buffer.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref AilGraphicList);

            // Associate the graphic list to the display.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

            // Display the buffer.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a binary image buffer for fast processing.
            AIL.MbufInquire(AilImage, AIL.M_SIZE_X, ref SizeX);
            AIL.MbufInquire(AilImage, AIL.M_SIZE_Y, ref SizeY);
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 1 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref AilBinImage);

            // Pause to show the original image.
            Console.Write("\nBLOB ANALYSIS:\n");
            Console.Write("--------------\n\n");
            Console.Write("This program determines the number of bolts, nuts and washers\n");
            Console.Write("in the image and finds their center of gravity.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Binarize image.
            AIL.MimBinarize(AilImage, AilBinImage, AIL.M_FIXED + AIL.M_GREATER_OR_EQUAL, IMAGE_THRESHOLD_VALUE, AIL.M_NULL);

            // Remove small particles and then remove small holes.
            AIL.MimOpen(AilBinImage, AilBinImage, MIN_BLOB_RADIUS, AIL.M_BINARY);
            AIL.MimClose(AilBinImage, AilBinImage, MIN_BLOB_RADIUS, AIL.M_BINARY);

            // Allocate a context.
            AIL.MblobAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilBlobContext);

            // Enable the Center Of Gravity feature calculation.
            AIL.MblobControl(AilBlobContext, AIL.M_CENTER_OF_GRAVITY + AIL.M_BINARY, AIL.M_ENABLE);

            // Allocate a blob result buffer.
            AIL.MblobAllocResult(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilBlobResult);

            // Calculate selected features for each blob.
            AIL.MblobCalculate(AilBlobContext, AilBinImage, AIL.M_NULL, AilBlobResult);

            // Exclude blobs whose area is too small.
            AIL.MblobSelect(AilBlobResult, AIL.M_EXCLUDE, AIL.M_AREA, AIL.M_LESS_OR_EQUAL, MIN_BLOB_AREA, AIL.M_NULL);

            // Get the total number of selected blobs.
            AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref TotalBlobs);
            Console.Write("There are {0} objects ", TotalBlobs);

            // Read and print the blob's center of gravity.
            CogX = new double[TotalBlobs];
            CogY = new double[TotalBlobs];
            if (CogX != null && CogY != null)
            {
                // Get the results.
                AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_CENTER_OF_GRAVITY_X + AIL.M_BINARY, CogX);
                AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_CENTER_OF_GRAVITY_Y + AIL.M_BINARY, CogY);

                // Print the center of gravity of each blob.
                Console.Write("and their centers of gravity are:\n");
                for (n = 0; n < TotalBlobs; n++)
                {
                    Console.Write("Blob #{0}: X={1,5:0.0}, Y={2,5:0.0}\n", n, CogX[n], CogY[n]);
                }

            }
            else
            {
                Console.Write("\nError: Not enough memory.\n");
            }

            // Draw a cross at the center of gravity of each blob.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);
            AIL.MblobDraw(AIL.M_DEFAULT, AilBlobResult, AilGraphicList, AIL.M_DRAW_CENTER_OF_GRAVITY, AIL.M_INCLUDED_BLOBS, AIL.M_DEFAULT);

            // Reverse what is considered to be the background so that
            // holes are seen as being blobs.
            AIL.MblobControl(AilBlobContext, AIL.M_FOREGROUND_VALUE, AIL.M_ZERO);

            // Add a feature to distinguish between types of holes.Since area
            // has already been added to the context, and the processing 
            // mode has been changed, all blobs will be re-included and the area 
            // of holes will be calculated automatically.
            AIL.MblobControl(AilBlobContext, AIL.M_COMPACTNESS, AIL.M_ENABLE);

            // Calculate selected features for each blob.
            AIL.MblobCalculate(AilBlobContext, AilBinImage, AIL.M_NULL, AilBlobResult);

            // Exclude small holes and large (the area around objects) holes.
            AIL.MblobSelect(AilBlobResult, AIL.M_EXCLUDE, AIL.M_AREA, AIL.M_OUT_RANGE, MIN_BLOB_AREA, MAX_BLOB_AREA);

            // Get the number of blobs with holes.
            AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref BlobsWithHoles);

            // Exclude blobs whose holes are compact (i.e.nuts).
            AIL.MblobSelect(AilBlobResult, AIL.M_EXCLUDE, AIL.M_COMPACTNESS, AIL.M_LESS_OR_EQUAL, MIN_COMPACTNESS, AIL.M_NULL);

            // Get the number of blobs with holes that are NOT compact.
            AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref BlobsWithRoughHoles);

            // Print results.
            Console.Write("\nIdentified objects:\n");
            Console.Write("{0} bolts\n", TotalBlobs - BlobsWithHoles);
            Console.Write("{0} nuts\n", BlobsWithHoles - BlobsWithRoughHoles);
            Console.Write("{0} washers\n\n", BlobsWithRoughHoles);
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MgraFree(AilGraphicList);
            AIL.MblobFree(AilBlobResult);
            AIL.MblobFree(AilBlobContext);
            AIL.MbufFree(AilBinImage);
            AIL.MbufFree(AilImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
