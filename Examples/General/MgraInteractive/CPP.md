---
title: "MgraInteractive"
description: "This program uses the capabilities of AIL's interactive graphics and extracts the blobs within a user defined region."
ms.language: "C++"
---

# MgraInteractive

> This program uses the capabilities of AIL's interactive graphics and extracts the blobs within a user defined region.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobGetResult`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MbufSetRegion`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAlloc`, `MgraAllocList`, `MgraControl`, `MgraControlList`, `MgraFree`, `MgraGetHookInfo`, `MgraHookFunction`, `MgraInquireList`, `MgraRectAngle`, `MgraText`, `MimBinarize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Finding/Locating, Counting, Modules, Buffer, Display, Graphics, Image Processing, Blob Analysis, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MGraInteractive.cpp
//
// Description: This program uses the capabilities of AIL's interactive
//              graphics, along with the Blob analysis module, to count 
//              objects within a user-defined region.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Data structure to handle parameters for the hook function. 
struct STestParameters
   {
   AIL_ID   AilDisplay,
            AilGraphicsList,
            AilGraphicsContext,
            AilBinImage,
            AilBlobContext,
            AilBlobResult;
   AIL_INT  RegionLabel;
   };

// Example function declarations.  
void           InteractivityExample(AIL_ID AilSystem, AIL_ID AilDisplay);

AIL_INT MFTYPE HookHandler(AIL_INT HookType, AIL_ID EventId, void* UserData);

void           CountObjects(AIL_ID AilDisplay, AIL_ID AilGraphicsList,
                            AIL_ID AilGraphicsContext, AIL_ID AilBinImage,
                            AIL_ID AilBlobContext, AIL_ID AilBlobResult);

//*****************************************************************************
// Main.
//*****************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication,     // Application identifier.
          AilSystem,          // System Identifier.     
          AilDisplay;         // Display identifier.    

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, 
      M_NULL, M_NULL);

   // Print example name.  
   MosPrintf(AIL_TEXT("\nINTERACTIVE REGIONS AND SUBPIXEL ANNOTATIONS:\n"));
   MosPrintf(AIL_TEXT("---------------------------------------------\n\n"));
   MosPrintf(AIL_TEXT("This program determines the number of blobs in a region\n"));
   MosPrintf(AIL_TEXT("defined interactively by the user. The extracted blob's\n"));
   MosPrintf(AIL_TEXT("features are drawn with subpixel accuracy in a zoomable\n"));
   MosPrintf(AIL_TEXT("display.\n\n"));

   // Run Interactivity Example. 
   InteractivityExample(AilSystem, AilDisplay);
 
   // Free defaults.     
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);
   
   return 0;
   }

//***************************************************************************
// Interactivity example. 

// Source image file specification.  
#define IMAGE_FILE M_IMAGE_PATH AIL_TEXT("Seals.mim")
// Threshold for image binarization. 
#define IMAGE_THRESHOLD_VALUE 110L 
// Initial region parameters. 
#define RECTANGLE_POSITION_X 160
#define RECTANGLE_POSITION_Y 310
#define RECTANGLE_WIDTH      200
#define RECTANGLE_HEIGHT     175
#define RECTANGLE_ANGLE      0
// Interactivity parameters. 
#define SELECTION_RADIUS     10
// Value of enter key in Ascii table 
#define ASCII_ENTER 13

void InteractivityExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID  AilImage,                  // Image buffer identifier.        
           AilGraphicsList,           // Graphics list identifier.       
           AilGraphicsContext,        // Graphics context identifier.    
           AilBinImage,               // Binary image buffer identifier. 
           AilBlobContext,        	  // Context identifier.             
           AilBlobResult;             // Blob result buffer identifier.  

   AIL_INT SizeX,                     // Size X of the source buffer.    
           SizeY,                     // Size Y of the source buffer.    
           RegionLabel;               // Label value of the region.      

   STestParameters DataStructure;     // Hook function data structure.   
   
   // Restore the source image. 
   MbufRestore(IMAGE_FILE, AilSystem, &AilImage);

   // Display the buffer. 
   MdispSelect(AilDisplay, AilImage);

   // Allocate a graphics list to hold the subpixel annotations. 
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraphicsList);

   // Increase the selection radius for easier interactivity. 
   MgraControlList(AilGraphicsList, M_LIST, M_DEFAULT, M_SELECTION_RADIUS,
      SELECTION_RADIUS);

   // Associate the graphics list to the display for annotations. 
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicsList);

   // Allocate a graphics context for the draw operations. 
   MgraAlloc(AilSystem, &AilGraphicsContext);

   // Enable the interactive mode. 
   MdispControl(AilDisplay, M_GRAPHIC_LIST_INTERACTIVE, M_ENABLE);

   // Enable the use of action keys 
   MgraControlList(AilGraphicsList, M_LIST, M_DEFAULT, M_ACTION_KEYS, M_ENABLE);

   // Add a selectable rectangular region.
   MgraRectAngle(AilGraphicsContext, AilGraphicsList, RECTANGLE_POSITION_X, 
      RECTANGLE_POSITION_Y, RECTANGLE_WIDTH, RECTANGLE_HEIGHT, RECTANGLE_ANGLE,
      M_CENTER_AND_DIMENSION);
   
   // Retrieve the label of the rectangle graphic. 
   MgraInquireList(AilGraphicsList, M_LIST, M_DEFAULT, M_LAST_LABEL, &RegionLabel);

   // Modify the line thickness of the rectangle. 
   MgraControlList(AilGraphicsList, M_GRAPHIC_LABEL(RegionLabel), M_DEFAULT, M_LINE_THICKNESS, 5.0);

   // Disable the selectable mode for the next annotations to the graphics list. 
   MgraControl(AilGraphicsContext, M_SELECTABLE, M_DISABLE);

   // Allocate a binary image buffer for fast processing. 
   MbufInquire(AilImage, M_SIZE_X, &SizeX);
   MbufInquire(AilImage, M_SIZE_Y, &SizeY);
   MbufAlloc2d(AilSystem, SizeX, SizeY, 1+M_UNSIGNED, M_IMAGE+M_PROC, &AilBinImage);
 
   // Binarize the source image. 
   MimBinarize(AilImage, AilBinImage, M_FIXED+M_LESS, IMAGE_THRESHOLD_VALUE, M_NULL);
   
   // Allocate a blob context and a blob result.  
   MblobAlloc(AilSystem, M_DEFAULT, M_DEFAULT, &AilBlobContext);
   MblobAllocResult(AilSystem, M_DEFAULT, M_DEFAULT, &AilBlobResult);

   // Select the blob features to calculate (Center Of Gravity and Box). 
   MblobControl(AilBlobContext, M_CENTER_OF_GRAVITY + M_BINARY, M_ENABLE);
   MblobControl(AilBlobContext, M_BOX, M_ENABLE);

   // Programmatically initialize the selected state of the rectangle region. 
   MgraControlList(AilGraphicsList, M_GRAPHIC_LABEL(RegionLabel), M_DEFAULT, 
      M_GRAPHIC_SELECTED, M_TRUE);

   // Perform and display a first count of the number of objects 
   // within the initial region.                                 
   CountObjects(AilDisplay, AilGraphicsList, AilGraphicsContext, AilBinImage,
      AilBlobContext, AilBlobResult);

   // Initialize the hook data structure, then associate the hook function to  
   // the "M_GRAPHIC_MODIFIED" event. The hook function will be called        
   // with any region interaction by the user.                                
   DataStructure.AilDisplay         = AilDisplay;
   DataStructure.AilGraphicsList    = AilGraphicsList;
   DataStructure.AilGraphicsContext = AilGraphicsContext;
   DataStructure.AilBinImage        = AilBinImage;
   DataStructure.RegionLabel        = RegionLabel;
   DataStructure.AilBlobContext     = AilBlobContext;
   DataStructure.AilBlobResult      = AilBlobResult;
      
   MgraHookFunction(AilGraphicsList, M_GRAPHIC_MODIFIED, &HookHandler, &DataStructure);

   MosPrintf(AIL_TEXT("You can try using your mouse or your keyboard to interactively\n"));
   MosPrintf(AIL_TEXT("modify the displayed region, such as moving, resizing, or\n"));
   MosPrintf(AIL_TEXT("rotating the region. If you do so, the results and annotations\n"));
   MosPrintf(AIL_TEXT("will be immediately updated.\n\n"));

   MosPrintf(AIL_TEXT("Press Enter to exit.\n"));
   AIL_INT PressedKey = 0;
   while(PressedKey != ASCII_ENTER)
      PressedKey = MosGetch();

   MgraHookFunction(AilGraphicsList, M_GRAPHIC_MODIFIED+M_UNHOOK,
      &HookHandler, &DataStructure);

   // Free all allocations. 
   MblobFree(AilBlobResult); 
   MblobFree(AilBlobContext); 
   MbufFree(AilBinImage);
   MgraFree(AilGraphicsContext);
   MgraFree(AilGraphicsList);
   MbufFree(AilImage);
   }


AIL_INT MFTYPE HookHandler(AIL_INT HookType, AIL_ID EventId, void* UserData)
   {
   STestParameters* pDataStructure = (STestParameters*)(UserData);

   // Check that the modified graphic is the rectangular region. 
   AIL_INT ModifiedGraphicLabel;
   MgraGetHookInfo(EventId, M_GRAPHIC_LABEL_VALUE, &ModifiedGraphicLabel);

   if (ModifiedGraphicLabel == pDataStructure->RegionLabel)
      {
      // Count objects and draw the corresponding annotations. 
      CountObjects(pDataStructure->AilDisplay,
                   pDataStructure->AilGraphicsList, 
                   pDataStructure->AilGraphicsContext,
                   pDataStructure->AilBinImage, 
                   pDataStructure->AilBlobContext, 
                   pDataStructure->AilBlobResult);
      }

   return M_NULL;
   }


#define MAX_TEXT_SIZE 100

void CountObjects(AIL_ID AilDisplay, AIL_ID AilGraphicsList, AIL_ID AilGraphicsContext,
                  AIL_ID AilBinImage, AIL_ID AilBlobContext, AIL_ID AilBlobResult)
   {
   AIL_INT NumberOfBlobs,
           NumberOfPrimitives,
           Index;

   AIL_TEXT_CHAR TextLabel[MAX_TEXT_SIZE];

   // Disable the display update for better performance. 
   MdispControl(AilDisplay, M_UPDATE, M_DISABLE);
      
   // Remove all elements from the graphics list, except the rectangle 
   // region primitive at index 0.                                      
   MgraInquireList(AilGraphicsList, M_LIST, M_DEFAULT, M_NUMBER_OF_GRAPHICS,
      &NumberOfPrimitives);
   for(Index = NumberOfPrimitives-1; Index > 0; Index--)
      {
      MgraControlList(AilGraphicsList, M_GRAPHIC_INDEX(Index), M_DEFAULT, M_DELETE,
         M_DEFAULT);
      }

   // Set the input region. The blob analysis will be done 
   // from the (filled) interactive rectangle.             
   MbufSetRegion(AilBinImage, AilGraphicsList, M_DEFAULT,
      M_RASTERIZE + M_FILL_REGION + M_USE_LINE_THICKNESS_1, M_DEFAULT);
      
   // Calculate the blobs and their features.  
   MblobCalculate(AilBlobContext, AilBinImage, M_NULL, AilBlobResult);

   // Get the total number of blobs.  
   MblobGetResult(AilBlobResult, M_GENERAL, M_NUMBER + M_TYPE_AIL_INT, &NumberOfBlobs);

   // Set the input units to display unit for the count annotations.  
   MgraControl(AilGraphicsContext, M_INPUT_UNITS, M_DISPLAY);
   MosSprintf(TextLabel, MAX_TEXT_SIZE, AIL_TEXT(" Number of blobs found: %2i "),
      (int) NumberOfBlobs);

   MgraControl(AilGraphicsContext, M_COLOR, M_COLOR_WHITE);
   MgraText(AilGraphicsContext, AilGraphicsList, 10, 10, TextLabel);
      
   // Restore the input units to pixel units for result annotations.  
   MgraControl(AilGraphicsContext, M_INPUT_UNITS, M_PIXEL);

   // Draw blob center of gravity annotation. 
   MgraControl(AilGraphicsContext, M_LINE_THICKNESS, 3.0);
   MgraControl(AilGraphicsContext, M_COLOR, M_COLOR_RED);
   MblobDraw(AilGraphicsContext, AilBlobResult, AilGraphicsList, M_DRAW_CENTER_OF_GRAVITY, 
      M_INCLUDED_BLOBS, M_DEFAULT);

   // Draw blob bounding box annotations. 
   MgraControl(AilGraphicsContext, M_COLOR, M_COLOR_GREEN);
   MgraControl(AilGraphicsContext, M_LINE_THICKNESS, 1.0);
   MblobDraw(AilGraphicsContext, AilBlobResult, AilGraphicsList, M_DRAW_BOX, 
      M_INCLUDED_BLOBS, M_DEFAULT);

   // Enable the display to update the drawings. 
   MdispControl(AilDisplay, M_UPDATE, M_ENABLE);
   }

```
