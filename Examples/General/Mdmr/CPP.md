---
title: "Mdmr"
description: "This program uses the SureDotOCR® module to read a product expiry date and lot number printed using a CIJ printer."
ms.language: "C++"
---

# Mdmr

> This program uses the SureDotOCR® module to read a product expiry date and lot number printed using a CIJ printer.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MdmrAlloc`, `MdmrAllocResult`, `MdmrControl`, `MdmrControlStringModel`, `MdmrDraw`, `MdmrFree`, `MdmrGetResult`, `MdmrImportFont`, `MdmrPreprocess`, `MdmrRead`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Packaging, Applications, Reading, Modules, Buffer, Display, Graphics, SureDotOCR®, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mdmr.cpp
//
// Description: This program uses the Dot Matrix Reader (SureDotOCR®) module to
//              read a product expiry date and lot number printed using a
//              CIJ printer.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// File specifications. 
#define IMAGE_FILE_TO_READ    M_IMAGE_PATH   AIL_TEXT("ExpiryDateAndLot.mim")
#define FONT_FILE_TO_IMPORT   M_CONTEXT_PATH AIL_TEXT("ExpiryDateAndLotFont5x7.mdmrf")

// Dot Matrix Reader settings. 
const AIL_DOUBLE STRING_DOT_DIAMETER = 6.0;
const AIL_DOUBLE TEXT_BLOCK_WIDTH    = 400;
const AIL_DOUBLE TEXT_BLOCK_HEIGHT   = 100;
const AIL_INT    EXPIRY_DATE_LENGTH  = 7;
const AIL_INT    LOT_NUMBER_LENGTH   = 7;

// Util. 
const AIL_INT TEXT_MAX_SIZE = 128;

//***************************************************************************
// Main. 
int MosMain(void)
   {
   AIL_ID AilApplication,                          // Application identifier.         
          AilSystem,                               // System identifier.              
          AilDisplay,                              // Display identifier.             
          AilImage,                                // Image buffer identifier.        
          AilOverlay,                              // Overlay image.                  
          AilDmrContext,                           // Dot Matrix context identifier.  
          AilDmrResult;                            // Dot Matrix result identifier.   

   AIL_INT NumberOfStrings = 0,                    // Total number of strings to read.
           StringSize = 0,                         // Number of strings characters.   
           StringModelIndex = 0,                   // String model index.             
           Index = 0;                              // Result index.                   

   AIL_TEXT_CHAR StringResult[TEXT_MAX_SIZE + 1],  // String of characters read.      
                 PrintText[TEXT_MAX_SIZE + 1];     // Util text.                      

   // Print module name. 
   MosPrintf(AIL_TEXT("\nDOT MATRIX READER (SureDotOCR) MODULE:\n"));
   MosPrintf(AIL_TEXT("--------------------------------------\n\n"));

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Restore the font definition image. 
   MbufRestore(IMAGE_FILE_TO_READ, AilSystem, &AilImage);

   // Display the image and prepare for overlay annotations. 
   MdispSelect(AilDisplay, AilImage);
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlay);

   // Allocate a new empty Dot Matrix Reader context. 
   MdmrAlloc(AilSystem, M_DOT_MATRIX, M_DEFAULT, &AilDmrContext);

   // Allocate a new empty Dot Matrix Reader result buffer. 
   MdmrAllocResult(AilSystem, M_DOT_MATRIX, M_DEFAULT, &AilDmrResult);

   // Import a Dot Matrix font. 
   MdmrImportFont(FONT_FILE_TO_IMPORT, M_DMR_FONT_FILE, AilDmrContext, M_DEFAULT, M_NULL, M_DEFAULT);

   // Add a new string model definition for the product lot number. 
   // ------------------------------------------------------------- 
   MdmrControl(AilDmrContext, M_STRING_ADD, M_DEFAULT);

   // Set the string model rank and size. 
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(0), M_DEFAULT, M_STRING_RANK,      1, M_DEFAULT, M_NULL);
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(0), M_DEFAULT, M_STRING_SIZE_MIN_MAX, LOT_NUMBER_LENGTH, LOT_NUMBER_LENGTH, M_NULL);

   // Add a new string model definition for the expiry date (YYYY MM DD). 
   // ------------------------------------------------------------------- 
   MdmrControl(AilDmrContext, M_STRING_ADD, M_DEFAULT);

   // Set the string model rank and size. 
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(1), M_DEFAULT, M_STRING_RANK,     0, M_DEFAULT, M_NULL);
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(1), M_DEFAULT, M_STRING_SIZE_MIN_MAX, EXPIRY_DATE_LENGTH, EXPIRY_DATE_LENGTH, M_NULL);

   // Set the string model constraint for an expiry date (DDMMMYY). 
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(1), M_POSITION_IN_STRING(0), M_ADD_PERMITTED_CHARS_ENTRY, M_FONT_LABEL(M_ANY), M_DIGITS,            M_NULL);
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(1), M_POSITION_IN_STRING(1), M_ADD_PERMITTED_CHARS_ENTRY, M_FONT_LABEL(M_ANY), M_DIGITS,            M_NULL);
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(1), M_POSITION_IN_STRING(2), M_ADD_PERMITTED_CHARS_ENTRY, M_FONT_LABEL(M_ANY), M_LETTERS_UPPERCASE, M_NULL);
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(1), M_POSITION_IN_STRING(3), M_ADD_PERMITTED_CHARS_ENTRY, M_FONT_LABEL(M_ANY), M_LETTERS_UPPERCASE, M_NULL);
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(1), M_POSITION_IN_STRING(4), M_ADD_PERMITTED_CHARS_ENTRY, M_FONT_LABEL(M_ANY), M_LETTERS_UPPERCASE, M_NULL);
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(1), M_POSITION_IN_STRING(5), M_ADD_PERMITTED_CHARS_ENTRY, M_FONT_LABEL(M_ANY), M_DIGITS,            M_NULL);
   MdmrControlStringModel(AilDmrContext, M_STRING_INDEX(1), M_POSITION_IN_STRING(6), M_ADD_PERMITTED_CHARS_ENTRY, M_FONT_LABEL(M_ANY), M_DIGITS,            M_NULL);

   MosPrintf(AIL_TEXT("A Dot Matrix Reader (SureDotOCR) context was set up to read\n")
             AIL_TEXT("an expiry date and a lot number from a target image.\n\n"));

   // Set the Dot Matrix dot diameter. 
   MdmrControl(AilDmrContext, M_DOT_DIAMETER, STRING_DOT_DIAMETER);

   // Set the maximum size of the string box. 
   MdmrControl(AilDmrContext, M_TEXT_BLOCK_WIDTH, TEXT_BLOCK_WIDTH);
   MdmrControl(AilDmrContext, M_TEXT_BLOCK_HEIGHT, TEXT_BLOCK_HEIGHT);

   // Preprocess the context. 
   MdmrPreprocess(AilDmrContext, M_DEFAULT);

   // Reading the string from a target image. 
   MdmrRead(AilDmrContext, AilImage, AilDmrResult, M_DEFAULT);

   // Get number of strings read and show the result. 
   MdmrGetResult(AilDmrResult, M_GENERAL, M_DEFAULT, M_STRING_NUMBER + M_TYPE_AIL_INT, &NumberOfStrings);

   // Draw the read result. 
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);
   MdmrDraw(M_DEFAULT, AilDmrResult, AilOverlay, M_DRAW_STRING_CHAR_BOX, M_DEFAULT, M_DEFAULT, M_DEFAULT);
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_CYAN);
   MdmrDraw(M_DEFAULT, AilDmrResult, AilOverlay, M_DRAW_STRING_CHAR_POSITION, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   if( NumberOfStrings > 0)
      {
      MosPrintf(AIL_TEXT("Result: %d strings have been read:\n\n"), NumberOfStrings);

      for (Index = 0; Index < NumberOfStrings; Index++)
         {
         // Print the read result. 
         MdmrGetResult(AilDmrResult, Index, M_GENERAL, M_STRING_MODEL_INDEX + M_TYPE_AIL_INT, &StringModelIndex);
         MdmrGetResult(AilDmrResult, Index, M_GENERAL, M_STRING + M_STRING_SIZE + M_TYPE_AIL_INT, &StringSize);
         MdmrGetResult(AilDmrResult, Index, M_GENERAL, M_STRING, &StringResult[0]);

         switch (StringModelIndex)
            {
            case 0:
               MosSprintf(PrintText, TEXT_MAX_SIZE, AIL_TEXT(" LOT# : %s "), StringResult);
               MgraText(M_DEFAULT, AilOverlay, 20, 20 + Index * 20, PrintText);
               MosPrintf(AIL_TEXT(" LOT# : %s\n"), StringResult);
               break;
            case 1:
               MosSprintf(PrintText, TEXT_MAX_SIZE, AIL_TEXT(" EXP. : %s "), StringResult);
               MgraText(M_DEFAULT, AilOverlay, 20, 20 + Index * 20, PrintText);
               MosPrintf(AIL_TEXT(" EXPIRY DATE: %s\n"), StringResult);
               break;
            default:
               MosPrintf(AIL_TEXT("Unexpected model index\n"));
               break;
            }
         }
      }
   else
      {
      MosPrintf(AIL_TEXT("Error: no string found.\n"));
      }

   // Pause to show results. 
   MosPrintf(AIL_TEXT("\nPress any key to end.\n\n"));
   MosGetch();

   // Free all allocations. 
   MdmrFree(AilDmrContext);
   MdmrFree(AilDmrResult);
   MbufFree(AilImage);

   // Free defaults. 
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }

```
