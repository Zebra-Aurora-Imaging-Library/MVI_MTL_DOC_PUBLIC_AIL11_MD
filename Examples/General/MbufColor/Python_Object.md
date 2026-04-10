---
title: "MbufColor"
description: "This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result."
ms.language: "Python Object"
---

# MbufColor

> This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result.

**Language:** Python Object

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufChild2d`, `MbufChildColor`, `MbufClear`, `MbufDiskInquire`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispSelect`, `MgraText`, `MimArith`, `MimConvert`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#########################################################################################
#
# File name: MBufColor.py
#
# Content:   This program demonstrates color buffer manipulations. It allocates
#            a displayable color image buffer, displays it, and loads a color
#            image into the left part. After that, color annotations are done
#            in each band of the color buffer. Finally, to increase the image
#            luminance, the image is converted to Hue, Saturation and Luminance
#            (HSL), a certain offset is added to the luminance component and
#            the image is converted back to Red, Green, Blue (RGB) into the
#            right part to display the result.
#
#            The example also demonstrates how to display multiple images
#            in a single display using child buffers.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#
#########################################################################################

from ZebraAuroraImagingObjectLibrary11 import *

class MbufColorExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._display = Disp.Display(self._system)
      self._graphicContext = Gra.Context(self._system)
      self._imageDisp = None

      self._imageFile = Paths.Images + "Bird.mim"
      self._imageLuminanceOffset = 40

   def __enter__(self):
      return self

   def Run(self):
      fileInfo = self._application.File.FileInfo(self._imageFile)
      sizeX = fileInfo.SizeX
      sizeY = fileInfo.SizeY
      sizeBand = fileInfo.SizeBand

      # Allocate a color display buffer twice the size of the source image and display it.
      self._imageDisp = Buf.Image(self._system, sizeBand, sizeX * 2, sizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDisp)

      self._imageDisp.Clear(Color8.Black)
      self._display.Select(self._imageDisp)

      # Define 2 child buffers that maps to the left and right part of the display
      # buffer, to put the source and destination color images.
      #
      self._ailLeftSubImage = self._imageDisp.Child2d(0, 0, sizeX, sizeY)
      self._ailRightSubImage = self._imageDisp.Child2d(sizeX, 0, sizeX, sizeY)

      # Load the color source image on the left.
      self._ailLeftSubImage.Load(self._imageFile)

      # Define child buffers that map to the red, green and blue components
      # of the source image.
      #
      self._ailRedBandSubImage = self._ailLeftSubImage.ChildColor(Buf.ColorBand.Red)
      self._ailGreenBandSubImage = self._ailLeftSubImage.ChildColor(Buf.ColorBand.Green)
      self._ailBlueBandSubImage = self._ailLeftSubImage.ChildColor(Buf.ColorBand.Blue)

      # Write color text annotations to show access in each individual band of the image.
      #
      self._graphicContext.Color = 0xFF
      self._graphicContext.DrawString(self._ailRedBandSubImage, sizeX / 16, sizeY / 8, " TOUCAN ")
      self._graphicContext.Color = 0x90
      self._graphicContext.DrawString(self._ailGreenBandSubImage, sizeX / 16, sizeY / 8, " TOUCAN ")
      self._graphicContext.Color = 0x00
      self._graphicContext.DrawString(self._ailBlueBandSubImage, sizeX / 16, sizeY / 8, " TOUCAN ")

      # Print a message.
      print("\nCOLOR OPERATIONS:")
      print("-----------------\n")
      print("A color source image was loaded on the left and color text")
      print("annotations were written in it.")
      print("Press any key to continue.\n")
      Os.Getch()

      # Convert image to Hue, Luminance, Saturation color space (HSL).
      Im.Convert(self._ailLeftSubImage, self._ailRightSubImage, Im.ConversionType.RgbToHsl)

      # Create a child buffer that maps to the luminance component.
      self._ailLumSubImage = self._ailRightSubImage.ChildColor(Buf.ColorBand.Luminance)

      # Add an offset to the luminance component.
      Im.Arith.Add(self._ailLumSubImage, self._imageLuminanceOffset, self._ailLumSubImage, True)

      # Convert image back to Red, Green, Blue color space (RGB) for display.
      Im.Convert(self._ailRightSubImage, self._ailRightSubImage, Im.ConversionType.HslToRgb)

      # Print a message.
      print("Luminance was increased using color image processing.")

      # Print a message.
      print("Press any key to end.\n")
      Os.Getch()

   def __exit__(self, exc_type, exc_value, tb):
      self._ailLumSubImage.Free() if self._ailLumSubImage is not None else None
      self._ailRedBandSubImage.Free() if self._ailRedBandSubImage is not None else None
      self._ailGreenBandSubImage.Free() if self._ailGreenBandSubImage is not None else None
      self._ailBlueBandSubImage.Free() if self._ailBlueBandSubImage is not None else None
      self._ailRightSubImage.Free() if self._ailRightSubImage is not None else None
      self._ailLeftSubImage.Free() if self._ailLeftSubImage is not None else None
      self._display.Free() if self._display is not None else None
      self._graphicContext.Free() if self._graphicContext is not None else None
      self._imageDisp.Free() if self._imageDisp is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with MbufColorExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print(exception)
      print("Press any key to exit.")
      Os.Getch()

```
