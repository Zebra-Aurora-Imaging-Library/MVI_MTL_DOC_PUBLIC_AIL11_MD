---
title: "MdispOverlay"
description: "This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video."
ms.language: "C#"
---

# MdispOverlay

> This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video.

**Language:** C#

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
using System.Collections.Generic;
using System.Text;
using System.Drawing;
using System.Runtime.InteropServices;

using Zebra.AuroraImagingLibrary;

namespace MDispOverlay
{
    class Program
    {
        // Target image.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "Board.mim";

        // Title for the display window.
        private const string WINDOW_TITLE = "Custom Title";

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;            // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                 // System identifier.     
            AIL_ID AilDisplay = AIL.M_NULL;                // Display identifier.    
            AIL_ID AilDigitizer = AIL.M_NULL;            // Digitizer identifier.  
            AIL_ID AilImage = AIL.M_NULL;                  // Image identifier.      

            // Allocate defaults
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // If the system have a digitizer, use it.
            if (AIL.MsysInquire(AilSystem, AIL.M_DIGITIZER_NUM, AIL.M_NULL) > 0)
            {
                AIL.MdigAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, ref AilDigitizer);
                AIL.MbufAllocColor(AilSystem,
                               AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_BAND, AIL.M_NULL),
                               AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X, AIL.M_NULL),
                               AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y, AIL.M_NULL),
                               8 + AIL.M_UNSIGNED,
                               AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC + AIL.M_GRAB,
                               ref AilImage);
                AIL.MbufClear(AilImage, 0);
            }
            else
            {
                // Restore a static image.
                AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);
            }

            // Change display window title.
            AIL.MdispControl(AilDisplay, AIL.M_TITLE, WINDOW_TITLE);

            // Display the image buffer.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Draw text and graphics annotations in the display overlay.
            OverlayDraw(AilDisplay);

            // If the system supports it, grab continuously in the displayed image.
            if (AilDigitizer != AIL.M_NULL)
                AIL.MdigGrabContinuous(AilDigitizer, AilImage);

            // Pause to show the image.
            Console.Write("\nOVERLAY ANNOTATIONS:\n");
            Console.Write("--------------------\n\n");
            Console.Write("Displaying an image with overlay annotations.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Stop the continuous grab and free digitizer if needed.
            if (AilDigitizer != AIL.M_NULL)
            {
                AIL.MdigHalt(AilDigitizer);
                AIL.MdigFree(AilDigitizer);

                // Pause to show the result.
                Console.Write("Displaying the last grabbed image.\n");
                Console.Write("Press any key to end.\n\n");
                Console.ReadKey(true);
            }

            // Free image.
            AIL.MbufFree(AilImage);

            // Free default allocations.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        // ---------------------------------------------------------------
        // Name:      OverlayDraw
        // Synopsis:  This function draws annotations in the display overlay.

        static void OverlayDraw(AIL_ID AilDisplay)
        {
            AIL_ID DefaultGraphicContext = AIL.M_DEFAULT;
            AIL_ID AilOverlayImage = AIL.M_NULL;
            AIL_INT ImageWidth, ImageHeight;
            IntPtr hCustomDC = IntPtr.Zero;

            // Prepare overlay buffer.
            //***************************

            // Enable the display of overlay annotations.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);

            // Inquire the overlay buffer associated with the display.
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            // Clear the overlay to transparent.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            // Disable the overlay display update to accelerate annotations.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_SHOW, AIL.M_DISABLE);

            // Inquire overlay size.
            ImageWidth = AIL.MbufInquire(AilOverlayImage, AIL.M_SIZE_X, AIL.M_NULL);
            ImageHeight = AIL.MbufInquire(AilOverlayImage, AIL.M_SIZE_Y, AIL.M_NULL);

            // Draw overlay annotations.
            //*********************************

            // Set the graphic text background to transparent.
            AIL.MgraControl(DefaultGraphicContext, AIL.M_BACKGROUND_MODE, AIL.M_TRANSPARENT);

            // Print a white string in the overlay image buffer.
            AIL.MgraControl(DefaultGraphicContext, AIL.M_COLOR, AIL.M_COLOR_WHITE);
            AIL.MgraText(DefaultGraphicContext, AilOverlayImage, ImageWidth / 9, ImageHeight / 5, " -------------------- ");
            AIL.MgraText(DefaultGraphicContext, AilOverlayImage, ImageWidth / 9, ImageHeight / 5 + 25, " - overlay Text - ");
            AIL.MgraText(DefaultGraphicContext, AilOverlayImage, ImageWidth / 9, ImageHeight / 5 + 50, " -------------------- ");

            // Print a green string in the overlay image buffer.
            AIL.MgraControl(DefaultGraphicContext, AIL.M_COLOR, AIL.M_COLOR_GREEN);
            AIL.MgraText(DefaultGraphicContext, AilOverlayImage, ImageWidth * 11 / 18, ImageHeight / 5, " ---------------------");
            AIL.MgraText(DefaultGraphicContext, AilOverlayImage, ImageWidth * 11 / 18, ImageHeight / 5 + 25, " - overlay Text - ");
            AIL.MgraText(DefaultGraphicContext, AilOverlayImage, ImageWidth * 11 / 18, ImageHeight / 5 + 50, " ---------------------");

            // Re-enable the overlay display after all annotations are done.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_SHOW, AIL.M_ENABLE);
#if !LINUX
            // Draw GDI color overlay annotation.
            //***********************************

            // The inquire might not be supported
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);

            // Create a device context to draw in the overlay buffer with GDI.
            AIL.MbufControl(AilOverlayImage, AIL.M_DC_ALLOC, AIL.M_DEFAULT);

            // Inquire the device context.
            hCustomDC = (IntPtr)AIL.MbufInquire(AilOverlayImage, AIL.M_DC_HANDLE, AIL.M_NULL);

            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE);

            // Perform operation if GDI drawing is supported.
            if (!hCustomDC.Equals(IntPtr.Zero))
            {
                // NOTE : The using blocks will automatically call the Dipose method on the GDI objects.
                //        This ensures that resources are freed even if an exception occurs.

                // Create a System.Drawing.Graphics object from the Device context
                using (Graphics DrawingGraphics = Graphics.FromHdc(hCustomDC))
                {
                    // Draw a blue cross.
                    using (Pen DrawingPen = new Pen(Color.Blue))
                    {
                        // Draw a blue cross in the overlay image
                        DrawingGraphics.DrawLine(DrawingPen, 0, (int)(ImageHeight / 2), ImageWidth, (int)(ImageHeight / 2));
                        DrawingGraphics.DrawLine(DrawingPen, (int)(ImageWidth / 2), 0, (int)(ImageWidth / 2), ImageHeight);

                        // Prepare transparent text annotations.
                        // Define the Brushes and fonts used to draw text
                        using (SolidBrush LeftBrush = new SolidBrush(Color.Red))
                        {
                            using (SolidBrush RightBrush = new SolidBrush(Color.Yellow))
                            {
                                using (Font OverlayFont = new Font(FontFamily.GenericSansSerif, 10, FontStyle.Bold))
                                {

                                    // Write text in the overlay image
                                    SizeF GDITextSize = DrawingGraphics.MeasureString("GDI Overlay Text", OverlayFont);
                                    DrawingGraphics.DrawString("GDI Overlay Text", OverlayFont, LeftBrush, System.Convert.ToInt32(ImageWidth / 4 - GDITextSize.Width / 2), System.Convert.ToInt32(ImageHeight * 3 / 4 - GDITextSize.Height / 2));
                                    DrawingGraphics.DrawString("GDI Overlay Text", OverlayFont, RightBrush, System.Convert.ToInt32(ImageWidth * 3 / 4 - GDITextSize.Width / 2), System.Convert.ToInt32(ImageHeight * 3 / 4 - GDITextSize.Height / 2));
                                }
                            }
                        }
                    }
                }

                //   // Delete device context.
                AIL.MbufControl(AilOverlayImage, AIL.M_DC_FREE, AIL.M_DEFAULT);

                //   // Signal that the overlay buffer was modified.
                AIL.MbufControl(AilOverlayImage, AIL.M_MODIFIED, AIL.M_DEFAULT);
            }
#endif
        }
    }
}

```
