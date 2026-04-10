---
title: "MdispWindowLeveling"
description: "This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling."
ms.language: "Python"
---

# MdispWindowLeveling

> This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling.

**Language:** Python

**Functions used:** `MbufControl`, `MappAlloc`, `MappFree`, `MbufAlloc1d`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispLut`, `MdispSelect`, `MdispSelectWindow`, `MgenLutRamp`, `MgraLine`, `MgraText`, `MimAllocResult`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# 
# File name: MdispWindowLeveling.py
#
# Synopsis:  This program shows how to display a 10-bit monochrome Medical image
#            and applies a LUT to do interactive Window Leveling.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################

import ail11 as AIL

# Image file to load.
IMAGE_NAME = "ArmsMono10bit.mim"
IMAGE_FILE = AIL.M_IMAGE_PATH + IMAGE_NAME

# Draw the LUT shape (disable it to reduce CPU usage). 
DRAW_LUT_SHAPE = AIL.M_YES

def MdispWindowLevelingExample():
   AilOriginalImage = None

   # Allocate the Aurora Imaging Library application, System and Display.
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Restore target image from disk.
   AilImage = AIL.MbufRestore(IMAGE_FILE, AilSystem)

   # Dynamically calculates the maximum value of the image using processing. 
   AilExtremeResult = AIL.MimAllocResult(AIL.MbufInquire(AilImage, AIL.M_OWNER_SYSTEM),
                                         1,
                                         AIL.M_EXTREME_LIST)

   AIL.MimFindExtreme(AilImage, AilExtremeResult, AIL.M_MAX_VALUE)

   ImageMaxValue = AIL.MimGetResult(AilExtremeResult, AIL.M_VALUE)[0]

   AIL.MimGetResult(AilExtremeResult, AIL.M_VALUE)
   
   AIL.MimFree(AilExtremeResult)

   # Set the maximum value of the image to indicate to Aurora Imaging Library how to initialize 
   # the default display LUT.
   AIL.MbufControl(AilImage, AIL.M_MAX, ImageMaxValue)

   # Display the image (to specify a user-defined window, use MdispSelectWindow()).
   AIL.MdispSelect(AilDisplay, AilImage)

   # Determine the maximum displayable value of the current display. 
   DisplaySizeBit = AIL.MdispInquire(AilDisplay, AIL.M_SIZE_BIT)
   DisplayMaxValue = (1<<DisplaySizeBit)-1

   # Print key information. 
   print("\nINTERACTIVE WINDOW LEVELING:")
   print("----------------------------\n")

   print("Image name : {IMAGE_NAME}".format(IMAGE_NAME=IMAGE_NAME))

   print("Image size : {ImageSizeX} x {ImageSizeY}".format(ImageSizeX=AIL.MbufInquire(AilImage, AIL.M_SIZE_X), ImageSizeY=AIL.MbufInquire(AilImage, AIL.M_SIZE_Y)))
   print("Image max  : {ImageMaxValue}".format(ImageMaxValue=ImageMaxValue))
   print("Display max:  {DisplayMaxValue}\n".format(DisplayMaxValue=DisplayMaxValue))

   # Allocate a LUT buffer according to the image maximum value and
   # display pixel depth.
   AilLut = AIL.MbufAlloc1d(AilSystem, ImageMaxValue + 1, 16 if DisplaySizeBit > 8 else 8 + AIL.M_UNSIGNED, AIL.M_LUT)

   # Generate a LUT with a full range ramp and set its maximum value. 
   AIL.MgenLutRamp(AilLut, 0, 0, ImageMaxValue, DisplayMaxValue)
   AIL.MbufControl(AilLut, AIL.M_MAX, DisplayMaxValue)

   # Set the display LUT. 
   AIL.MdispLut(AilDisplay, AilLut)

   # Interactive Window Leveling using keyboard.
   print("Keys assignment:\n")
   print("Arrow keys :    Left=move Left, Right=move Right, Down=Narrower, Up=Wider.")
   print("Intensity keys: L=Lower,  U=Upper,  R=Reset.")
   print("Press enter to end.\n")

   # Modify LUT shape according to the arrow keys and update it. 
   Ch = 0
   Start = 0
   End = ImageMaxValue
   InflectionLevel = DisplayMaxValue
   Step = int((ImageMaxValue + 1) / 128)
   Step = max(Step, 4)

   while (chr(Ch) != '\r'):
      # Left arrow: Move region left.
      if Ch == 0x4B:
         Start -= Step
         End -= Step

      # Right arrow: Move region right.
      elif Ch == 0x4D:
         Start += Step
         End += Step

      # Down arrow: Narrow region. 
      elif Ch == 0x50:
         Start += Step
         End -= Step
      
      # Up arrow: Widen region. 
      elif Ch == 0x48:
         Start -= Step
         End += Step

      # L key: Lower inflexion point. 
      elif chr(Ch) == "L" or chr(Ch) == "l":
         InflectionLevel -= 1

      # U key: Upper inflexion point.
      elif chr(Ch) == "U" or chr(Ch) == "u":
         InflectionLevel += 1

      # R key: Reset the LUT to full image range.
      elif chr(Ch) == "R" or chr(Ch) == "r":
         Start = 0
         End = ImageMaxValue 
         InflectionLevel = DisplayMaxValue

      # Saturate.
      End = min(End, ImageMaxValue)
      Start = min(Start, End)
      End = max(End, Start)
      Start = max(Start, 0)
      End = max(End, 0)
      InflectionLevel = max(InflectionLevel, 0)
      InflectionLevel = min(InflectionLevel, DisplayMaxValue)
      print("Inflection points: Low=({Start},0), High=({End},{InflectionLevel}).   \r".format(Start=Start, End=End, InflectionLevel=InflectionLevel), end='')

      # Generate a LUT with 3 slopes and saturated at both ends.
      AIL.MgenLutRamp(AilLut, 0, 0, Start, 0)
      AIL.MgenLutRamp(AilLut, Start, 0, End, InflectionLevel)
      AIL.MgenLutRamp(AilLut, End, InflectionLevel, ImageMaxValue, DisplayMaxValue)

      # Update the display LUT.
      AIL.MdispLut(AilDisplay, AilLut)

      if DRAW_LUT_SHAPE:
         if not AilOriginalImage:
            AilOriginalImage = AIL.MbufRestore(IMAGE_FILE, AilSystem)
         DrawLutShape(AilDisplay, AilOriginalImage, AilImage, Start, End, InflectionLevel, ImageMaxValue, DisplayMaxValue)

      # If its an arrow key, get the second code. 
      Ch = AIL.MosGetch()
      if Ch == 0xE0:
            Ch = AIL.MosGetch()
   
   print("\n")

   # Free all allocations.
   AIL.MbufFree(AilLut)
   AIL.MbufFree(AilImage)
   if AilOriginalImage != AIL.M_NULL:
      AIL.MbufFree(AilOriginalImage)
   
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0 


# Function to draw the current LUT's shape in the image.

# Note: This simple annotation method requires significant update
#       and CPU time since it repaints the entire image every time.

def DrawLutShape(AilDisplay, AilOriginalImage, AilImage, Start, End, InflexionIntensity, ImageMaxValue, DisplayMaxValue):
   # Inquire image dimensions. 
   ImageSizeX = AIL.MbufInquire(AilImage, AIL.M_SIZE_X)
   ImageSizeY = AIL.MbufInquire(AilImage, AIL.M_SIZE_Y)

   # Calculate the drawing parameters. 
   Xstep  = ImageSizeX / ImageMaxValue
   Xstart = Start * Xstep
   Xend   = End * Xstep
   Ystep  = (ImageSizeY / 4.0) / DisplayMaxValue
   Ymin   = ImageSizeY - 2
   Yinf   = Ymin - (InflexionIntensity * Ystep)
   Ymax   = Ymin - (DisplayMaxValue * Ystep)

   # To increase speed, disable display update until all annotations are done. 
   AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_DISABLE)

   # Restore the original image.
   AIL.MbufCopy(AilOriginalImage, AilImage)
   
   # Draw axis max and min values. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, float(ImageMaxValue))
   AIL.MgraText(AIL.M_DEFAULT, AilImage, 4, int(Ymin - 22), "0")
   AIL.MgraText(AIL.M_DEFAULT, AilImage, 4, int(Ymax - 16), str(DisplayMaxValue))
   AIL.MgraText(AIL.M_DEFAULT, AilImage, ImageSizeX - 38 , Ymin - 22, str(ImageMaxValue))

   # Draw LUT Shape (X axis is display values and Y is image values). 
   AIL.MgraLine(AIL.M_DEFAULT, AilImage, 0, int(Ymin), int(Xstart), int(Ymin))
   AIL.MgraLine(AIL.M_DEFAULT, AilImage, int(Xstart), int(Ymin), int(Xend), int(Yinf))
   AIL.MgraLine(AIL.M_DEFAULT, AilImage, int(Xend), int(Yinf), ImageSizeX - 1, int(Ymax))

   # Enable display update to show the result. 
   AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_ENABLE)


   
if __name__ == "__main__":
   MdispWindowLevelingExample()

```
