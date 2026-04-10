---
title: "MbufColor"
description: "This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result."
ms.language: "Python"
---

# MbufColor

> This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result.

**Language:** Python

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufChild2d`, `MbufChildColor`, `MbufClear`, `MbufDiskInquire`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispSelect`, `MgraText`, `MimArith`, `MimConvert`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#########################################################################################
#
#
# File name: MBufColor.py
#
# Synopsis:  This program demonstrates color buffer manipulations. It allocates 
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
#########################################################################################

import ail11 as AIL

# Source image file specifications. 
IMAGE_FILE             = AIL.M_IMAGE_PATH + "Bird.mim"

# Luminance offset to add to the image. 
IMAGE_LUMINANCE_OFFSET = 40

# Main function.
def MbufColorExample():

   # Allocate defaults.
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Allocate a color display buffer twice the size of the source image and display it. 
   SizeX = AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_X)
   SizeY = AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_Y) 
   AilImage = AIL.MbufAllocColor(AilSystem,
                  AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BAND),
                  SizeX * 2,
                  SizeY,
                  AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_TYPE),
                  AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC)
   AIL.MbufClear(AilImage, 0)
   AIL.MdispSelect(AilDisplay, AilImage)

   # Define 2 child buffers that maps to the left and right part of the display 
   # buffer, to put the source and destination color images.
   AilLeftSubImage = AIL.MbufChild2d(AilImage, 0, 0, SizeX, SizeY)
   AilRightSubImage = AIL.MbufChild2d(AilImage, SizeX, 0, SizeX, SizeY)

   # Load the color source image on the left.
   AIL.MbufLoad(IMAGE_FILE, AilLeftSubImage)

   # Define child buffers that map to the red, green and blue components
   # of the source image.
   AilRedBandSubImage = AIL.MbufChildColor(AilLeftSubImage, AIL.M_RED)
   AilGreenBandSubImage = AIL.MbufChildColor(AilLeftSubImage, AIL.M_GREEN)
   AilBlueBandSubImage = AIL.MbufChildColor(AilLeftSubImage, AIL.M_BLUE)

   # Write color text annotations to show access in each individual band of the image.
   
   # Note that this is typically done more simply by using:
   # AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_RGB888(0xFF,0x90,0x00))
   # AIL.MgraText(AIL.M_DEFAULT, AilLeftSubImage, ...)
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xFF)
   AIL.MgraText(AIL.M_DEFAULT, AilRedBandSubImage,   SizeX/16, SizeY/8, " TOUCAN ")
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0x90)
   AIL.MgraText(AIL.M_DEFAULT, AilGreenBandSubImage, SizeX/16, SizeY/8, " TOUCAN ")
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0x00)
   AIL.MgraText(AIL.M_DEFAULT, AilBlueBandSubImage,  SizeX/16, SizeY/8, " TOUCAN ")
   
   # Print a message. 
   print("\nCOLOR OPERATIONS:")
   print("-----------------\n")
   print("A color source image was loaded on the left and color text")
   print("annotations were written in it.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Convert image to Hue, Saturation, Luminance color space (HSL). 
   AIL.MimConvert(AilLeftSubImage, AilRightSubImage, AIL.M_RGB_TO_HSL)
  
   # Create a child buffer that maps to the luminance component. 
   AilLumSubImage = AIL.MbufChildColor(AilRightSubImage, AIL.M_LUMINANCE)
     
   # Add an offset to the luminance component. 
   AIL.MimArith(AilLumSubImage, IMAGE_LUMINANCE_OFFSET, AilLumSubImage, 
                                                      AIL.M_ADD_CONST + AIL.M_SATURATION)
  
   # Convert image back to Red, Green, Blue color space (RGB) for display. 
   AIL.MimConvert(AilRightSubImage, AilRightSubImage, AIL.M_HSL_TO_RGB) 

   # Print a message. 
   print("Luminance was increased using color image processing.")
  
   # Print a message. 
   print("Press any key to end.")
   AIL.MosGetch()

   # Release sub-images and color image buffer. 
   AIL.MbufFree(AilLumSubImage)
   AIL.MbufFree(AilRedBandSubImage)
   AIL.MbufFree(AilGreenBandSubImage)
   AIL.MbufFree(AilBlueBandSubImage)
   AIL.MbufFree(AilRightSubImage)
   AIL.MbufFree(AilLeftSubImage)
   AIL.MbufFree(AilImage)

   # Release defaults. 
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0


if __name__ == "__main__":
   MbufColorExample()
```
