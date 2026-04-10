---
title: "MimFFT"
description: "This program uses the Fast Fourier Transform to filter an image."
ms.language: "C++"
---

# MimFFT

> This program uses the Fast Fourier Transform to filter an image.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufChild2d`, `MbufClear`, `MbufCopy`, `MbufFree`, `MbufLoad`, `MbufPut2d`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraArc`, `MimTransform`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImFFT.cpp
//
// Description: This program uses the Fast Fourier Transform to filter an image.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>
#include <math.h>

// Target image specifications.
#define NOISY_IMAGE                    M_IMAGE_PATH AIL_TEXT("noise.mim")
#define IMAGE_WIDTH                    256
#define IMAGE_HEIGHT                   256
#define X_NEGATIVE_FREQUENCY_POSITION   63
#define X_POSITIVE_FREQUENCY_POSITION  191
#define Y_FREQUENCY_POSITION           127
#define CIRCLE_WIDTH                     9

int MosMain(void)
{
   AIL_ID AilApplication,   // Application identifier.                  
          AilSystem,        // System identifier.                       
          AilDisplay,       // Display identifier.                      
          AilImage,         // Image buffer identifier.                 
          AilOverlayImage,  // Overlay image buffer identifier.         
          AilSubImage00,    // Child buffer identifier.                 
          AilSubImage01,    // Child buffer identifier.                 
          AilSubImage10,    // Child buffer identifier.                 
          AilSubImage11,    // Child buffer identifier.                 
          AilTransformReal, // Real part of the transformed image.      
          AilTransformIm;   // Imaginary part of the transformed image. 

   float  ZeroVal = 0.0;

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay,M_NULL, M_NULL);

   // Allocate a display buffer and clear it.
   MbufAlloc2d(AilSystem, IMAGE_WIDTH*2, IMAGE_HEIGHT*2,
               8+M_UNSIGNED, M_IMAGE+M_PROC+M_DISP, &AilImage);
   MbufClear(AilImage, 0L);

   // Display the image buffer and prepare for overlay annotations.
   MdispSelect(AilDisplay, AilImage);
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);

   // Allocate child buffers in the 4 quadrants of the display image.
   MbufChild2d(AilImage, 0L, 0L, IMAGE_WIDTH, IMAGE_HEIGHT, &AilSubImage00);
   MbufChild2d(AilImage, IMAGE_WIDTH, 0L,IMAGE_WIDTH, IMAGE_HEIGHT, &AilSubImage01);
   MbufChild2d(AilImage, 0L, IMAGE_HEIGHT, IMAGE_WIDTH, IMAGE_HEIGHT, &AilSubImage10);
   MbufChild2d(AilImage, IMAGE_WIDTH, IMAGE_HEIGHT, IMAGE_WIDTH, IMAGE_HEIGHT, 
                                                                 &AilSubImage11);

   // Allocate processing buffers.
   MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 32+M_FLOAT, 
                                          M_IMAGE+M_PROC, &AilTransformReal);
   MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 32+M_FLOAT, 
                                          M_IMAGE+M_PROC, &AilTransformIm);

   // Load a noisy image.
   MbufLoad(NOISY_IMAGE, AilSubImage00);

   // Print a message on the screen.
   MosPrintf(AIL_TEXT("\nFFT:\n"));
   MosPrintf(AIL_TEXT("----\n\n"));
   MosPrintf(AIL_TEXT("The frequency spectrum of a noisy image will be computed ")
                                       AIL_TEXT("to remove the periodic noise.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // The image is Fourier transformed to obtain the magnitude of the
   // spectrum. This result will be used to design the filter.
   MimTransform(AilSubImage00, M_NULL, AilTransformReal, AilTransformIm,
                M_FFT, M_FORWARD+M_CENTER+M_MAGNITUDE+M_LOG_SCALE);
   MbufCopy(AilTransformReal, AilSubImage10);

   // Draw circles in the overlay around the points of interest.
   MbufCopy(AilTransformReal, AilSubImage11);
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_YELLOW);
   MgraArc(M_DEFAULT, AilOverlayImage, X_NEGATIVE_FREQUENCY_POSITION,
           Y_FREQUENCY_POSITION+IMAGE_HEIGHT, CIRCLE_WIDTH, CIRCLE_WIDTH, 0, 360);
   MgraArc(M_DEFAULT, AilOverlayImage, X_POSITIVE_FREQUENCY_POSITION,
           Y_FREQUENCY_POSITION+IMAGE_HEIGHT, CIRCLE_WIDTH, CIRCLE_WIDTH, 0, 360);
   MgraArc(M_DEFAULT, AilOverlayImage, X_NEGATIVE_FREQUENCY_POSITION+IMAGE_WIDTH,
           Y_FREQUENCY_POSITION+IMAGE_HEIGHT, CIRCLE_WIDTH, CIRCLE_WIDTH, 0, 360);
   MgraArc(M_DEFAULT, AilOverlayImage, X_POSITIVE_FREQUENCY_POSITION+IMAGE_WIDTH,
           Y_FREQUENCY_POSITION+IMAGE_HEIGHT, CIRCLE_WIDTH, CIRCLE_WIDTH, 0, 360);

   // Put zero in the spectrum where the noise is located.
   MbufPut2d(AilSubImage11,   X_NEGATIVE_FREQUENCY_POSITION,
                              Y_FREQUENCY_POSITION, 1, 1, &ZeroVal);
   MbufPut2d(AilSubImage11,   X_POSITIVE_FREQUENCY_POSITION,
                              Y_FREQUENCY_POSITION, 1, 1, &ZeroVal);

   // Compute the Fast Fourier Transform of the image.
   MimTransform(AilSubImage00, M_NULL, AilTransformReal,
                AilTransformIm, M_FFT, M_FORWARD+M_CENTER);

   // Filter the image in the frequency domain.
   MbufPut2d(AilTransformReal, X_NEGATIVE_FREQUENCY_POSITION,
                               Y_FREQUENCY_POSITION, 1, 1, &ZeroVal);
   MbufPut2d(AilTransformReal, X_POSITIVE_FREQUENCY_POSITION,
                               Y_FREQUENCY_POSITION, 1, 1, &ZeroVal);
   MbufPut2d(AilTransformIm, X_NEGATIVE_FREQUENCY_POSITION,
                             Y_FREQUENCY_POSITION, 1, 1, &ZeroVal);
   MbufPut2d(AilTransformIm, X_POSITIVE_FREQUENCY_POSITION,
                             Y_FREQUENCY_POSITION, 1, 1, &ZeroVal);

   // Recover the image in the spatial domain.
   MimTransform(AilTransformReal, AilTransformIm,
                AilSubImage01, M_NULL, M_FFT, M_REVERSE+M_CENTER+M_SATURATION);

   // Print a message.
   MosPrintf(AIL_TEXT("The frequency components of the noise are located ")
                                AIL_TEXT("in the center of the circles.\n"));
   MosPrintf(AIL_TEXT("The noise was removed by setting these frequency ")
                                         AIL_TEXT("components to zero.\n"));
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   // Free buffers.
   MbufFree(AilSubImage00);
   MbufFree(AilSubImage01);
   MbufFree(AilSubImage10);
   MbufFree(AilSubImage11);
   MbufFree(AilImage);
   MbufFree(AilTransformReal);
   MbufFree(AilTransformIm);

   // Free defaults.
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}

```
