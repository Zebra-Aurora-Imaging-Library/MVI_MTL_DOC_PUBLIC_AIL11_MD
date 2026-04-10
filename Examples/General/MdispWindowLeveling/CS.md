---
title: "MdispWindowLeveling"
description: "This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling."
ms.language: "C#"
---

# MdispWindowLeveling

> This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling.

**Language:** C#

**Functions used:** `MbufControl`, `MappAlloc`, `MappFree`, `MbufAlloc1d`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispLut`, `MdispSelect`, `MdispSelectWindow`, `MgenLutRamp`, `MgraLine`, `MgraText`, `MimAllocResult`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdispWindowLeveling.cs
//
// Description: This program shows how to display a 10-bit monochrome Medical image
//              and applies a LUT to do interactive Window Leveling.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MDispWindowLeveling
{
    class Program
    {
        // Image file to load.
        private const string IMAGE_NAME = "ArmsMono10bit.mim";
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + IMAGE_NAME;

        // Draw the LUT shape (if disabled reduces CPU usage).
        private const int DRAW_LUT_SHAPE = AIL.M_YES;
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;         // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;              // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;             // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;               // Image buffer identifier.
            AIL_ID AilOriginalImage = AIL.M_NULL;       // Image buffer identifier.
            AIL_ID AilLut = AIL.M_NULL;                 // Lut buffer identifier.
            AIL_INT ImageSizeX = 0;
            AIL_INT ImageSizeY = 0;
            AIL_INT ImageMaxValue = 0;
            AIL_INT DisplaySizeBit = 0;
            AIL_INT DisplayMaxValue = 0;
            AIL_INT Start = 0;
            AIL_INT End = 0;
            AIL_INT Step = 0;
            AIL_INT InflectionLevel = 0;

            ConsoleKeyInfo Ch = new ConsoleKeyInfo();

            // Allocate the application, System and Display.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore target image from disk.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);

            // Dynamically calculates the maximum value of the image using processing.
            AIL_ID AilExtremeResult = AIL.M_NULL;
            AIL.MimAllocResult((AIL_ID)AIL.MbufInquire(AilImage, AIL.M_OWNER_SYSTEM, AIL.M_NULL), 1, AIL.M_EXTREME_LIST, ref AilExtremeResult);
            AIL.MimFindExtreme(AilImage, AilExtremeResult, AIL.M_MAX_VALUE);
            AIL.MimGetResult(AilExtremeResult, AIL.M_VALUE, ref ImageMaxValue);
            AIL.MimFree(AilExtremeResult);

            // Set the maximum value of the image to indicate to Aurora Imaging Library how to initialize 
            // the default display LUT.
            //
            AIL.MbufControl(AilImage, AIL.M_MAX, (double)ImageMaxValue);

            // Display the image (to specify a user-defined window, use AIL.MdispSelectWindow()).
            AIL.MdispSelect(AilDisplay, AilImage);

            // Determine the maximum displayable value of the current display.
            AIL.MdispInquire(AilDisplay, AIL.M_SIZE_BIT, ref DisplaySizeBit);
            DisplayMaxValue = (1 << (int)DisplaySizeBit) - 1;

            // Print key information.
            Console.Write("\nINTERACTIVE WINDOW LEVELING:\n");
            Console.Write("----------------------------\n\n");

            Console.Write("Image name : {0}\n", IMAGE_NAME);

            Console.Write("Image size : {0} x {1}\n", AIL.MbufInquire(AilImage, AIL.M_SIZE_X, ref ImageSizeX), AIL.MbufInquire(AilImage, AIL.M_SIZE_Y, ref ImageSizeY));
            Console.Write("Image max  : {0,4}\n", ImageMaxValue);
            Console.Write("Display max: {0,4}\n\n", DisplayMaxValue);

            // Allocate a LUT buffer according to the image maximum value and display pixel depth.
            AIL.MbufAlloc1d(AilSystem, ImageMaxValue + 1, ((DisplaySizeBit > 8) ? 16 : 8) + AIL.M_UNSIGNED, AIL.M_LUT, ref AilLut);

            // Generate a LUT with a full range ramp and set its maximum value.
            AIL.MgenLutRamp(AilLut, 0, 0, ImageMaxValue, (double)DisplayMaxValue);
            AIL.MbufControl(AilLut, AIL.M_MAX, (double)DisplayMaxValue);

            // Set the display LUT.
            AIL.MdispLut(AilDisplay, AilLut);

            // Interactive Window Leveling using keyboard.
            Console.Write("Keys assignment:\n\n");
            Console.Write("Arrow keys :    Left=move Left, Right=move Right, Down=Narrower, Up=Wider.\n");
            Console.Write("Intensity keys: L=Lower,  U=Upper,  R=Reset.\n");
            Console.Write("Press enter to end.\n\n");

            // Modify LUT shape according to the arrow keys and update it.
            Start = 0;
            End = ImageMaxValue;
            InflectionLevel = DisplayMaxValue;
            Step = (ImageMaxValue + 1) / 128;
            Step = Math.Max((int)Step, 4);
            while (Ch.Key != ConsoleKey.Enter)
            {
                switch (Ch.Key)
                {
                    // Left arrow: Move region left.
                    case ConsoleKey.LeftArrow:
                        { Start -= Step; End -= Step; break; }

                    // Right arrow: Move region right.
                    case ConsoleKey.RightArrow:
                        { Start += Step; End += Step; break; }

                    // Down arrow: Narrow region.
                    case ConsoleKey.DownArrow:
                        { Start += Step; End -= Step; break; }

                    // Up arrow: Widen region.
                    case ConsoleKey.UpArrow:
                        { Start -= Step; End += Step; break; }

                    // L key: Lower inflexion point.
                    case ConsoleKey.L:
                        { InflectionLevel--; break; }

                    // U key: Upper inflexion point.
                    case ConsoleKey.U:
                        { InflectionLevel++; break; }

                    // R key: Reset the LUT to full image range.
                    case ConsoleKey.R:
                        { Start = 0; End = ImageMaxValue; InflectionLevel = DisplayMaxValue; break; }
                }

                // Saturate.
                End = Math.Min((int)End, (int)ImageMaxValue);
                Start = Math.Min((int)Start, (int)End);
                End = Math.Max((int)End, (int)Start);
                Start = Math.Max((int)Start, 0);
                End = Math.Max((int)End, 0);
                InflectionLevel = Math.Max((int)InflectionLevel, 0);
                InflectionLevel = Math.Min((int)InflectionLevel, (int)DisplayMaxValue);
                Console.Write("Inflection points: Low=({0},0), High=({1},{2}).   \r", Start, End, InflectionLevel);

                // Generate a LUT with 3 slopes and saturated at both ends.
                AIL.MgenLutRamp(AilLut, 0, 0, Start, 0);
                AIL.MgenLutRamp(AilLut, Start, 0, End, (double)InflectionLevel);
                AIL.MgenLutRamp(AilLut, End, (double)InflectionLevel, ImageMaxValue, (double)DisplayMaxValue);

                // Update the display LUT.
                AIL.MdispLut(AilDisplay, AilLut);

                // Draw the current LUT's shape in the image.
                // Note: This simple annotation method requires
                //       significant update and CPU time.
                //
                if (DRAW_LUT_SHAPE == AIL.M_YES)
                {
                    if (AilOriginalImage == AIL.M_NULL)
                    {
                        AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilOriginalImage);
                    }
                    DrawLutShape(AilDisplay, AilOriginalImage, AilImage, Start, End, InflectionLevel, ImageMaxValue, DisplayMaxValue);
                }

                // If its an arrow key, get the second code.
                Ch = Console.ReadKey(true);
            }
            Console.Write("\n\n");

            // Free all allocations.
            AIL.MbufFree(AilLut);
            AIL.MbufFree(AilImage);
            if (AilOriginalImage != AIL.M_NULL)
            {
                AIL.MbufFree(AilOriginalImage);
            }
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        // Function to draw the current LUT's shape in the image.
        //
        //   Note: This simple annotation method requires significant update
        //  and CPU time since it repaints the entire image every time.
        //
        static void DrawLutShape(AIL_ID AilDisplay, AIL_ID AilOriginalImage, AIL_ID AilImage, AIL_INT Start, AIL_INT End, AIL_INT InflexionIntensity, AIL_INT ImageMaxValue, AIL_INT DisplayMaxValue)
        {
            double Xstart = 0.0;
            double Xend = 0.0;
            double Xstep = 0.0;
            double Ymin = 0.0;
            double Yinf = 0.0;
            double Ymax = 0.0;
            double Ystep = 0.0;
            AIL_INT ImageSizeX = 0;
            AIL_INT ImageSizeY = 0;


            // Inquire image dimensions.
            AIL.MbufInquire(AilImage, AIL.M_SIZE_X, ref ImageSizeX);
            AIL.MbufInquire(AilImage, AIL.M_SIZE_Y, ref ImageSizeY);

            // Calculate the drawing parameters.
            Xstep = (double)ImageSizeX / (double)ImageMaxValue;
            Xstart = Start * Xstep;
            Xend = End * Xstep;
            Ystep = ((double)ImageSizeY / 4.0) / (double)DisplayMaxValue;
            Ymin = ((double)ImageSizeY - 2);
            Yinf = Ymin - (InflexionIntensity * Ystep);
            Ymax = Ymin - (DisplayMaxValue * Ystep);

            // To increase speed, disable display update until all annotations are done.
            AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_DISABLE);

            // Restore the original image.
            AIL.MbufCopy(AilOriginalImage, AilImage);

            // Draw axis max and min values.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, (double)ImageMaxValue);
            AIL.MgraText(AIL.M_DEFAULT, AilImage, 4, (int)Ymin - 22, "0");
            AIL.MgraText(AIL.M_DEFAULT, AilImage, 4, (int)Ymax - 16, String.Format("{0}", DisplayMaxValue));
            AIL.MgraText(AIL.M_DEFAULT, AilImage, ImageSizeX - 38, (int)Ymin - 22, String.Format("{0}", ImageMaxValue));

            // Draw LUT Shape (X axis is display values and Y is image values).
            AIL.MgraLine(AIL.M_DEFAULT, AilImage, 0, (int)Ymin, (int)Xstart, (int)Ymin);
            AIL.MgraLine(AIL.M_DEFAULT, AilImage, (int)Xstart, (int)Ymin, (int)Xend, (int)Yinf);
            AIL.MgraLine(AIL.M_DEFAULT, AilImage, (int)Xend, (int)Yinf, ImageSizeX - 1, (int)Ymax);

            // Enable display update to show the result.
            AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_ENABLE);
        }
    }
}

```
