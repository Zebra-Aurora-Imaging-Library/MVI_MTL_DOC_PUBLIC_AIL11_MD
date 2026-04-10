---
title: "Mblob"
description: "This example uses the Blob module to analyze an image of some nuts and bolts."
ms.language: "C++"
---

# Mblob

> This example uses the Blob module to analyze an image of some nuts and bolts.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobGetResult`, `MblobSelect`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraFree`, `MimBinarize`, `MimClose`, `MimOpen`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Graphics, Image Processing, Blob Analysis, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MBlob.cpp
//
// Description: This program loads an image of some nuts, bolts and washers, 
//              determines the number of each of these, finds and marks
//              their center of gravity using the Blob analysis module.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>  
 
// Target image file specifications.   
#define IMAGE_FILE            M_IMAGE_PATH AIL_TEXT("BoltsNutsWashers.mim")
#define IMAGE_THRESHOLD_VALUE 26L 

// Minimum and maximum area of blobs. 
#define MIN_BLOB_AREA         50L 
#define MAX_BLOB_AREA         50000L

// Radius of the smallest particles to keep. 
#define MIN_BLOB_RADIUS       3L

// Minimum hole compactness corresponding to a washer. 
#define MIN_COMPACTNESS       1.5


int MosMain(void)
{ 
   AIL_ID     AilApplication,                 // Application identifier.           
              AilSystem,                      // System identifier.                
              AilDisplay,                     // Display identifier.               
              AilImage,                       // Image buffer identifier.          
              AilGraphicList,                 // Graphic list identifier.          
              AilBinImage,                    // Binary image buffer identifier.   
              AilBlobResult,                  // Blob result buffer identifier.    
              AilBlobContext;                 // Blob Context identifier.          
   AIL_INT    TotalBlobs,                     // Total number of blobs.            
              BlobsWithHoles,                 // Number of blobs with holes.       
              BlobsWithRoughHoles,            // Number of blobs with rough holes. 
              n,                              // Counter.                          
              SizeX,                          // Size X of the source buffer.      
              SizeY;                          // Size Y of the source buffer.       
                                               
   AIL_DOUBLE *CogX,                          // X coordinate of center of gravity.
              *CogY;                          // Y coordinate of center of gravity.
    
   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Restore source image into image buffer.  
   MbufRestore(IMAGE_FILE, AilSystem, &AilImage);

   // Allocate a graphic list to hold the subpixel annotations to draw. 
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraphicList);

   // Associate the graphic list to the display. 
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

   // Display the buffer. 
   MdispSelect(AilDisplay, AilImage);

   // Allocate a binary image buffer for fast processing. 
   MbufInquire(AilImage, M_SIZE_X, &SizeX);
   MbufInquire(AilImage, M_SIZE_Y, &SizeY);
   MbufAlloc2d(AilSystem, SizeX, SizeY, 1+M_UNSIGNED, M_IMAGE+M_PROC, &AilBinImage);

   // Pause to show the original image.  
   MosPrintf(AIL_TEXT("\nBLOB ANALYSIS:\n"));
   MosPrintf(AIL_TEXT("--------------\n\n"));
   MosPrintf(AIL_TEXT("This program determines the number of bolts, nuts and washers\n"));
   MosPrintf(AIL_TEXT("in the image and finds their center of gravity.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();
 
   // Binarize image. 
   MimBinarize(AilImage, AilBinImage, M_FIXED+M_GREATER_OR_EQUAL, 
                                      IMAGE_THRESHOLD_VALUE, M_NULL);

   // Remove small particles and then remove small holes. 
   MimOpen(AilBinImage, AilBinImage, MIN_BLOB_RADIUS, M_BINARY);
   MimClose(AilBinImage, AilBinImage, MIN_BLOB_RADIUS, M_BINARY);
 
   // Allocate a context.  
   MblobAlloc(AilSystem, M_DEFAULT, M_DEFAULT, &AilBlobContext);
  
   // Enable the Center Of Gravity feature calculation.  
   MblobControl(AilBlobContext, M_CENTER_OF_GRAVITY + M_BINARY, M_ENABLE);
 
   // Allocate a blob result buffer. 
   MblobAllocResult(AilSystem, M_DEFAULT, M_DEFAULT, &AilBlobResult); 
 
   // Calculate selected features for each blob.  
   MblobCalculate(AilBlobContext, AilBinImage, M_NULL, AilBlobResult);
 
   // Exclude blobs whose area is too small.  
   MblobSelect(AilBlobResult, M_EXCLUDE, M_AREA, M_LESS_OR_EQUAL, 
                                                 MIN_BLOB_AREA, M_NULL); 
 
   // Get the total number of selected blobs.  
   MblobGetResult(AilBlobResult, M_DEFAULT, M_NUMBER + M_TYPE_AIL_INT, &TotalBlobs);
   MosPrintf(AIL_TEXT("There are %d objects "), (int) TotalBlobs); 
  
   // Read and print the blob's center of gravity.  
   if ((CogX = new AIL_DOUBLE[TotalBlobs]) && 
       (CogY = new AIL_DOUBLE[TotalBlobs]))
      { 
      // Get the results. 
      MblobGetResult(AilBlobResult, M_DEFAULT, M_CENTER_OF_GRAVITY_X + M_BINARY, CogX);
      MblobGetResult(AilBlobResult, M_DEFAULT, M_CENTER_OF_GRAVITY_Y + M_BINARY, CogY);
    
      // Print the center of gravity of each blob.      
      MosPrintf(AIL_TEXT("and their centers of gravity are:\n")); 
      for(n=0; n < TotalBlobs; n++)
         MosPrintf(AIL_TEXT("Blob #%d: X=%5.1f, Y=%5.1f\n"), (int) n, CogX[n], CogY[n]);

      delete [] CogX; CogX = NULL;
      delete [] CogY; CogY = NULL;
      }
   else
      MosPrintf(AIL_TEXT("\nError: Not enough memory.\n")); 
  
   // Draw a cross at the center of gravity of each blob.   
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);
   MblobDraw(M_DEFAULT, AilBlobResult, AilGraphicList, M_DRAW_CENTER_OF_GRAVITY, 
                                                       M_INCLUDED_BLOBS, M_DEFAULT);

   // Reverse what is considered to be the background so that
   // holes are seen as being blobs. 
   MblobControl(AilBlobContext, M_FOREGROUND_VALUE, M_ZERO);

   // Add a feature to distinguish between types of holes. Since area
   // has already been added to the context, and the processing 
   // mode has been changed, all blobs will be re-included and the area 
   // of holes will be calculated automatically.
   MblobControl(AilBlobContext, M_COMPACTNESS, M_ENABLE);

   // Calculate selected features for each blob. 
   MblobCalculate(AilBlobContext, AilBinImage, M_NULL, AilBlobResult);

   // Exclude small holes and large (the area around objects) holes. 
   MblobSelect(AilBlobResult, M_EXCLUDE, M_AREA, M_OUT_RANGE, 
                                         MIN_BLOB_AREA, MAX_BLOB_AREA);

   // Get the number of blobs with holes. 
   MblobGetResult(AilBlobResult, M_DEFAULT, M_NUMBER + M_TYPE_AIL_INT, &BlobsWithHoles);

   // Exclude blobs whose holes are compact (i.e. nuts). 
   MblobSelect(AilBlobResult, M_EXCLUDE, M_COMPACTNESS, M_LESS_OR_EQUAL, 
                                                        MIN_COMPACTNESS, M_NULL);

   // Get the number of blobs with holes that are NOT compact. 
   MblobGetResult(AilBlobResult, M_DEFAULT, M_NUMBER + M_TYPE_AIL_INT, &BlobsWithRoughHoles);

   // Print results. 
   MosPrintf(AIL_TEXT("\nIdentified objects:\n"));
   MosPrintf(AIL_TEXT("%d bolts\n"), (int) (TotalBlobs-BlobsWithHoles));
   MosPrintf(AIL_TEXT("%d nuts\n"), (int) (BlobsWithHoles - BlobsWithRoughHoles));
   MosPrintf(AIL_TEXT("%d washers\n\n"), (int) (BlobsWithRoughHoles));
   MosPrintf(AIL_TEXT("Press any key to end.\n\n")); 
   MosGetch();

   // Free all allocations. 
   MgraFree(AilGraphicList);
   MblobFree(AilBlobResult); 
   MblobFree(AilBlobContext);
   MbufFree(AilBinImage);
   MbufFree(AilImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}

```
