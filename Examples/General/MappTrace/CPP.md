---
title: "MappTrace"
description: "This example shows how to generate an AIL trace using the Zebra Profiler utility. To generate a trace, you must open Zebra Profiler (accessible from the Aurora Imaging Control Center) and select 'Generate New Trace' from the File menu."
ms.language: "C++"
---

# MappTrace

> This example shows how to generate an AIL trace using the Zebra Profiler utility. To generate a trace, you must open Zebra Profiler (accessible from the Aurora Imaging Control Center) and select 'Generate New Trace' from the File menu.

**Language:** C++

**Functions used:** `MappAlloc`, `MappControl`, `MappFree`, `MappInquire`, `MappTrace`, `MbufAlloc2d`, `MbufAllocColor`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimArith`, `MimBinarize`, `MimConvert`, `MimHistogramEqualize`, `MsysAlloc`, `MsysFree`, `MthrAlloc`, `MthrControl`, `MthrFree`, `MthrWait`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MappTrace.cpp
//
// Description: This example shows how to explicitly control and generate a trace for 
//              functions and how to visualize it using the Aurora Imaging Profiler utility. 
//              To generate a trace, you must open Aurora Imaging Profiler (accessible from the 
//              Aurora Imaging Control Center) and select 'Generate New Trace' from the 'File' menu 
//              before to run your application.
//              
// Note:        By default, all applications are traceable without code modifications.
//              You can try this using Aurora Imaging Profiler with any example (Ex: MappStart).
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Trace related defines. 
#define TRACE_TAG_HOOK_START                       1
#define TRACE_TAG_PROCESSING                       2
#define TRACE_TAG_PREPROCESSING                    3

// General defines. 
#define COLOR_BROWN                                M_RGB888(100,65,50)
#define BUFFERING_SIZE_MAX                         3
#define NUMBER_OF_FRAMES_TO_PROCESS                10

// Function prototype. 
AIL_INT MFTYPE HookFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr);
typedef struct
{
   AIL_ID  AilImageDisp;
   AIL_ID  AilImageTemp1;
   AIL_ID  AilImageTemp2;
   AIL_INT ProcessedImageCount;
   AIL_ID  DoneEvent;
} HookDataStruct;

// Main function. 
int MosMain(void)
{
   AIL_ID   AilApplication = M_NULL;
   AIL_ID   AilSystem = M_NULL;
   AIL_ID   AilDisplay = M_NULL;
   AIL_ID   AilDigitizer = M_NULL;
   AIL_ID   AilGrabBuf[BUFFERING_SIZE_MAX] = { 0 };
   AIL_ID   AilDummyBuffer = M_NULL;

   AIL_INT TracesActivated = M_NO;
   AIL_INT NbGrabBuf = 0;
   AIL_INT SizeX = 0, SizeY = 0;

   HookDataStruct UserHookData;

   MosPrintf(AIL_TEXT("\nPROGRAM TRACING AND PROFILING:\n"));
   MosPrintf(AIL_TEXT(  "----------------------------------\n\n"));

   MosPrintf(AIL_TEXT("This example shows how to generate a trace for the execution\n"));
   MosPrintf(AIL_TEXT("of the functions, and to visualize it using\n"));
   MosPrintf(AIL_TEXT("the Aurora Imaging Profiler utility.\n\n"));
   MosPrintf(AIL_TEXT("ACTION REQUIRED:\n\n"));
#if M_AIL_USE_WINDOWS
   MosPrintf(AIL_TEXT("Open 'Aurora Imaging Profiler' from the 'Aurora Imaging Control Center' and\n"));
   MosPrintf(AIL_TEXT("select 'Generate New Trace' from the 'File' menu.\n\n"));
#else
   MosPrintf(AIL_TEXT("Open 'Aurora Imaging Configurator' from the 'Aurora Imaging Control Center' and select the\n"));
   MosPrintf(AIL_TEXT("'Profiler trace' page in 'Benchmarks and Utilities'.\n"));
#endif
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   //************** Untraceable code section **************

   // The following code will not be visible in the trace. 

   // Application allocation. 
   // At Application allocation time, M_TRACE_LOG_DISABLE can be used to ensures that 
   // an application will not be traceable regardless of Aurora Imaging Profiler or Aurora Imaging Configurator requests
   // unless traces are explicitly enabled in the program using an MappControl command.
   MappAlloc(AIL_TEXT("M_DEFAULT"), M_TRACE_LOG_DISABLE, &AilApplication);

   // Dummy calls that will be invisible in the trace. 
   MsysAlloc(M_DEFAULT, M_SYSTEM_HOST, M_DEFAULT, M_DEFAULT, &AilSystem);
   MbufAllocColor(AilSystem, 3, 128, 128, 8 + M_UNSIGNED, M_IMAGE, &AilDummyBuffer);
   MbufClear(AilDummyBuffer, 0L);
   MbufFree(AilDummyBuffer);
   MsysFree(AilSystem);

   //******************************************************

   // Explicitly allow trace logging after a certain point if Aurora Imaging Profiler has
   // requested a trace. Note that M_TRACE = M_ENABLE can be used to force the log 
   // of a trace even if Profiler is not opened; M_TRACE = M_DISABLE can prevent 
   // logging of code section.
   MappControl(M_DEFAULT, M_TRACE, M_DEFAULT);

   // Inquire if the traces are active (i.e. Profiler is open and waiting for a trace). 
   MappInquire(M_DEFAULT, M_TRACE_ACTIVE, &TracesActivated);

   if (TracesActivated == M_YES)
   {
      // Create custom trace markers: setting custom names and colors. 

      // Initialize a custom Tag for the grab callback function with a unique color (blue). 
      MappTrace(M_DEFAULT,
         M_TRACE_SET_TAG_INFORMATION,
         TRACE_TAG_HOOK_START,
         M_COLOR_BLUE, AIL_TEXT("Grab Callback Marker"));

      // Initialize the custom Tag for the processing section. 
      MappTrace(M_DEFAULT,
         M_TRACE_SET_TAG_INFORMATION,
         TRACE_TAG_PROCESSING,
         M_DEFAULT, AIL_TEXT("Processing Section"));

      // Initialize the custom Tag for the preprocessing with a unique color (brown). 
      MappTrace(M_DEFAULT,
         M_TRACE_SET_TAG_INFORMATION,
         TRACE_TAG_PREPROCESSING,
         COLOR_BROWN, AIL_TEXT("Preprocessing Marker"));
   }

   // Allocate objects. 
   MsysAlloc(M_DEFAULT, M_SYSTEM_HOST, M_DEFAULT, M_DEFAULT, &AilSystem);
   MdigAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, &AilDigitizer);
   MdispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, &AilDisplay);

   SizeX = MdigInquire(AilDigitizer, M_SIZE_X, M_NULL);
   SizeY = MdigInquire(AilDigitizer, M_SIZE_Y, M_NULL);

   MbufAllocColor(AilSystem, 3, SizeX, SizeY, 8+M_UNSIGNED,
      M_IMAGE + M_GRAB+M_PROC+M_DISP, &UserHookData.AilImageDisp);
   MdispSelect(AilDisplay, UserHookData.AilImageDisp);

   // Allocate the processing temporary buffers. 
   MbufAlloc2d(AilSystem, SizeX, SizeY, 8+M_UNSIGNED, M_PROC+M_IMAGE, 
      &UserHookData.AilImageTemp1);
   MbufAlloc2d(AilSystem, SizeX, SizeY, 8+M_UNSIGNED, M_PROC+M_IMAGE,
      &UserHookData.AilImageTemp2);

   // Allocate the grab buffers. 
   for (NbGrabBuf = 0; NbGrabBuf < BUFFERING_SIZE_MAX; NbGrabBuf++)
   {
      MbufAllocColor(AilSystem, 3, SizeX, SizeY, 8 + M_UNSIGNED,
         M_IMAGE + M_GRAB + M_PROC, &AilGrabBuf[NbGrabBuf]);
   }

   // Initialize the user's processing function data structure. 
   UserHookData.ProcessedImageCount = 0;
   MthrAlloc(AilSystem, M_EVENT, M_NOT_SIGNALED + M_AUTO_RESET, M_NULL,
      M_NULL, &UserHookData.DoneEvent);

   // Start the processing. The processing function is called with every frame grabbed.
   MdigProcess(AilDigitizer, AilGrabBuf, BUFFERING_SIZE_MAX, M_START,
      M_DEFAULT, HookFunction, &UserHookData);

   // Stop the processing when the event is triggered. 
   MthrWait(UserHookData.DoneEvent, M_EVENT_WAIT + M_EVENT_TIMEOUT(2000), M_NULL);

   // Stop the processing. 
   MdigProcess(AilDigitizer, AilGrabBuf, BUFFERING_SIZE_MAX, M_STOP, M_DEFAULT,
      HookFunction, &UserHookData);

   // Free the grab and temporary buffers. 
   for (NbGrabBuf = 0; NbGrabBuf < BUFFERING_SIZE_MAX; NbGrabBuf++)
      MbufFree(AilGrabBuf[NbGrabBuf]);
   MbufFree(UserHookData.AilImageTemp1);
   MbufFree(UserHookData.AilImageTemp2);

   // Free defaults. 
   MthrFree(UserHookData.DoneEvent);
   MappFreeDefault(AilApplication,
      AilSystem,
      AilDisplay,
      AilDigitizer,
      UserHookData.AilImageDisp);

   // If Aurora Imaging Profiler activated the traces, the trace file is now ready. 
   if (TracesActivated == M_YES)
   {
      MosPrintf(AIL_TEXT("A PROCESSING SEQUENCE WAS EXECUTED AND LOGGED A NEW TRACE:\n\n"));
#if M_AIL_USE_WINDOWS      
      MosPrintf(AIL_TEXT("The trace can now be loaded in Aurora Imaging Profiler by selecting the\n"));
      MosPrintf(AIL_TEXT("corresponding file listed in the 'Trace Generation' dialog.\n\n"));

      MosPrintf(AIL_TEXT("Once loaded, Aurora Imaging Profiler's main window displays the 'Main'\n"));
      MosPrintf(AIL_TEXT("and the 'MdigProcess' threads of the application.\n\n"));
      
      MosPrintf(AIL_TEXT("- This main window can now be used to select a section\n"));
      MosPrintf(AIL_TEXT("  of a thread and to zoom or pan in it.\n\n"));

      MosPrintf(AIL_TEXT("- The right pane shows detailed statistics as well as a\n"));
      MosPrintf(AIL_TEXT("  'Quick Access' list displaying all function calls.\n\n"));

      MosPrintf(AIL_TEXT("- The 'User Markers' tab lists the markers and sections logged\n"));
      MosPrintf(AIL_TEXT("  during the execution. For example, selecting 'Tag:Processing'\n"));
      MosPrintf(AIL_TEXT("  allows double-clicking to refocus the display on the related\n"));
      MosPrintf(AIL_TEXT("  calls.\n\n"));

      MosPrintf(AIL_TEXT("- By clicking a particular function call, either in the\n"));
      MosPrintf(AIL_TEXT("  'main view' or in the 'Quick Access', additional details\n"));
      MosPrintf(AIL_TEXT("  are displayed, such as its parameters and execution time.\n\n"));
#else
      MosPrintf(AIL_TEXT("The trace is now available in 'Profiler trace' page of Aurora Imaging Configurator\n"));
      MosPrintf(AIL_TEXT("Copy the trace file to a Windows machine and open it with the Profiler utility.\n\n"));
#endif
   }
   else
   {
#if M_AIL_USE_WINDOWS
      MosPrintf(AIL_TEXT("ERROR: No active tracing detected in Profiler!\n\n"));
#else
      MosPrintf(AIL_TEXT("ERROR: No active tracing detected in 'Profiler trace' page of Aurora Imaging Configurator!\n\n"));
#endif
   }
   MosPrintf(AIL_TEXT("Press any key to end."));
   MosGetch();

   return 0;
}

AIL_INT MFTYPE HookFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr)
{
   AIL_INT64 PreprocReturnValue = -1;
   AIL_ID CurrentImage = M_NULL;

   HookDataStruct *UserDataPtr = (HookDataStruct *)HookDataPtr;

   // Add a marker to indicate the reception of a new grabbed image. 
   MappTrace(M_DEFAULT,
      M_TRACE_MARKER,
      TRACE_TAG_HOOK_START,
      M_NULL,
      AIL_TEXT("New Image grabbed"));

   // Retrieve the AIL_ID of the grabbed buffer. 
   MdigGetHookInfo(HookId, M_MODIFIED_BUFFER + M_BUFFER_ID, &CurrentImage);

   // Start a Section to highlight the processing calls on the image. 
   MappTrace(M_DEFAULT,
      M_TRACE_SECTION_START,
      TRACE_TAG_PROCESSING,
      UserDataPtr->ProcessedImageCount,
      AIL_TEXT("Processing Image"));

   // Add a Marker to indicate the start of the preprocessing section. 
   MappTrace(M_DEFAULT,
      M_TRACE_MARKER,
      TRACE_TAG_PREPROCESSING,
      UserDataPtr->ProcessedImageCount,
      AIL_TEXT("Start Preprocessing"));

   // Do the preprocessing. 
   MimConvert(CurrentImage, UserDataPtr->AilImageTemp1, M_RGB_TO_L);
   MimHistogramEqualize(UserDataPtr->AilImageTemp1, UserDataPtr->AilImageTemp1, 
      M_UNIFORM, M_NULL, 55, 200);

   // Add a Marker to indicate the end of the preprocessing section. 
   MappTrace(M_DEFAULT,
      M_TRACE_MARKER,
      TRACE_TAG_PREPROCESSING,
      UserDataPtr->ProcessedImageCount,
      AIL_TEXT("End Preprocessing"));

   // Do the main processing. 
   MimBinarize(UserDataPtr->AilImageTemp1, UserDataPtr->AilImageTemp2,
      M_IN_RANGE, 120, 140);
   MimBinarize(UserDataPtr->AilImageTemp1, UserDataPtr->AilImageTemp1,
      M_IN_RANGE, 220, 255);
   MimArith(UserDataPtr->AilImageTemp1, UserDataPtr->AilImageTemp2, 
      UserDataPtr->AilImageDisp, M_OR);

   // End the Section that highlights the processing. 
   MappTrace(M_DEFAULT,
      M_TRACE_SECTION_END,
      TRACE_TAG_PROCESSING,
      UserDataPtr->ProcessedImageCount,
      AIL_TEXT("Processing Image End"));

   // Signal that processing has been completed. 
   if (++(UserDataPtr->ProcessedImageCount) >= NUMBER_OF_FRAMES_TO_PROCESS)
      MthrControl(UserDataPtr->DoneEvent, M_EVENT_SET, M_SIGNALED);
   return 0;
}

```
