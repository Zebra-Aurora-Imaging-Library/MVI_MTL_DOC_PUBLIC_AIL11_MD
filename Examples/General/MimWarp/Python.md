---
title: "MimWarp"
description: "This program performs three types of warp transformations. First the image is stretched according to four specified reference points. Then it is warped on a sinusoid, and finally, the program loops while warping the image on a sphere."
ms.language: "Python"
---

# MimWarp

> This program performs three types of warp transformations. First the image is stretched according to four specified reference points. Then it is warped on a sinusoid, and finally, the program loops while warping the image on a sphere.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufAlloc2d`, `MbufChild2d`, `MbufChildMove`, `MbufClear`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufPut1d`, `MbufPut2d`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgenWarpParameter`, `MimWarp`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: MimWarp.py 
#
# Synopsis:  : This program performs three types of warp transformations. 
#              First the image is stretched according to four specified 
#              reference points. Then it is warped on a sinusoid, and 
#              finally, the program loops while warping the image on a 
#              sphere.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################
 
import ail11 as AIL
import math
import sys

# Target image specifications. 
IMAGE_FILE             =  AIL.M_IMAGE_PATH + "BaboonMono.mim"
INTERPOLATION_MODE     =  AIL.M_NEAREST_NEIGHBOR
FIXED_POINT_PRECISION = lambda   : AIL.M_FIXED_POINT + 0 if INTERPOLATION_MODE == AIL.M_NEAREST_NEIGHBOR else AIL.M_FIXED_POINT + 6
FLOAT_TO_FIXED_POINT  = lambda x : 1 * x if INTERPOLATION_MODE == AIL.M_NEAREST_NEIGHBOR else 64 * x
ROTATION_STEP         = 1


def MimWarpExample():
   FourCornerMatrix = [
            0.0,             # X coordinate of quadrilateral's 1st corner 
            0.0,             # Y coordinate of quadrilateral's 1st corner 
            456.0,           # X coordinate of quadrilateral's 2nd corner 
            62.0,            # Y coordinate of quadrilateral's 2nd corner 
            333.0,           # X coordinate of quadrilateral's 3rd corner 
            333.0,           # Y coordinate of quadrilateral's 3rd corner 
            100.0,           # X coordinate of quadrilateral's 4th corner 
            500.0,           # Y coordinate of quadrilateral's 4th corner 
            0.0,           # X coordinate of rectangle's top-left corner 
            0.0,           # Y coordinate of rectangle's top-left corner 
            511.0,           # X coordinate of rectangle's bottom-right corner 
            511.0 ]          # Y coordinate of rectangle's bottom-right corner 
   Precision     = FIXED_POINT_PRECISION()
   Interpolation = INTERPOLATION_MODE
   OffsetX       = 0
   ImageWidth = 0
   ImageHeight = 0
   ImageType = AIL.M_NULL
   i = 0
   j = 0
   FramesPerSecond = 0
   Time = 0
   NbLoop = 0

   # Allocate defaults. 
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Restore the source image. 
   AilSourceImage = AIL.MbufRestore(IMAGE_FILE, AilSystem)

   # Allocate a display buffers and show the source image. 
   ImageWidth = AIL.MbufInquire(AilSourceImage, AIL.M_SIZE_X)
   ImageHeight = AIL.MbufInquire(AilSourceImage, AIL.M_SIZE_Y)
   ImageType = AIL.MbufInquire(AilSourceImage, AIL.M_TYPE)

   AilDisplayImage = AIL.MbufAlloc2d(AilSystem, ImageWidth, ImageHeight, ImageType, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP)
   AIL.MbufCopy(AilSourceImage, AilDisplayImage)
   AIL.MdispSelect(AilDisplay, AilDisplayImage)

   # Print a message. 
   print("\nWARPING:")
   print("--------\n")
   print("This image will be warped using different methods.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Four-corner LUT warping 
   #-------------------------
   
   # Allocate 2 LUT buffers. 
   AilLutX = AIL.MbufAlloc2d(AilSystem, ImageWidth, ImageHeight, 16 + AIL.M_SIGNED, AIL.M_LUT)
   AilLutY = AIL.MbufAlloc2d(AilSystem, ImageWidth, ImageHeight, 16 + AIL.M_SIGNED, AIL.M_LUT)

   # Allocate the coefficient buffer. 
   Ail4CornerArray = AIL.MbufAlloc2d(AilSystem, 12, 1, 32 + AIL.M_FLOAT, AIL.M_ARRAY)

   # Put warp values into the coefficient buffer. 
   AIL.MbufPut1d(Ail4CornerArray, 0, 12, FourCornerMatrix)

   # Generate LUT buffers. 
   AIL.MgenWarpParameter(Ail4CornerArray, AilLutX, AilLutY, AIL.M_WARP_4_CORNER + Precision, AIL.M_DEFAULT, 0.0, 0.0)

   # Clear the destination. 
   AIL.MbufClear(AilDisplayImage, 0)

   # Warp the image. 
   AIL.MimWarp(AilSourceImage, AilDisplayImage, AilLutX, AilLutY, AIL.M_WARP_LUT + Precision,
           Interpolation)

   # Print a message. 
   print("The image was warped from an arbitrary quadrilateral to a square.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Sinusoidal LUT warping 
   #------------------------

   # Allocate user-defined LUTs. 
   AilLutXPtr = [None] *ImageHeight*ImageWidth
   AilLutYPtr = [None] *ImageHeight*ImageWidth
  
   # Fill the LUT with a sinusoidal waveforms with a 6-bit precision.
   for j in range(ImageHeight):
      for i in range(ImageWidth):
         AilLutYPtr[i + (j * ImageWidth)] = FLOAT_TO_FIXED_POINT(j + int(20 * math.sin(0.03 * i)))
         AilLutXPtr[i + (j * ImageWidth)] = FLOAT_TO_FIXED_POINT(i + int(20 * math.sin(0.03 * j)))
   
   # Put the values into the LUT buffers.
   AIL.MbufPut2d(AilLutX, 0, 0, ImageWidth, ImageHeight, AilLutXPtr)
   AIL.MbufPut2d(AilLutY, 0, 0, ImageWidth, ImageHeight, AilLutYPtr)

   # Clear the destination. 
   AIL.MbufClear(AilDisplayImage,0)

   # Warp the image. 
   AIL.MimWarp(AilSourceImage,AilDisplayImage,AilLutX,AilLutY,AIL.M_WARP_LUT + Precision,
           Interpolation)
   
   # wait for a key 
   print("The image was warped on two sinusoidal waveforms.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Continuous spherical LUT warping 
   #--------------------------------

   # Allocate temporary buffer. 
   AIL.MbufFree(AilSourceImage)
   AilSourceImage = AIL.MbufAlloc2d(AilSystem, ImageWidth*2, ImageHeight, ImageType, 
                                        AIL.M_IMAGE + AIL.M_PROC)
                            
   # Reload the image. 
   AIL.MbufLoad(IMAGE_FILE, AilSourceImage)
   
   # Fill the LUTs with a sphere pattern with a 6-bit precision.
   GenerateSphericLUT(ImageWidth, ImageHeight, AilLutXPtr,AilLutYPtr)
   AIL.MbufPut2d(AilLutX, 0, 0, ImageWidth, ImageHeight, AilLutXPtr)
   AIL.MbufPut2d(AilLutY, 0, 0, ImageWidth, ImageHeight, AilLutYPtr)

   # Duplicate the buffer to allow wrap around in the warping. 
   AIL.MbufCopy(AilSourceImage, AilDisplayImage)
   ChildWindow = AIL.MbufChild2d(AilSourceImage, ImageWidth, 0, ImageWidth, ImageHeight)
   AIL.MbufCopy(AilDisplayImage, ChildWindow)
   AIL.MbufFree(ChildWindow)
  
   # Clear the destination. 
   AIL.MbufClear(AilDisplayImage,0)

   # Print a message and start the timer. 
   print("The image is continuously warped on a sphere.")
   print("Press any key to stop.\n")
   AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL)

   # Create a child in the buffer containing the two images. 
   ChildWindow = AIL.MbufChild2d(AilSourceImage, OffsetX, 0, ImageWidth, ImageHeight)

   # Warp the image continuously. 
   while ((not AIL.MosKbhit()) or (OffsetX != (ImageWidth/4))):
      # Move the child to the new position 
      AIL.MbufChildMove(ChildWindow, OffsetX, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
      
      # Warp the child in the window. 
      AIL.MimWarp(ChildWindow, AilDisplayImage, AilLutX, AilLutY, 
                           AIL.M_WARP_LUT+Precision, Interpolation)

      # Update the offset (shift the window to the right). 
      OffsetX += ROTATION_STEP
      
      # Reset the offset if the child is outside the buffer. 
      if OffsetX > ImageWidth - 1:
         OffsetX = 0

      NbLoop += 1

      # Calculate and print the number of frames per second processed. 
      Time = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)
      FramesPerSecond = NbLoop/Time
      print("Processing speed: {FramesPerSecond:.0f} Images/Sec.".format(FramesPerSecond=FramesPerSecond), end="\r")
   
   AIL.MosGetch()
   print("\nPress any key to end.")
   AIL.MosGetch()

   # Free objects. 

   AIL.MbufFree(ChildWindow)
   AIL.MbufFree(AilLutX)
   AIL.MbufFree(AilLutY)
   AIL.MbufFree(Ail4CornerArray)
   AIL.MbufFree(AilSourceImage)
   AIL.MbufFree(AilDisplayImage)
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0

# Generate two custom LUTs used to map the image on a sphere. 
# ----------------------------------------------------------- 
def GenerateSphericLUT(ImageWidth, ImageHeight, AilLutXPtr, AilLutYPtr):     
   # Set the radius of the sphere 
   Radius = 200.0         

   # Generate the X and Y buffers 
   for j in range(ImageHeight):
      k = j * ImageWidth

      vtmp = (j - (ImageHeight/2)) / Radius

      # Check that still in the sphere (in the Y axis). 
      if abs(vtmp) < 1.0:
         # We scan from top to bottom, so reverse the value obtained above
         # and obtain the angle. 
         vtmp = math.acos(-vtmp)
         if vtmp == 0.0:
            vtmp=0.0000001
      
         # Compute the position to fetch in the source. 
         v = ((vtmp/3.1415926) * (ImageHeight - 1) + 0.5)

         # Compute the Y coordinate of the sphere. 
         tmp = Radius * math.sin(vtmp)

         for i in range(ImageWidth):
            # Check that still in the sphere. 
            utmp = float((i - (ImageWidth/2)) / tmp)
            if abs(utmp) < 1.0:
               utmp = math.acos(-utmp)
            
               # Compute the position to fetch (fold the image in four).  
               AilLutXPtr[i + k] = int(FLOAT_TO_FIXED_POINT(((utmp/3.1415926) * float((ImageWidth/2) - 1) + 0.5)))
               AilLutYPtr[i + k] = int(FLOAT_TO_FIXED_POINT(v))
            else:
               # Default position (fetch outside the buffer to 
               # activate the clear overscan). 
               AilLutXPtr[i + k] = int(FLOAT_TO_FIXED_POINT(ImageWidth))
               AilLutYPtr[i + k] = int(FLOAT_TO_FIXED_POINT(ImageHeight)) 
      else:
         for  i in range(ImageWidth):
            # Default position (fetch outside the buffer for clear overscan). 
            AilLutXPtr[i + k] = int(FLOAT_TO_FIXED_POINT(ImageWidth))
            AilLutYPtr[i + k] = int(FLOAT_TO_FIXED_POINT(ImageHeight))
           
if __name__ == "__main__":
   MimWarpExample()
```
