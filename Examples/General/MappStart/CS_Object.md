---
title: "MappStart"
description: "This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors."
ms.language: "C# Object"
---

# MappStart

> This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors.

**Language:** C# Object

**Functions used:** `MgraAlloc`, `MappAlloc`, `MappControl`, `MappFree`, `MappGetError`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraControl`, `MgraFont`, `MgraFree`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, What's New, Older, Advanced, Board specific, DistributedAIL specific, Licenses, Hardwares, Languages, Notes

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MappStart.cs
//
// Description: This program allocates an application and system, then displays
//              messages using TrueType fonts. It also shows how to check for errors.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using Zebra.AuroraImagingObjectLibrary;

namespace MAppStart
    {
    class MappStartExample : IDisposable
        {
        public MappStartExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _graContext = new Gra.Context(_system);
            _display = new Disp.Display(_system);
            _imageDisp = new Buf.Image(_system, 1, 640, 480, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDisp);

            _imageDisp.Clear(Color8.Black);
            _display.Select(_imageDisp);
            }
        public void Run()
            {
            _graContext.Color = 0xF0;
            _graContext.SetFont(Gra.FontName.DefaultLarge);
            _graContext.DrawString(_imageDisp, 120L, 20L, "Aurora Imaging Library");

            _graContext.Font.Size = 12;
            _graContext.SetFont("AILFont");
            _graContext.DrawString(_imageDisp, 40L, 80L, "English");

            _graContext.Font.Size = 16;
            _graContext.SetFont("AILFont" + ":Bold");
            _graContext.DrawString(_imageDisp, 40L, 140L, "Français");

            _graContext.Font.Size = 24;
            _graContext.SetFont("AILFont" + ":Italic");
            _graContext.DrawString(_imageDisp, 40L, 220L, "Italiano");

            _graContext.Font.Size = 30;
            _graContext.SetFont("AILFont" + ":Bold:Italic");
            _graContext.DrawString(_imageDisp, 40L, 300L, "Deutsch");

            // Print a message.
            Console.WriteLine("\nINTERNATIONAL TEXT ANNOTATION:");
            Console.WriteLine("------------------------------");
            Console.WriteLine("\nThis example demonstrates the support of TrueType fonts by MgraText.\n");

            bool missingFont = false;

            try
                {
                _graContext.Font.Size = 36;

                if (_system.OwnerApplication.PlatformOsType == App.PlatformOsType.Linux)
                   _graContext.SetFont("Monospace");
                else
                   _graContext.SetFont("Courier New");

                _graContext.DrawString(_imageDisp, 40L, 380L, "Español");
                }
            catch (Exception)
                {
                missingFont = true;
                }

            try
                {
                // Draw Greek, Japanese and Korean
                _graContext.SetFont("AILFont");

                _graContext.Font.Size = 12;
                _graContext.DrawString(_imageDisp, 400L, 80L, "ελληνιδ");
                }
            catch (Exception)
                {
                missingFont = true;
                }

            try
                {
                _graContext.Font.Size = 16;
                _graContext.DrawString(_imageDisp, 400L, 140L, "日本語");
                }
            catch (Exception)
                {
                missingFont = true;
                }

            try
                {
                _graContext.Font.Size = 24;
                _graContext.DrawString(_imageDisp, 400L, 220L, "한국어");
                }
            catch (Exception)
                {
                missingFont = true;
                }

            try
                {
                // Draw Arabic and Hebrew
                _graContext.Text.Direction = Gra.TextDirection.RightToLeft;

                _graContext.Font.Size = 30;
                _graContext.DrawString(_imageDisp, 400L, 320L, "עברית");
                }
            catch (Exception)
                {
                missingFont = true;
                }

            try
                {
                _graContext.Font.Size = 36;
                _graContext.DrawString(_imageDisp, 400L, 380L, "ﻋﺮﺑﻲ");
                }
            catch (Exception)
                {
                missingFont = true;
                }

            if (missingFont)
                {
                Console.WriteLine("Note: Some Unicode fonts are not available\n");
                }
            // Wait for a key press.
            Console.WriteLine("Press any key to end.");
            Console.ReadKey(true);
            }

        public void Dispose()
            {
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
        };

    class Program
        {
        static void Main(string[] args)
            {
            try
                {
                using (var example = new MappStartExample())
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
