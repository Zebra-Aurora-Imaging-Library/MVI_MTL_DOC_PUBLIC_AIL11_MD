---
title: "MdispOverlay"
description: "This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video."
ms.language: "Python Object"
---

# MdispOverlay

> This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video.

**Language:** Python Object

**Functions used:** `MbufClear`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAllocColor`, `MbufControl`, `MbufFree`, `MbufRestore`, `MbufInquire`, `MdigAlloc`, `MdigFree`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraControl`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
############################################################################
#
# File name  : MDispOverlay.py
#
# Content    :  This program shows how to display an image while creating
#               text and graphics annotations on top of it using AIL
#               graphic functions and windows GDI drawing under Windows.
#               If the target system supports grabbing, the annotations
#               are done on top of a continuous grab.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#
############################################################################

from ZebraAuroraImagingObjectLibrary11 import *

class MdispOverlayExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._graphicContext = Gra.Context(self._system)
      self._digitizer = None
      self._windowTitle = "Custom Title"
      if self._system.DigitizerNum > 0:
         self._digitizer = Dig.Digitizer(self._system)
         self._imageDisp = Buf.Image(self._system, 3, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab)
         self._imageDisp.Clear(Color8.Black)
      else:
         self._imageFile = Paths.Images + "Board.mim"
         self._imageDisp = Buf.Image(self._imageFile, self._system)
      self._display = Disp.Display(self._system)

   def __enter__(self):
      self._display.Title = self._windowTitle
      self._display.Select(self._imageDisp)
      return self

   def OverlayDraw(self, display, graphicContext):
      # Prepare overlay buffer. #
      ###########################

      # Enable the display of the overlay annotations.
      display.Overlay.Enabled = Disp.Overlay.Enable

      # Inquire the overlay buffer associated with the display.
      display.Overlay.Show = Disp.OverlayShow.Disable

      # Clear the overlay to transparent.
      oWidth = display.Overlay.Image.SizeX
      oHeight = display.Overlay.Image.SizeY

      # Disable the overlay display update to accelerate annotations.
      display.Overlay.Show = Disp.OverlayShow.Disable

      # Inquire overlay size.
      oWidth = display.Overlay.Image.SizeX
      oHeight = display.Overlay.Image.SizeY

      # Draw overlay annotations. #
      #################################

      # Set the graphic text background to transparent.
      graphicContext.BackgroundMode = Gra.BackgroundMode.Transparent

      # Print a white string in the overlay image buffer.
      graphicContext.Color = Color8.White
      graphicContext.DrawString(display.Overlay.Image, oWidth / 9, oHeight / 5, " -------------------- ")
      graphicContext.DrawString(display.Overlay.Image, oWidth / 9, oHeight / 5 + 25, " - overlay Text - ")
      graphicContext.DrawString(display.Overlay.Image, oWidth / 9, oHeight / 5 + 50, " -------------------- ")

      # Print a green string in the overlay image buffer.
      graphicContext.Color = Color8.Green
      graphicContext.DrawString(display.Overlay.Image, oWidth * 11 / 18, oHeight / 5, " -------------------- ")
      graphicContext.DrawString(display.Overlay.Image, oWidth * 11 / 18, oHeight / 5 + 25, " - overlay Text - ")
      graphicContext.DrawString(display.Overlay.Image, oWidth * 11 / 18, oHeight / 5 + 50, " -------------------- ")

      # Re-enable the overlay display after all annotations are done.
      display.Overlay.Show = Disp.OverlayShow.Enable

      try:
         import win32ui
         import win32api
         import win32con

         # Draw GDI color overlay annotation #
         #####################################

         # Disable error printing for the inquire might not be supported.
         self._application.ErrorGlobalMode = App.ErrorMode.PrintDisable

         # Create a device context to draw in the overlay buffer with GDI.
         display.Overlay.Image.DcAlloc()

         # Inquire the device context.
         hCustomDC = win32ui.CreateDCFromHandle(display.Overlay.Image.DcHandle)

         # Re-enable error printing.
         self._application.ErrorGlobalMode = App.ErrorMode.PrintEnable

         # Perform operation if GDI drawing is supported.
         if hCustomDC:
            hor = [(0, int(oHeight / 2)), (int(oWidth), int(oHeight / 2))]
            ver = [(int(oWidth / 2), 0), (int(oWidth / 2), int(oHeight))]

            # Draw a blue cross.
            hpen = win32ui.CreatePen(win32con.PS_SOLID, 1, win32api.RGB(0, 0, 255))
            hpenOld = hCustomDC.SelectObject(hpen)

            hCustomDC.Polyline(hor)
            hCustomDC.Polyline(ver)

            hCustomDC.SelectObject(hpenOld)

            # Prepare transparent text annotations.
            hCustomDC.SetBkMode(win32con.TRANSPARENT)
            chText = "GDI Overlay Text"
            (x, y) = hCustomDC.GetTextExtentPoint(chText)

            # Write red text.
            txt = (int(oWidth*3/18), int(oHeight*17/24), int(oWidth*3/18) + x, int(oHeight*17/24) + y)
            hCustomDC.SetTextColor(win32api.RGB(255, 0, 0))
            hCustomDC.DrawText(chText, txt, win32con.DT_RIGHT)

            # Write yellow text.
            txt = (int(oWidth*12/18), int(oHeight*17/24), int(oWidth*12/18) + x, int(oHeight*17/24) + y)
            hCustomDC.SetTextColor(win32api.RGB(255, 255, 0))
            hCustomDC.DrawText(chText, txt, win32con.DT_RIGHT)

            # Delete device context.
            display.Overlay.Image.DcFree()

            # Signal that the overlay buffer was modified.
            display.Overlay.Image.Modified()
      except:
         pass
         # GDI drawing is not supported

   def Run(self):
      # Draw text and graphis annotations in the display overlay.
      self.OverlayDraw(self._display, self._graphicContext)

      # If the system supports it, grab continuously in the displayed image.
      if (self._digitizer):
         self._digitizer.GrabContinuous(self._imageDisp)

      # Pause to show the image.
      print("\nOVERLAY ANNOTATIONS:")
      print("--------------------\n")
      print("Displaying an image with overlay annotations.")
      print("Press any key to continue.\n")
      Os.Getch()

      # Stop the continuous grab and free digitizer if needed.
      if (self._digitizer):
         self._digitizer.Halt()

         # Pause to show the result.
         print("Displaying the last grabbed image.")
         print("Press any key to end.\n")
         Os.Getch()

   def __exit__(self, exc_type, exc_value, tb):
      self._display.Free() if self._display is not None else None
      self._graphicContext.Free() if self._graphicContext is not None else None
      self._imageDisp.Free() if self._imageDisp is not None else None
      self._digitizer.Free() if self._digitizer is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with MdispOverlayExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print(exception)
      print("Press any key to exit.")
      Os.Getch()

```
