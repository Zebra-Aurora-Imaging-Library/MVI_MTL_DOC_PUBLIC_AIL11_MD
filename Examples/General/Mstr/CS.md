---
title: "Mstr"
description: "This program uses the String Reader module to define a font from an image containing a mosaic of Quebec license plates. Two String Models are then defined and parameterized to read only some valid Quebec license plates. A license plate reading is then performed in a target image of a car on the road."
ms.language: "C#"
---

# Mstr

> This program uses the String Reader module to define a font from an image containing a mosaic of Quebec license plates. Two String Models are then defined and parameterized to read only some valid Quebec license plates. A license plate reading is then performed in a target image of a car on the road.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufFree`, `MbufLoad`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MstrAlloc`, `MstrAllocResult`, `MstrControl`, `MstrDraw`, `MstrEditFont`, `MstrFree`, `MstrGetResult`, `MstrPreprocess`, `MstrRead`, `MstrSetConstraint`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Surveillance/ITS, Applications, ANPR/LPR, Modules, Buffer, Display, Graphics, String Reader, What's New, Older, AIL 10.0 U94

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MStr.cs
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

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MStr
{
    class Program
    {
        // Image file specifications.
        private const string IMAGE_FILE_DEFINITION = AIL.M_IMAGE_PATH + "QcPlates.mim";
        private const string IMAGE_FILE_TO_READ = AIL.M_IMAGE_PATH + "LicPlate.mim";

        // String containing all characters used for font definition.
        private const string TEXT_DEFINITION = "AVS300CVK781JNK278 EBX380QKN918HCC839 YRH900ZQR756977AMQ GPK742389EYE569ESQ";

        // Font normalization size Y.
        private const int NORMALIZATION_SIZE_Y = 20;

        // Max size of plate string.
        private const int STRING_MAX_SIZE = 32;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;           // Image buffer identifier.
            AIL_ID AilOverlayImage = AIL.M_NULL;    // Overlay image.
            AIL_ID AilStrContext = AIL.M_NULL;      // String context identifier.
            AIL_ID AilStrResult = AIL.M_NULL;       // String result buffer identifier.
            AIL_INT NumberOfStringRead = 0;         // Total number of strings to read.
            double Score = 0.0;                     // String score.
            StringBuilder StringResult = new StringBuilder(STRING_MAX_SIZE + 1); // String of characters read.
            double Time = 0.0;                      // Time variable.

            // Print module name.
            Console.Write("\nSTRING READER MODULE:\n");
            Console.Write("---------------------\n\n");

            // Allocate defaults
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore the font definition image
            AIL.MbufRestore(IMAGE_FILE_DEFINITION, AilSystem, ref AilImage);

            // Display the image and prepare for overlay annotations.
            AIL.MdispSelect(AilDisplay, AilImage);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            // Allocate a new empty String Reader context.
            AIL.MstrAlloc(AilSystem, AIL.M_FONT_BASED, AIL.M_DEFAULT, ref AilStrContext);

            // Allocate a new empty String Reader result buffer.
            AIL.MstrAllocResult(AilSystem, AIL.M_DEFAULT, ref AilStrResult);

            AIL.MstrControl(AilStrContext, AIL.M_CONTEXT, AIL.M_ENCODING, AIL.M_UNICODE);

            // Add a new empty user defined font to the context.
            AIL.MstrControl(AilStrContext, AIL.M_CONTEXT, AIL.M_FONT_ADD, AIL.M_USER_DEFINED);

            // Add user-defined characters from the license plate mosaic image. 
            // Note that the string that identifies the character values to associate with the 
            // character representations (TEXT_DEFINITION) includes repeated character values 
            // (e.g. 'E'); each instance of a repeated character value causes the character 
            // representation associated with this value to be overwritten.
            AIL.MstrEditFont(AilStrContext, AIL.M_FONT_INDEX(0), AIL.M_CHAR_ADD, AIL.M_USER_DEFINED + AIL.M_FOREGROUND_BLACK, AilImage, TEXT_DEFINITION);

            // Draw all the characters in the font in the overlay image. 
            // Note that since M_ORIGINAL is specified, characters are drawn at the location 
            // from which they were originally extracted from the license plate mosaic image.
            // This allows you to see which character representation was used for a repeated 
            // character value in TEXT_DEFINITION.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN);
            AIL.MstrDraw(AIL.M_DEFAULT, AilStrContext, AilOverlayImage, AIL.M_DRAW_CHAR, AIL.M_FONT_INDEX(0), AIL.M_ORIGINAL);

            // Normalize the characters of the font to an appropriate size.
            AIL.MstrEditFont(AilStrContext, AIL.M_FONT_INDEX(0), AIL.M_CHAR_NORMALIZE, AIL.M_SIZE_Y, NORMALIZATION_SIZE_Y);

            // Add 2 new empty strings models to the context for the 2 valid types of 
            // plates (3 letters followed by 3 numbers or 3 numbers followed by 3 letters)
            // Note that the read time increases with the number of strings in the context.
            AIL.MstrControl(AilStrContext, AIL.M_CONTEXT, AIL.M_STRING_ADD, AIL.M_USER_DEFINED);
            AIL.MstrControl(AilStrContext, AIL.M_CONTEXT, AIL.M_STRING_ADD, AIL.M_USER_DEFINED);

            // Set number of strings to read.
            AIL.MstrControl(AilStrContext, AIL.M_CONTEXT, AIL.M_STRING_NUMBER, 1);

            // Set number of expected characters for all string models to 6 characters.
            AIL.MstrControl(AilStrContext, AIL.M_STRING_INDEX(AIL.M_ALL), AIL.M_STRING_SIZE_MIN, 6);
            AIL.MstrControl(AilStrContext, AIL.M_STRING_INDEX(AIL.M_ALL), AIL.M_STRING_SIZE_MAX, 6);

            // Set default constraint to uppercase letter for all string models
            AIL.MstrSetConstraint(AilStrContext, AIL.M_STRING_INDEX(0), AIL.M_DEFAULT, AIL.M_LETTER + AIL.M_UPPERCASE, IntPtr.Zero);
            AIL.MstrSetConstraint(AilStrContext, AIL.M_STRING_INDEX(1), AIL.M_DEFAULT, AIL.M_LETTER + AIL.M_UPPERCASE, IntPtr.Zero);

            // Set constraint of 3 last characters to digit for the first string model
            AIL.MstrSetConstraint(AilStrContext, AIL.M_STRING_INDEX(0), 3, AIL.M_DIGIT, IntPtr.Zero);
            AIL.MstrSetConstraint(AilStrContext, AIL.M_STRING_INDEX(0), 4, AIL.M_DIGIT, IntPtr.Zero);
            AIL.MstrSetConstraint(AilStrContext, AIL.M_STRING_INDEX(0), 5, AIL.M_DIGIT, IntPtr.Zero);

            // Set constraint of 3 first characters to digit for the second string model
            AIL.MstrSetConstraint(AilStrContext, AIL.M_STRING_INDEX(1), 0, AIL.M_DIGIT, IntPtr.Zero);
            AIL.MstrSetConstraint(AilStrContext, AIL.M_STRING_INDEX(1), 1, AIL.M_DIGIT, IntPtr.Zero);
            AIL.MstrSetConstraint(AilStrContext, AIL.M_STRING_INDEX(1), 2, AIL.M_DIGIT, IntPtr.Zero);

            // Pause to show the font definition.
            Console.Write("This program has defined a font with this Quebec plates mosaic image.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Clear the display overlay.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            // Load image to read into image buffer.
            AIL.MbufLoad(IMAGE_FILE_TO_READ, AilImage);

            // Preprocess the String Reader context.
            AIL.MstrPreprocess(AilStrContext, AIL.M_DEFAULT);

            // Dummy first call for bench measure purpose only (bench stabilization, cache effect, etc...). This first call is NOT required by the application.
            AIL.MstrRead(AilStrContext, AilImage, AilStrResult);

            // Reset the timer.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            // Perform the read operation on the specified target image.
            AIL.MstrRead(AilStrContext, AilImage, AilStrResult);

            // Read the time.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref Time);

            // Get number of strings read and show the result.
            AIL.MstrGetResult(AilStrResult, AIL.M_GENERAL, AIL.M_STRING_NUMBER + AIL.M_TYPE_AIL_INT, ref NumberOfStringRead);
            if (NumberOfStringRead >= 1)
            {
                Console.Write("The license plate was read successfully ({0:#.##} ms)\n\n", Time * 1000);

                // Draw read result.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_BLUE);
                AIL.MstrDraw(AIL.M_DEFAULT, AilStrResult, AilOverlayImage, AIL.M_DRAW_STRING, AIL.M_ALL, AIL.M_DEFAULT);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN);
                AIL.MstrDraw(AIL.M_DEFAULT, AilStrResult, AilOverlayImage, AIL.M_DRAW_STRING_BOX, AIL.M_ALL, AIL.M_DEFAULT);

                // Print the read result.
                Console.Write(" String                  Score\n");
                Console.Write(" -----------------------------------\n");
                AIL.MstrGetResult(AilStrResult, 0, AIL.M_STRING, StringResult);
                AIL.MstrGetResult(AilStrResult, 0, AIL.M_STRING_SCORE, ref Score);
                Console.Write(" {0}                  {1:0.0}\n", StringResult.ToString(), Score);
            }
            else
            {
                Console.Write("Error: Plate was not read.\n");
            }

            // Pause to show results.
            Console.Write("\nPress any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MstrFree(AilStrContext);
            AIL.MstrFree(AilStrResult);
            AIL.MbufFree(AilImage);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
