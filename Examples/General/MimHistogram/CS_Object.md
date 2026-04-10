---
title: "MimHistogram"
description: "This program loads an image of a tissue sample, calculates its intensity histogram and draws it."
ms.language: "C# Object"
---

# MimHistogram

> This program loads an image of a tissue sample, calculates its intensity histogram and draws it.

**Language:** C# Object

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
using Zebra.AuroraImagingObjectLibrary;
using static Zebra.AuroraImagingObjectLibrary.Im;

namespace MimHistogram
    {
    class MimHistogramExample : IDisposable
        {
        public MimHistogramExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _graContext = new Gra.Context(_system);
            _display = new Disp.Display(_system);
            _imageDisp = new Buf.Image();
            }
        public void Run()
            {
            var xStart = new double[_histNumIntensities];
            var yStart = new double[_histNumIntensities];
            var xEnd = new double[_histNumIntensities];
            var yEnd = new double[_histNumIntensities];

            // Restore source image into an automatically allocated image buffer.
            _imageDisp.Restore(_imageFile, _system);

            // Display the image buffer and prepare for overlay annotations.
            _display.Select(_imageDisp);
            var overlay = _display.Overlay;

            overlay.Enabled = Disp.Overlay.Enable;

            // Allocate a histogram result buffer.
            _histogramList = new Im.HistogramList(_system, _histNumIntensities);

            // Calculate the histogram.
            Im.Histogram(_imageDisp, _histogramList);

            // Get the results.
            var histValues = _histogramList.Values;

            // Draw the histogram in the overlay.
            _graContext.Color = Color8.Red;
            for (int i = 0; i < _histNumIntensities; i++)
                {
                xStart[i] = i + _histPosX + 1;
                yStart[i] = _histPosY;
                xEnd[i] = i + _histPosX + 1;
                yEnd[i] = _histPosY - (histValues[i] / _histScaleFactor);
                }
            _graContext.DrawLineList(overlay.Image, _histNumIntensities, xStart, yStart, xEnd, yEnd);

            // Print a message.
            Console.Write("\nHISTOGRAM:\n");
            Console.Write("----------\n\n");
            Console.Write("The histogram of the image was calculated and drawn.\n");
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);
            }
        public void Dispose()
            {
            _histogramList?.Free();
            _display?.Free();
            _graContext?.Free();
            _imageDisp?.Free();
            _system?.Free();
            _application?.Free();
            }

        private readonly App.Application _application;
        private readonly Sys.System _system;
        private readonly Buf.Image _imageDisp;
        private readonly Gra.Context _graContext;
        private readonly Disp.Display _display;
        private Im.HistogramList _histogramList;

        // Number of possible pixel intensities.
        private const int _histNumIntensities = 256;
        private const int _histScaleFactor = 8;
        private const int _histPosX = 250;
        private const int _histPosY = 450;

        // Target image file specifications.
        private const string _imageFile = Paths.Images + "Cell.mbufi";
        };

    class Program
        {
        static void Main(string[] args)
            {
            try
                {
                using (var example = new MimHistogramExample())
                    {
                    example.Run();
                    }
                }
            catch (AIOLException exception)
                {
                Console.WriteLine("Encountered an exception during the example.");
                Console.WriteLine(exception.Message);
                Console.WriteLine("Press any key to exit.");
                Console.ReadKey();
                }
            }
        }
    }

```
