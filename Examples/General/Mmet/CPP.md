---
title: "Mmet"
description: "This program uses the metrology module to measure geometric feature and tolerances on parts."
ms.language: "C++"
---

# Mmet

> This program uses the metrology module to measure geometric feature and tolerances on parts.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `McalAssociate`, `McalFree`, `McalFixture`, `McalRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MmetAddFeature`, `MmetAddTolerance`, `MmetAlloc`, `MmetAllocResult`, `MmetCalculate`, `MmetControl`, `MmetDraw`, `MmetFree`, `MmetGetResult`, `MmetSetRegion`, `MmodAllocResult`, `MmodControl`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodPreprocess`, `MmodRestore`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Measuring, Modules, Buffer, Display, Graphics, Calibration, Geometric Model Finder, Metrology, What's New, Older, AIL 10.0 U116

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mmet.cpp
//
// Description: This program uses the Metrology module to measure geometric 
//              features and to validate tolerance relationships between features.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Example selection.
#define RUN_SIMPLE_IMAGE_EXAMPLE     M_YES
#define RUN_COMPLETE_IMAGE_EXAMPLE   M_YES

// Example functions declarations.
void SimpleImageExample(AIL_ID AilSystem, AIL_ID AilDisplay);
void CompleteImageExample(AIL_ID AilSystem, AIL_ID AilDisplay);

/*****************************************************************************
 Main.
 *****************************************************************************/
int MosMain(void)
   {
   AIL_ID AilApplication,     // Application identifier.
          AilSystem,          // System Identifier.     
          AilDisplay;         // Display identifier.    

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Print module name.
   MosPrintf(AIL_TEXT("\nMETROLOGY MODULE:\n"));
   MosPrintf(AIL_TEXT("-------------------\n\n"));

   #if (RUN_SIMPLE_IMAGE_EXAMPLE)
   SimpleImageExample(AilSystem, AilDisplay);
   #endif

   #if (RUN_COMPLETE_IMAGE_EXAMPLE)
   CompleteImageExample(AilSystem, AilDisplay);
   #endif

   // Free defaults.
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);
   
   return 0;
   }

// *****************************************************************************
// Simple example. 
// *****************************************************************************
// Source image file specification.
#define METROL_SIMPLE_IMAGE_FILE M_IMAGE_PATH AIL_TEXT("SingleModel.mim")

// Region parameters
#define TOP_RING_POSITION_X        240
#define TOP_RING_POSITION_Y        155
#define TOP_RING_START_RADIUS        2
#define TOP_RING_END_RADIUS         15 

#define MIDDLE_RING_POSITION_X     240
#define MIDDLE_RING_POSITION_Y     190
#define MIDDLE_RING_START_RADIUS     2
#define MIDDLE_RING_END_RADIUS      15

#define BOTTOM_RECT_POSITION_X     320
#define BOTTOM_RECT_POSITION_Y     265
#define BOTTOM_RECT_WIDTH          170
#define BOTTOM_RECT_HEIGHT          20
#define BOTTOM_RECT_ANGLE          180

// Tolerance parameters
#define PERPENDICULARITY_MIN       0.5
#define PERPENDICULARITY_MAX       0.5

// Color definitions
#define FAIL_COLOR       M_RGB888(255,0,0)
#define PASS_COLOR       M_RGB888(0,255,0)
#define REGION_COLOR     M_RGB888(0,100,255)
#define FEATURE_COLOR    M_RGB888(255,0,255)

void SimpleImageExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID AilImage,                    // Image buffer identifier.
          GraphicList,                 // Graphic list identifier.
          AilMetrolContext,            // Metrology Context       
          AilMetrolResult;             // Metrology Result        

   AIL_DOUBLE  Status,
               Value;

   AIL_INT FeatureIndexForTopConstructedPoint    = M_FEATURE_INDEX(1); 
   AIL_INT FeatureIndexForMiddleConstructedPoint = M_FEATURE_INDEX(2); 
   AIL_INT FeatureIndexForConstructedSegment[2]  = { M_FEATURE_INDEX(3),M_FEATURE_INDEX(4) };
   AIL_INT FeatureIndexForTolerance[2]           = { M_FEATURE_INDEX(5),M_FEATURE_INDEX(6) };

   // Restore and display the source image.
   MbufRestore(METROL_SIMPLE_IMAGE_FILE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &GraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

   // Allocate metrology context and result.
   MmetAlloc(AilSystem, M_DEFAULT, &AilMetrolContext);
   MmetAllocResult(AilSystem, M_DEFAULT, &AilMetrolResult);

   // Add a first measured circle feature to context and set its search region
   MmetAddFeature(AilMetrolContext, M_MEASURED, M_CIRCLE, M_DEFAULT, 
                  M_DEFAULT, M_NULL, M_NULL, 0, M_DEFAULT);

   MmetSetRegion(AilMetrolContext, M_FEATURE_INDEX(1), M_DEFAULT, M_RING, 
                 TOP_RING_POSITION_X, TOP_RING_POSITION_Y, TOP_RING_START_RADIUS, 
                 TOP_RING_END_RADIUS, M_NULL, M_NULL);

   // Add a second measured circle feature to context and set its search region
   MmetAddFeature(AilMetrolContext, M_MEASURED, M_CIRCLE, M_DEFAULT,
                  M_DEFAULT, M_NULL, M_NULL, 0, M_DEFAULT);

   MmetSetRegion(AilMetrolContext, M_FEATURE_INDEX(2), M_DEFAULT, M_RING, 
                 MIDDLE_RING_POSITION_X, MIDDLE_RING_POSITION_Y,  
                 MIDDLE_RING_START_RADIUS,MIDDLE_RING_END_RADIUS, M_NULL, M_NULL);

   // Add a first constructed point feature to context
   MmetAddFeature(AilMetrolContext, M_CONSTRUCTED, M_POINT, M_DEFAULT,
                  M_CENTER, &FeatureIndexForTopConstructedPoint, M_NULL, 1, M_DEFAULT);

   // Add a second constructed point feature to context
   MmetAddFeature(AilMetrolContext, M_CONSTRUCTED, M_POINT, M_DEFAULT,
                  M_CENTER, &FeatureIndexForMiddleConstructedPoint, M_NULL, 1, M_DEFAULT);

   // Add a constructed segment feature to context passing through the two points
   MmetAddFeature(AilMetrolContext, M_CONSTRUCTED, M_SEGMENT, M_DEFAULT,
                  M_CONSTRUCTION, FeatureIndexForConstructedSegment, M_NULL, 2, M_DEFAULT);

   // Add a first segment feature to context and set its search region
   MmetAddFeature(AilMetrolContext, M_MEASURED, M_SEGMENT, M_DEFAULT,
                  M_DEFAULT, M_NULL, M_NULL, 0, M_DEFAULT);

   MmetSetRegion(AilMetrolContext, M_FEATURE_INDEX(6), M_DEFAULT, M_RECTANGLE, 
                 BOTTOM_RECT_POSITION_X, BOTTOM_RECT_POSITION_Y, BOTTOM_RECT_WIDTH, 
                 BOTTOM_RECT_HEIGHT, BOTTOM_RECT_ANGLE, M_NULL);

   // Add perpendicularity tolerance
   MmetAddTolerance(AilMetrolContext, M_PERPENDICULARITY, M_DEFAULT, PERPENDICULARITY_MIN, 
                    PERPENDICULARITY_MAX, FeatureIndexForTolerance, M_NULL, 2, M_DEFAULT);

   // Calculate
   MmetCalculate(AilMetrolContext, AilImage, AilMetrolResult, M_DEFAULT);

   // Draw region
   MgraControl(M_DEFAULT, M_COLOR, REGION_COLOR);
   MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_REGION, M_DEFAULT, 
                                                                        M_DEFAULT);
   MosPrintf(AIL_TEXT("Regions used to calculate measured features:\n"));
   MosPrintf(AIL_TEXT("- two measured circles\n"));
   MosPrintf(AIL_TEXT("- one measured segment\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Clear annotations.
   MgraClear(M_DEFAULT, GraphicList);

   MgraControl(M_DEFAULT, M_COLOR, FEATURE_COLOR);
   MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_FEATURE, M_DEFAULT, 
                                                                         M_DEFAULT);
   MosPrintf(AIL_TEXT("Calculated features:\n"));

   MmetGetResult(AilMetrolResult, M_FEATURE_INDEX(1), M_RADIUS, &Value);
   MosPrintf(AIL_TEXT("- first measured circle:  radius=%.2f\n"), Value);

   MmetGetResult(AilMetrolResult, M_FEATURE_INDEX(2), M_RADIUS, &Value);
   MosPrintf(AIL_TEXT("- second measured circle: radius=%.2f\n"), Value);

   MmetGetResult(AilMetrolResult, M_FEATURE_INDEX(5), M_LENGTH, &Value);
   MosPrintf(AIL_TEXT("- constructed segment between the two circle centers: ")
             AIL_TEXT("length=%.2f\n"), Value);

   MmetGetResult(AilMetrolResult, M_FEATURE_INDEX(6), M_LENGTH, &Value);
   MosPrintf(AIL_TEXT("- measured segment: length=%.2f\n"), Value);   

   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Get angularity tolerance status and value.
   MmetGetResult(AilMetrolResult, M_TOLERANCE_INDEX(0), M_STATUS, &Status);
   MmetGetResult(AilMetrolResult, M_TOLERANCE_INDEX(0), M_VALUE, &Value);

   if(Status==M_PASS)
      {
      MgraControl(M_DEFAULT, M_COLOR, PASS_COLOR);
      MosPrintf(AIL_TEXT("Perpendicularity between the two segments: %.2f ")
                AIL_TEXT("degrees.\n"), Value);
      }
   else
      {
      MgraControl(M_DEFAULT, M_COLOR, FAIL_COLOR);
      MosPrintf(AIL_TEXT("Perpendicularity between the two segments - Fail.\n"));
      }
   MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_TOLERANCE, 
                                           M_TOLERANCE_INDEX(0), M_DEFAULT);   

   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Free all allocations.
   MgraFree(GraphicList);
   MmetFree(AilMetrolResult);
   MmetFree(AilMetrolContext);
   MbufFree(AilImage);
   }

// *****************************************************************************
// Complete example. 
// *****************************************************************************
// Source image, calibration and model finder context file specification.
#define METROL_CALIBRATION_FILE   M_IMAGE_PATH AIL_TEXT("Hook.mca")
#define METROL_COMPLETE_IMAGE_FILE M_IMAGE_PATH AIL_TEXT("Hook.tif")
#define METROL_MODEL_FINDER_FILE  M_IMAGE_PATH AIL_TEXT("Hook.mmf")

// Region parameters
#define CIRCLE1_LABEL            1
#define RING1_POSITION_X         1.10
#define RING1_POSITION_Y         0.80
#define RING1_START_RADIUS       0.20
#define RING1_END_RADIUS         0.50

#define CIRCLE2_LABEL            2
#define RING2_POSITION_X         1.10
#define RING2_POSITION_Y         3.00
#define RING2_START_RADIUS       0.10
#define RING2_END_RADIUS         0.40

#define SEGMENT1_LABEL           3
#define RECT1_POSITION_X         0.10
#define RECT1_POSITION_Y         2.40
#define RECT1_WIDTH              1.40
#define RECT1_HEIGHT             0.30
#define RECT1_ANGLE              90.0

#define SEGMENT2_LABEL           4
#define RECT2_POSITION_X         0.90
#define RECT2_POSITION_Y         2.80
#define RECT2_WIDTH              0.40
#define RECT2_HEIGHT             0.20
#define RECT2_ANGLE             165.0

#define POINT1_LABEL             5
#define SEG1_START_X             1.60
#define SEG1_START_Y             1.50
#define SEG1_END_X               1.60
#define SEG1_END_Y               2.40

// Tolerance parameters
#define MIN_DISTANCE_LABEL       1
#define ANGULARITY_LABEL         2
#define MAX_DISTANCE_LABEL       3

#define MIN_DISTANCE_VALUE_MIN   1.40
#define MIN_DISTANCE_VALUE_MAX   1.60
#define MAX_DISTANCE_VALUE_MIN   0.40
#define MAX_DISTANCE_VALUE_MAX   0.60
#define ANGULARITY_VALUE_MIN     65.0
#define ANGULARITY_VALUE_MAX     75.0

void CompleteImageExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID AilImage,                        // Image buffer identifier.
          GraphicList,                     // Graphic list identifier.
          AilCalibration,                  // Calibration context     
          AilMetrolContext,                // Metrology Context       
          AilMetrolResult,                 // Metrology Result        
          AilModelFinderContext,           // Model Finder Context    
          AilModelFinderResult,            // Model Finder Result     
          FixtureOffset;                   // Fixture offset          
   
   AIL_DOUBLE Status,
              Value;

   AIL_INT MinDistanceFeatureLabels[2]  = { CIRCLE1_LABEL , CIRCLE2_LABEL  };
   AIL_INT AngularityFeatureLabels[2]   = { SEGMENT1_LABEL, SEGMENT2_LABEL };
   AIL_INT MaxDistanceFeatureLabels[2]  = { POINT1_LABEL  , POINT1_LABEL   };
   AIL_INT MaxDistanceFeatureIndices[2] = { 0             , 1              };

   // Restore and display the source image.
   MbufRestore(METROL_COMPLETE_IMAGE_FILE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

    // Restore and associate calibration context to source image
   McalRestore(METROL_CALIBRATION_FILE, AilSystem, M_DEFAULT, &AilCalibration);
   McalAssociate(AilCalibration, AilImage, M_DEFAULT);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &GraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

   // Allocate metrology context and result.
   MmetAlloc(AilSystem, M_DEFAULT, &AilMetrolContext);
   MmetAllocResult(AilSystem, M_DEFAULT, &AilMetrolResult);

   // Add a first measured circle feature to context and set its search region.
   MmetAddFeature(AilMetrolContext, M_MEASURED, M_CIRCLE, CIRCLE1_LABEL, 
                  M_DEFAULT, M_NULL, M_NULL, 0, M_DEFAULT);

   MmetSetRegion(AilMetrolContext, M_FEATURE_LABEL(CIRCLE1_LABEL), M_DEFAULT, M_RING, 
                 RING1_POSITION_X, RING1_POSITION_Y, RING1_START_RADIUS, RING1_END_RADIUS, 
                 M_NULL, M_NULL);

   // Add a second measured circle feature to context and set its search region.
   MmetAddFeature(AilMetrolContext, M_MEASURED, M_CIRCLE, CIRCLE2_LABEL,
                  M_DEFAULT, M_NULL, M_NULL, 0, M_DEFAULT);
   
   MmetSetRegion(AilMetrolContext, M_FEATURE_LABEL(CIRCLE2_LABEL), M_DEFAULT, M_RING, 
                 RING2_POSITION_X, RING2_POSITION_Y, RING2_START_RADIUS, RING2_END_RADIUS, 
                 M_NULL, M_NULL);

   // Add a first measured segment feature to context and set its search region.
   MmetAddFeature(AilMetrolContext, M_MEASURED, M_SEGMENT, SEGMENT1_LABEL,
                  M_DEFAULT, M_NULL, M_NULL, 0, M_DEFAULT);

   MmetSetRegion(AilMetrolContext, M_FEATURE_LABEL(SEGMENT1_LABEL), M_DEFAULT, M_RECTANGLE, 
                 RECT1_POSITION_X, RECT1_POSITION_Y, RECT1_WIDTH, RECT1_HEIGHT, 
                 RECT1_ANGLE, M_NULL);

   MmetControl(AilMetrolContext, M_FEATURE_LABEL(SEGMENT1_LABEL), M_EDGEL_ANGLE_RANGE, 10);

   // Add a second measured segment feature to context and set its search region.
   MmetAddFeature(AilMetrolContext, M_MEASURED, M_SEGMENT, SEGMENT2_LABEL,
                  M_INNER_FIT, M_NULL, M_NULL, 0, M_DEFAULT);

   MmetSetRegion(AilMetrolContext, M_FEATURE_LABEL(SEGMENT2_LABEL), M_DEFAULT, M_RECTANGLE, 
                 RECT2_POSITION_X, RECT2_POSITION_Y, RECT2_WIDTH, RECT2_HEIGHT, 
                 RECT2_ANGLE, M_NULL);

   // Add a second measured segment feature to context and set its search region.
   MmetAddFeature(AilMetrolContext, M_MEASURED, M_POINT, POINT1_LABEL,
                  M_DEFAULT, M_NULL, M_NULL, 0, M_DEFAULT);

   MmetSetRegion(AilMetrolContext, M_FEATURE_LABEL(POINT1_LABEL), M_DEFAULT, M_SEGMENT, 
                 SEG1_START_X, SEG1_START_Y, SEG1_END_X, SEG1_END_Y, 
                 M_NULL, M_NULL);

   //Set the polarity and the maximum number of points to detect along the segment region.
   MmetControl(AilMetrolContext, M_FEATURE_LABEL(POINT1_LABEL), 
                     M_EDGEL_RELATIVE_ANGLE, M_SAME_OR_REVERSE);
   MmetControl(AilMetrolContext, M_FEATURE_LABEL(POINT1_LABEL), M_NUMBER_MAX, 2);

   // Add minimum distance tolerance.
   MmetAddTolerance(AilMetrolContext, M_DISTANCE_MIN, MIN_DISTANCE_LABEL, 
                    MIN_DISTANCE_VALUE_MIN, MIN_DISTANCE_VALUE_MAX,  
                    MinDistanceFeatureLabels, M_NULL, 2, M_DEFAULT);

   // Add angularity tolerance.
   MmetAddTolerance(AilMetrolContext, M_ANGULARITY, ANGULARITY_LABEL, 
                    ANGULARITY_VALUE_MIN, ANGULARITY_VALUE_MAX,  
                    AngularityFeatureLabels, M_NULL, 2, M_DEFAULT);

   
   // Add maximum distance tolerance.
   MmetAddTolerance(AilMetrolContext, M_DISTANCE_MAX, MAX_DISTANCE_LABEL, 
                    MAX_DISTANCE_VALUE_MIN, MAX_DISTANCE_VALUE_MAX,  
                    MaxDistanceFeatureLabels, MaxDistanceFeatureIndices, 2, M_DEFAULT);

   // Calculate.
   MmetCalculate(AilMetrolContext, AilImage, AilMetrolResult, M_DEFAULT);

   // Draw features.
   MgraControl(M_DEFAULT, M_COLOR, REGION_COLOR);
   MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_REGION, M_DEFAULT, 
                                                                        M_DEFAULT);
   MosPrintf(AIL_TEXT("Regions used to calculate measured features:\n"));
   MosPrintf(AIL_TEXT("- two measured circle features\n"));
   MosPrintf(AIL_TEXT("- two measured segment features\n"));
   MosPrintf(AIL_TEXT("- one measured points feature\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Clear annotations.
   MgraClear(M_DEFAULT, GraphicList);

   MgraControl(M_DEFAULT, M_COLOR, FEATURE_COLOR);
   MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_FEATURE, M_DEFAULT, 
                                                                         M_DEFAULT);
   MosPrintf(AIL_TEXT("Calculated features:\n"));

   MmetGetResult(AilMetrolResult, M_FEATURE_LABEL(CIRCLE1_LABEL), M_RADIUS, &Value);
   MosPrintf(AIL_TEXT("- first measured circle:   radius=%.2fmm\n"), Value);

   MmetGetResult(AilMetrolResult, M_FEATURE_LABEL(CIRCLE2_LABEL), M_RADIUS, &Value);
   MosPrintf(AIL_TEXT("- second measured circle:  radius=%.2fmm\n"), Value);

   MmetGetResult(AilMetrolResult, M_FEATURE_LABEL(SEGMENT1_LABEL), M_LENGTH, &Value);
   MosPrintf(AIL_TEXT("- first measured segment:  length=%.2fmm\n"), Value);

   MmetGetResult(AilMetrolResult, M_FEATURE_LABEL(SEGMENT2_LABEL), M_LENGTH, &Value);
   MosPrintf(AIL_TEXT("- second measured segment: length=%.2fmm\n"), Value);

   MosPrintf(AIL_TEXT("- two measured points\n"));

   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Get angularity tolerance status and value.
   MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(ANGULARITY_LABEL), M_STATUS, &Status);
   MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(ANGULARITY_LABEL), M_VALUE, &Value);

   if(Status==M_PASS)
      {
      MgraControl(M_DEFAULT, M_COLOR, PASS_COLOR);
      MosPrintf(AIL_TEXT("Angularity value: %.2f degrees.\n"), Value);
      }
   else
      {
      MgraControl(M_DEFAULT, M_COLOR, FAIL_COLOR);
      MosPrintf(AIL_TEXT("Angularity value - Fail.\n"));
      }
   MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_TOLERANCE, 
                            M_TOLERANCE_LABEL(ANGULARITY_LABEL), M_DEFAULT);   

   // Get min distance tolerance status and value.
   MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), M_STATUS, 
                                                                         &Status);
   MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), M_VALUE, 
                                                                         &Value);

   if(Status==M_PASS)
      {
      MgraControl(M_DEFAULT, M_COLOR, PASS_COLOR);
      MosPrintf(AIL_TEXT("Min distance tolerance value: %.2f mm.\n"), Value);
      }
   else
      {
      MgraControl(M_DEFAULT, M_COLOR, FAIL_COLOR);
      MosPrintf(AIL_TEXT("Min distance tolerance value - Fail.\n"));
      }
   MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_TOLERANCE, 
                           M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), M_DEFAULT);   

   // Get max distance tolerance status and value.
   MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), M_STATUS, 
                                                                         &Status);
   MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), M_VALUE, 
                                                                         &Value);

   if(Status==M_PASS)
      {
      MgraControl(M_DEFAULT, M_COLOR, PASS_COLOR);
      MosPrintf(AIL_TEXT("Max distance tolerance value: %.2f mm.\n"), Value);
      }
   else
      {
      MgraControl(M_DEFAULT, M_COLOR, FAIL_COLOR);
      MosPrintf(AIL_TEXT("Max distance tolerance value - Fail.\n"));
      }
   MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_TOLERANCE, 
                            M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), M_DEFAULT);   
   
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Clear annotations.
   MgraClear(M_DEFAULT, GraphicList);

   // Restore the model finder context and calibrate it.
   MmodRestore(METROL_MODEL_FINDER_FILE, AilSystem, M_DEFAULT, &AilModelFinderContext);
   MmodControl(AilModelFinderContext, 0, M_ASSOCIATED_CALIBRATION, AilCalibration);

   // Allocate a result buffer.
   MmodAllocResult(AilSystem, M_DEFAULT, &AilModelFinderResult);

   // Find object occurrence.
   MmodPreprocess(AilModelFinderContext, M_DEFAULT);
   MmodFind(AilModelFinderContext, AilImage, AilModelFinderResult);

   // Get number of found occurrences.
   MmodGetResult(AilModelFinderResult, M_GENERAL, M_NUMBER, &Value);

   if(Value==1)
      {
      MmodDraw(M_DEFAULT, AilModelFinderResult, GraphicList, 
                      M_DRAW_POSITION+M_DRAW_BOX, M_DEFAULT, M_DEFAULT);
      MosPrintf(AIL_TEXT("Found occurrence using Model Finder.\n"));
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Clear annotations.
      MgraClear(M_DEFAULT, GraphicList);

      // Set the new context position.
      FixtureOffset = McalAlloc(AilSystem, M_FIXTURING_OFFSET, M_DEFAULT, M_NULL);
      McalFixture(AilImage, FixtureOffset, M_MOVE_RELATIVE, M_RESULT_MOD, AilModelFinderResult, 0, M_DEFAULT, M_DEFAULT, M_DEFAULT);

      // Calculate.
      MmetCalculate(AilMetrolContext, AilImage, AilMetrolResult, M_DEFAULT);

      // Draw features.
      MgraControl(M_DEFAULT, M_COLOR, REGION_COLOR);
      MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, 
                          M_DRAW_REGION, M_DEFAULT, M_DEFAULT);
      MosPrintf(AIL_TEXT("Regions used to calculate measured ")
                AIL_TEXT("features at the new location.\n"));
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Clear annotations.
      MgraClear(M_DEFAULT, GraphicList);

      MgraControl(M_DEFAULT, M_COLOR, FEATURE_COLOR);
      MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, 
                              M_DRAW_FEATURE, M_DEFAULT, M_DEFAULT);
      MosPrintf(AIL_TEXT("Calculated features.\n"));

      MmetGetResult(AilMetrolResult, M_FEATURE_LABEL(CIRCLE1_LABEL), M_RADIUS, &Value);
      MosPrintf(AIL_TEXT("- first measured circle:   radius=%.2fmm\n"), Value);

      MmetGetResult(AilMetrolResult, M_FEATURE_LABEL(CIRCLE2_LABEL), M_RADIUS, &Value);
      MosPrintf(AIL_TEXT("- second measured circle:  radius=%.2fmm\n"), Value);

      MmetGetResult(AilMetrolResult, M_FEATURE_LABEL(SEGMENT1_LABEL), M_LENGTH, &Value);
      MosPrintf(AIL_TEXT("- first measured segment:  length=%.2fmm\n"), Value);

      MmetGetResult(AilMetrolResult, M_FEATURE_LABEL(SEGMENT2_LABEL), M_LENGTH, &Value);
      MosPrintf(AIL_TEXT("- second measured segment: length=%.2fmm\n"), Value);

      MosPrintf(AIL_TEXT("- two measured points\n"));

      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Get angularity tolerance status and value.
      MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(ANGULARITY_LABEL), M_STATUS, 
                                                                          &Status);
      MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(ANGULARITY_LABEL), M_VALUE, 
                                                                          &Value);

      if(Status==M_PASS)
         {
         MgraControl(M_DEFAULT, M_COLOR, PASS_COLOR);
         MosPrintf(AIL_TEXT("Angularity value: %.2f degrees.\n"), Value);
         }
      else
         {
         MgraControl(M_DEFAULT, M_COLOR, FAIL_COLOR);
         MosPrintf(AIL_TEXT("Angularity value - Fail.\n"));
         }
      MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_TOLERANCE, 
                                   M_TOLERANCE_LABEL(ANGULARITY_LABEL), M_DEFAULT);   

      // Get min distance tolerance status and value.
      MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), M_STATUS, 
                                                                            &Status);
      MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), M_VALUE, 
                                                                            &Value);

      if(Status==M_PASS)
         {
         MgraControl(M_DEFAULT, M_COLOR, PASS_COLOR);
         MosPrintf(AIL_TEXT("Min distance tolerance value: %.2f mm.\n"), Value);
         }
      else
         {
         MgraControl(M_DEFAULT, M_COLOR, FAIL_COLOR);
         MosPrintf(AIL_TEXT("Min distance tolerance value - Fail.\n"));
         }
      MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_TOLERANCE, 
                                  M_TOLERANCE_LABEL(MIN_DISTANCE_LABEL), M_DEFAULT);   

      // Get max distance tolerance status and value.
      MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), M_STATUS, 
                                                                            &Status);
      MmetGetResult(AilMetrolResult, M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), M_VALUE, 
                                                                            &Value);

      if(Status==M_PASS)
         {
         MgraControl(M_DEFAULT, M_COLOR, PASS_COLOR);
         MosPrintf(AIL_TEXT("Max distance tolerance value: %.2f mm.\n"), Value);
         }
      else
         {
         MgraControl(M_DEFAULT, M_COLOR, FAIL_COLOR);
         MosPrintf(AIL_TEXT("Max distance tolerance value - Fail.\n"));
         }
      MmetDraw(M_DEFAULT, AilMetrolResult, GraphicList, M_DRAW_TOLERANCE, 
                                  M_TOLERANCE_LABEL(MAX_DISTANCE_LABEL), M_DEFAULT);
      
      MosPrintf(AIL_TEXT("Press any key to quit.\n\n"));
      MosGetch();

      // Free the calfixture allocation.
      McalFree(FixtureOffset);
      }
   else
      {
      MosPrintf(AIL_TEXT("Occurrence not found.\n"));
      MosPrintf(AIL_TEXT("Press any key to quit.\n\n"));
      MosGetch();
      }

   // Free all allocations.
   MgraFree(GraphicList);
   MmodFree(AilModelFinderContext);
   MmodFree(AilModelFinderResult);
   MmetFree(AilMetrolResult);
   MmetFree(AilMetrolContext);
   McalFree(AilCalibration);
   MbufFree(AilImage);
   }

```
