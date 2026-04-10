---
title: "MimProcessing"
description: "This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample."
ms.language: "C++"
---

# MimProcessing

> This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MappInquire`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispLut`, `MdispSelect`, `MimAllocResult`, `MimArith`, `MimBinarize`, `MimClose`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MimLabel`, `MimOpen`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Image Processing, What's New, AIL 10.0 SP6, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImProcessing.cpp
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

#include <AIL.h>

// Target image file specifications. 
#define IMAGE_FILE                     M_IMAGE_PATH AIL_TEXT("Cell.mbufi")
#define IMAGE_CUP                      M_IMAGE_PATH AIL_TEXT("PlasticCup.mim")
#define IMAGE_SMALL_PARTICLE_RADIUS    1

void ExtractParticlesExample(AIL_ID AilApplication, AIL_ID AilSystem, AIL_ID AilDisplay);
void ExtractForegroundExample(AIL_ID AilApplication, AIL_ID AilSystem, AIL_ID AilDisplay);

int MosMain(void)
{
   AIL_ID   AilApplication,      // Application identifier.
            AilSystem,           // System identifier.     
            AilDisplay;          // Display identifier.    

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem,
                    &AilDisplay, M_NULL, M_NULL);

   // Show header 
   MosPrintf(AIL_TEXT("\nIMAGE PROCESSING:\n"));
   MosPrintf(AIL_TEXT("-----------------\n\n"));
   MosPrintf(AIL_TEXT("This program shows two image processing examples.\n"));

   // Example about extracting particles in an image 
   ExtractParticlesExample(AilApplication, AilSystem, AilDisplay);

   // Example about isolating objects from the background in an image 
   ExtractForegroundExample(AilApplication, AilSystem, AilDisplay);

   // Free all allocations. 
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}

void ExtractParticlesExample(AIL_ID AilApplication, AIL_ID AilSystem, AIL_ID AilDisplay)
{
   AIL_ID   AilImage,             // Image buffer identifier. 
            AilExtremeResult = 0; // Extreme result buffer identifier. 

#if (!M_AIL_LITE)
   AIL_ID   AilRemoteApplication = 0; // Remote application identifier.
#endif

   AIL_INT  MaxLabelNumber = 0;   // Highest label value.
   AIL_INT  LicenseModules = 0;   // List of available modules

   // Restore source image and display it. 
   MbufRestore(IMAGE_FILE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Pause to show the original image. 
   MosPrintf(AIL_TEXT("\n1) Particles extraction:\n"));
   MosPrintf(AIL_TEXT("-----------------\n\n"));
   MosPrintf(AIL_TEXT("This first example extracts the dark particles in an image.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Binarize the image with an automatically calculated threshold so that
   // particles are represented in white and the background removed.
   MimBinarize(AilImage, AilImage, M_BIMODAL + M_LESS_OR_EQUAL, M_NULL, M_NULL);

   // Print a message for the extracted particles. 
   MosPrintf(AIL_TEXT("These particles were extracted from the original image.\n"));

#if (!M_AIL_LITE)
   // If Aurora Imaging Library IM module is available, count and label the larger particles. 
   MsysInquire(AilSystem, M_OWNER_APPLICATION, &AilRemoteApplication);
   MappInquire(AilRemoteApplication, M_LICENSE_MODULES, &LicenseModules);
   if(LicenseModules & M_LICENSE_IM)
   {
      // Pause to show the extracted particles. 
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Close small holes. 
      MimClose(AilImage, AilImage, IMAGE_SMALL_PARTICLE_RADIUS, M_BINARY);

      // Remove small particles. 
      MimOpen(AilImage, AilImage, IMAGE_SMALL_PARTICLE_RADIUS, M_BINARY);

      // Label the image. 
      MimLabel(AilImage, AilImage, M_DEFAULT);

      // The largest label value corresponds to the number of particles in the image.
      MimAllocResult(AilSystem, 1L, M_EXTREME_LIST, &AilExtremeResult);
      MimFindExtreme(AilImage, AilExtremeResult, M_MAX_VALUE);
      MimGetResult(AilExtremeResult, M_VALUE, &MaxLabelNumber);
      MimFree(AilExtremeResult);

      // Multiply the labeling result to augment the gray level of the particles. 
      MimArith(AilImage, (AIL_INT)(256L / (AIL_DOUBLE)MaxLabelNumber), AilImage,
               M_MULT_CONST);

      // Display the resulting particles in pseudo-color. 
      MdispLut(AilDisplay, M_PSEUDO);

      // Print results. 
      MosPrintf(AIL_TEXT("There were %d large particles in the original image.\n"),
                (int)MaxLabelNumber);
   }
#endif

   // Pause to show the result. 
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Reset AilDisplay to M_DEFAULT. 
   MdispLut(AilDisplay, M_DEFAULT);

   // Free all allocations. 
   MbufFree(AilImage);
}

void ExtractForegroundExample(AIL_ID AilApplication, AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID   AilImage,             // Image buffer identifier. 
            AilExtremeResult = 0; // Extreme result buffer identifier. 
   AIL_INT  MaxLabelNumber = 0;   // Highest label value. 
   AIL_INT  LicenseModules = 0;   // List of available modules 

   // Restore source image and display it. 
   MbufRestore(IMAGE_CUP, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Pause to show the original image. 
   MosPrintf(AIL_TEXT("\n2) Background removal:\n"));
   MosPrintf(AIL_TEXT("-----------------\n\n"));
   MosPrintf(AIL_TEXT("This second example separates a cup on a table from the background using MimBinarize() with an M_DOMINANT mode.\n"));
   MosPrintf(AIL_TEXT("In this case, the dominant mode (black background) is separated from the rest. Note, using an M_BIMODAL mode\n"));
   MosPrintf(AIL_TEXT("would give another result because the background and the cup would be considered as the same mode.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Binarize the image with an automatically calculated threshold so that
   // cup and table are represented in white and the background removed.
   MimBinarize(AilImage, AilImage, M_DOMINANT + M_LESS_OR_EQUAL, M_NULL, M_NULL);

   // Print a message for the extracted cup and table. 
   MosPrintf(AIL_TEXT("The cup and the table were separated from the background with M_DOMINANT mode of MimBinarize.\n"));
      
   // Pause to show the result. 
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   // Free all allocations. 
   MbufFree(AilImage);
   }


```
