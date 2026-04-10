---
title: "Mdlocr"
description: "This program uses the Deep Learning Ocr module to read a product expiry date. First all strings are read, then a model is defined based on the first read to filter out the target string using its size and dimensions."
ms.language: "Python"
---

# Mdlocr

> This program uses the Deep Learning Ocr module to read a product expiry date. First all strings are read, then a model is defined based on the first read to filter out the target string using its size and dimensions.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MdlocrAlloc`, `MdlocrAllocResult`, `MdlocrControl`, `MdlocrDefineModelFromResult`, `MdlocrDraw`, `MdlocrFree`, `MdlocrGetResult`, `MdlocrPreprocess`, `MdlocrRead`, `MgraControl`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Packaging, Applications, Reading, Modules, Buffer, Display, Deep Learning OCR, Graphics, What's New, AIL 10.0 SP7

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: Mdlocr.py
#
# Synopsis: This program uses the Deep Learning Ocr module to read a product
# expiry date. 
# First all strings are read, then a model is defined based on the first
# read to filter out the target string using its size and dimensions.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################

 
import ail11 as AIL
import os

# image file specifications.
IMAGE_FILE_TO_READ = os.path.join(AIL.M_IMAGE_PATH, "ExpiryDateAndLot2.mim")
# Max string sizes.
STRING_MAX_SIZE = 256
# Invalid string index
INVALID_TARGET_READ_INDEX = -1
# The target string
TARGET_STRING = "2012JL03ERB1A2217"

def Read(AilDisplay, AilDlocrContext, AilImage, 
         AilOverlayImage, AilDlocrResult, StringMatchingMode):
   # Perform the read operation on the specified target image.
   AIL.MdlocrRead(AilDlocrContext, AilImage, AilDlocrResult, StringMatchingMode)

   # Get the status of the operation.
   ReadStatus = AIL.MdlocrGetResult(AilDlocrResult, AIL.M_GENERAL, AIL.M_GENERAL, AIL.M_STATUS)
   if ReadStatus != AIL.M_COMPLETE:
      print("Read operation failed.")
      return INVALID_TARGET_READ_INDEX

   # Get number of strings read and show the result.
   NumberOfStringRead = AIL.MdlocrGetResult(AilDlocrResult, AIL.M_GENERAL, AIL.M_GENERAL, 
                                            AIL.M_STRING_NUMBER + AIL.M_TYPE_AIL_INT) 

   # Clear the display overlay.
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)

   # Find the target string index.
   TargetStringIndex = INVALID_TARGET_READ_INDEX

   if(NumberOfStringRead >= 1):
      print("The image was read successfully.\n")

      # Draw read result.
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_CYAN)
      AIL.MdlocrDraw(AIL.M_DEFAULT, AilDlocrResult, AilOverlayImage, AIL.M_DRAW_STRING, 
                     AIL.M_ALL, AIL.M_DEFAULT, AIL.M_DEFAULT)
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN)
      AIL.MdlocrDraw(AIL.M_DEFAULT, AilDlocrResult, AilOverlayImage, AIL.M_DRAW_STRING_BOX,
                 AIL.M_ALL, AIL.M_DEFAULT, AIL.M_DEFAULT)
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED)
      AIL.MdlocrDraw(AIL.M_DEFAULT, AilDlocrResult, AilOverlayImage, AIL.M_DRAW_STRING_INDEX,
                 AIL.M_ALL, AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Print the read result.
      print(" String")
      print(" -----------------------------------")
      for i in range(NumberOfStringRead):
         StringResult = AIL.MdlocrGetResult(AilDlocrResult, i, AIL.M_GENERAL, AIL.M_STRING)
         print(" {StringResult}".format(StringResult=StringResult))

         # Check if the string read is the one being looked for.
         if StringResult == TARGET_STRING:
            TargetStringIndex = i
      print()

   if TargetStringIndex == INVALID_TARGET_READ_INDEX:
      print("The target string {Target} was not found.".format(Target=TARGET_STRING))
   return TargetStringIndex

def MdlocrExample():
   # Print module name.
   print("[EXAMPLE NAME]")
   print("Mdlocr\n")
   print("[SYNOPSIS]")
   print("This program uses the Deep Learning Ocr module to read a product")
   print("expiry date.")
   print("First all strings are read, then a model is defined based on the first")
   print("read to filter out the target string using its size and dimensions.\n")
   print("[MODULES USED]")
   print("Deep Learning OCR, Buffer, Display, Graphics.\n")
 
   # Allocate defaults.
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Restore the font definition image.
   AilImage = AIL.MbufRestore(IMAGE_FILE_TO_READ, AilSystem)

   # Display the image and prepare for overlay annotations.
   AIL.MdispSelect(AilDisplay, AilImage)
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)

   # Allocate a new empty dlocr context.
   AilDlocrContext = AIL.MdlocrAlloc(AilSystem, AIL.M_OCR_NET1_BALANCED_V1, AIL.M_DEFAULT)

   # Allocate a new empty dlocr result buffer.
   AilDlocrResult = AIL.MdlocrAllocResult(AilSystem, AIL.M_DLOCR_READ_RESULT, AIL.M_DEFAULT)

   # Set the context's target image max size X and Y
   #   Note that these dimension can be superior to encompass several images.
   MaxSizeX = AIL.MbufInquire(AilImage, AIL.M_SIZE_X)
   MaxSizeY = AIL.MbufInquire(AilImage, AIL.M_SIZE_Y)
   AIL.MdlocrControl(AilDlocrContext, AIL.M_TARGET_MAX_SIZE_X, MaxSizeX)
   AIL.MdlocrControl(AilDlocrContext, AIL.M_TARGET_MAX_SIZE_Y, MaxSizeY)

   # Preprocess the dlocr context.
   AIL.MdlocrPreprocess(AilDlocrContext, AIL.M_DEFAULT)

   # First read. 
   TargetStringIndex = Read(AilDisplay, AilDlocrContext, AilImage, AilOverlayImage, AilDlocrResult, AIL.M_READ_ALL)

   if TargetStringIndex != INVALID_TARGET_READ_INDEX:
      # Pause to show results.
      print(
         "First read without string model, the best before date was detected among")
      print(
         "others. We note the associated index {iTarget} (in red). It will be used in the next step".format(iTarget=TargetStringIndex))
      print(
         "to filter out the targeted string.")

      print("Press any key to continue.\n")
      AIL.MosGetch()

      # Define model from result.
      AIL.MdlocrDefineModelFromResult(AilDlocrContext, AIL.M_DEFAULT, AilDlocrResult, 
                                    TargetStringIndex, AIL.M_DEFAULT)

      # This modification necessitate a new Preprocess.
      AIL.MdlocrPreprocess(AilDlocrContext, AIL.M_DEFAULT)

      # Second read.
      Read(AilDisplay, AilDlocrContext, AilImage, AilOverlayImage, AilDlocrResult, AIL.M_MODEL_BASED)

      # Pause to show results.
      print(
         "We defined a string model from the result to filter out based on string size")
      print(
         "and character dimensions.")
      print(
         "Second read with the string model. Targeted string is detected.")

   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free all allocations.
   AIL.MdlocrFree(AilDlocrContext)
   AIL.MdlocrFree(AilDlocrResult)
   AIL.MbufFree(AilImage)
   
   # Free defaults.
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0

if __name__ == "__main__":
   MdlocrExample()

```
