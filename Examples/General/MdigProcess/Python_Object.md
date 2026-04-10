---
title: "MdigProcess"
description: "This program shows the use of the MdigProcess() function to perform real-time processing."
ms.language: "Python Object"
---

# MdigProcess

> This program shows the use of the MdigProcess() function to perform real-time processing.

**Language:** Python Object

**Functions used:** `MdigGrabContinuous`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MimArith`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
#
#  File name: MdigProcess.py
#
#   Synopsis:  This program shows the use of the MdigProcess() function and its multiple
#              buffering acquisition to do robust real-time processing.
#
#              The user's processing code to execute is located in a callback function
#              that will be called for each frame acquired (see ProcessingFunction()).
#
#        Note: The average processing time must be shorter than the grab time or some
#              frames will be missed. Also, if the processing results are not displayed
#              and the frame count is not drawn or printed, the CPU usage is reduced
#              significantly.
#
#  (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
#  All Rights Reserved

from ZebraAuroraImagingObjectLibrary11 import *

def ProcessingFunction(sender, e):
   try:
      stringPosX = 20
      stringPosY = 20

      modifiedBuffer = e.ModifiedBuffer

      # Increment the frame counter.
      sender._nbGrabbedFrames = sender._nbGrabbedFrames + 1

      # Print and draw the frame count (remove to reduce CPU usage).
      print("Processing frame #{:d}.\r".format(sender._nbGrabbedFrames), end='')

      sender._graphicContext.DrawString(modifiedBuffer, stringPosX, stringPosY, str(sender._nbGrabbedFrames))

      # Execute the processing and update the display.
      Im.Arith.Not(modifiedBuffer, sender._imageDisp)

   except AIOLException as exception:
      # User-defined event/processing functions should always handle their own exceptions
      print("Encountered an exception during the hook.")
      print(exception)
      print("Press any key to exit.")
      sender._exceptionInProcess = True

class MdigProcessExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._digitizer = Dig.Digitizer(self._system)
      self._imageDisp = Buf.Image(self._system, 1, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab)
      self._display = Disp.Display(self._system)
      self._graphicContext = Gra.Context(self._system)

      self._nbGrabImageMax = 20
      self._nbGrabbedFrames = 0
      self._exceptionInProcess = False
      
      self._ailGrabBufferList = []
      for i in range(self._nbGrabImageMax):
         imageBuf = Buf.Image(self._system, 1, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcGrab)
         imageBuf.Clear(Color8.Black)
         self._ailGrabBufferList.append(imageBuf)

   def __enter__(self):
      self._imageDisp.Clear(Color8.Black)
      self._display.Select(self._imageDisp)

      return self

   def Run(self):
         # Register Hook function
         self._digitizer.Process.Register(ProcessingFunction, self)

         # Print a message
         print("\nMULTIPLE BUFFERED PROCESSING.")
         print("-----------------------------\n")
         print("Press any key to start processing.\n")

         # Grab continuously on the display and wait for a key press.
         self._digitizer.GrabContinuous(self._imageDisp)
         Os.Getch()

         # Halt continuous grab.
         self._digitizer.Halt()

         # Start the processing. The processing function is called with every frame grabbed.
         self._digitizer.Process.Start(self._ailGrabBufferList)

         # Here the main() is free to perform other tasks while the processing is executing.
         # ---------------------------------------------------------------------------------

         # Print a message and wait for a key press after a minimum number of frames.
         print("Press any key to stop.\n")
         while not Os.Kbhit() and not self._exceptionInProcess:
            pass

         # Stop the processing.
         self._digitizer.Process.Stop()
         Os.Getch()

         # Return if we encountered an exception during processing.
         if self._exceptionInProcess:
            return

         # Print statistics.
         processFrameCount = self._digitizer.ProcessFrameCount
         processFrameRate = self._digitizer.ProcessFrameRate
         print("\n\n{processFrameCount} frames grabbed at {processFrameRate:.1f} frames/sec ({rate:.1f} ms/frame)."
               .format(processFrameCount=processFrameCount, processFrameRate=processFrameRate, rate=1000.0 / processFrameRate))
         print("Press any key to end.\n")
         Os.Getch()

   def __exit__(self, exc_type, exc_value, tb):
      for buffer in self._ailGrabBufferList:
         buffer.Free()
      self._imageDisp.Free() if self._imageDisp is not None else None
      self._display.Free() if self._display is not None else None
      self._graphicContext.Free() if self._graphicContext is not None else None
      self._digitizer.Free() if self._digitizer is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with MdigProcessExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print(exception)
      print("Press any key to exit.")
      Os.Getch()

```
