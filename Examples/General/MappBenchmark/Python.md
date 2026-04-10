---
title: "MappBenchmark"
description: "This program uses the MappTimer function to demonstrate the benchmarking of AIL functions."
ms.language: "Python"
---

# MappBenchmark

> This program uses the MappTimer function to demonstrate the benchmarking of AIL functions.

**Language:** Python

**Functions used:** `MappAlloc`, `MappControlMp`, `MappFree`, `MappTimer`, `MbufAllocColor`, `MbufCopy`, `MbufDiskInquire`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimRotate`, `MsysAlloc`, `MsysFree`, `MsysInquire`, `MthrInquireMp`, `MthrWait`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Image Processing, What's New, AIL 10.0 SP6, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: MappBenchmark.py
#
# Synopsis:  This program uses the MappTimer function to demonstrate the 
#            benchmarking offunctions. It can be used as a template 
#            to benchmark any Aurora Imaging Library or custom processing function accurately.
#
#            It takes into account different factors that can influence 
#            the timing such as dll load time and OS inaccuracy when 
#            measuring a very short time.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################
 
import ail11 as AIL
import os

# Target image specifications. 
IMAGE_FILE  = os.path.join(AIL.M_IMAGE_PATH, "LargeWafer.mim")
ROTATE_ANGLE = -15

# Timing loop iterations setting. 
MINIMUM_BENCHMARK_TIME = 2.0 # In seconds (1.0 and more recommended). 
ESTIMATION_NB_LOOP     =  10
DEFAULT_NB_LOOP        = 100

# Processing function parameters structure. 
class PROC_PARAM:
   def __init__(self):
      self.AilSourceImage      = None   # Image buffer identifier. 
      self.AilDestinationImage = None   # Image buffer identifier. 

def MappBenchmarkExample():
   ProcessingParam = PROC_PARAM()

   # Allocate defaults. 
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Get the system's owner application.
   AilSystemOwnerApplication = AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION)

   # Get the system's current thread identifier. 
   AilSystemCurrentThreadId = AIL.MsysInquire(AilSystem, AIL.M_CURRENT_THREAD_ID)

   # Restore image into an automatically allocated image buffer. 
   AilDisplayImage = AIL.MbufRestore(IMAGE_FILE, AilSystem)

   # Display the image buffer. 
   AIL.MdispSelect(AilDisplay, AilDisplayImage)

   # Allocate the processing objects. 
   ProcessingInit(AilSystem, ProcessingParam)

   # Pause to show the original image. 
   print("\nPROCESSING FUNCTION BENCHMARKING:")
   print("---------------------------------\n")
   print("This program times a processing function under different conditions.")
   print("Press any key to start.\n")
   AIL.MosGetch()
   print("PROCESSING TIME FOR {SizeX}x{SizeY}:".format(SizeX=AIL.MbufInquire(ProcessingParam.AilDestinationImage, AIL.M_SIZE_X), SizeY=AIL.MbufInquire(ProcessingParam.AilDestinationImage, AIL.M_SIZE_Y)))
   print("------------------------------\n")

   # Benchmark the processing function without multi-processing. 
   # ------------------------------------------------------------
   AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_DISABLE, AIL.M_NULL)   
   TimeOneCore, FPSOneCore = Benchmark(ProcessingParam)

   # Show the resulting image and the timing results. 
   AIL.MbufCopy(ProcessingParam.AilDestinationImage, AilDisplayImage)
   print("Without multi-processing (  1 CPU core ): {TimeOneCore:.3f} ms ( {FPSOneCore:.1f} fps)\n".format(TimeOneCore=TimeOneCore, FPSOneCore=FPSOneCore))
   AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL)   

   # Enable all performance level.
   AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE_PERFORMANCE_LEVEL, AIL.M_ALL, AIL.M_ENABLE, AIL.M_NULL)

   # Inquire the number of performance level. This will return 1 on non-hybrid processor.
   NbPerformanceLevel   = AIL.MappInquireMp(AilSystemOwnerApplication, AIL.M_MP_NB_PERFORMANCE_LEVEL, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL);

   for CurrentMaxPerfLevel in range(NbPerformanceLevel, 0, -1):

      if CurrentMaxPerfLevel > 1:
         print("Benchmark result with core performance level 1 to {CurrentMaxPerfLevel}.\n".format(CurrentMaxPerfLevel=CurrentMaxPerfLevel))
      else:
         print("Benchmark result with core performance level 1.\n");

      # Benchmark the processing function with multi-processing. 
      # ---------------------------------------------------------
      AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_ENABLE, AIL.M_NULL)   
      AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_CORE_SHARING, AIL.M_DEFAULT, AIL.M_ENABLE, AIL.M_NULL) 
      NbCoresUsed = AIL.MthrInquireMp(AilSystemCurrentThreadId, AIL.M_CORE_NUM_EFFECTIVE, AIL.M_DEFAULT, AIL.M_DEFAULT)
      if NbCoresUsed > 1:
         TimeAllCores, FPSAllCores = Benchmark(ProcessingParam)

         # Show the resulting image and the timing results. 
         AIL.MbufCopy(ProcessingParam.AilDestinationImage, AilDisplayImage)
   
         print("Using multi-processing   (  {NbCoresUsed} CPU cores): {TimeAllCores:.3f} ms ( {FPSAllCores:.1f} fps)".format(NbCoresUsed=NbCoresUsed, TimeAllCores=TimeAllCores, FPSAllCores=FPSAllCores))
      AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL)   
      AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_CORE_SHARING, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL) 

      # Benchmark the processing function with multi-processing but no hyper-threading. 
      # --------------------------------------------------------------------------------

      # If Hyper-threading is available on the PC, test if the performance is bette
      #   with it disabled. Sometimes, too many cores sharing the same CPU resources that
      #   are already fully occupied can reduce the overall performance.
   
      AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_ENABLE, AIL.M_NULL)   
      AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_CORE_SHARING, AIL.M_DEFAULT, AIL.M_DISABLE, AIL.M_NULL) 
      NbCoresUsedNoCS = AIL.MthrInquireMp(AilSystemCurrentThreadId, AIL.M_CORE_NUM_EFFECTIVE, AIL.M_DEFAULT, AIL.M_DEFAULT)
      if NbCoresUsedNoCS != NbCoresUsed:
         TimeAllCoresNoCS, FPSAllCoresNoCS = Benchmark(ProcessingParam)

         # Show the resulting image and the timing results. 
         AIL.MbufCopy(ProcessingParam.AilDestinationImage, AilDisplayImage)
         print("Using multi-processing   (  {NbCoresUsedNoCS} CPU cores): {TimeAllCoresNoCS:.3f} ms ( {FPSAllCoresNoCS:.1f} fps), no Hyper-Thread."
               .format(NbCoresUsedNoCS=NbCoresUsedNoCS,TimeAllCoresNoCS=TimeAllCoresNoCS,FPSAllCoresNoCS=FPSAllCoresNoCS)), 
      AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL)   
      AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_CORE_SHARING, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL)  

      # Disable the last performance level.
      AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE_PERFORMANCE_LEVEL, CurrentMaxPerfLevel, AIL.M_DISABLE, AIL.M_NULL);

      # Show a comparative summary of the timing results. 
      if NbCoresUsed > 1:
         print("Benchmark is {Time:.1f} times faster with multi-processing.".format(Time=TimeOneCore/TimeAllCores)) 
      
      if NbCoresUsedNoCS != NbCoresUsed:
         print("Benchmark is {Time:.1f} times faster with multi-processing and no Hyper-Thread.\n".format(Time=TimeOneCore/TimeAllCoresNoCS))

   # Wait for a key press. 
   print("Press any key to end.")
   AIL.MosGetch()

   # Free all allocations. 
   ProcessingFree(ProcessingParam)
   AIL.MdispSelect(AilDisplay, AIL.M_NULL)
   AIL.MbufFree(AilDisplayImage)
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)   
   return 0

#****************************************************************************
# Benchmark function. 
#****************************************************************************
def Benchmark(ProcParamPtr):
   EstimatedNbLoop = DEFAULT_NB_LOOP
   MinTime = 0

   # Wait for the completion of all functions in this thread. 
   AIL.MthrWait(AIL.M_DEFAULT, AIL.M_THREAD_WAIT, AIL.M_NULL)

   # Call the processing once before benchmarking for a more accurate time.
   # This compensates for Dll load time, etc.
   
   StartTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ)
   ProcessingExecute(ProcParamPtr)  
   AIL.MthrWait(AIL.M_DEFAULT, AIL.M_THREAD_WAIT, AIL.M_NULL)
   EndTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ)

   MinTime = EndTime - StartTime

   # Estimate the number of loops required to benchmark the processing for 
   # the specified minimum time.
   
   for n in range(ESTIMATION_NB_LOOP):
      StartTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ)
      ProcessingExecute(ProcParamPtr)
      AIL.MthrWait(AIL.M_DEFAULT, AIL.M_THREAD_WAIT, AIL.M_NULL)
      EndTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ)

      Time = EndTime-StartTime
      MinTime = Time if Time < MinTime else MinTime
   
   if MinTime > 0:
      EstimatedNbLoop = int(MINIMUM_BENCHMARK_TIME / MinTime) + 1

   # Benchmark the processing according to the estimated number of loops. 
   StartTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ)
   for n in range(EstimatedNbLoop):
      ProcessingExecute(ProcParamPtr)

   AIL.MthrWait(AIL.M_DEFAULT, AIL.M_THREAD_WAIT, AIL.M_NULL)
   EndTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ)

   Time = EndTime-StartTime

   FramesPerSecond = EstimatedNbLoop/Time
   Time = Time*1000/EstimatedNbLoop

   return Time, FramesPerSecond

#****************************************************************************
# Processing initialization function.
# ****************************************************************************
def ProcessingInit(AilSystem, ProcParamPtr):
   # Allocate a source buffer. 
   ProcParamPtr.AilSourceImage = AIL.MbufAllocColor(AilSystem, 
                                 AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BAND),
                                 AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_X),
                                 AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_Y),
                                 AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BIT) + AIL.M_UNSIGNED,
                                 AIL.M_IMAGE + AIL.M_PROC)

   # Load the image into the source image.  
   AIL.MbufLoad(IMAGE_FILE, ProcParamPtr.AilSourceImage)

   # Allocate a destination buffer. 
   ProcParamPtr.AilDestinationImage = AIL.MbufAllocColor(AilSystem, 
                                                         AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BAND),
                                                         AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_X),
                                                         AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_Y),
                                                         AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BIT) + AIL.M_UNSIGNED,
                                                         AIL.M_IMAGE + AIL.M_PROC)

#****************************************************************************
# Processing execution function.
# ***************************************************************************
def ProcessingExecute(ProcParamPtr):
   AIL.MimRotate(ProcParamPtr.AilSourceImage, ProcParamPtr.AilDestinationImage, ROTATE_ANGLE,
             AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_BILINEAR + AIL.M_OVERSCAN_CLEAR)

#****************************************************************************
# Processing free function.
# ***************************************************************************
def ProcessingFree(ProcParamPtr):
   # Free all processing allocations.  
   AIL.MbufFree(ProcParamPtr.AilSourceImage)
   AIL.MbufFree(ProcParamPtr.AilDestinationImage)

if __name__ == "__main__":
   MappBenchmarkExample()

```
