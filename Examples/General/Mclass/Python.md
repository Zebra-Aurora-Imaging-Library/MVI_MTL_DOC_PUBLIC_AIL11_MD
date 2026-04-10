---
title: "Mclass"
description: "This example identifies the type of product using a pre-trained classification module."
ms.language: "Python"
---

# Mclass

> This example identifies the type of product using a pre-trained classification module.

**Language:** Python

**Functions used:** `MclassPredict`, `MclassPreprocess`, `MclassRestore`, `MclassGetResult`, `MclassInquire`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraControl`, `MgraFont`, `MgraRect`, `MgraText`, `MimResize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Identifying, Inspecting/Proofing/Verifying, Modules, Classification, Buffer, Display, Graphics, Image Processing, What's New, Older, AIL 10.0 U94

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#*************************************************************************************
#
# File name: Mclass.py
#
# Synopsis:  This example identifies the type of pastas using a 
# pre-trained classification module. 
#
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved

import ail11 as AIL
import os

# Path definitions.
EXAMPLE_IMAGE_DIR_PATH   = os.path.join(AIL.M_IMAGE_PATH, "Classification", "Pasta")
EXAMPLE_CLASS_CTX_PATH   = os.path.join(EXAMPLE_IMAGE_DIR_PATH, "PastaEx.mclass")
TARGET_IMAGE_DIR_PATH    = os.path.join(EXAMPLE_IMAGE_DIR_PATH, "Products")

DIG_IMAGE_FOLDER         = TARGET_IMAGE_DIR_PATH
DIG_REMOTE_IMAGE_FOLDER  = "remote:///" + TARGET_IMAGE_DIR_PATH

# Util constant.
BUFFERING_SIZE_MAX = 10

#/****************************************************************************
#    Main.
#/****************************************************************************

# User's processing function hook data structure.
class HookDataStruct():
   def __init__(self, ClassCtx, ClassRes, AilDisplay, AilDispChild, NumberOfCategories, AilOverlayImage, SourceSizeX, SourceSizeY, NumberOfFrames):      
      self.ClassCtx = ClassCtx
      self.ClassRes = ClassRes
      self.AilDisplay = AilDisplay
      self.AilDispChild = AilDispChild
      self.NbCategories = NumberOfCategories
      self.AilOverlayImage = AilOverlayImage
      self.SourceSizeX = SourceSizeX
      self.SourceSizeY = SourceSizeY
      self.NbOfFrames = NumberOfFrames

def MclassExample():
   NumberOfCategories = 0

   # Allocate objects.
   AilApplication = AIL.MappAlloc(AIL.M_NULL, AIL.M_DEFAULT)
   AilSystem = AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   SystemType = AIL.MsysInquire(AilSystem, AIL.M_SYSTEM_TYPE, AIL.M_NULL)
   if SystemType != AIL.M_SYSTEM_HOST_TYPE:
      AIL.MsysFree(AilSystem)
      AilSystem = AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_HOST, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL)
                
   AilDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, ("M_DEFAULT"), AIL.M_DEFAULT)

   AilSystemLocation = AIL.MsysInquire(AilSystem, AIL.M_LOCATION, AIL.M_NULL);
   DigImageFolder = DIG_IMAGE_FOLDER
   if AilSystemLocation == AIL.M_REMOTE:
      DigImageFolder = DIG_REMOTE_IMAGE_FOLDER
   AilDigitizer = AIL.MdigAlloc(AilSystem, AIL.M_DEFAULT, DigImageFolder, AIL.M_DEFAULT)

   # Print the example synopsis.
   print("[EXAMPLE NAME]")
   print("Mclass\n")
   print("[SYNOPSIS]")
   print("This programs shows the use of a pre-trained classification")
   print("tool to recognize product categories.\n")
   print("[MODULES USED]")
   print("Classification, Buffer, Display, Graphics, Image Processing.\n")

   # Wait for user.
   print("Press any key to continue.\n")
   AIL.MosGetch()
   
   print("Restoring the classification context from file..", end="")
   ClassCtx = AIL.MclassRestore(EXAMPLE_CLASS_CTX_PATH, AilSystem, AIL.M_DEFAULT)
   print(".", end="")

   # Preprocess the context.
   AIL.MclassPreprocess(ClassCtx, AIL.M_DEFAULT)
   print(".ready.")

   NumberOfCategories = AIL.MclassInquire(ClassCtx, AIL.M_CONTEXT, AIL.M_NUMBER_OF_CLASSES + AIL.M_TYPE_AIL_INT)
   InputSizeX = AIL.MclassInquire(ClassCtx, AIL.M_DEFAULT_SOURCE_LAYER, AIL.M_SIZE_X + AIL.M_TYPE_AIL_INT)
   InputSizeY = AIL.MclassInquire(ClassCtx, AIL.M_DEFAULT_SOURCE_LAYER, AIL.M_SIZE_Y + AIL.M_TYPE_AIL_INT)

   if(NumberOfCategories > 0):
      # Inquire and print source layer information.
      print(" - The classifier was trained to recognize {:d} categories".format(NumberOfCategories))
      print(" - The classifier was trained for {:d}x{:d} source images\n".format(InputSizeX, InputSizeY))

      # Allocate a classification result buffer.
      ClassRes = AIL.MclassAllocResult(AilSystem, AIL.M_PREDICT_CNN_RESULT, AIL.M_DEFAULT)

      # Inquire the size of the source image.
      SourceSizeX = AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X)
      SourceSizeY = AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y)

      # Setup the example display.
      AilDispImage, AilDispChild, AilOverlayImage = SetupDisplay(AilSystem, AilDisplay, SourceSizeX, SourceSizeY, ClassCtx, NumberOfCategories)

      # Retrieve the number of frame in the source directory.
      NumberOfFrames = AIL.MdigInquire(AilDigitizer, AIL.M_SOURCE_NUMBER_OF_FRAMES)

      AilGrabBufferList = [0]*BUFFERING_SIZE_MAX   # image identifier
      AilChildBufferList = [0]*BUFFERING_SIZE_MAX  # child identifier

      # Allocate the grab buffers.
      for BufIndex in range(0, BUFFERING_SIZE_MAX):
         AilGrabBufferList[BufIndex] = AIL.MbufAlloc2d(AilSystem, SourceSizeX, SourceSizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC)
         AilChildBufferList[BufIndex] = AIL.MbufChild2d(AilGrabBufferList[BufIndex], int((SourceSizeX - InputSizeX) / 2), int((SourceSizeY - InputSizeY) / 2), InputSizeX, InputSizeY)
         AIL.MobjControl(AilGrabBufferList[BufIndex], AIL.M_OBJECT_USER_DATA_PTR, AilChildBufferList[BufIndex])
         
      
      # Initialize the user's processing function data structure.
      ClassificationData = HookDataStruct(ClassCtx, ClassRes, AilDisplay, AilDispChild, NumberOfCategories, AilOverlayImage, SourceSizeX, SourceSizeY, NumberOfFrames)
   
      # Start the processing. The processing function is called with every frame grabbed.
      ClassificationFuncPtr = AIL.AIL_DIG_HOOK_FUNCTION_PTR(ClassificationFunc)
      
      # Start the grab.
      if(NumberOfFrames != AIL.M_INFINITE):
         ClassificationData = AIL.MdigProcess(AilDigitizer, AilGrabBufferList, BUFFERING_SIZE_MAX, AIL.M_SEQUENCE + AIL.M_COUNT(NumberOfFrames), AIL.M_SYNCHRONOUS, ClassificationFuncPtr, ClassificationData)
      else:
         ClassificationData = AIL.MdigProcess(AilDigitizer, AilGrabBufferList, BUFFERING_SIZE_MAX, AIL.M_START, AIL.M_DEFAULT, ClassificationFuncPtr, ClassificationData)

      # Ready to exit.
      print("\nPress any key to exit.")
      AIL.MosGetch()

      # Stop the digitizer.
      AIL.MdigProcess(AilDigitizer, AilGrabBufferList, BUFFERING_SIZE_MAX, AIL.M_STOP, AIL.M_DEFAULT, ClassificationFuncPtr, ClassificationData)

      AIL.MbufFree(AilDispChild)
      AIL.MbufFree(AilDispImage)

      for BufIndex in range(0, BUFFERING_SIZE_MAX):
        AIL.MbufFree(AilChildBufferList[BufIndex])
        AIL.MbufFree(AilGrabBufferList[BufIndex])
         
      AIL.MclassFree(ClassRes)
      AIL.MclassFree(ClassCtx)

   # Free the allocated resources.
   AIL.MdigFree(AilDigitizer)
   AIL.MdispFree(AilDisplay)
   AIL.MsysFree(AilSystem)
   AIL.MappFree(AilApplication)

   return 0
   

def SetupDisplay(AilSystem, AilDisplay, SourceSizeX, SourceSizeY, ClassCtx, NbCategories):
   # Allocate a color buffer.
   IconSize = int(SourceSizeY / NbCategories)
   AilDispImage = AIL.MbufAllocColor(AilSystem, 3, SourceSizeX + IconSize, SourceSizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, AIL.M_NULL)
   AIL.MbufClear(AilDispImage, AIL.M_COLOR_BLACK)
   AilDispChild = AIL.MbufChild2d(AilDispImage, 0, 0, SourceSizeX, SourceSizeY, AIL.M_NULL)

   # Set annotation color.
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED)

   # Setup the display.
   for iter in range(0, NbCategories):

      # Allocate a child buffer per product categorie.   
      AilChildSample = AIL.MbufChild2d(AilDispImage, SourceSizeX, iter * IconSize, IconSize, IconSize)

      # Load the sample image.
      AilImageLoader = AIL.MclassInquire(ClassCtx, AIL.M_CLASS_INDEX(iter), AIL.M_CLASS_ICON_ID + AIL.M_TYPE_AIL_ID)
      
      if (AilImageLoader != AIL.M_NULL):
         AIL.MimResize(AilImageLoader, AilChildSample, AIL.M_FILL_DESTINATION, AIL.M_FILL_DESTINATION, AIL.M_BICUBIC + AIL.M_OVERSCAN_FAST) 

      # Draw an initial red rectangle around the buffer.
      AIL.MgraRect(AIL.M_DEFAULT, AilChildSample, 0, 1, IconSize - 1, IconSize - 2)

      # Free the allocated buffers.
      AIL.MbufFree(AilChildSample)
      

   # Display the window with black color.
   AIL.MdispSelect(AilDisplay, AilDispImage)

   # Prepare for overlay annotations.
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlay = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, AIL.M_NULL)

   return AilDispImage, AilDispChild, AilOverlay


def ClassificationFunc(HookType, EventId, DataPtr):
   AilImage = AIL.MdigGetHookInfo(EventId, AIL.M_MODIFIED_BUFFER + AIL.M_BUFFER_ID)

   data = DataPtr
   AIL.MdispControl(data.AilDisplay, AIL.M_UPDATE, AIL.M_DISABLE)
   pAilInputImage = AIL.MobjInquire(AilImage, AIL.M_OBJECT_USER_DATA_PTR)
   
   # Display the new target image.
   AIL.MbufCopy(AilImage, data.AilDispChild)

   # Perform product recognition using the classification module.
   AIL.MclassPredict(data.ClassCtx, pAilInputImage, data.ClassRes, AIL.M_DEFAULT)

   # Retrieve best classification score and class index.
   BestScore = AIL.MclassGetResult(data.ClassRes, AIL.M_GENERAL, AIL.M_BEST_CLASS_SCORE + AIL.M_TYPE_AIL_DOUBLE)[0]

   BestIndex = AIL.MclassGetResult(data.ClassRes, AIL.M_GENERAL, AIL.M_BEST_CLASS_INDEX + AIL.M_TYPE_AIL_INT)[0]

   # Clear the overlay buffer.
   AIL.MdispControl(data.AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_TRANSPARENT_COLOR)
   
   # Draw a green rectangle around the winning sample.
   IconSize = int(data.SourceSizeY / data.NbCategories)
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN)
   AIL.MgraRect(AIL.M_DEFAULT, data.AilOverlayImage, data.SourceSizeX, (BestIndex*IconSize)+1, data.SourceSizeX + IconSize - 1, (BestIndex + 1)*IconSize - 2)

   # Print the classification accuracy in the sample buffer.
   Accuracy_text = "{:.1f}%".format(BestScore) + " score"
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKGROUND_MODE, AIL.M_TRANSPARENT)
   AIL.MgraFont(AIL.M_DEFAULT, AIL.M_FONT_DEFAULT_SMALL)
   AIL.MgraText(AIL.M_DEFAULT, data.AilOverlayImage, data.SourceSizeX+ 2, BestIndex*IconSize + 4, Accuracy_text)

   # Update the display.
   AIL.MdispControl(data.AilDisplay, AIL.M_UPDATE, AIL.M_ENABLE)

   # Wait for the user.
   if (data.NbOfFrames != AIL.M_INFINITE):
      print("Press any key to continue.", end="\r")
      AIL.MosGetch()
      
   return 0
   
if __name__ == "__main__":
   MclassExample()

```
