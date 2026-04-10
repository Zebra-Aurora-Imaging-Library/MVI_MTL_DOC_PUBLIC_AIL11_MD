---
title: "MbufColor"
description: "This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result."
ms.language: "C# Object"
---

# MbufColor

> This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result.

**Language:** C# Object

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufChild2d`, `MbufChildColor`, `MbufClear`, `MbufDiskInquire`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispSelect`, `MgraText`, `MimArith`, `MimConvert`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MbufColor.cs
//
// Description: This program demonstrates color buffer manipulations. It allocates
//              a displayable color image buffer, displays it, and loads a color
//              image into the left part. After that, color annotations are done
//              in each band of the color buffer. Finaly, to increase the image
//              luminance, the image is converted to Hue, Luminance and Saturation
//              (HLS), a certain offset is added to the luminance component and
//              the image is converted back to Red, Green, Blue (RGB) into the
//              right part to display the result.
//             
//              The example also demonstrates how to display multiple images
//              in a single display using child buffers.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
//////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using Zebra.AuroraImagingObjectLibrary;
using System.Linq;

namespace MBufColor
    {
    class MbufColorExample : IDisposable
        {
        public MbufColorExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _graContext = new Gra.Context(_system);
            _display = new Disp.Display(_system);
            }

        public void Run()
            {
            var fileInfo = _application.File.FileInfo(_imageFile);
            var sizeX = fileInfo.SizeX;
            var sizeY = fileInfo.SizeY;
            var sizeBand = fileInfo.SizeBand;

            // Allocate a color display buffer twice the size of the source image and display it.
            _imageDisp = new Buf.Image(_system, sizeBand, sizeX * 2, sizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDisp);

            _imageDisp.Clear(Color8.Black);
            _display.Select(_imageDisp);

            // Define 2 child buffers that maps to the left and right part of the display
            // buffer, to put the source and destination color images.
            //
            _leftSubImage = _imageDisp.Child2d(0, 0, sizeX, sizeY);
            _rightSubImage = _imageDisp.Child2d(sizeX, 0, sizeX, sizeY);

            // Load the color source image on the left.
            _leftSubImage.Load(_imageFile);

            // Define child buffers that map to the red, green and blue components
            // of the source image.
            //
            _redBandSubImage = _leftSubImage.ChildColor(Buf.ColorBand.Red);
            _greenBandSubImage = _leftSubImage.ChildColor(Buf.ColorBand.Green);
            _blueBandSubImage = _leftSubImage.ChildColor(Buf.ColorBand.Blue);

            // Write color text annotations to show access in each individual band of the image.
            //
            _graContext.Color = 0xFF;
            _graContext.DrawString(_redBandSubImage, sizeX / 16, sizeY / 8, " TOUCAN ");
            _graContext.Color = 0x90;
            _graContext.DrawString(_greenBandSubImage, sizeX / 16, sizeY / 8, " TOUCAN ");
            _graContext.Color = 0x00;
            _graContext.DrawString(_blueBandSubImage, sizeX / 16, sizeY / 8, " TOUCAN ");

            // Print a message.
            Console.WriteLine("\nCOLOR OPERATIONS:");
            Console.WriteLine("-----------------\n");
            Console.WriteLine("A color source image was loaded on the left and color text");
            Console.WriteLine("annotations were written in it.");
            Console.WriteLine("Press any key to continue.\n");
            Console.ReadKey(true);

            // Convert image to Hue, Luminance, Saturation color space (HSL).
            Im.Convert(_leftSubImage, _rightSubImage, Im.ConversionType.RgbToHsl);

            // Create a child buffer that maps to the luminance component.
            _lumSubImage = _rightSubImage.ChildColor(Buf.ColorBand.Luminance);

            // Add an offset to the luminance component.
            Im.Arith.Add(_lumSubImage, _imageLuminanceOffset, _lumSubImage, true);

            // Convert image back to Red, Green, Blue color space (RGB) for display.
            Im.Convert(_rightSubImage, _rightSubImage, Im.ConversionType.HslToRgb);

            // Print a message.
            Console.WriteLine("Luminance was increased using color image processing.");

            // Print a message.
            Console.WriteLine("Press any key to end.");
            Console.ReadKey(true);
            }

        public void Dispose()
            {
            _lumSubImage?.Free();
            _redBandSubImage?.Free();
            _greenBandSubImage?.Free();
            _blueBandSubImage?.Free();
            _rightSubImage?.Free();
            _leftSubImage?.Free();
            _display?.Free();
            _graContext?.Free();
            _imageDisp?.Free();
            _system?.Free();
            _application?.Free();
            }

        private readonly App.Application _application;
        private readonly Sys.System _system;
        private readonly Gra.Context _graContext;
        private readonly Disp.Display _display;

        private Buf.Image _imageDisp;
        private Buf.Image _lumSubImage;
        private Buf.Image _redBandSubImage;
        private Buf.Image _greenBandSubImage;
        private Buf.Image _blueBandSubImage;
        private Buf.Image _rightSubImage;
        private Buf.Image _leftSubImage;

        // Source image file specifications.
        private const string _imageFile = Paths.Images + "Bird.mim";

        // Luminance offset to add to the image.
        private const int _imageLuminanceOffset = 40;
        }

    class Program
        {
        static void Main(string[] args)
            {
            try
                {
                using (var example = new MbufColorExample())
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
