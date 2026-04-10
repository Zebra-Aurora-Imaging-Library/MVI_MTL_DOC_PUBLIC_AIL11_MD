---
title: "Mthread"
description: "This program shows how to use threads in an AIL application and synchronize them with events. It creates 4 processing threads that are used to work in 4 different regions of a display buffer."
ms.language: "Python"
---

# Mthread

> This program shows how to use threads in an AIL application and synchronize them with events. It creates 4 processing threads that are used to work in 4 different regions of a display buffer.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MappInquire`, `MappTimer`, `MbufAlloc2d`, `MbufClear`, `MbufCopy`, `MbufCopyColor2d`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MdispInquire`, `MgraArcFill`, `MgraRectFill`, `MgraText`, `MimArith`, `MimConvolve`, `MimRotate`, `MsysAlloc`, `MsysFree`, `MsysInquire`, `MthrAlloc`, `MthrControl`, `MthrFree`, `MthrWait`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: Mthread.py 
#
# Synopsis:  This program shows how to use threads in an application and
#            synchronize them with event. It creates 4 processing threads that
#            are used to work in 4 different regions of a display buffer.
#
#     Thread usage:
#      - The main thread starts a processing thread in each of the 4 different
#        quarters of a display buffer. The main thread then waits for a key to
#        be pressed to stop them.
#      - The top-left and bottom-left threads work in a loop, as follows: the
#        top-left thread adds a constant to its buffer, then sends an event to
#        the bottom-left thread. The bottom-left thread waits for the event
#        from the top-left thread, rotates the top-left buffer image, then sends an
#        event to the top-left thread to start a new loop.
#      - The top-right and bottom-right threads work the same way as the
#        top-left and bottom-left threads, except that the bottom-right thread
#        performs an edge detection operation, rather than a rotation.
#
#      Note : - Under AIL-Lite, the threads will do graphic annotations instead.
#             - Comment out the MdispSelect() if you wish to avoid benchmarking
#               the display update overhead on CPU usage and processing rate.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################


import ail11 as AIL
import os

# Local defines.
IMAGE_FILE         = os.path.join(AIL.M_IMAGE_PATH, "Bird.mim")
IMAGE_WIDTH        = 256
IMAGE_HEIGHT       = 240
STRING_LENGTH_MAX  = 40
STRING_POS_X       = 10
STRING_POS_Y       = 220
DRAW_RADIUS_NUMBER = 5
DRAW_RADIUS_STEP   = 10
DRAW_CENTER_POSX   = 196
DRAW_CENTER_POSY   = 180

class ThreadParam:
   def __init__(self):
      self.Id = None
      self.System = None
      self.OrgImage = None
      self.SrcImage = None
      self.DstImage = None
      self.DispImage = None
      self.DispOffsetX = None
      self.DispOffsetY = None
      self.ReadyEvent = None
      self.DoneEvent = None
      self.NumberOfIteration = None
      self.Radius = None
      self.Exit = None
      self.LicenseModules = None
      self.WorkerThreadParam = None


# Main function: #
# -------------- #
def MthreadExample():

   TParTopLeft = ThreadParam()
   TParTopRight = ThreadParam()
   TParBotLeft = ThreadParam()
   TParBotRight = ThreadParam()

   # Allocate defaults. 
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Allocate and display the main image buffer. 
   AilImage = AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH * 2, IMAGE_HEIGHT * 2, 8 + AIL.M_UNSIGNED,
                              AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP)

   AIL.MbufClear(AilImage, 0)
   AIL.MdispSelect(AilDisplay, AilImage)
   TParTopLeft.DispImage = AIL.MdispInquire(AilDisplay, AIL.M_SELECTED)

   # Allocate an image buffer to keep the original.
   AilOrgImage = AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED,
                                 AIL.M_IMAGE + AIL.M_PROC)

   # Allocate a processing buffer for each thread.
   TParTopLeft.SrcImage = AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC)
   TParBotLeft.DstImage = AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC)
   TParTopRight.SrcImage = AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC)
   TParBotRight.DstImage = AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC)

   # Allocate synchronization events. 
   TParTopLeft.DoneEvent = AIL.MthrAlloc(AilSystem, AIL.M_EVENT, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL)
   TParBotLeft.DoneEvent = AIL.MthrAlloc(AilSystem, AIL.M_EVENT, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL)
   TParTopRight.DoneEvent = AIL.MthrAlloc(AilSystem, AIL.M_EVENT, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL)
   TParBotRight.DoneEvent = AIL.MthrAlloc(AilSystem, AIL.M_EVENT, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL)

   # Inquire licenses. 
   AilRemoteApplication = AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION)
   LicenseModules = AIL.MappInquire(AilRemoteApplication, AIL.M_LICENSE_MODULES)

   # Initialize remaining thread parameters. 
   TParTopLeft.System               = AilSystem
   TParTopLeft.OrgImage             = AilOrgImage
   TParTopLeft.DstImage             = TParTopLeft.SrcImage
   TParTopLeft.DispOffsetX          = 0
   TParTopLeft.DispOffsetY          = 0
   TParTopLeft.ReadyEvent           = TParBotLeft.DoneEvent
   TParTopLeft.NumberOfIteration    = 0
   TParTopLeft.Radius               = 0
   TParTopLeft.Exit                 = 0
   TParTopLeft.LicenseModules       = LicenseModules
   TParTopLeft.WorkerThreadParam     = TParBotLeft

   TParBotLeft.System               = AilSystem
   TParBotLeft.OrgImage             = 0
   TParBotLeft.SrcImage             = TParTopLeft.DstImage
   TParBotLeft.DispImage            = TParTopLeft.DispImage
   TParBotLeft.DispOffsetX          = 0
   TParBotLeft.DispOffsetY          = IMAGE_HEIGHT
   TParBotLeft.ReadyEvent           = TParTopLeft.DoneEvent
   TParBotLeft.NumberOfIteration    = 0
   TParBotLeft.Radius               = 0
   TParBotLeft.Exit                 = 0
   TParBotLeft.LicenseModules       = LicenseModules
   TParBotLeft.WorkerThreadParam     = 0

   TParTopRight.System              = AilSystem
   TParTopRight.OrgImage            = AilOrgImage
   TParTopRight.DstImage            = TParTopRight.SrcImage
   TParTopRight.DispImage           = TParTopLeft.DispImage
   TParTopRight.DispOffsetX         = IMAGE_WIDTH
   TParTopRight.DispOffsetY         = 0
   TParTopRight.ReadyEvent          = TParBotRight.DoneEvent
   TParTopRight.NumberOfIteration   = 0
   TParTopRight.Radius              = 0
   TParTopRight.Exit                = 0
   TParTopRight.LicenseModules      = LicenseModules
   TParTopRight.WorkerThreadParam    = TParBotRight

   TParBotRight.System              = AilSystem
   TParBotRight.OrgImage            = 0
   TParBotRight.SrcImage            = TParTopRight.DstImage
   TParBotRight.DispImage           = TParTopLeft.DispImage
   TParBotRight.DispOffsetX         = IMAGE_WIDTH
   TParBotRight.DispOffsetY         = IMAGE_HEIGHT
   TParBotRight.ReadyEvent          = TParTopRight.DoneEvent
   TParBotRight.NumberOfIteration   = 0
   TParBotRight.Radius              = 0
   TParBotRight.Exit                = 0
   TParBotRight.LicenseModules      = LicenseModules
   TParBotRight.WorkerThreadParam    = 0

   # Initialize the original image to process. 
   AIL.MbufLoad(IMAGE_FILE, AilOrgImage)

   # Start the 4 threads. 

   TopThreadPtr = AIL.AIL_THREAD_FUNCTION_PTR(TopThread)
   BotLeftThreadPtr = AIL.AIL_THREAD_FUNCTION_PTR(BotLeftThread)
   BotRightThreadPtr = AIL.AIL_THREAD_FUNCTION_PTR(BotRightThread)

   TParTopLeft.Id = AIL.MthrAlloc(AilSystem, AIL.M_THREAD, AIL.M_DEFAULT, TopThreadPtr, TParTopLeft)
   TParBotLeft.Id = AIL.MthrAlloc(AilSystem, AIL.M_THREAD, AIL.M_DEFAULT, BotLeftThreadPtr, TParBotLeft)
   TParTopRight.Id = AIL.MthrAlloc(AilSystem, AIL.M_THREAD, AIL.M_DEFAULT, TopThreadPtr, TParTopRight)
   TParBotRight.Id = AIL.MthrAlloc(AilSystem, AIL.M_THREAD, AIL.M_DEFAULT, BotRightThreadPtr, TParBotRight)

   # Start the timer.
   AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL)

   # Set events to start operation of top-left and top-right threads. 
   AIL.MthrControl(TParTopLeft.ReadyEvent,  AIL.M_EVENT_SET, AIL.M_SIGNALED)
   AIL.MthrControl(TParTopRight.ReadyEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED)

   # Report that the threads are started and wait for a key press to stop them.
   print("\nMULTI-THREADING:")
   print("----------------\n")
   print("4 threads running...")
   print("Press any key to stop.\n")
   AIL.MosGetch()

   # Signal the threads to exit.
   TParTopLeft.Exit  = 1
   TParTopRight.Exit = 1

   # Wait for all threads to terminate. 
   AIL.MthrWait(TParTopLeft.Id , AIL.M_THREAD_END_WAIT, AIL.M_NULL)
   AIL.MthrWait(TParBotLeft.Id , AIL.M_THREAD_END_WAIT, AIL.M_NULL)
   AIL.MthrWait(TParTopRight.Id, AIL.M_THREAD_END_WAIT, AIL.M_NULL)
   AIL.MthrWait(TParBotRight.Id, AIL.M_THREAD_END_WAIT, AIL.M_NULL)

   # Stop the timer and calculate the number of frames per second processed.
   Time = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)
   FramesPerSecond = (TParTopLeft.NumberOfIteration + TParBotLeft.NumberOfIteration +
                     TParTopRight.NumberOfIteration + TParBotRight.NumberOfIteration)/Time

   # Print statistics. 
   print("Top-left iterations done:     {NumIteration}.".format(NumIteration=TParTopLeft.NumberOfIteration))
   print("Bottom-left iterations done:  {NumIteration}.".format(NumIteration=TParBotLeft.NumberOfIteration))
   print("Top-right iterations done:    {NumIteration}.".format(NumIteration=TParTopRight.NumberOfIteration))
   print("Bottom-right iterations done: {NumIteration}.\n".format(NumIteration=TParBotRight.NumberOfIteration))
   print("Processing speed for the 4 threads: {FramesPerSecond} Images/Sec.\n".format(FramesPerSecond=round(FramesPerSecond)))
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free threads.
   AIL.MthrFree(TParTopLeft.Id)
   AIL.MthrFree(TParBotLeft.Id)
   AIL.MthrFree(TParTopRight.Id)
   AIL.MthrFree(TParBotRight.Id)

   # Free events. 
   AIL.MthrFree(TParTopLeft.DoneEvent)
   AIL.MthrFree(TParBotLeft.DoneEvent)
   AIL.MthrFree(TParTopRight.DoneEvent)
   AIL.MthrFree(TParBotRight.DoneEvent)

   # Free buffers. 
   AIL.MbufFree(TParTopLeft.SrcImage)
   AIL.MbufFree(TParTopRight.SrcImage)
   AIL.MbufFree(TParBotLeft.DstImage)
   AIL.MbufFree(TParBotRight.DstImage)
   AIL.MbufFree(AilOrgImage)
   AIL.MbufFree(AilImage)

   # Free defaults. 
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0

# Top-left and top-right threads' function (Add an offset): #
# --------------------------------------------------------- #
def TopThread(ThreadParameters):
   TPar = ThreadParameters

   while not TPar.Exit:
      # Wait for bottom ready event before proceeding. 
      AIL.MthrWait(TPar.ReadyEvent, AIL.M_EVENT_WAIT, AIL.M_NULL)

      # For better visual effect, reset SrcImage to the original image regularly. 
      if (TPar.NumberOfIteration % 192) == 0:
         AIL.MbufCopy(TPar.OrgImage, TPar.SrcImage)

      if TPar.LicenseModules & AIL.M_LICENSE_IM:
         # Add a constant to the image. 
         AIL.MimArith(TPar.SrcImage, 1, TPar.DstImage, AIL.M_ADD_CONST + AIL.M_SATURATION)
      else:
         # Under AIL-Lite draw a variable size rectangle in the image. 
         TPar.Radius = TPar.WorkerThreadParam.Radius = (TPar.NumberOfIteration % DRAW_RADIUS_NUMBER) * DRAW_RADIUS_STEP
         AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xff)
         AIL.MgraRectFill(AIL.M_DEFAULT, TPar.DstImage, 
                          DRAW_CENTER_POSX - TPar.Radius, DRAW_CENTER_POSY - TPar.Radius, 
                          DRAW_CENTER_POSX + TPar.Radius, DRAW_CENTER_POSY + TPar.Radius)

      # Increment iteraton count and draw text.
      TPar.NumberOfIteration += 1
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xFF)
      AIL.MgraText(AIL.M_DEFAULT, TPar.DstImage, STRING_POS_X, STRING_POS_Y, str(TPar.NumberOfIteration))

      # Update the display. 
      if TPar.DispImage:
         AIL.MbufCopyColor2d(TPar.DstImage,
                             TPar.DispImage,
                             AIL.M_ALL_BANDS, 0, 0,
                             AIL.M_ALL_BANDS,
                             TPar.DispOffsetX,
                             TPar.DispOffsetY,
                             IMAGE_WIDTH,
                             IMAGE_HEIGHT)
      
      # Signal to the bottom thread that the first part of the processing is completed. s
      AIL.MthrControl(TPar.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED)

   # Require the bottom thread to exit. 
   TPar.WorkerThreadParam.Exit = 1

   # Signal the bottom thread to wake up. 
   AIL.MthrControl(TPar.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED)

   # Before exiting the thread, make sure that all the commands are executed. 
   AIL.MthrWait(TPar.System, AIL.M_THREAD_WAIT, AIL.M_NULL)
   return 1

# Bottom-left thread function (Rotate): #
# ------------------------------------- #
def BotLeftThread(ThreadParameters):
   TPar = ThreadParameters
   Angle = 0.0
   AngleIncrement = 0.5

   while not TPar.Exit:
      # Wait for the event in top-left function to be ready before proceeding. 
      AIL.MthrWait(TPar.ReadyEvent, AIL.M_EVENT_WAIT, AIL.M_NULL)

      if TPar.LicenseModules & AIL.M_LICENSE_IM:
         # Rotate the image. 
         AIL.MimRotate(TPar.SrcImage, TPar.DstImage, Angle,
                       AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT,
                       AIL.M_NEAREST_NEIGHBOR + AIL.M_OVERSCAN_CLEAR)

         Angle += AngleIncrement

         if Angle >= 360:
            Angle -= 360
      else:
         # Under AIL-Lite copy the top-left image and draw 
         # a variable size filled circle in the image. 
         AIL.MbufCopy(TPar.SrcImage, TPar.DstImage)
         AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0x80)
         AIL.MgraArcFill(AIL.M_DEFAULT, TPar.DstImage, DRAW_CENTER_POSX, DRAW_CENTER_POSY,
                                                       TPar.Radius, TPar.Radius, 0, 360)
      # Increment iteration count and draw text. 
      TPar.NumberOfIteration += 1
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xFF)
      AIL.MgraText(AIL.M_DEFAULT, TPar.DstImage, STRING_POS_X, STRING_POS_Y, str(TPar.NumberOfIteration))

      # Update the display. 
      if TPar.DispImage:
         AIL.MbufCopyColor2d(TPar.DstImage,
                             TPar.DispImage,
                             AIL.M_ALL_BANDS, 0, 0,
                             AIL.M_ALL_BANDS,
                             TPar.DispOffsetX,
                             TPar.DispOffsetY,
                             IMAGE_WIDTH,
                             IMAGE_HEIGHT)
      
      # Signal to the top-left thread that the last part of the processing is completed. 
      AIL.MthrControl(TPar.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED)
      
   # Before exiting the thread, make sure that all the commands are executed. 
   AIL.MthrWait(TPar.System, AIL.M_THREAD_WAIT, AIL.M_NULL)

   return 1

# Bottom-right thread function (Edge Detect): #
# ------------------------------------------- #
def BotRightThread(ThreadParameters):
   TPar = ThreadParameters

   while not TPar.Exit:
      # Wait for the event in top-right function to be ready before proceeding. 
      AIL.MthrWait(TPar.ReadyEvent, AIL.M_EVENT_WAIT, AIL.M_NULL)

      if TPar.LicenseModules & AIL.M_LICENSE_IM:
         # Perform an edge detection operation on the image. 
         AIL.MimConvolve(TPar.SrcImage, TPar.DstImage, AIL.M_EDGE_DETECT_SOBEL_FAST)
      else:
         # Under AIL-Lite copy the top-right image and draw 
         # a variable size filled circle in the image. 
         AIL.MbufCopy(TPar.SrcImage, TPar.DstImage)
         AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0x40)
         AIL.MgraArcFill(AIL.M_DEFAULT, TPar.DstImage, DRAW_CENTER_POSX, DRAW_CENTER_POSY,
                         TPar.Radius / 2, TPar.Radius / 2, 0, 360)

      # Increment iteration count and draw text. 
      TPar.NumberOfIteration += 1
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xFF)
      AIL.MgraText(AIL.M_DEFAULT, TPar.DstImage, STRING_POS_X, STRING_POS_Y, str(TPar.NumberOfIteration))

      # Update the display. 
      if TPar.DispImage:
         AIL.MbufCopyColor2d(TPar.DstImage,
                             TPar.DispImage,
                             AIL.M_ALL_BANDS, 0, 0,
                             AIL.M_ALL_BANDS,
                             TPar.DispOffsetX,
                             TPar.DispOffsetY,
                             IMAGE_WIDTH,
                             IMAGE_HEIGHT)
         

      # Signal to the top-right thread that the last part of the processing is completed. 
      AIL.MthrControl(TPar.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED)
      
      
   # Before exiting the thread, make sure that all the commands are executed. 
   AIL.MthrWait(TPar.System, AIL.M_THREAD_WAIT, AIL.M_NULL)
   return 1
   
if __name__ == "__main__":
  MthreadExample()

```
