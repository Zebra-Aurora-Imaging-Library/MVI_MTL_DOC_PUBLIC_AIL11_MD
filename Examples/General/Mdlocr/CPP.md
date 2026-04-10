---
title: "Mdlocr"
description: "This program uses the Deep Learning Ocr module to read a product expiry date. First all strings are read, then a model is defined based on the first read to filter out the target string using its size and dimensions."
ms.language: "C++"
---

# Mdlocr

> This program uses the Deep Learning Ocr module to read a product expiry date. First all strings are read, then a model is defined based on the first read to filter out the target string using its size and dimensions.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MdlocrAlloc`, `MdlocrAllocResult`, `MdlocrControl`, `MdlocrDefineModelFromResult`, `MdlocrDraw`, `MdlocrFree`, `MdlocrGetResult`, `MdlocrPreprocess`, `MdlocrRead`, `MgraControl`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Packaging, Applications, Reading, Modules, Buffer, Display, Deep Learning OCR, Graphics, What's New, AIL 10.0 SP7

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mdlocr.cpp
//
// Description: Synopsis: This program uses the Deep Learning Ocr module to read
//              a product expiry date. 
//              First all strings are read, then a model is defined based on the first
//              read to filter out the target string using its size and dimensions.
// 
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>  

// image file specifications.   
#define IMAGE_FILE_TO_READ    M_IMAGE_PATH AIL_TEXT("ExpiryDateAndLot2.mim")
// Max string sizes. 
#define STRING_MAX_SIZE             256L
// Invalid string index
#define INVALID_TARGET_READ_INDEX   -1L
// The target string
#define TARGET_STRING AIL_TEXT("2012JL03ERB1A2217")

//***************************************************************************
// Read. 
AIL_INT Read(AIL_ID AilDisplay, AIL_ID AilDlocrContext, AIL_ID AilImage,
          AIL_ID AilOverlayImage, AIL_ID AilDlocrResult, AIL_INT StringMatchingMode)
   {
   AIL_INT ReadStatus;                             // Status of the read operation
   AIL_INT NumberOfStringRead;                     // Total number of strings to read.
   AIL_TEXT_CHAR StringResult[STRING_MAX_SIZE+1];  // String of characters read.      

   // Perform the read operation on the specified target image. 
   MdlocrRead(AilDlocrContext, AilImage, AilDlocrResult, StringMatchingMode);

   // Get the status of the operation
   MdlocrGetResult(AilDlocrResult, M_GENERAL, M_DEFAULT, M_STATUS, &ReadStatus);
   if(ReadStatus != M_COMPLETE)
      {
      MosPrintf(AIL_TEXT(
         "Read operation failed.\n"));
      return INVALID_TARGET_READ_INDEX;
      }

   // Get number of strings read and show the result. 
   MdlocrGetResult(AilDlocrResult, M_GENERAL, M_GENERAL, M_STRING_NUMBER + M_TYPE_AIL_INT, 
                   &NumberOfStringRead);

   // Clear the display overlay. 
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);

   // Find the target string index.
   AIL_INT TargetStringIndex = INVALID_TARGET_READ_INDEX;

   if(NumberOfStringRead >= 1)
      {
      MosPrintf(
         AIL_TEXT("The image was read successfully.\n\n"));

      // Draw read result. 
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_CYAN);
      MdlocrDraw(M_DEFAULT, AilDlocrResult, AilOverlayImage, M_DRAW_STRING,
                 M_ALL, M_DEFAULT, M_DEFAULT);
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);
      MdlocrDraw(M_DEFAULT, AilDlocrResult, AilOverlayImage, M_DRAW_STRING_BOX,
                 M_ALL, M_DEFAULT, M_DEFAULT);
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);
      MdlocrDraw(M_DEFAULT, AilDlocrResult, AilOverlayImage, M_DRAW_STRING_INDEX,
                 M_ALL, M_DEFAULT, M_DEFAULT);

      // Print the read result. 
      MosPrintf(AIL_TEXT(" String\n") );
      MosPrintf(AIL_TEXT(" -----------------------------------\n") );
      for(AIL_INT i = 0; i < NumberOfStringRead; ++i)
         {
         MdlocrGetResult(AilDlocrResult, i, M_GENERAL, M_STRING, StringResult);
         MosPrintf(AIL_TEXT(" %s\n"), StringResult);

         // Check if the string read is the one being looked for.
         if(MosStrcmp(StringResult, TARGET_STRING) == 0)
            {
            TargetStringIndex = i; 
            }
         }
      MosPrintf(AIL_TEXT("\n"));
      }

   if(TargetStringIndex == INVALID_TARGET_READ_INDEX)
      {
      MosPrintf(AIL_TEXT(
         "Target string '%s' was not found.\n"), TARGET_STRING);
      } 

   return TargetStringIndex;
   }
 
//***************************************************************************
// Main. 
int MosMain(void)
   { 
   AIL_ID AilApplication;                   // Application identifier.        
   AIL_ID AilSystem;                        // System identifier.             
   AIL_ID AilDisplay;                       // Display identifier.            
   AIL_ID AilImage;                         // Image buffer identifier.       
   AIL_ID AilOverlayImage;                  // Overlay image.                 
   AIL_ID AilDlocrContext;                  // Dlocr context identifier.      
   AIL_ID AilDlocrResult;                   // Dlocr result buffer identifier.

   AIL_INT MaxSizeX;
   AIL_INT MaxSizeY;

   // Print the example synopsis. 
   MosPrintf(AIL_TEXT(
      "[EXAMPLE NAME]\n"));
   MosPrintf(AIL_TEXT(
      "Mdlocr\n\n"));
   MosPrintf(AIL_TEXT(
      "[SYNOPSIS]\n"));
   MosPrintf(AIL_TEXT(
      "This program uses the Deep Learning Ocr module to read a product\n"));
   MosPrintf(AIL_TEXT(
      "expiry date.\n"));
   MosPrintf(AIL_TEXT(
      "First all strings are read, then a model is defined based on the first\n"));
   MosPrintf(AIL_TEXT(
      "read to filter out the target string using its size and dimensions.\n\n"));
   MosPrintf(AIL_TEXT(
      "[MODULES USED]\n"));
   MosPrintf(AIL_TEXT(
      "Deep Learning OCR, Buffer, Display, Graphics.\n\n"));
 
   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Restore the font definition image. 
   MbufRestore(IMAGE_FILE_TO_READ, AilSystem, &AilImage);

   // Display the image and prepare for overlay annotations. 
   MdispSelect(AilDisplay, AilImage);
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);

   // Allocate a new empty dlocr context. 
   MdlocrAlloc(AilSystem, M_OCR_NET1_BALANCED_V1, M_DEFAULT, &AilDlocrContext);

   // Allocate a new empty dlocr result buffer. 
   MdlocrAllocResult(AilSystem, M_DLOCR_READ_RESULT, M_DEFAULT, &AilDlocrResult);

   // Set the context's target image max size X and Y
   // Note that these dimensions can be superior to encompass several images.
   MbufInquire(AilImage, M_SIZE_X, &MaxSizeX);
   MbufInquire(AilImage, M_SIZE_Y, &MaxSizeY);
   MdlocrControl(AilDlocrContext, M_TARGET_MAX_SIZE_X, MaxSizeX);
   MdlocrControl(AilDlocrContext, M_TARGET_MAX_SIZE_Y, MaxSizeY);

   // Preprocess the dlocr context. 
   MdlocrPreprocess(AilDlocrContext, M_DEFAULT);

   // First read. 
   AIL_INT TargetStringIndex = Read(AilDisplay, AilDlocrContext, AilImage, AilOverlayImage, AilDlocrResult, M_READ_ALL);

   // Check if the read operation successfully completed. 
   if(TargetStringIndex != INVALID_TARGET_READ_INDEX)
      { 
      // Pause to show results.  
      MosPrintf(AIL_TEXT(
         "First read without string model, the best before date was detected among\n"));
      MosPrintf(AIL_TEXT(
         "others. We note the associated index %d (in red). It will be used in the next step\n"), TargetStringIndex);
      MosPrintf(AIL_TEXT(
         "to filter out the targeted string.\n"));

      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Define string model from result. 
      MdlocrDefineModelFromResult(AilDlocrContext, M_DEFAULT, AilDlocrResult,
                                  TargetStringIndex, M_DEFAULT);

      // This modification necessitates a new Preprocess. 
      MdlocrPreprocess(AilDlocrContext, M_DEFAULT);

      // Second read. 
      Read(AilDisplay, AilDlocrContext, AilImage, AilOverlayImage, AilDlocrResult, M_MODEL_BASED);

      // Pause to show results.  
      MosPrintf(AIL_TEXT(
         "We defined a string model from the result to filter out based on string size\n"));
      MosPrintf(AIL_TEXT(
         "and character dimensions.\n"));
      MosPrintf(AIL_TEXT(
         "Second read with the string model. Target string is detected.\n"));
      }

   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch(); 

   MdlocrFree(AilDlocrContext);
   MdlocrFree(AilDlocrResult);
   MbufFree(AilImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }


```
