---
title: "MimProcessing"
description: "This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample."
ms.language: "C++ Object"
---

# MimProcessing

> This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample.

**Language:** C++ Object

**Functions used:** `MappAlloc`, `MappFree`, `MappInquire`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispLut`, `MdispSelect`, `MimAllocResult`, `MimArith`, `MimBinarize`, `MimClose`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MimLabel`, `MimOpen`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Image Processing, What's New, AIL 10.0 SP6, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImProcessing.cpp
//
// Description: This program show the usage of image processing. Under Aurora Imaging Library Lite, 
//              it binarizes two images to isolate specific zones.
//              Under Aurora Imaging Library full, it also uses different image processing primitives 
//              to determine the number of cell nuclei that are larger than a 
//              certain size and show them in pseudo-color.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIOL.h>
using namespace Zebra::AuroraImagingObjectLibrary;

class MImProcessingExample
   {
   public:
      MImProcessingExample()
         : _application(App::AllocInitFlags::Default),
         _system(_application),
         _graContext(_system),
         _display(_system)
         {}

      ~MImProcessingExample() = default;

      void Run()
         {
         // Show header.
         Os::Printf(AilText("\nIMAGE PROCESSING:\n"));
         Os::Printf(AilText("-----------------\n\n"));
         Os::Printf(AilText("This program shows two image processing examples.\n"));

         // Example about extracting particles in an image.
         ExtractParticlesExample();

         // Example about isolating objects from the background in an image.
         ExtractForegroundExample();
         }

      void ExtractParticlesExample()
         {
         // Restore source image and display it.
         auto imageDisp = Buf::Image(_imageFile, _system);
         _display.Select(imageDisp);

         // Pause to show the original image.
         Os::Printf(AilText("\n1) Particles extraction:\n"));
         Os::Printf(AilText("-----------------\n\n"));
         Os::Printf(AilText("This first example extracts the dark particles in an image.\n"));
         Os::Printf(AilText("Press any key to continue.\n\n"));
         Os::Getch();

         // Binarize the image with an automatically calculated threshold so that
         // particles are represented in white and the background removed.
         Im::Binarize(imageDisp, imageDisp, Im::BinarizeConditionAndThreshModes::LessOrEqual | Im::BinarizeConditionAndThreshModes::Bimodal, 0, 0);

         // Print a message for the extracted particles.
         Os::Printf(AilText("These particles were extracted from the original image.\n"));

#if (!M_AIL_LITE)
         // if Aurora Imaging Library IM module is available, count and label the larger particles.
         auto licenseModules = _application.LicenseModules();

         if(static_cast<bool>(licenseModules & App::LicenseModules::Im))
            {
            // Pause to show the extracted particles.
            Os::Printf(AilText("Press any key to continue.\n\n"));
            Os::Getch();

            // Close small holes.
            Im::Close(imageDisp, imageDisp, _ImageSmallParticleRadius, Im::CloseProcMode::Binary);

            // Remove small particles.
            Im::Open(imageDisp, imageDisp, _ImageSmallParticleRadius, Im::OpenProcMode::Binary);

            // Label the image.
            Im::Label(imageDisp, imageDisp, Im::LabelProcModes::M8Connected | Im::LabelProcModes::Grayscale);

            // The largest label value corresponds to the number of particles in the image.
            auto extremeList = Im::ExtremeList(_system, 1);
            Im::FindExtreme(imageDisp, extremeList, Im::FindExtremeExtremeTypes::MaxValue);
            std::vector<int64_t> values;
            extremeList.Values(values);
            auto maxLabelNumber = values[0];

            // Multiply the labeling result to augment the gray level of the particles.
            Im::Arith::Multiply(imageDisp, (int)(256 / (double)maxLabelNumber), imageDisp, false);

            // Display the resulting particles in pseudo-color.
            _display.Lut(Buf::Lut::PseudoLut());

            // Print results.
            Os::Printf(AilText("There were %d large particles in the original image.\n"), maxLabelNumber);
            }
#endif
         // Pause to show the result.
         Os::Printf(AilText("Press any key to continue.\n\n"));
         Os::Getch();

         // Reset display LUT to default.
         _display.Lut(Buf::Lut::DefaultLut());
         }


      void ExtractForegroundExample()
         {
         // Restore source image and display it.
         auto imageDisp = Buf::Image(_imageCup, _system);
         _display.Select(imageDisp);

         // Pause to show the original image.
         Os::Printf(AilText("\n2) Background removal:\n"));
         Os::Printf(AilText("-----------------\n\n"));
         Os::Printf(AilText("This second example separates a cup on a table from the background using MimBinarize() with an M_DOMINANT mode.\n"));
         Os::Printf(AilText("In this case, the dominant mode (black background) is separated from the rest. Note, using an M_BIMODAL mode\n"));
         Os::Printf(AilText("would give another result because the background and the cup would be considered as the same mode.\n"));
         Os::Printf(AilText("Press any key to continue.\n\n"));
         Os::Getch();

         // Binarize the image with an automatically calculated threshold so that
         // particles are represented in white and the background removed.
         Im::Binarize(imageDisp, imageDisp, Im::BinarizeConditionAndThreshModes::Dominant | Im::BinarizeConditionAndThreshModes::LessOrEqual, 0, 0);

         // Print a message for the extracted cup and table.
         Os::Printf(AilText("The cup and the table were separated from the background with M_DOMINANT mode of MimBinarize.\n"));

         // Pause to show the result.
         Os::Printf(AilText("Press any key to end.\n\n"));
         Os::Getch();
         }

   private:
      App::Application _application;
      Sys::System _system;
      Gra::Context _graContext;
      Disp::Display _display;

      const ailstring _imageFile = Paths::Images + AilText("Cell.mbufi");
      const ailstring _imageCup = Paths::Images + AilText("PlasticCup.mim");
      const int _ImageSmallParticleRadius = 1;
   };


int MosMain()
   {
   try
      {
      MImProcessingExample example;
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
