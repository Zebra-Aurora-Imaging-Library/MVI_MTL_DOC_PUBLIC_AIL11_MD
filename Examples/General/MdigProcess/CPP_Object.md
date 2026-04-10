---
title: "MdigProcess"
description: "This program shows the use of the MdigProcess() function to perform real-time processing."
ms.language: "C++ Object"
---

# MdigProcess

> This program shows the use of the MdigProcess() function to perform real-time processing.

**Language:** C++ Object

**Functions used:** `MdigGrabContinuous`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MimArith`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdigProcess.cpp
//
// Description: This program shows the use of the MdigProcess() function and its multiple
//              buffering acquisition to do robust real-time processing.
//
//              The user's processing code to execute is located in a callback function
//              that will be called for each frame acquired (see ProcessingFunction()).
//
// Note:        The average processing time must be shorter than the grab time or some
//              frames will be missed. Also, if the processing results are not displayed
//              and the frame count is not drawn or printed, the CPU usage is reduced
//              significantly.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIOL.h>
using namespace Zebra::AuroraImagingObjectLibrary;

class MdigProcessExample
   {
   private:
      static const int _bufferingSizeMax = 20;
      int _processedImageCount = 0;
      bool _exceptionInProcess = false;

      App::Application _application;
      Sys::System _system;
      Dig::Digitizer _digitizer;
      Disp::Display _display;
      Gra::Context _graphicContext;
      Buf::Image _imageDisp;
      std::vector<Buf::Image> _ailGrabBufferList;

   public:
      MdigProcessExample()
         : _application(App::AllocInitFlags::Default)
         , _system(_application)
         , _digitizer(_system)
         , _display(_system)
         , _graphicContext(_system)

         , _imageDisp(_system, 1, _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcDispGrab)
         {
         for(int i = 0; i < _bufferingSizeMax; i++)
            {
            _ailGrabBufferList.emplace_back(_system, 1, _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcGrab);
            _ailGrabBufferList.back().Clear(Color8::Black);
            }
         _imageDisp.Clear(Color8::Black);
         _display.Select(_imageDisp);
         }

      ~MdigProcessExample() = default;

      int64_t ProcessingFunction(const Dig::ProcessEventArgs& Event)
         {
         try
            {
            const auto stringPosX = 20;
            const auto stringPosY = 20;

            Buf::Image modifiedBuffer = Event.ModifiedBuffer();

            // Increment the frame counter.
            _processedImageCount++;

            // Print and draw the frame count (remove to reduce CPU usage).
            Os::Printf(AilText("Processing frame #%d.\r"), _processedImageCount);

            ailstring text = AilToString(_processedImageCount);
            _graphicContext.DrawString(modifiedBuffer, stringPosX, stringPosY, text);

            // Execute the processing and update the display.
            Im::Arith::Not(modifiedBuffer, _imageDisp);

            return 0;
            }
         catch(const AIOLException& exception)
            {
            // User-defined event/processing functions should always handle their own exceptions
            Os::Printf(AilText("Encountered an exception during the hook.\n"));
            Os::Printf(AilText("%s \n"), exception.Message().c_str());
            Os::Printf(AilText("Press any key to exit. \n"));
            _exceptionInProcess = true;
            return 1;
            }
         }

      void Run()
         {
         // Register Hook function
         _digitizer.Process.Register(&MdigProcessExample::ProcessingFunction, *this);

         // Print a message
         Os::Printf(AilText("\nMULTIPLE BUFFERED PROCESSING.\n"));
         Os::Printf(AilText("-----------------------------\n\n"));
         Os::Printf(AilText("Press any key to start processing.\n\n"));

         // Grab continuously on the display and wait for a key press.
         _digitizer.GrabContinuous(_imageDisp);
         Os::Getch();

         // Halt continuous grab.
         _digitizer.Halt();

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

         // Return if we encountered an exception during processing.
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
   try
      {
      MdigProcessExample example;
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
