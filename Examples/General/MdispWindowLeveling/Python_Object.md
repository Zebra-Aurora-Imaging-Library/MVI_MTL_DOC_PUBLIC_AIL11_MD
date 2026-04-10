---
title: "MdispWindowLeveling"
description: "This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling."
ms.language: "Python Object"
---

# MdispWindowLeveling

> This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling.

**Language:** Python Object

**Functions used:** `MbufControl`, `MappAlloc`, `MappFree`, `MbufAlloc1d`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispLut`, `MdispSelect`, `MdispSelectWindow`, `MgenLutRamp`, `MgraLine`, `MgraText`, `MimAllocResult`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#########################################################################################
#
# File name : MdispWindowLeveling.py
#
# Content   : This program shows how to display a 10-bit monochrome Medical image
#             and applies a LUT to do interactive Window Leveling.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#
#########################################################################################

from ZebraAuroraImagingObjectLibrary11 import *

class MdispWindowLevelingExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._display = Disp.Display(self._system)
      self._graphicContext = Gra.Context(self._system)

      self._imageDisp = None
      self._originalImage = None
      self._lut = None

      self._imageName = "ArmsMono10bit.mim"
      self._imageFile = Paths.Images + self._imageName

   def __enter__(self):
      return self

   def Run(self):
      # Restore target image from disk.
      self._imageDisp = Buf.Image(self._imageFile, self._system)

      # Dynamically calculates the maximum value of the image using processing.
      self._extremeList = Im.ExtremeList(self._system, 1)
      Im.FindExtreme(self._imageDisp, self._extremeList, Im.FindExtremeExtremeTypes.MaxValue)
      imageMaxValue = self._extremeList.Values[0]

      # Set the maximum value of the image to indicate to Aurora Imaging Library how to initialize the default display LUT.
      self._imageDisp.Max = imageMaxValue

      # Display the image (to specify a user-defined window, use MdispSelectWindow()).
      self._display.Select(self._imageDisp)

      # Determine the maximum displayable value of the current display.
      displaySizeBit = self._display.SizeBit
      displayMaxValue = (1<<displaySizeBit)-1

      # Print key information.
      print("\nINTERACTIVE WINDOW LEVELING:")
      print("----------------------------\n")

      print("Image name : {IMAGE_NAME}".format(IMAGE_NAME=self._imageName))
      print("Image size : {ImageSizeX} x {ImageSizeY}".format(ImageSizeX=self._imageDisp.SizeX, ImageSizeY=self._imageDisp.SizeY))
      print("Image max  : {ImageMaxValue}".format(ImageMaxValue=imageMaxValue))
      print("Display max:  {DisplayMaxValue}\n".format(DisplayMaxValue=displayMaxValue))

      # Allocate a LUT buffer according to the image maximum value and display pixel depth.
      self._lut = Buf.Lut(self._system, self._imageDisp.SizeBand, imageMaxValue + 1, 1, Buf.AllocType.Unsigned8)

      # Generate a LUT with a full range ramp and set its maximum value.
      Gen.LutRamp(self._lut, 0, 0, imageMaxValue, displayMaxValue)
      self._lut.Max = displayMaxValue

      # Set the display LUT.
      self._display.Lut = self._lut

      # Interactive Window Leveling using keyboard.
      print("Keys assignment:\n")
      print("Arrow keys :    Left=move Left, Right=move Right, Down=Narrower, Up=Wider.")
      print("Intensity keys: L=Lower,  U=Upper,  R=Reset.")
      print("Press enter to end.\n")

      # Modify LUT shape according to the arrow keys and update it.
      start = 0
      end = imageMaxValue
      inflectionLevel = displayMaxValue
      step = (int)((imageMaxValue + 1) / 128)
      step = max(step, 4)

      ch = 0
      while (chr(ch) != '\r'):
         # Left arrow: Move region left.
         if ch == 0x4B:
            start -= step
            end -= step

         # Right arrow: Move region right.
         elif ch == 0x4D:
            start += step
            end += step

         # Down arrow: Narrow region.
         elif ch == 0x50:
            start += step
            end -= step

         # Up arrow: Widen region.
         elif ch == 0x48:
            start -= step
            end += step

         # L key: Lower inflexion point.
         elif ch == "L" or chr(ch) == "l":
            inflectionLevel -= 1

         # U key: Upper inflexion point.
         elif ch == "U" or chr(ch) == "u":
            inflectionLevel += 1

         # R key: Reset the LUT to full image range.
         elif ch == "R" or chr(ch) == "r":
            start = 0
            end = imageMaxValue
            inflectionLevel = displayMaxValue

         # Saturate.
         end = min(end, imageMaxValue)
         start = min(start, end)
         end = max(end, start)
         start = max(start, 0)
         end = max(end, 0)
         inflectionLevel = max(inflectionLevel, 0)
         inflectionLevel = min(inflectionLevel, displayMaxValue)
         print("Inflection points: Low=({Start},0), High=({End},{InflectionLevel}).   \r".format(Start=start, End=end, InflectionLevel=inflectionLevel), end='')

         # Generate a LUT with 3 slopes and saturated at both ends.
         Gen.LutRamp(self._lut, 0, 0, start, 0)
         Gen.LutRamp(self._lut, start, 0, end, inflectionLevel)
         Gen.LutRamp(self._lut, end, inflectionLevel, imageMaxValue, displayMaxValue)

         # Update the display LUT.
         self._display.Lut = self._lut

         # Draw the current LUT's shape in the image.
         # Note: This simple annotation method requires
         #       significant update and CPU time.=
         if(self._originalImage == None):
            self._originalImage = Buf.Image(self._imageFile, self._system)
         self.DrawLutShape(start, end, inflectionLevel, imageMaxValue, displayMaxValue)

         # If its an arrow key, get the second code.
         ch = Os.Getch()
         if ch == 0xE0:
            ch = Os.Getch()

   def DrawLutShape(self, start, end, inflexionIntensity, imageMaxValue, displayMaxValue):
       imageSizeX = self._imageDisp.SizeX
       imageSizeY = self._imageDisp.SizeY

       # Calculate the drawing parameters.
       xStep = imageSizeX / imageMaxValue
       xStart = start * xStep
       xEnd = end * xStep
       yStep = (imageSizeY / 4.0) / displayMaxValue
       yMin = (imageSizeY - 2)
       yInf = yMin - (inflexionIntensity * yStep)
       yMax = yMin - (displayMaxValue * yStep)

       # To increase speed, disable display update until all annotations are done.
       self._display.Update.Display = Disp.Update.Disable

       # Restore the original image.
       Buf.Copy(self._originalImage, self._imageDisp)

       # Draw axis max and min values.
       self._graphicContext.Color = imageMaxValue
       self._graphicContext.DrawString(self._imageDisp, 4, yMin - 22, "0")
       self._graphicContext.DrawString(self._imageDisp, 4, yMax - 16, str(displayMaxValue))
       self._graphicContext.DrawString(self._imageDisp, imageSizeX - 38, yMin - 22, str(imageMaxValue))

       # Draw LUT Shape (X axis is display values and Y is image values).
       self._graphicContext.DrawLine(self._imageDisp, 0, yMin, xStart, yMin)
       self._graphicContext.DrawLine(self._imageDisp, xStart, yMin, xEnd, yInf)
       self._graphicContext.DrawLine(self._imageDisp, xEnd, yInf, imageSizeX - 1, yMax)

       # Enable display update to show the result.
       self._display.Update.Display = Disp.Update.Enable

   def __exit__(self, exc_type, exc_value, tb):
      self._extremeList.Free() if self._extremeList is not None else None
      self._display.Free() if self._display is not None else None
      self._graphicContext.Free() if self._graphicContext is not None else None
      self._imageDisp.Free() if self._imageDisp is not None else None
      self._originalImage.Free() if self._originalImage is not None else None
      self._lut.Free() if self._lut is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with MdispWindowLevelingExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print(exception)
      print("Press any key to exit.")
      Os.Getch()

```
