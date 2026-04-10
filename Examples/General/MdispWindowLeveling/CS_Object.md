---
title: "MdispWindowLeveling"
description: "This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling."
ms.language: "C# Object"
---

# MdispWindowLeveling

> This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling.

**Language:** C# Object

**Functions used:** `MbufControl`, `MappAlloc`, `MappFree`, `MbufAlloc1d`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispLut`, `MdispSelect`, `MdispSelectWindow`, `MgenLutRamp`, `MgraLine`, `MgraText`, `MimAllocResult`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdispWindowLeveling.cs
//
// Description: This program shows how to display a 10-bit monochrome Medical image
//              and applies a LUT to do interactive Window Leveling.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using Zebra.AuroraImagingObjectLibrary;
using static Zebra.AuroraImagingObjectLibrary.Im;

namespace MdispWindowLeveling
    {
    class MdispWindowLevelingExample : IDisposable
        {
        public MdispWindowLevelingExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _display = new Disp.Display(_system);
            _graphicContext = new Gra.Context(_system);
            }
        public void Run()
            {
            // Restore target image from disk.
            _imageDisp = new Buf.Image(_imageFile, _system);

            // Dynamically calculates the maximum value of the image using processing.
            _extremeList = new Im.ExtremeList(_system, 1);
            Im.FindExtreme(_imageDisp, _extremeList, Im.FindExtremeExtremeTypes.MaxValue);
            var imageMaxValue = _extremeList.Values[0];

            // Set the maximum value of the image to indicate to Aurora Imaging Library how to initialize
            // the default display LUT.
            _imageDisp.Max = imageMaxValue;

            // Display the image (to specify a user-defined window, use AIL.MdispSelectWindow()).
            _display.Select(_imageDisp);

            // Determine the maximum displayable value of the current display.
            var displaySizeBit = _display.SizeBit;
            var displayMaxValue = (1 << (int)displaySizeBit) - 1;

            // Print key information.
            Console.WriteLine("\nINTERACTIVE WINDOW LEVELING:");
            Console.WriteLine("----------------------------\n");

            Console.WriteLine("Image name : {0}", _imageName);
            Console.WriteLine("Image size : {0} x {1}", _imageDisp.SizeX, _imageDisp.SizeY);
            Console.WriteLine("Image max  : {0,4}", imageMaxValue);
            Console.WriteLine("Display max: {0,4}\n", displayMaxValue);

            // Allocate a LUT buffer according to the image maximum value and display pixel depth.
            _lut = new Buf.Lut(_system, _imageDisp.SizeBand, imageMaxValue + 1, 1, Buf.AllocType.Unsigned8);

            // Generate a LUT with a full range ramp and set its maximum value.
            Gen.LutRamp(_lut, 0, 0, imageMaxValue, displayMaxValue);
            _lut.Max = displayMaxValue;

            // Set the display LUT.
            _display.Lut = _lut;

            // Interactive Window Leveling using keyboard.
            Console.WriteLine("Keys assignment:\n");
            Console.WriteLine("Arrow keys :    Left=move Left, Right=move Right, Down=Narrower, Up=Wider.");
            Console.WriteLine("Intensity keys: L=Lower,  U=Upper,  R=Reset.");
            Console.WriteLine("Press enter to end.\n");

            // Modify LUT shape according to the arrow keys and update it.
            long start = 0;
            var end = imageMaxValue;
            var inflectionLevel = displayMaxValue;
            var step = (imageMaxValue + 1) / 128;
            step = Math.Max(step, 4);

            var ch = new ConsoleKeyInfo();
            while (ch.Key != ConsoleKey.Enter)
                {
                switch (ch.Key)
                    {
                    // Left arrow: Move region left.
                    case ConsoleKey.LeftArrow:
                            { start -= step; end -= step; break; }

                    // Right arrow: Move region right.
                    case ConsoleKey.RightArrow:
                            { start += step; end += step; break; }

                    // Down arrow: Narrow region.
                    case ConsoleKey.DownArrow:
                            { start += step; end -= step; break; }

                    // Up arrow: Widen region.
                    case ConsoleKey.UpArrow:
                            { start -= step; end += step; break; }

                    // L key: Lower inflexion point.
                    case ConsoleKey.L:
                            { inflectionLevel--; break; }

                    // U key: Upper inflexion point.
                    case ConsoleKey.U:
                            { inflectionLevel++; break; }

                    // R key: Reset the LUT to full image range.
                    case ConsoleKey.R:
                            { start = 0; end = imageMaxValue; inflectionLevel = displayMaxValue; break; }
                    }

                // Saturate.
                end = Math.Min(end, imageMaxValue);
                start = Math.Min(start, end);
                end = Math.Max(end, start);
                start = Math.Max(start, 0);
                end = Math.Max(end, 0);
                inflectionLevel = Math.Max(inflectionLevel, 0);
                inflectionLevel = Math.Min(inflectionLevel, displayMaxValue);
                Console.Write("Inflection points: Low=({0},0), High=({1},{2}).   \r", start, end, inflectionLevel);

                // Generate a LUT with 3 slopes and saturated at both ends.
                Gen.LutRamp(_lut, 0, 0, start, 0);
                Gen.LutRamp(_lut, start, 0, end, (double)inflectionLevel);
                Gen.LutRamp(_lut, end, (double)inflectionLevel, imageMaxValue, (double)displayMaxValue);

                // Update the display LUT.
                _display.Lut = _lut;

                // Draw the current LUT's shape in the image.
                // Note: This simple annotation method requires
                //       significant update and CPU time.
                //
                if (_originalImage == null)
                    {
                    _originalImage = new Buf.Image(_imageFile, _system);
                    }
                DrawLutShape(start, end, inflectionLevel, imageMaxValue, displayMaxValue);

                // If its an arrow key, get the second code.
                ch = Console.ReadKey(true);
                }
            Console.WriteLine("\n");
            }

        // Function to draw the current LUT's shape in the image.
        //
        //   Note: This simple annotation method requires significant update
        //   and CPU time since it repaints the entire image every time.
        //
        void DrawLutShape(long start, long end, int inflexionIntensity, long imageMaxValue, int displayMaxValue)
            {
            var imageSizeX = _imageDisp.SizeX;
            var imageSizeY = _imageDisp.SizeY;

            // Calculate the drawing parameters.
            var xStep = imageSizeX / (double)imageMaxValue;
            var xStart = start * xStep;
            var xEnd = end * xStep;
            var yStep = imageSizeY / 4.0 / displayMaxValue;
            var yMin = (double)imageSizeY - 2;
            var yInf = yMin - inflexionIntensity * yStep;
            var yMax = yMin - displayMaxValue * yStep;

            // To increase speed, disable display update until all annotations are done.
            _display.Update.Display = Disp.Update.Disable;

            // Restore the original image.
            Buf.Copy(_originalImage, _imageDisp);

            // Draw axis max and min values.
            _graphicContext.Color = imageMaxValue;
            _graphicContext.DrawString(_imageDisp, 4, yMin - 22, "0");
            _graphicContext.DrawString(_imageDisp, 4, yMax - 16, string.Format("{0}", displayMaxValue));
            _graphicContext.DrawString(_imageDisp, imageSizeX - 38, yMin - 22, string.Format("{0}", imageMaxValue));

            // Draw LUT Shape (X axis is display values and Y is image values).
            _graphicContext.DrawLine(_imageDisp, 0, yMin, xStart, yMin);
            _graphicContext.DrawLine(_imageDisp, xStart, yMin, xEnd, yInf);
            _graphicContext.DrawLine(_imageDisp, xEnd, yInf, imageSizeX - 1, yMax);

            // Enable display update to show the result.
            _display.Update.Display = Disp.Update.Enable;
            }

        public void Dispose()
            {
            _extremeList?.Free();
            _lut?.Free();
            _imageDisp?.Free();
            _originalImage?.Free();
            _graphicContext?.Free();
            _display?.Free();
            _system?.Free();
            _application?.Free();
            }

        private readonly App.Application _application;
        private readonly Sys.System _system;
        private readonly Disp.Display _display;
        private readonly Gra.Context _graphicContext;

        private Buf.Image _imageDisp;
        private Buf.Image _originalImage;
        private Buf.Lut _lut;
        Im.ExtremeList _extremeList;

        // Image file to load.
        private const string _imageName = "ArmsMono10bit.mim";
        private const string _imageFile = Paths.Images + _imageName;
        };

    class Program
        {
        static void Main(string[] args)
            {
            try
                {
                using (var example = new MdispWindowLevelingExample())
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
