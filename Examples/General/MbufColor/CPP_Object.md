---
title: "MbufColor"
description: "This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result."
ms.language: "C++ Object"
---

# MbufColor

> This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result.

**Language:** C++ Object

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufChild2d`, `MbufChildColor`, `MbufClear`, `MbufDiskInquire`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispSelect`, `MgraText`, `MimArith`, `MimConvert`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MBufColor.cpp
//
// Description: This program demonstrates color buffer manipulations. It allocates 
//              a displayable color image buffer, displays it, and loads a color   
//              image into the left part. After that, color annotations are done 
//              in each band of the color buffer. Finally, to increase the image
//              luminance, the image is converted to Hue, Saturation and Luminance
//              (HSL), a certain offset is added to the luminance component and 
//              the image is converted back to Red, Green, Blue (RGB) into the 
//              right part to display the result. 
//           
//              The example also demonstrates how to display multiple images 
//              in a single display using child buffers.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIOL.h>
using namespace Zebra::AuroraImagingObjectLibrary;

class MbufColorExample
   {
   private:
      App::Application _application;
      Sys::System _system;
      Buf::Image _imageDisp;
      Disp::Display _display;
      Gra::Context _graphicContext;
      Buf::Image _lumSubImage;
      Buf::Image _redBandSubImage;
      Buf::Image _greenBandSubImage;
      Buf::Image _blueBandSubImage;
      Buf::Image _rightSubImage;
      Buf::Image _leftSubImage;

      // Source image file specifications.
      const ailstring _imageFile = Paths::Images + AilText("Bird.mim");

      // Luminance offset to add to the image.
      const int _imageLuminanceOffset = 40;

   public:
      MbufColorExample()
         : _application(App::AllocInitFlags::Default)
         , _system(_application)
         , _display(_system)
         , _graphicContext(_system)
         {}

      ~MbufColorExample()
         {
         _lumSubImage.Free();
         _redBandSubImage.Free();
         _greenBandSubImage.Free();
         _blueBandSubImage.Free();
         _rightSubImage.Free();
         _leftSubImage.Free();
         }

      void Run()
         {
         auto fileInfo = _application.File.FileInfo(_imageFile);
         auto sizeX = fileInfo.SizeX();
         auto sizeY = fileInfo.SizeY();
         auto sizeBand = fileInfo.SizeBand();

         // Allocate a color display buffer twice the size of the source image and display it.
         _imageDisp = Buf::Image(_system, sizeBand, sizeX * 2, sizeY, Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcDisp);

         _imageDisp.Clear(Color8::Black);
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
         _redBandSubImage = _leftSubImage.ChildColor(Buf::ColorBand::Red);
         _greenBandSubImage = _leftSubImage.ChildColor(Buf::ColorBand::Green);
         _blueBandSubImage = _leftSubImage.ChildColor(Buf::ColorBand::Blue);

         // Write color text annotations to show access in each individual band of the image.
         //
         _graphicContext.Color(0xFF);
         _graphicContext.DrawString(_redBandSubImage, sizeX / static_cast<double>(16), sizeY / static_cast<double>(8), AilText(" TOUCAN "));
         _graphicContext.Color(0x90);
         _graphicContext.DrawString(_greenBandSubImage, sizeX / static_cast<double>(16), sizeY / static_cast<double>(8), AilText(" TOUCAN "));
         _graphicContext.Color(0x00);
         _graphicContext.DrawString(_blueBandSubImage, sizeX / static_cast<double>(16), sizeY / static_cast<double>(8), AilText(" TOUCAN "));

         // Print a message.
         Os::Printf(AilText("\nCOLOR OPERATIONS:\n"));
         Os::Printf(AilText("-----------------\n\n"));
         Os::Printf(AilText("A color source image was loaded on the left and color text\n"));
         Os::Printf(AilText("annotations were written in it.\n"));
         Os::Printf(AilText("Press any key to continue.\n\n"));
         Os::Getch();

         // Convert image to Hue, Luminance, Saturation color space (HSL).
         Im::Convert(_leftSubImage, _rightSubImage, Im::ConversionType::RgbToHsl);

         // Create a child buffer that maps to the luminance component.
         _lumSubImage = _rightSubImage.ChildColor(Buf::ColorBand::Luminance);

         // Add an offset to the luminance component.
         Im::Arith::Add(_lumSubImage, _imageLuminanceOffset, _lumSubImage, true);

         // Convert image back to Red, Green, Blue color space (RGB) for display.
         Im::Convert(_rightSubImage, _rightSubImage, Im::ConversionType::HslToRgb);

         // Print a message.
         Os::Printf(AilText("Luminance was increased using color image processing.\n"));

         // Print a message.
         Os::Printf(AilText("Press any key to end.\n"));
         Os::Getch();
         }
   };

int MosMain()
   {
   try
      {
      MbufColorExample example;
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
