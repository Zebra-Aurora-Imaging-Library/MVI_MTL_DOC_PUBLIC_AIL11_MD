---
title: "MappStart"
description: "This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors."
ms.language: "Python Object"
---

# MappStart

> This program allocates an AIL application and system, then displays messages using truetype fonts. It also shows how to check for errors.

**Language:** Python Object

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

from ZebraAuroraImagingObjectLibrary11 import *

class MappStartExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._graphicContext = Gra.Context(self._system)
      self._display = Disp.Display(self._system)
      self._imageDisp = Buf.Image(self._system, 1, 640, 480, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDisp)

   def __enter__(self):
      self._imageDisp.Clear(Color8.Black)
      self._display.Select(self._imageDisp)
      return self

   def Run(self):
      self._graphicContext.Color = 0xF0
      self._graphicContext.SetFont(Gra.FontName.DefaultLarge)
      self._graphicContext.DrawString(self._imageDisp, 120, 20, "Aurora Imaging Library")

      self._graphicContext.Font.Size = 12
      self._graphicContext.SetFont("AILFont")
      self._graphicContext.DrawString(self._imageDisp, 40, 80, "English")

      self._graphicContext.Font.Size = 16
      self._graphicContext.SetFont("AILFont" + ":Bold")
      self._graphicContext.DrawString(self._imageDisp, 40, 140, "Français")

      self._graphicContext.Font.Size = 24
      self._graphicContext.SetFont("AILFont" + ":Italic")
      self._graphicContext.DrawString(self._imageDisp, 40, 220, "Italiano")

      self._graphicContext.Font.Size = 30
      self._graphicContext.SetFont("AILFont" + ":Bold:Italic")
      self._graphicContext.DrawString(self._imageDisp, 40, 300, "Deutsch")


      # Print a message.
      print("\nINTERNATIONAL TEXT ANNOTATION:")
      print("------------------------------")
      print("\nThis example demonstrates the support of TrueType fonts by MgraText.\n")

      missingFont = False

      try:
         self._graphicContext.Font.Size = 36
         if self._system.OwnerApplication.PlatformOsType == App.PlatformOsType.Linux:
            self._graphicContext.SetFont("Monospace")
         else:
            self._graphicContext.SetFont("Courier New")
         self._graphicContext.DrawString(self._imageDisp, 40, 380, "Español")
      except:
         missingFont = True

      try:
         # Draw Greek, Japanese and Korean
         self._graphicContext.SetFont("AILFont")

         self._graphicContext.Font.Size = 12
         self._graphicContext.DrawString(self._imageDisp, 400, 80, "ελληνιδ")
      except:
         missingFont = True

      try:
         self._graphicContext.Font.Size = 16
         self._graphicContext.DrawString(self._imageDisp, 400, 140, "日本語")
      except:
         missingFont = True

      try:
         self._graphicContext.Font.Size = 24
         self._graphicContext.DrawString(self._imageDisp, 400, 220, "한국어")
      except:
         missingFont = True

      try:
         # Draw Arabic and Hebrew
         self._graphicContext.Text.Direction = Gra.TextDirection.RightToLeft

         self._graphicContext.Font.Size = 30
         self._graphicContext.DrawString(self._imageDisp, 400, 320, "עברית")
      except:
         missingFont = True

      try:
         self._graphicContext.Font.Size = 36
         self._graphicContext.DrawString(self._imageDisp, 400, 380, "ﻋﺮﺑﻲ")
      except:
         missingFont = True

      if missingFont:
         print("Note: Some Unicode fonts are not available\n")


      # Wait for a key press.
      print("Press any key to end.")
      Os.Getch()

   def __exit__(self, exc_type, exc_value, tb):
      self._display.Free() if self._display is not None else None
      self._graphicContext.Free() if self._graphicContext is not None else None
      self._imageDisp.Free() if self._imageDisp is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with MappStartExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print(exception)
      print("Press any key to exit.")
      Os.Getch()


```
