---
title: "MdispOverlay"
description: "This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video."
ms.language: "C# Object"
---

# MdispOverlay

> This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video.

**Language:** C# Object

**Functions used:** `MbufClear`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAllocColor`, `MbufControl`, `MbufFree`, `MbufRestore`, `MbufInquire`, `MdigAlloc`, `MdigFree`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraControl`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MDispOverlay.cs
//
// Description: This program shows how to display an image while creating
//              textand graphics annotations on top of it using AIL
//              graphic functionsand windows GDI drawing under Windows
//              or cairo drawing under Linux.
//              If the target system supports grabbing, the annotations
//              are done on top of a continuous grab.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using Zebra.AuroraImagingObjectLibrary;
using System.Drawing;

namespace MdispOverlay
    {
    class MdispOverlayExample : IDisposable
        {
        public MdispOverlayExample()
            {
            _application = new App.Application(App.AllocInitFlags.Default);
            _system = new Sys.System(_application);
            _graphicContext = new Gra.Context(_system);
            _display = new Disp.Display(_system);

            if (_system.DigitizerNum > 0)
                {
                _digitizer = new Dig.Digitizer(_system);
                _imageDisp = new Buf.Image(_system, 3, _digitizer.SizeX, _digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab);
                _imageDisp.Clear(Color8.Black);
                }
            else
                {
                _imageDisp = new Buf.Image(_imageFile, _system);
                }

            _display.Title = _windowTitle;
            _display.Select(_imageDisp);
            }

        public void Run()
            {
            OverlayDraw(_display, _graphicContext);

            // If the system supports it, grab continuously in the displayed image.
            if (_digitizer?.IsAllocated == true)
                {
                _digitizer.GrabContinuous(_imageDisp);
                }

            // Pause to show the image.
            Console.Write("\nOVERLAY ANNOTATIONS:\n");
            Console.Write("--------------------\n\n");
            Console.Write("Displaying an image with overlay annotations.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Stop the continuous grab and free digitizer if needed.
            if (_digitizer != null)
                {
                _digitizer.Halt();

                // Pause to show the result.
                Console.Write("Displaying the last grabbed image.\n");
                Console.Write("Press any key to end.\n\n");
                Console.ReadKey(true);
                }
            }

        private void OverlayDraw(Disp.Display display, Gra.Context graphicContext)
            {
            // Prepare overlay buffer.
            //***************************

            // Enable the display of overlay annotations.
            display.Overlay.Enabled = Disp.Overlay.Enable;

            // Disable the overlay display update to accelerate annotations.
            display.Overlay.Show = Disp.OverlayShow.Disable;

            // Inquire overlay size.
            var oWidth = display.Overlay.Image.SizeX;
            var oHeight = display.Overlay.Image.SizeY;

            // Draw overlay annotations.
            //*********************************

            // Set the graphic text background to transparent.
            graphicContext.BackgroundMode = Gra.BackgroundMode.Transparent;
            display.Overlay.Clear(Color8.Transparent);

            // Print a white string in the overlay image buffer.
            graphicContext.Color = Color8.White;
            graphicContext.DrawString(display.Overlay.Image, oWidth / 9, oHeight / 5, " -------------------- ");
            graphicContext.DrawString(display.Overlay.Image, oWidth / 9, oHeight / 5 + 25, " - overlay Text - ");
            graphicContext.DrawString(display.Overlay.Image, oWidth / 9, oHeight / 5 + 50, " -------------------- ");

            // Print a green string in the overlay image buffer.
            graphicContext.Color = Color8.Green;
            graphicContext.DrawString(display.Overlay.Image, oWidth * 11 / 18, oHeight / 5, " -------------------- ");
            graphicContext.DrawString(display.Overlay.Image, oWidth * 11 / 18, oHeight / 5 + 25, " - overlay Text - ");
            graphicContext.DrawString(display.Overlay.Image, oWidth * 11 / 18, oHeight / 5 + 50, " -------------------- ");

            // Re-enable the overlay display after all annotations are done.
            display.Overlay.Show = Disp.OverlayShow.Enable;

            // Draw GDI color overlay annotation.
            //***********************************

            // The inquire might not be supported
            _application.ErrorGlobalMode = App.ErrorMode.PrintDisable;

            // Create a device context to draw in the overlay buffer with GDI.
            display.Overlay.Image.DcAlloc();

            // Inquire the device context.
            var hCustomDC = (IntPtr)display.Overlay.Image.DcHandle;

            _application.ErrorGlobalMode = App.ErrorMode.PrintEnable;

            // Perform operation if GDI drawing is supported.
            if (!hCustomDC.Equals(IntPtr.Zero))
                {
                // NOTE : The using blocks will automatically call the Dipose method on the GDI objects.
                //        This ensures that resources are freed even if an exception occurs.

                // Create a System.Drawing.Graphics object from the Device context
                using (Graphics drawingGraphics = Graphics.FromHdc(hCustomDC))
                    {
                    // Draw a blue cross.
                    using (var drawingPen = new Pen(Color.Blue))
                        {
                        // Draw a blue cross in the overlay image
                        drawingGraphics.DrawLine(drawingPen, 0, (int)(oHeight / 2), oWidth, (int)(oHeight / 2));
                        drawingGraphics.DrawLine(drawingPen, (int)(oWidth / 2), 0, (int)(oWidth / 2), oHeight);

                        // Prepare transparent text annotations.
                        // Define the Brushes and fonts used to draw text
                        using (var LeftBrush = new SolidBrush(Color.Red))
                            {
                            using (var RightBrush = new SolidBrush(Color.Yellow))
                                {
                                using (var overlayFont = new Font(FontFamily.GenericSansSerif, 10, FontStyle.Bold))
                                    {
                                    // Write text in the overlay image
                                    SizeF gdiTextSize = drawingGraphics.MeasureString("GDI Overlay Text", overlayFont);
                                    drawingGraphics.DrawString("GDI Overlay Text", overlayFont, LeftBrush, System.Convert.ToInt32(oWidth / 4 - gdiTextSize.Width / 2), System.Convert.ToInt32(oHeight * 3 / 4 - gdiTextSize.Height / 2));
                                    drawingGraphics.DrawString("GDI Overlay Text", overlayFont, RightBrush, System.Convert.ToInt32(oWidth * 3 / 4 - gdiTextSize.Width / 2), System.Convert.ToInt32(oHeight * 3 / 4 - gdiTextSize.Height / 2));
                                    }
                                }
                            }
                        }
                    }

                // Delete device context.
                display.Overlay.Image.DcFree();

                // Signal that the overlay buffer was modified.
                display.Overlay.Image.Modified();
                }
            }

        public void Dispose()
            {
            _digitizer?.Free();
            _graphicContext?.Free();
            _display?.Free();
            _imageDisp?.Free();
            _system?.Free();
            _application?.Free();
            }

        private readonly App.Application _application;
        private readonly Sys.System _system;
        private readonly Dig.Digitizer _digitizer;
        private readonly Buf.Image _imageDisp;
        private readonly Disp.Display _display;
        private readonly Gra.Context _graphicContext;

        // Target image.
        private const string _imageFile = Paths.Images + "Board.mim";

        // Title for the display window.
        private const string _windowTitle = "Custom Title";
        };

    class Program
        {
        static void Main(string[] args)
            {
            try
                {
                using (var example = new MdispOverlayExample())
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
