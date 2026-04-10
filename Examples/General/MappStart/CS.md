---
title: "MappStart"
description: "This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors."
ms.language: "C#"
---

# MappStart

> This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors.

**Language:** C#

**Functions used:** `MgraAlloc`, `MappAlloc`, `MappControl`, `MappFree`, `MappGetError`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraControl`, `MgraFont`, `MgraFree`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, What's New, Older, Advanced, Board specific, DistributedAIL specific, Licenses, Hardwares, Languages, Notes

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MappStart.cs
//
// Description: This program allocates an application and system, then displays
//              messages using TrueType fonts. It also shows how to check for errors.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;

using Zebra.AuroraImagingLibrary;

namespace MAppStart
    {
    class Program
    {
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;         // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;              // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;             // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;               // Image buffer identifier.
            AIL_ID AilGraphicContextId = AIL.M_NULL;    // Graphic context identifier

            // Allocate a default application, system, display and image.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, ref AilImage);

            // Allocate a graphic context
            AIL.MgraAlloc(AilSystem, ref AilGraphicContextId);

            // Perform graphic operations in the display image.
            AIL.MgraControl(AilGraphicContextId, AIL.M_COLOR, 0xF0);
            AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_LARGE);
            AIL.MgraText(AilGraphicContextId, AilImage, 120L, 20L, "Aurora Imaging Library");

            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 12);
            AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF);
            AIL.MgraText(AilGraphicContextId, AilImage, 40L, 80L, "English");

            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);

            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 16);
            AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF + ":Bold");
            AIL.MgraText(AilGraphicContextId, AilImage, 40L, 140L, "Français");

            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 24);
            AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF + ":Italic");
            AIL.MgraText(AilGraphicContextId, AilImage, 40L, 220L, "Italiano");

            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 30);
            AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF + ":Bold:Italic");
            AIL.MgraText(AilGraphicContextId, AilImage, 40L, 300L, "Deutsch");

            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 36);

            // Check the type of OS where the code is executing
            // Note: Distributed Aurora Imaging Library supports clusters of Linux and Windows PCs
            //       so the remote PC could be running a different operating system
            AIL_ID OwnerApplication = AIL.M_NULL;
            AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION, ref OwnerApplication);

            if (AIL.MappInquire(OwnerApplication, AIL.M_PLATFORM_OS_TYPE, AIL.M_NULL) == AIL.M_OS_LINUX)
               {
               AIL.MgraFont(AilGraphicContextId, "Monospace");
               }
            else
               {
               AIL.MgraFont(AilGraphicContextId, "Courier New");
               }
            AIL.MgraText(AilGraphicContextId, AilImage, 40L, 380L, "Español");

            // Draw Greek, Japanese and Korean
            AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF);

            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 12);
            AIL.MgraText(AilGraphicContextId, AilImage, 400L, 80L, "ελληνιδ");

            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 16);
            AIL.MgraText(AilGraphicContextId, AilImage, 400L, 140L, "日本語");

            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 24);
            AIL.MgraText(AilGraphicContextId, AilImage, 400L, 220L, "한국어");

            // Draw Arabic and Hebrew
            AIL.MgraControl(AilGraphicContextId, AIL.M_TEXT_DIRECTION, AIL.M_RIGHT_TO_LEFT);
            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 30);
            AIL.MgraText(AilGraphicContextId, AilImage, 400L, 320L, "עברית");

            AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 36);
            AIL.MgraText(AilGraphicContextId, AilImage, 400L, 380L, "ﻋﺮﺑﻲ");

            // Print a message.
            Console.WriteLine();
            Console.WriteLine("INTERNATIONAL TEXT ANNOTATION:");
            Console.WriteLine("------------------------------");
            Console.WriteLine();
            Console.WriteLine("This example demonstrates the support of TrueType fonts by MgraText.");
            Console.WriteLine();

            if (AIL.MappGetError(AIL.M_DEFAULT, AIL.M_GLOBAL + AIL.M_SYNCHRONOUS, AIL.M_NULL) != AIL.M_NULL_ERROR)
               {
                Console.WriteLine("Note: Some Unicode fonts are not available");
                Console.WriteLine();
               }

            // Wait for a key press.
            Console.WriteLine("Press any key to end.");
            Console.ReadKey(true);

            // Free Graphic Context
            AIL.MgraFree(AilGraphicContextId);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AilImage);
        }
    }
}

```
