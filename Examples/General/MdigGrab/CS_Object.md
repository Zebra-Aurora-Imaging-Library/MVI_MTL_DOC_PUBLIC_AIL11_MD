---
title: "MdigGrab"
description: "This program demonstrates how to grab from a camera in continuous and monoshot mode."
ms.language: "C# Object"
---

# MdigGrab

> This program demonstrates how to grab from a camera in continuous and monoshot mode.

**Language:** C# Object

**Functions used:** `MdispAlloc`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGrab`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdigGrab.cs
//
// Description: This program demonstrates how to grab from a camera in
//              continuous and monoshot mode.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using Zebra.AuroraImagingObjectLibrary;

namespace MDigGrab
    {
    class MdigGrabExample : IDisposable
        {
        public MdigGrabExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _digitizer = new Dig.Digitizer(_system);
            _display = new Disp.Display(_system);
            _imageDisp = new Buf.Image(_system, _digitizer.SizeBand, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab);

            _imageDisp.Clear(Color8.Black);
            _display.Select(_imageDisp);
            }

        public void Run()
            {
            _digitizer.GrabContinuous(_imageDisp);

            // When a key is pressed, halt.
            Console.Write("\nDIGITIZER ACQUISITION:\n");
            Console.Write("----------------------\n\n");
            Console.Write("Continuous image grab in progress.\n");
            Console.Write("Press any key to stop.\n\n");
            Console.ReadKey(true);

            // Stop continuous grab.
            _digitizer.Halt();

            // Pause to show the result.
            Console.Write("Continuous grab stopped.\n\n");
            Console.Write("Press any key to do a single image grab.\n\n");
            Console.ReadKey(true);

            // Monoshot grab.
            _digitizer.Grab(_imageDisp);

            // Pause to show the result.
            Console.Write("Displaying the grabbed image.\n");
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);
            }

        public void Dispose()
            {
            _display?.Free();
            _imageDisp?.Free();
            _digitizer?.Free();
            _system?.Free();
            _application?.Free();
            }

        private readonly App.Application _application;
        private readonly Sys.System _system;
        private readonly Dig.Digitizer _digitizer;
        private readonly Buf.Image _imageDisp;
        private readonly Disp.Display _display;
        };

    class Program
        {
        static void Main(string[] args)
            {
            try
                {
                using (var example = new MdigGrabExample())
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
