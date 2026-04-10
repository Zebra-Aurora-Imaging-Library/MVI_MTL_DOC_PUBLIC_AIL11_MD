---
title: "MimLocatePeak1d"
description: "This program finds the peak in each column of an input sequence and reconstruct the height of a 3D object using it."
ms.language: "C#"
---

# MimLocatePeak1d

> This program finds the peak in each column of an input sequence and reconstruct the height of a 3D object using it.

**Language:** C#

**Functions used:** `M3ddispAlloc`, `M3ddispControl`, `M3ddispFree`, `M3ddispSelect`, `MappControl`, `MbufAllocComponent`, `MbufAllocContainer`, `MbufConvert3d`, `McalControl`, `McalUniform`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MimAlloc`, `MappAlloc`, `MappFree`, `MappTimer`, `MbufAlloc2d`, `MbufClear`, `MbufCopy`, `MbufDiskInquire`, `MbufFree`, `MbufImportSequence`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MimAllocResult`, `MimDraw`, `MimFree`, `MimGetResult`, `MimLocatePeak1d`, `MimStatCalculate`, `MimArith`, `MimClip`, `MimControl`, `MimResize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Image Processing, 3D Display, Calibration, Graphics, What's New, AIL 10.0 SP4, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImLocatePeak1d.cs
//
// Description: This program finds the peak in each column of an input sequence
//              and reconstruct the height of a 3D object using it.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////


using System;
using System.Text;
using System.Threading;

using Zebra.AuroraImagingLibrary;

namespace MimLocatePeak1d
{
    class Program
    {
        // Input sequence specifications.
        private const string SEQUENCE_FILE = AIL.M_IMAGE_PATH + "HandWithLaser.avi";

        //     ^            +
        //     |        +       +
        //     |      + <-Width-> + <------------
        //     |     +             +             | Min contrast
        //     | ++++               ++++++++ <---
        //     |
        //     |
        //     ------------------------------>
        //        Peak intensity profile

        // Peak detection parameters.
        private const int LINE_WIDTH_AVERAGE = 20;
        private const int LINE_WIDTH_DELTA = 20;
        private const double MIN_CONTRAST = 100.0;
        private const int NB_FIXED_POINT = 4;

        // M3D display parameters
        private const double M3D_MESH_SCALING_X = 1.0;
        private const double M3D_MESH_SCALING_Y = 4.0;
        private const double M3D_MESH_SCALING_Z = -0.13;

        static int Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;          // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;               // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;              // Display identifier.
            AIL_ID AilDisplayImage = AIL.M_NULL;         // Image buffer identifier.
            AIL_ID AilGraList = AIL.M_NULL;              // Graphic list identifier.
            AIL_ID AilImage = AIL.M_NULL;                // Image buffer identifier.
            AIL_ID AilPosYImage = AIL.M_NULL;            // Image buffer identifier.
            AIL_ID AilValImage = AIL.M_NULL;             // Image buffer identifier.
            AIL_ID AilContext = AIL.M_NULL;              // Processing context identifier.
            AIL_ID AilLocatePeak = AIL.M_NULL;           // Processing result identifier.
            AIL_ID AilStatContext = AIL.M_NULL;          // Statistics context identifier.
            AIL_ID AilExtreme = AIL.M_NULL;              // Result buffer identifier.

            AIL_INT SizeX = 0;
            AIL_INT SizeY = 0;
            AIL_INT NumberOfImages = 0;
            double FrameRate = 0.0;
            AIL_INT n = 0;
            double PreviousTime = 0.0;
            double StartTime = 0.0;
            double EndTime = 0.0;
            double TotalProcessTime = 0.0;
            double WaitTime = 0.0;
            AIL_INT[] ExtremePosY = new AIL_INT[] { 0, 0 };

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Inquire characteristics of the input sequence.
            AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_SIZE_X, ref SizeX);
            AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_SIZE_Y, ref SizeY);
            AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_NUMBER_OF_IMAGES, ref NumberOfImages);
            AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_FRAME_RATE, ref FrameRate);

            // Allocate buffers to hold images.
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref AilImage);
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_DISP, ref AilDisplayImage);
            AIL.MbufAlloc2d(AilSystem, SizeX, NumberOfImages, 16 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref AilPosYImage);
            AIL.MbufAlloc2d(AilSystem, SizeX, NumberOfImages, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref AilValImage);

            // Allocate context for AIL.MimLocatePeak1D
            AIL.MimAlloc(AilSystem, AIL.M_LOCATE_PEAK_1D_CONTEXT, AIL.M_DEFAULT, ref AilContext);

            // Allocate result for AIL.MimLocatePeak1D
            AIL.MimAllocResult(AilSystem, AIL.M_DEFAULT, AIL.M_LOCATE_PEAK_1D_RESULT, ref AilLocatePeak);

            // Allocate graphic list.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref AilGraList);

            // Select display.
            AIL.MdispSelect(AilDisplay, AilDisplayImage);
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraList);

            // Print a message.
            Console.WriteLine();
            Console.WriteLine("EXTRACTING 3D IMAGE FROM A LASER LINE (SHEET-OF-LIGHT):");
            Console.WriteLine("--------------------------------------------------------");
            Console.WriteLine();
            Console.WriteLine("The position of a laser line is being extracted from an image");
            Console.WriteLine("to generate a depth image.");
            Console.WriteLine();

            // Open the sequence file for reading.
            AIL.MbufImportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_OPEN);

            // Preprocess the context.
            AIL.MimLocatePeak1d(AilContext, AilImage, AilLocatePeak, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_PREPROCESS, AIL.M_DEFAULT);

            // Read and process all images in the input sequence.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref PreviousTime);
            TotalProcessTime = 0.0;

            for (n = 0; n < NumberOfImages; n++)
            {
                // Read image from sequence.
                AIL.MbufImportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_LOAD, AIL.M_NULL, ref AilImage, AIL.M_DEFAULT, 1, AIL.M_READ);

                // Display the image.
                AIL.MbufCopy(AilImage, AilDisplayImage);

                // Locate the peak in each column of the image.
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref StartTime);

                AIL.MimLocatePeak1d(AilContext, AilImage, AilLocatePeak, LINE_WIDTH_AVERAGE, LINE_WIDTH_DELTA, MIN_CONTRAST, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Draw extracted peaks.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);
                AIL.MgraClear(AIL.M_DEFAULT, AilGraList);
                AIL.MimDraw(AIL.M_DEFAULT, AilLocatePeak, AIL.M_NULL, AilGraList, AIL.M_DRAW_PEAKS + AIL.M_CROSS, AIL.M_ALL, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Draw peak's data to depth map.
                AIL.MimDraw(AIL.M_DEFAULT, AilLocatePeak, AIL.M_NULL, AilPosYImage, AIL.M_DRAW_DEPTH_MAP_ROW, n, AIL.M_NULL, AIL.M_FIXED_POINT + NB_FIXED_POINT);
                AIL.MimDraw(AIL.M_DEFAULT, AilLocatePeak, AIL.M_NULL, AilValImage, AIL.M_DRAW_INTENSITY_MAP_ROW, n, AIL.M_NULL, AIL.M_FIXED_POINT + NB_FIXED_POINT);

                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref EndTime);
                TotalProcessTime += EndTime - StartTime;

                // Wait to have a proper frame rate.
                WaitTime = (1.0 / FrameRate) - (EndTime - PreviousTime);
                if (WaitTime > 0)
                {
                    AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_WAIT, ref WaitTime);
                }
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref PreviousTime);
            }

            AIL.MgraClear(AIL.M_DEFAULT, AilGraList);

            // Close the sequence file.
            AIL.MbufImportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_CLOSE);

            Console.WriteLine("{0} images processed in {1,7:f2} s ({2,7:f2} ms/image).",
               (int)NumberOfImages, TotalProcessTime,
               TotalProcessTime / NumberOfImages * 1000.0);

            // Pause to show the result.
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            Console.WriteLine("The reconstructed images are being displayed.");

            // Draw extracted peak position in each column of each image.
            int VisualizationDelayMsec = 10;
            for (n = 0; n < NumberOfImages; n++)
            {
                // Display the result image.
                AIL.MbufClear(AilImage, 0);
                AIL.MimDraw(AIL.M_DEFAULT, AilPosYImage, AilValImage, AilImage, AIL.M_DRAW_PEAKS + AIL.M_1D_COLUMNS + AIL.M_LINES, n, 1, AIL.M_FIXED_POINT + NB_FIXED_POINT);
                AIL.MbufCopy(AilImage, AilDisplayImage);

                Thread.Sleep(VisualizationDelayMsec);
            }

            // Pause to show the result.
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Try to allocate 3D display.
            AIL_ID AilDisplay3D = Alloc3dDisplayId(AilSystem);
            if (AilDisplay3D != AIL.M_NULL)
            {
                AIL.McalUniform(AilPosYImage, 0.0, 0.0, M3D_MESH_SCALING_X, M3D_MESH_SCALING_Y, 0.0, AIL.M_DEFAULT);
                AIL.McalControl(AilPosYImage, AIL.M_GRAY_LEVEL_SIZE_Z, M3D_MESH_SCALING_Z);
                AIL_ID ContainerId = AIL.MbufAllocContainer(AilSystem, AIL.M_PROC | AIL.M_DISP, AIL.M_DEFAULT, AIL.M_NULL);
                AIL.MbufConvert3d(AilPosYImage, ContainerId, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT);
                AIL_ID Reflectance = AIL.MbufAllocComponent(ContainerId, 1, SizeX, NumberOfImages, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE, AIL.M_COMPONENT_REFLECTANCE, AIL.M_NULL);
                AIL.MbufCopy(AilValImage, Reflectance);
                Console.WriteLine("The depth buffer is displayed using a 3D display.");
                Console.WriteLine("Press <R> on the display window to stop/start the rotation.");
                Console.WriteLine();

                // Hide display
                AIL.MdispControl(AilDisplay, AIL.M_WINDOW_SHOW, AIL.M_DISABLE);

                AIL.M3ddispSelect(AilDisplay3D, ContainerId, AIL.M_SELECT, AIL.M_DEFAULT);
                AutoRotate3dDisplay(AilDisplay3D);

                // Pause to show the result.
                Console.WriteLine("Press any key to end.");
                Console.ReadKey(true);
                AIL.M3ddispFree(AilDisplay3D);
                AIL.MbufFree(ContainerId);
            }
            else
            {
                Console.WriteLine("The depth buffer is displayed using a 2D display.");

                // Find the remapping for result buffers.
                AIL.MimAlloc(AilSystem, AIL.M_STATISTICS_CONTEXT, AIL.M_DEFAULT, ref AilStatContext);
                AIL.MimAllocResult(AilSystem, AIL.M_DEFAULT, AIL.M_STATISTICS_RESULT, ref AilExtreme);

                AIL.MimControl(AilStatContext, AIL.M_STAT_MIN, AIL.M_ENABLE);
                AIL.MimControl(AilStatContext, AIL.M_STAT_MAX, AIL.M_ENABLE);
                AIL.MimControl(AilStatContext, AIL.M_CONDITION, AIL.M_NOT_EQUAL);
                AIL.MimControl(AilStatContext, AIL.M_COND_LOW, 0xFFFF);


                AIL.MimStatCalculate(AilStatContext, AilPosYImage, AilExtreme, AIL.M_DEFAULT);
                AIL.MimGetResult(AilExtreme, AIL.M_STAT_MIN + AIL.M_TYPE_AIL_INT, ref ExtremePosY[0]);
                AIL.MimGetResult(AilExtreme, AIL.M_STAT_MAX + AIL.M_TYPE_AIL_INT, ref ExtremePosY[1]);

                AIL.MimFree(AilExtreme);
                AIL.MimFree(AilStatContext);

                // Free the display and reallocate a new one of the proper dimension for results.
                AIL.MbufFree(AilDisplayImage);
                AIL.MbufAlloc2d(AilSystem,
                   (AIL_INT)((double)SizeX * (M3D_MESH_SCALING_X > 0 ? M3D_MESH_SCALING_X : -M3D_MESH_SCALING_X)),
                   (AIL_INT)((double)NumberOfImages * M3D_MESH_SCALING_Y),
                   8 + AIL.M_UNSIGNED,
                   AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP,
                   ref AilDisplayImage);

                AIL.MdispSelect(AilDisplay, AilDisplayImage);

                // Display the height buffer.
                AIL.MimClip(AilPosYImage, AilPosYImage, AIL.M_GREATER, ExtremePosY[1], AIL.M_NULL, ExtremePosY[1], AIL.M_NULL);
                AIL.MimArith(AilPosYImage, ExtremePosY[0], AilPosYImage, AIL.M_SUB_CONST);
                AIL.MimArith(AilPosYImage, ((ExtremePosY[1] - ExtremePosY[0]) / 255.0) + 1, AilPosYImage, AIL.M_DIV_CONST);
                AIL.MimResize(AilPosYImage, AilDisplayImage, AIL.M_FILL_DESTINATION, AIL.M_FILL_DESTINATION, AIL.M_BILINEAR);

                // Pause to show the result.
                Console.WriteLine("Press any key to end.");
                Console.ReadKey(true);
            }

            // Free all allocations.
            AIL.MimFree(AilLocatePeak);
            AIL.MimFree(AilContext);
            AIL.MbufFree(AilImage);
            AIL.MgraFree(AilGraList);
            AIL.MbufFree(AilDisplayImage);
            AIL.MbufFree(AilPosYImage);
            AIL.MbufFree(AilValImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);

            return 0;
        }

        // ****************************************************************************
        // Allocates a 3D display and returns its identifier. 
        // ****************************************************************************
        private static AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
        {
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);
            AIL_ID AilDisplay3D = AIL.M3ddispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, AIL.M_NULL);
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE);

            if (AilDisplay3D == AIL.M_NULL)
            {
                Console.WriteLine();
                Console.WriteLine("The current system does not support the 3D display.");
                Console.WriteLine();
            }
            return AilDisplay3D;
        }

        // ***************************************************************************
        // Auto rotate the 3D object
        // ***************************************************************************
        private static void AutoRotate3dDisplay(AIL_ID AilDisplay)
        {
            AIL.M3ddispControl(AilDisplay, AIL.M_AUTO_ROTATE, AIL.M_ENABLE);
        }
    }
}

```
