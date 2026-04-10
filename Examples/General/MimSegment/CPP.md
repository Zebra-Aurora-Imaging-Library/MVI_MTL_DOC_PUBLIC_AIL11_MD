---
title: "MimSegment"
description: "This program uses the watershed and the edge detection functions to remove the background of an image with a non-linear lighting. Then, the watershed and the distance functions are used to separate the touching objects."
ms.language: "C++"
---

# MimSegment

> This program uses the watershed and the edge detection functions to remove the background of an image with a non-linear lighting. Then, the watershed and the distance functions are used to separate the touching objects.

**Language:** C++

**Functions used:** `MsysFree`, `MappFree`, `MbufFree`, `MbufGet2d`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimArith`, `MimClip`, `MimDistance`, `MimEdgeDetect`, `MimWatershed`, `MsysAlloc`, `MappAlloc`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImSegment.cpp
//
// Description: This program uses the watershed and the edge detection functions 
//              to remove the background of an image with a non-linear lighting.
//              Then, the watershed and the distance functions are used to separate
//              the touching objects.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Source image specifications.
#define IMAGE_FILE         M_IMAGE_PATH AIL_TEXT("pills.mim")

// Minimal distance and gradient variations for the watershed calculation.
#define WATERSHED_MINIMAL_GRADIENT_VARIATION       45
#define WATERSHED_MINIMAL_DISTANCE_VARIATION        2

// Position used to fetch the label of the background.
#define PIXEL_FETCH_POSITION_X                2
#define PIXEL_FETCH_POSITION_Y                2

//  Main function.
int MosMain(void)
{
   AIL_ID    AilApplication,    // Application identifier. 
             AilSystem,         // System identifier.      
             AilDisplay,        // Display identifier.     
             AilImage,          // Image buffer identifier.
             AilImageWatershed; // Image buffer identifier.
   AIL_INT32 lFetchedValue = 0; // Label of the background.

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem,
                             &AilDisplay, M_NULL, M_NULL);

   // Restore the source image into two automatically allocated
   // image buffers and select one of them to the display.
   MbufRestore(IMAGE_FILE, AilSystem, &AilImageWatershed);
   MbufRestore(IMAGE_FILE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Pause to show the original image.
   MosPrintf(AIL_TEXT("\nSEGMENTATION:\n"));
   MosPrintf(AIL_TEXT("-------------\n\n"));
   MosPrintf(AIL_TEXT("An edge detection followed by a watershed will be used to remove\n"));
   MosPrintf(AIL_TEXT("the background.\nPress any key to continue.\n\n"));
   MosGetch();

   // Perform a edge detection on the original image.
   MimEdgeDetect(AilImageWatershed, AilImageWatershed, M_NULL, M_SOBEL,
                 M_REGULAR_EDGE_DETECT, M_NULL);

   // Find the basins of the edge detected image that have a minimal gray scale
   // variation of WATERSHED_MINIMAL_GRADIENT_VARIATION.
   MimWatershed(AilImageWatershed, M_NULL, AilImageWatershed, 
                WATERSHED_MINIMAL_GRADIENT_VARIATION, M_MINIMA_FILL+M_BASIN);

   // Fetch the label of the background, clip it to zero and clip the other labels to 
   // the maximum value of the buffer.
   MbufGet2d(AilImageWatershed, PIXEL_FETCH_POSITION_X, PIXEL_FETCH_POSITION_Y, 
                                                        1, 1, &lFetchedValue);
   MimClip(AilImageWatershed, AilImageWatershed, M_EQUAL, lFetchedValue, 0, 0, 0);
   MimClip(AilImageWatershed, AilImage, M_NOT_EQUAL, 0, 0, 0xFF, 0);

   // Pause to show the binarized image.
   MosPrintf(AIL_TEXT("A distance transformation followed by a watershed will be used \n"));
   MosPrintf(AIL_TEXT("to separate the touching pills.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Perform a distance transformation on the binarized image.
   MimDistance(AilImage, AilImageWatershed, M_CHAMFER_3_4);

   // Find the watersheds of the image.
   MimWatershed(AilImageWatershed, M_NULL, AilImageWatershed,
                WATERSHED_MINIMAL_DISTANCE_VARIATION,
                M_STRAIGHT_WATERSHED+M_MAXIMA_FILL+M_SKIP_LAST_LEVEL+M_WATERSHED);
    
   // AND the watershed image with the binarized image to separate the touching pills.
   MimArith(AilImageWatershed, AilImage, AilImage, M_AND);

   // Pause to show the segmented image.
   MosPrintf(AIL_TEXT("Here are the segmented pills.\n"));
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   // Free all allocations.
   MbufFree(AilImageWatershed);
   MbufFree(AilImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}

```
