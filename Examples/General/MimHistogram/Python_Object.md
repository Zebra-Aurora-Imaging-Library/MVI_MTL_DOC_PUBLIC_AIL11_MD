---
title: "MimHistogram"
description: "This program loads an image of a tissue sample, calculates its intensity histogram and draws it."
ms.language: "Python Object"
---

# MimHistogram

> This program loads an image of a tissue sample, calculates its intensity histogram and draws it.

**Language:** Python Object

**Functions used:** `MsysFree`, `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraLines`, `MimAllocResult`, `MimFree`, `MimGetResult`, `MimHistogram`, `MsysAlloc`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
#
# File name: MimHistogram.py
#
# Synopsis:  This program loads an image of a tissue sample, calculates its intensity
#            histogram and draws it.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################

from ZebraAuroraImagingObjectLibrary11 import *

class MimHistogramExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._graContext = Gra.Context(self._system)
      self._display = Disp.Display(self._system)
      self._imageDisp = Buf.Image()

      # Number of possible pixel intensities.
      self._histNumIntensities  = 256
      self._histScaleFactor     = 8
      self._histPosX       = 250
      self._histPosY       = 450

      # Target image file specifications.
      self._imageFile = AIL.M_IMAGE_PATH + "Cell.mbufi"

   def __enter__(self):
      return self

   def Run(self):
      xStart = []
      yStart = []
      xEnd = []
      yEnd = []

      # Restore source image into an automatically allocated image buffer.
      self._imageDisp.Restore(self._imageFile, self._system)

      # Display the image buffer and prepare for overlay annotations.
      self._display.Select(self._imageDisp)
      overlay = self._display.Overlay

      overlay.Enabled = Disp.Overlay.Enable

      # Allocate a histogram result buffer.
      self._histogramList = Im.HistogramList(self._system, self._histNumIntensities)

      # Calculate the histogram.
      Im.Histogram(self._imageDisp, self._histogramList)

      # Get the results.
      histValues = self._histogramList.Values

      # Draw the histogram in the overlay.
      self._graContext.Color = Color8.Red

      for i in range(self._histNumIntensities):
          xStart.append(i + self._histPosX + 1)
          yStart.append(self._histPosY)
          xEnd.append(i + self._histPosX + 1)
          yEnd.append(self._histPosY - (histValues[i] / self._histScaleFactor))
      self._graContext.DrawLineList(overlay.Image, self._histNumIntensities, xStart, yStart, xEnd, yEnd)

      # Print a message.
      print("\nHISTOGRAM:")
      print("----------\n")
      print("The histogram of the image was calculated and drawn.")
      print("Press any key to end.\n")
      Os.Getch()

   def __exit__(self, exc_type, exc_value, tb):
      self._histogramList.Free() if self._histogramList is not None else None
      self._display.Free() if self._display is not None else None
      self._graContext.Free() if self._graContext is not None else None
      self._imageDisp.Free() if self._imageDisp is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with MimHistogramExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print(exception)
      print("Press any key to exit.")
      Os.Getch()

```
