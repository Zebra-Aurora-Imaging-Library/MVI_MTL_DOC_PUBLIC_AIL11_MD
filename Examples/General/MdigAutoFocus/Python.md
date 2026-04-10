---
title: "MdigAutoFocus"
description: "This program performs an autofocus operation using the MdigFocus() function. Since the way to move a motorized camera lens is device-specific, we will not include real lens movement control and image grab but will simulate the lens focus with a smooth operation."
ms.language: "Python"
---

# MdigAutoFocus

> This program performs an autofocus operation using the MdigFocus() function. Since the way to move a motorized camera lens is device-specific, we will not include real lens movement control and image grab but will simulate the lens focus with a smooth operation.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufClear`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdigFocus`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraLine`, `MimConvolve`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Graphics, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: MdigAutoFocus.py
#
# Synopsis:  This program performs an autofocus operation using the 
#            MdigFocus() function. Since the way to move a motorized
#            camera lens is device-specific, we will not include real
#            lens movement control and image grab but will simulate 
#            the lens focus with a smooth operation. 
#
#     Note : Under AIL-Lite, the out of focus lens simulation is not supported.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################

 
import ail11 as AIL
import os

# Source image file specification. 
IMAGE_FILE                    = os.path.join(AIL.M_IMAGE_PATH, "BaboonMono.mim")

# Lens mechanical characteristics.
FOCUS_MAX_NB_POSITIONS        = 100
FOCUS_MIN_POSITION            = 0
FOCUS_MAX_POSITION            = (FOCUS_MAX_NB_POSITIONS - 1)
FOCUS_START_POSITION          = 10

# Autofocus search properties.
FOCUS_MAX_POSITION_VARIATION  = AIL.M_DEFAULT
FOCUS_MODE                    = AIL.M_SMART_SCAN
FOCUS_SENSITIVITY             = 1

# User Data structure definition.
class DigHookUserData:
   def __init__(self):
      self.SourceImage = None
      self.FocusImage = None
      self.Display = None
      self.Iteration = None


#*****************************************************************************#
#  Main application function.                                                 #
def MdigAutoFocusExample():
   UserData = DigHookUserData()

   # Allocate defaults. 
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT,  DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Load the source image. 
   AilSource = AIL.MbufRestore(IMAGE_FILE, AilSystem)
   AilCameraFocus = AIL.MbufRestore(IMAGE_FILE, AilSystem)
   AIL.MbufClear(AilCameraFocus, 0)

   # Select image on the display. 
   AIL.MdispSelect(AilDisplay, AilCameraFocus)

   # Simulate the first image grab. 
   SimulateGrabFromCamera(AilSource, AilCameraFocus, FOCUS_START_POSITION, AilDisplay)

   # Initialize user data needed within the hook function. 
   UserData.SourceImage = AilSource
   UserData.FocusImage  = AilCameraFocus
   UserData.Iteration   = 0
   UserData.Display     = AilDisplay

   # Pause to show the original image. 
   print("\nAUTOFOCUS:")
   print("----------\n")
   print("Automatic focusing operation will be done on this image.")
   print("Press any key to continue.\n")
   AIL.MosGetch()
   print("Autofocusing...\n")
  
   # Perform Autofocus.
   # Since lens movement is hardware specific, no digitizer is used here.
   # We simulate the lens movement with by smoothing the image data in 
   # the hook function instead.
   MoveLensHookFunctionPtr = AIL.AIL_DIG_HOOK_FUNCTION_PTR(MoveLensHookFunction)
   FocusPos = AIL.MdigFocus(AIL.M_NULL,
                            AilCameraFocus,
                            AIL.M_DEFAULT,
                            MoveLensHookFunctionPtr,
                            UserData,
                            FOCUS_MIN_POSITION,
                            FOCUS_START_POSITION,
                            FOCUS_MAX_POSITION,
                            FOCUS_MAX_POSITION_VARIATION,
                            FOCUS_MODE + FOCUS_SENSITIVITY) 

   # Print the best focus position and number of iterations. 
   print("The best focus position is {FocusPos}.".format(FocusPos=FocusPos))
   print("The best focus position found in {Iteration} iterations.\n".format(Iteration=UserData.Iteration))
   print("Press any key to end.")
   AIL.MosGetch()

  # Free all allocations. 
   AIL.MbufFree(AilSource)
   AIL.MbufFree(AilCameraFocus)
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0


#********************************************************************************
# Autofocus hook function responsible to move the lens.                        
def MoveLensHookFunction(HookType,
                         Position,
                         UserDataHookPtr):
   UserData = UserDataHookPtr

   # Here, the lens position must be changed according to the Position parameter.
   #   In that case, we simulate the lens position change followed by a grab.

   if HookType == AIL.M_CHANGE or HookType == AIL.M_ON_FOCUS:
      SimulateGrabFromCamera( UserData.SourceImage, 
                              UserData.FocusImage, 
                              Position,
                              UserData.Display 
                              )
      UserData.Iteration += 1

   return 0

#*********************************************************************************
# Utility function to simulate a grab from a camera at different lens position    
# by smoothing the original image. It should be replaced with a true camera grab. 
#                                                                                 
# Note that this lens simulation will not work under AIL-lite because it uses     
# MimConvolve().                                                                  

# Lens simulation characteristics. 
FOCUS_BEST_POSITION            = (FOCUS_MAX_NB_POSITIONS/2)

def SimulateGrabFromCamera(SourceImage,FocusImage, 
                           Iteration,  AnnotationDisplay):
   # Throw an error under AIL-lite since lens simulation cannot be used. 
   if  AIL.M_AIL_LITE:
      raise Exception("Replace the SimulateGrabFromCamera()function with a true image grab.")

   # Compute number of smooths needed to simulate focus. 
   NbSmoothNeeded = int(abs(Iteration - FOCUS_BEST_POSITION))

   # Buffer inquires. 
   BufType        = AIL.MbufInquire(FocusImage, AIL.M_TYPE)
   BufSizeX       = AIL.MbufInquire(FocusImage, AIL.M_SIZE_X)
   BufSizeY       = AIL.MbufInquire(FocusImage, AIL.M_SIZE_Y)

   if(NbSmoothNeeded == 0):
      # Directly copy image source to destination. 
      AIL.MbufCopy(SourceImage, FocusImage)

   elif(NbSmoothNeeded == 1):
      # Directly convolve image from source to destination. 
      AIL.MimConvolve(SourceImage, FocusImage, AIL.M_SMOOTH)
   
   else:
      SourceOwnerSystem = AIL.MbufInquire(SourceImage, AIL.M_OWNER_SYSTEM)

      # Allocate temporary buffer. */ 
      TempBuffer = AIL.MbufAlloc2d(SourceOwnerSystem, BufSizeX, BufSizeY, 
                  BufType, AIL.M_IMAGE + AIL.M_PROC)
   
      # Perform first smooth. 
      AIL.MimConvolve(SourceImage, TempBuffer, AIL.M_SMOOTH)
         
      # Perform smooths. 
      for i in range(1, NbSmoothNeeded-1):
         AIL.MimConvolve(TempBuffer, TempBuffer, AIL.M_SMOOTH)

      # Perform last smooth. 
      AIL.MimConvolve(TempBuffer, FocusImage, AIL.M_SMOOTH)

      # Free temporary buffer. 
      AIL.MbufFree(TempBuffer)
      
   # Draw position cursor. 
   DrawCursor(AnnotationDisplay, Iteration)


#**************************************************************
# Draw position of the focus lens.                             

# Cursor specifications. 
CURSOR_SIZE                  =  14
CURSOR_COLOR                 =  AIL.M_COLOR_GREEN

def DrawCursor(AnnotationDisplay, Position):
   # Prepare for overlay annotations. 
   AIL.MdispControl(AnnotationDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AIL.MdispControl(AnnotationDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)
   AnnotationImage = AIL.MdispInquire(AnnotationDisplay, AIL.M_OVERLAY_ID)
   BufSizeX = AIL.MbufInquire(AnnotationImage, AIL.M_SIZE_X)
   BufSizeY = AIL.MbufInquire(AnnotationImage, AIL.M_SIZE_Y)
   CURSOR_POSITION              =  ((BufSizeY*7)/8)
   CursorColor = CURSOR_COLOR
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, CursorColor)

   # Write annotations. 
   n = (BufSizeX / FOCUS_MAX_NB_POSITIONS)
   AIL.MgraLine(AIL.M_DEFAULT, AnnotationImage, 0.0, CURSOR_POSITION + CURSOR_SIZE, 
                                        BufSizeX - 1, CURSOR_POSITION + CURSOR_SIZE)
   AIL.MgraLine(AIL.M_DEFAULT, AnnotationImage, Position * n, CURSOR_POSITION + CURSOR_SIZE,
                                        Position * n - CURSOR_SIZE, CURSOR_POSITION)
   AIL.MgraLine(AIL.M_DEFAULT, AnnotationImage, Position * n, CURSOR_POSITION + CURSOR_SIZE, 
                                        Position * n + CURSOR_SIZE, CURSOR_POSITION)
   AIL.MgraLine(AIL.M_DEFAULT, AnnotationImage, Position * n - CURSOR_SIZE, CURSOR_POSITION, 
                                        Position * n + CURSOR_SIZE, CURSOR_POSITION)

   

if __name__ == "__main__":
   MdigAutoFocusExample()

```
