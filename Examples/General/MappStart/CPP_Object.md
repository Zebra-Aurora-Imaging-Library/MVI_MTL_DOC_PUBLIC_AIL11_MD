---
title: "MappStart"
description: "This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors."
ms.language: "C++ Object"
---

# MappStart

> This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors.

**Language:** C++ Object

**Functions used:** `MgraAlloc`, `MappAlloc`, `MappControl`, `MappFree`, `MappGetError`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraControl`, `MgraFont`, `MgraFree`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, What's New, Older, Advanced, Board specific, DistributedAIL specific, Licenses, Hardwares, Languages, Notes

```cpp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MAppStart.cpp
//
// Description: This program allocates an application and system, then displays
//              messages using TrueType fonts. It also shows how to check for errors.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIOL.h>
using namespace Zebra::AuroraImagingObjectLibrary;

class MappStartExample
   {
   public:
      MappStartExample()
         : _application(App::AllocInitFlags::Default)
         , _system(_application)
         , _graContext(_system)
         , _imageDisp(_system, 1, 640, 480, Buf::AllocType::Unsigned8, Buf::AllocAttributes::ProcDispGrab)
         , _display(_system)
         {
         _imageDisp.Clear(Color8::Black);
         _display.Select(_imageDisp);
         }

      ~MappStartExample() = default;

      void Run()
         {
         _graContext.Color(0xF0);
         _graContext.SetFont(Gra::FontName::DefaultLarge);
         _graContext.DrawString(_imageDisp, 120L, 20L, AilText("Aurora Imaging Library"));

         _graContext.Font.Size(12);
         _graContext.SetFont(AilText("AILFont"));
         _graContext.DrawString(_imageDisp, 40L, 80L, AilText("English"));

         _graContext.Font.Size(16);
         _graContext.SetFont(AilText("AILFont") AilText(":Bold"));
         _graContext.DrawString(_imageDisp, 40L, 140L, AilText("Français"));

         _graContext.Font.Size(24);
         _graContext.SetFont(AilText("AILFont") AilText(":Italic"));
         _graContext.DrawString(_imageDisp, 40L, 220L, AilText("Italiano"));

         _graContext.Font.Size(30);
         _graContext.SetFont(AilText("AILFont") AilText(":Bold:Italic"));
         _graContext.DrawString(_imageDisp, 40L, 300L, AilText("Deutsch"));

         _graContext.Font.Size(36);
         if (_system.OwnerApplication().PlatformOsType() == App::PlatformOsType::Linux)
            _graContext.SetFont(AilText("Monospace"));
         else
            _graContext.SetFont(AilText("Courier New"));
         _graContext.DrawString(_imageDisp, 40L, 380L, AilText("Español"));

         // Print a message.
         Os::Printf(AilText("\nINTERNATIONAL TEXT ANNOTATION:\n"));
         Os::Printf(AilText("------------------------------\n"));
         Os::Printf(AilText("\nThis example demonstrates the support"));
         Os::Printf(AilText(" of TrueType fonts by MgraText.\n\n"));

         bool missingFont = false;
         try
            {
            // Draw Greek, Japanese and Korean
            _graContext.SetFont(AilText("AILFont"));

            _graContext.Font.Size(12);
            _graContext.DrawString(_imageDisp, 400L, 80L, AilText("ελληνιδ"));
            }
         catch(...)
            {
            missingFont = true;
            }

         try
            {
            _graContext.Font.Size(16);
            _graContext.DrawString(_imageDisp, 400L, 140L, AilText("日本語"));
            }
         catch(...)
            {
            missingFont = true;
            }

         try
            {
            _graContext.Font.Size(24);
            _graContext.DrawString(_imageDisp, 400L, 220L, AilText("한국어"));
            }
         catch(...)
            {
            missingFont = true;
            }

         try
            {
            // Draw Arabic and Hebrew
            _graContext.Text.Direction(Gra::TextDirection::RightToLeft);

            _graContext.Font.Size(30);
            _graContext.DrawString(_imageDisp, 400L, 320L, AilText("עברית"));
            }
         catch(...)
            {
            missingFont = true;
            }

         try
            {
            _graContext.Font.Size(36);
            _graContext.DrawString(_imageDisp, 400L, 380L, AilText("ﻋﺮﺑﻲ"));
            }
         catch(...)
            {
            missingFont = true;
            }

         if (missingFont)
            {
            Os::Printf(AilText("Note: Some Unicode fonts are not available\n\n"));
            }

         // Wait for a key press.
         Os::Printf(AilText("Press any key to end.\n"));
         Os::Getch();
         }

   private:
      App::Application _application;
      Sys::System _system;
      Gra::Context _graContext;
      Buf::Image _imageDisp;
      Disp::Display _display;
   };


int MosMain()
   {
   try
      {
      MappStartExample example;
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
