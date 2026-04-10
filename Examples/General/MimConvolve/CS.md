---
title: "MimConvolve"
description: "This example performs a 3x3 convolution using a custom kernel."
ms.language: "C#"
---

# MimConvolve

> This example performs a 3x3 convolution using a custom kernel.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufAlloc2d`, `MbufControl`, `MbufFree`, `MbufPut`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MdispZoom`, `MimConvolve`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MimConvolve.cs
//
// Description: This program performs a 3x3 convolution using a custom kernel
//              and calculates the convolution time.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MImConvolve
{
    class Program
    {
        // Target image specifications.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "BaboonMono.mim";
        private const int ZOOM_VALUE = 2;

        // Kernel data definition.
        private const int KERNEL_WIDTH = 3;
        private const int KERNEL_HEIGHT = 3;
        private const int KERNEL_DEPTH = 8;
        private const int KERNEL_NORMALIZATION = 16;
        private static readonly byte[,] KernelData = new byte[KERNEL_HEIGHT, KERNEL_WIDTH] { { 1, 2, 1 }, { 2, 4, 2 }, { 1, 2, 1 } };

        // Timing loop iterations.
        private const int NB_LOOP = 100;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.
            AIL_ID AilDisplayImage = AIL.M_NULL;    // Image buffer identifier.
            AIL_ID AilImage = AIL.M_NULL;           // Image buffer identifier.
            AIL_ID AilKernel = AIL.M_NULL;          // Custom kernel identifier.
            int n = 0;
            double Time = 0.0;

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore source image into an automatically allocated image buffers.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilDisplayImage);

            // Zoom display to see the result of image processing better.
            AIL.MdispZoom(AilDisplay, ZOOM_VALUE, ZOOM_VALUE);

            // Display the image buffer.
            AIL.MdispSelect(AilDisplay, AilDisplayImage);

            // Pause to show the original image.
            Console.Write("\nIMAGE PROCESSING:\n");
            Console.Write("-----------------\n\n");
            Console.Write("This program performs a convolution on the displayed image.\n");
            Console.Write("It uses a custom smoothing kernel.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Allocate a kernel.
            AIL.MbufAlloc2d(AilSystem, KERNEL_WIDTH, KERNEL_HEIGHT, KERNEL_DEPTH + AIL.M_UNSIGNED, AIL.M_KERNEL, ref AilKernel);

            // Put the custom data in it.
            AIL.MbufPut(AilKernel, KernelData);

            // Set a normalization (divide) factor to have a kernel with
            // a sum equal to one.
            AIL.MbufControl(AilKernel, AIL.M_NORMALIZATION_FACTOR, KERNEL_NORMALIZATION);

            // Convolve the image using the kernel.
            AIL.MimConvolve(AilImage, AilDisplayImage, AilKernel);

            // Now time the convolution (MimConvolve()):
            // Overscan calculation is disabled and a destination image that
            // is not displayed is used to have the real convolution time. Also the 
            // function must be called once before the timing loop for more accurate 
            // time (dll load, ...).
            AIL.MbufControl(AilKernel, AIL.M_OVERSCAN, AIL.M_DISABLE);
            AIL.MimConvolve(AilDisplayImage, AilImage, AilKernel);
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);
            for (n = 0; n < NB_LOOP; n++)
            {
                AIL.MimConvolve(AilDisplayImage, AilImage, AilKernel);
            }
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref Time);

            // Pause to show the result.
            Console.Write("Convolve time: {0:0.000} ms.\n", Time * 1000 / NB_LOOP);
            Console.Write("Press any key to terminate.\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MbufFree(AilKernel);
            AIL.MbufFree(AilImage);
            AIL.MbufFree(AilDisplayImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);

        }
    }
}

```
