---
title: "Mclass"
description: "This example identifies the type of product using a pre-trained classification module."
ms.language: "C++"
---

# Mclass

> This example identifies the type of product using a pre-trained classification module.

**Language:** C++

**Functions used:** `MclassPredict`, `MclassPreprocess`, `MclassRestore`, `MclassGetResult`, `MclassInquire`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraControl`, `MgraFont`, `MgraRect`, `MgraText`, `MimResize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Identifying, Inspecting/Proofing/Verifying, Modules, Classification, Buffer, Display, Graphics, Image Processing, What's New, Older, AIL 10.0 U94

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mclass.cpp
//
// Description: This example identifies the type of pastas using a 
//              pre-trained classification module. 
//
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Path definitions.
#define EXAMPLE_IMAGE_DIR_PATH   M_IMAGE_PATH AIL_TEXT("/Classification/Pasta/")
#define EXAMPLE_CLASS_CTX_PATH   EXAMPLE_IMAGE_DIR_PATH AIL_TEXT("PastaEx.mclass")
#define TARGET_IMAGE_DIR_PATH    EXAMPLE_IMAGE_DIR_PATH AIL_TEXT("Products")

#define DIG_IMAGE_FOLDER         TARGET_IMAGE_DIR_PATH
#define DIG_REMOTE_IMAGE_FOLDER  AIL_TEXT("remote:///") TARGET_IMAGE_DIR_PATH

// Util constant.
#define BUFFERING_SIZE_MAX 10

// Function declarations.
struct ClassStruct
   {
   AIL_INT NbCategories,
           NbOfFrames,
           SourceSizeX,
           SourceSizeY;

   AIL_ID ClassCtx,
          ClassRes,
          AilDisplay,
          AilDispChild,
          AilOverlayImage;
   };

AIL_INT MFTYPE ClassificationFunc(AIL_INT HookType,
                                  AIL_ID  EventId,
                                  void*   pHookData);

void ProcessStatus(AIL_INT Status);

void SetupDisplay(AIL_ID  AilSystem,
                  AIL_ID  AilDisplay,
                  AIL_INT SourceSizeX,
                  AIL_INT SourceSizeY,
                  AIL_ID  ClassCtx,
                  AIL_ID  &AilDispImage,
                  AIL_ID  &AilDispChild,
                  AIL_ID  &AilOverlay,
                  AIL_INT NbCategories);

//****************************************************************************
//    Main.
//****************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication,    // Application identifier
          AilSystem,         // System identifier
          AilDisplay,        // Display identifier
          AilOverlay,        // Overlay identifier
          AilDigitizer,      // Digitizer identifier
          AilDispImage,      // Image identifier
          AilDispChild,      // Image identifier
          ClassCtx,          // Classification Context
          ClassRes;          // Classification Result

   AIL_ID AilGrabBufferList[BUFFERING_SIZE_MAX],   // Image identifier
          AilChildBufferList[BUFFERING_SIZE_MAX];  // Child identifier

   AIL_INT NumberOfCategories,
           BufIndex,
           SourceSizeX,
           SourceSizeY,
           InputSizeX,
           InputSizeY;

   NumberOfCategories = 0;

   // Allocate objects.
   MappAlloc(M_NULL, M_DEFAULT, &AilApplication);
   MsysAlloc(M_DEFAULT, M_SYSTEM_DEFAULT, M_DEFAULT, M_DEFAULT, &AilSystem);
   AIL_INT SystemType {};
   MsysInquire(AilSystem, M_SYSTEM_TYPE, &SystemType);
   if(SystemType != M_SYSTEM_HOST_TYPE)
      {
      MsysFree(AilSystem);
      MsysAlloc(M_DEFAULT, M_SYSTEM_HOST, M_DEFAULT, M_DEFAULT, &AilSystem);
      }

   MdispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, &AilDisplay);
    
   AIL_INT AilSystemLocation = MsysInquire(AilSystem, M_LOCATION, M_NULL);
   AIL_CONST_TEXT_PTR DigImageFolder = (AilSystemLocation == M_REMOTE) ? DIG_REMOTE_IMAGE_FOLDER : DIG_IMAGE_FOLDER;
   MdigAlloc(AilSystem, M_DEFAULT, DigImageFolder, M_DEFAULT, &AilDigitizer);
      
   // Print the example synopsis.
   MosPrintf(AIL_TEXT("[EXAMPLE NAME]\n"));
   MosPrintf(AIL_TEXT("Mclass\n\n"));
   MosPrintf(AIL_TEXT("[SYNOPSIS]\n"));
   MosPrintf(AIL_TEXT("This programs shows the use of a pre-trained classification\n"));
   MosPrintf(AIL_TEXT("tool to recognize product categories.\n\n"));
   MosPrintf(AIL_TEXT("[MODULES USED]\n"));
   MosPrintf(AIL_TEXT("Classification, Buffer, Display, Graphics, Image Processing.\n\n"));

   // Wait for user.
   MosPrintf(AIL_TEXT("Press any key to continue.\n"));
   MosGetch();
   
   MosPrintf(AIL_TEXT("Restoring the classification context from file.."));
   MclassRestore(EXAMPLE_CLASS_CTX_PATH, AilSystem, M_DEFAULT, &ClassCtx);
   MosPrintf(AIL_TEXT("."));

   // Preprocess the context.
   MclassPreprocess(ClassCtx, M_DEFAULT);
   MosPrintf(AIL_TEXT(".ready.\n"));

   MclassInquire(ClassCtx, M_CONTEXT, M_NUMBER_OF_CLASSES   + M_TYPE_AIL_INT, &NumberOfCategories);
   MclassInquire(ClassCtx, M_DEFAULT_SOURCE_LAYER, M_SIZE_X + M_TYPE_AIL_INT, &InputSizeX);
   MclassInquire(ClassCtx, M_DEFAULT_SOURCE_LAYER, M_SIZE_Y + M_TYPE_AIL_INT, &InputSizeY);

   if(NumberOfCategories > 0)
      {
      // Inquire and print source layer information.
      MosPrintf(AIL_TEXT(" - The classifier was trained to recognize %d categories\n"), NumberOfCategories);
      MosPrintf(AIL_TEXT(" - The classifier was trained for %dx%d source images\n\n"), InputSizeX, InputSizeY);

      // Allocate a classification result buffer.
      MclassAllocResult(AilSystem, M_PREDICT_CNN_RESULT, M_DEFAULT, &ClassRes);

      // Inquire the size of the source image.
      MdigInquire(AilDigitizer, M_SIZE_X, &SourceSizeX);
      MdigInquire(AilDigitizer, M_SIZE_Y, &SourceSizeY);

      // Setup the example display.
      SetupDisplay(AilSystem,
                   AilDisplay,
                   SourceSizeX,
                   SourceSizeY,
                   ClassCtx,
                   AilDispImage,
                   AilDispChild,
                   AilOverlay,
                   NumberOfCategories);

      // Retrieve the number of frame in the source directory.
      AIL_INT NumberOfFrames;
      MdigInquire(AilDigitizer, M_SOURCE_NUMBER_OF_FRAMES, &NumberOfFrames);

      // Prepare data for Hook Function.
      ClassStruct ClassificationData;
      ClassificationData.ClassCtx = ClassCtx;
      ClassificationData.ClassRes = ClassRes;
      ClassificationData.AilDisplay = AilDisplay;
      ClassificationData.AilDispChild = AilDispChild;
      ClassificationData.NbCategories = NumberOfCategories;
      ClassificationData.AilOverlayImage = AilOverlay;
      ClassificationData.SourceSizeX = SourceSizeX;
      ClassificationData.SourceSizeY = SourceSizeY;
      ClassificationData.NbOfFrames = NumberOfFrames;

      // Allocate the grab buffers.
      for(BufIndex = 0; BufIndex < BUFFERING_SIZE_MAX; BufIndex++)
         {
         MbufAlloc2d(AilSystem, SourceSizeX, SourceSizeY, 8 + M_UNSIGNED, M_IMAGE + M_GRAB + M_PROC, &AilGrabBufferList[BufIndex]);
         MbufChild2d(AilGrabBufferList[BufIndex], (SourceSizeX - InputSizeX) / 2, (SourceSizeY - InputSizeY) / 2, InputSizeX, InputSizeY, &AilChildBufferList[BufIndex]);
         MobjControl(AilGrabBufferList[BufIndex], M_OBJECT_USER_DATA_PTR, &AilChildBufferList[BufIndex]);
         }

      // Start the grab.
      if(NumberOfFrames != M_INFINITE)
         MdigProcess(AilDigitizer, AilGrabBufferList, BUFFERING_SIZE_MAX, M_SEQUENCE + M_COUNT(NumberOfFrames), M_SYNCHRONOUS, &ClassificationFunc, &ClassificationData);
      else
         MdigProcess(AilDigitizer, AilGrabBufferList, BUFFERING_SIZE_MAX, M_START, M_DEFAULT, &ClassificationFunc, &ClassificationData);

      // Ready to exit.
      MosPrintf(AIL_TEXT("\nPress any key to exit.\n"));
      MosGetch();

      // Stop the digitizer.
      MdigProcess(AilDigitizer, AilGrabBufferList, BUFFERING_SIZE_MAX, M_STOP, M_DEFAULT, M_NULL, M_NULL);

      MbufFree(AilDispChild);
      MbufFree(AilDispImage);

      for(BufIndex = 0; BufIndex < BUFFERING_SIZE_MAX; BufIndex++)
         {
         MbufFree(AilChildBufferList[BufIndex]);
         MbufFree(AilGrabBufferList[BufIndex]);
         }

      MclassFree(ClassRes);
      MclassFree(ClassCtx);
      }

   // Free the allocated resources.
   MdigFree(AilDigitizer);

   MdispFree(AilDisplay);
   MsysFree(AilSystem);
   MappFree(AilApplication);

   return 0;
   }

void SetupDisplay(AIL_ID  AilSystem,
                  AIL_ID  AilDisplay,
                  AIL_INT SourceSizeX,
                  AIL_INT SourceSizeY,
                  AIL_ID  ClassCtx,
                  AIL_ID  &AilDispImage,
                  AIL_ID  &AilDispChild,
                  AIL_ID  &AilOverlay,
                  AIL_INT NbCategories
                  )
   {
   AIL_ID AilImageLoader,  // Image identifier       
          AilChildSample;  // Child image identifier
          
   // Allocate a color buffer.
   AIL_INT IconSize = SourceSizeY / NbCategories;
   AilDispImage = MbufAllocColor(AilSystem, 3, SourceSizeX + IconSize, SourceSizeY, 8 + M_UNSIGNED, M_IMAGE + M_PROC + M_DISP, M_NULL);
   MbufClear(AilDispImage, M_COLOR_BLACK);
   AilDispChild = MbufChild2d(AilDispImage, 0, 0, SourceSizeX, SourceSizeY, M_NULL);

   // Set annotation color.
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);

   // Setup the display.
   for (int iter = 0; iter < NbCategories; iter++)
      {
      // Allocate a child buffer per product categorie.   
      MbufChild2d(AilDispImage, SourceSizeX, iter * IconSize, IconSize, IconSize, &AilChildSample);

      // Load the sample image.
      MclassInquire(ClassCtx, M_CLASS_INDEX(iter), M_CLASS_ICON_ID + M_TYPE_AIL_ID, &AilImageLoader);
      
      if (AilImageLoader != M_NULL)
         { MimResize(AilImageLoader, AilChildSample, M_FILL_DESTINATION, M_FILL_DESTINATION, M_BICUBIC + M_OVERSCAN_FAST); }

      // Draw an initial red rectangle around the buffer.
      MgraRect(M_DEFAULT, AilChildSample, 0, 1, IconSize - 1, IconSize - 2);

      // Free the allocated buffers.
      MbufFree(AilChildSample);
      }

   // Display the window with black color.
   MdispSelect(AilDisplay, AilDispImage);

   // Prepare for overlay annotations.
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   AilOverlay = MdispInquire(AilDisplay, M_OVERLAY_ID, M_NULL);
   }

void ProcessStatus(AIL_INT Status)
   {
   if(Status != M_COMPLETE)
      {
      MosPrintf(AIL_TEXT("The prediction failed to complete.\n"));
      MosPrintf(AIL_TEXT("The status returned was: "));
      switch(Status)
         {
         default:
         case M_INTERNAL_ERROR:
            MosPrintf(AIL_TEXT("M_INTERNAL_ERROR\n"));
            break;
         case M_PREDICT_NOT_PERFORMED:
            MosPrintf(AIL_TEXT("M_PREDICT_NOT_PERFORMED\n"));
            break;
         case M_CURRENTLY_PREDICTING:
            MosPrintf(AIL_TEXT("M_CURRENTLY_PREDICTING\n"));
            break;
         case M_STOPPED_BY_REQUEST:
            MosPrintf(AIL_TEXT("M_STOPPED_BY_REQUEST\n"));
            break;
         case M_TIMEOUT_REACHED:
            MosPrintf(AIL_TEXT("M_TIMEOUT_REACHED\n"));
            break;
         case M_NOT_ENOUGH_MEMORY:
            MosPrintf(AIL_TEXT("M_NOT_ENOUGH_MEMORY\n"));
            break;
         case M_INVALID_REGION_BUFFER:
            MosPrintf(AIL_TEXT("M_INVALID_REGION_BUFFER\n"));
            break;
         }
      }
   }

AIL_INT MFTYPE ClassificationFunc(AIL_INT /*HookType*/, AIL_ID EventId, void* DataPtr)
   {
   AIL_ID AilImage, *pAilInputImage;
   MdigGetHookInfo(EventId, M_MODIFIED_BUFFER + M_BUFFER_ID, &AilImage);

   ClassStruct* data = static_cast<ClassStruct*>(DataPtr);
   MdispControl(data->AilDisplay, M_UPDATE, M_DISABLE);
   MobjInquire(AilImage, M_OBJECT_USER_DATA_PTR, (void**)&pAilInputImage);
   
   // Display the new target image.
   MbufCopy(AilImage, data->AilDispChild);

   // Perform product recognition using the classification module.
   MclassPredict(data->ClassCtx, *pAilInputImage, data->ClassRes, M_DEFAULT);

   AIL_INT Status {};
   MclassGetResult(data->ClassRes, M_DEFAULT, M_STATUS + M_TYPE_AIL_INT, &Status);
   ProcessStatus(Status);

   // Retrieve best classification score and class index.
   AIL_DOUBLE BestScore;
   MclassGetResult(data->ClassRes, M_GENERAL, M_BEST_CLASS_SCORE + M_TYPE_AIL_DOUBLE, &BestScore);

   AIL_INT BestIndex;
   MclassGetResult(data->ClassRes, M_GENERAL, M_BEST_CLASS_INDEX + M_TYPE_AIL_INT, &BestIndex);

   // Clear the overlay buffer.
   MdispControl(data->AilDisplay, M_OVERLAY_CLEAR, M_TRANSPARENT_COLOR);
   
   // Draw a green rectangle around the winning sample.
   AIL_INT IconSize = data->SourceSizeY / data->NbCategories;
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);
   MgraRect(M_DEFAULT, data->AilOverlayImage, data->SourceSizeX, (BestIndex*IconSize)+1, data->SourceSizeX + IconSize - 1, (BestIndex + 1)*IconSize - 2);

   // Print the classification accuracy in the sample buffer.
   AIL_TEXT_CHAR Accuracy_text[256];
   MosSprintf(Accuracy_text, 256, AIL_TEXT("%.1lf%% score"), BestScore);
   MgraControl(M_DEFAULT, M_BACKGROUND_MODE, M_TRANSPARENT);
   MgraFont(M_DEFAULT, M_FONT_DEFAULT_SMALL);
   MgraText(M_DEFAULT, data->AilOverlayImage, data->SourceSizeX+ 2, BestIndex*IconSize + 4, Accuracy_text);

   // Update the display.
   MdispControl(data->AilDisplay, M_UPDATE, M_ENABLE);

   // Wait for the user.
   if (data->NbOfFrames != M_INFINITE)
      {
      MosPrintf(AIL_TEXT("Press any key to continue.\r"));
      MosGetch();
      }

   return 0;
   }

```
