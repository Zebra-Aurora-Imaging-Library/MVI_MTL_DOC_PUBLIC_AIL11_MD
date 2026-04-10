---
title: "MimHistogram"
description: "This program loads an image of a tissue sample, calculates its intensity histogram and draws it."
ms.language: "C++ Object"
---

# MimHistogram

> This program loads an image of a tissue sample, calculates its intensity histogram and draws it.

**Language:** C++ Object

**Functions used:** `MsysFree`, `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraLines`, `MimAllocResult`, `MimFree`, `MimGetResult`, `MimHistogram`, `MsysAlloc`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImHistogram.cpp
//
// Description: This program loads an image of a tissue sample, calculates its intensity
//              histogramand draws it.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIOL.h>
#include <stdlib.h>
#include <array>

using namespace Zebra::AuroraImagingObjectLibrary;

class MdigDoubleBufferingExample
   {
   public:
      MdigDoubleBufferingExample()
         : _application(App::AllocInitFlags::Default)
         , _system(_application)
         , _graphics(_system)
         , _display(_system)
         , _imageDisp()
         {}

      ~MdigDoubleBufferingExample() = default;

      void Run()
         {
         std::array<double, _histNumIntensities> xStart {};
         std::array<double, _histNumIntensities> yStart {};
         std::array<double, _histNumIntensities> xEnd {};
         std::array<double, _histNumIntensities> yEnd {};

         // Restore source image into an automatically allocated image buffer.
         _imageDisp.Restore(_imageFile, _system);

         // Display the image buffer and prepare for overlay annotations.
         _display.Select(_imageDisp);
         auto overlay = _display.Overlay;

         overlay.Enabled(Disp::Overlay::Enable);

         // Allocate a histogram result buffer.
         _histogramList = Im::HistogramList(_system, _histNumIntensities);

         // Calculate the histogram.
         Im::Histogram(_imageDisp, _histogramList);

         // Get the results.
         std::vector<int64_t> histValues;
         _histogramList.Values(histValues);
         _histogramList.Free();

         // Draw the histogram in the overlay.
         _graphics.Color(Color8::Red);
         for(int i = 0; i < _histNumIntensities; i++)
            {
            xStart[i] = i + _histPosX + 1;
            yStart[i] = _histPosY;
            xEnd[i] = i + _histPosX + 1;
            yEnd[i] = double(_histPosY - (histValues[i] / _histScaleFactor));
            }
         _graphics.DrawLineList(overlay.Image(), _histNumIntensities, xStart.data(), yStart.data(), xEnd.data(), yEnd.data());

         // Print a message.
         Os::Printf(AilText("\nHISTOGRAM:\n"));
         Os::Printf(AilText("----------\n\n"));
         Os::Printf(AilText("The histogram of the image was calculated and drawn.\n"));
         Os::Printf(AilText("Press any key to end.\n\n"));
         Os::Getch();
         }

   private:
      App::Application _application;
      Sys::System _system;
      Buf::Image _imageDisp;
      Disp::Display _display;
      Gra::Context _graphics;
      Im::HistogramList _histogramList;

      // Number of possible pixel intensities.
      static const int _histNumIntensities = 256;
      const int _histScaleFactor = 8;
      const int _histPosX = 250;
      const int _histPosY = 450;

      // Target image file specifications.
      const ailstring _imageFile = Paths::Images + AilText("Cell.mbufi");
   };


int MosMain()
   {
   try
      {
      MdigDoubleBufferingExample example;
      example.Run();
      }
   catch(const AIOLException& exception)
      {
      Os::Printf(AilText("Encountered an exception during the example. \n"));
      Os::Printf(AilText("\t %s\n "), exception.Message().c_str());
      Os::Printf(AilText("\t Press any key to exit. \n"));
      Os::Getch();
      }
   return 0;
   }

```
