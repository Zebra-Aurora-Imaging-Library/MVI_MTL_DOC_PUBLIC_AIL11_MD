---
title: "Mblob"
description: "This example uses the Blob module to analyze an image of some nuts and bolts."
ms.language: "Python"
---

# Mblob

> This example uses the Blob module to analyze an image of some nuts and bolts.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobGetResult`, `MblobSelect`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraFree`, `MimBinarize`, `MimClose`, `MimOpen`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Graphics, Image Processing, Blob Analysis, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#*****************************************************************************
#
# File name: AIL.Mblob.py
#
# Synopsis:  This program loads an image of some nuts, bolts and washers, 
#             determines the number of each of these, finds and marks
#             their center of gravity using the Blob analysis module.
# 
#  (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
#  All Rights Reserved
#

import ail11 as AIL
 
# Target image file specifications.
IMAGE_FILE            = AIL.M_IMAGE_PATH + "BoltsNutsWashers.mim"
IMAGE_THRESHOLD_VALUE = 26 

# Minimum and maximum area of blobs.
MIN_BLOB_AREA         = 50 
MAX_BLOB_AREA         = 50000

# Radius of the smallest particles to keep. 
MIN_BLOB_RADIUS       = 3

# Minimum hole compactness corresponding to a washer. 
MIN_COMPACTNESS       = 1.5


def MblobExample():

   # Allocate defaults. 
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Restore source image into image buffer.  
   AilImage = AIL.MbufRestore(IMAGE_FILE, AilSystem)

   # Allocate a graphic list to hold the subpixel annotations to draw. 
   AilGraphicList = AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT)

   # Associate the graphic list to the display. 
   AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList)

   # Display the buffer. 
   AIL.MdispSelect(AilDisplay, AilImage)

   # Allocate a binary image buffer for fast processing. 
   SizeX = AIL.MbufInquire(AilImage, AIL.M_SIZE_X)
   SizeY = AIL.MbufInquire(AilImage, AIL.M_SIZE_Y)
   AilBinImage = AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 1+AIL.M_UNSIGNED, AIL.M_IMAGE+AIL.M_PROC)

   # Pause to show the original image.  
   print("BLOB ANALYSIS:")
   print("--------------\n")
   print("This program determines the number of bolts, nuts and washers")
   print("in the image and finds their center of gravity.")
   print("Press any key to continue.\n")
   AIL.MosGetch()
 
   # Binarize image. 
   AIL.MimBinarize(AilImage, AilBinImage, AIL.M_FIXED+AIL.M_GREATER_OR_EQUAL, IMAGE_THRESHOLD_VALUE, AIL.M_NULL)

   # Remove small particles and then remove small holes. 
   AIL.MimOpen(AilBinImage, AilBinImage, MIN_BLOB_RADIUS, AIL.M_BINARY)
   AIL.MimClose(AilBinImage, AilBinImage, MIN_BLOB_RADIUS, AIL.M_BINARY)
 
   # Allocate a context.  
   AilBlobContext = AIL.MblobAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT)
  
   # Enable the Center Of Gravity feature calculation.  
   AIL.MblobControl(AilBlobContext, AIL.M_CENTER_OF_GRAVITY + AIL.M_BINARY, AIL.M_ENABLE)
 
   # Allocate a blob result buffer. 
   AilBlobResult = AIL.MblobAllocResult(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT) 
 
   # Calculate selected features for each blob.  
   AIL.MblobCalculate(AilBlobContext, AilBinImage, AIL.M_NULL, AilBlobResult)
 
   # Exclude blobs whose area is too small.  
   AIL.MblobSelect(AilBlobResult, AIL.M_EXCLUDE, AIL.M_AREA, AIL.M_LESS_OR_EQUAL, MIN_BLOB_AREA, AIL.M_NULL) 
 
   # Get the total number of selected blobs.  
   TotalBlobs = AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT)
   print("There are {} objects and their centers of gravity are:".format(TotalBlobs)) 
  
   # Get the resuls. 
   CogX = AIL.MblobGetResult(AilBlobResult, AIL.M_INCLUDED_BLOBS, AIL.M_CENTER_OF_GRAVITY_X + AIL.M_BINARY)
   CogY = AIL.MblobGetResult(AilBlobResult, AIL.M_INCLUDED_BLOBS, AIL.M_CENTER_OF_GRAVITY_Y + AIL.M_BINARY)
          
   for n in range (0, TotalBlobs):
      print("Blob #{}: X={:5.1f}, Y={:5.1f}".format(int(n), CogX[n], CogY[n]))
  
   # Draw a cross at the center of gravity of each blob.   
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED)
   AIL.MblobDraw(AIL.M_DEFAULT, AilBlobResult, AilGraphicList, AIL.M_DRAW_CENTER_OF_GRAVITY, AIL.M_INCLUDED_BLOBS, AIL.M_DEFAULT)

   # Reverse what is considered to be the background so that holes are seen as being blobs. 
    
   AIL.MblobControl(AilBlobContext, AIL.M_FOREGROUND_VALUE, AIL.M_ZERO)

   # Add a feature to distinguish between types of holes. Since area
   # has already been added to the context, and the processing 
   # mode has been changed, all blobs will be re-included and the area 
   # of holes will be calculated automatically.
    
   AIL.MblobControl(AilBlobContext, AIL.M_COMPACTNESS, AIL.M_ENABLE)

   # Calculate selected features for each blob. 
   AIL.MblobCalculate(AilBlobContext, AilBinImage, AIL.M_NULL, AilBlobResult)

   # Exclude small holes and large (the area around objects) holes. 
   AIL.MblobSelect(AilBlobResult, AIL.M_EXCLUDE, AIL.M_AREA, AIL.M_OUT_RANGE, MIN_BLOB_AREA, MAX_BLOB_AREA)

   # Get the number of blobs with holes. 
   BlobsWithHoles = AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT)

   # Exclude blobs whose holes are compact (i.e. nuts). 
   AIL.MblobSelect(AilBlobResult, AIL.M_EXCLUDE, AIL.M_COMPACTNESS, AIL.M_LESS_OR_EQUAL, MIN_COMPACTNESS, AIL.M_NULL)

   # Get the number of blobs with holes that are NOT compact. 
   BlobsWithRoughHoles = AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT)

   # Print results. 
   print("\nIdentified objects:")
   print("{} bolts".format(int(TotalBlobs-BlobsWithHoles)))
   print("{} nuts".format(int(BlobsWithHoles - BlobsWithRoughHoles)))
   print("{} washers\n".format(int(BlobsWithRoughHoles)))
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.MgraFree(AilGraphicList)
   AIL.MblobFree(AilBlobResult) 
   AIL.MblobFree(AilBlobContext)
   AIL.MbufFree(AilBinImage)
   AIL.MbufFree(AilImage)
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0

if __name__ == "__main__":
   MblobExample()

```
