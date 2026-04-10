---
title: "Mcode"
description: "This program decodes a linear Code 39 and a DataMatrix code."
ms.language: "Python"
---

# Mcode

> This program decodes a linear Code 39 and a DataMatrix code.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MbufChild2d`, `MbufFree`, `MbufRestore`, `McodeAlloc`, `McodeAllocResult`, `McodeControl`, `McodeFree`, `McodeGetResult`, `McodeModel`, `McodeRead`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraRect`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Applications, Identifying, Modules, Buffer, Display, Graphics, Code Reader, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: MCode.py
#
# Synopsis:  This program decodes a linear Code 39 and a DataMatrix code.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################

 
import ail11 as AIL
import os

# Target image character specifications. 
IMAGE_FILE                   = os.path.join(AIL.M_IMAGE_PATH, "Code.mim")

# Regions around 1D code. 
BARCODE_REGION_TOP_LEFT_X    = 256
BARCODE_REGION_TOP_LEFT_Y    =  80
BARCODE_REGION_SIZE_X        = 290
BARCODE_REGION_SIZE_Y        =  60

# Regions around 2D code. 
DATAMATRIX_REGION_TOP_LEFT_X =   8
DATAMATRIX_REGION_TOP_LEFT_Y = 312
DATAMATRIX_REGION_SIZE_X     = 118
DATAMATRIX_REGION_SIZE_Y     = 105

# Maximum length of the string to read. 
STRING_LENGTH_MAX            = 64


def McodeExample():
   AnnotationColor = AIL.M_COLOR_GREEN
   AnnotationBackColor = AIL.M_COLOR_GRAY

   # Allocate defaults.
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Restore source image into an automatically allocated image buffer. 
   AilImage = AIL.MbufRestore(IMAGE_FILE, AilSystem)

   # Display the image buffer. 
   AIL.MdispSelect(AilDisplay, AilImage)
       
   # Prepare for overlay annotations. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)

   # Prepare bar code results buffer 
   CodeResults = AIL.McodeAllocResult(AilSystem, AIL.M_DEFAULT)

   # Pause to show the original image. 
   print("\n1D and 2D CODE READING:")
   print("-----------------------\n")
   print("This program will decode a linear Code 39 and a DataMatrix code.")
   print("Press any key to continue.\n")
   AIL.MosGetch()


   # 1D BARCODE READING: 
   
   # Create a read region around the code to speedup reading. 
   BarCodeRegion = AIL.MbufChild2d(AilImage, BARCODE_REGION_TOP_LEFT_X, BARCODE_REGION_TOP_LEFT_Y,
               BARCODE_REGION_SIZE_X, BARCODE_REGION_SIZE_Y)
   
   # Allocate CODE objects. 
   Barcode = AIL.McodeAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AIL.McodeModel(Barcode, AIL.M_ADD, AIL.M_CODE39, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_NULL)

   # Read codes from image. 
   AIL.McodeRead(Barcode, BarCodeRegion, CodeResults)
    
   # Get decoding status. 
   BarcodeStatus = AIL.McodeGetResult(CodeResults, AIL.M_GENERAL, AIL.M_GENERAL, AIL.M_STATUS + AIL.M_TYPE_AIL_INT)

   # Check if decoding was successful. 
   if BarcodeStatus == AIL.M_STATUS_READ_OK:
      # Get decoded string. 
      BarcodeString = AIL.McodeGetResult(CodeResults, 0, AIL.M_GENERAL, AIL.M_STRING)

      # Draw the decoded strings and read region in the overlay image. 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor) 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKCOLOR, AnnotationBackColor) 
      OutputString = "\"{BarcodeString}\" ".format(BarcodeString=BarcodeString)
      AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, BARCODE_REGION_TOP_LEFT_X + 10,  BARCODE_REGION_TOP_LEFT_Y + 80, " 1D linear 39 bar code:           ")
      AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, BARCODE_REGION_TOP_LEFT_X + 200, BARCODE_REGION_TOP_LEFT_Y + 80, OutputString)
      AIL.MgraRect(AIL.M_DEFAULT, AilOverlayImage,
                   BARCODE_REGION_TOP_LEFT_X, BARCODE_REGION_TOP_LEFT_Y,
                   BARCODE_REGION_TOP_LEFT_X + BARCODE_REGION_SIZE_X, 
                   BARCODE_REGION_TOP_LEFT_Y + BARCODE_REGION_SIZE_Y)

   # Free objects.
   AIL.McodeFree(Barcode)
   AIL.MbufFree(BarCodeRegion)

   # 2D CODE READING:
  
   # Create a read region around the code to speedup reading.
   DataMatrixRegion = AIL.MbufChild2d(AilImage, DATAMATRIX_REGION_TOP_LEFT_X, DATAMATRIX_REGION_TOP_LEFT_Y,
               DATAMATRIX_REGION_SIZE_X, DATAMATRIX_REGION_SIZE_Y)
  
   # Allocate CODE objects.
   DataMatrixCode = AIL.McodeAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AIL.McodeModel(DataMatrixCode, AIL.M_ADD, AIL.M_DATAMATRIX, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_NULL)


   # Set the foreground value for the DataMatrix since it is different
   # from the default value. 
   AIL.McodeControl(DataMatrixCode, AIL.M_FOREGROUND_VALUE, AIL.M_FOREGROUND_WHITE) 
    
   # Read codes from image.
   AIL.McodeRead(DataMatrixCode, DataMatrixRegion, CodeResults)
    
   # Get decoding status.
   DataMatrixStatus = AIL.McodeGetResult(CodeResults, AIL.M_GENERAL, AIL.M_GENERAL, AIL.M_STATUS + AIL.M_TYPE_AIL_INT)

   # Check if decoding was successful. 
   if DataMatrixStatus == AIL.M_STATUS_READ_OK:
      # Get decoded string. 
      DataMatrixString = AIL.McodeGetResult(CodeResults, 0, AIL.M_GENERAL, AIL.M_STRING)

      # Draw the decoded strings and read region in the overlay image. 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor) 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKCOLOR, AnnotationBackColor) 
      # Replace non printable characters with space.
      for n in range(len(DataMatrixString)):
         if DataMatrixString[n] < '0' or DataMatrixString[n] > 'Z':
            DataMatrixString = list(DataMatrixString)
            DataMatrixString[n] = ' '
            DataMatrixString = "".join(DataMatrixString)
      OutputString = "\"{DataMatrixString}\" ".format(DataMatrixString=DataMatrixString)
      AIL.MgraText(AIL.M_DEFAULT, 
               AilOverlayImage, 
               DATAMATRIX_REGION_TOP_LEFT_X,  
               DATAMATRIX_REGION_TOP_LEFT_Y+120, 
               " 2D Data Matrix code:                  ")
      AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, DATAMATRIX_REGION_TOP_LEFT_X + 200, 
               DATAMATRIX_REGION_TOP_LEFT_Y + 120, OutputString)
      AIL.MgraRect(AIL.M_DEFAULT, AilOverlayImage,
               DATAMATRIX_REGION_TOP_LEFT_X, DATAMATRIX_REGION_TOP_LEFT_Y,
               DATAMATRIX_REGION_TOP_LEFT_X + DATAMATRIX_REGION_SIZE_X, 
               DATAMATRIX_REGION_TOP_LEFT_Y + DATAMATRIX_REGION_SIZE_Y)

   # Free objects. 
   AIL.McodeFree(DataMatrixCode)
   AIL.MbufFree(DataMatrixRegion)

   # Free results buffer. 
   AIL.McodeFree(CodeResults)

   # Pause to show the results. 
   if DataMatrixStatus == AIL.M_STATUS_READ_OK and BarcodeStatus == AIL.M_STATUS_READ_OK:
      print("Decoding was successful and the strings were written under each code.")
   else:
      print("Decoding error found.")
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free other allocations. 
   AIL.MbufFree(AilImage)
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0

if __name__ == "__main__":
   McodeExample()

```
