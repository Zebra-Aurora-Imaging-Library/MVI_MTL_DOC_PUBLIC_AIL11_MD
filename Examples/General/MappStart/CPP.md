---
title: "MappStart"
description: "This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors."
ms.language: "C++"
---

# MappStart

> This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors.

**Language:** C++

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
#include <AIL.h>

int MosMain(void)
{
   AIL_ID AilApplication,      // Application identifier.    
          AilSystem,           // System identifier.         
          AilDisplay,          // Display identifier.        
          AilImage,            // Image buffer identifier.   
          AilGraphicContextId; // Graphic context identifier.  

   // Allocate a default application, system, display and image. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, 
                                       &AilDisplay, M_NULL, &AilImage);

   // Allocate a graphic context 
   MgraAlloc(AilSystem,  &AilGraphicContextId);

   // Perform graphic operations in the display image. 
   MgraControl(AilGraphicContextId, M_COLOR, 0xF0);
   MgraFont(AilGraphicContextId, M_FONT_DEFAULT_LARGE);
   MgraText(AilGraphicContextId, AilImage, 120L, 20L, AIL_TEXT("Aurora Imaging Library"));
    
   MgraControl(AilGraphicContextId,M_FONT_SIZE, 12);
   MgraFont(AilGraphicContextId, M_FONT_DEFAULT_TTF);
   MgraText(AilGraphicContextId, AilImage, 40L, 80L, AIL_TEXT("English"));

   MappControl(M_DEFAULT, M_ERROR, M_PRINT_DISABLE);

   MgraControl(AilGraphicContextId,M_FONT_SIZE, 16);
   MgraFont(AilGraphicContextId, M_FONT_DEFAULT_TTF AIL_TEXT(":Bold"));
   MgraText(AilGraphicContextId, AilImage, 40L, 140L, AIL_TEXT("Français"));

   MgraControl(AilGraphicContextId,M_FONT_SIZE, 24);
   MgraFont(AilGraphicContextId, M_FONT_DEFAULT_TTF AIL_TEXT(":Italic"));
   MgraText(AilGraphicContextId, AilImage, 40L, 220L, AIL_TEXT("Italiano"));

   MgraControl(AilGraphicContextId,M_FONT_SIZE, 30);
   MgraFont(AilGraphicContextId, 
               M_FONT_DEFAULT_TTF AIL_TEXT(":Bold:Italic"));
   MgraText(AilGraphicContextId, AilImage, 40L ,300L, AIL_TEXT("Deutsch"));

   // Trying OS specific font. 
   MgraControl(AilGraphicContextId,M_FONT_SIZE, 36);

   // Check the type of OS where the code is executing
   // Note: Distributed Aurora Imaging Library supports clusters of Linux and Windows PCs
   //       so the remote PC could be running a different operating system
   AIL_ID OwnerApplication = M_NULL;
   MsysInquire(AilSystem, M_OWNER_APPLICATION, &OwnerApplication);

   if(MappInquire(OwnerApplication, M_PLATFORM_OS_TYPE, M_NULL) == M_OS_LINUX)
      MgraFont(AilGraphicContextId, AIL_TEXT("Monospace"));
   else
      MgraFont(AilGraphicContextId, AIL_TEXT("Courier New"));
   MgraText(AilGraphicContextId, AilImage, 40L ,380L, AIL_TEXT("Español"));


#if M_AIL_USE_TTF_UNICODE
   // Draw Greek, Japanese and Korean 
   MgraFont(AilGraphicContextId, M_FONT_DEFAULT_TTF);
   MgraControl(AilGraphicContextId,M_FONT_SIZE, 12);
   MgraText(AilGraphicContextId, AilImage, 400L,  80L, AIL_TEXT("ελληνιδ"));
      
   MgraControl(AilGraphicContextId,M_FONT_SIZE, 16);
   MgraText(AilGraphicContextId, AilImage, 400L, 140L, AIL_TEXT("日本語"));

   MgraControl(AilGraphicContextId,M_FONT_SIZE, 24);
   MgraText(AilGraphicContextId, AilImage, 400L,  220L, AIL_TEXT("한국어"));
      
   // Draw Arabic and Hebrew 
   MgraControl(AilGraphicContextId,M_TEXT_DIRECTION, M_RIGHT_TO_LEFT);
   MgraControl(AilGraphicContextId,M_FONT_SIZE, 30);
   MgraText(AilGraphicContextId, AilImage, 400L, 320L, AIL_TEXT("עברית"));
      
   MgraControl(AilGraphicContextId,M_FONT_SIZE, 36);
   MgraText(AilGraphicContextId, AilImage, 400L,  380L, AIL_TEXT("ﻋﺮﺑﻲ"));
 #endif     
   
   // Print a message. 
   MosPrintf(AIL_TEXT("\nINTERNATIONAL TEXT ANNOTATION:\n"));
   MosPrintf(AIL_TEXT("------------------------------\n"));
   MosPrintf(AIL_TEXT("\nThis example demonstrates the support"));
   MosPrintf(AIL_TEXT(" of TrueType fonts by MgraText.\n\n"));

   if(MappGetError(M_DEFAULT, M_GLOBAL + M_SYNCHRONOUS, 0) != M_NULL_ERROR)
      MosPrintf(AIL_TEXT("Note: Some Unicode fonts are not available\n\n"));

   // Wait for a key press. 
   MosPrintf(AIL_TEXT("Press any key to end.\n"));
   MosGetch();

   // Free Graphic Context. 
   MgraFree(AilGraphicContextId);
     
   // Free defaults. 
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, AilImage);

   return 0;
}


```
