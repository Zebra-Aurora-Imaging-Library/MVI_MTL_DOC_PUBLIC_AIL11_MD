---
title: "MappTrace"
description: "This example shows how to generate an AIL trace using the Zebra Profiler utility. To generate a trace, you must open Zebra Profiler (accessible from the Aurora Imaging Control Center) and select 'Generate New Trace' from the File menu."
ms.language: "Python"
---

# MappTrace

> This example shows how to generate an AIL trace using the Zebra Profiler utility. To generate a trace, you must open Zebra Profiler (accessible from the Aurora Imaging Control Center) and select 'Generate New Trace' from the File menu.

**Language:** Python

**Functions used:** `MappAlloc`, `MappControl`, `MappFree`, `MappInquire`, `MappTrace`, `MbufAlloc2d`, `MbufAllocColor`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimArith`, `MimBinarize`, `MimConvert`, `MimHistogramEqualize`, `MsysAlloc`, `MsysFree`, `MthrAlloc`, `MthrControl`, `MthrFree`, `MthrWait`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Image Processing, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#########################################################################################
#
#
# File name: MappTrace.py
#
# Synopsis:  This example shows how to explicitly control and generate a trace for 
#           functions and how to visualize it using the Aurora Imaging Profiler utility. 
#            To generate a trace, you must open Aurora Imaging Profiler (accessible from the 
#            Aurora Imaging Control Center) and select 'Generate New Trace' from the 'File' menu 
#            before to run your Aurora Imaging Library application.
# 
# Note:      By default, all Aurora Imaging Library applications are traceable without code modifications.
#            You can try this using Aurora Imaging Profiler with any Aurora Imaging Library example (Ex: MappStart).
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#########################################################################################

import ail11 as AIL

# Trace related defines.
TRACE_TAG_HOOK_START         = 1
TRACE_TAG_PROCESSING         = 2
TRACE_TAG_PREPROCESSING      = 3

# General defines
COLOR_BROWN                  = AIL.M_RGB888(100, 65, 50)
BUFFERING_SIZE_MAX           = 3
NUMBER_OF_FRAMES_TO_PROCESS  = 10

class HookDataStruct():
   def __init__(self):
      self.AilImageDisp = 0
      self.AilImageTemp1 = 0
      self.AilImageTemp2 = 0
      self.ProcessedImageCount = 0
      self.DoneEvent = 0

def MappTraceExample():
   TracesActivated = AIL.M_NO
   AilGrabBuf = []

   UserHookData = HookDataStruct()

   print("\nPROGRAM TRACING AND PROFILING:")
   print(  "----------------------------------\n")

   print("This example shows how to generate a trace for the execution")
   print("of the functions, and to visualize it using")
   print("the Aurora Imaging Profiler utility.\n")
   print("ACTION REQUIRED:\n")
   if AIL.M_AIL_USE_WINDOWS:
      print("Open 'Aurora Imaging Profiler' from the 'Aurora Imaging Control Center' and")
      print("select 'Generate New Trace' from the 'File' menu.\n")
   else:
      print("Open 'AilConfig' from the 'Aurora Imaging Control Center' and select the")
      print("'PROfiler trace' page in 'Benchmarks and Utilities'.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   ############### Untraceable code section ###############

   # The following code will not be visible in the trace. 

   # Application allocation. 
   # At application allocation time, M_TRACE_LOG_DISABLE can be used to ensures that 
   # an application will not be traceable regardless of Aurora Imaging Profiler or AilConfig requests
   # unless traces are explicitly enabled in the program using an MappControl command.
   
   AilApplication = AIL.MappAlloc("M_DEFAULT", AIL.M_TRACE_LOG_DISABLE)

   # Dummy calls that will be invisible in the trace. 
   AilSystem = AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_HOST, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AilDummyBuffer = AIL.MbufAllocColor(AilSystem, 3, 128, 128, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE)
   AIL.MbufClear(AilDummyBuffer, 0)
   AIL.MbufFree(AilDummyBuffer)
   AIL.MsysFree(AilSystem)

   ########################################################

   # Explicitly allow trace logging after a certain point if Aurora Imaging Profiler has
   # requested a trace. Note that M_TRACE = M_ENABLE can be used to force the log 
   # of a trace even if Profiler is not opened; M_TRACE = M_DISABLE can prevent 
   # logging of code section.
   
   AIL.MappControl(AIL.M_DEFAULT, AIL.M_TRACE, AIL.M_DEFAULT)

   # Inquire if the traces are active (i.e. Profiler is open and waiting for a trace).
   TracesActivated = AIL.MappInquire(AIL.M_DEFAULT, AIL.M_TRACE_ACTIVE)

   if TracesActivated == AIL.M_YES:
      # Create custom trace markers: setting custom names and colors. 

      # Initialize a custom Tag for the grab callback function with a unique color (blue). 
      AIL.MappTrace(AIL.M_DEFAULT,
                    AIL.M_TRACE_SET_TAG_INFORMATION,
                    TRACE_TAG_HOOK_START,
                    AIL.M_COLOR_BLUE, 
                    "Grab Callback Marker")

      # Initialize the custom Tag for the processing section. 
      AIL.MappTrace(AIL.M_DEFAULT,
                    AIL.M_TRACE_SET_TAG_INFORMATION,
                    TRACE_TAG_PROCESSING,
                    AIL.M_DEFAULT,
                    "Processing Section")

      # Initialize the custom Tag for the preprocessing with a unique color (brown). 
      AIL.MappTrace(AIL.M_DEFAULT,
                    AIL.M_TRACE_SET_TAG_INFORMATION,
                    TRACE_TAG_PREPROCESSING,
                    COLOR_BROWN, 
                    "Preprocessing Marker")

   # Allocate objects.
   AilSystem = AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_HOST, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AilDigitizer = AIL.MdigAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT)
   AilDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT)

   SizeX = AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X)
   SizeY = AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y)

   UserHookData.AilImageDisp = AIL.MbufAllocColor(AilSystem, 3, SizeX, SizeY, 
      8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC + AIL.M_DISP)
   AIL.MdispSelect(AilDisplay, UserHookData.AilImageDisp)

   # Allocate the processing temporary buffers. 
   UserHookData.AilImageTemp1 = AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 
      8 + AIL.M_UNSIGNED, AIL.M_PROC + AIL.M_IMAGE)
   UserHookData.AilImageTemp2 = AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 
      8 + AIL.M_UNSIGNED, AIL.M_PROC + AIL.M_IMAGE)
   
   # Allocate grab buffers.
   for NbGrabBuf in range(0, BUFFERING_SIZE_MAX):
      AilGrabBuf.append(AIL.MbufAllocColor(AilSystem, 3, SizeX, SizeY, 8 + AIL.M_UNSIGNED,
         AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC))
   
   # Initialize the user's processing function data structure.
   UserHookData.ProcessedImageCount = 0
   UserHookData.DoneEvent = AIL.MthrAlloc(AilSystem, AIL.M_EVENT, 
      AIL.M_NOT_SIGNALED + AIL.M_AUTO_RESET, AIL.M_NULL, AIL.M_NULL)

   # Start processing. The processing function is called with every frame grabbed.
   HookFunctionPtr = AIL.AIL_DIG_HOOK_FUNCTION_PTR(HookFunction)
   AIL.MdigProcess(AilDigitizer, AilGrabBuf, BUFFERING_SIZE_MAX, AIL.M_START,
      AIL.M_DEFAULT, HookFunctionPtr, UserHookData)

   # Stop the processing when the event is triggered.
   AIL.MthrWait(UserHookData.DoneEvent, AIL.M_EVENT_WAIT + AIL.M_EVENT_TIMEOUT(2000), AIL.M_NULL)

   # Stop the processing.
   AIL.MdigProcess(AilDigitizer, AilGrabBuf, BUFFERING_SIZE_MAX, AIL.M_STOP, AIL.M_DEFAULT,
      HookFunctionPtr, UserHookData)

   # Free the grab and temporary buffers.
   for NbGrabBuf in range(0, BUFFERING_SIZE_MAX):
      AIL.MbufFree(AilGrabBuf[NbGrabBuf])
   AIL.MbufFree(UserHookData.AilImageTemp1)
   AIL.MbufFree(UserHookData.AilImageTemp2)

   # Free defaults.
   AIL.MthrFree(UserHookData.DoneEvent)
   AIL.MappFreeDefault(AilApplication,
      AilSystem,
      AilDisplay,
      AilDigitizer,
      UserHookData.AilImageDisp)
   
   # If Aurora Imaging Profiler activated the traces, the trace file is now ready. 
   if TracesActivated == AIL.M_YES:
      print("A PROCESSING SEQUENCE WAS EXECUTED AND LOGGED A NEW TRACE:\n")
      
      print("The trace can now be loaded in Aurora Imaging Profiler by selecting the")
      print("corresponding file listed in the 'Trace Generation' dialog.\n")

      print("Once loaded, Aurora Imaging Profiler's main window displays the 'Main'")
      print("and the 'MdigProcess' threads of the application.\n")
      
      print("- This main window can now be used to select a section")
      print("  of a thread and to zoom or pan in it.\n")

      print("- The right pane shows detailed statistics as well as a")
      print("  'Quick Access' list displaying allfunction calls.\n")

      print("- The 'User Markers' tab lists the markers and sections logged")
      print("  during the execution. For example, selecting 'Tag:Processing'")
      print("  allows double-clicking to refocus the display on the related")
      print("  calls.\n")

      print("- By clicking a particularfunction call, either in the")
      print("  'main view' or in the 'Quick Access', additional details")
      print("  are displayed, such as its parameters and execution time.\n")
   
   else:
      print("ERROR: No active tracing detected in Profiler!\n")

   print("Press any key to end.")
   AIL.MosGetch()

   return 0

def HookFunction(HookType, HookId, HookDataPtr):
   PreprocReturnValue = -1
   CurrentImage = AIL.M_NULL
   
   UserDataPtr = HookDataPtr

   # Add a marker to indicate the reception of a new grabbed image.
   AIL.MappTrace(AIL.M_DEFAULT,
                 AIL.M_TRACE_MARKER,
                 TRACE_TAG_HOOK_START,
                 AIL.M_NULL,
                 "New Image grabbed")

   # Retrieve the AIL_ID of the grabbed buffer
   CurrentImage = AIL.MdigGetHookInfo(HookId, AIL.M_MODIFIED_BUFFER + AIL.M_BUFFER_ID)

   # Start a Section to highlight the processing calls on the image. 
   AIL.MappTrace(AIL.M_DEFAULT,
                 AIL.M_TRACE_SECTION_START,
                 TRACE_TAG_PROCESSING,
                 UserDataPtr.ProcessedImageCount,
                 "Processing Image")

   # Add a Marker to indicate the start of the preprocessing section. 
   AIL.MappTrace(AIL.M_DEFAULT,
                 AIL.M_TRACE_MARKER,
                 TRACE_TAG_PREPROCESSING,
                 UserDataPtr.ProcessedImageCount,
                 "Start Preprocessing")

   # Do the preprocessing. 
   AIL.MimConvert(CurrentImage, UserDataPtr.AilImageTemp1, AIL.M_RGB_TO_L)
   AIL.MimHistogramEqualize(UserDataPtr.AilImageTemp1, UserDataPtr.AilImageTemp1, 
      AIL.M_UNIFORM, AIL.M_NULL, 55, 200)

   # Add a Marker to indicate the end of the preprocessing section. 
   AIL.MappTrace(AIL.M_DEFAULT,
                 AIL.M_TRACE_MARKER,
                 TRACE_TAG_PREPROCESSING,
                 UserDataPtr.ProcessedImageCount,
                 "End Preprocessing")

   # Do the main processing. 
   AIL.MimBinarize(UserDataPtr.AilImageTemp1, UserDataPtr.AilImageTemp2,
      AIL.M_IN_RANGE, 120, 140)
   AIL.MimBinarize(UserDataPtr.AilImageTemp1, UserDataPtr.AilImageTemp1,
      AIL.M_IN_RANGE, 220, 255)
   AIL.MimArith(UserDataPtr.AilImageTemp1, UserDataPtr.AilImageTemp2, 
      UserDataPtr.AilImageDisp, AIL.M_OR)
   
   # End the Section that highlights the processing. 
   AIL.MappTrace(AIL.M_DEFAULT,
                 AIL.M_TRACE_SECTION_END,
                 TRACE_TAG_PROCESSING,
                 UserDataPtr.ProcessedImageCount,
                 "Processing Image End")

   # Signal that processing has been completed. 
   UserDataPtr.ProcessedImageCount += 1
   if UserDataPtr.ProcessedImageCount >= NUMBER_OF_FRAMES_TO_PROCESS:
      AIL.MthrControl(UserDataPtr.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED)
   
   return 0

if __name__ == "__main__":
   MappTraceExample()
```
