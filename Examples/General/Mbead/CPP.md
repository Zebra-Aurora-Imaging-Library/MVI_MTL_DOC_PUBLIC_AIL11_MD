---
title: "Mbead"
description: "This program performs several bead inspections on mechanical parts."
ms.language: "C++"
---

# Mbead

> This program performs several bead inspections on mechanical parts.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbeadAlloc`, `MbeadAllocResult`, `MbeadControl`, `MbeadDraw`, `MbeadFree`, `MbeadGetResult`, `MbeadInquire`, `MbeadTemplate`, `MbeadTrain`, `MbeadVerify`, `MbufFree`, `MbufRestore`, `McalAlloc`, `McalDraw`, `McalFixture`, `McalFree`, `McalUniform`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MmodAlloc`, `MmodAllocResult`, `MmodDefine`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Measuring, Modules, Buffer, Display, Graphics, Calibration, Geometric Model Finder, Bead Inspection, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Mbead.cpp
//
// Description: This program uses the Bead module to train a bead template
//              and then to inspect a defective bead in a target image.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Example functions declarations.   
void FixturedBeadExample(AIL_ID AilSystem, AIL_ID AilDisplay);
void PredefinedBeadExample(AIL_ID AilSystem, AIL_ID AilDisplay);

// Utility definitions. 
#define USER_POSITION_COLOR        M_COLOR_RED
#define USER_TEMPLATE_COLOR        M_COLOR_CYAN
#define TRAINED_BEAD_WIDTH_COLOR   M_RGB888(255, 128,   0)
#define MODEL_FINDER_COLOR         M_COLOR_GREEN
#define COORDINATE_SYSTEM_COLOR    M_RGB888(164, 164,   0)
#define RESULT_SEARCH_BOX_COLOR    M_COLOR_CYAN
#define PASS_BEAD_WIDTH_COLOR      M_COLOR_GREEN
#define PASS_BEAD_POSITION_COLOR   M_COLOR_GREEN
#define FAIL_NOT_FOUND_COLOR       M_COLOR_RED
#define FAIL_SMALL_WIDTH_COLOR     M_RGB888(255,   128, 0)
#define FAIL_EDGE_OFFSET_COLOR     M_COLOR_RED

//***************************************************************************
// Main. 
//***************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication, // Application identifier. 
      AilSystem,          // System Identifier.      
      AilDisplay;         // Display identifier.     

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Run fixtured bead example. 
   FixturedBeadExample(AilSystem, AilDisplay);

   // Run predefined bead example. 
   PredefinedBeadExample(AilSystem, AilDisplay);

   // Free defaults.     
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }

//***************************************************************************
// Fixtured 'stripe' bead example. 

// Target image specifications. 
#define IMAGE_FILE_TRAINING       M_IMAGE_PATH AIL_TEXT("BeadTraining.mim")
#define IMAGE_FILE_TARGET         M_IMAGE_PATH AIL_TEXT("BeadTarget.mim")

// Bead stripe training data definition. 
#define NUMBER_OF_TRAINING_POINTS 13

static AIL_DOUBLE  TrainingPointsX[NUMBER_OF_TRAINING_POINTS] =
   {180, 280, 400, 430, 455, 415, 370, 275, 185, 125, 105, 130, 180};
static AIL_DOUBLE  TrainingPointsY[NUMBER_OF_TRAINING_POINTS] =
   {190, 215, 190, 200, 260, 330, 345, 310, 340, 305, 265, 200, 190};

// Max angle correction. 
#define MAX_ANGLE_CORRECTION_VALUE 20.0

// Max offset deviation. 
#define MAX_DEVIATION_OFFSET_VALUE 25.0

// Maximum negative width variation. 
#define WIDTH_DELTA_NEG_VALUE       2.0

// Model region  definition. 
#define MODEL_ORIGIN_X              145
#define MODEL_ORIGIN_Y              115
#define MODEL_SIZE_X                275
#define MODEL_SIZE_Y                 60

void FixturedBeadExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID AilGraList,            // Graphic list identifier.        
          AilImageTraining,      // Image buffer identifier.        
          AilImageTarget,        // Image buffer identifier.        
          AilBeadContext,        // Bead context identifier.        
          AilBeadResult,         // Bead result identifier.         
          AilModelFinderContext, // Model finder context identifier.
          AilModelFinderResult,  // Model finder result identifier. 
          AilFixturingOffset;    // Fixturing offset identifier.    

   AIL_DOUBLE NominalWidth,      // Nominal width result value. 
              AvWidth,           // Average width result value. 
              GapCov,            // Gap coverage result value.  
              MaxGap,            // Maximum gap result value.   
              Score;             // Bead score result value.    

   // Restore source images into automatically allocated image buffers. 
   MbufRestore(IMAGE_FILE_TRAINING, AilSystem, &AilImageTraining);
   MbufRestore(IMAGE_FILE_TARGET,   AilSystem, &AilImageTarget);

   // Display the training image buffer. 
   MdispSelect(AilDisplay, AilImageTraining);

   // Allocate a graphic list to hold the subpixel annotations to draw. 
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraList);

   // Associate the graphic list to the display for annotations. 
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraList);

   // Original template image. 
   MosPrintf(AIL_TEXT("\nFIXTURED BEAD INSPECTION:\n"));
   MosPrintf(AIL_TEXT("-------------------------\n\n"));
   MosPrintf(AIL_TEXT("This program performs a bead inspection on a mechanical part.\n"));
   MosPrintf(AIL_TEXT("In the first step, a bead template context is trained using an "));
   MosPrintf(AIL_TEXT("image.\nIn the second step, a mechanical part, at an arbitrary "));
   MosPrintf(AIL_TEXT("angle and with\na defective bead, is inspected.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate a bead context. 
   MbeadAlloc(AilSystem, M_DEFAULT, M_DEFAULT, &AilBeadContext);

   // Allocate a bead result. 
   MbeadAllocResult(AilSystem, M_DEFAULT, &AilBeadResult);

   // Add a bead template. 
   MbeadTemplate(AilBeadContext, M_ADD, M_DEFAULT, M_TEMPLATE_LABEL(1), 
      NUMBER_OF_TRAINING_POINTS, TrainingPointsX, TrainingPointsY, M_NULL, M_DEFAULT);

   // Set template input units to world units.    
   MbeadControl(AilBeadContext, M_TEMPLATE_LABEL(1), M_TEMPLATE_INPUT_UNITS, M_WORLD);

   // Set the bead 'edge type' search properties. 
   MbeadControl(AilBeadContext, M_TEMPLATE_LABEL(1), M_ANGLE_ACCURACY_MAX_DEVIATION, 
      MAX_ANGLE_CORRECTION_VALUE);

   // Set the maximum valid bead deformation. 
   MbeadControl(AilBeadContext, M_TEMPLATE_LABEL(1), M_OFFSET_MAX, 
      MAX_DEVIATION_OFFSET_VALUE);

   // Set the valid bead minimum width criterion. 
   MbeadControl(AilBeadContext, M_TEMPLATE_LABEL(1), M_WIDTH_DELTA_NEG,
      WIDTH_DELTA_NEG_VALUE);

   // Display the bead polyline. 
   MgraControl(M_DEFAULT, M_COLOR, USER_TEMPLATE_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadContext, AilGraList, M_DRAW_POSITION_POLYLINE, 
      M_USER, M_ALL, M_ALL, M_DEFAULT);

   // Display the bead training points. 
   MgraControl(M_DEFAULT, M_COLOR, USER_POSITION_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadContext, AilGraList, M_DRAW_POSITION, 
      M_USER, M_ALL, M_ALL, M_DEFAULT);

   // Pause to show the template image and user points. 
   MosPrintf(AIL_TEXT("The initial points specified by the user (in red) are\n"));
   MosPrintf(AIL_TEXT("used to train the bead information from an image.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Set a 1:1 calibration to the training image. 
   McalUniform(AilImageTraining, 0, 0, 1, 1, 0, M_DEFAULT);

   // Train the bead context. 
   MbeadTrain(AilBeadContext, AilImageTraining, M_DEFAULT);

   // Display the trained bead.    
   MgraControl(M_DEFAULT, M_COLOR, TRAINED_BEAD_WIDTH_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadContext, AilGraList, M_DRAW_WIDTH, 
      M_TRAINED, M_TEMPLATE_LABEL(1), M_ALL, M_DEFAULT);

   // Retrieve the trained nominal width. 
   MbeadInquire(AilBeadContext,M_TEMPLATE_LABEL(1), M_TRAINED_WIDTH_NOMINAL, 
      &NominalWidth);

   MosPrintf(AIL_TEXT("The template has been trained and is displayed in orange.\n"));
   MosPrintf(AIL_TEXT("Its nominal trained width is %.2f pixels.\n\n"), NominalWidth);

   // Define model to further fixture the bead template. 
   MmodAlloc(AilSystem, M_GEOMETRIC, M_DEFAULT, &AilModelFinderContext);

   MmodAllocResult(AilSystem, M_DEFAULT, &AilModelFinderResult);

   MmodDefine(AilModelFinderContext, M_IMAGE, AilImageTraining, 
      MODEL_ORIGIN_X, MODEL_ORIGIN_Y, MODEL_SIZE_X, MODEL_SIZE_Y);

   // Preprocess the model. 
   MmodPreprocess(AilModelFinderContext, M_DEFAULT);   

   // Allocate a fixture object. 
   McalAlloc(AilSystem, M_FIXTURING_OFFSET, M_DEFAULT, &AilFixturingOffset);

   // Learn the relative offset of the model. 
   McalFixture(M_NULL, AilFixturingOffset, M_LEARN_OFFSET, M_MODEL_MOD, 
      AilModelFinderContext, 0, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   // Display the model. 
   MgraControl(M_DEFAULT, M_COLOR, MODEL_FINDER_COLOR);
   MmodDraw(M_DEFAULT, AilModelFinderContext, AilGraList, M_DRAW_BOX+M_DRAW_POSITION, 
      M_DEFAULT, M_ORIGINAL); 

   MosPrintf(AIL_TEXT("A Model Finder model (in green) is also defined to\n"));
   MosPrintf(AIL_TEXT("further fixture the bead verification operation.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Clear the overlay annotation. 
   MgraClear(M_DEFAULT, AilGraList);

   // Display the target image buffer. 
   MdispSelect(AilDisplay, AilImageTarget);

   // Find the location of the fixture using Model Finder. 
   MmodFind(AilModelFinderContext, AilImageTarget, AilModelFinderResult);

   // Display the found model occurrence. 
   MgraControl(M_DEFAULT, M_COLOR, MODEL_FINDER_COLOR);
   MmodDraw(M_DEFAULT, AilModelFinderResult, AilGraList, M_DRAW_BOX+M_DRAW_POSITION, 
      M_DEFAULT, M_DEFAULT);

   // Apply fixture offset to the target image. 
   McalFixture(AilImageTarget, AilFixturingOffset, M_MOVE_RELATIVE, M_RESULT_MOD, 
      AilModelFinderResult, 0, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   // Display the relative coordinate system. 
   MgraControl(M_DEFAULT, M_COLOR, COORDINATE_SYSTEM_COLOR);
   McalDraw(M_DEFAULT, M_NULL, AilGraList, M_DRAW_RELATIVE_COORDINATE_SYSTEM, 
      M_DEFAULT, M_DEFAULT);

   // Perform the inspection of the bead in the fixtured target image. 
   MbeadVerify(AilBeadContext, AilImageTarget, AilBeadResult, M_DEFAULT);

   // Display the result search boxes. 
   MgraControl(M_DEFAULT, M_COLOR, RESULT_SEARCH_BOX_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadResult, AilGraList, M_DRAW_SEARCH_BOX, M_ALL, 
      M_ALL, M_ALL, M_DEFAULT);

   MosPrintf(AIL_TEXT("The mechanical part's position and angle (in green) were "));
   MosPrintf(AIL_TEXT("located\nusing Model Finder, and the bead's search boxes "));
   MosPrintf(AIL_TEXT("(in cyan) were\npositioned accordingly.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Clear the overlay annotation. 
   MgraClear(M_DEFAULT, AilGraList);

   // Display the moved relative coordinate system. 
   MgraControl(M_DEFAULT, M_COLOR, COORDINATE_SYSTEM_COLOR);
   McalDraw(M_DEFAULT, M_NULL, AilGraList, M_DRAW_RELATIVE_COORDINATE_SYSTEM, 
      M_DEFAULT, M_DEFAULT);

   // Display the pass bead sections. 
   MgraControl(M_DEFAULT, M_COLOR, PASS_BEAD_WIDTH_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadResult, AilGraList, M_DRAW_WIDTH, M_PASS, 
      M_TEMPLATE_LABEL(1), M_ALL, M_DEFAULT);

   // Display the missing bead sections. 
   MgraControl(M_DEFAULT, M_COLOR, FAIL_NOT_FOUND_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadResult, AilGraList, M_DRAW_SEARCH_BOX, 
      M_FAIL_NOT_FOUND, M_ALL, M_ALL, M_DEFAULT);

   // Display bead sections which do not meet the minimum width criteria. 
   MgraControl(M_DEFAULT, M_COLOR, FAIL_SMALL_WIDTH_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadResult, AilGraList, M_DRAW_SEARCH_BOX, 
      M_FAIL_WIDTH_MIN, M_TEMPLATE_LABEL(1), M_ALL, M_DEFAULT);

   // Retrieve and display general bead results. 
   MbeadGetResult(AilBeadResult, M_TEMPLATE_LABEL(1), M_GENERAL, M_SCORE, &Score);
   MbeadGetResult(AilBeadResult, M_TEMPLATE_LABEL(1), M_GENERAL, M_GAP_COVERAGE, &GapCov);
   MbeadGetResult(AilBeadResult, M_TEMPLATE_LABEL(1), M_GENERAL, M_WIDTH_AVERAGE, &AvWidth);
   MbeadGetResult(AilBeadResult, M_TEMPLATE_LABEL(1), M_GENERAL, M_GAP_MAX_LENGTH, &MaxGap);

   MosPrintf(AIL_TEXT("The bead has been inspected:\n"));
   MosPrintf(AIL_TEXT(" -Passing bead sections (green) cover %.2f%% of the bead\n"), Score);
   MosPrintf(AIL_TEXT(" -Missing bead sections (red) cover %.2f%% of the bead\n"), GapCov);
   MosPrintf(AIL_TEXT(" -Sections outside the specified tolerances are drawn in orange\n"));
   MosPrintf(AIL_TEXT(" -The bead's average width is %.2f pixels\n"), AvWidth);
   MosPrintf(AIL_TEXT(" -The bead's longest gap section is %.2f pixels\n"), MaxGap);

   // Pause to show the result. 
   MosPrintf(AIL_TEXT("Press any key to continue.\n"));
   MosGetch();

   // Free all allocations. 
   MmodFree(AilModelFinderContext);
   MmodFree(AilModelFinderResult);
   MbeadFree(AilBeadContext);
   MbeadFree(AilBeadResult);
   McalFree(AilFixturingOffset);
   MbufFree(AilImageTraining);
   MbufFree(AilImageTarget);
   MgraFree(AilGraList);
   }


//***************************************************************************
// Predefined 'edge' bead example. 

// Target image specifications. 
#define CAP_FILE_TARGET         M_IMAGE_PATH AIL_TEXT("Cap.mim")

// Template attributes definition. 
#define CIRCLE_CENTER_X                330.0
#define CIRCLE_CENTER_Y                230.0
#define CIRCLE_RADIUS                  120.0

// Edge threshold value. 
#define EDGE_THRESHOLD_VALUE            25.0

// Max offset found and deviation tolerance. 
#define MAX_CONTOUR_DEVIATION_OFFSET     5.0
#define MAX_CONTOUR_FOUND_OFFSET        25.0

void PredefinedBeadExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID AilOverlayImage,   // Overlay buffer identifier.  
      AilImageTarget,        // Image buffer identifier.    
      AilBeadContext,        // Bead context identifier.    
      AilBeadResult;         // Bead result identifier.     

   AIL_DOUBLE MaximumOffset; // Maximum offset result value.

   // Restore target image into an automatically allocated image buffers. 
   MbufRestore(CAP_FILE_TARGET, AilSystem, &AilImageTarget);

   // Display the training image buffer. 
   MdispSelect(AilDisplay, AilImageTarget);

   // Prepare the overlay for annotations. 
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_TRANSPARENT_COLOR);

   // Original template image. 
   MosPrintf(AIL_TEXT("\nPREDEFINED BEAD INSPECTION:\n"));
   MosPrintf(AIL_TEXT("---------------------------\n\n"));
   MosPrintf(AIL_TEXT("This program performs a bead inspection of a bottle\n"));
   MosPrintf(AIL_TEXT("cap's contour using a predefined circular bead.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate a bead context. 
   MbeadAlloc(AilSystem, M_DEFAULT, M_DEFAULT, &AilBeadContext);

   // Allocate a bead result. 
   MbeadAllocResult(AilSystem, M_DEFAULT, &AilBeadResult);

   // Add the bead templates. 
   MbeadTemplate(AilBeadContext, M_ADD, M_BEAD_EDGE, M_TEMPLATE_LABEL(1), 
      0, M_NULL, M_NULL, M_NULL, M_DEFAULT);

   // Set the bead shape properties. 
   MbeadControl(AilBeadContext, M_TEMPLATE_LABEL(1), M_TRAINING_PATH, M_CIRCLE);

   MbeadControl(AilBeadContext, M_TEMPLATE_LABEL(1), M_TEMPLATE_CIRCLE_CENTER_X,
      CIRCLE_CENTER_X);
   MbeadControl(AilBeadContext, M_TEMPLATE_LABEL(1), M_TEMPLATE_CIRCLE_CENTER_Y,
      CIRCLE_CENTER_Y);
   MbeadControl(AilBeadContext, M_TEMPLATE_LABEL(1), M_TEMPLATE_CIRCLE_RADIUS,
      CIRCLE_RADIUS);

   // Set the edge threshold value to extract the object shape. 
   MbeadControl(AilBeadContext, M_TEMPLATE_LABEL(1), M_THRESHOLD_VALUE,
      EDGE_THRESHOLD_VALUE);

   // Using the default fixed user defined nominal edge width. 
   MbeadControl(AilBeadContext, M_ALL, M_WIDTH_NOMINAL_MODE, M_USER_DEFINED);

   // Set the maximal expected contour deformation. 
   MbeadControl(AilBeadContext, M_ALL, M_FAIL_WARNING_OFFSET, MAX_CONTOUR_FOUND_OFFSET);

   // Set the maximum valid bead deformation. 
   MbeadControl(AilBeadContext, M_ALL, M_OFFSET_MAX, MAX_CONTOUR_DEVIATION_OFFSET);      

   // Display the bead in the overlay image. 
   MgraControl(M_DEFAULT, M_COLOR, USER_TEMPLATE_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadContext, AilOverlayImage, M_DRAW_POSITION, 
      M_USER, M_ALL, M_ALL, M_DEFAULT);

   // The bead template is entirely defined and is trained without sample image. 
   MbeadTrain(AilBeadContext, M_NULL, M_DEFAULT);

   // Display the trained bead. 
   MgraControl(M_DEFAULT, M_COLOR, TRAINED_BEAD_WIDTH_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadContext, AilOverlayImage, M_DRAW_SEARCH_BOX, 
      M_TRAINED, M_ALL, M_ALL, M_DEFAULT);

   // Pause to show the template image and user points. 
   MosPrintf(AIL_TEXT("A circular template that was parametrically defined by the\n"));
   MosPrintf(AIL_TEXT("user is displayed (in cyan). The template has been trained\n")); 
   MosPrintf(AIL_TEXT("and the resulting search is displayed (in orange).\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Perform the inspection of the bead in the fixtured target image. 
   MbeadVerify(AilBeadContext, AilImageTarget, AilBeadResult, M_DEFAULT);

   // Clear the overlay annotation. 
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_TRANSPARENT_COLOR);

   // Display the pass bead sections. 
   MgraControl(M_DEFAULT, M_COLOR, PASS_BEAD_POSITION_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadResult, AilOverlayImage, M_DRAW_POSITION, M_PASS, 
      M_ALL, M_ALL, M_DEFAULT);

   // Display the offset bead sections. 
   MgraControl(M_DEFAULT, M_COLOR, FAIL_EDGE_OFFSET_COLOR);
   MbeadDraw(M_DEFAULT, AilBeadResult, AilOverlayImage, M_DRAW_POSITION, M_FAIL_OFFSET, 
      M_ALL, M_ALL, M_DEFAULT);

   // Retrieve and display general bead results. 
   MbeadGetResult(AilBeadResult, M_TEMPLATE_LABEL(1), M_GENERAL, M_OFFSET_MAX,
      &MaximumOffset);

   MosPrintf(AIL_TEXT("The bottle cap shape has been inspected:\n"));
   MosPrintf(AIL_TEXT(" -Sections outside the specified offset"));
   MosPrintf(AIL_TEXT(" tolerance are drawn in red\n"));
   MosPrintf(AIL_TEXT(" -The maximum offset value is %0.2f pixels.\n\n"), MaximumOffset);

   // Pause to show the result. 
   MosPrintf(AIL_TEXT("Press any key to terminate.\n"));
   MosGetch();

   // Free all allocations. 
   MbeadFree(AilBeadContext);
   MbeadFree(AilBeadResult);
   MbufFree(AilImageTarget);
   }

```
