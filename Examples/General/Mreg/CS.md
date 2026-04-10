---
title: "Mreg"
description: "This program uses the registration module to form a mosaic of three images taken from a camera at unknown translation."
ms.language: "C#"
---

# Mreg

> This program uses the registration module to form a mosaic of three images taken from a camera at unknown translation.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraFree`, `MgraLine`, `MregAlloc`, `MregAllocResult`, `MregCalculate`, `MregControl`, `MregDraw`, `MregFree`, `MregGetResult`, `MregTransformCoordinate`, `MregTransformImage`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Applications, Stitching, Modules, Buffer, Display, Graphics, Registration, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mreg.cs
//
// Description: This program uses the registration module to form a mosaic of 
//              three images taken from a camera at unknown translation.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MReg
{
    class Program
    {
        // Number of images to register.
        private const int NUM_IMAGES_TO_REGISTER = 3;

        // Image file specifications.
        private const string IMAGE_FILES_SOURCE = AIL.M_IMAGE_PATH + "CircuitBoardPart{0}.mim";

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;                             // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                                  // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                                 // Display identifier.
            AIL_ID AilGraphicList = AIL.M_NULL;                             // graphic list identifier.
            AIL_ID[] AilSourceImages = new AIL_ID[NUM_IMAGES_TO_REGISTER];  // Source images buffer identifiers.
            AIL_ID AilMosaicImage = AIL.M_NULL;                             // Mosaic image buffer identifier.
            AIL_ID AilRegContext = AIL.M_NULL;                              // Registration context identifier.
            AIL_ID AilRegResult = AIL.M_NULL;                               // Registration result identifier.
            AIL_INT Result = 0;                                             // Result of the registration.
            AIL_INT MosaicSizeX = 0;                                        // Size of the mosaic.
            AIL_INT MosaicSizeY = 0;
            AIL_INT MosaicSizeBand = 0;                                     // Characteristics of mosaic image.
            AIL_INT MosaicType = 0;

            string[] ImageFilesSource = new string[NUM_IMAGES_TO_REGISTER];

            // Allocate defaults
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Load source image names.
            for (int i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
            {
                ImageFilesSource[i] = String.Format(IMAGE_FILES_SOURCE, i);
            }

            // Print module name.
            Console.Write("\nREGISTRATION MODULE:\n");
            Console.Write("---------------------\n\n");

            // Print comment.
            Console.Write("This program will make a mosaic from many source images.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Restore the source images.
            for (int i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
            {
                AIL.MbufRestore(ImageFilesSource[i], AilSystem, ref AilSourceImages[i]);
            }

            // Display the source images.
            for (int i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
            {
                AIL.MdispSelect(AilDisplay, AilSourceImages[i]);

                // Pause to show each image.
                Console.Write("image {0}.\n", i);
                Console.Write("Press any key to continue.\n\n");
                Console.ReadKey(true);
            }

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref AilGraphicList);
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

            // Allocate a new empty registration context.
            AIL.MregAlloc(AilSystem, AIL.M_STITCHING, AIL.M_DEFAULT, ref AilRegContext);

            // Allocate a new empty registration result buffer.
            AIL.MregAllocResult(AilSystem, AIL.M_DEFAULT, ref AilRegResult);

            // Set the transformation type to translation.
            AIL.MregControl(AilRegContext, AIL.M_CONTEXT, AIL.M_TRANSFORMATION_TYPE, AIL.M_TRANSLATION);

            // By default, each image will be registered with the previous in the list
            // No need to set other location parameters.

            // Set range to 100% in order to search all possible translations.
            AIL.MregControl(AilRegContext, AIL.M_CONTEXT, AIL.M_LOCATION_DELTA, 100);

            // Calculate the registration on the images.
            AIL.MregCalculate(AilRegContext, AilSourceImages, AilRegResult, NUM_IMAGES_TO_REGISTER, AIL.M_DEFAULT);

            // Verify if registration is successful.
            AIL.MregGetResult(AilRegResult, AIL.M_GENERAL, AIL.M_RESULT + AIL.M_TYPE_AIL_INT, ref Result);
            if (Result == AIL.M_SUCCESS)
            {
                // Get the size of the required mosaic buffer.
                AIL.MregGetResult(AilRegResult, AIL.M_GENERAL, AIL.M_MOSAIC_SIZE_X + AIL.M_TYPE_AIL_INT, ref MosaicSizeX);
                AIL.MregGetResult(AilRegResult, AIL.M_GENERAL, AIL.M_MOSAIC_SIZE_Y + AIL.M_TYPE_AIL_INT, ref MosaicSizeY);

                // The mosaic type will be the same as the source images.
                AIL.MbufInquire(AilSourceImages[0], AIL.M_SIZE_BAND, ref MosaicSizeBand);
                AIL.MbufInquire(AilSourceImages[0], AIL.M_TYPE, ref MosaicType);

                // Allocate mosaic image.
                AIL.MbufAllocColor(AilSystem, MosaicSizeBand, MosaicSizeX, MosaicSizeY, MosaicType, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilMosaicImage);

                // Compose the mosaic from the source images.
                AIL.MregTransformImage(AilRegResult, AilSourceImages, AilMosaicImage, NUM_IMAGES_TO_REGISTER, AIL.M_BILINEAR + AIL.M_OVERSCAN_CLEAR, AIL.M_DEFAULT);

                // Display the mosaic image and prepare for overlay annotations.
                AIL.MdispSelect(AilDisplay, AilMosaicImage);
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_RGB888(0, 0xFF, 0));

                // Pause to show the mosaic.
                Console.Write("mosaic image.\n");
                Console.Write("Press any key to continue.\n\n");
                Console.ReadKey(true);

                // Draw the box of all source images in the mosaic.
                AIL.MregDraw(AIL.M_DEFAULT, AilRegResult, AilGraphicList, AIL.M_DRAW_BOX, AIL.M_ALL, AIL.M_DEFAULT);

                // Draw a cross at the center of each image in the mosaic.
                for (int i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
                {
                    // Coordinates of the center of the source image.
                    double SourcePosX = 0.5 * (double)AIL.MbufInquire(AilSourceImages[i], AIL.M_SIZE_X, AIL.M_NULL);
                    double SourcePosY = 0.5 * (double)AIL.MbufInquire(AilSourceImages[i], AIL.M_SIZE_Y, AIL.M_NULL);

                    // Transform the coordinates to the mosaic.
                    double MosaicPosX = 0;
                    double MosaicPosY = 0;
                    AIL.MregTransformCoordinate(AilRegResult, i, AIL.M_MOSAIC, SourcePosX, SourcePosY, ref MosaicPosX, ref MosaicPosY, AIL.M_DEFAULT);
                    int MosaicPosXAilInt = (int)(MosaicPosX + 0.5);
                    int MosaicPosYAilInt = (int)(MosaicPosY + 0.5);

                    // Draw the cross in the mosaic.
                    AIL.MgraLine(AIL.M_DEFAULT, AilGraphicList, MosaicPosXAilInt - 4, MosaicPosYAilInt, MosaicPosXAilInt + 4, MosaicPosYAilInt);
                    AIL.MgraLine(AIL.M_DEFAULT, AilGraphicList, MosaicPosXAilInt, MosaicPosYAilInt - 4, MosaicPosXAilInt, MosaicPosYAilInt + 4);
                }

                Console.Write("The bounding boxes and the center of all source images\n");
                Console.Write("have been drawn in the mosaic.\n");
            }
            else
            {
                Console.Write("Error: Registration was not successful.\n");
            }

            // Pause to show results.
            Console.Write("\nPress any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MgraFree(AilGraphicList);
            if (AilMosaicImage != AIL.M_NULL)
            {
                AIL.MbufFree(AilMosaicImage);
            }

            AIL.MregFree(AilRegContext);
            AIL.MregFree(AilRegResult);

            for (int i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
            {
                AIL.MbufFree(AilSourceImages[i]);
            }

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
