---
title: "MimHistogram"
description: "This program loads an image of a tissue sample, calculates its intensity histogram and draws it."
ms.language: "Python"
---

# MimHistogram

> This program loads an image of a tissue sample, calculates its intensity histogram and draws it.

**Language:** Python

**Functions used:** `MsysFree`, `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraLines`, `MimAllocResult`, `MimFree`, `MimGetResult`, `MimHistogram`, `MsysAlloc`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: MimHistogram.py 
#
# Synopsis:  This program loads an image of a tissue sample, calculates its intensity
#            histogram and draws it.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
 
import ail11 as AIL

# Target image file specifications.
IMAGE_FILE = AIL.M_IMAGE_PATH + "Cell.mbufi"

# Number of possible pixel intensities.
HIST_NUM_INTENSITIES  = 256
HIST_SCALE_FACTOR     = 8
HIST_X_POSITION       = 250
HIST_Y_POSITION       = 450

def MimHistogramExample():
   XStart = []
   YStart = []
   XEnd = []
   YEnd = []

   AnnotationColor = AIL.M_COLOR_RED

   # Allocate a default application, system, display and image.
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Restore source image into an automatically allocated image buffer.
   AilImage = AIL.MbufRestore(IMAGE_FILE, AilSystem)
   
   # Display the image buffer and prepare for overlay annotations.
   AIL.MdispSelect(AilDisplay, AilImage)
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)
   
   # Allocate a histogram result buffer.
   HistResult = AIL.MimAllocResult(AilSystem, HIST_NUM_INTENSITIES, AIL.M_HIST_LIST)
   
   # Calculate the histogram.
   AIL.MimHistogram(AilImage, HistResult)
   
   # Get the results.
   HistValues = AIL.MimGetResult(HistResult, AIL.M_VALUE)

   # Draw the histogram in the overlay.
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor)
   for i in range(HIST_NUM_INTENSITIES):
      XStart.append(i+HIST_X_POSITION+1)
      YStart.append(HIST_Y_POSITION)
      XEnd.append(i+HIST_X_POSITION+1)
      YEnd.append(HIST_Y_POSITION-(int(HistValues[i]/HIST_SCALE_FACTOR)))
      
   AIL.MgraLines(AIL.M_DEFAULT, AilOverlayImage, HIST_NUM_INTENSITIES, 
                        XStart, YStart, XEnd, YEnd, AIL.M_DEFAULT)
                        
   # Print a message.
   print("\nHISTOGRAM:")
   print("----------\n")
   print("The histogram of the image was calculated and drawn.")
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.MimFree(HistResult)
   AIL.MbufFree(AilImage)
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)
   
   return 0

if __name__ == "__main__":
   MimHistogramExample()

```
