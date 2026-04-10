---
title: "Mdmr"
description: "This program uses the SureDotOCR® module to read a product expiry date and lot number printed using a CIJ printer."
ms.language: "C#"
---

# Mdmr

> This program uses the SureDotOCR® module to read a product expiry date and lot number printed using a CIJ printer.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MdmrAlloc`, `MdmrAllocResult`, `MdmrControl`, `MdmrControlStringModel`, `MdmrDraw`, `MdmrFree`, `MdmrGetResult`, `MdmrImportFont`, `MdmrPreprocess`, `MdmrRead`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Packaging, Applications, Reading, Modules, Buffer, Display, Graphics, SureDotOCR®, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mdmr.cs
//
// Description: This program uses the Dot Matrix Reader (SureDotOCR®) module to
//              read a product expiry date and lot number printed using a
//              CIJ printer.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Text;
using Zebra.AuroraImagingLibrary;

namespace Mdmr
{
    class Program
    {
        // File specifications.
        const string IMAGE_FILE_TO_READ = AIL.M_IMAGE_PATH + "ExpiryDateAndLot.mim";
        const string FONT_FILE_TO_IMPORT = AIL.M_CONTEXT_PATH + "ExpiryDateAndLotFont5x7.mdmrf";

        // Dot Matrix Reader settings.
        const double STRING_DOT_DIAMETER = 6.0;
        const double TEXT_BLOCK_WIDTH = 400;
        const double TEXT_BLOCK_HEIGHT = 100;
        static readonly AIL_INT EXPIRY_DATE_LENGTH = 7;
        static readonly AIL_INT LOT_NUMBER_LENGTH = 7;

        // Util.
        const int TEXT_MAX_SIZE = 128;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;                                 // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                                      // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                                     // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;                                       // Image buffer identifier.
            AIL_ID AilOverlay = AIL.M_NULL;                                     // Overlay image.
            AIL_ID AilDmrContext = AIL.M_NULL;                                  // Dot Matrix context identifier.
            AIL_ID AilDmrResult = AIL.M_NULL;                                   // Dot Matrix result identifier.

            AIL_INT NumberOfStrings = 0;                                        // Total number of strings to read.
            AIL_INT StringSize = 0;                                             // Number of strings characters.
            AIL_INT StringModelIndex = 0;                                       // String model index.
            AIL_INT Index = 0;                                                  // Result index.

            StringBuilder StringResult = new StringBuilder(TEXT_MAX_SIZE + 1);  // String of characters read.
            string PrintText;                                                   // Util text.

            // Print module name.
            Console.Write("\nDOT MATRIX READER (SureDotOCR) MODULE:\n");
            Console.Write("--------------------------------------\n\n");

            // Allocate defaults
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore the font definition image
            AIL.MbufRestore(IMAGE_FILE_TO_READ, AilSystem, ref AilImage);

            // Display the image and prepare for overlay annotations.
            AIL.MdispSelect(AilDisplay, AilImage);
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlay);

            // Allocate a new empty Dot Matrix Reader context.
            AIL.MdmrAlloc(AilSystem, AIL.M_DOT_MATRIX, AIL.M_DEFAULT, ref AilDmrContext);

            // Allocate a new empty Dot Matrix Reader result buffer.
            AIL.MdmrAllocResult(AilSystem, AIL.M_DOT_MATRIX, AIL.M_DEFAULT, ref AilDmrResult);

            // Import a Dot Matrix font.
            AIL.MdmrImportFont(FONT_FILE_TO_IMPORT, AIL.M_DMR_FONT_FILE, AilDmrContext, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_DEFAULT);

            // Add a new string model definition for the product lot number.
            // -------------------------------------------------------------
            AIL.MdmrControl(AilDmrContext, AIL.M_STRING_ADD, AIL.M_DEFAULT);

            // Set the string model rank and size
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(0), AIL.M_DEFAULT, AIL.M_STRING_RANK, 1, AIL.M_DEFAULT, AIL.M_NULL);
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(0), AIL.M_DEFAULT, AIL.M_STRING_SIZE_MIN_MAX, LOT_NUMBER_LENGTH, LOT_NUMBER_LENGTH, AIL.M_NULL);

            // Add a new string model definition for the expiry date (YYYY MM DD).
            // -------------------------------------------------------------------
            AIL.MdmrControl(AilDmrContext, AIL.M_STRING_ADD, AIL.M_DEFAULT);

            // Set the string model rank and size
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_DEFAULT, AIL.M_STRING_RANK, 0, AIL.M_DEFAULT, AIL.M_NULL);
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_DEFAULT, AIL.M_STRING_SIZE_MIN_MAX, EXPIRY_DATE_LENGTH, EXPIRY_DATE_LENGTH, AIL.M_NULL);

            // Set the string model constraint for an expiry date (DDMMMYY).
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(0), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_DIGITS, AIL.M_NULL);
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(1), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_DIGITS, AIL.M_NULL);
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(2), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_LETTERS_UPPERCASE, AIL.M_NULL);
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(3), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_LETTERS_UPPERCASE, AIL.M_NULL);
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(4), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_LETTERS_UPPERCASE, AIL.M_NULL);
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(5), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_DIGITS, AIL.M_NULL);
            AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(6), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_DIGITS, AIL.M_NULL);

            Console.Write("A Dot Matrix Reader (SureDotOCR) context was set up to read\n" +
                          "an expiry date and a lot number from a target image.\n\n");

            // Set the Dot Matrix dot diameter.
            AIL.MdmrControl(AilDmrContext, AIL.M_DOT_DIAMETER, STRING_DOT_DIAMETER);

            // Set the maximum size of the string box.
            AIL.MdmrControl(AilDmrContext, AIL.M_TEXT_BLOCK_WIDTH, TEXT_BLOCK_WIDTH);
            AIL.MdmrControl(AilDmrContext, AIL.M_TEXT_BLOCK_HEIGHT, TEXT_BLOCK_HEIGHT);

            // Preprocess the context.
            AIL.MdmrPreprocess(AilDmrContext, AIL.M_DEFAULT);

            // Reading the string from a target image.
            AIL.MdmrRead(AilDmrContext, AilImage, AilDmrResult, AIL.M_DEFAULT);

            // Get number of strings read and show the result.
            AIL.MdmrGetResult(AilDmrResult, AIL.M_GENERAL, AIL.M_DEFAULT, AIL.M_STRING_NUMBER + AIL.M_TYPE_AIL_INT, ref NumberOfStrings);

            // Draw the read result.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN);
            AIL.MdmrDraw(AIL.M_DEFAULT, AilDmrResult, AilOverlay, AIL.M_DRAW_STRING_CHAR_BOX, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_CYAN);
            AIL.MdmrDraw(AIL.M_DEFAULT, AilDmrResult, AilOverlay, AIL.M_DRAW_STRING_CHAR_POSITION, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            if (NumberOfStrings > 0)
            {
                Console.Write("Result: {0} strings have been read:\n\n", NumberOfStrings);

                for (Index = 0; Index < NumberOfStrings; Index++)
                {
                    // Print the read result.
                    AIL.MdmrGetResult(AilDmrResult, Index, AIL.M_GENERAL, AIL.M_STRING_MODEL_INDEX + AIL.M_TYPE_AIL_INT, ref StringModelIndex);
                    AIL.MdmrGetResult(AilDmrResult, Index, AIL.M_GENERAL, AIL.M_STRING + AIL.M_STRING_SIZE + AIL.M_TYPE_AIL_INT, ref StringSize);
                    AIL.MdmrGetResult(AilDmrResult, Index, AIL.M_GENERAL, AIL.M_STRING, StringResult);

                    switch ((int)StringModelIndex)
                    {
                        case 0:
                            PrintText = string.Format(" LOT# : {0} ", StringResult);
                            AIL.MgraText(AIL.M_DEFAULT, AilOverlay, 20, 20 + Index * 20, PrintText);
                            Console.Write(" LOT# : {0}\n", StringResult);
                            break;

                        case 1:
                            PrintText = string.Format(" EXP. : {0} ", StringResult);
                            AIL.MgraText(AIL.M_DEFAULT, AilOverlay, 20, 20 + Index * 20, PrintText.ToString());
                            Console.Write(" EXPIRY DATE: {0}\n", StringResult);
                            break;

                        default:
                            Console.Write("Unexpected model index\n");
                            break;
                    }
                }
            }
            else
            {
                Console.Write("Error: no string found.\n");
            }

            // Pause to show results.
            Console.Write("\nPress any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MdmrFree(AilDmrContext);
            AIL.MdmrFree(AilDmrResult);
            AIL.MbufFree(AilImage);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
