---
title: "ObjectAPIInterop"
description: "This program shows the interoperability between the Object API and the Classic API. This can be used either to transition existing code progressively towards the Object API or to be able to use a functionality not yet available in the Object API."
ms.language: "Python Object"
---

# ObjectAPIInterop

> This program shows the interoperability between the Object API and the Classic API. This can be used either to transition existing code progressively towards the Object API or to be able to use a functionality not yet available in the Object API.

**Language:** Python Object

**Functions used:** `MappAlloc`, `MappFree`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobSelect`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAlloc`, `MgraControl`, `MimBinarize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Image Processing, What's New, AIL 11.0

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
#
#  File name: ObjectAPIInterop.py
#
#   Synopsis:  This program shows the interoperability between the object API
#              and the classic API.
#              This can be used either to transition existing code
#              progressively towards the object API or to enable the
#              use of a functionality not yet available in the object API.
#
#  (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
#  All Rights Reserved

from ZebraAuroraImagingObjectLibrary11 import *
import ail11 as AIL

def ProcessingFunction(sender, processEvent):
   try:
      # Increment the frame counter.
      sender._nbGrabbedFrames = sender._nbGrabbedFrames + 1

      # Print and draw the frame count (remove to reduce CPU usage).
      print("Processing frame #{:d}.\r".format(sender._nbGrabbedFrames), end='')
      
      # Grab the modified image.
      modifiedImage = processEvent.ModifiedBuffer

      # Allocate a binary image containing the data of the modified image.
      Buf.Copy(modifiedImage, sender._binImage)

      # The following code uses modules that are not yet available in the object API.
      # To use an object from the object API with the classic API, use its identifier (AIL_ID) in the classic function calls.

      # Binarize the image for processing.
      AIL.MimBinarize(sender._binImage.Id, sender._binImage.Id, AIL.M_DOMINANT + AIL.M_LESS_OR_EQUAL, AIL.M_NULL, AIL.M_NULL)

      # Set the blob context to the foreground value.
      AIL.MblobControl(sender._blobContextId, AIL.M_FOREGROUND_VALUE, AIL.M_ZERO)

      # Enable the desired features for the blob calculation.
      AIL.MblobControl(sender._blobContextId, AIL.M_MIN_AREA_BOX, AIL.M_ENABLE)

      # Calculate the blobs.
      AIL.MblobCalculate(sender._blobContextId, sender._binImage.Id, AIL.M_NULL, sender._blobResultId)

      # Select blobs based on area.
      AIL.MblobSelect(sender._blobResultId, AIL.M_INCLUDE_ONLY, AIL.M_AREA, AIL.M_GREATER_OR_EQUAL, 250, AIL.M_NULL)

      # Draw the selected blobs.
      sender._display.Overlay.Clear(Color8.Transparent)
      AIL.MblobDraw(sender._graphicContext.Id, sender._blobResultId, sender._display.Overlay.Image.Id, AIL.M_DRAW_BOX + AIL.M_DRAW_BOX_CENTER, AIL.M_INCLUDED_BLOBS, AIL.M_DEFAULT)

      Buf.Copy(modifiedImage, sender._imageDisp)

   except AIOLException as exception:
      # User-defined event/processing functions should always handle their own exceptions.
      print("Encountered an exception during the hook.")
      print("The function {} threw the following exception:".format(exception.Function))
      print("   {}\n".format(exception.Message))
      print("Press any key to exit.")
      sender._exceptionInProcess = True

class HybridApiExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._digitizer = Dig.Digitizer(self._system)
      self._imageDisp = Buf.Image(self._system, 1, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab)
      self._binImage = Buf.Image(self._system, 1, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab)
      self._display = Disp.Display(self._system)
      self._graphicContext = Gra.Context(self._system)
      
      # Allocate the classic API objects.
      self._blobContextId = AIL.MblobAlloc(self._system.Id, AIL.M_DEFAULT, AIL.M_DEFAULT)
      self._blobResultId = AIL.MblobAllocResult(self._system.Id, AIL.M_DEFAULT, AIL.M_DEFAULT)

      self._nbGrabImageMax = 20
      self._nbGrabbedFrames = 0
      self._exceptionInProcess = False
      
      self._ailGrabBufferList = []
      for i in range(self._nbGrabImageMax):
         imageBuf = Buf.Image(self._system, 1, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcGrab)
         imageBuf.Clear(Color8.Black)
         self._ailGrabBufferList.append(imageBuf)

   def __enter__(self):
      self._graphicContext.Color = Color8.Green
      self._imageDisp.Clear(Color8.Black)
      self._display.Select(self._imageDisp)

      return self

   def Run(self):
         print("");
         print("CLASSIC <-> OBJECT API:")
         print("-----------------------\n")
         print("This example shows the interoperability")
         print("between the object API and the classic API.")
         print("The image acquisition uses the object API and the")
         print("image processing is performed using the classic API.")
         print("")
         print("This code is useful to transition existing code")
         print("towards the object API or to enable the use of a")
         print("functionality not yet available in the object API.")
         print("")
         
         # Enable the display's overlay for processing.
         self._display.Overlay.Enabled = Disp.Overlay.Enable
         
         # Register the hook function.
         self._digitizer.Process.Register(ProcessingFunction, self)

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

         # Return if an exception is encountered during processing.
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
      # Free the classic API objects.
      AIL.MblobFree(self._blobContextId)
      AIL.MblobFree(self._blobResultId)
      
      for buffer in self._ailGrabBufferList:
         buffer.Free()
         
      self._imageDisp.Free() if self._imageDisp is not None else None
      self._binImage.Free() if self._binImage is not None else None
      self._display.Free() if self._display is not None else None
      self._graphicContext.Free() if self._graphicContext is not None else None
      self._digitizer.Free() if self._digitizer is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with HybridApiExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print("The function {} threw the following exception:".format(exception.Function))
      print("   {}\n".format(exception.Message))
      print("Press any key to exit.")
      Os.Getch()

```
