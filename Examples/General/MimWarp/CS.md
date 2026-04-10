---
title: "MimWarp"
description: "This program performs three types of warp transformations. First the image is stretched according to four specified reference points. Then it is warped on a sinusoid, and finally, the program loops while warping the image on a sphere."
ms.language: "C#"
---

# MimWarp

> This program performs three types of warp transformations. First the image is stretched according to four specified reference points. Then it is warped on a sinusoid, and finally, the program loops while warping the image on a sphere.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufAlloc2d`, `MbufChild2d`, `MbufChildMove`, `MbufClear`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufPut1d`, `MbufPut2d`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgenWarpParameter`, `MimWarp`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MimWarp.cs 
//
// Description: This program performs three types of warp transformations. 
//              First the image is stretched according to four specified 
//              reference points. Then it is warped on a sinusoid, and 
//              finally, the program loops while warping the image on a 
//              sphere.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MImWarp
{
    class Program
    {
        // Target image specifications.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "BaboonMono.mim";
        private const int INTERPOLATION_MODE = AIL.M_NEAREST_NEIGHBOR;
        private static int FIXED_POINT_PRECISION
        {
            get { return AIL.M_FIXED_POINT + ((INTERPOLATION_MODE == AIL.M_NEAREST_NEIGHBOR) ? 0 : 6); }
        }

        private static int FLOAT_TO_FIXED_POINT(double x)
        {
            return (int)(((INTERPOLATION_MODE == AIL.M_NEAREST_NEIGHBOR) ? 1 : 64) * x);
        }

        private const int ROTATION_STEP = 1;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;         // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;              // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;             // Display identifier.
            AIL_ID AilDisplayImage = AIL.M_NULL;        // Image buffer identifier.
            AIL_ID AilSourceImage = AIL.M_NULL;         // Image buffer identifier.
            AIL_ID Ail4CornerArray = AIL.M_NULL;        // Coefficients buffer identifier.
            AIL_ID AilLutX = AIL.M_NULL;                // Lut buffer identifier.
            AIL_ID AilLutY = AIL.M_NULL;                // Lut buffer identifier.
            AIL_ID ChildWindow = AIL.M_NULL;            // Child Image identifier.
            float[] FourCornerMatrix = new float[12] 
            {
                0.0F,                                       // X coordinate of quadrilateral's 1st corner
                0.0F,                                       // Y coordinate of quadrilateral's 1st corner
                456.0F,                                     // X coordinate of quadrilateral's 2nd corner
                62.0F,                                      // Y coordinate of quadrilateral's 2nd corner
                333.0F,                                     // X coordinate of quadrilateral's 3rd corner
                333.0F,                                     // Y coordinate of quadrilateral's 3rd corner
                100.0F,                                     // X coordinate of quadrilateral's 4th corner
                500.0F,                                     // Y coordinate of quadrilateral's 4th corner
                0.0F,                                       // X coordinate of rectangle's top-left corner
                0.0F,                                       // Y coordinate of rectangle's top-left corner
                511.0F,                                     // X coordinate of rectangle's bottom-right corner
                511.0F 
            };                                   // Y coordinate of rectangle's bottom-right corner
            AIL_INT Precision = FIXED_POINT_PRECISION;
            AIL_INT Interpolation = INTERPOLATION_MODE;
            short[] AilLutXPtr, AilLutYPtr;
            AIL_INT OffsetX = 0;
            AIL_INT ImageWidth = 0;
            AIL_INT ImageHeight = 0;
            AIL_INT ImageType = 0;
            AIL_INT i = 0;
            AIL_INT j = 0;
            double FramesPerSecond = 0.0;
            double Time = 0.0;
            double NbLoop = 0.0;

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore the source image.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilSourceImage);

            // Allocate a display buffers and show the source image.
            AIL.MbufAlloc2d(AilSystem,
                AIL.MbufInquire(AilSourceImage, AIL.M_SIZE_X, ref ImageWidth),
                AIL.MbufInquire(AilSourceImage, AIL.M_SIZE_Y, ref ImageHeight),
                AIL.MbufInquire(AilSourceImage, AIL.M_TYPE, ref ImageType),
                AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilDisplayImage);
            AIL.MbufCopy(AilSourceImage, AilDisplayImage);
            AIL.MdispSelect(AilDisplay, AilDisplayImage);

            // Print a message.
            Console.Write("\nWARPING:\n");
            Console.Write("--------\n\n");
            Console.Write("This image will be warped using different methods.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);


            // Four-corner LUT warping
            //-------------------------

            // Allocate 2 LUT buffers.
            AIL.MbufAlloc2d(AilSystem, ImageWidth, ImageHeight, 16 + AIL.M_SIGNED, AIL.M_LUT, ref AilLutX);
            AIL.MbufAlloc2d(AilSystem, ImageWidth, ImageHeight, 16 + AIL.M_SIGNED, AIL.M_LUT, ref AilLutY);

            // Allocate the coefficient buffer.
            AIL.MbufAlloc2d(AilSystem, 12, 1, 32 + AIL.M_FLOAT, AIL.M_ARRAY, ref Ail4CornerArray);

            // Put warp values into the coefficient buffer.
            AIL.MbufPut1d(Ail4CornerArray, 0, 12, FourCornerMatrix);

            // Generate LUT buffers.
            AIL.MgenWarpParameter(Ail4CornerArray, AilLutX, AilLutY, AIL.M_WARP_4_CORNER + Precision, AIL.M_DEFAULT, 0.0, 0.0);

            // Clear the destination.
            AIL.MbufClear(AilDisplayImage, 0);

            // Warp the image.
            AIL.MimWarp(AilSourceImage, AilDisplayImage, AilLutX, AilLutY, AIL.M_WARP_LUT + Precision, Interpolation);

            // Print a message.
            Console.Write("The image was warped from an arbitrary quadrilateral to a square.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);


            // Sinusoidal LUT warping
            //------------------------

            // Allocate user-defined LUTs.
            AilLutXPtr = new short[ImageHeight * ImageWidth];
            AilLutYPtr = new short[ImageHeight * ImageWidth];

            // Fill the LUT with a sinusoidal waveforms with a 6-bit precision.
            for (j = 0; j < ImageHeight; j++)
            {
                for (i = 0; i < ImageWidth; i++)
                {
                    AilLutYPtr[i + (j * ImageWidth)] = (short)FLOAT_TO_FIXED_POINT(((j) + (int)((20 * Math.Sin(0.03 * i)))));
                    AilLutXPtr[i + (j * ImageWidth)] = (short)FLOAT_TO_FIXED_POINT(((i) + (int)((20 * Math.Sin(0.03 * j)))));
                }
            }

            // Put the values into the LUT buffers.
            AIL.MbufPut2d(AilLutX, 0, 0, ImageWidth, ImageHeight, AilLutXPtr);
            AIL.MbufPut2d(AilLutY, 0, 0, ImageWidth, ImageHeight, AilLutYPtr);

            // Clear the destination.
            AIL.MbufClear(AilDisplayImage, 0);

            // Warp the image.
            AIL.MimWarp(AilSourceImage, AilDisplayImage, AilLutX, AilLutY, AIL.M_WARP_LUT + Precision, Interpolation);

            // wait for a key
            Console.Write("The image was warped on two sinusoidal waveforms.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Continuous spherical LUT warping
            //--------------------------------

            // Allocate temporary buffer.
            AIL.MbufFree(AilSourceImage);
            AIL.MbufAlloc2d(AilSystem, ImageWidth * 2, ImageHeight, ImageType, AIL.M_IMAGE + AIL.M_PROC, ref AilSourceImage);

            // Reload the image.
            AIL.MbufLoad(IMAGE_FILE, AilSourceImage);

            // Fill the LUTs with a sphere pattern with a 6-bit precision.
            GenerateSphericLUT(ImageWidth, ImageHeight, AilLutXPtr, AilLutYPtr);
            AIL.MbufPut2d(AilLutX, 0, 0, ImageWidth, ImageHeight, AilLutXPtr);
            AIL.MbufPut2d(AilLutY, 0, 0, ImageWidth, ImageHeight, AilLutYPtr);

            // Duplicate the buffer to allow wrap around in the warping.
            AIL.MbufCopy(AilSourceImage, AilDisplayImage);
            AIL.MbufChild2d(AilSourceImage, ImageWidth, 0, ImageWidth, ImageHeight, ref ChildWindow);
            AIL.MbufCopy(AilDisplayImage, ChildWindow);
            AIL.MbufFree(ChildWindow);

            // Clear the destination.
            AIL.MbufClear(AilDisplayImage, 0);

            // Print a message and start the timer.
            Console.Write("The image is continuously warped on a sphere.\n");
            Console.Write("Press any key to stop.\n\n");
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            // Create a child in the buffer containing the two images.
            AIL.MbufChild2d(AilSourceImage, OffsetX, 0, ImageWidth, ImageHeight, ref ChildWindow);

            // Warp the image continuously.
            while ((!Console.KeyAvailable) || (OffsetX != (ImageWidth / 4)))
            {
                // Move the child to the new position
                AIL.MbufChildMove(ChildWindow, OffsetX, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Warp the child in the window.
                AIL.MimWarp(ChildWindow, AilDisplayImage, AilLutX, AilLutY, AIL.M_WARP_LUT + Precision, Interpolation);

                // Update the offset (shift the window to the right).
                OffsetX += ROTATION_STEP;

                // Reset the offset if the child is outside the buffer.
                if (OffsetX > ImageWidth - 1)
                {
                    OffsetX = 0;
                }

                NbLoop++;

                // Calculate and print the number of frames per second processed.
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref Time);
                FramesPerSecond = NbLoop / Time;
                Console.Write("Processing speed: {0:0} Images/Sec.\r", FramesPerSecond);
            }
            Console.ReadKey(true);
            Console.Write("\nPress any key to end.\n");
            Console.ReadKey(true);

            // Free objects.
            AIL.MbufFree(ChildWindow);
            AIL.MbufFree(AilLutX);
            AIL.MbufFree(AilLutY);
            AIL.MbufFree(Ail4CornerArray);
            AIL.MbufFree(AilSourceImage);
            AIL.MbufFree(AilDisplayImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        // Generate two custom LUTs used to map the image on a sphere.
        // -----------------------------------------------------------
        static void GenerateSphericLUT(AIL_INT ImageWidth, AIL_INT ImageHeight, short[] AilLutXPtr, short[] AilLutYPtr)
        {
            AIL_INT i, j, k;
            double utmp, vtmp, tmp;
            short v;

            // Set the radius of the sphere
            double Radius = 200.0;

            // Generate the X and Y buffers
            for (j = 0; j < ImageHeight; j++)
            {
                k = j * ImageWidth;

                // Check that still in the sphere (in the Y axis).
                if (Math.Abs(vtmp = ((double)(j - (ImageHeight / 2)) / Radius)) < 1.0)
                {
                    // We scan from top to bottom, so reverse the value obtained above
                    // and obtain the angle.
                    vtmp = Math.Acos(-vtmp);
                    if (vtmp == 0.0)
                    {
                        vtmp = 0.0000001;
                    }

                    // Compute the position to fetch in the source.
                    v = (short)((vtmp / 3.1415926) * (double)(ImageHeight - 1) + 0.5);

                    // Compute the Y coordinate of the sphere.
                    tmp = Radius * Math.Sin(vtmp);

                    for (i = 0; i < ImageWidth; i++)
                    {
                        // Check that still in the sphere.
                        if (Math.Abs(utmp = ((double)(i - (ImageWidth / 2)) / tmp)) < 1.0)
                        {
                            utmp = Math.Acos(-utmp);

                            // Compute the position to fetch (fold the image in four).
                            AilLutXPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(((utmp / 3.1415926) *
                                     (double)((ImageWidth / 2) - 1) + 0.5));
                            AilLutYPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(v);
                        }
                        else
                        {
                            // Default position (fetch outside the buffer to 
                            // activate the clear overscan).
                            AilLutXPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(ImageWidth);
                            AilLutYPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(ImageHeight);
                        }
                    }
                }
                else
                {
                    for (i = 0; i < ImageWidth; i++)
                    {
                        // Default position (fetch outside the buffer for clear overscan).
                        AilLutXPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(ImageWidth);
                        AilLutYPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(ImageHeight);
                    }
                }
            }
        }
    }
}

```
