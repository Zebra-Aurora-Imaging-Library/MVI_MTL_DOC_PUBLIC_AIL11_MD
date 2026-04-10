---
title: "MbufPointerAccess"
description: "This program shows how to use the pointer of a AIL buffer in order to directly access its data."
ms.language: "C#"
---

# MbufPointerAccess

> This program shows how to use the pointer of a AIL buffer in order to directly access its data.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufChildColor`, `MbufControl`, `MbufFree`, `MbufInquire`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MbufPointerAccess.cs
//
// Description: This program shows how to use the pointer of a
//              buffer in order to directly access its data.
//
// Note:        This program does not support Distributed Aurora Imaging Library (DAIL).
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MBufPointerAccess
{
    class Program
    {
        // Target image size.
        private const int IMAGE_SIZE_X = 512;
        private const int IMAGE_SIZE_Y = 512;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;  // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;       // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;      // Display identifier.

            Console.WriteLine();
            Console.WriteLine("Buffer pointer access example.");
            Console.WriteLine("----------------------------------");
            Console.WriteLine();

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Run the custom function only if the target system's memory is local and accessible.
            if (AIL.MsysInquire(AilSystem, AIL.M_LOCATION, AIL.M_NULL) == AIL.M_LOCAL)
            {

                // Pointer access example for a monochrome buffer
                MonochromeBufferPointerAccessExample(AilSystem, AilDisplay);

                // Pointer access example for a color packed buffer.
                ColorPackedBufferPointerAccessExample(AilSystem, AilDisplay);

                // Pointer access example for a color planar buffer.
                ColorPlanarBufferPointerAccessExample(AilSystem, AilDisplay);
            }
            else
            {
                // Print that the example don't run remotely.
                Console.WriteLine("This example doesn't run with Distributed AIL.");
                // Wait for a key to terminate.
                Console.WriteLine("Press a key to terminate.");
                Console.WriteLine();
                Console.ReadKey(true);
            }

            // Free allocated objects.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        // Pointer access example for a monochrome buffer.
        // ------------------------------------------------

        // Pixel value calculation parameters.
        private const double X_REF1 = -0.500;
        private const double Y_REF1 = +0.002;
        private const double DIM1 = +3.200;

        static void MonochromeBufferPointerAccessExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;       // Image buffer identifier.
            AIL_INT AilImagePtr = AIL.M_NULL;   // Image pointer.
            AIL_INT AilImagePitch = AIL.M_NULL; // Image pitch.
            AIL_INT x, y;                       // Buffer access variables.
            AIL_UINT Value;                     // Value to write.

            Console.Write("- The data of a 8bits monochrome buffer is modified\n");
            Console.Write("  using its pointer to directly access the memory.\n\n");

            // Allocate a monochrome buffer.
            AIL.MbufAlloc2d(AilSystem, IMAGE_SIZE_X, IMAGE_SIZE_Y, 8 + AIL.M_UNSIGNED,
               AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilImage);

            // Lock buffer for direct access.
            AIL.MbufControl(AilImage, AIL.M_LOCK, AIL.M_DEFAULT);

            // Retrieving buffer data pointer and pitch information.
            AIL.MbufInquire(AilImage, AIL.M_HOST_ADDRESS, ref AilImagePtr);
            AIL.MbufInquire(AilImage, AIL.M_PITCH, ref AilImagePitch);

            // Direct Access to the buffer's data.
            if (AilImagePtr != AIL.M_NULL)
            {
                unsafe // Unsafe code block to allow manipulating memory addresses
                {
                    // Convert the address inquired from Aurora Imaging Library to a fixed size pointer.
                    // The AIL_INT type cannot be converted to a byte * so use the
                    // IntPtr portable type
                    //
                    IntPtr AilImagePtrIntPtr = AilImagePtr;
                    byte* AilImageAddr = (byte*)AilImagePtrIntPtr;

                    // For each row.
                    for (y = 0; y < IMAGE_SIZE_Y; y++)
                    {
                        // For each column.
                        for (x = 0; x < IMAGE_SIZE_X; x++)
                        {
                            // Calculate the pixel value.
                            Value = Mandelbrot(x, y, X_REF1, Y_REF1, DIM1);

                            // Write the pixel using its pointer.
                            AilImageAddr[x] = (byte)(Value);
                        }

                        // Move pointer to the next line taking into account the image's pitch.
                        AilImageAddr += AilImagePitch;
                    }


                    //  Signals that the buffer data has been updated.
                    AIL.MbufControl(AilImage, AIL.M_MODIFIED, AIL.M_DEFAULT);

                    // Unlock buffer.
                    AIL.MbufControl(AilImage, AIL.M_UNLOCK, AIL.M_DEFAULT);

                    // Select to display.
                    AIL.MdispSelect(AilDisplay, AilImage);
                }
            }
            else
            {
                Console.Write("The source buffer has no accessible memory\n");
                Console.Write("address on this specific system. Try changing\n");
                Console.Write("the system in the Aurora Imaging Configurator utility.\n\n");
            }

            // Print a message and wait for a key.
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Free allocation.
            AIL.MbufFree(AilImage);
        }

        // Pointer access example for a color packed buffer.
        // --------------------------------------------------

        // Pixel value calculation parameters.
        private const double X_REF2 = -1.1355;
        private const double Y_REF2 = -0.2510;
        private const double DIM2 = +0.1500;

        // Utility to pack B,G,R values into 32 bits integer.
        static uint PackToBGR32(Byte b, Byte g, Byte r)
        {
            return ((uint)b | (uint)(g << 8) | (uint)(r << 16));
        }

        static void ColorPackedBufferPointerAccessExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;       // Image buffer identifier.
            AIL_INT AilImagePtr = AIL.M_NULL;   // Image pointer.
            AIL_INT AilImagePitch = AIL.M_NULL; // Image pitch.
            AIL_INT x, y, NbBand = 3;           // Buffer access variables.
            AIL_UINT Value;                     // Value to write.
            uint Value_BGR32;                   // Value to write.

            Console.Write("- The data of a 32bits color packed buffer is modified\n");
            Console.Write("  using its pointer to directly access the memory.\n\n");

            // Allocate a monochrome buffer.
            AIL.MbufAllocColor(AilSystem, NbBand, IMAGE_SIZE_X, IMAGE_SIZE_Y, 8 + AIL.M_UNSIGNED,
               AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP + AIL.M_BGR32 + AIL.M_PACKED, ref AilImage);

            // Lock buffer for direct access.
            AIL.MbufControl(AilImage, AIL.M_LOCK, AIL.M_DEFAULT);

            // Retrieving buffer data pointer and pitch information.
            AIL.MbufInquire(AilImage, AIL.M_HOST_ADDRESS, ref AilImagePtr);
            AIL.MbufInquire(AilImage, AIL.M_PITCH, ref AilImagePitch);

            // Custom modification of the buffer's data.
            if (AilImagePtr != AIL.M_NULL)
            {
                unsafe // Unsafe code block to allow manipulating memory addresses
                {
                    // Convert the address inquired from Aurora Imaging Library to a fixed size pointer.
                    // The AIL_INT type cannot be converted to a byte * so use the
                    // IntPtr portable type
                    //
                    IntPtr AilImagePtrIntPtr = AilImagePtr;
                    uint* AilImageAddr = (uint*)AilImagePtrIntPtr;

                    // For each row.
                    for (y = 0; y < IMAGE_SIZE_Y; y++)
                    {
                        // For each column.
                        for (x = 0; x < IMAGE_SIZE_X; x++)
                        {
                            // Calculate the pixel value.
                            Value = Mandelbrot(x, y, X_REF2, Y_REF2, DIM2);

                            Value_BGR32 = PackToBGR32(
                               (byte)GetColorFromIndex(AIL.M_BLUE, (AIL_INT)Value, 255),
                               (byte)GetColorFromIndex(AIL.M_GREEN, (AIL_INT)Value, 255),
                               (byte)GetColorFromIndex(AIL.M_RED, (AIL_INT)Value, 255)
                               );

                            // Write the pixel using its pointer.
                            AilImageAddr[x] = (uint)Value_BGR32;
                        }

                        // Move pointer to the next line taking into account the image's pitch.
                        AilImageAddr += AilImagePitch;
                    }

                    //  Signals that the buffer data has been updated.
                    AIL.MbufControl(AilImage, AIL.M_MODIFIED, AIL.M_DEFAULT);

                    // Unlock buffer.
                    AIL.MbufControl(AilImage, AIL.M_UNLOCK, AIL.M_DEFAULT);

                    // Select to display.
                    AIL.MdispSelect(AilDisplay, AilImage);
                }
            }
            else
            {
                Console.Write("The source buffer has no accessible memory\n");
                Console.Write("address on this specific system. Try changing\n");
                Console.Write("the system in the Aurora Imaging Configurator utility.\n\n");
            }

            // Print a message and wait for a key.
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Free allocation.
            AIL.MbufFree(AilImage);
        }

        // Pointer access example for a color planar buffer.
        // ------------------------------------------------

        // Pixel value calculation parameters.
        private const double X_REF3 = -0.7453;
        private const double Y_REF3 = +0.1127;
        private const double DIM3 = +0.0060;

        static void ColorPlanarBufferPointerAccessExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;         // Image buffer identifier.
            AIL_ID AilImageBand = AIL.M_NULL;     // Image band identifier.
            AIL_INT AilImageBandPtr = AIL.M_NULL; // Image pointer.
            AIL_INT AilImagePitch = AIL.M_NULL;   // Image pitch.
            AIL_INT x, y, i, NbBand = 3;          // Buffer access variables.
            AIL_UINT Value;                       // Value to write.

            AIL_INT[] ColorBand = new AIL_INT[] { AIL.M_RED, AIL.M_GREEN, AIL.M_BLUE };

            Console.Write("- The data of a 24bits color planar buffer is modified using\n");
            Console.Write("  each color band pointer's to directly access the memory.\n\n");

            // Allocate a monochrome buffer.
            AIL.MbufAllocColor(AilSystem, NbBand, IMAGE_SIZE_X, IMAGE_SIZE_Y, 8 + AIL.M_UNSIGNED,
               AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP + AIL.M_PLANAR, ref AilImage);

            // Retrieving buffer pitch information.
            AIL.MbufInquire(AilImage, AIL.M_PITCH, ref AilImagePitch);

            // Lock buffer for direct access.
            AIL.MbufControl(AilImage, AIL.M_LOCK, AIL.M_DEFAULT);

            // Verifying the buffer has a host address.
            AIL.MbufChildColor(AilImage, AIL.M_RED, ref AilImageBand);
            AIL.MbufInquire(AilImageBand, AIL.M_HOST_ADDRESS, ref AilImageBandPtr);
            AIL.MbufFree(AilImageBand);

            if (AilImageBandPtr != AIL.M_NULL)
            {
                unsafe // Unsafe code block to allow manipulating memory addresses
                {
                    // For each color band.
                    for (i = 0; i < NbBand; i++)
                    {
                        // Retrieving buffer color band pointer.
                        AIL.MbufChildColor(AilImage, (AIL_INT)ColorBand[i], ref AilImageBand);
                        AIL.MbufInquire(AilImageBand, AIL.M_HOST_ADDRESS, ref AilImageBandPtr);

                        // Convert the address inquired from Aurora Imaging Library to a fixed size pointer.
                        // The AIL_INT type cannot be converted to a byte * so use the
                        // IntPtr portable type
                        //
                        IntPtr AilImageBandPtrIntPtr = AilImageBandPtr;
                        byte* AilImageBandAddr = (byte*)AilImageBandPtrIntPtr;

                        // For each row.
                        for (y = 0; y < IMAGE_SIZE_Y; y++)
                        {
                            // For each column.
                            for (x = 0; x < IMAGE_SIZE_X; x++)
                            {
                                // Calculate the pixel value.
                                Value = Mandelbrot(x, y, X_REF3, Y_REF3, DIM3);

                                // Write the pixel using its pointer.
                                AilImageBandAddr[x] = (byte)GetColorFromIndex(ColorBand[i], (AIL_INT)Value, 255);
                            }

                            // Move pointer to the next line taking into account the image's pitch.
                            AilImageBandAddr += AilImagePitch;
                        }

                        AIL.MbufFree(AilImageBand);
                    }
                }

                //  Signals that the buffer data has been updated.
                AIL.MbufControl(AilImage, AIL.M_MODIFIED, AIL.M_DEFAULT);

                // Unlock buffer.
                AIL.MbufControl(AilImage, AIL.M_UNLOCK, AIL.M_DEFAULT);

                // Select to display.
                AIL.MdispSelect(AilDisplay, AilImage);
            }
            else
            {
                Console.Write("The source buffer has no accessible memory\n");
                Console.Write("address on this specific system. Try changing\n");
                Console.Write("the system in the Aurora Imaging Configurator utility.\n\n");
            }

            // Print a message and wait for a key.
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Free allocation.
            AIL.MbufFree(AilImage);
        }

        // Mandelbrot fractal utility functions.
        static double Remap(double pos, double size, double min, double max)
        {
            return ((((max) - (min)) / (size)) * (pos) + (min));
        }

        static AIL_UINT Mandelbrot(AIL_INT PosX, AIL_INT PosY, double RefX,
           double RefY, double Dim)
        {
            const int maxIter = 256;
            double xMin = RefX - 0.5 * Dim;
            double xMax = RefX + 0.5 * Dim;
            double yMin = RefY - 0.5 * Dim;
            double yMax = RefY + 0.5 * Dim;
            double x0 = Remap((double)PosX, (double)IMAGE_SIZE_X, xMin, xMax);
            double y0 = Remap((double)PosY, (double)IMAGE_SIZE_Y, yMin, yMax);
            double x = 0.0;
            double y = 0.0;
            AIL_UINT Iter = 0;

            while ((x * x + y * y < 4) && (Iter < maxIter))
            {
                double Temp = x * x - y * y + x0;
                y = 2 * x * y + y0;
                x = Temp;
                Iter++;
            }

            return Math.Min(255, Iter);
        }

        // Calculate color from index.
        static AIL_INT GetColorFromIndex(AIL_INT Band, AIL_INT Index, AIL_INT MaxIndex)
        {
            AIL_INT[] Segments = { };
            AIL_INT[] SegmentsR = { 0, 0, 0, 255, 255, 128 };
            AIL_INT[] SegmentsG = { 0, 0, 255, 255, 0, 0 };
            AIL_INT[] SegmentsB = { 128, 255, 255, 0, 0, 0 };

            switch ((int)Band)
            {
                case AIL.M_RED:
                    Segments = SegmentsR;
                    break;
                case AIL.M_GREEN:
                    Segments = SegmentsG;
                    break;
                case AIL.M_BLUE:
                    Segments = SegmentsB;
                    break;
            }

            double RemapedIndex = Index * MaxIndex / 256.0;
            AIL_INT SegmentIndex = (AIL_INT)(RemapedIndex * 5.0 / 256.0);
            double Slope = (Segments[SegmentIndex + 1] - Segments[SegmentIndex]) / (256.0 / 5.0);
            double Offset = (Segments[SegmentIndex] - Slope * SegmentIndex * 256.0 / 5.0);
            AIL_INT Value = (AIL_INT)(Slope * RemapedIndex + Offset + 0.5);

            return Value;
        }
    }
}

```
