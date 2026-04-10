---
title: "Mreg"
description: "This program uses the registration module to form a mosaic of three images taken from a camera at unknown translation."
ms.language: "C++"
---

# Mreg

> This program uses the registration module to form a mosaic of three images taken from a camera at unknown translation.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraFree`, `MgraLine`, `MregAlloc`, `MregAllocResult`, `MregCalculate`, `MregControl`, `MregDraw`, `MregFree`, `MregGetResult`, `MregTransformCoordinate`, `MregTransformImage`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Applications, Stitching, Modules, Buffer, Display, Graphics, Registration, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mreg.cpp
//
// Description: This program uses the registration module to form a mosaic of 
//              three images taken from a camera at unknown translation.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>  
#include <stdlib.h>  

// Number of images to register.
#define NUM_IMAGES_TO_REGISTER    3

// image file specifications.
#define IMAGE_FILES_SOURCE M_IMAGE_PATH AIL_TEXT("CircuitBoardPart%d.mim")

int MosMain(void)
   {
   AIL_ID      AilApplication,                 // Application identifier.          
               AilSystem,                      // System identifier.               
               AilDisplay,                     // Display identifier.              
               AilGraphicList,                 // Graphic list identifier.         
               AilSourceImages
                     [NUM_IMAGES_TO_REGISTER], // Source images buffer identifiers.
               AilMosaicImage = M_NULL,        // Mosaic image buffer identifier.  
               AilRegContext,                  // Registration context identifier. 
               AilRegResult;                   // Registration result identifier.  
   AIL_INT     i;                              // Iterator.                        
   AIL_INT     Result;                         // Result of the registration.      
   AIL_INT     MosaicSizeX,                    // Size of the mosaic.              
               MosaicSizeY;
   AIL_INT     MosaicSizeBand,                 // Characteristics of mosaic image. 
               MosaicType;
   AIL_TEXT_CHAR ImageFilesSource[NUM_IMAGES_TO_REGISTER][260];
   
   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Load source image names.
   for (i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
      {
      MosSprintf(ImageFilesSource[i], 260, IMAGE_FILES_SOURCE, i);
      }

   // Print module name.
   MosPrintf(AIL_TEXT("\nREGISTRATION MODULE:\n"));
   MosPrintf(AIL_TEXT("---------------------\n\n"));
 
   // Print comment.
   MosPrintf(AIL_TEXT("This program will make a mosaic from many source images.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Restore the source images.
   for (i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
      MbufRestore(ImageFilesSource[i], AilSystem, &AilSourceImages[i]);

   // Display the source images.
   for (i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
      {
      MdispSelect(AilDisplay, AilSourceImages[i]);

      // Pause to show each image.
      MosPrintf(AIL_TEXT("image %ld.\n"), i);
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();
      }

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraphicList);
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

   // Allocate a new empty registration context.
   MregAlloc(AilSystem, M_STITCHING, M_DEFAULT, &AilRegContext);

   // Allocate a new empty registration result buffer.
   MregAllocResult(AilSystem, M_DEFAULT, &AilRegResult);

   // Set the transformation type to translation.
   MregControl(AilRegContext, M_CONTEXT, M_TRANSFORMATION_TYPE,  M_TRANSLATION);

   // By default, each image will be registered with the previous in the list
   // No need to set other location parameters.

   // Set range to 100% in order to search all possible translations.
   MregControl(AilRegContext, M_CONTEXT, M_LOCATION_DELTA, 100);

   // Calculate the registration on the images.
   MregCalculate(AilRegContext, AilSourceImages, AilRegResult, 
                                          NUM_IMAGES_TO_REGISTER, M_DEFAULT);

   // Verify if registration is successful.
   MregGetResult(AilRegResult, M_GENERAL, M_RESULT + M_TYPE_AIL_INT, &Result);
   if( Result == M_SUCCESS )
      {
      // Get the size of the required mosaic buffer.
      MregGetResult(AilRegResult, M_GENERAL, M_MOSAIC_SIZE_X + M_TYPE_AIL_INT, 
                                             &MosaicSizeX);
      MregGetResult(AilRegResult, M_GENERAL, M_MOSAIC_SIZE_Y + M_TYPE_AIL_INT, 
                                             &MosaicSizeY);

      // The mosaic type will be the same as the source images.
      MbufInquire(AilSourceImages[0], M_SIZE_BAND, &MosaicSizeBand);
      MbufInquire(AilSourceImages[0], M_TYPE     , &MosaicType    );

      // Allocate mosaic image.
      MbufAllocColor(AilSystem, MosaicSizeBand, MosaicSizeX, MosaicSizeY, MosaicType, 
                                              M_IMAGE+M_PROC+M_DISP, &AilMosaicImage);

      // Compose the mosaic from the source images.
      MregTransformImage(AilRegResult, AilSourceImages, AilMosaicImage, 
                         NUM_IMAGES_TO_REGISTER, M_BILINEAR+M_OVERSCAN_CLEAR, M_DEFAULT);

      // Display the mosaic image and prepare for overlay annotations.
      MdispSelect(AilDisplay, AilMosaicImage);
      MgraControl(M_DEFAULT, M_COLOR, M_RGB888(0, 0xFF, 0));

      // Pause to show the mosaic.
      MosPrintf(AIL_TEXT("mosaic image.\n"));
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Draw the box of all source images in the mosaic.
      MregDraw(M_DEFAULT, AilRegResult, AilGraphicList, M_DRAW_BOX, M_ALL, M_DEFAULT);

      // Draw a cross at the center of each image in the mosaic.
      for (i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
         {
         AIL_DOUBLE  SourcePosX,
                     SourcePosY,
                     MosaicPosX,
                     MosaicPosY;
         AIL_INT     MosaicPosXAilInt,
                     MosaicPosYAilInt;

         // Coordinates of the center of the source image.
         SourcePosX = 0.5 * (AIL_DOUBLE)MbufInquire(AilSourceImages[i], M_SIZE_X, M_NULL);
         SourcePosY = 0.5 * (AIL_DOUBLE)MbufInquire(AilSourceImages[i], M_SIZE_Y, M_NULL);

         // Transform the coordinates to the mosaic.
         MregTransformCoordinate(AilRegResult, i, M_MOSAIC, SourcePosX, SourcePosY, 
                                                  &MosaicPosX, &MosaicPosY, M_DEFAULT);
         MosaicPosXAilInt = (AIL_INT)(MosaicPosX + 0.5);
         MosaicPosYAilInt = (AIL_INT)(MosaicPosY + 0.5);

         // Draw the cross in the mosaic.
         MgraLine(M_DEFAULT, AilGraphicList, MosaicPosXAilInt - 4, MosaicPosYAilInt    , 
                                             MosaicPosXAilInt + 4, MosaicPosYAilInt    );
         MgraLine(M_DEFAULT, AilGraphicList, MosaicPosXAilInt    , MosaicPosYAilInt - 4, 
                                             MosaicPosXAilInt    , MosaicPosYAilInt + 4);
         }

      MosPrintf(AIL_TEXT("The bounding boxes and the center of all source images\n"));
      MosPrintf(AIL_TEXT("have been drawn in the mosaic.\n"));
      }
   else
      {
      MosPrintf(AIL_TEXT("Error: Registration was not successful.\n"));
      }

   // Pause to show results.
   MosPrintf(AIL_TEXT("\nPress any key to end.\n\n"));
   MosGetch();

   // Free all allocations.
   MgraFree(AilGraphicList);
   if (AilMosaicImage != M_NULL)
      MbufFree(AilMosaicImage);
   MregFree(AilRegContext);
   MregFree(AilRegResult);
   for (i = 0; i < NUM_IMAGES_TO_REGISTER; i++)
      MbufFree(AilSourceImages[i]);

   // Free defaults.
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }

```
