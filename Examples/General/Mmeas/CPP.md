---
title: "Mmeas"
description: "This example uses the Measurement module to calculate the position, width and angle of objects in an image."
ms.language: "C++"
---

# Mmeas

> This example uses the Measurement module to calculate the position, width and angle of objects in an image.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufRestore`, `McalAlloc`, `McalFixture`, `McalFree`, `McalUniform`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraControl`, `MgraFree`, `MgraText`, `MimConvert`, `MmeasAllocMarker`, `MmeasDraw`, `MmeasFindMarker`, `MmeasFree`, `MmeasGetResult`, `MmeasSetMarker`, `MmeasSetScore`, `MmodAlloc`, `MmodAllocResult`, `MmodControl`, `MmodDefine`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Semiconductor, Electronics, Applications, Measuring, Counting, Modules, Buffer, Display, Graphics, Image Processing, Calibration, Geometric Model Finder, Measurement, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mmeas.cpp
//
// Description: This program consists of 3 examples that use the Measurement module 
//              to calculate the position, width and angle of objects in an image. 
//              The first one measures the position, width and angle of a stripe
//              in an image, and marks its center and edges. The second one measures
//              the average position, width and angle of a row of pins on a chip.
//              Finally the third example uses the fixturing capability to measure
//              the gap width of objects relative to the object's positions.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////


#include <AIL.h>

// Example selection.
#define RUN_SINGLE_MEASUREMENT_EXAMPLE      M_YES
#define RUN_MULTIPLE_MEASUREMENT_EXAMPLE    M_YES
#define RUN_FIXTURED_MEASUREMENT_EXAMPLE    M_YES

// Example functions declarations.
void SingleMeasurementExample(AIL_ID AilSystem, AIL_ID AilDisplay);
void MultipleMeasurementExample(AIL_ID AilSystem, AIL_ID AilDisplay);
void FixturedMeasurementExample(AIL_ID AilSystem, AIL_ID AilDisplay);

//*****************************************************************************
//Main.
//*****************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication,     // Application identifier.
          AilSystem,          // System Identifier.     
          AilDisplay;         // Display identifier.    

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Print module name.
   MosPrintf(AIL_TEXT("\nMEASUREMENT MODULE:\n"));
   MosPrintf(AIL_TEXT("-------------------\n\n"));

#if (RUN_SINGLE_MEASUREMENT_EXAMPLE)
   SingleMeasurementExample(AilSystem, AilDisplay);
#endif

#if (RUN_MULTIPLE_MEASUREMENT_EXAMPLE)
   MultipleMeasurementExample(AilSystem, AilDisplay);
#endif

#if (RUN_FIXTURED_MEASUREMENT_EXAMPLE)
   FixturedMeasurementExample(AilSystem, AilDisplay);
#endif

   // Free defaults.
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }


//*****************************************************************************
// Single measurement example. 
//*****************************************************************************

// Source image file specification.
#define MEAS_IMAGE_FILE        M_IMAGE_PATH AIL_TEXT("lead.mim")

// Processing region specification.
#define MEAS_BOX_WIDTH         128
#define MEAS_BOX_HEIGHT        100
#define MEAS_BOX_POS_X         166
#define MEAS_BOX_POS_Y         130

// Target stripe typical specifications.
#define STRIPE_POLARITY_LEFT   M_POSITIVE
#define STRIPE_POLARITY_RIGHT  M_NEGATIVE
#define STRIPE_WIDTH           45L
#define STRIPE_WIDTH_VARIATION 10L

void SingleMeasurementExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID     AilImage,                     // Image buffer identifier. 
              AilGraphicList,               // Graphic list identifier. 
              StripeMarker;                 // Stripe marker identifier.
   AIL_DOUBLE StripeCenterX,                // Stripe X center position.
              StripeCenterY,                // Stripe Y center position.
              StripeWidth,                  // Stripe width.            
              StripeAngle;                  // Stripe angle.            
   AIL_DOUBLE CrossColor = M_COLOR_YELLOW,  // Cross drawing color.     
              BoxColor   = M_COLOR_RED;     // Box drawing color.       

   // Restore and display the source image.
   MbufRestore(MEAS_IMAGE_FILE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraphicList);

   // Associate the graphic list to the display.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

   // Allocate a stripe marker.
   MmeasAllocMarker(AilSystem, M_STRIPE, M_DEFAULT, &StripeMarker);

   // Specify the stripe characteristics.
   MmeasSetMarker(StripeMarker, M_POLARITY, STRIPE_POLARITY_LEFT, STRIPE_POLARITY_RIGHT);

   // Set score function to find the expected width
   MmeasSetScore(StripeMarker,
                 M_STRIPE_WIDTH_SCORE,
                 STRIPE_WIDTH - STRIPE_WIDTH_VARIATION,
                 STRIPE_WIDTH - STRIPE_WIDTH_VARIATION,
                 STRIPE_WIDTH + STRIPE_WIDTH_VARIATION,
                 STRIPE_WIDTH + STRIPE_WIDTH_VARIATION,
                 M_DEFAULT,
                 M_DEFAULT,
                 M_DEFAULT);

   MmeasSetMarker(StripeMarker, M_BOX_ANGLE_MODE, M_ENABLE, M_NULL);

   // Specify the search region size and position.
   MmeasSetMarker(StripeMarker, M_BOX_ORIGIN, MEAS_BOX_POS_X, MEAS_BOX_POS_Y);
   MmeasSetMarker(StripeMarker, M_BOX_SIZE,   MEAS_BOX_WIDTH, MEAS_BOX_HEIGHT);

   // Draw the contour of the measurement region.
   MgraControl(M_DEFAULT, M_COLOR, BoxColor);
   MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList, M_DRAW_SEARCH_REGION,
      M_DEFAULT, M_MARKER);

   // Pause to show the original image.
   MosPrintf(AIL_TEXT("Position, width and angle of the stripe ")
             AIL_TEXT("in the highlighted box\n"));
   MosPrintf(AIL_TEXT("will be calculated and the center "));
   MosPrintf(AIL_TEXT("and edges will be marked.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Clear the annotations.
   MgraClear(M_DEFAULT, AilGraphicList);   

   // Find the stripe and measure its width and angle.
   MmeasFindMarker(M_DEFAULT, AilImage , StripeMarker, M_DEFAULT);

   // Get the stripe position, width and angle.
   MmeasGetResult(StripeMarker, M_POSITION,     &StripeCenterX, &StripeCenterY);
   MmeasGetResult(StripeMarker, M_STRIPE_WIDTH, &StripeWidth,   M_NULL);
   MmeasGetResult(StripeMarker, M_ANGLE,        &StripeAngle,   M_NULL);

   // Draw the contour of the found measurement region.
   MgraControl(M_DEFAULT, M_COLOR, BoxColor);
   MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList, M_DRAW_SEARCH_REGION,
      M_DEFAULT, M_RESULT);

   // Draw a cross on the center, left edge and right edge of the found stripe.
   MgraControl(M_DEFAULT, M_COLOR, CrossColor);
   MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList, M_DRAW_POSITION, M_DEFAULT, 
                                                                        M_RESULT);
   MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList, 
             M_DRAW_POSITION+M_EDGE_FIRST+M_EDGE_SECOND, M_DEFAULT, M_RESULT);

   // Print the result.
   MosPrintf(AIL_TEXT("The stripe in the image is at position %.2f,%.2f and\n"),
                                         StripeCenterX, StripeCenterY);
   MosPrintf(AIL_TEXT("is %.2f pixels wide with an angle of %.2f degrees.\n"),
                                         StripeWidth, StripeAngle);
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Remove the graphic list association to the display.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, M_NULL);

   // Free all allocations.
   MgraFree(AilGraphicList);
   MmeasFree(StripeMarker);
   MbufFree(AilImage);
   }


// *****************************************************************************
// Multiple measurement example. 
// *****************************************************************************

// Source image file specification.
#define MULT_MEAS_IMAGE_FILE     M_IMAGE_PATH AIL_TEXT("chip.mim")

// Processing region specification.
#define MULT_MEAS_BOX_WIDTH      230
#define MULT_MEAS_BOX_HEIGHT     7
#define MULT_MEAS_BOX_POS_X      220
#define MULT_MEAS_BOX_POS_Y      171

// Target stripe specifications.
#define MULT_STRIPES_ORIENTATION      M_VERTICAL 
#define MULT_STRIPES_POLARITY_LEFT    M_POSITIVE
#define MULT_STRIPES_POLARITY_RIGHT   M_NEGATIVE
#define MULT_STRIPES_NUMBER           12
 
void MultipleMeasurementExample(AIL_ID AilSystem, AIL_ID AilDisplay)
{
   AIL_ID     AilImage,                    // Image buffer identifier. 
              AilGraphicList,              // Graphic list identifier. 
              StripeMarker;                // Stripe marker identifier.
   AIL_DOUBLE MeanAngle,                   // Stripe mean angle.       
              MeanWidth,                   // Stripe mean width.       
              MeanSpacing;                 // Stripe mean spacing.     
   AIL_DOUBLE CrossColor = M_COLOR_YELLOW, // Cross drawing color.     
              BoxColor   = M_COLOR_RED;    // Box drawing color.       

   // Restore and display the source image.
   MbufRestore(MULT_MEAS_IMAGE_FILE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);
   
   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraphicList);

   // Associate the graphic list to the display.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

   // Allocate a stripe marker.
   MmeasAllocMarker(AilSystem, M_STRIPE, M_DEFAULT, &StripeMarker);

   // Specify the stripe characteristics.
   MmeasSetMarker(StripeMarker, M_NUMBER, MULT_STRIPES_NUMBER, M_NULL);
   MmeasSetMarker(StripeMarker, M_POLARITY, MULT_STRIPES_POLARITY_LEFT, 
                                            MULT_STRIPES_POLARITY_RIGHT);
   MmeasSetMarker(StripeMarker, M_ORIENTATION, MULT_STRIPES_ORIENTATION, M_NULL);

   // Specify the measurement region size and position.
   MmeasSetMarker(StripeMarker, M_BOX_ORIGIN, MULT_MEAS_BOX_POS_X, MULT_MEAS_BOX_POS_Y);
   MmeasSetMarker(StripeMarker, M_BOX_SIZE,   MULT_MEAS_BOX_WIDTH, MULT_MEAS_BOX_HEIGHT);

   // Draw the contour of the measurement region.
   MgraControl(M_DEFAULT, M_COLOR, BoxColor);
   MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList, M_DRAW_SEARCH_REGION,
      M_DEFAULT, M_MARKER);

   // Pause to show the original image.
   MosPrintf(AIL_TEXT("The position and angle of a row of pins on a chip ")
                                          AIL_TEXT("will be calculated.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();
  
   // Find the stripe and measure its width and angle.
   MmeasFindMarker(M_DEFAULT, AilImage, StripeMarker, M_POSITION + M_ANGLE + M_STRIPE_WIDTH);

   // Draw the contour of the measurement region.
   MgraControl(M_DEFAULT, M_COLOR, BoxColor);
   MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList, M_DRAW_SEARCH_REGION,
      M_DEFAULT, M_RESULT);

   // Draw a cross at the center of each stripe found.
   MgraControl(M_DEFAULT, M_COLOR, CrossColor);
   MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList, M_DRAW_POSITION, M_RESULT_ALL_OCCURRENCES, M_RESULT);

   // Get the stripe's width, angle and spacing.
   MmeasGetResult(StripeMarker, M_ANGLE        + M_MEAN, &MeanAngle,   M_NULL);
   MmeasGetResult(StripeMarker, M_STRIPE_WIDTH + M_MEAN, &MeanWidth,   M_NULL);
   MmeasGetResult(StripeMarker, M_SPACING      + M_MEAN, &MeanSpacing, M_NULL);

   // Print the results.
   MosPrintf(AIL_TEXT("The center and angle of each pin have been marked.\n\n"));
   MosPrintf(AIL_TEXT("The statistics for the pins are:\n"));
   MosPrintf(AIL_TEXT("Average angle   : %5.2f\n"), MeanAngle);
   MosPrintf(AIL_TEXT("Average width   : %5.2f\n"), MeanWidth);
   MosPrintf(AIL_TEXT("Average spacing : %5.2f\n"), MeanSpacing);
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Remove the graphic list association to the display.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, M_NULL);

   // Free all allocations.
   MgraFree(AilGraphicList);
   MmeasFree(StripeMarker);
   MbufFree(AilImage);
}

// *****************************************************************************
// Fixtured measurement example. 
// *****************************************************************************

// Source image file specification.
#define FIXTURED_MEAS_IMAGE_FILE        M_IMAGE_PATH AIL_TEXT("Fuse.mim")

// Processing region specification.
#define FIXTURED_MEAS_BOX_OFFSET_X      400
#define FIXTURED_MEAS_BOX_OFFSET_Y      290
#define FIXTURED_MEAS_BOX_WIDTH         100
#define FIXTURED_MEAS_BOX_HEIGHT         15

// Model region specification.
#define FIXTURED_MODEL_OFFSET_X         395
#define FIXTURED_MODEL_OFFSET_Y         200
#define FIXTURED_MODEL_SIZE_X           110
#define FIXTURED_MODEL_SIZE_Y           120

#define FIXTURED_IMAGE_SIZE_X           512
#define FIXTURED_IMAGE_SIZE_Y           384

// Target stripe typical specifications.
#define FIXTURED_STRIPE_POLARITY_LEFT       M_POSITIVE
#define FIXTURED_STRIPE_POLARITY_RIGHT      M_OPPOSITE

void FixturedMeasurementExample(AIL_ID AilSystem, AIL_ID AilDisplay) 
   {
   AIL_ID     AilSourceImage,               // Source image buffer identifier.  
              AilImage,                     // Image buffer identifier.         
              AilModContext,                // Model finder context identifier. 
              AilModResult,                 // Model finder result identifier.  
              AilFixturingOffset,           // Fixturing object identifier.     
              StripeMarker;                 // Stripe marker identifier.        

   AIL_ID     AilGraphicList;               // Graphic list identifier.         

   AIL_DOUBLE StripeWidth,                  // Stripe width.                    
              PositionX,                    // Occurrence position X.           
              PositionY;                    // Occurrence position Y.           

   AIL_INT    SizeX,                        // Source image size X.             
              SizeY,                        // Source image size Y.             
              NbOccurrences,                // Number of found occurrences.     
              NbStripes,                    // Number of found stripes.         
              Index;                        // Occurrence index.                

   AIL_TEXT_CHAR TextIndex[128];            // Text of the occurrence index.    
   
   // Restore the source image.
   MbufRestore(FIXTURED_MEAS_IMAGE_FILE, AilSystem, &AilSourceImage);
   MbufInquire(AilSourceImage, M_SIZE_X, &SizeX);
   MbufInquire(AilSourceImage, M_SIZE_Y, &SizeY);

   // Allocate, then compute the source image luminance.
   MbufAlloc2d(AilSystem, SizeX, SizeY, 8+M_UNSIGNED, M_IMAGE+M_PROC+M_DISP, &AilImage);
   MimConvert(AilSourceImage, AilImage, M_RGB_TO_L);   

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraphicList);   

   // Select the image to the display.
   MdispSelect(AilDisplay, AilImage);

   // Associate the graphic list to the display.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

   // Allocate a stripe marker.
   MmeasAllocMarker(AilSystem, M_STRIPE, M_DEFAULT, &StripeMarker);

   // Set inputs units to world in order to fixture the region.
   MmeasSetMarker(StripeMarker, M_SEARCH_REGION_INPUT_UNITS, M_WORLD, M_NULL);

   // Calibrate the destination image to receive the world units annotations.
   McalUniform(AilImage, 0.0, 0.0, 1.0, 1.0, 0.0, M_DEFAULT);

   // Specify the stripe characteristics.
   MmeasSetMarker(StripeMarker, M_BOX_ORIGIN, FIXTURED_MEAS_BOX_OFFSET_X,
      FIXTURED_MEAS_BOX_OFFSET_Y);
   MmeasSetMarker(StripeMarker, M_POLARITY, FIXTURED_STRIPE_POLARITY_LEFT,
      FIXTURED_STRIPE_POLARITY_RIGHT);
   MmeasSetMarker(StripeMarker, M_SEARCH_REGION_CLIPPING, M_MAXIMIZE_AREA, M_NULL);

   // Specify the search region size and position.
   MmeasSetMarker(StripeMarker, M_BOX_SIZE, FIXTURED_MEAS_BOX_WIDTH,
      FIXTURED_MEAS_BOX_HEIGHT);

   // Draw the contour of the measurement region.
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_BLUE);
   MgraControl(M_DEFAULT, M_INPUT_UNITS, M_WORLD);
   MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList, M_DRAW_SEARCH_REGION,
         M_DEFAULT, M_MARKER);
   MgraControl(M_DEFAULT, M_INPUT_UNITS, M_PIXEL);

   MosPrintf(AIL_TEXT("A measurement stripe marker (in blue) is defined.\n"));

   // Define model to further fixture the measurement marker.
   MmodAlloc(AilSystem, M_GEOMETRIC, M_DEFAULT, &AilModContext);

   MmodAllocResult(AilSystem, M_DEFAULT, &AilModResult);

   MmodDefine(AilModContext, M_IMAGE, AilImage, 
      FIXTURED_MODEL_OFFSET_X, FIXTURED_MODEL_OFFSET_Y, 
      FIXTURED_MODEL_SIZE_X, FIXTURED_MODEL_SIZE_Y);

   MmodControl(AilModContext, M_DEFAULT, M_NUMBER, M_ALL);
   MmodControl(AilModContext, M_CONTEXT, M_SPEED,  M_VERY_HIGH);

   // Preprocess the model.
   MmodPreprocess(AilModContext, M_DEFAULT);      

   // Display the model.
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);      
   MmodDraw(M_DEFAULT, AilModContext, AilGraphicList, M_DRAW_BOX+M_DRAW_POSITION,  
      M_DEFAULT, M_ORIGINAL); 

   MosPrintf(AIL_TEXT("A Model Finder model (in green) is defined to\n"));
   MosPrintf(AIL_TEXT("further fixture the measurement operation.\n\n"));

   MosPrintf(AIL_TEXT("The stripe marker determines the gap between\n"));
   MosPrintf(AIL_TEXT("the fuse connectors. Model Finder tracks the\n"));
   MosPrintf(AIL_TEXT("fuses while the attached fixturing automatically\n"));
   MosPrintf(AIL_TEXT("relocates the marker relative to the found fuses.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate a fixture object.
   McalAlloc(AilSystem, M_FIXTURING_OFFSET, M_DEFAULT, &AilFixturingOffset);

   // Learn the relative offset of the model.
   McalFixture(M_NULL, AilFixturingOffset, M_LEARN_OFFSET, M_MODEL_MOD, 
      AilModContext, 0, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   // Find the location of the fixtures using Model Finder.
   MmodFind(AilModContext, AilImage, AilModResult);

   // Display and retrieve the number of occurrences found.
   MgraClear(M_DEFAULT, AilGraphicList);
   MmodDraw(M_DEFAULT, AilModResult, AilGraphicList, M_DRAW_POSITION+M_DRAW_BOX,
      M_DEFAULT, M_DEFAULT);

   MmodGetResult(AilModResult, M_DEFAULT, M_NUMBER+M_TYPE_AIL_INT, &NbOccurrences);

   MosPrintf(AIL_TEXT("Locating the parts: %d occurrences are found.\n"), NbOccurrences);
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   MosPrintf(AIL_TEXT("The measurement tool is moved relative to each piece.\n"));
   MosPrintf(AIL_TEXT("A graphic list is used to display the results with\n"));
   MosPrintf(AIL_TEXT("subpixel annotations.\n\n"));

   // Clear the annotations.
   MgraClear(M_DEFAULT, AilGraphicList);

   for(Index=0; Index<NbOccurrences; Index++)
      {
      // Display the found model occurrence position.
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);
      MmodDraw(M_DEFAULT, AilModResult, AilGraphicList, M_DRAW_POSITION,
         Index, M_DEFAULT);

      MmodGetResult(AilModResult, Index, M_POSITION_X+M_TYPE_AIL_DOUBLE, &PositionX);
      MmodGetResult(AilModResult, Index, M_POSITION_Y+M_TYPE_AIL_DOUBLE, &PositionY);
      
      MosSprintf(TextIndex, 128, AIL_TEXT("%d"), Index);
      MgraText(M_DEFAULT, AilGraphicList, PositionX-20, PositionY, TextIndex);

      // Apply a fixture offset to the implicit 1:1 calibration of the target image
      McalFixture(AilImage, AilFixturingOffset, M_MOVE_RELATIVE, M_RESULT_MOD, 
         AilModResult, Index, M_DEFAULT, M_DEFAULT, M_DEFAULT);

      // Find the stripe and measure its width and angle.
      MmeasFindMarker(M_DEFAULT, AilImage, StripeMarker, M_POSITION+M_STRIPE_WIDTH);

      // Get the number of found results.
      MmeasGetResult(StripeMarker, M_NUMBER+M_TYPE_AIL_INT, &NbStripes, M_NULL);

      if(NbStripes==1)
         {
         // Get the stripe width.
         MmeasGetResult(StripeMarker, M_STRIPE_WIDTH, &StripeWidth, M_NULL);

         // Draw the contour of the found measurement region.
         MgraControl(M_DEFAULT, M_COLOR, M_COLOR_BLUE);
         MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList, M_DRAW_SEARCH_REGION,
            M_DEFAULT, M_RESULT);

         // Draw a cross on the center, left edge and right edge of the found stripe.
         MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);
         MmeasDraw(M_DEFAULT, StripeMarker, AilGraphicList,  M_DRAW_WIDTH,
            M_DEFAULT, M_RESULT);

         // Print the result.
         MosPrintf(AIL_TEXT("The gap (in red) of occurrence %d is %.2f pixels wide.\n"),
            Index, StripeWidth);
         }
      else
         {
         MosPrintf(AIL_TEXT("The gap of occurrence %d could not be measured.\n"), Index);
         }
      }

   MosPrintf(AIL_TEXT("Press any key to end.\n"));
   MosGetch();

   // Free all allocations.
   MgraFree(AilGraphicList);
   MmeasFree(StripeMarker);
   MmodFree(AilModContext);
   MmodFree(AilModResult);
   McalFree(AilFixturingOffset);
   MbufFree(AilImage);
   MbufFree(AilSourceImage);
   }

```
