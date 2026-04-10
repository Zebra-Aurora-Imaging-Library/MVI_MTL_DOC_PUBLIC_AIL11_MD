---
title: "MbufColor"
description: "This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result."
ms.language: "C++"
---

# MbufColor

> This example demonstrates color buffer manipulations. It first loads a color image and adds color annotations. If image processing is available, it converts the image to Hue, Luminance and Saturation (HLS), adds a certain offset to the luminance component and converts the image back to Red, Green, Blue (RGB) to display the result.

**Language:** C++

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufChild2d`, `MbufChildColor`, `MbufClear`, `MbufDiskInquire`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispSelect`, `MgraText`, `MimArith`, `MimConvert`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MBufColor.cpp
//
// Description: This program demonstrates color buffer manipulations. It allocates 
//              a displayable color image buffer, displays it, and loads a color   
//              image into the left part. After that, color annotations are done 
//              in each band of the color buffer. Finally, to increase the image
//              luminance, the image is converted to Hue, Saturation and Luminance
//              (HSL), a certain offset is added to the luminance component and 
//              the image is converted back to Red, Green, Blue (RGB) into the 
//              right part to display the result. 
//           
//              The example also demonstrates how to display multiple images 
//              in a single display using child buffers.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h> 

// Source image file specifications.  
#define IMAGE_FILE              M_IMAGE_PATH AIL_TEXT("Bird.mim")

// Luminance offset to add to the image. 
#define IMAGE_LUMINANCE_OFFSET  40L

// Main function. 
int MosMain(void)
{ 
   AIL_ID  AilApplication,       // Application identifier.
           AilSystem,            // System identifier.     
           AilDisplay,           // Display identifier.    
           AilImage,             // Image buffer identifier.
           AilLeftSubImage,      // Sub-image buffer identifier for original image. 
           AilRightSubImage,     // Sub-image buffer identifier for processed image.
           AilLumSubImage=0,     // Sub-image buffer identifier for luminance.      
           AilRedBandSubImage,   // Sub-image buffer identifier for red component.  
           AilGreenBandSubImage, // Sub-image buffer identifier for green component.
           AilBlueBandSubImage;  // Sub-image buffer identifier for blue component. 
   AIL_INT SizeX, SizeY, SizeBand, Type;

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Allocate a color display buffer twice the size of the source image and display it. 
   MbufAllocColor(AilSystem,
                  MbufDiskInquire(IMAGE_FILE, M_SIZE_BAND, &SizeBand),
                  MbufDiskInquire(IMAGE_FILE, M_SIZE_X, &SizeX) * 2,
                  MbufDiskInquire(IMAGE_FILE, M_SIZE_Y, &SizeY),
                  MbufDiskInquire(IMAGE_FILE, M_TYPE,   &Type),
                  M_IMAGE+M_DISP+M_PROC, &AilImage);
   MbufClear(AilImage, 0L);
   MdispSelect(AilDisplay, AilImage);

   // Define 2 child buffers that maps to the left and right part of the display 
   // buffer, to put the source and destination color images.
   MbufChild2d(AilImage, 0L, 0L, SizeX, SizeY, &AilLeftSubImage);
   MbufChild2d(AilImage, SizeX, 0L, SizeX, SizeY, &AilRightSubImage);

   // Load the color source image on the left. 
   MbufLoad(IMAGE_FILE, AilLeftSubImage);
      
   // Define child buffers that map to the red, green and blue components
   // of the source image.
   MbufChildColor(AilLeftSubImage, M_RED,   &AilRedBandSubImage);
   MbufChildColor(AilLeftSubImage, M_GREEN, &AilGreenBandSubImage);
   MbufChildColor(AilLeftSubImage, M_BLUE,  &AilBlueBandSubImage);

   // Write color text annotations to show access in each individual band of the image.

   // Note that this is typically done more simply by using:
   // MgraControl(M_DEFAULT, M_COLOR, M_RGB888(0xFF,0x90,0x00));
   // MgraText(M_DEFAULT, AilLeftSubImage, ...);
   MgraControl(M_DEFAULT, M_COLOR, 0xFF);
   MgraText(M_DEFAULT, AilRedBandSubImage,   SizeX/16, SizeY/8, AIL_TEXT(" TOUCAN "));
   MgraControl(M_DEFAULT, M_COLOR, 0x90);
   MgraText(M_DEFAULT, AilGreenBandSubImage, SizeX/16, SizeY/8, AIL_TEXT(" TOUCAN "));
   MgraControl(M_DEFAULT, M_COLOR, 0x00);
   MgraText(M_DEFAULT, AilBlueBandSubImage,  SizeX/16, SizeY/8, AIL_TEXT(" TOUCAN "));
 
   // Print a message. 
   MosPrintf(AIL_TEXT("\nCOLOR OPERATIONS:\n"));
   MosPrintf(AIL_TEXT("-----------------\n\n"));
   MosPrintf(AIL_TEXT("A color source image was loaded on the left and color text\n"));
   MosPrintf(AIL_TEXT("annotations were written in it.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Convert image to Hue, Saturation, Luminance color space (HSL). 
   MimConvert(AilLeftSubImage, AilRightSubImage, M_RGB_TO_HSL);
  
   // Create a child buffer that maps to the luminance component. 
   MbufChildColor(AilRightSubImage, M_LUMINANCE, &AilLumSubImage);
     
   // Add an offset to the luminance component. 
   MimArith(AilLumSubImage, IMAGE_LUMINANCE_OFFSET, AilLumSubImage, 
                                                      M_ADD_CONST+M_SATURATION);
  
   // Convert image back to Red, Green, Blue color space (RGB) for display. 
   MimConvert(AilRightSubImage, AilRightSubImage, M_HSL_TO_RGB); 

   // Print a message. 
   MosPrintf(AIL_TEXT("Luminance was increased using color image processing.\n"));
  
   // Print a message. 
   MosPrintf(AIL_TEXT("Press any key to end.\n"));
   MosGetch();

   // Release sub-images and color image buffer. 
   MbufFree(AilLumSubImage);
   MbufFree(AilRedBandSubImage);
   MbufFree(AilGreenBandSubImage);
   MbufFree(AilBlueBandSubImage);
   MbufFree(AilRightSubImage);
   MbufFree(AilLeftSubImage);
   MbufFree(AilImage);

   // Release defaults. 
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}


```
