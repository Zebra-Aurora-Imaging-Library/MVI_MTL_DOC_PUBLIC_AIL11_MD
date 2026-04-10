---
title: "MimHistogram"
description: "This program loads an image of a tissue sample, calculates its intensity histogram and draws it."
ms.language: "C++"
---

# MimHistogram

> This program loads an image of a tissue sample, calculates its intensity histogram and draws it.

**Language:** C++

**Functions used:** `MsysFree`, `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraLines`, `MimAllocResult`, `MimFree`, `MimGetResult`, `MimHistogram`, `MsysAlloc`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImHistogram.cpp
//
// Description: This program loads an image of a tissue sample, calculates its intensity
//              histogramand draws it.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Target image file specifications. 
#define IMAGE_FILE  M_IMAGE_PATH AIL_TEXT("Cell.mbufi")

// Number of possible pixel intensities. 
#define HIST_NUM_INTENSITIES  256
#define HIST_SCALE_FACTOR     8
#define HIST_X_POSITION       250
#define HIST_Y_POSITION       450


/* Main function. */
int MosMain(void)
   { 
   AIL_ID     AilApplication,                   // Application identifier       
              AilSystem,                        // System identifier.           
              AilDisplay,                       // Display identifier.          
              AilImage,                         // Image buffer identifier.     
              AilOverlayImage,                  // Overlay buffer identifier.   
              HistResult;                       // Histogram buffer identifier. 
   AIL_INT    HistValues[HIST_NUM_INTENSITIES]; // Histogram values.            
   AIL_INT    XStart[HIST_NUM_INTENSITIES], YStart[HIST_NUM_INTENSITIES],
              XEnd[HIST_NUM_INTENSITIES],   YEnd[HIST_NUM_INTENSITIES];
   AIL_DOUBLE AnnotationColor = M_COLOR_RED;
   AIL_INT    i; 

   // Allocate the default system and image buffer. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);
 
   // Restore source image into an automatically allocated image buffer. 
   MbufRestore(IMAGE_FILE, AilSystem, &AilImage);
  
   // Display the image buffer and prepare for overlay annotations. 
   MdispSelect(AilDisplay, AilImage);
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);
   
   // Allocate a histogram result buffer. 
   MimAllocResult(AilSystem, HIST_NUM_INTENSITIES, M_HIST_LIST, &HistResult);
 
   // Calculate the histogram. 
   MimHistogram(AilImage, HistResult);
 
   // Get the results. 
   MimGetResult(HistResult, M_VALUE, HistValues);

   // Draw the histogram in the overlay. 
   MgraControl(M_DEFAULT, M_COLOR, AnnotationColor);
   for(i=0; i<HIST_NUM_INTENSITIES; i++)
      {
      XStart[i] = i+HIST_X_POSITION+1,
      YStart[i] = HIST_Y_POSITION;
      XEnd[i]   = i+HIST_X_POSITION+1, 
      YEnd[i] = HIST_Y_POSITION-(HistValues[i]/HIST_SCALE_FACTOR);
      }
   MgraLines(M_DEFAULT, AilOverlayImage, HIST_NUM_INTENSITIES, 
                        XStart, YStart, XEnd, YEnd, M_DEFAULT);

   // Print a message. 
   MosPrintf(AIL_TEXT("\nHISTOGRAM:\n"));
   MosPrintf(AIL_TEXT("----------\n\n"));
   MosPrintf(AIL_TEXT("The histogram of the image was calculated and drawn.\n"));
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();
 
   // Free all allocations. 
   MimFree(HistResult);
   MbufFree(AilImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}


```
