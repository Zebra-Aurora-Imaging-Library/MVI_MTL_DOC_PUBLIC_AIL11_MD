---
title: "ObjectAPIInterop"
description: "This program shows the interoperability between the Object API and the Classic API. This can be used either to transition existing code progressively towards the Object API or to be able to use a functionality not yet available in the Object API."
ms.language: "C++ Object"
---

# ObjectAPIInterop

> This program shows the interoperability between the Object API and the Classic API. This can be used either to transition existing code progressively towards the Object API or to be able to use a functionality not yet available in the Object API.

**Language:** C++ Object

**Functions used:** `MappAlloc`, `MappFree`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobSelect`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAlloc`, `MgraControl`, `MimBinarize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Image Processing, What's New, AIL 11.0

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    ObjectAPIInterop.cpp
//
// Description: This program shows the interoperability between the object API
//              and the classic API.
//              This can be used either to transition existing code
//              progressively towards the object API or to enable the
//              use of a functionality not yet available in the object API.
// 
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIOL.h>
#include <AIL.h>

using namespace Zebra::AuroraImagingObjectLibrary;

class HybridApiExample
   {
   private:
      static const int _bufferingSizeMax = 20;
      int _processedImageCount = 0;
      bool _exceptionInProcess = false;

      // Member variables using the object API.
      App::Application _application;
      Sys::System      _system;
      Dig::Digitizer   _digitizer;
      Disp::Display    _display;
      Gra::Context     _graphicContext;
      Buf::Image       _imageDisp;
      Buf::Image       _binImage;

      std::vector<Buf::Image> _ailGrabBufferList;

      // Member variables using the classic API.
      AIL_ID _blobContextId;
      AIL_ID _blobResultId;

   public:
      HybridApiExample()
         : _application(App::AllocInitFlags::Default)
         , _system(_application)
         , _digitizer(_system)
         , _display(_system)
         , _graphicContext(_system)
         , _imageDisp(_system, 1, _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcDispGrab)
         , _binImage(_system, 1, _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcDispGrab)
         {
         // Allocate the classic API objects.
         MblobAlloc(_system.Id(), M_DEFAULT, M_DEFAULT, &_blobContextId);
         MblobAllocResult(_system.Id(), M_DEFAULT, M_DEFAULT, &_blobResultId);

         for(int i = 0; i < _bufferingSizeMax; i++)
            {
            _ailGrabBufferList.emplace_back(Buf::Image(_system, 1, _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcGrab));
            _ailGrabBufferList.back().Clear(Color8::Black);
            }

         _graphicContext.Color(Color8::Green);
         _imageDisp.Clear(Color8::Black);
         _display.Select(_imageDisp);
         }

      ~HybridApiExample()
         {
         // Free the classic API objects.
         MblobFree(_blobContextId);
         MblobFree(_blobResultId);
         };

      int64_t ProcessingFunction(const Dig::ProcessEventArgs& Event)
         {
         try
            {
            // Increment the frame counter.
            _processedImageCount++;

            // Print and draw the frame count (remove to reduce CPU usage).
            Os::Printf(AilText("Processing frame #%d.\r"), _processedImageCount);

            // Grab the modified image.
            Buf::Image modifiedImage = Event.ModifiedBuffer();

            // Allocate a binary image containing the data of the modified image.
            Buf::Copy(modifiedImage, _binImage);

            // The following code uses modules that are not yet available in the object API.
            // To use an object from the object API with the classic API, use its identifier (AIL_ID) in the classic function calls.

            // Binarize the image for processing.
            MimBinarize(_binImage.Id(), _binImage.Id(), M_DOMINANT + M_LESS_OR_EQUAL, M_NULL, M_NULL);

            // Set the blob context to the foreground value.
            MblobControl(_blobContextId, M_FOREGROUND_VALUE, M_ZERO);

            // Enable the desired features for the blob calculation.
            MblobControl(_blobContextId, M_MIN_AREA_BOX, M_ENABLE);

            // Calculate the blobs.
            MblobCalculate(_blobContextId, _binImage.Id(), M_NULL, _blobResultId);

            // Select blobs based on area.
            MblobSelect(_blobResultId, M_INCLUDE_ONLY, M_AREA, M_GREATER_OR_EQUAL, 250, M_NULL);

            // Draw the selected blobs.
            _display.Overlay.Clear(Color8::Transparent);
            MblobDraw(_graphicContext.Id(), _blobResultId, _display.Overlay.Image().Id(), M_DRAW_BOX + M_DRAW_BOX_CENTER, M_INCLUDED_BLOBS, M_DEFAULT);

            Buf::Copy(modifiedImage, _imageDisp);

            return 0;
            }
         catch(const AIOLException& exception)
            {
            // User-defined event/processing functions should always handle their own exceptions.
            Os::Printf(AilText("Encountered an exception during the hook.\n"));
            Os::Printf(AilText("The function %s threw the following exception:\n"), exception.Function().c_str());
            Os::Printf(AilText("   %s\n\n"), exception.Message().c_str());
            Os::Printf(AilText("Press any key to exit. \n"));
            _exceptionInProcess = true;
            return 1;
            }
         }

      void Run()
         {
         Os::Printf(AilText("\n"));
         Os::Printf(AilText("CLASSIC <-> OBJECT API:\n"));
         Os::Printf(AilText("-----------------------\n\n"));
         Os::Printf(AilText("This example shows the interoperability\n"));
         Os::Printf(AilText("between the object API and the classic API.\n"));
         Os::Printf(AilText("The image acquisition uses the object API and the\n"));
         Os::Printf(AilText("image processing is performed using the classic API.\n"));
         Os::Printf(AilText("\n"));
         Os::Printf(AilText("This code is useful to transition existing code\n"));
         Os::Printf(AilText("towards the object API or to enable the use of a\n"));
         Os::Printf(AilText("functionality not yet available in the object API.\n"));
         Os::Printf(AilText("\n"));

         // Enable the display's overlay for processing.
         _display.Overlay.Enabled(Disp::Overlay::Enable);

         // Register the hook function.
         _digitizer.Process.Register(&HybridApiExample::ProcessingFunction, *this);

         // Start the processing. The processing function is called with every frame grabbed.
         _digitizer.Process.Start(_ailGrabBufferList);

         // Here the main() is free to perform other tasks while the processing is executing.
         // ---------------------------------------------------------------------------------

         // Print a message and wait for a key press after a minimum number of frames.
         Os::Printf(AilText("Press any key to stop.\n\n"));
         while(!Os::Kbhit() && !_exceptionInProcess);

         // Stop the processing.
         _digitizer.Process.Stop();
         Os::Getch();

         // Return if an exception is encountered during processing.
         if(_exceptionInProcess)
            {
            return;
            }

         // Print statistics.
         auto ProcessFrameCount = _digitizer.ProcessFrameCount();
         auto ProcessFrameRate = _digitizer.ProcessFrameRate();
         Os::Printf(AilText("\n\n%d frames grabbed at %.1f frames/sec (%.1f ms/frame).\n"),
                    ProcessFrameCount, ProcessFrameRate, 1000.0 / ProcessFrameRate);
         Os::Printf(AilText("Press any key to end.\n"));
         Os::Getch();
         }
   };

int MosMain()
   {
   HybridApiExample example;
   try
      {
      example.Run();
      }
   catch(const AIOLException& exception)
      {
      Os::Printf(AilText("Encountered an exception during the example.\n"));
      Os::Printf(AilText("The function %s threw the following exception:\n"), exception.Function().c_str());
      Os::Printf(AilText("   %s\n\n"), exception.Message().c_str());
      Os::Printf(AilText("Press any key to exit. \n"));
      Os::Getch();
      }
   return 0;
   }

```
