---
title: "MimSegment"
description: "This program uses the watershed and the edge detection functions to remove the background of an image with a non-linear lighting. Then, the watershed and the distance functions are used to separate the touching objects."
ms.language: "C#"
---

# MimSegment

> This program uses the watershed and the edge detection functions to remove the background of an image with a non-linear lighting. Then, the watershed and the distance functions are used to separate the touching objects.

**Language:** C#

**Functions used:** `MsysFree`, `MappFree`, `MbufFree`, `MbufGet2d`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimArith`, `MimClip`, `MimDistance`, `MimEdgeDetect`, `MimWatershed`, `MsysAlloc`, `MappAlloc`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImSegment.cs
//
// Description: This program uses the watershed and the edge detection functions 
//              to remove the background of an image with a non-linear lighting.
//              Then, the watershed and the distance functions are used to separate
//              the touching objects.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MImSegment
{
    class Program
    {
        // Source image specifications.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "pills.mim";

        // Minimal distance and gradient variations for the watershed calculation.
        private const int WATERSHED_MINIMAL_GRADIENT_VARIATION = 45;
        private const int WATERSHED_MINIMAL_DISTANCE_VARIATION = 2;

        // Position used to fetch the label of the background.
        private const int PIXEL_FETCH_POSITION_X = 2;
        private const int PIXEL_FETCH_POSITION_Y = 2;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;         // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;              // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;             // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;               // Image buffer identifier.
            AIL_ID AilImageWatershed = AIL.M_NULL;      // Image buffer identifier.
            int[] lFetchedValue = new int[1] { 0 };     // Label of the background.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore the source image into two automatically allocated
            // image buffers and select one of them to the display.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImageWatershed);
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Pause to show the original image.
            Console.Write("\nSEGMENTATION:\n");
            Console.Write("-------------\n\n");
            Console.Write("An edge detection followed by a watershed will be used to remove\n");
            Console.Write("the background.\nPress any key to continue.\n\n");
            Console.ReadKey(true);

            // Perform a edge detection on the original image.
            AIL.MimEdgeDetect(AilImageWatershed, AilImageWatershed, AIL.M_NULL, AIL.M_SOBEL, AIL.M_REGULAR_EDGE_DETECT, AIL.M_NULL);

            // Find the basins of the edge detected image that have a minimal gray scale
            // variation of WATERSHED_MINIMAL_GRADIENT_VARIATION.
            AIL.MimWatershed(AilImageWatershed, AIL.M_NULL, AilImageWatershed, WATERSHED_MINIMAL_GRADIENT_VARIATION, AIL.M_MINIMA_FILL + AIL.M_BASIN);

            // Fetch the label of the background, clip it to zero and clip the other labels to 
            // the maximum value of the buffer.
            AIL.MbufGet2d(AilImageWatershed, PIXEL_FETCH_POSITION_X, PIXEL_FETCH_POSITION_Y, 1, 1, lFetchedValue);
            AIL.MimClip(AilImageWatershed, AilImageWatershed, AIL.M_EQUAL, lFetchedValue[0], 0, 0, 0);
            AIL.MimClip(AilImageWatershed, AilImage, AIL.M_NOT_EQUAL, 0, 0, 0xFF, 0);

            // Pause to show the binarized image.
            Console.Write("A distance transformation followed by a watershed will be used \n");
            Console.Write("to separate the touching pills.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Perform a distance transformation on the binarized image.
            AIL.MimDistance(AilImage, AilImageWatershed, AIL.M_CHAMFER_3_4);

            // Find the watersheds of the image.
            AIL.MimWatershed(AilImageWatershed, AIL.M_NULL, AilImageWatershed, WATERSHED_MINIMAL_DISTANCE_VARIATION, AIL.M_STRAIGHT_WATERSHED + AIL.M_MAXIMA_FILL + AIL.M_SKIP_LAST_LEVEL + AIL.M_WATERSHED);

            // AND the watershed image with the binarized image to separate the touching pills.
            AIL.MimArith(AilImageWatershed, AilImage, AilImage, AIL.M_AND);

            // Pause to show the segmented image.
            Console.Write("Here are the segmented pills.\n");
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MbufFree(AilImageWatershed);
            AIL.MbufFree(AilImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
