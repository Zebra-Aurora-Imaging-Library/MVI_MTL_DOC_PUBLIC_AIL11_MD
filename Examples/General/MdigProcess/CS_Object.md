---
title: "MdigProcess"
description: "This program shows the use of the MdigProcess() function to perform real-time processing."
ms.language: "C# Object"
---

# MdigProcess

> This program shows the use of the MdigProcess() function to perform real-time processing.

**Language:** C# Object

**Functions used:** `MdigGrabContinuous`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MimArith`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdigProcess.cs
//
// Description: This program shows the use of the MdigProcess() function and its multiple
//              buffering acquisition to do robust real-time processing.
//
//              The user's processing code to execute is located in a callback function
//              that will be called for each frame acquired (see ProcessingFunction()).
//
// Note:        The average processing time must be shorter than the grab time or some
//              frames will be missed. Also, if the processing results are not displayed
//              and the frame count is not drawn or printed, the CPU usage is reduced
//              significantly.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using Zebra.AuroraImagingObjectLibrary;

namespace MdigProcess
    {
    class MdigProcessExample : IDisposable
        {
        void ProcessingFunction(object sender, Dig.ProcessEventArgs e)
            {
            try
                {
                const int StringPosX = 20;
                const int StringPosY = 20;

                Buf.Image modifiedBuffer = e.ModifiedBuffer;

                // Increment the frame counter.
                _processedImageCount++;

                // Print and draw the frame count (remove to reduce CPU usage).
                Console.Write($"Processing frame #{_processedImageCount}.\r");

                _graphicContext.DrawString(modifiedBuffer, StringPosX, StringPosY, _processedImageCount.ToString());

                // Execute the processing and update the display.
                Im.Arith.Not(modifiedBuffer, _imageDisp);
                }
            catch (AIOLException exception)
                {
                // User-defined event/processing functions should always handle their own exceptions
                Console.WriteLine("Encountered an exception during the hook.");
                Console.WriteLine(exception.Message);
                Console.WriteLine("Press any key to exit.");
                _exceptionInProcess = true;
                }
            }

        public MdigProcessExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _digitizer = new Dig.Digitizer(_system);
            _display = new Disp.Display(_system);
            _graphicContext = new Gra.Context(_system);

            _imageDisp = new Buf.Image(_system, 1, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab);

            for(int i = 0; i < _bufferingSizeMax; i++)
                {
                var imageBuffer = new Buf.Image(_system, 1, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcGrab);
                imageBuffer.Clear(Color8.Black);
                _ailGrabBufferList.Add(imageBuffer);
                }

            _imageDisp.Clear(Color8.Black);
            _display.Select(_imageDisp);
            }

        public void Run()
            {
            // Print a message.
            Console.WriteLine("\nMULTIPLE BUFFERED PROCESSING.");
            Console.WriteLine("-----------------------------\n");
            Console.WriteLine("Press any key to start processing.\n");

            // Grab continuously on the display and wait for a key press.
            _digitizer.GrabContinuous(_imageDisp);
            Console.ReadKey(true);

            // Halt continuous grab.
            _digitizer.Halt();

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

            // Return if we encountered an exception during processing.
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
            foreach(var buffer in _ailGrabBufferList)
                {
                buffer?.Free();
                }
            _imageDisp?.Free();
            _graphicContext?.Free();
            _display?.Free();
            _digitizer?.Free();
            _system?.Free();
            _application?.Free();
            }

        private readonly App.Application _application;
        private readonly Sys.System _system;
        private readonly Dig.Digitizer _digitizer;
        private readonly Buf.Image _imageDisp;
        private readonly Disp.Display _display;
        private readonly Gra.Context _graphicContext;
        private readonly List<Buf.Image> _ailGrabBufferList = new List<Buf.Image>();
        private const int _bufferingSizeMax = 20;

        private int _processedImageCount = 0;
        private bool _exceptionInProcess = false;
        };

    static class Program
        {
        static void Main(string[] args)
            {
            try
                {
                using (var example = new MdigProcessExample())
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
