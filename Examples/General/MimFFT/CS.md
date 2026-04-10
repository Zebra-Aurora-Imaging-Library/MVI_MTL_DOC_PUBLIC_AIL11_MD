---
title: "MimFFT"
description: "This program uses the Fast Fourier Transform to filter an image."
ms.language: "C#"
---

# MimFFT

> This program uses the Fast Fourier Transform to filter an image.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufChild2d`, `MbufClear`, `MbufCopy`, `MbufFree`, `MbufLoad`, `MbufPut2d`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraArc`, `MimTransform`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImFFT.cs
//
// Description: This program uses the Fast Fourier Transform to filter an image.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MImFFT
{
    class Program
    {
        // Target image specifications.
        private const string NOISY_IMAGE = AIL.M_IMAGE_PATH + "noise.mim";
        private const int IMAGE_WIDTH = 256;
        private const int IMAGE_HEIGHT = 256;
        private const int X_NEGATIVE_FREQUENCY_POSITION = 63;
        private const int X_POSITIVE_FREQUENCY_POSITION = 191;
        private const int Y_FREQUENCY_POSITION = 127;
        private const int CIRCLE_WIDTH = 9;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;           // Image buffer identifier.
            AIL_ID AilOverlayImage = AIL.M_NULL;    // Overlay image buffer identifier.
            AIL_ID AilSubImage00 = AIL.M_NULL;      // Child buffer identifier.
            AIL_ID AilSubImage01 = AIL.M_NULL;      // Child buffer identifier.
            AIL_ID AilSubImage10 = AIL.M_NULL;      // Child buffer identifier.
            AIL_ID AilSubImage11 = AIL.M_NULL;      // Child buffer identifier.
            AIL_ID AilTransformReal = AIL.M_NULL;   // Real part of the transformed image.
            AIL_ID AilTransformIm = AIL.M_NULL;     // Imaginary part of the transformed image.

            float[] ZeroVal = new float[1];
            ZeroVal[0] = 0.0F;

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Allocate a display buffer and clear it.
            AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH * 2, IMAGE_HEIGHT * 2, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilImage);
            AIL.MbufClear(AilImage, 0L);

            // Display the image buffer and prepare for overlay annotations.
            AIL.MdispSelect(AilDisplay, AilImage);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            // Allocate child buffers in the 4 quadrants of the display image.
            AIL.MbufChild2d(AilImage, 0, 0, IMAGE_WIDTH, IMAGE_HEIGHT, ref AilSubImage00);
            AIL.MbufChild2d(AilImage, IMAGE_WIDTH, 0, IMAGE_WIDTH, IMAGE_HEIGHT, ref AilSubImage01);
            AIL.MbufChild2d(AilImage, 0, IMAGE_HEIGHT, IMAGE_WIDTH, IMAGE_HEIGHT, ref AilSubImage10);
            AIL.MbufChild2d(AilImage, IMAGE_WIDTH, IMAGE_HEIGHT, IMAGE_WIDTH, IMAGE_HEIGHT, ref AilSubImage11);

            // Allocate processing buffers.
            AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 32 + AIL.M_FLOAT, AIL.M_IMAGE + AIL.M_PROC, ref AilTransformReal);
            AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 32 + AIL.M_FLOAT, AIL.M_IMAGE + AIL.M_PROC, ref AilTransformIm);

            // Load a noisy image.
            AIL.MbufLoad(NOISY_IMAGE, AilSubImage00);

            // Print a message on the screen.
            Console.Write("\nFFT:\n");
            Console.Write("----\n\n");
            Console.Write("The frequency spectrum of a noisy image will be computed to remove the periodic noise.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // The image is Fourier transformed to obtain the magnitude of the
            // spectrum. This result will be used to design the filter.
            AIL.MimTransform(AilSubImage00, AIL.M_NULL, AilTransformReal, AilTransformIm, AIL.M_FFT, AIL.M_FORWARD + AIL.M_CENTER + AIL.M_MAGNITUDE + AIL.M_LOG_SCALE);
            AIL.MbufCopy(AilTransformReal, AilSubImage10);

            // Draw circles in the overlay around the points of interest.
            AIL.MbufCopy(AilTransformReal, AilSubImage11);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_YELLOW);
            AIL.MgraArc(AIL.M_DEFAULT, AilOverlayImage, X_NEGATIVE_FREQUENCY_POSITION, Y_FREQUENCY_POSITION + IMAGE_HEIGHT, CIRCLE_WIDTH, CIRCLE_WIDTH, 0, 360);
            AIL.MgraArc(AIL.M_DEFAULT, AilOverlayImage, X_POSITIVE_FREQUENCY_POSITION, Y_FREQUENCY_POSITION + IMAGE_HEIGHT, CIRCLE_WIDTH, CIRCLE_WIDTH, 0, 360);
            AIL.MgraArc(AIL.M_DEFAULT, AilOverlayImage, X_NEGATIVE_FREQUENCY_POSITION + IMAGE_WIDTH, Y_FREQUENCY_POSITION + IMAGE_HEIGHT, CIRCLE_WIDTH, CIRCLE_WIDTH, 0, 360);
            AIL.MgraArc(AIL.M_DEFAULT, AilOverlayImage, X_POSITIVE_FREQUENCY_POSITION + IMAGE_WIDTH, Y_FREQUENCY_POSITION + IMAGE_HEIGHT, CIRCLE_WIDTH, CIRCLE_WIDTH, 0, 360);

            // Put zero in the spectrum where the noise is located.
            AIL.MbufPut2d(AilSubImage11, X_NEGATIVE_FREQUENCY_POSITION, Y_FREQUENCY_POSITION, 1, 1, ZeroVal);
            AIL.MbufPut2d(AilSubImage11, X_POSITIVE_FREQUENCY_POSITION, Y_FREQUENCY_POSITION, 1, 1, ZeroVal);

            // Compute the Fast Fourier Transform of the image.
            AIL.MimTransform(AilSubImage00, AIL.M_NULL, AilTransformReal, AilTransformIm, AIL.M_FFT, AIL.M_FORWARD + AIL.M_CENTER);

            // Filter the image in the frequency domain.
            AIL.MbufPut2d(AilTransformReal, X_NEGATIVE_FREQUENCY_POSITION, Y_FREQUENCY_POSITION, 1, 1, ZeroVal);
            AIL.MbufPut2d(AilTransformReal, X_POSITIVE_FREQUENCY_POSITION, Y_FREQUENCY_POSITION, 1, 1, ZeroVal);
            AIL.MbufPut2d(AilTransformIm, X_NEGATIVE_FREQUENCY_POSITION, Y_FREQUENCY_POSITION, 1, 1, ZeroVal);
            AIL.MbufPut2d(AilTransformIm, X_POSITIVE_FREQUENCY_POSITION, Y_FREQUENCY_POSITION, 1, 1, ZeroVal);

            // Recover the image in the spatial domain.
            AIL.MimTransform(AilTransformReal, AilTransformIm, AilSubImage01, AIL.M_NULL, AIL.M_FFT, AIL.M_REVERSE + AIL.M_CENTER + AIL.M_SATURATION);

            // Print a message.
            Console.Write("The frequency components of the noise are located in the center of the circles.\n");
            Console.Write("The noise was removed by setting these frequency components to zero.\n");
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free buffers.
            AIL.MbufFree(AilSubImage00);
            AIL.MbufFree(AilSubImage01);
            AIL.MbufFree(AilSubImage10);
            AIL.MbufFree(AilSubImage11);
            AIL.MbufFree(AilImage);
            AIL.MbufFree(AilTransformReal);
            AIL.MbufFree(AilTransformIm);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);

        }
    }
}

```
