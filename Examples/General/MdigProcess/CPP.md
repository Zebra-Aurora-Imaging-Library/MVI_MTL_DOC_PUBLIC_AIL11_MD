---
title: "MdigProcess"
description: "This program shows the use of the MdigProcess() function to perform real-time processing."
ms.language: "C++"
---

# MdigProcess

> This program shows the use of the MdigProcess() function to perform real-time processing.

**Language:** C++

**Functions used:** `MdigGrabContinuous`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MimArith`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdigProcess.cpp
//
// Description: This program shows the use of the MdigProcess() function and its multiple
//              buffering acquisition to do robust real-time processing.
//
//              The user's processing code to execute is located in a callback function
//              that will be called for each frame acquired (see ProcessingFunction()).
//
// Note:        The average processing time must be shorter than the grab time or some
//              frames will be missed. Also, if the processing results are not displayed
//              and the frame count is not drawn or printed, the CPU usage is reduced
//              significantly.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Number of images in the buffering grab queue.
// Generally, increasing this number gives a better real-time grab.
#define BUFFERING_SIZE_MAX 20

// User's processing function prototype. 
AIL_INT MFTYPE ProcessingFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr);

// User's processing function hook data structure. 
typedef struct
   {
   AIL_ID  AilImageDisp;
   AIL_INT ProcessedImageCount;
   } HookDataStruct;


// Main function. 
// ---------------

int MosMain(void)
{
   AIL_ID AilApplication;
   AIL_ID AilSystem     ;
   AIL_ID AilDigitizer  ;
   AIL_ID AilDisplay    ;
   AIL_ID AilImageDisp  ;
   AIL_ID AilGrabBufferList[BUFFERING_SIZE_MAX] = { 0 };
   AIL_INT AilGrabBufferListSize;
   AIL_INT ProcessFrameCount   = 0;
   AIL_DOUBLE ProcessFrameRate = 0;
   HookDataStruct UserHookData;

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay,
      &AilDigitizer, M_NULL);

   // Allocate a monochrome display buffer. 
   MbufAlloc2d(AilSystem,
      MdigInquire(AilDigitizer, M_SIZE_X, M_NULL),
      MdigInquire(AilDigitizer, M_SIZE_Y, M_NULL),
      8 + M_UNSIGNED,
      M_IMAGE + M_GRAB + M_PROC + M_DISP,
      &AilImageDisp);
   MbufClear(AilImageDisp, M_COLOR_BLACK);

   // Display the image buffer. 
   MdispSelect(AilDisplay, AilImageDisp);

   // Print a message. 
   MosPrintf(AIL_TEXT("\nMULTIPLE BUFFERED PROCESSING.\n"));
   MosPrintf(AIL_TEXT("-----------------------------\n\n"));
   MosPrintf(AIL_TEXT("Press any key to start processing.\n\n"));

   // Grab continuously on the display and wait for a key press. 
   MdigGrabContinuous(AilDigitizer, AilImageDisp);
   MosGetch();

   // Halt continuous grab. 
   MdigHalt(AilDigitizer);

   // Allocate the grab buffers and clear them. 
   for (AilGrabBufferListSize = 0; AilGrabBufferListSize<BUFFERING_SIZE_MAX;
      AilGrabBufferListSize++)
      {
      // Minimum number of buffers required. 
      if (AilGrabBufferListSize == 2)
         MappControl(M_DEFAULT, M_ERROR, M_PRINT_DISABLE);

      MbufAlloc2d(AilSystem,
         MdigInquire(AilDigitizer, M_SIZE_X, M_NULL),
         MdigInquire(AilDigitizer, M_SIZE_Y, M_NULL),
         8 + M_UNSIGNED,
         M_IMAGE + M_GRAB + M_PROC,
         &AilGrabBufferList[AilGrabBufferListSize]);
      if (AilGrabBufferList[AilGrabBufferListSize])
         MbufClear(AilGrabBufferList[AilGrabBufferListSize], 0xFF);
      else
         break;
      }
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_ENABLE);

   // Initialize the user's processing function data structure. 
   UserHookData.AilImageDisp        = AilImageDisp;
   UserHookData.ProcessedImageCount = 0;

   // Start the processing. The processing function is called with every frame grabbed. 
   MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize,
               M_START, M_DEFAULT, ProcessingFunction, &UserHookData);


   // Here the main() is free to perform other tasks while the processing is executing. 
   // --------------------------------------------------------------------------------- 


   // Print a message and wait for a key press after a minimum number of frames. 
   MosPrintf(AIL_TEXT("Press any key to stop.                    \n\n"));
   MosGetch();

   // Stop the processing. 
   MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize,
               M_STOP, M_DEFAULT, ProcessingFunction, &UserHookData);

   // Print statistics. 
   MdigInquire(AilDigitizer, M_PROCESS_FRAME_COUNT,  &ProcessFrameCount);
   MdigInquire(AilDigitizer, M_PROCESS_FRAME_RATE,   &ProcessFrameRate);
   MosPrintf(AIL_TEXT("\n\n%d frames grabbed at %.1f frames/sec (%.1f ms/frame).\n"),
                        (int)ProcessFrameCount, ProcessFrameRate, 1000.0/ProcessFrameRate);
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   // Free the grab buffers. 
   while(AilGrabBufferListSize > 0)
         MbufFree(AilGrabBufferList[--AilGrabBufferListSize]);

   // Free display buffer. 
   MbufFree(AilImageDisp);

   // Release defaults. 
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, M_NULL);

   return 0;
}


// User's processing function called every time a grab buffer is ready. 
// -------------------------------------------------------------------- 

// Local defines. 
#define STRING_LENGTH_MAX  20
#define STRING_POS_X       20
#define STRING_POS_Y       20

AIL_INT MFTYPE ProcessingFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr)
   {
   HookDataStruct *UserHookDataPtr = (HookDataStruct *)HookDataPtr;
   AIL_ID ModifiedBufferId;
   AIL_TEXT_CHAR Text[STRING_LENGTH_MAX]= {AIL_TEXT('\0'),};

   // Retrieve the AIL_ID of the grabbed buffer. 
   MdigGetHookInfo(HookId, M_MODIFIED_BUFFER+M_BUFFER_ID, &ModifiedBufferId);

   // Increment the frame counter. 
   UserHookDataPtr->ProcessedImageCount++;

   // Print and draw the frame count (remove to reduce CPU usage). 
   MosPrintf(AIL_TEXT("Processing frame #%d.\r"), (int)UserHookDataPtr->ProcessedImageCount);
   MosSprintf(Text, STRING_LENGTH_MAX, AIL_TEXT("%d"), 
                                       (int)UserHookDataPtr->ProcessedImageCount);
   MgraText(M_DEFAULT, ModifiedBufferId, STRING_POS_X, STRING_POS_Y, Text);

   // Execute the processing and update the display. 
   MimArith(ModifiedBufferId, M_NULL, UserHookDataPtr->AilImageDisp, M_NOT);
   

   return 0;
   }

```
