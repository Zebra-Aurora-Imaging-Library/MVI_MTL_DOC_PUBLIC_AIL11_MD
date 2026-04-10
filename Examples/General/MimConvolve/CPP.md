---
title: "MimConvolve"
description: "This example performs a 3x3 convolution using a custom kernel."
ms.language: "C++"
---

# MimConvolve

> This example performs a 3x3 convolution using a custom kernel.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufAlloc2d`, `MbufControl`, `MbufFree`, `MbufPut`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MdispZoom`, `MimConvolve`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MimConvolve.cpp
//
// Description: This program performs a 3x3 convolution using a custom kernel
//              and calculates the convolution time.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Target image specifications.
#define IMAGE_FILE       M_IMAGE_PATH AIL_TEXT("BaboonMono.mim")
#define ZOOM_VALUE       2

// Kernel data definition.
#define KERNEL_WIDTH          3L
#define KERNEL_HEIGHT         3L
#define KERNEL_DEPTH          8L
#define KERNEL_NORMALIZATION  16L
unsigned char  KernelData[KERNEL_HEIGHT][KERNEL_WIDTH] =
               { {1, 2, 1},
                 {2, 4, 2},
                 {1, 2, 1}
               };

// Timing loop iterations.
#define NB_LOOP 100

int MosMain(void)
{
   AIL_ID AilApplication,  // Application identifier.  
          AilSystem,       // System identifier.       
          AilDisplay,      // Display identifier.      
          AilDisplayImage, // Image buffer identifier. 
          AilImage,        // Image buffer identifier. 
          AilKernel;       // Custom kernel identifier.
   long   n;
   AIL_DOUBLE Time;   

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem,
                    &AilDisplay, M_NULL, M_NULL);

   // Restore source image into an automatically allocated image buffers.
   MbufRestore(IMAGE_FILE, AilSystem, &AilImage);
   MbufRestore(IMAGE_FILE, AilSystem, &AilDisplayImage);

   // Zoom display to see the result of image processing better.
   MdispZoom(AilDisplay, ZOOM_VALUE, ZOOM_VALUE);

   // Display the image buffer.
   MdispSelect(AilDisplay, AilDisplayImage);
   
   // Pause to show the original image.
   MosPrintf(AIL_TEXT("\nIMAGE PROCESSING:\n"));
   MosPrintf(AIL_TEXT("-----------------\n\n"));
   MosPrintf(AIL_TEXT("This program performs a convolution on the displayed image.\n"));
   MosPrintf(AIL_TEXT("It uses a custom smoothing kernel.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate a kernel.
   MbufAlloc2d(AilSystem, KERNEL_WIDTH, KERNEL_HEIGHT,
               KERNEL_DEPTH+M_UNSIGNED, M_KERNEL, &AilKernel);

   // Put the custom data in it.
   MbufPut(AilKernel,KernelData);

   // Set a normalization (divide) factor to have a kernel with
   // a sum equal to one.
   MbufControl(AilKernel,M_NORMALIZATION_FACTOR,KERNEL_NORMALIZATION);

   // Convolve the image using the kernel.
   MimConvolve(AilImage, AilDisplayImage, AilKernel);

   // Now time the convolution (MimConvolve()):
   // Overscan calculation is disabled and a destination image that
   // is not displayed is used to have the real convolution time. Also the 
   // function must be called once before the timing loop for more accurate 
   // time (dll load, ...).
   MbufControl(AilKernel, M_OVERSCAN, M_DISABLE);
   MimConvolve(AilDisplayImage, AilImage, AilKernel);  
   MappTimer(M_DEFAULT, M_TIMER_RESET+M_SYNCHRONOUS, M_NULL);
   for (n= 0; n < NB_LOOP; n++)
         MimConvolve(AilDisplayImage, AilImage, AilKernel);  
   MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &Time);

   // Pause to show the result.
   MosPrintf(AIL_TEXT("Convolve time: %.3f ms.\n"), Time*1000/NB_LOOP);
   MosPrintf(AIL_TEXT("Press any key to terminate.\n"));
   MosGetch();

   // Free all allocations.
   MbufFree(AilKernel);
   MbufFree(AilImage);
   MbufFree(AilDisplayImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);
   
   return 0;
}

```
