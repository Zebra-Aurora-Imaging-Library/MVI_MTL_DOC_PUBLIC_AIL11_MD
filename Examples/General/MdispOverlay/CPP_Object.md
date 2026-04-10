---
title: "MdispOverlay"
description: "This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video."
ms.language: "C++ Object"
---

# MdispOverlay

> This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video.

**Language:** C++ Object

**Functions used:** `MbufClear`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAllocColor`, `MbufControl`, `MbufFree`, `MbufRestore`, `MbufInquire`, `MdigAlloc`, `MdigFree`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraControl`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MDispOverlay.cpp
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

#include <AIOL.h>
#if M_AIL_USE_WINDOWS
#include <windows.h>
#else
#include <stdlib.h>
#include <cairo.h>
#endif
using namespace Zebra::AuroraImagingObjectLibrary;

class MdispOverlayExample
   {
   private:

      void OverlayDraw(Disp::Display& display, Gra::Context& graphicContext)
         {
         // Prepare overlay buffer.
         //////////////////////////

         // Enable the display of overlay annotations.
         display.Overlay.Enabled(Disp::Overlay::Enable);

         // Clear the overlay to transparent.
         display.Overlay.Clear(Color8::Transparent);

         // Disable the overlay display update to accelerate annotations.
         display.Overlay.Show(Disp::OverlayShow::Disable);

         // Inquire overlay image
         auto oImage = display.Overlay.Image();

         // Inquire overlay size.
         auto oWidth = (double)oImage.SizeX();
         auto oHeight = (double)oImage.SizeY();

         // Draw overlay annotations.
         ////////////////////////////////

         // Set the graphic text background to transparent.
         graphicContext.BackgroundMode(Gra::BackgroundMode::Transparent);

         // Print a white string in the overlay image buffer.
         graphicContext.Color(Color8::White);
         graphicContext.DrawString(oImage, oWidth / 9, oHeight / 5, AilText(" -------------------- "));
         graphicContext.DrawString(oImage, oWidth / 9, oHeight / 5 + 25, AilText(" - overlay Text - "));
         graphicContext.DrawString(oImage, oWidth / 9, oHeight / 5 + 50, AilText(" -------------------- "));

         // Print a green string in the overlay image buffer.
         graphicContext.Color(Color8::Green);
         graphicContext.DrawString(oImage, oWidth * 11 / 18, oHeight / 5, AilText(" -------------------- "));
         graphicContext.DrawString(oImage, oWidth * 11 / 18, oHeight / 5 + 25, AilText(" - overlay Text - "));
         graphicContext.DrawString(oImage, oWidth * 11 / 18, oHeight / 5 + 50, AilText(" -------------------- "));

         // Re-enable the overlay display after all annotations are done.
         display.Overlay.Show(Disp::OverlayShow::Enable);

         // Draw GDI color overlay annotation.
         /////////////////////////////////////

         _application.ErrorGlobalMode(App::ErrorMode::PrintDisable);
#if M_AIL_USE_WINDOWS
         HDC            hCustomDC;
         HGDIOBJ        hpen, hpenOld;
         // Create a device context to draw in the overlay buffer with GDI.
         oImage.DcAlloc();

         // Inquire the device context.
         hCustomDC = ((HDC)oImage.DcHandle());

         // Re-enable error printing
         _application.ErrorGlobalMode(App::ErrorMode::PrintEnable);

         // Perform operation if GDI drawing is supported.
         if(hCustomDC)
            {
            POINT hor[2];
            POINT ver[2];
            SIZE  txtSz;
            RECT  txt;

            // Draw a blue cross.
            hpen = CreatePen(PS_SOLID, 1, RGB(0, 0, 255));
            hpenOld = SelectObject(hCustomDC, hpen);

            hor[0].x = (long)0;
            hor[0].y = (long)(oHeight / 2);
            hor[1].x = (long)oWidth;
            hor[1].y = (long)(oHeight / 2);
            Polyline(hCustomDC, hor, 2);

            ver[0].x = (long)(oWidth / 2);
            ver[0].y = (long)0;
            ver[1].x = (long)(oWidth / 2);
            ver[1].y = (long)oHeight;
            Polyline(hCustomDC, ver, 2);

            SelectObject(hCustomDC, hpenOld);
            DeleteObject(hpen);

            // Prepare transparent text annotations.
            SetBkMode(hCustomDC, TRANSPARENT);
            ailstring chText = AilText("GDI Overlay Text");
            GetTextExtentPoint(hCustomDC, chText.c_str(), (int)chText.size(), &txtSz);

            // Write red text.
            txt.left = (long)(oWidth * 3 / 18);
            txt.top = (long)(oHeight * 17 / 24);
            txt.right = (long)(txt.left + txtSz.cx);
            txt.bottom = (long)(txt.top + txtSz.cy);
            SetTextColor(hCustomDC, RGB(255, 0, 0));
            DrawText(hCustomDC, chText.c_str(), (int)chText.size(), &txt, DT_RIGHT);

            // Write yellow text.
            txt.left = (long)oWidth * 12 / 18;
            txt.top = (long)oHeight * 17 / 24;
            txt.right = (long)(txt.left + txtSz.cx);
            txt.bottom = (long)(txt.top + txtSz.cy);
            SetTextColor(hCustomDC, RGB(255, 255, 0));
            DrawText(hCustomDC, chText.c_str(), (int)chText.size(), &txt, DT_RIGHT);

            // Delete device context.
            oImage.DcFree();

            // Signal that the overlay buffer was modified.
            oImage.Modified();
            }
#else
         cairo_surface_t * surface = nullptr;

         // Create a device context to draw in the overlay buffer with Cairo.
         oImage.SurfaceAlloc();

         // Inquire the device context.
         surface = (cairo_surface_t *)oImage.SurfaceHandle();

         // Re-enable error printing
         _application.ErrorGlobalMode(App::ErrorMode::PrintEnable);

         // Perform operation if Cairo drawing supported.
         if(surface)
            {
            cairo_t *cr;
            cr = cairo_create (surface);

            cairo_set_source_rgb (cr, 0, 0, 1);
            // Draw a blue cross in the overlay buffer.
            cairo_move_to (cr, 0, oHeight/2);
            cairo_line_to (cr, oWidth, oHeight/2);
            cairo_stroke (cr);
            cairo_move_to (cr, oWidth/2, 0);
            cairo_line_to (cr, oWidth/2, oHeight);
            cairo_stroke (cr);

            // Write Red text in the overlay buffer.
            ailstring chText ="Cairo Overlay Text ";
            cairo_select_font_face (cr, "monospace", CAIRO_FONT_SLANT_NORMAL, CAIRO_FONT_WEIGHT_NORMAL);
            cairo_set_source_rgb (cr, 1, 0, 0);
            cairo_set_font_size(cr, 13);
            cairo_move_to(cr, oWidth*3/18, oHeight*4/6);
            cairo_show_text(cr, chText.c_str());

            // Write Yellow text in the overlay buffer.
            cairo_set_source_rgb (cr, 1, 1, 0);
            cairo_set_font_size(cr, 13);
            cairo_move_to(cr,  oWidth*12/18,oHeight*4/6);
            cairo_show_text(cr, chText.c_str());

            cairo_surface_flush(surface);
            cairo_destroy(cr);

            // Delete device context.
            oImage.SurfaceFree();

            // Signal that the overlay buffer was modified.
            oImage.Modified();
            }
#endif
         }

   public:

      void Run()
         {
         // Draw text and graphics annotations in the display overlay.
         OverlayDraw(_display, _graphicContext);

         // If the system supports it, grab continuously in the displayed image.
         if(_digitizer.IsAllocated())
            _digitizer.GrabContinuous(_imageDisp);

         // Pause to show the image.
         Os::Printf(AilText("\nOVERLAY ANNOTATIONS:\n"));
         Os::Printf(AilText("--------------------\n\n"));
         Os::Printf(AilText("Displaying an image with overlay annotations.\n"));
         Os::Printf(AilText("Press any key to continue.\n\n"));
         Os::Getch();

         if(_digitizer.IsAllocated())
            {
            _digitizer.Halt();

            // Pause to show the result.
            Os::Printf(AilText("Displaying the last grabbed image.\n"));
            Os::Printf(AilText("Press any key to end.\n\n"));
            Os::Getch();
            }
         }

      MdispOverlayExample()
         : _application(App::AllocInitFlags::Default)
         , _system(_application)
         , _display(_system)
         , _graphicContext(_system)
         {
         if(_system.DigitizerNum() > 0)
            {
            _digitizer = Dig::Digitizer(_system);
            _imageDisp = Buf::Image(_system, 3, _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcDispGrab);
            _imageDisp.Clear(Color8::Black);
            }
         else
            {
            _imageDisp.Restore(_imageFile, _system);
            }
         _display.Title(_windowTitle);
         _display.Select(_imageDisp);
         }

      ~MdispOverlayExample() = default;

   private:

      App::Application _application;
      Sys::System _system;
      Dig::Digitizer _digitizer;
      Buf::Image _imageDisp;
      Disp::Display _display;
      Gra::Context _graphicContext;

      // Target image.
      const ailstring _imageFile = Paths::Images + AilText("Board.mim");

      // Title for the display window.
      const ailstring _windowTitle = AilText("Custom Title");
   };


int MosMain()
   {
   try
      {
      MdispOverlayExample example;
      example.Run();
      }
   catch(const AIOLException& exception)
      {
      Os::Printf(AilText("Encountered an exception during the example. \n"));
      Os::Printf(AilText("%s \n"), exception.Message().c_str());
      Os::Printf(AilText("Press any key to exit. \n"));
      Os::Getch();
      }
   return 0;
   }

```
