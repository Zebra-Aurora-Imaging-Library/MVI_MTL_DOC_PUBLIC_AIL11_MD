---
title: "MbufColor"
description: "This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result."
ms.language: "C#"
---

# MbufColor

> This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result.

**Language:** C#

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufChild2d`, `MbufChildColor`, `MbufClear`, `MbufDiskInquire`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispSelect`, `MgraText`, `MimArith`, `MimConvert`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MbufColor.cs
//
// Description: This program demonstrates color buffer manipulations. It allocates
//              a displayable color image buffer, displays it, and loads a color
//              image into the left part. After that, color annotations are done
//              in each band of the color buffer. Finaly, to increase the image
//              luminance, the image is converted to Hue, Luminance and Saturation
//              (HLS), a certain offset is added to the luminance component and
//              the image is converted back to Red, Green, Blue (RGB) into the
//              right part to display the result.
//             
//              The example also demonstrates how to display multiple images
//              in a single display using child buffers.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
//////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MBufColor
{
    class Program
    {
        // Source image file specifications.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "Bird.mim";

        // Luminance offset to add to the image.
        private const int IMAGE_LUMINANCE_OFFSET = 40;

        // Main function.
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;             // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                  // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                 // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;                   // Image buffer identifier.
            AIL_ID AilLeftSubImage = AIL.M_NULL;            // Sub-image buffer identifier for original image.
            AIL_ID AilRightSubImage = AIL.M_NULL;           // Sub-image buffer identifier for processed image.
            AIL_ID AilLumSubImage = AIL.M_NULL;             // Sub-image buffer identifier for luminance.
            AIL_ID AilRedBandSubImage = AIL.M_NULL;         // Sub-image buffer identifier for red component.
            AIL_ID AilGreenBandSubImage = AIL.M_NULL;       // Sub-image buffer identifier for green component.
            AIL_ID AilBlueBandSubImage = AIL.M_NULL;        // Sub-image buffer identifier for blue component.
            AIL_INT SizeX = 0;
            AIL_INT SizeY = 0;
            AIL_INT SizeBand = 0;
            AIL_INT Type = 0;

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Allocate a color display buffer twice the size of the source image and display it.
            AIL.MbufAllocColor(AilSystem,
                           AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BAND, ref SizeBand),
                           AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_X, ref SizeX) * 2,
                           AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_Y, ref SizeY),
                           AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_TYPE, ref Type),
                           AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC, ref AilImage);
            AIL.MbufClear(AilImage, 0);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Define 2 child buffers that maps to the left and right part of the display 
            // buffer, to put the source and destination color images.
            //
            AIL.MbufChild2d(AilImage, 0, 0, SizeX, SizeY, ref AilLeftSubImage);
            AIL.MbufChild2d(AilImage, SizeX, 0, SizeX, SizeY, ref AilRightSubImage);

            // Load the color source image on the left.
            AIL.MbufLoad(IMAGE_FILE, AilLeftSubImage);

            // Define child buffers that map to the red, green and blue components
            // of the source image.
            //
            AIL.MbufChildColor(AilLeftSubImage, AIL.M_RED, ref AilRedBandSubImage);
            AIL.MbufChildColor(AilLeftSubImage, AIL.M_GREEN, ref AilGreenBandSubImage);
            AIL.MbufChildColor(AilLeftSubImage, AIL.M_BLUE, ref AilBlueBandSubImage);

            // Write color text annotations to show access in each individual band of the image.
            //
            // Note that this is typically done more simply by using:
            //  AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_RGB(0xFF,0x90,0x00));
            //  AIL.MgraText(AIL.M_DEFAULT, AilLeftSubImage, ...);

            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xFF);
            AIL.MgraText(AIL.M_DEFAULT, AilRedBandSubImage, SizeX / 16, SizeY / 8, " TOUCAN ");
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0x90);
            AIL.MgraText(AIL.M_DEFAULT, AilGreenBandSubImage, SizeX / 16, SizeY / 8, " TOUCAN ");
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0x00);
            AIL.MgraText(AIL.M_DEFAULT, AilBlueBandSubImage, SizeX / 16, SizeY / 8, " TOUCAN ");

            // Print a message.
            Console.Write("\nCOLOR OPERATIONS:\n");
            Console.Write("-----------------\n\n");
            Console.Write("A color source image was loaded on the left and color text\n");
            Console.Write("annotations were written in it.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Convert image to Hue, Luminance, Saturation color space (HSL).
            AIL.MimConvert(AilLeftSubImage, AilRightSubImage, AIL.M_RGB_TO_HSL);

            // Create a child buffer that maps to the luminance component.
            AIL.MbufChildColor(AilRightSubImage, AIL.M_LUMINANCE, ref AilLumSubImage);

            // Add an offset to the luminance component.
            AIL.MimArith(AilLumSubImage, IMAGE_LUMINANCE_OFFSET, AilLumSubImage, AIL.M_ADD_CONST + AIL.M_SATURATION);

            // Convert image back to Red, Green, Blue color space (RGB) for display.
            AIL.MimConvert(AilRightSubImage, AilRightSubImage, AIL.M_HSL_TO_RGB);

            // Print a message.
            Console.Write("Luminance was increased using color image processing.\n");

            // Print a message.
            Console.Write("Press any key to end.\n");
            Console.ReadKey(true);

            // Release sub-images and color image buffer.
            AIL.MbufFree(AilLumSubImage);
            AIL.MbufFree(AilRedBandSubImage);
            AIL.MbufFree(AilGreenBandSubImage);
            AIL.MbufFree(AilBlueBandSubImage);
            AIL.MbufFree(AilRightSubImage);
            AIL.MbufFree(AilLeftSubImage);
            AIL.MbufFree(AilImage);

            // Release defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
