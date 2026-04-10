---
title: "MdispWindowLeveling"
description: "This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling."
ms.language: "C++ Object"
---

# MdispWindowLeveling

> This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling.

**Language:** C++ Object

**Functions used:** `MbufControl`, `MappAlloc`, `MappFree`, `MbufAlloc1d`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispLut`, `MdispSelect`, `MdispSelectWindow`, `MgenLutRamp`, `MgraLine`, `MgraText`, `MimAllocResult`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdispWindowLeveling.cpp
//
// Description: This program shows how to display a 10-bit monochrome Medical image
//              and applies a LUT to do interactive Window Leveling.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIOL.h>
using namespace Zebra::AuroraImagingObjectLibrary;

#define MosMin(a, b) (((a) < (b)) ? (a) : (b))
#define MosMax(a, b) (((a) > (b)) ? (a) : (b))

class MdispWindowLevelingExample
   {
   public:
      MdispWindowLevelingExample()
         : _application(App::AllocInitFlags::Default)
         , _system(_application)
         , _display(_system)
         , _graphicContext(_system)
         , _originalImage()
         {}

      ~MdispWindowLevelingExample() = default;

      void Run()
         {
         // Restore target image from disk.
         _imageDisp = Buf::Image(_imageFile, _system);

         // Dynamically calculates the maximum value of the image using processing.
         _extremeList = Im::ExtremeList(_system, 1);
         Im::FindExtreme(_imageDisp, _extremeList, Im::FindExtremeExtremeTypes::MaxValue);
         std::vector<int64_t> results;
         _extremeList.Values(results);
         int64_t imageMaxValue = results[0];

         // Set the maximum value of the image to indicate to Aurora Imaging Library how to initialize
         // the default display LUT.
         _imageDisp.Max((double)imageMaxValue);

         // Display the image (to specify a user-defined window, use MdispSelectWindow()).
         _display.Select(_imageDisp);

         // Determine the maximum displayable value of the current display.
         auto displaySizeBit = _display.SizeBit();
         auto displayMaxValue = (1 << (int)displaySizeBit) - 1;

         // Print key information.
         Os::Printf(AilText("\nINTERACTIVE WINDOW LEVELING:\n"));
         Os::Printf(AilText("----------------------------\n\n"));

         Os::Printf(AilText("Image name : %s\n"), _imageName.c_str());
         Os::Printf(AilText("Image size : %d x %d\n"), _imageDisp.SizeX(), _imageDisp.SizeY());
         Os::Printf(AilText("Image max  : %d\n"), imageMaxValue);
         Os::Printf(AilText("Display max:  %d\n\n"), displayMaxValue);

         // Allocate a LUT buffer according to the image maximum value and display pixel depth.
         _lut = Buf::Lut(_system, _imageDisp.SizeBand(), imageMaxValue + 1, 1, Buf::AllocType::Unsigned8);

         // Generate a LUT with a full range ramp and set its maximum value.
         Gen::LutRamp(_lut, 0, 0, imageMaxValue, displayMaxValue);
         _lut.Max(displayMaxValue);

         // Set the display LUT.
         _display.Lut(_lut);

         // Interactive Window Leveling using keyboard.
         Os::Printf(AilText("Keys assignment:\n\n"));
         Os::Printf(AilText("Arrow keys :    Left=move Left, Right=move Right, Down=Narrower, Up=Wider.\n"));
         Os::Printf(AilText("Intensity keys: L=Lower,  U=Upper,  R=Reset.\n"));
         Os::Printf(AilText("Press enter to end.\n\n"));

         // Modify LUT shape according to the arrow keys and update it.
         int64_t start = 0;
         auto end = imageMaxValue;
         auto inflectionLevel = displayMaxValue;
         auto step = (imageMaxValue + 1) / 128;
         step = MosMax(step, 4);

         int64_t ch = 0;
         while(ch != '\r')
            {
            switch(ch)
               {
               // Left arrow: Move region left.
               case 0x4B:
               { start -= step; end -= step; break; }

               // Right arrow: Move region right.
               case 0x4D:
               { start += step; end += step; break; }

               // Down arrow: Narrow region.
               case 0x50:
               { start += step; end -= step; break; }

               // Up arrow: Widen region.
               case 0x48:
               { start -= step; end += step; break; }

               // L key: Lower inflexion point.
               case 'L':
               case 'l':
               { inflectionLevel--; break; }

               // U key: Upper inflexion point.
               case 'U':
               case 'u':
               { inflectionLevel++; break; }

               // R key: Reset the LUT to full image range.
               case 'R':
               case 'r':
               { start = 0; end = imageMaxValue; inflectionLevel = displayMaxValue; break; }
               }

            // Saturate.
            end = MosMin(end, imageMaxValue);
            start = MosMin(start, end);
            end = MosMax(end, start);
            start = MosMax(start, 0);
            end = MosMax(end, 0);
            inflectionLevel = MosMax(inflectionLevel, 0);
            inflectionLevel = MosMin(inflectionLevel, displayMaxValue);
            Os::Printf(AilText("Inflection points: Low=(%d,0), High=(%d,%d).   \r"), start, end, inflectionLevel);

            // Generate a LUT with 3 slopes and saturated at both ends.
            Gen::LutRamp(_lut, 0, 0, start, 0);
            Gen::LutRamp(_lut, start, 0, end, inflectionLevel);
            Gen::LutRamp(_lut, end, inflectionLevel, imageMaxValue, displayMaxValue);

            // Update the display LUT.
            _display.Lut(_lut);

            // Draw the current LUT's shape in the image.
            // Note: This simple annotation method requires
            //       significant update and CPU time.
            //
            if(!_originalImage.IsAllocated())
               {
               _originalImage = Buf::Image(_imageFile, _system);
               }

            DrawLutShape(start, end, inflectionLevel, imageMaxValue, displayMaxValue);

            // If its an arrow key, get the second code.
            if((ch = Os::Getch()) == 0xE0)
               ch = Os::Getch();
            }
         Os::Printf(AilText("\n\n"));
         }

   private:
      void DrawLutShape(int64_t start, int64_t end, int64_t inflexionIntensity, int64_t imageMaxValue, int64_t displayMaxValue)
         {
         auto imageSizeX = _imageDisp.SizeX();
         auto imageSizeY = _imageDisp.SizeY();

         // Calculate the drawing parameters.
         auto xStep = (double)imageSizeX / (double)imageMaxValue;
         auto xStart = start * xStep;
         auto xEnd = end * xStep;
         auto yStep = ((double)imageSizeY / 4.0) / (double)displayMaxValue;
         auto yMin = ((double)imageSizeY - 2);
         auto yInf = yMin - (inflexionIntensity * yStep);
         auto yMax = yMin - (displayMaxValue * yStep);

         // To increase speed, disable display update until all annotations are done.
         _display.Update.Display(Disp::Update::Disable);

         // Restore the original image.
         Buf::Copy(_originalImage, _imageDisp);

         // Draw axis max and min values.
         _graphicContext.Color((double)imageMaxValue);
         _graphicContext.DrawString(_imageDisp, 4, (int)yMin - 22, AilText("0"));
         _graphicContext.DrawString(_imageDisp, 4, (int)yMax - 16, AilToString(displayMaxValue));
         _graphicContext.DrawString(_imageDisp, (double)imageSizeX - 38, (double)yMin - 22, AilToString(imageMaxValue));

         // Draw LUT Shape (X axis is display values and Y is image values).
         _graphicContext.DrawLine(_imageDisp, 0, (int)yMin, (int)xStart, (int)yMin);
         _graphicContext.DrawLine(_imageDisp, (double)xStart, (double)yMin, (double)xEnd, (double)yInf);
         _graphicContext.DrawLine(_imageDisp, (double)xEnd, (double)yInf, (double)imageSizeX - 1, (double)yMax);

         // Enable display update to show the result.
         _display.Update.Display(Disp::Update::Enable);
         }

   private:
      App::Application _application;
      Sys::System _system;
      Disp::Display _display;
      Gra::Context _graphicContext;

      Im::ExtremeList _extremeList;
      Buf::Image _imageDisp;
      Buf::Image _originalImage;
      Buf::Lut _lut;

      ailstring _imageName = AilText("ArmsMono10bit.mim");
      ailstring _imageFile = Paths::Images + _imageName;
   };

int MosMain()
   {
   try
      {
      MdispWindowLevelingExample example;
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
