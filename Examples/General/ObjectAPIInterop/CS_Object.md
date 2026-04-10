---
title: "ObjectAPIInterop"
description: "This program shows the interoperability between the Object API and the Classic API. This can be used either to transition existing code progressively towards the Object API or to be able to use a functionality not yet available in the Object API."
ms.language: "C# Object"
---

# ObjectAPIInterop

> This program shows the interoperability between the Object API and the Classic API. This can be used either to transition existing code progressively towards the Object API or to be able to use a functionality not yet available in the Object API.

**Language:** C# Object

**Functions used:** `MappAlloc`, `MappFree`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobSelect`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAlloc`, `MgraControl`, `MimBinarize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Image Processing, What's New, AIL 11.0

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    ObjectAPIInterop.cs
//
// Description: This program shows the interoperability between the object API
//              and the classic API.
//              This can be used either to transition existing code
//              progressively towards the object API or to enable the
//              use of a functionality not yet available in the object API.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using Zebra.AuroraImagingObjectLibrary;
using Zebra.AuroraImagingLibrary;
using System.Collections.Generic;

namespace ObjectAPIInterop
    {
    class HybridApiExample : IDisposable
        {
        void ProcessingFunction(object sender, Dig.ProcessEventArgs processEvent)
            {
            try
                {
                // Increment the frame counter.
                _processedImageCount++;

                // Print and draw the frame count (remove to reduce CPU usage).
                Console.Write($"Processing frame #{_processedImageCount}.\r");

                // Grab the modified image.
                Buf.Image modifiedImage = processEvent.ModifiedBuffer;

                // Allocate a binary image containing the data of the modified image.
                Buf.Copy(modifiedImage, _binImage);

                // The following code uses modules that are not yet available in the object API.
                // To use an object from the object API with the classic API, use its identifier (AIL_ID) in the classic function calls.

                // Binarize the image for processing.
                AIL.MimBinarize(_binImage.Id, _binImage.Id, AIL.M_DOMINANT + AIL.M_LESS_OR_EQUAL, AIL.M_NULL, AIL.M_NULL);

                // Set the blob context to the foreground value.
                AIL.MblobControl(_blobContextId, AIL.M_FOREGROUND_VALUE, AIL.M_ZERO);

                // Enable the desired features for the blob calculation.
                AIL.MblobControl(_blobContextId, AIL.M_MIN_AREA_BOX, AIL.M_ENABLE);

                // Calculate the blobs.
                AIL.MblobCalculate(_blobContextId, _binImage.Id, AIL.M_NULL, _blobResultId);

                // Select blobs based on area.
                AIL.MblobSelect(_blobResultId, AIL.M_INCLUDE_ONLY, AIL.M_AREA, AIL.M_GREATER_OR_EQUAL, 250, AIL.M_NULL);

                // Draw the selected blobs.
                _display.Overlay.Clear(Color8.Transparent);
                AIL.MblobDraw(_graphicContext.Id, _blobResultId, _display.Overlay.Image.Id, AIL.M_DRAW_BOX + AIL.M_DRAW_BOX_CENTER, AIL.M_INCLUDED_BLOBS, AIL.M_DEFAULT);

                Buf.Copy(modifiedImage, _imageDisp);
                }
            catch (AIOLException exception)
                {
                // User-defined event/processing functions should always handle their own exceptions.
                Console.WriteLine("Encountered an exception during the hook.");
                Console.WriteLine($"The function {exception.Function} threw the following exception:");
                Console.WriteLine($"   {exception.Message}\n");
                Console.WriteLine("Press any key to exit.");
                _exceptionInProcess = true;
                }
            }

        public HybridApiExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _digitizer = new Dig.Digitizer(_system);
            _display = new Disp.Display(_system);
            _graphicContext = new Gra.Context(_system);

            _imageDisp = new Buf.Image(_system, 1, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab);
            _binImage = new Buf.Image(_system, 1, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab);

            // Allocate the classic API objects.
            AIL.MblobAlloc(_system.Id, AIL.M_DEFAULT, AIL.M_DEFAULT, ref _blobContextId);
            AIL.MblobAllocResult(_system.Id, AIL.M_DEFAULT, AIL.M_DEFAULT, ref _blobResultId);

            for (int i = 0; i < _bufferingSizeMax; i++)
                {
                var imageBuffer = new Buf.Image(_system, 1, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcGrab);
                imageBuffer.Clear(Color8.Black);
                _ailGrabBufferList.Add(imageBuffer);
                }

            _graphicContext.Color = Color8.Green;
            _imageDisp.Clear(Color8.Black);
            _display.Select(_imageDisp);
            }

        public void Run()
            {
            Console.WriteLine("");
            Console.WriteLine("CLASSIC <-> OBJECT API:");
            Console.WriteLine("-----------------------\n");
            Console.WriteLine("This example shows the interoperability");
            Console.WriteLine("between the object API and the classic API.");
            Console.WriteLine("The image acquisition uses the object API and the");
            Console.WriteLine("image processing is performed using the classic API.");
            Console.WriteLine("");
            Console.WriteLine("This code is useful to transition existing code");
            Console.WriteLine("towards the object API or to enable the use of a");
            Console.WriteLine("functionality not yet available in the object API.");
            Console.WriteLine("");

            // Enable the display's overlay for processing.
            _display.Overlay.Enabled = Disp.Overlay.Enable;

            // Register the hook function.
            _digitizer.Process.OnDigProcessEvent += ProcessingFunction;

            // Start the processing. The processing function is called with every frame grabbed.
            _digitizer.Process.Start(_ailGrabBufferList);

            // Here the main() is free to perform other tasks while the processing is executing.
            // ---------------------------------------------------------------------------------

            // Print a message and wait for a key press after a minimum number of frames.
            Console.WriteLine("Press any key to stop.\n");
            while (!Console.KeyAvailable && !_exceptionInProcess)
                {}

            // Stop the processing.
            _digitizer.Process.Stop();
            Console.ReadKey(true);

            // Return if an exception is encountered during processing.
            if (_exceptionInProcess)
                {
                return;
                }

            // Print statistics.
            var processFrameCount = _digitizer.ProcessFrameCount;
            var processFrameRate = _digitizer.ProcessFrameRate;
            Console.WriteLine("\n\n{0} frames grabbed at {1:0.0} frames/sec ({2:0.0} ms/frame).", processFrameCount, processFrameRate, 1000.0 / processFrameRate);
            Console.WriteLine("Press any key to end.\n");
            Console.ReadKey(true);
            }

        public void Dispose()
            {
            // Free the classic API objects.
            AIL.MblobFree(_blobContextId);
            AIL.MblobFree(_blobResultId);

            foreach (var buffer in _ailGrabBufferList)
                {
                buffer?.Free();
                }

            _binImage?.Free();
            _imageDisp?.Free();
            _graphicContext?.Free();
            _display?.Free();
            _digitizer?.Free();
            _system?.Free();
            _application?.Free();
            }

        private readonly App.Application _application;
        private readonly Sys.System      _system;
        private readonly Dig.Digitizer   _digitizer;
        private readonly Buf.Image       _imageDisp;
        private readonly Buf.Image       _binImage;
        private readonly Disp.Display    _display;
        private readonly Gra.Context     _graphicContext;

        private readonly List<Buf.Image> _ailGrabBufferList = new List<Buf.Image>();

        private readonly AIL_ID _blobContextId;
        private readonly AIL_ID _blobResultId;

        private const int _bufferingSizeMax = 20;

        private int  _processedImageCount = 0;
        private bool _exceptionInProcess  = false;
        };

    static class Program
        {
        static void Main(string[] args)
            {
            using (var example = new HybridApiExample())
                {
                try
                    {
                    example.Run();
                    }
                catch (AIOLException exception)
                    {
                    Console.WriteLine("Encountered an exception during the example.");
                    Console.WriteLine(exception.Message);
                    Console.WriteLine("Press any key to exit.");
                    Console.ReadKey(true);
                    }
                }
            }
        }
    }

```
