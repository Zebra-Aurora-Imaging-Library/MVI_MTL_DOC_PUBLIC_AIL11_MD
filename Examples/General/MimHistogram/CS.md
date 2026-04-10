---
title: "MimHistogram"
description: "This program loads an image of a tissue sample, calculates its intensity histogram and draws it."
ms.language: "C#"
---

# MimHistogram

> This program loads an image of a tissue sample, calculates its intensity histogram and draws it.

**Language:** C#

**Functions used:** `MsysFree`, `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraLines`, `MimAllocResult`, `MimFree`, `MimGetResult`, `MimHistogram`, `MsysAlloc`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImHistogram.cs
//
// Description: This program loads an image of a tissue sample, calculates its intensity
//              histogramand draws it.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MImHistogram
{
    class Program
    {
        // Target image file specifications.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "Cell.mbufi";

        // Number of possible pixel intensities.
        private const int HIST_NUM_INTENSITIES = 256;
        private const int HIST_SCALE_FACTOR = 8;
        private const int HIST_X_POSITION = 250;
        private const int HIST_Y_POSITION = 450;

        // Main function.
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;                   // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                        // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                       // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;                         // Image buffer identifier.
            AIL_ID AilOverlayImage = AIL.M_NULL;                  // Overlay buffer identifier.
            AIL_ID HistResult = AIL.M_NULL;                       // Histogram buffer identifier.
            AIL_INT[] HistValues = new AIL_INT[HIST_NUM_INTENSITIES];     // Histogram values.
            double[] XStart = new double[HIST_NUM_INTENSITIES];
            double[] YStart = new double[HIST_NUM_INTENSITIES];
            double[] XEnd = new double[HIST_NUM_INTENSITIES];
            double[] YEnd = new double[HIST_NUM_INTENSITIES];
            double AnnotationColor = AIL.M_COLOR_RED;

            // Allocate the default system and image buffer.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore source image into an automatically allocated image buffer.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);

            // Display the image buffer and prepare for overlay annotations.
            AIL.MdispSelect(AilDisplay, AilImage);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            // Allocate a histogram result buffer.
            AIL.MimAllocResult(AilSystem, HIST_NUM_INTENSITIES, AIL.M_HIST_LIST, ref HistResult);

            // Calculate the histogram.
            AIL.MimHistogram(AilImage, HistResult);

            // Get the results.
            AIL.MimGetResult(HistResult, AIL.M_VALUE, HistValues);

            // Draw the histogram in the overlay.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor);
            for (int i = 0; i < HIST_NUM_INTENSITIES; i++)
            {
                XStart[i] = i + HIST_X_POSITION + 1;
                YStart[i] = HIST_Y_POSITION;
                XEnd[i] = i + HIST_X_POSITION + 1;
                YEnd[i] = HIST_Y_POSITION - (HistValues[i] / HIST_SCALE_FACTOR);
            }
            AIL.MgraLines(AIL.M_DEFAULT, AilOverlayImage, HIST_NUM_INTENSITIES, XStart, YStart, XEnd, YEnd, AIL.M_DEFAULT);

            // Print a message.
            Console.Write("\nHISTOGRAM:\n");
            Console.Write("----------\n\n");
            Console.Write("The histogram of the image was calculated and drawn.\n");
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MimFree(HistResult);
            AIL.MbufFree(AilImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
