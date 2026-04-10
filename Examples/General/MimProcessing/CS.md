---
title: "MimProcessing"
description: "This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample."
ms.language: "C#"
---

# MimProcessing

> This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MappInquire`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispLut`, `MdispSelect`, `MimAllocResult`, `MimArith`, `MimBinarize`, `MimClose`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MimLabel`, `MimOpen`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Image Processing, What's New, AIL 10.0 SP6, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImProcessing.cs
//
// Description: This program show the usage of image processing. Under Aurora Imaging Library Lite, 
//              it binarizes two images to isolate specific zones.
//              Under Aurora Imaging Library full, it also uses different image processing primitives 
//              to determine the number of cell nuclei that are larger than a 
//              certain size and show them in pseudo-color.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MImProcessing
{
    class Program
    {
        // Target image file specifications.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "Cell.mbufi";
        private const string IMAGE_CUP = AIL.M_IMAGE_PATH + "PlasticCup.mim";
        private const int IMAGE_SMALL_PARTICLE_RADIUS = 1;

        static void ExtractParticlesExample(AIL_ID AilApplication, AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilRemoteApplication = AIL.M_NULL;   // Remote Application identifier if running on a remote computer
            AIL_ID AilImage = AIL.M_NULL;               // Image buffer identifier.
            AIL_ID AilExtremeResult = 0;                // Extreme result buffer identifier.
            AIL_INT MaxLabelNumber = 0;                 // Highest label value.
            AIL_INT LicenseModules = 0;                 // List of available modules.

            // Restore source image and display it.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Pause to show the original image.
            Console.Write("\n1) Particles extraction:\n");
            Console.Write("-----------------\n\n");
            Console.Write("This first example extracts the dark particles in an image.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Binarize the image with an automatically calculated threshold so that 
            // particles are represented in white and the background removed.
            AIL.MimBinarize(AilImage, AilImage, AIL.M_BIMODAL + AIL.M_LESS_OR_EQUAL, AIL.M_NULL, AIL.M_NULL);

            // Print a message for the extracted particles.
            Console.Write("These particles were extracted from the original image.\n");

            // if Aurora Imaging Library IM module is available, count and label the larger particles.
            AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION, ref AilRemoteApplication);
            AIL.MappInquire(AilRemoteApplication, AIL.M_LICENSE_MODULES, ref LicenseModules);

            if ((LicenseModules & AIL.M_LICENSE_IM) != 0)
            {
                // Pause to show the extracted particles.
                Console.Write("Press any key to continue.\n\n");
                Console.ReadKey(true);

                // Close small holes.
                AIL.MimClose(AilImage, AilImage, IMAGE_SMALL_PARTICLE_RADIUS, AIL.M_BINARY);

                // Remove small particles.
                AIL.MimOpen(AilImage, AilImage, IMAGE_SMALL_PARTICLE_RADIUS, AIL.M_BINARY);

                // Label the image.
                AIL.MimLabel(AilImage, AilImage, AIL.M_DEFAULT);

                // The largest label value corresponds to the number of particles in the image.
                AIL.MimAllocResult(AilSystem, 1, AIL.M_EXTREME_LIST, ref AilExtremeResult);
                AIL.MimFindExtreme(AilImage, AilExtremeResult, AIL.M_MAX_VALUE);
                AIL.MimGetResult(AilExtremeResult, AIL.M_VALUE, ref MaxLabelNumber);
                AIL.MimFree(AilExtremeResult);

                // Multiply the labeling result to augment the gray level of the particles.
                AIL.MimArith(AilImage, (int)(256 / (double)MaxLabelNumber), AilImage, AIL.M_MULT_CONST);

                // Display the resulting particles in pseudo-color.
                AIL.MdispLut(AilDisplay, AIL.M_PSEUDO);

                // Print results.
                Console.Write("There were {0} large particles in the original image.\n", MaxLabelNumber);
            }

            // Pause to show the result.
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            /* Reset AilDisplay to M_DEFAULT. */
            AIL.MdispLut(AilDisplay, AIL.M_DEFAULT);

            // Free all allocations.
            AIL.MbufFree(AilImage);
        }
        static void ExtractForegroundExample(AIL_ID AilApplication, AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;               // Image buffer identifier.
            
            // Restore source image and display it.
            AIL.MbufRestore(IMAGE_CUP, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Pause to show the original image.
            Console.Write("\n2) Background removal:\n");
            Console.Write("-----------------\n\n");
            Console.Write("This second example separates a cup on a table from the background using MimBinarize() with an M_DOMINANT mode.\n");
            Console.Write("In this case, the dominant mode (black background) is separated from the rest. Note, using an M_BIMODAL mode\n");
            Console.Write("would give another result because the background and the cup would be considered as the same mode.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Binarize the image with an automatically calculated threshold so that 
            // particles are represented in white and the background removed.
            AIL.MimBinarize(AilImage, AilImage, AIL.M_DOMINANT + AIL.M_LESS_OR_EQUAL, AIL.M_NULL, AIL.M_NULL);

            // Print a message for the extracted cup and table.
            Console.Write("The cup and the table were separated from the background with M_DOMINANT mode of MimBinarize.\n");

            // Pause to show the result.
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MbufFree(AilImage);
        }

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;         // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;              // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;             // Display identifier.
            
            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Show header
            Console.Write("\nIMAGE PROCESSING:\n");
            Console.Write("-----------------\n\n");
            Console.Write("This program shows two image processing examples.\n");

            // Example about extracting particles in an image 
            ExtractParticlesExample(AilApplication, AilSystem, AilDisplay);

            // Example about isolating objects from the background in an image
            ExtractForegroundExample(AilApplication, AilSystem, AilDisplay);

            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
