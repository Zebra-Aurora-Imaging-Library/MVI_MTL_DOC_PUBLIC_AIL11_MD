---
title: "Mfunc"
description: "This example shows the use of the Function Developer Toolkit and how AIL and custom code can be mixed to create a custom AIL function that accesses the data pointer of an AIL buffer directly in order to process it. The example creates a Main AIL function that registers all the parameters to AIL and calls the Worker function. The Worker function retrieves all the parameters, gets the pointers to the AIL image buffers, uses them to access the data directly, and adds a constant."
ms.language: "Python"
---

# Mfunc

> This example shows the use of the Function Developer Toolkit and how AIL and custom code can be mixed to create a custom AIL function that accesses the data pointer of an AIL buffer directly in order to process it. The example creates a Main AIL function that registers all the parameters to AIL and calls the Worker function. The Worker function retrieves all the parameters, gets the pointers to the AIL image buffers, uses them to access the data directly, and adds a constant.

**Language:** Python

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufControl`, `MbufDiskInquire`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MdispAlloc`, `MdispSelect`, `MsysAlloc`, `MsysFree`, `MsysInquire`, `MfuncAlloc`, `MfuncParamAilId`, `MfuncParamDataPointer`, `MfuncCall`, `MfuncErrorReport`, `MfuncFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: MFunc.py 
#
# Synopsis:  This example shows the use of the function Developer Toolkit and how 
#            Aurora Imaging Library and custom code can be mixed to create a custom function that 
#            accesses the data pointer of a buffer directly in order to process it.
#
#            The example creates a User function that registers all the parameters 
#            to Aurora Imaging Library and calls the Worker function. The Worker function retrieves
#            all the parameters, gets the pointers to the image buffers, uses them to access 
#            the data directly and adds a constant.
#             
#            Note: The images must be 8-bit unsigned and have a valid Host pointer.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################


import ail11 as AIL
import ctypes
import os

# Function specifications. 
FUNCTION_NB_PARAM             = 4
FUNCTION_OPCODE_ADD_CONSTANT  = 1 
FUNCTION_PARAMETER_ERROR_CODE = 1
FUNCTION_SUPPORTED_IMAGE_TYPE = 8 + AIL.M_UNSIGNED

# Target image file name. 
IMAGE_FILE = os.path.join(AIL.M_IMAGE_PATH, "BoltsNutsWashers.mim")

# User function definition. #
# ------------------------------- #
def AddConstant(SrcImageId, DstImageId, ConstantToAdd):
   Func = AIL.AIL_ID(0)
   WorkerReturnValue = AIL.AIL_INT(0)

   WorkerAddConstantPtr = AIL.AIL_FUNC_FUNCTION_PTR(WorkerAddConstant)

   # Allocate a function context that will be used to call a target 
   #  Worker function locally on the Host to do the processing.
   AIL.MfuncAlloc("AddConstant", FUNCTION_NB_PARAM, 
                  WorkerAddConstantPtr, AIL.M_NULL, AIL.M_NULL,
                  AIL.M_USER_MODULE_1 + FUNCTION_OPCODE_ADD_CONSTANT, 
                  AIL.M_LOCAL + AIL.M_SYNCHRONOUS_FUNCTION,
                  ctypes.byref(Func))

   # Register the parameters.
   AIL.MfuncParamAilId(Func.value, 1, SrcImageId, AIL.M_IMAGE, AIL.M_IN)
   AIL.MfuncParamAilId(Func.value, 2, DstImageId, AIL.M_IMAGE, AIL.M_OUT)
   AIL.MfuncParamAilInt(Func.value, 3, ConstantToAdd)
   AIL.MfuncParamDataPointer(Func.value, 4, ctypes.byref(WorkerReturnValue), ctypes.sizeof(AIL.AIL_INT), AIL.M_OUT)

   # Call the target Worker function. 
   AIL.MfuncCall(Func.value)

   # Free the function context. 
   AIL.MfuncFree(Func.value)
   
   # Return the value received from the Worker function. 
   return WorkerReturnValue.value

# Worker function definition. #
# ------------------------------ #
def WorkerAddConstant(Func):
   SrcImageId = AIL.AIL_ID(0)
   DstImageId = AIL.AIL_ID(0)

   ConstantToAdd = AIL.AIL_INT(0)
   TempValue = AIL.AIL_INT(0)

   WorkerReturnValueMemAddr = AIL.AIL_INT(0)

   # Read the parameters. 
   AIL.MfuncParamValue(Func, 1, ctypes.byref(SrcImageId))
   AIL.MfuncParamValue(Func, 2, ctypes.byref(DstImageId))
   AIL.MfuncParamValue(Func, 3, ctypes.byref(ConstantToAdd))
   AIL.MfuncParamValue(Func, 4, ctypes.byref(WorkerReturnValueMemAddr))

   # Lock buffers for direct access.
   AIL.MbufControl(SrcImageId.value, AIL.M_LOCK, AIL.M_DEFAULT)
   AIL.MbufControl(DstImageId.value, AIL.M_LOCK, AIL.M_DEFAULT)

   # Read image information
   SrcImageMemAddr = AIL.MbufInquire(SrcImageId.value, AIL.M_HOST_ADDRESS)
   SrcImageSizeX = AIL.MbufInquire(SrcImageId.value, AIL.M_SIZE_X)
   SrcImageSizeY = AIL.MbufInquire(SrcImageId.value, AIL.M_SIZE_Y)
   SrcImageType = AIL.MbufInquire(SrcImageId.value, AIL.M_TYPE)
   SrcImagePitchByte = AIL.MbufInquire(SrcImageId.value, AIL.M_PITCH_BYTE)
   DstImageMemAddr = AIL.MbufInquire(DstImageId.value, AIL.M_HOST_ADDRESS)
   DstImageSizeX = AIL.MbufInquire(DstImageId.value, AIL.M_SIZE_X)
   DstImageSizeY = AIL.MbufInquire(DstImageId.value, AIL.M_SIZE_Y)
   DstImageType = AIL.MbufInquire(DstImageId.value, AIL.M_TYPE)
   DstImagePitchByte = AIL.MbufInquire(DstImageId.value, AIL.M_PITCH_BYTE)

   # Cast the addresses to a pointer of uint8
   SrcImageDataPtr = ctypes.cast(SrcImageMemAddr, ctypes.POINTER(ctypes.c_uint8))
   DstImageDataPtr = ctypes.cast(DstImageMemAddr, ctypes.POINTER(ctypes.c_uint8))

   # Reduce the destination area to process if necessary. 
   if SrcImageSizeX < DstImageSizeX:
      DstImageSizeX = SrcImageSizeX
   if SrcImageSizeY < DstImageSizeY:
      DstImageSizeY = SrcImageSizeY

   # If images have the proper type and a valid host pointer,
   # execute the operation using custom C code.
   if (SrcImageType == DstImageType and SrcImageType == FUNCTION_SUPPORTED_IMAGE_TYPE 
      and SrcImageDataPtr != AIL.M_NULL and DstImageDataPtr != AIL.M_NULL):
      for y in range(DstImageSizeY):
         for x in range(DstImageSizeX):
            # Calculate the value to write. 
            TempValue = SrcImageDataPtr[x] + ConstantToAdd.value

            # Write the value if no overflow, else saturate. 
            if (TempValue <= 0xff):
               DstImageDataPtr[x] = ctypes.c_uint8(TempValue).value
            else:
               DstImageDataPtr[x] = 0xff

         # Move pointer to the next line taking into account the image's pitch. 
         SrcImageMemAddr += SrcImagePitchByte
         DstImageMemAddr += DstImagePitchByte
         SrcImageDataPtr = ctypes.cast(SrcImageMemAddr, ctypes.POINTER(ctypes.c_uint8))
         DstImageDataPtr = ctypes.cast(DstImageMemAddr, ctypes.POINTER(ctypes.c_uint8))
      
      # Get the variable WorkerReturnValue at the memory address to modify it 
      WorkerReturnValuePtr = ctypes.cast(WorkerReturnValueMemAddr.value, ctypes.POINTER(AIL.AIL_INT))
      
      # Return a null error code to the User function. 
      WorkerReturnValuePtr.contents.value = AIL.M_NULL
      

   else:
      # Buffer cannot be processed. Report an error. 
      AIL.MfuncErrorReport(Func,AIL.M_FUNC_ERROR + FUNCTION_PARAMETER_ERROR_CODE,
                           "Invalid parameter.",
                           "Image type not supported or invalid target system.",
                           "Image must be 8-bit and have a valid host address.",
                           AIL.M_NULL
                           )

      # Get the variable WorkerReturnValue at the memory address to modify it 
      WorkerReturnValuePtr = ctypes.cast(WorkerReturnValueMemAddr.value, ctypes.POINTER(AIL.AIL_INT))

      # Return an error code to the User function. 
      WorkerReturnValuePtr.contents.value = AIL.M_FUNC_ERROR + FUNCTION_PARAMETER_ERROR_CODE

   # Unlock buffers.
   AIL.MbufControl(SrcImageId.value, AIL.M_UNLOCK, AIL.M_DEFAULT)
   AIL.MbufControl(DstImageId.value, AIL.M_UNLOCK, AIL.M_DEFAULT)

   # Signal to Aurora Imaging Library that the destination buffer was modified. 
   AIL.MbufControl(DstImageId.value, AIL.M_MODIFIED, AIL.M_DEFAULT); 


# Main to test the custom function. #
# --------------------------------- #
def MfuncExample():
   # Allocate default application, system and display. 
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Load source image into a Host memory image buffer.
   AilImage = AIL.MbufAlloc2d(AilSystem, 
                              AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_X, AIL.M_NULL), 
                              AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_Y, AIL.M_NULL), 
                              8 + AIL.M_UNSIGNED, 
                              AIL.M_IMAGE + AIL.M_DISP + AIL.M_HOST_MEMORY,
                              )

   AIL.MbufLoad(IMAGE_FILE, AilImage)

   # Display the image.
   AIL.MdispSelect(AilDisplay, AilImage)

   # Pause.
   print("\nFunction Developer Toolkit:")
   print("---------------------------------\n")
   print("This example creates a custom function that processes")
   print("an image using its data pointer directly.\n")
   print("Target image was loaded.")
   print("Press a key to continue.\n")
   AIL.MosGetch()

   # Run the custom function only if the target system's memory is local and accessible. 
   if AIL.MsysInquire(AilSystem, AIL.M_LOCATION, AIL.M_NULL) == AIL.M_LOCAL:
      # Process the image directly with the custom function. 
      ReturnValue = AddConstant(AilImage, AilImage, 0x40)

      # Print a conclusion message. 
      if ReturnValue == AIL.M_NULL:
         print("The white level of the image was augmented.")
      else:
         print("An error was returned by the Worker function.")

   else:
      # Print that the example don't run remotely. 
      print("This example doesn't run with Distributed AIL.")

   # Wait for a key to terminate. 
   print("Press a key to terminate.\n")
   AIL.MosGetch()

   # Free all allocations.
   AIL.MbufFree(AilImage)
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0 
   
if __name__ == "__main__":
   MfuncExample()

```
