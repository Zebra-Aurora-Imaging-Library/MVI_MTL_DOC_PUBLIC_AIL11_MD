---
title: "Mstr"
description: "This program uses the String Reader module to define a font from an image containing a mosaic of Quebec license plates. Two String Models are then defined and parameterized to read only some valid Quebec license plates. A license plate reading is then performed in a target image of a car on the road."
ms.language: "C++"
---

# Mstr

> This program uses the String Reader module to define a font from an image containing a mosaic of Quebec license plates. Two String Models are then defined and parameterized to read only some valid Quebec license plates. A license plate reading is then performed in a target image of a car on the road.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufFree`, `MbufLoad`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MstrAlloc`, `MstrAllocResult`, `MstrControl`, `MstrDraw`, `MstrEditFont`, `MstrFree`, `MstrGetResult`, `MstrPreprocess`, `MstrRead`, `MstrSetConstraint`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Surveillance/ITS, Applications, ANPR/LPR, Modules, Buffer, Display, Graphics, String Reader, What's New, Older, AIL 10.0 U94

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MStr.cpp
//
// Description: This program uses the String Reader module to define a font 
//              from an image containing a mosaic of Quebec license plates.
//              Two String Models are then defined and parameterized to read 
//              only some valid Quebec license plates.
//              A license plate reading is then performed in a target image 
//              of a car on the road.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>  

// Image file specifications.
#define IMAGE_FILE_DEFINITION M_IMAGE_PATH AIL_TEXT("QcPlates.mim")
#define IMAGE_FILE_TO_READ    M_IMAGE_PATH AIL_TEXT("LicPlate.mim")

// String containing all characters used for font definition.
#define TEXT_DEFINITION       "AVS300CVK781JNK278 EBX380QKN918HCC839 "\
                              "YRH900ZQR756977AMQ GPK742389EYE569ESQ"

// Font normalization size Y.
#define NORMALIZATION_SIZE_Y    20L

// Max size of plate string.
#define STRING_MAX_SIZE         32L


//*****************************************************************************
// Main.
int MosMain(void)
   { 
   AIL_ID AilApplication,                          // Application identifier.         
          AilSystem,                               // System identifier.              
          AilDisplay,                              // Display identifier.             
          AilImage,                                // Image buffer identifier.        
          AilOverlayImage,                         // Overlay image.                  
          AilStrContext,                           // String context identifier.      
          AilStrResult;                            // String result buffer identifier.
   AIL_INT NumberOfStringRead;                     // Total number of strings to read.
   AIL_DOUBLE Score;                               // String score.                   
   AIL_TEXT_CHAR StringResult[STRING_MAX_SIZE+1];  // String of characters read.      
   AIL_DOUBLE Time = 0.0;                          // Time variable.                    

   // Print module name.
   MosPrintf(AIL_TEXT("\nSTRING READER MODULE:\n"));
   MosPrintf(AIL_TEXT("---------------------\n\n"));
 
   // Allocate defaults
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Restore the font definition image
   MbufRestore(IMAGE_FILE_DEFINITION, AilSystem, &AilImage);

   // Display the image and prepare for overlay annotations.
   MdispSelect(AilDisplay, AilImage);
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);

   // Allocate a new empty String Reader context.
   MstrAlloc( AilSystem, M_FONT_BASED, M_DEFAULT, &AilStrContext);

   // Allocate a new empty String Reader result buffer.
   MstrAllocResult(AilSystem, M_DEFAULT, &AilStrResult);

   // Add a new empty user defined font to the context.
   MstrControl(AilStrContext, M_CONTEXT, M_FONT_ADD, M_USER_DEFINED);

   // Add user-defined characters from the license plate mosaic image. 
   // Note that the string that identifies the character values to associate with the 
   // character representations (TEXT_DEFINITION) includes repeated character values 
   // (e.g. 'E'); each instance of a repeated character value causes the character 
   // representation associated with this value to be overwritten.
   MstrEditFont(AilStrContext, M_FONT_INDEX(0), M_CHAR_ADD, 
                M_USER_DEFINED + M_FOREGROUND_BLACK, 
                AilImage, (void *)TEXT_DEFINITION, M_NULL);

   // Draw all the characters in the font in the overlay image. 
   // Note that since M_ORIGINAL is specified, characters are drawn at the location 
   // from which they were originally extracted from the license plate mosaic image.
   // This allows you to see which character representation was used for a repeated 
   // character value in TEXT_DEFINITION.
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);
   MstrDraw(M_DEFAULT, AilStrContext, AilOverlayImage, M_DRAW_CHAR, 
            M_FONT_INDEX(0), M_NULL, M_ORIGINAL);

   // Normalize the characters of the font to an appropriate size.
   MstrEditFont(AilStrContext, M_FONT_INDEX(0), M_CHAR_NORMALIZE,
                M_SIZE_Y, NORMALIZATION_SIZE_Y, M_NULL, M_NULL);

   // Add 2 new empty strings models to the context for the 2 valid types of 
   // plates (3 letters followed by 3 numbers or 3 numbers followed by 3 letters)
   // Note that the read time increases with the number of strings in the context.
   MstrControl(AilStrContext, M_CONTEXT, M_STRING_ADD, M_USER_DEFINED);
   MstrControl(AilStrContext, M_CONTEXT, M_STRING_ADD, M_USER_DEFINED);

   // Set number of strings to read.
   MstrControl(AilStrContext, M_CONTEXT, M_STRING_NUMBER, 1);

   // Set number of expected characters for all string models to 6 characters.
   MstrControl(AilStrContext, M_STRING_INDEX(M_ALL), M_STRING_SIZE_MIN, 6);
   MstrControl(AilStrContext, M_STRING_INDEX(M_ALL), M_STRING_SIZE_MAX, 6);

   // Set default constraint to uppercase letter for all string models
   MstrSetConstraint(AilStrContext, M_STRING_INDEX(0), M_DEFAULT, 
                                            M_LETTER + M_UPPERCASE, M_NULL );
   MstrSetConstraint(AilStrContext, M_STRING_INDEX(1), M_DEFAULT, 
                                            M_LETTER + M_UPPERCASE, M_NULL );
    
   // Set constraint of 3 last characters to digit for the first string model
   MstrSetConstraint(AilStrContext, M_STRING_INDEX(0), 3, M_DIGIT, M_NULL );
   MstrSetConstraint(AilStrContext, M_STRING_INDEX(0), 4, M_DIGIT, M_NULL );
   MstrSetConstraint(AilStrContext, M_STRING_INDEX(0), 5, M_DIGIT, M_NULL );

   // Set constraint of 3 first characters to digit for the second string model
   MstrSetConstraint(AilStrContext, M_STRING_INDEX(1), 0, M_DIGIT, M_NULL );
   MstrSetConstraint(AilStrContext, M_STRING_INDEX(1), 1, M_DIGIT, M_NULL );
   MstrSetConstraint(AilStrContext, M_STRING_INDEX(1), 2, M_DIGIT, M_NULL );

   // Pause to show the font definition.
   MosPrintf(AIL_TEXT("This program has defined a font with this ")
                          AIL_TEXT("Quebec plates mosaic image.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Clear the display overlay.
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);

   // Load image to read into image buffer.
   MbufLoad(IMAGE_FILE_TO_READ, AilImage);

   // Preprocess the String Reader context.
   MstrPreprocess(AilStrContext, M_DEFAULT);

   // Dummy first call for bench measure purpose only (bench stabilization, 
   // cache effect, etc...). This first call is NOT required by the application.
   MstrRead(AilStrContext, AilImage, AilStrResult);

   // Reset the timer.
   MappTimer(M_DEFAULT, M_TIMER_RESET+M_SYNCHRONOUS, M_NULL);
   
   // Perform the read operation on the specified target image.
   MstrRead(AilStrContext, AilImage, AilStrResult);

   // Read the time.
   MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &Time);

   // Get number of strings read and show the result.
   MstrGetResult(AilStrResult, M_GENERAL, M_STRING_NUMBER + M_TYPE_AIL_INT, 
                                                             &NumberOfStringRead);
   if( NumberOfStringRead >= 1)
      {
      MosPrintf(AIL_TEXT("The license plate was read successfully (%.2lf ms)\n\n"), 
                                                                        Time*1000 );

      // Draw read result.
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_BLUE);
      MstrDraw(M_DEFAULT, AilStrResult, AilOverlayImage, M_DRAW_STRING, M_ALL, 
                                                                   M_NULL, M_DEFAULT); 
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);
      MstrDraw(M_DEFAULT, AilStrResult, AilOverlayImage, M_DRAW_STRING_BOX, M_ALL, 
                                                                    M_NULL, M_DEFAULT);

      // Print the read result.
      MosPrintf(AIL_TEXT(" String                  Score\n") );
      MosPrintf(AIL_TEXT(" -----------------------------------\n") );
      MstrGetResult(AilStrResult, 0, M_STRING+M_TYPE_TEXT_CHAR, StringResult);
      MstrGetResult(AilStrResult, 0, M_STRING_SCORE, &Score);
      MosPrintf(AIL_TEXT(" %s                  %.1lf\n"), StringResult, Score ); 
      }
   else
      {
      MosPrintf(AIL_TEXT("Error: Plate was not read.\n"));
      }

   // Pause to show results.
   MosPrintf(AIL_TEXT("\nPress any key to end.\n\n"));
   MosGetch();

   // Free all allocations.
   MstrFree(AilStrContext);
   MstrFree(AilStrResult);
   MbufFree(AilImage);
   
   // Free defaults.
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }




```
