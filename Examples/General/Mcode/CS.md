---
title: "Mcode"
description: "This program decodes a linear Code 39 and a DataMatrix code."
ms.language: "C#"
---

# Mcode

> This program decodes a linear Code 39 and a DataMatrix code.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufChild2d`, `MbufFree`, `MbufRestore`, `McodeAlloc`, `McodeAllocResult`, `McodeControl`, `McodeFree`, `McodeGetResult`, `McodeModel`, `McodeRead`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraRect`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Applications, Identifying, Modules, Buffer, Display, Graphics, Code Reader, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mcode.cs
//
// Description: This program decodes a 1D Code 39 linear Barcode and a 2D DataMatrix code.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;
using Zebra.AuroraImagingLibrary;

namespace MCode
{
    class Program
    {
        static void Main(string[] args)
        {
            const string IMAGE_FILE = AIL.M_IMAGE_PATH + "Code.mim";
            const int STRING_LENGTH_MAX = 64;

            // Regions around 1D code.
            const int BARCODE_REGION_TOP_LEFT_X = 256;
            const int BARCODE_REGION_TOP_LEFT_Y = 80;
            const int BARCODE_REGION_SIZE_X = 290;
            const int BARCODE_REGION_SIZE_Y = 60;

            // Regions around 2D code.
            const int DATAMATRIX_REGION_TOP_LEFT_X = 8;
            const int DATAMATRIX_REGION_TOP_LEFT_Y = 312;
            const int DATAMATRIX_REGION_SIZE_X = 118;
            const int DATAMATRIX_REGION_SIZE_Y = 105;

            AIL_ID AilApplication = AIL.M_NULL;                     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                         // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;                           // Image buffer identifier.
            AIL_ID AilOverlayImage = AIL.M_NULL;                    // Image buffer identifier.
            AIL_ID DataMatrixRegion = AIL.M_NULL;                   // Child containing DataMatrix.
            AIL_ID DataMatrixCode = AIL.M_NULL;                     // DataMatrix 2D code identifier.
            AIL_ID BarCodeRegion = AIL.M_NULL;                      // Child containing Code39.
            AIL_ID Barcode = AIL.M_NULL;                            // Code39 barcode identifier.
            AIL_ID CodeResults = AIL.M_NULL;                        // Barcode results identifier.
            AIL_INT BarcodeStatus = AIL.M_NULL;                     // Decoding status.
            AIL_INT DataMatrixStatus = AIL.M_NULL;                  // Decoding status.

            double AnnotationColor = AIL.M_COLOR_GREEN;
            double AnnotationBackColor = AIL.M_COLOR_GRAY;
            int n = 0;
            StringBuilder BarcodeString = new StringBuilder(STRING_LENGTH_MAX); // Array of characters read.
            StringBuilder DataMatrixString = new StringBuilder(STRING_LENGTH_MAX); // Array of characters read.


            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore source image into an automatically allocated image buffer.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);

            // Display the image buffer.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Prepare for overlay annotations.
            AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE);
            AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID, ref AilOverlayImage);

            // Prepare bar code results buffer
            AIL.McodeAllocResult(AilSystem, AIL.M_DEFAULT, ref CodeResults);

            // Pause to show the original image.
            Console.Write("\n1D and 2D CODE READING:\n");
            Console.Write("-----------------------\n\n");
            Console.Write("This program will decode a linear Code 39 and a DataMatrix code.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);


            // 1D BARCODE READING:

            // Create a read region around the code to speedup reading.
            AIL.MbufChild2d(AilImage, BARCODE_REGION_TOP_LEFT_X, BARCODE_REGION_TOP_LEFT_Y, BARCODE_REGION_SIZE_X, BARCODE_REGION_SIZE_Y, ref BarCodeRegion);

            // Allocate CODE objects.
            AIL.McodeAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref Barcode);
            AIL.McodeModel(Barcode, AIL.M_ADD, AIL.M_CODE39, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_NULL);

            // Read codes from image.
            AIL.McodeRead(Barcode, BarCodeRegion, CodeResults);

            // Get decoding status.
            AIL.McodeGetResult(CodeResults, AIL.M_GENERAL, AIL.M_GENERAL, AIL.M_STATUS + AIL.M_TYPE_AIL_INT, ref BarcodeStatus);

            // Check if decoding was successful.
            if (BarcodeStatus == AIL.M_STATUS_READ_OK)
            {
                // Get decoded string.
                AIL.McodeGetResult(CodeResults, 0, AIL.M_GENERAL, AIL.M_STRING, BarcodeString);

                // Draw the decoded strings and read region in the overlay image.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKCOLOR, AnnotationBackColor);
                AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, BARCODE_REGION_TOP_LEFT_X + 10, BARCODE_REGION_TOP_LEFT_Y + 80, " 1D linear 39 bar code:           ");
                AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, BARCODE_REGION_TOP_LEFT_X + 200, BARCODE_REGION_TOP_LEFT_Y + 80, "\"" + BarcodeString.ToString() + "\"");
                AIL.MgraRect(AIL.M_DEFAULT, AilOverlayImage, BARCODE_REGION_TOP_LEFT_X, BARCODE_REGION_TOP_LEFT_Y, BARCODE_REGION_TOP_LEFT_X + BARCODE_REGION_SIZE_X, BARCODE_REGION_TOP_LEFT_Y + BARCODE_REGION_SIZE_Y);
            }

            // Free objects.
            AIL.McodeFree(Barcode);
            AIL.MbufFree(BarCodeRegion);

            // 2D CODE READING:

            // Create a read region around the code to speedup reading.
            AIL.MbufChild2d(AilImage, DATAMATRIX_REGION_TOP_LEFT_X, DATAMATRIX_REGION_TOP_LEFT_Y, DATAMATRIX_REGION_SIZE_X, DATAMATRIX_REGION_SIZE_Y, ref DataMatrixRegion);

            // Allocate CODE objects.
            AIL.McodeAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref DataMatrixCode);
            AIL.McodeModel(DataMatrixCode, AIL.M_ADD, AIL.M_DATAMATRIX, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_NULL);


            // Set the foreground value for the DataMatrix since it is different from the default value.
            AIL.McodeControl(DataMatrixCode, AIL.M_FOREGROUND_VALUE, AIL.M_FOREGROUND_WHITE);

            // Read codes from image.
            AIL.McodeRead(DataMatrixCode, DataMatrixRegion, CodeResults);

            // Get decoding status.
            AIL.McodeGetResult(CodeResults, AIL.M_GENERAL, AIL.M_GENERAL, AIL.M_STATUS + AIL.M_TYPE_AIL_INT, ref DataMatrixStatus);

            // Check if decoding was successful.
            if (DataMatrixStatus == AIL.M_STATUS_READ_OK)
            {
                // Get decoded string.
                AIL.McodeGetResult(CodeResults, 0, AIL.M_GENERAL, AIL.M_STRING, DataMatrixString);

                // Draw the decoded strings and read region in the overlay image.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AnnotationColor);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKCOLOR, AnnotationBackColor);
                for (n = 0; n < DataMatrixString.Length; n++) // Replace non printable characters with space.*/
                {
                    if ((DataMatrixString[n] < '0') || (DataMatrixString[n] > 'Z'))
                    {
                        DataMatrixString[n] = ' ';
                    }
                }
                AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, DATAMATRIX_REGION_TOP_LEFT_X, DATAMATRIX_REGION_TOP_LEFT_Y + 120, " 2D Data Matrix code:                  ");
                AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, DATAMATRIX_REGION_TOP_LEFT_X + 200, DATAMATRIX_REGION_TOP_LEFT_Y + 120, "\"" + DataMatrixString.ToString() + "\" ");
                AIL.MgraRect(AIL.M_DEFAULT, AilOverlayImage, DATAMATRIX_REGION_TOP_LEFT_X, DATAMATRIX_REGION_TOP_LEFT_Y, DATAMATRIX_REGION_TOP_LEFT_X + DATAMATRIX_REGION_SIZE_X, DATAMATRIX_REGION_TOP_LEFT_Y + DATAMATRIX_REGION_SIZE_Y);
            }

            // Free objects.
            AIL.McodeFree(DataMatrixCode);
            AIL.MbufFree(DataMatrixRegion);

            // Free results buffer.
            AIL.McodeFree(CodeResults);

            // Pause to show the results.
            if ((DataMatrixStatus == AIL.M_STATUS_READ_OK) && (BarcodeStatus == AIL.M_STATUS_READ_OK))
            {
                Console.Write("Decoding was successful and the strings were written under each code.\n");
            }
            else
            {
                Console.Write("Decoding error found.\n");
            }
            Console.Write("Press any key to end.\n");
            Console.ReadKey(true);

            // Free other allocations.
            AIL.MbufFree(AilImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
