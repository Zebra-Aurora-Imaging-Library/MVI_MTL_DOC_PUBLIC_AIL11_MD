---
title: "MdigGrab"
description: "This program demonstrates how to grab from a camera in continuous and monoshot mode."
ms.language: "C++ Object"
---

# MdigGrab

> This program demonstrates how to grab from a camera in continuous and monoshot mode.

**Language:** C++ Object

**Functions used:** `MdispAlloc`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGrab`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MDigGrab.cpp
//
// Description: This program demonstrates how to grab from a camera in
//              continuousand monoshot mode.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
//////////////////////////////////////////////////////////////////////////////

#include <AIOL.h>
using namespace Zebra::AuroraImagingObjectLibrary;

class MdigGrabExample
   {
   public:
      MdigGrabExample()
         : _application(App::AllocInitFlags::Default)
         , _system(_application)
         , _digitizer(_system)
         , _imageDisp(_system, _digitizer.SizeBand(), _digitizer.SizeX(), _digitizer.SizeY(), Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcDispGrab)
         , _display(_system)
         {
         _imageDisp.Clear(Color8::Black);

         //Display the image buffer.
         _display.Select(_imageDisp);
         }

      ~MdigGrabExample() = default;

      void Run()
         {
         // Grab continuously.
         _digitizer.GrabContinuous(_imageDisp);

         // When a key is pressed, halt.
         Os::Printf(AilText("\nDIGITIZER ACQUISITION:\n"));
         Os::Printf(AilText("----------------------\n\n"));
         Os::Printf(AilText("Continuous image grab in progress.\n"));
         Os::Printf(AilText("Press any key to stop.\n\n"));
         Os::Getch();

         // Stop continuous grab.
         _digitizer.Halt();

         // Pause to show the result.
         Os::Printf(AilText("Continuous grab stopped.\n\n"));
         Os::Printf(AilText("Press any key to do a single image grab.\n\n"));
         Os::Getch();

         // Monoshot grab.
         _digitizer.Grab(_imageDisp);

         // Pause to show the result.
         Os::Printf(AilText("Displaying the grabbed image.\n"));
         Os::Printf(AilText("Press any key to end.\n\n"));
         Os::Getch();
         }

   private:
      App::Application _application;
      Sys::System _system;
      Dig::Digitizer _digitizer;
      Buf::Image _imageDisp;
      Disp::Display _display;
   };


int MosMain()
   {
   try
      {
      MdigGrabExample example;
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
