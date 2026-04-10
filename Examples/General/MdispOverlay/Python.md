---
title: "MdispOverlay"
description: "This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video."
ms.language: "Python"
---

# MdispOverlay

> This program shows how to display an image while creating text and graphics annotations on top of it using AIL graphic functions and windows GDI drawing under Windows. If the target system supports grabbing, the annotations are done on top of live video.

**Language:** Python

**Functions used:** `MbufClear`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAllocColor`, `MbufControl`, `MbufFree`, `MbufRestore`, `MbufInquire`, `MdigAlloc`, `MdigFree`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraControl`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: MDispOverlay.py
#
# Synopsis:  This program shows how to display an image while creating
#            text and graphics annotations on top of it using AIL
#            graphic functions and windows GDI drawing under Windows.
#            If the target system supports grabbing, the annotations
#            are done on top of a continuous grab.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################
 
import ail11 as AIL

# Target image.
IMAGE_FILE   = AIL.M_IMAGE_PATH + "Board.mim"

# Title for the display window.
WINDOW_TITLE = "Custom Title"

def MdispOverlayExample():

   # Allocate defaults.
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)
   AilDigitizer = None
   # If the system has a digitizer, use it.
   if AIL.MsysInquire(AilSystem, AIL.M_DIGITIZER_NUM):
      AilDigitizer = AIL.MdigAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT)
      AilImage = AIL.MbufAllocColor(AilSystem,
                                    AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_BAND),
                                    AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X),
                                    AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y),
                                    8 + AIL.M_UNSIGNED,
                                    AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC + AIL.M_GRAB)
      AIL.MbufClear(AilImage, 0)
   
   else:
      # Restore a static image.
      AilImage = AIL.MbufRestore(IMAGE_FILE, AilSystem)

   # Change display window title.
   AIL.MdispControl(AilDisplay, AIL.M_TITLE, WINDOW_TITLE)

   # Display the image buffer.
   AIL.MdispSelect(AilDisplay, AilImage)

   # Draw text and graphis annotations in the display overlay.
   OverlayDraw(AilDisplay)

   # If the system supports it, grab continuously in the displayed image. 
   if (AilDigitizer):
      AIL.MdigGrabContinuous(AilDigitizer, AilImage)

   # Pause to show the image.
   print("\nOVERLAY ANNOTATIONS:")
   print("--------------------\n")
   print("Displaying an image with overlay annotations.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Stop the continuous grab and free digitizer if needed.
   if (AilDigitizer):
      AIL.MdigHalt(AilDigitizer)
      AIL.MdigFree(AilDigitizer)

      # Pause to show the result. 
      print("Displaying the last grabbed image.")
      print("Press any key to end.\n")
      AIL.MosGetch()

   # Free image.
   AIL.MbufFree(AilImage)   

   # Free default allocations.
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0

# --------------------------------------------------------------- 
# Name:      OverlayDraw
# Synopsis:  This function draws annotations in the display overlay.

def OverlayDraw(AilDisplay):

   # Prepare overlay buffer. #
   ###########################

   # Enable the display of the overlay annotations.
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)

   # Inquire the overlay buffer associated with the display. 
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)

   # Clear the overlay to transparent. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)

   # Disable the overlay display update to accelerate annotations. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_SHOW, AIL.M_DISABLE)

   # Inquire overlay size. 
   ImageWidth  = AIL.MbufInquire(AilOverlayImage, AIL.M_SIZE_X)
   ImageHeight = AIL.MbufInquire(AilOverlayImage, AIL.M_SIZE_Y)

   # Draw overlay annotations. #
   #################################

   # Set the graphic text background to transparent. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKGROUND_MODE, AIL.M_TRANSPARENT)

   # Print a white string in the overlay image buffer. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_WHITE)
   AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, ImageWidth/9, ImageHeight/5,    " -------------------- ")
   AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, ImageWidth/9, ImageHeight/5+25, " - overlay Text - ")
   AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, ImageWidth/9, ImageHeight/5+50, " -------------------- ")

   # Print a green string in the overlay image buffer. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN)
   AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, ImageWidth*11/18, ImageHeight/5,    " ---------------------")
   AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, ImageWidth*11/18, ImageHeight/5+25, " - overlay Text - ")
   AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, ImageWidth*11/18, ImageHeight/5+50, " ---------------------")

   # Re-enable the overlay display after all annotations are done. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_SHOW, AIL.M_ENABLE)

   try:
      import win32ui
      import win32api
      import win32con

      # Draw GDI color overlay annotation #
      #####################################

      # Disable error printing for the inquire might not be supported.
      AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE)

      # Create a device context to draw in the overlay buffer with GDI. 
      AIL.MbufControl(AilOverlayImage, AIL.M_DC_ALLOC, AIL.M_DEFAULT)
      # Inquire the device context. 
      hCustomDC = win32ui.CreateDCFromHandle(AIL.MbufInquire(AilOverlayImage, AIL.M_DC_HANDLE))

      # Re-enable error printing. 
      AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE)

      # Perform operation if GDI drawing is supported. 
      if hCustomDC:
         Hor = [(0, int(ImageHeight / 2)), (int(ImageWidth), int(ImageHeight / 2))]
         Ver = [(int(ImageWidth / 2), 0), (int(ImageWidth / 2), int(ImageHeight))]

         # Draw a blue cross.
         hpen = win32ui.CreatePen(win32con.PS_SOLID, 1, win32api.RGB(0, 0, 255))
         hpenOld = hCustomDC.SelectObject(hpen)

         hCustomDC.Polyline(Hor)
         hCustomDC.Polyline(Ver)

         hCustomDC.SelectObject(hpenOld)

         # Prepare transparent text annotations. 
         hCustomDC.SetBkMode(win32con.TRANSPARENT)
         chText = "GDI Overlay Text"
         (x, y) = hCustomDC.GetTextExtentPoint(chText)

         # Write red text.
         Txt = (int(ImageWidth*3/18), int(ImageHeight*17/24), int(ImageWidth*3/18) + x, int(ImageHeight*17/24) + y)
         hCustomDC.SetTextColor(win32api.RGB(255, 0, 0))
         hCustomDC.DrawText(chText, Txt, win32con.DT_RIGHT)

         # Write yellow text.
         Txt = (int(ImageWidth*12/18), int(ImageHeight*17/24), int(ImageWidth*12/18) + x, int(ImageHeight*17/24) + y)
         hCustomDC.SetTextColor(win32api.RGB(255, 255, 0))
         hCustomDC.DrawText(chText, Txt, win32con.DT_RIGHT)

         # Delete device context. 
         AIL.MbufControl(AilOverlayImage, AIL.M_DC_FREE, AIL.M_DEFAULT)

         # Signal that the overlay buffer was modified. 
         AIL.MbufControl(AilOverlayImage, AIL.M_MODIFIED, AIL.M_DEFAULT)
   except:
      pass
      # GDI drawing is not supported

if __name__ == "__main__":
   MdispOverlayExample()

```
