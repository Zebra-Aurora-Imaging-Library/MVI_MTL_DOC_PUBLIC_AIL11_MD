---
title: "MimProcessing"
description: "This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample."
ms.language: "C# Object"
---

# MimProcessing

> This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample.

**Language:** C# Object

**Functions used:** `MappAlloc`, `MappFree`, `MappInquire`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispLut`, `MdispSelect`, `MimAllocResult`, `MimArith`, `MimBinarize`, `MimClose`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MimLabel`, `MimOpen`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Image Processing, What's New, AIL 10.0 SP6, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImProcessing.cs
//
// Description: This program show the usage of image processing. Under Aurora Imaging Library Lite, 
//              it binarizes two images to isolate specific zones.
//              Under Aurora Imaging Library full, it also uses different image processing primitives 
//              to determine the number of cell nuclei that are larger than a 
//              certain size and show them in pseudo-color.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using Zebra.AuroraImagingObjectLibrary;

namespace MimProcessing
    {
    class MimProcessingExample : IDisposable
        {
        public MimProcessingExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _graContext = new Gra.Context(_system);
            _display = new Disp.Display(_system);
            }
        public void Run()
            {
            // Show header.
            Console.WriteLine("\nIMAGE PROCESSING:");
            Console.WriteLine("-----------------\n");
            Console.WriteLine("This program shows two image processing examples.");

            // Example about extracting particles in an image.
            ExtractParticlesExample();

            // Example about isolating objects from the background in an image.
            ExtractForegroundExample();
            }

        public void ExtractParticlesExample()
            {
            Buf.Image imageDisp = null;
            Im.ExtremeList extremeList = null;

            try
                {
                // Restore source image and display it.
                imageDisp = new Buf.Image(_imageFile, _system);
                _display.Select(imageDisp);

                // Pause to show the original image.
                Console.WriteLine("\n1) Particles extraction:");
                Console.WriteLine("-----------------\n");
                Console.WriteLine("This first example extracts the dark particles in an image.");
                Console.WriteLine("Press any key to continue.\n");
                Console.ReadKey(true);

                // Binarize the image with an automatically calculated threshold so that 
                // particles are represented in white and the background removed.
                Im.Binarize(imageDisp, imageDisp, Im.BinarizeConditionAndThreshModes.LessOrEqual | Im.BinarizeConditionAndThreshModes.Bimodal, 0, 0);

                // Print a message for the extracted particles.
                Console.WriteLine("These particles were extracted from the original image.");

                // if Aurora Imaging Library IM module is available, count and label the larger particles.
                var licenseModules = _application.LicenseModules;

                if ((licenseModules & App.LicenseModules.Im) != 0)
                    {
                    // Pause to show the extracted particles.
                    Console.WriteLine("Press any key to continue.\n");
                    Console.ReadKey(true);

                    // Close small holes.
                    Im.Close(imageDisp, imageDisp, _imageSmallParticleRadius, Im.CloseProcMode.Binary);

                    // Remove small particles.
                    Im.Open(imageDisp, imageDisp, _imageSmallParticleRadius, Im.OpenProcMode.Binary);

                    // Label the image.
                    Im.Label(imageDisp, imageDisp, Im.LabelProcModes.M8Connected | Im.LabelProcModes.Grayscale);

                    // The largest label value corresponds to the number of particles in the image.
                    extremeList = new Im.ExtremeList(_system, 1);
                    Im.FindExtreme(imageDisp, extremeList, Im.FindExtremeExtremeTypes.MaxValue);
                    var maxLabelNumber = extremeList.Values[0];

                    // Multiply the labeling result to augment the gray level of the particles.
                    Im.Arith.Multiply(imageDisp, (int)(256 / (double)maxLabelNumber), imageDisp, false);

                    // Display the resulting particles in pseudo-color.
                    _display.Lut = Buf.Lut.PseudoLut;

                    // Print results.
                    Console.WriteLine($"There were {maxLabelNumber} large particles in the original image.");
                    }

                // Pause to show the result.
                Console.WriteLine("Press any key to continue.\n");
                Console.ReadKey(true);
                }
            finally
                {
                // Reset AilDisplay to M_DEFAULT.
                _display.Lut = Buf.Lut.DefaultLut;

                // Free all allocations.
                imageDisp?.Free();
                extremeList?.Free();
                }
            }

        public void ExtractForegroundExample()
            {
            Buf.Image imageDisp = null;

            try
                {
                // Restore source image and display it.
                imageDisp = new Buf.Image(_imageCup, _system);
                _display.Select(imageDisp);

                // Pause to show the original image.
                Console.WriteLine("\n2) Background removal:");
                Console.WriteLine("-----------------\n");
                Console.WriteLine("This second example separates a cup on a table from the background using MimBinarize() with an M_DOMINANT mode.");
                Console.WriteLine("In this case, the dominant mode (black background) is separated from the rest. Note, using an M_BIMODAL mode");
                Console.WriteLine("would give another result because the background and the cup would be considered as the same mode.");
                Console.WriteLine("Press any key to continue.\n");
                Console.ReadKey(true);

                // Binarize the image with an automatically calculated threshold so that 
                // particles are represented in white and the background removed.
                Im.Binarize(imageDisp, imageDisp, Im.BinarizeConditionAndThreshModes.Dominant | Im.BinarizeConditionAndThreshModes.LessOrEqual, 0, 0);

                // Print a message for the extracted cup and table.
                Console.WriteLine("The cup and the table were separated from the background with M_DOMINANT mode of MimBinarize.");

                // Pause to show the result.
                Console.WriteLine("Press any key to end.\n");
                Console.ReadKey(true);
                }
            finally
                {
                // Free all allocations.
                imageDisp?.Free();
                }
            }

        public void Dispose()
            {
            _display?.Free();
            _graContext?.Free();
            _system?.Free();
            _application?.Free();
            }

        private readonly App.Application _application;
        private readonly Sys.System _system;
        private readonly Gra.Context _graContext;
        private readonly Disp.Display _display;

        private const string _imageFile = Paths.Images + "Cell.mbufi";
        private const string _imageCup = Paths.Images + "PlasticCup.mim";
        private const int _imageSmallParticleRadius = 1;
        };

    class Program
        {
        static void Main(string[] args)
            {
            try
                {
                using (var example = new MimProcessingExample())
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
