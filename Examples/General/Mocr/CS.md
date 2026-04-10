---
title: "Mocr"
description: "This program uses the OCR module to calibrate a predefined OCR font (SEMI font) and to read a string in the image. The string read is then printed to the screen and the calibrated font is saved to disk."
ms.language: "C#"
---

# Mocr

> This program uses the OCR module to calibrate a predefined OCR font (SEMI font) and to read a string in the image. The string read is then printed to the screen and the calibrated font is saved to disk.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufChild2d`, `MbufClear`, `MbufDiskInquire`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraFont`, `MgraText`, `MocrAllocResult`, `MocrCalibrateFont`, `MocrCopyFont`, `MocrFree`, `MocrGetResult`, `MocrReadString`, `MocrRestoreFont`, `MocrSaveFont`, `MocrSetConstraint`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Semiconductor, Applications, Reading, Modules, Buffer, Display, Graphics, OCR, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MOcr.cs
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

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MOcr
{
    class Program
    {
        // *****************************************************************************
        // OCR SEMI font read example.
        // *****************************************************************************

        // Target image character specifications.
        private const string CHAR_IMAGE_FILE = AIL.M_IMAGE_PATH + "OcrSemi1292.mim";
        private const double CHAR_SIZE_X_MIN = 22.0;
        private const double CHAR_SIZE_X_MAX = 23.0;
        private const double CHAR_SIZE_X_STEP = 0.50;
        private const double CHAR_SIZE_Y_MIN = 43.0;
        private const double CHAR_SIZE_Y_MAX = 44.0;
        private const double CHAR_SIZE_Y_STEP = 0.50;

        // Target reading specifications.
        private const int READ_REGION_POS_X = 30;
        private const int READ_REGION_POS_Y = 40;
        private const int READ_REGION_WIDTH = 420;
        private const int READ_REGION_HEIGHT = 70;
        private const double READ_SCORE_MIN = 50.0;

        // Font file names. 
        private const string FONT_FILE_IN = AIL.M_CONTEXT_PATH + "SEMI_M12-92.mfo";
        private const string FONT_FILE_OUT = AIL.M_TEMP_DIR + "Semi1292Calibrated.mfo";

        // Length of the string to read (null terminated).
        private const int STRING_LENGTH = 13;
        private const string STRING_CALIBRATION = "M940902-MXD5";

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;               // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                    // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                   // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;                     // Image buffer identifier.
            AIL_ID AilSubImage = AIL.M_NULL;                  // Sub-image buffer identifier.
            AIL_ID AilFontSubImage = AIL.M_NULL;              // Font display sub image.
            AIL_ID AilOverlayImage = AIL.M_NULL;              // Overlay image.
            AIL_ID OcrFont = AIL.M_NULL;                      // OCR font identifier.
            AIL_ID OcrResult = AIL.M_NULL;                    // OCR result buffer identifier.
            double Score = 0;                                 // Reading score.
            AIL_INT SizeX = 0;                                // Source image size x.
            AIL_INT SizeY = 0;                                // Source image size y.
            AIL_INT Type = 0;                                 // Source image type.

            StringBuilder ReadString = new StringBuilder(STRING_LENGTH); // Characters to read.

            Console.Write("\nOCR MODULE (SEMI font reading):\n");
            Console.Write("-------------------------------\n\n");

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Load and display the source image into a new image buffer.
            AIL.MbufAlloc2d(AilSystem,
                        AIL.MbufDiskInquire(CHAR_IMAGE_FILE, AIL.M_SIZE_X, ref SizeX),
                        AIL.MbufDiskInquire(CHAR_IMAGE_FILE, AIL.M_SIZE_Y, ref SizeY) * 3 / 2,
                        AIL.MbufDiskInquire(CHAR_IMAGE_FILE, AIL.M_TYPE, ref Type),
                        AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP,
                        ref AilImage);
            AIL.MbufClear(AilImage, 0);
            AIL.MbufLoad(CHAR_IMAGE_FILE, AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Restrict the region of the image where to read the string.
            AIL.MbufChild2d(AilImage, READ_REGION_POS_X, READ_REGION_POS_Y,
                        READ_REGION_WIDTH, READ_REGION_HEIGHT, ref AilSubImage);

            // Define the bottom of the image as the region where to copy the font representation.
            AIL.MbufChild2d(AilImage, 50, SizeY + 10, SizeX - 100, (SizeY / 3) - 10, ref AilFontSubImage);


            // Restore the OCR character font from disk.
            AIL.MocrRestoreFont(FONT_FILE_IN, AIL.M_RESTORE, AilSystem, ref OcrFont);

            /* Show the font representation. */
            AIL.MocrCopyFont(AilFontSubImage, OcrFont, AIL.M_COPY_FROM_FONT + AIL.M_ALL_CHAR, "");

            // Pause to show the original image.
            Console.Write("The SEMI string at the top will be read using the font displayed at the bottom.\n\n");
            Console.Write("Calibrating SEMI font...\n\n");

            // Calibrate the OCR font.
            AIL.MocrCalibrateFont(AilSubImage, OcrFont, STRING_CALIBRATION,
                              CHAR_SIZE_X_MIN, CHAR_SIZE_X_MAX, CHAR_SIZE_X_STEP,
                              CHAR_SIZE_Y_MIN, CHAR_SIZE_Y_MAX, CHAR_SIZE_Y_STEP,
                              AIL.M_DEFAULT);

            // Set the user-specific character constraints for each string position.
            AIL.MocrSetConstraint(OcrFont, 0, AIL.M_LETTER);                // A to Z only
            AIL.MocrSetConstraint(OcrFont, 1, AIL.M_DIGIT, "9");            // 9      only
            AIL.MocrSetConstraint(OcrFont, 2, AIL.M_DIGIT);                 // 0 to 9 only
            AIL.MocrSetConstraint(OcrFont, 3, AIL.M_DIGIT);                 // 0 to 9 only
            AIL.MocrSetConstraint(OcrFont, 4, AIL.M_DIGIT);                 // 0 to 9 only
            AIL.MocrSetConstraint(OcrFont, 5, AIL.M_DIGIT);                 // 0 to 9 only
            AIL.MocrSetConstraint(OcrFont, 6, AIL.M_DIGIT);                 // 0 to 9 only
            AIL.MocrSetConstraint(OcrFont, 7, AIL.M_DEFAULT, "-");          // -      only
            AIL.MocrSetConstraint(OcrFont, 8, AIL.M_LETTER, "M");           // M      only
            AIL.MocrSetConstraint(OcrFont, 9, AIL.M_LETTER, "X");           // X      only
            AIL.MocrSetConstraint(OcrFont, 10, AIL.M_LETTER, "ABCDEFGH");   // SEMI checksum
            AIL.MocrSetConstraint(OcrFont, 11, AIL.M_DIGIT, "01234567");    // SEMI checksum

            // Pause before the read operation.
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Allocate an OCR result buffer.
            AIL.MocrAllocResult(AilSystem, AIL.M_DEFAULT, ref OcrResult);

            // Read the string.
            AIL.MocrReadString(AilSubImage, OcrFont, OcrResult);

            // Get the string and its reading score.
            AIL.MocrGetResult(OcrResult, AIL.M_STRING, ReadString);
            AIL.MocrGetResult(OcrResult, AIL.M_STRING_SCORE, ref Score);

            // Print the result.
            Console.Write("\nThe string read is: \"{0}\" (score: {1:P1}).\n\n", ReadString.ToString(), Score / 100);

            // Draw the string in the overlay under the reading region.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);
            AIL.MgraFont(AIL.M_DEFAULT, AIL.M_FONT_DEFAULT_LARGE);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_YELLOW);
            AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage,
                READ_REGION_POS_X + (READ_REGION_WIDTH / 4),
                READ_REGION_POS_Y + READ_REGION_HEIGHT + 50,
                ReadString.ToString());

            // Save the calibrated font if the reading score was sufficiently high.
            if (Score > READ_SCORE_MIN)
            {
                AIL.MocrSaveFont(FONT_FILE_OUT, AIL.M_SAVE, OcrFont);
                Console.Write("Read successful, calibrated OCR font was saved to disk.\n");
            }
            else
            {
                Console.Write("Error: Read score too low, calibrated OCR font not saved.\n");
            }
            Console.Write("Press any key to end.\n\n\n");
            Console.ReadKey(true);

            // Clear the overlay.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT);

            // Free all allocations.
            AIL.MocrFree(OcrFont);
            AIL.MocrFree(OcrResult);
            AIL.MbufFree(AilSubImage);
            AIL.MbufFree(AilFontSubImage);
            AIL.MbufFree(AilImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
