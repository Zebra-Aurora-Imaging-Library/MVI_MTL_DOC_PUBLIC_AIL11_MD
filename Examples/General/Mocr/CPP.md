---
title: "Mocr"
description: "This program uses the OCR module to calibrate a predefined OCR font (SEMI font) and to read a string in the image. The string read is then printed to the screen and the calibrated font is saved to disk."
ms.language: "C++"
---

# Mocr

> This program uses the OCR module to calibrate a predefined OCR font (SEMI font) and to read a string in the image. The string read is then printed to the screen and the calibrated font is saved to disk.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufChild2d`, `MbufClear`, `MbufDiskInquire`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraFont`, `MgraText`, `MocrAllocResult`, `MocrCalibrateFont`, `MocrCopyFont`, `MocrFree`, `MocrGetResult`, `MocrReadString`, `MocrRestoreFont`, `MocrSaveFont`, `MocrSetConstraint`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Semiconductor, Applications, Reading, Modules, Buffer, Display, Graphics, OCR, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MOcr.cpp
//
// Description: This program uses the OCR module to read a SEMI font string:
//              the example calibrates a predefined OCR font (semi font) 
//              and uses it to read a string in the image. The string read
//              is then printed to the screen and the calibrated font is
//              saved to disk.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// *****************************************************************************
// OCR SEMI font read example.
// *****************************************************************************

// Target image character specifications.
#define CHAR_IMAGE_FILE        M_IMAGE_PATH AIL_TEXT("OcrSemi1292.mim")
#define CHAR_SIZE_X_MIN        22.0
#define CHAR_SIZE_X_MAX        23.0
#define CHAR_SIZE_X_STEP       0.50
#define CHAR_SIZE_Y_MIN        43.0
#define CHAR_SIZE_Y_MAX        44.0
#define CHAR_SIZE_Y_STEP       0.50

// Target reading specifications.
#define READ_REGION_POS_X      30L  
#define READ_REGION_POS_Y      40L
#define READ_REGION_WIDTH      420L
#define READ_REGION_HEIGHT     70L
#define READ_SCORE_MIN         50.0

// Font file names.
#define FONT_FILE_IN           M_CONTEXT_PATH AIL_TEXT("SEMI_M12-92.mfo")
#define FONT_FILE_OUT          M_TEMP_DIR AIL_TEXT("Semi1292Calibrated.mfo")

// Length of the string to read (null terminated).
#define STRING_LENGTH          13L
#define STRING_CALIBRATION     AIL_TEXT("M940902-MXD5")

int MosMain(void)
   {
   AIL_ID AilApplication,               // Application identifier.       
          AilSystem,                    // System identifier.            
          AilDisplay,                   // Display identifier.           
          AilImage,                     // Image buffer identifier.      
          AilSubImage,                  // Sub-image buffer identifier.  
          AilFontSubImage,              // Font display sub image.       
          AilOverlayImage,              // Overlay image.                
          OcrFont,                      // OCR font identifier.          
          OcrResult;                    // OCR result buffer identifier. 
   AIL_TEXT_CHAR String[STRING_LENGTH]; // Array of characters to read.  
   AIL_DOUBLE Score;                    // Reading score.                
   AIL_INT  SizeX, SizeY, Type;         // Source image dimensions.      

   MosPrintf(AIL_TEXT("\nOCR MODULE (SEMI font reading):\n"));
   MosPrintf(AIL_TEXT("-------------------------------\n\n"));

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Load and display the source image into a new image buffer.
   MbufAlloc2d(AilSystem, 
               MbufDiskInquire(CHAR_IMAGE_FILE, M_SIZE_X, &SizeX),
               MbufDiskInquire(CHAR_IMAGE_FILE, M_SIZE_Y, &SizeY)*3/2,
               MbufDiskInquire(CHAR_IMAGE_FILE, M_TYPE,   &Type),
               M_IMAGE+M_PROC+M_DISP,
               &AilImage);
   MbufClear(AilImage, 0);
   MbufLoad(CHAR_IMAGE_FILE, AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Restrict the region of the image where to read the string.
   MbufChild2d(AilImage, READ_REGION_POS_X, READ_REGION_POS_Y,
               READ_REGION_WIDTH, READ_REGION_HEIGHT, &AilSubImage);

   // Define the bottom of the image as the region where to copy the font representation.
   MbufChild2d(AilImage, 50, SizeY+10, SizeX-100, (SizeY/3)-10, &AilFontSubImage);

   // Restore the OCR character font from disk.
   MocrRestoreFont(FONT_FILE_IN, M_RESTORE, AilSystem, &OcrFont);

   // Show the font representation.
   MocrCopyFont(AilFontSubImage, OcrFont, M_COPY_FROM_FONT+M_ALL_CHAR, M_NULL );

   // Pause to show the original image.
   MosPrintf(AIL_TEXT("The SEMI string at the top will be read using the ")
                              AIL_TEXT("font displayed at the bottom.\n\n"));
   MosPrintf(AIL_TEXT("Calibrating SEMI font...\n\n"));

   // Calibrate the OCR font.
   MocrCalibrateFont(AilSubImage, OcrFont, STRING_CALIBRATION,
                     CHAR_SIZE_X_MIN, CHAR_SIZE_X_MAX, CHAR_SIZE_X_STEP,
                     CHAR_SIZE_Y_MIN, CHAR_SIZE_Y_MAX, CHAR_SIZE_Y_STEP,
                     M_DEFAULT);

   // Set the user-specific character constraints for each string position.
   MocrSetConstraint(OcrFont, 0,  M_LETTER, M_NULL);        // A to Z only
   MocrSetConstraint(OcrFont, 1,  M_DIGIT,  AIL_TEXT("9")); // 9      only
   MocrSetConstraint(OcrFont, 2,  M_DIGIT,  M_NULL);        // 0 to 9 only 
   MocrSetConstraint(OcrFont, 3,  M_DIGIT,  M_NULL);        // 0 to 9 only 
   MocrSetConstraint(OcrFont, 4,  M_DIGIT,  M_NULL);        // 0 to 9 only 
   MocrSetConstraint(OcrFont, 5,  M_DIGIT,  M_NULL);        // 0 to 9 only 
   MocrSetConstraint(OcrFont, 6,  M_DIGIT,  M_NULL);        // 0 to 9 only 
   MocrSetConstraint(OcrFont, 7,  M_DEFAULT,AIL_TEXT("-")); // -      only
   MocrSetConstraint(OcrFont, 8,  M_LETTER, AIL_TEXT("M")); // M      only
   MocrSetConstraint(OcrFont, 9,  M_LETTER, AIL_TEXT("X")); // X      only
   MocrSetConstraint(OcrFont, 10, M_LETTER, AIL_TEXT("ABCDEFGH")); // SEMI checksum
   MocrSetConstraint(OcrFont, 11, M_DIGIT,  AIL_TEXT("01234567")); // SEMI checksum 

   // Pause before the read operation.
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate an OCR result buffer.
   MocrAllocResult(AilSystem, M_DEFAULT, &OcrResult);

   // Read the string.
   MocrReadString(AilSubImage, OcrFont, OcrResult);

   // Get the string and its reading score.
   MocrGetResult(OcrResult, M_STRING, String);
   MocrGetResult(OcrResult, M_STRING_SCORE,  &Score);

   // Print the result.
   MosPrintf(AIL_TEXT("\nThe string read is: \"%s\" (score: %.1f%%).\n\n"), String, Score);

   // Draw the string in the overlay under the reading region.
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);
   MgraFont(M_DEFAULT, M_FONT_DEFAULT_LARGE);
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_YELLOW);
   MgraText(M_DEFAULT, AilOverlayImage, READ_REGION_POS_X+(READ_REGION_WIDTH/4),
                                        READ_REGION_POS_Y+READ_REGION_HEIGHT+50, String);

   // Save the calibrated font if the reading score was sufficiently high.
   if (Score > READ_SCORE_MIN)
      {
      MocrSaveFont(FONT_FILE_OUT, M_SAVE, OcrFont);
      MosPrintf(AIL_TEXT("Read successful, calibrated OCR font was saved to disk.\n"));
      }
   else 
      {
      MosPrintf(AIL_TEXT("Error: Read score too low, calibrated OCR font not saved.\n"));
      }
   MosPrintf(AIL_TEXT("Press any key to end.\n\n\n"));
   MosGetch();

   // Clear the overlay.
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);

   // Free all allocations.
   MocrFree(OcrFont);
   MocrFree(OcrResult);
   MbufFree(AilSubImage);
   MbufFree(AilFontSubImage);
   MbufFree(AilImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}


```
