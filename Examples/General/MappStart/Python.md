---
title: "MappStart"
description: "This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors."
ms.language: "Python"
---

# MappStart

> This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors.

**Language:** Python

**Functions used:** `MgraAlloc`, `MappAlloc`, `MappControl`, `MappFree`, `MappGetError`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraControl`, `MgraFont`, `MgraFree`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, What's New, Older, Advanced, Board specific, DistributedAIL specific, Licenses, Hardwares, Languages, Notes

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: MAppStart.py
#
# Content:   This program allocates an application and system, then
#            displays messages using TrueType fonts. It also shows how to
#            check for errors.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#
##########################################################################

import ail11 as AIL

def MappStartExample():

   # Allocate a default application, system, display and image.
   AilApplication, AilSystem, AilDisplay, AilImage = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL)

   # Allocate a graphic context
   AilGraphicContextId = AIL.MgraAlloc(AilSystem)

   # Perform graphic operations in the display image
   AIL.MgraControl(AilGraphicContextId, AIL.M_COLOR, 0xF0)
   AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_LARGE)
   AIL.MgraText(AilGraphicContextId, AilImage, 120, 20, "Aurora Imaging Library")

   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 12)
   AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF)
   AIL.MgraText(AilGraphicContextId, AilImage, 40, 80, "English")

   AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE)

   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 16)
   AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF + ":Bold")
   AIL.MgraText(AilGraphicContextId, AilImage, 40, 140, "Français")

   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 24)
   AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF + ":Italic")
   AIL.MgraText(AilGraphicContextId, AilImage, 40, 220, "Italiano")

   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 30)
   AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF + ":Bold:Italic")
   AIL.MgraText(AilGraphicContextId, AilImage, 40, 300, "Deutsch")

   # Trying OS specific font
   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 36)

   # Check the type of OS where the code is executing
   OwnerApplication = AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION)
   if AIL.MappInquire(OwnerApplication, AIL.M_PLATFORM_OS_TYPE) == AIL.M_OS_LINUX:
      AIL.MgraFont(AilGraphicContextId, "Monospace")
   else:
      AIL.MgraFont(AilGraphicContextId, "Courier New")
   AIL.MgraText(AilGraphicContextId, AilImage, 40, 380, "Español")

   # Assuming TTF Unicode support is available in the Python API
   AIL.MgraFont(AilGraphicContextId, AIL.M_FONT_DEFAULT_TTF)
   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 12)
   AIL.MgraText(AilGraphicContextId, AilImage, 400, 80, "ελληνιδ")

   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 16)
   AIL.MgraText(AilGraphicContextId, AilImage, 400, 140, "日本語")

   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 24)
   AIL.MgraText(AilGraphicContextId, AilImage, 400, 220, "한국어")

   # Draw Arabic and Hebrew
   AIL.MgraControl(AilGraphicContextId, AIL.M_TEXT_DIRECTION, AIL.M_RIGHT_TO_LEFT)
   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 30)
   AIL.MgraText(AilGraphicContextId, AilImage, 400, 320, "עברית")

   AIL.MgraControl(AilGraphicContextId, AIL.M_FONT_SIZE, 36)
   AIL.MgraText(AilGraphicContextId, AilImage, 400, 380, "ﻋﺮﺑﻲ")

   # Print a message
   print()
   print("INTERNATIONAL TEXT ANNOTATION:")
   print("------------------------------")
   print()
   print("This example demonstrates the support of TrueType fonts by MgraText.")
   print()
   
   if AIL.MappGetError(AIL.M_DEFAULT, AIL.M_GLOBAL + AIL.M_SYNCHRONOUS, 0) != AIL.M_NULL_ERROR:
      print("Note: Some Unicode fonts are not available\n")

   # Wait for a key press.
   print("Press any key to end.")
   AIL.MosGetch()

   # Free Graphic Context
   AIL.MgraFree(AilGraphicContextId)
   # Free defaults.
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AilImage)
   
   return 0


if __name__ == "__main__":
   MappStartExample()

```
