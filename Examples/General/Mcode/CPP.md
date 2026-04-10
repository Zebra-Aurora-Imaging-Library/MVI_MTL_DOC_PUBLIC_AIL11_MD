---
title: "Mcode"
description: "This program decodes a linear Code 39 and a DataMatrix code."
ms.language: "C++"
---

# Mcode

> This program decodes a linear Code 39 and a DataMatrix code.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufChild2d`, `MbufFree`, `MbufRestore`, `McodeAlloc`, `McodeAllocResult`, `McodeControl`, `McodeFree`, `McodeGetResult`, `McodeModel`, `McodeRead`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraRect`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Applications, Identifying, Modules, Buffer, Display, Graphics, Code Reader, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mcode.cpp
//
// Description: This program decodes a 1D Code 39 linear Barcode and a 2D DataMatrix code.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Target image character specifications. 
#define IMAGE_FILE                    M_IMAGE_PATH AIL_TEXT("Code.mim")

// Regions around 1D code. 
#define BARCODE_REGION_TOP_LEFT_X     256L
#define BARCODE_REGION_TOP_LEFT_Y      80L
#define BARCODE_REGION_SIZE_X         290L
#define BARCODE_REGION_SIZE_Y          60L

// Regions around 2D code. 
#define DATAMATRIX_REGION_TOP_LEFT_X    8L
#define DATAMATRIX_REGION_TOP_LEFT_Y  312L
#define DATAMATRIX_REGION_SIZE_X      118L
#define DATAMATRIX_REGION_SIZE_Y      105L

// Maximum length of the string to read. 
#define STRING_LENGTH_MAX              64L


int MosMain(void)
{
   AIL_ID      AilApplication,        // Application identifier.       
               AilSystem,             // System identifier.            
               AilDisplay,            // Display identifier.           
               AilImage,              // Image buffer identifier.      
               AilOverlayImage,       // Image buffer identifier.      
               DataMatrixRegion,      // Child containing DataMatrix.  
               DataMatrixCode,        // DataMatrix 2D code identifier.
               BarCodeRegion,         // Child containing Code39       
               Barcode,               // Code39 barcode identifier.    
               CodeResults;           // Barcode results identifier.   
   AIL_INT     BarcodeStatus;         // Decoding status.              
   AIL_INT     DataMatrixStatus;      // Decoding status.              
   AIL_DOUBLE  AnnotationColor = M_COLOR_GREEN, AnnotationBackColor = M_COLOR_GRAY;
   AIL_INT     n; 
   AIL_TEXT_CHAR OutputString    [STRING_LENGTH_MAX]; // Array of characters to draw.
   AIL_TEXT_CHAR BarcodeString   [STRING_LENGTH_MAX]; // Array of characters read.   
   AIL_TEXT_CHAR DataMatrixString[STRING_LENGTH_MAX]; // Array of characters read.   


   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Restore source image into an automatically allocated image buffer. 
   MbufRestore(IMAGE_FILE, AilSystem, &AilImage);

   // Display the image buffer. 
   MdispSelect(AilDisplay, AilImage);
       
   // Prepare for overlay annotations. 
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);

   // Prepare bar code results buffer 
   McodeAllocResult(AilSystem, M_DEFAULT, &CodeResults);

    // Pause to show the original image. 
   MosPrintf(AIL_TEXT("\n1D and 2D CODE READING:\n"));
   MosPrintf(AIL_TEXT("-----------------------\n\n"));
   MosPrintf(AIL_TEXT("This program will decode a linear Code 39 and a DataMatrix code.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();


   // 1D BARCODE READING: 
   
   // Create a read region around the code to speedup reading. 
   MbufChild2d(AilImage, BARCODE_REGION_TOP_LEFT_X, BARCODE_REGION_TOP_LEFT_Y,
               BARCODE_REGION_SIZE_X, BARCODE_REGION_SIZE_Y, &BarCodeRegion);
   
   // Allocate CODE objects. 
   McodeAlloc(AilSystem, M_DEFAULT, M_DEFAULT, &Barcode);
   McodeModel(Barcode, M_ADD, M_CODE39, M_NULL, M_DEFAULT, M_NULL);

   // Read codes from image. 
   McodeRead(Barcode, BarCodeRegion, CodeResults);
    
   // Get decoding status. 
   McodeGetResult(CodeResults, M_GENERAL, M_GENERAL, M_STATUS+M_TYPE_AIL_INT, &BarcodeStatus);

   // Check if decoding was successful. 
   if (BarcodeStatus == M_STATUS_READ_OK)
      {
      // Get decoded string. 
      McodeGetResult(CodeResults, 0, M_GENERAL, M_STRING, BarcodeString);

      // Draw the decoded strings and read region in the overlay image. 
      MgraControl(M_DEFAULT, M_COLOR, AnnotationColor); 
      MgraControl(M_DEFAULT, M_BACKCOLOR, AnnotationBackColor); 
      MosSprintf(OutputString, STRING_LENGTH_MAX, AIL_TEXT("\"%s\""), BarcodeString); 
      MgraText(M_DEFAULT, AilOverlayImage, BARCODE_REGION_TOP_LEFT_X+10,
               BARCODE_REGION_TOP_LEFT_Y+80, AIL_TEXT(" 1D linear 39 bar code:           "));
      MgraText(M_DEFAULT, AilOverlayImage, BARCODE_REGION_TOP_LEFT_X+200, 
               BARCODE_REGION_TOP_LEFT_Y+80, OutputString);
      MgraRect(M_DEFAULT, AilOverlayImage,
               BARCODE_REGION_TOP_LEFT_X, BARCODE_REGION_TOP_LEFT_Y,
               BARCODE_REGION_TOP_LEFT_X+BARCODE_REGION_SIZE_X, 
               BARCODE_REGION_TOP_LEFT_Y+BARCODE_REGION_SIZE_Y);
      }

   // Free objects. 
   McodeFree(Barcode);
   MbufFree(BarCodeRegion);

   // 2D CODE READING: 
  
   // Create a read region around the code to speedup reading. 
   MbufChild2d(AilImage, DATAMATRIX_REGION_TOP_LEFT_X, DATAMATRIX_REGION_TOP_LEFT_Y,
               DATAMATRIX_REGION_SIZE_X, DATAMATRIX_REGION_SIZE_Y, &DataMatrixRegion);
  
   // Allocate CODE objects. 
   McodeAlloc(AilSystem, M_DEFAULT, M_DEFAULT, &DataMatrixCode);
   McodeModel(DataMatrixCode, M_ADD, M_DATAMATRIX, M_NULL, M_DEFAULT, M_NULL);


   // Set the foreground value for the DataMatrix since it is different 
   // from the default value. 
   McodeControl(DataMatrixCode, M_FOREGROUND_VALUE, M_FOREGROUND_WHITE); 
    
   // Read codes from image. 
   McodeRead(DataMatrixCode, DataMatrixRegion, CodeResults);
    
   // Get decoding status. 
   McodeGetResult(CodeResults, M_GENERAL, M_GENERAL, M_STATUS + M_TYPE_AIL_INT, &DataMatrixStatus);

   // Check if decoding was successful. 
   if (DataMatrixStatus == M_STATUS_READ_OK)
      {
      // Get decoded string. 
      McodeGetResult(CodeResults, 0, M_GENERAL, M_STRING, DataMatrixString);

      // Draw the decoded strings and read region in the overlay image. 
      MgraControl(M_DEFAULT, M_COLOR, AnnotationColor); 
      MgraControl(M_DEFAULT, M_BACKCOLOR, AnnotationBackColor); 
      // Replace non printable characters with space.
      for (n=0; DataMatrixString[n] != AIL_TEXT('\0'); n++) 
          if ((DataMatrixString[n] < AIL_TEXT('0')) || (DataMatrixString[n] > AIL_TEXT('Z')))
              DataMatrixString[n] = AIL_TEXT(' ');
      MosSprintf(OutputString, STRING_LENGTH_MAX, AIL_TEXT("\"%s\" "), DataMatrixString); 
      MgraText(M_DEFAULT, 
               AilOverlayImage, 
               DATAMATRIX_REGION_TOP_LEFT_X,  
               DATAMATRIX_REGION_TOP_LEFT_Y+120, 
               AIL_TEXT(" 2D Data Matrix code:                  "));
      MgraText(M_DEFAULT, AilOverlayImage, DATAMATRIX_REGION_TOP_LEFT_X+200, 
               DATAMATRIX_REGION_TOP_LEFT_Y+120, OutputString);
      MgraRect(M_DEFAULT, AilOverlayImage,
               DATAMATRIX_REGION_TOP_LEFT_X, DATAMATRIX_REGION_TOP_LEFT_Y,
               DATAMATRIX_REGION_TOP_LEFT_X+DATAMATRIX_REGION_SIZE_X, 
               DATAMATRIX_REGION_TOP_LEFT_Y+DATAMATRIX_REGION_SIZE_Y);
      }

   // Free objects. 
   McodeFree(DataMatrixCode);
   MbufFree(DataMatrixRegion);

   // Free results buffer. 
   McodeFree(CodeResults);

   // Pause to show the results. 
   if ( (DataMatrixStatus == M_STATUS_READ_OK) && (BarcodeStatus == M_STATUS_READ_OK) )
      MosPrintf(AIL_TEXT("Decoding was successful and the strings were ")
                                   AIL_TEXT("written under each code.\n"));
   else
      MosPrintf(AIL_TEXT("Decoding error found.\n"));    
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   // Free other allocations. 
   MbufFree(AilImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}

```
