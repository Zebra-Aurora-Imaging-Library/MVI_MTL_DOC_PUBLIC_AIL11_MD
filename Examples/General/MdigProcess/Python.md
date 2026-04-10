---
title: "MdigProcess"
description: "This program shows the use of the MdigProcess() function to perform real-time processing."
ms.language: "Python"
---

# MdigProcess

> This program shows the use of the MdigProcess() function to perform real-time processing.

**Language:** Python

**Functions used:** `MdigGrabContinuous`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MimArith`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# 
#  File name: MdigProcess.py  
#
#   Synopsis:  This program shows the use of the MdigProcess() function and its multiple
#              buffering acquisition to do robust real-time processing.           
#  
#              The user's processing code to execute is located in a callback function 
#              that will be called for each frame acquired (see ProcessingFunction()).
#    
#        Note: The average processing time must be shorter than the grab time or some
#              frames will be missed. Also, if the processing results are not displayed
#              and the frame count is not drawn or printed, the CPU usage is reduced 
#              significantly.
#
#  (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
#  All Rights Reserved

import ail11 as AIL

# User's processing function hook data structure.
class HookDataStruct():
   def __init__(self, AilDigitizer, AilImageDisp, ProcessedImageCount):
      self.AilDigitizer = AilDigitizer
      self.AilImageDisp = AilImageDisp
      self.ProcessedImageCount = ProcessedImageCount

# Number of images in the buffering grab queue.
# Generally, increasing this number gives a better real-time grab.
BUFFERING_SIZE_MAX = 20

# User's processing function called every time a grab buffer is ready.
# --------------------------------------------------------------------

# Local defines. 
STRING_POS_X       = 20 
STRING_POS_Y       = 20 

def ProcessingFunction(HookType, HookId, HookDataPtr):
   
   # Retrieve the AIL_ID of the grabbed buffer. 
   ModifiedBufferId = AIL.MdigGetHookInfo(HookId, AIL.M_MODIFIED_BUFFER + AIL.M_BUFFER_ID)
   
   # Extract the userdata structure
   UserData = HookDataPtr

   # Increment the frame counter.
   UserData.ProcessedImageCount += 1
   
   # Print and draw the frame count (remove to reduce CPU usage).
   print("Processing frame #{:d}.\r".format(UserData.ProcessedImageCount), end='')
   AIL.MgraText(AIL.M_DEFAULT, ModifiedBufferId, STRING_POS_X, STRING_POS_Y, "{:d}".format(UserData.ProcessedImageCount))
   
   # Execute the processing and update the display.
   AIL.MimArith(float(ModifiedBufferId), 0.0, UserData.AilImageDisp, AIL.M_NOT)
   
   return 0
   
# Main function.
# ---------------

def MdigProcessExample():
   # Allocate defaults.
   AilApplication = AIL.MappAlloc("M_DEFAULT", AIL.M_DEFAULT)
   AilSystem = AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AilDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT)
   AilDigitizer = AIL.MdigAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT)

   SizeX = AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X)
   SizeY = AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y)

   AilImageDisp = AIL.MbufAlloc2d(AilSystem,
                                     SizeX,
                                     SizeY, 
                                     8 + AIL.M_UNSIGNED, 
                                     AIL.M_IMAGE + 
                                     AIL.M_PROC + AIL.M_DISP + AIL.M_GRAB)

   AIL.MbufClear(AilImageDisp, AIL.M_COLOR_BLACK)
   AIL.MdispSelect(AilDisplay, AilImageDisp)
     
   # Print a message.
   print("\nMULTIPLE BUFFERED PROCESSING.")
   print("-----------------------------\n")

   # Grab continuously on the display and wait for a key press
   AIL.MdigGrabContinuous(AilDigitizer, AilImageDisp)
   print("Press any key to start processing.\n")
   AIL.MosGetch()

   # Halt continuous grab. 
   AIL.MdigHalt(AilDigitizer)

   # Allocate the grab buffers and clear them.
   AilGrabBufferList = []
   AilGrabBufferListSize = 0
   for n in range(0, BUFFERING_SIZE_MAX):
      if (n == 2):
         AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE)
      AilGrabBufferList.append(AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC))
      if (AilGrabBufferList[n] != AIL.M_NULL):
         AIL.MbufClear(AilGrabBufferList[n], 0xFF)
         AilGrabBufferListSize += 1
      else:
         break
   AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE)

   # Initialize the user's processing function data structure.
   UserHookData = HookDataStruct(AilDigitizer, AilImageDisp, 0)
   
   # Start the processing. The processing function is called with every frame grabbed.
   ProcessingFunctionPtr = AIL.AIL_DIG_HOOK_FUNCTION_PTR(ProcessingFunction)
   AIL.MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize, AIL.M_START, AIL.M_DEFAULT, ProcessingFunctionPtr, UserHookData)

   # Here the main() is free to perform other tasks while the processing is executing.
   # ---------------------------------------------------------------------------------

   # Print a message and wait for a key press after a minimum number of frames.
   print("Press any key to stop.                    \n")
   AIL.MosGetch()

   # Stop the processing.
   AIL.MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize, AIL.M_STOP, AIL.M_DEFAULT, ProcessingFunctionPtr, UserHookData)

   # Print statistics.
   ProcessFrameCount = AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_COUNT)
   ProcessFrameRate = AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_RATE)

   if (ProcessFrameRate > 0.0):
      print("\n\n{:d} frames grabbed at {:.1f} frames/sec ({:.1f} ms/frame).".format(ProcessFrameCount, ProcessFrameRate, 1000.0/ProcessFrameRate))
   else:
      print("\n\nNo data was grabbed.\n")

   print("Press any key to end.\n")
   AIL.MosGetch()
      
   # Free the grab buffers.
   for id in range(0, AilGrabBufferListSize):
      AIL.MbufFree(AilGrabBufferList[id])
      
   # Release defaults.
   AIL.MbufFree(AilImageDisp)
   AIL.MdispFree(AilDisplay)
   AIL.MdigFree(AilDigitizer)
   AIL.MsysFree(AilSystem)
   AIL.MappFree(AilApplication)
   
   return
   
if __name__ == "__main__":
   MdigProcessExample()

```
