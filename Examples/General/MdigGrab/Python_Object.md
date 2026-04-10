---
title: "MdigGrab"
description: "This program demonstrates how to grab from a camera in continuous and monoshot mode."
ms.language: "Python Object"
---

# MdigGrab

> This program demonstrates how to grab from a camera in continuous and monoshot mode.

**Language:** Python Object

**Functions used:** `MdispAlloc`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGrab`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#########################################################################################
#
# File name  : MdigGrab.py
#
# Content    :  This program demonstrates how to grab from a camera in
#               continuous and monoshot mode.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#
#########################################################################################

from ZebraAuroraImagingObjectLibrary11 import *

class MdigGrabExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._digitizer = Dig.Digitizer(self._system)
      self._imageDisp = Buf.Image(self._system, self._digitizer.SizeBand, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab)
      self._display = Disp.Display(self._system)

   def __enter__(self):
      self._imageDisp.Clear(Color8.Black)
      self._display.Select(self._imageDisp)
      return self

   def Run(self):
      # Grab continuously.
      self._digitizer.GrabContinuous(self._imageDisp)

      # When a key is pressed, halt.
      print("\nDIGITIZER ACQUISITION:")
      print("----------------------\n")
      print("Continuous image grab in progress.")
      print("Press any key to stop.\n")
      Os.Getch()

      # Stop continuous grab
      self._digitizer.Halt()

      # Pause to show the result.
      print("Continuous grab stopped.\n")
      print("Press any key to do a single image grab.\n")
      Os.Getch()

      # Monoshot grab.
      self._digitizer.Grab(self._imageDisp)

      # Pause to show the result.
      print("Displaying the grabbed image.")
      print("Press any key to end.\n")
      Os.Getch()

   def __exit__(self, exc_type, exc_value, tb):
      self._display.Free() if self._display is not None else None
      self._imageDisp.Free() if self._imageDisp is not None else None
      self._digitizer.Free() if self._digitizer is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with MdigGrabExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print(exception)
      print("Press any key to exit.")
      Os.Getch()

```
