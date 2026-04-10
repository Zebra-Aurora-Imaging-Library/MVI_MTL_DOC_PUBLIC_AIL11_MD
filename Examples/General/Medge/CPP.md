---
title: "Medge"
description: "This program uses the AIL Edge Finder module to find and measure the outer diameter of good seals in a target image."
ms.language: "C++"
---

# Medge

> This program uses the AIL Edge Finder module to find and measure the outer diameter of good seals in a target image.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MedgeAlloc`, `MedgeAllocResult`, `MedgeCalculate`, `MedgeControl`, `MedgeDraw`, `MedgeFree`, `MedgeGetResult`, `MedgeSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Counting, Finding/Locating, Measuring, Modules, Buffer, Display, Graphics, Edge Finder, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Medge.cpp
//
// Description: This program uses the Edge Finder module to find and measure
//              the outer diameter of good seals in a target image. 
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Source image file specifications. 
#define CONTOUR_IMAGE               M_IMAGE_PATH AIL_TEXT("Seals.mim")
#define CONTOUR_MAX_RESULTS         100
#define CONTOUR_MAXIMUM_ELONGATION  0.8
#define CONTOUR_DRAW_COLOR          M_COLOR_GREEN
#define CONTOUR_LABEL_COLOR         M_COLOR_RED


 // Main function. 
int MosMain(void)
   {
   AIL_ID      AilApplication,                         // Application identifier.  
               AilSystem,                              // System Identifier.       
               AilDisplay,                             // Display identifier.      
               AilImage,                               // Image buffer identifier. 
               GraphicList,                            // Graphic list identifier. 
               AilEdgeContext,                         // Edge context.             
               AilEdgeResult;                          // Edge result identifier.     
   AIL_DOUBLE  EdgeDrawColor = CONTOUR_DRAW_COLOR,     // Edge draw color.         
               LabelDrawColor = CONTOUR_LABEL_COLOR;   // Text draw color.         
   AIL_INT     NumEdgeFound  = 0L,                     // Number of edges found.   
               NumResults    = 0L,                     // Number of results found.    
               i;                                      // Loop variable.           
   AIL_DOUBLE MeanFeretDiameter[CONTOUR_MAX_RESULTS];  // Edge mean Feret diameter.

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Restore the image and display it. 
   MbufRestore(CONTOUR_IMAGE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Allocate a graphic list to hold the subpixel annotations to draw. 
   MgraAllocList(AilSystem, M_DEFAULT, &GraphicList);

   // Associate the graphic list to the display for annotations. 
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

   // Pause to show the original image.  
   MosPrintf(AIL_TEXT("\nEDGE MODULE:\n"));  
   MosPrintf(AIL_TEXT("------------\n\n"));  
   MosPrintf(AIL_TEXT("This program determines the outer seal diameters ")
             AIL_TEXT("in the displayed image \n"));
   MosPrintf(AIL_TEXT("by detecting and analyzing contours ")
             AIL_TEXT("with the Edge Finder module.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));  
   MosGetch();
   
   // Allocate a Edge Finder context. 
   MedgeAlloc(AilSystem, M_CONTOUR, M_DEFAULT, &AilEdgeContext); 

   // Allocate a result buffer. 
   MedgeAllocResult(AilSystem, M_DEFAULT, &AilEdgeResult);

   // Enable features to compute. 
   MedgeControl(AilEdgeContext, M_MOMENT_ELONGATION,                M_ENABLE);
   MedgeControl(AilEdgeContext, M_FERET_MEAN_DIAMETER+M_SORT1_DOWN, M_ENABLE);

   // Calculate edges and features. 
   MedgeCalculate(AilEdgeContext, AilImage, M_NULL, M_NULL, M_NULL, AilEdgeResult, 
                                                                    M_DEFAULT);

   // Get the number of edges found. 
   MedgeGetResult(AilEdgeResult, M_DEFAULT, M_NUMBER_OF_CHAINS+M_TYPE_AIL_INT, 
                                                               &NumEdgeFound, M_NULL);

   // Draw edges in the source image to show the result. 
   MgraControl(M_DEFAULT, M_COLOR, EdgeDrawColor);
   MedgeDraw(M_DEFAULT, AilEdgeResult, GraphicList, M_DRAW_EDGES, M_DEFAULT, 
                                                                      M_DEFAULT);

   // Pause to show the edges. 
   MosPrintf(AIL_TEXT("%d edges were found in the image.\n"), (int) NumEdgeFound);
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));  
   MosGetch();
 
   // Exclude elongated edges. 
   MedgeSelect(AilEdgeResult, M_EXCLUDE, M_MOMENT_ELONGATION, 
                                         M_LESS, CONTOUR_MAXIMUM_ELONGATION, M_NULL);   

   // Exclude inner chains. 
   MedgeSelect(AilEdgeResult, M_EXCLUDE, M_INCLUDED_EDGES, M_INSIDE_BOX, M_NULL, M_NULL);

   // Draw remaining edges and their index to show the result. 
   MgraClear(M_DEFAULT, GraphicList);
   MgraControl(M_DEFAULT, M_COLOR, EdgeDrawColor);
   MedgeDraw(M_DEFAULT, AilEdgeResult, GraphicList, M_DRAW_EDGES, M_DEFAULT,
                                                                      M_DEFAULT);

   // Pause to show the results. 
   MosPrintf(AIL_TEXT("Elongated edges and inner edges of each seal were removed.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();
      
   // Get the number of edges found. 
   MedgeGetResult(AilEdgeResult, M_DEFAULT, M_NUMBER_OF_CHAINS+M_TYPE_AIL_INT, 
                                                                  &NumResults, M_NULL);

   // If the right number of edges were found. 
   if ( (NumResults >= 1) && (NumResults <= CONTOUR_MAX_RESULTS) )
      {
      // Draw the index of each edge. 
      MgraControl(M_DEFAULT, M_COLOR, LabelDrawColor);
      MedgeDraw(M_DEFAULT, AilEdgeResult, GraphicList, M_DRAW_INDEX, 
                                                                 M_DEFAULT, M_DEFAULT);  

      // Get the mean Feret diameters.       
      MedgeGetResult(AilEdgeResult, M_DEFAULT, M_FERET_MEAN_DIAMETER, 
                                                            MeanFeretDiameter, M_NULL);

      // Print the results. 
      MosPrintf(AIL_TEXT("Mean diameter of the %d outer edges are:\n\n"), (int) NumResults);
      MosPrintf(AIL_TEXT("Index   Mean diameter \n"));
      for (i=0; i<NumResults; i++)
         {
         MosPrintf(AIL_TEXT("%-11d%-13.2f\n"), (int) i, MeanFeretDiameter[i]);
         }
      }
   else
      {
      MosPrintf(AIL_TEXT("Edges have not been found or the number of ")
                              AIL_TEXT("found edges is greater than\n"));
      MosPrintf(AIL_TEXT("the specified maximum number of edges !\n\n"));
      }
    
   // Wait for a key press.  
   MosPrintf(AIL_TEXT("\nPress any key to end.\n"));
   MosGetch();

   // Free objects. 
   MgraFree(GraphicList);
   MbufFree(AilImage);
   MedgeFree(AilEdgeContext);
   MedgeFree(AilEdgeResult);

   // Free defaults.     
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }

```
