---
title: "Mmod"
description: "This example demonstrates the usage of the Geometric Model Finder module."
ms.language: "C++"
---

# Mmod

> This example demonstrates the usage of the Geometric Model Finder module.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufFree`, `MbufLoad`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MmodAlloc`, `MmodAllocResult`, `MmodControl`, `MmodDefine`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Finding/Locating, Modules, Buffer, Display, Graphics, Geometric Model Finder, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MMod.cpp
//
// Description: This program uses the Geometric Model Finder module to define geometric 
//              models and searches for these models in a target image. A simple single 
//              model example (1 model, 1 occurrence, good search conditions) is  
//              presented first, followed by a more complex example (multiple models, 
//              multiple occurrences in a complex scene with bad search conditions).
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////
 
#include <AIL.h>

// Example functions declarations.
void SingleModelExample(AIL_ID AilSystem, AIL_ID AilDisplay);
void MultipleModelsExample(AIL_ID AilSystem, AIL_ID AilDisplay);

//****************************************************************************
// Main. 
//****************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication,     // Application identifier.
          AilSystem,          // System Identifier.     
          AilDisplay;         // Display identifier.    


   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Run single model example.
   SingleModelExample(AilSystem, AilDisplay);

   // Run multiple model example.
   MultipleModelsExample(AilSystem, AilDisplay);

   // Free defaults.
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }


//****************************************************************************
// Single model example.

// Source image file specifications.
#define SINGLE_MODEL_IMAGE          M_IMAGE_PATH AIL_TEXT("SingleModel.mim")

// Target image file specifications.
#define SINGLE_MODEL_TARGET_IMAGE   M_IMAGE_PATH AIL_TEXT("SingleTarget.mim")

// Search speed: M_VERY_HIGH for faster search, M_MEDIUM for precision and robustness.
#define SINGLE_MODEL_SEARCH_SPEED   M_VERY_HIGH 

// Model specifications.
#define MODEL_OFFSETX               176L
#define MODEL_OFFSETY               136L
#define MODEL_SIZEX                 128L
#define MODEL_SIZEY                 128L
#define MODEL_MAX_OCCURRENCES       16L

void SingleModelExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID      AilImage,                         // Image buffer identifier.
               GraphicList;                      // Graphic list identifier.
   AIL_ID      AilSearchContext,                 // Search context          
               AilResult;                        // Result identifier.      
   AIL_DOUBLE  ModelDrawColor = M_COLOR_RED;     // Model draw color.       
   AIL_INT     Model[MODEL_MAX_OCCURRENCES],     // Model index.            
               NumResults  = 0L;                 // Number of results found.
   AIL_DOUBLE  Score[MODEL_MAX_OCCURRENCES],     // Model correlation score.
               XPosition[MODEL_MAX_OCCURRENCES], // Model X position.       
               YPosition[MODEL_MAX_OCCURRENCES], // Model Y position.       
               Angle[MODEL_MAX_OCCURRENCES],     // Model occurrence angle. 
               Scale[MODEL_MAX_OCCURRENCES],     // Model occurrence scale. 
               Time = 0.0;                       // Bench variable.         
   int         i;                                // Loop variable.          


   // Restore the model image and display it.
   MbufRestore(SINGLE_MODEL_IMAGE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &GraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);
   
   // Allocate a Geometric Model Finder context.
   MmodAlloc(AilSystem, M_GEOMETRIC, M_DEFAULT, &AilSearchContext);

   // Allocate a result buffer.
   MmodAllocResult(AilSystem, M_DEFAULT, &AilResult);

   // Define the model.
   MmodDefine(AilSearchContext, M_IMAGE, AilImage,
              MODEL_OFFSETX, MODEL_OFFSETY, MODEL_SIZEX, MODEL_SIZEY);

   // Set the search speed.
   MmodControl(AilSearchContext, M_CONTEXT, M_SPEED, SINGLE_MODEL_SEARCH_SPEED);

   // Preprocess the search context.
   MmodPreprocess(AilSearchContext, M_DEFAULT);
   
   // Draw box and position it in the source image to show the model.
   MgraControl(M_DEFAULT, M_COLOR, ModelDrawColor);
   MmodDraw(M_DEFAULT, AilSearchContext, GraphicList,
            M_DRAW_BOX+M_DRAW_POSITION, 0, M_ORIGINAL);

   // Pause to show the model.
   MosPrintf(AIL_TEXT("\nGEOMETRIC MODEL FINDER:\n"));
   MosPrintf(AIL_TEXT("-----------------------\n\n"));
   MosPrintf(AIL_TEXT("A model context was defined with "));
   MosPrintf(AIL_TEXT("the model in the displayed image.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();
   
   // Clear annotations.
   MgraClear(M_DEFAULT, GraphicList);

   // Load the single model target image.
   MbufLoad(SINGLE_MODEL_TARGET_IMAGE, AilImage);   

   // Dummy first call for bench measure purpose only (bench stabilization, 
   // cache effect, etc...). This first call is NOT required by the application.
   MmodFind(AilSearchContext, AilImage, AilResult);

   // Reset the timer.
   MappTimer(M_DEFAULT, M_TIMER_RESET+M_SYNCHRONOUS, M_NULL);
   
   // Find the model.
   MmodFind(AilSearchContext, AilImage, AilResult);

   // Read the find time.
   MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &Time);

   // Get the number of models found.
   MmodGetResult(AilResult, M_DEFAULT, M_NUMBER+M_TYPE_AIL_INT, &NumResults);

   // If a model was found above the acceptance threshold.
   if ( (NumResults >= 1) && (NumResults <= MODEL_MAX_OCCURRENCES) )
      {
      // Get the results of the single model.
      MmodGetResult(AilResult, M_DEFAULT, M_INDEX+M_TYPE_AIL_INT, Model);
      MmodGetResult(AilResult, M_DEFAULT, M_POSITION_X, XPosition);
      MmodGetResult(AilResult, M_DEFAULT, M_POSITION_Y, YPosition);
      MmodGetResult(AilResult, M_DEFAULT, M_ANGLE, Angle);
      MmodGetResult(AilResult, M_DEFAULT, M_SCALE, Scale);
      MmodGetResult(AilResult, M_DEFAULT, M_SCORE, Score);

      // Print the results for each model found.
      MosPrintf(AIL_TEXT("The model was found in the target image:\n\n"));
      MosPrintf(AIL_TEXT("Result   Model   X Position   Y Position   ")
                                  AIL_TEXT("Angle   Scale   Score\n\n"));      
      for (i=0; i<NumResults; i++)         
         {
         MosPrintf(AIL_TEXT("%-9d%-8d%-13.2f%-13.2f%-8.2f%-8.2f%-5.2f%%\n"), 
               i, (int) Model[i], XPosition[i], YPosition[i], 
               Angle[i], Scale[i], Score[i]);                 
         }
      MosPrintf(AIL_TEXT("\nThe search time is %.1f ms\n\n"), Time*1000.0);
      
      // Draw edges, position and box over the occurrences that were found.
      for (i=0; i<NumResults; i++)
         {         
         MgraControl(M_DEFAULT, M_COLOR, ModelDrawColor);
         MmodDraw(M_DEFAULT,  AilResult, GraphicList,
                  M_DRAW_EDGES+M_DRAW_BOX+M_DRAW_POSITION, i, M_DEFAULT);
         }
      }
   else
      {
      MosPrintf(AIL_TEXT("The model was not found or the number of models ")
                                         AIL_TEXT("found is greater than\n"));
      MosPrintf(AIL_TEXT("the specified maximum number of occurrence !\n\n"));
      }

   // Wait for a key to be pressed.
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Free objects.
   MgraFree(GraphicList);
   MbufFree(AilImage);
   MmodFree(AilSearchContext);
   MmodFree(AilResult);
   }


//****************************************************************************
// Multiple models example.

// Source image file specifications.
#define MULTI_MODELS_IMAGE          M_IMAGE_PATH AIL_TEXT("MultipleModel.mim")

// Target image file specifications.
#define MULTI_MODELS_TARGET_IMAGE   M_IMAGE_PATH AIL_TEXT("MultipleTarget.mim")

// Search speed: M_VERY_HIGH for faster search, M_MEDIUM for precision and robustness.
#define MULTI_MODELS_SEARCH_SPEED   M_VERY_HIGH 

// Number of models.
#define NUMBER_OF_MODELS            3L
#define MODELS_MAX_OCCURRENCES     16L

// Model 1 specifications.
#define MODEL0_OFFSETX              34L
#define MODEL0_OFFSETY              93L
#define MODEL0_SIZEX                214L
#define MODEL0_SIZEY                76L
#define MODEL0_DRAW_COLOR           M_COLOR_RED

// Model 2 specifications.
#define MODEL1_OFFSETX              73L
#define MODEL1_OFFSETY              232L
#define MODEL1_SIZEX                150L
#define MODEL1_SIZEY                154L
#define MODEL1_REFERENCEX           23L
#define MODEL1_REFERENCEY           127L
#define MODEL1_DRAW_COLOR           M_COLOR_GREEN

// Model 3 specifications.
#define MODEL2_OFFSETX              308L
#define MODEL2_OFFSETY              39L
#define MODEL2_SIZEX                175L
#define MODEL2_SIZEY                357L
#define MODEL2_REFERENCEX           62L
#define MODEL2_REFERENCEY           150L
#define MODEL2_DRAW_COLOR           M_COLOR_BLUE

// Models array specifications.
#define MODELS_ARRAY_SIZE      3L
#define MODELS_OFFSETX         {MODEL0_OFFSETX, MODEL1_OFFSETX, MODEL2_OFFSETX}
#define MODELS_OFFSETY         {MODEL0_OFFSETY, MODEL1_OFFSETY, MODEL2_OFFSETY}
#define MODELS_SIZEX           {MODEL0_SIZEX, MODEL1_SIZEX, MODEL2_SIZEX}
#define MODELS_SIZEY           {MODEL0_SIZEY, MODEL1_SIZEY, MODEL2_SIZEY}
#define MODELS_DRAW_COLOR      {MODEL0_DRAW_COLOR, MODEL1_DRAW_COLOR, MODEL2_DRAW_COLOR}


void MultipleModelsExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID     AilImage,                                         // Image buffer identifier. 
              GraphicList;                                      // Graphic list identifier. 
   AIL_ID     AilSearchContext,                                 // Search context           
              AilResult;                                        // Result identifier.       
   AIL_INT    Models[MODELS_MAX_OCCURRENCES],                   // Model indices.           
              ModelsOffsetX[MODELS_ARRAY_SIZE] = MODELS_OFFSETX,// Model X offsets array.   
              ModelsOffsetY[MODELS_ARRAY_SIZE] = MODELS_OFFSETY,// Model Y offsets array.   
              ModelsSizeX[MODELS_ARRAY_SIZE]   = MODELS_SIZEX,  // Model X sizes array.     
              ModelsSizeY[MODELS_ARRAY_SIZE]   = MODELS_SIZEY;  // Model Y sizes array.     
   AIL_DOUBLE ModelsDrawColor[MODELS_ARRAY_SIZE]=MODELS_DRAW_COLOR; // Model drawing colors.
   AIL_INT    NumResults  = 0L;                                 // Number of results found. 
   AIL_DOUBLE Score[MODELS_MAX_OCCURRENCES],                    // Model correlation scores.
              XPosition[MODELS_MAX_OCCURRENCES],                // Model X positions.       
              YPosition[MODELS_MAX_OCCURRENCES],                // Model Y positions.       
              Angle[MODELS_MAX_OCCURRENCES],                    // Model occurrence angles. 
              Scale[MODELS_MAX_OCCURRENCES],                    // Model occurrence scales. 
              Time = 0.0;                                       // Time variable.           
   int        i;                                                // Loop variable            

   // Restore the model image and display it.
   MbufRestore(MULTI_MODELS_IMAGE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &GraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

   // Allocate a geometric model finder.
   MmodAlloc(AilSystem, M_GEOMETRIC, M_DEFAULT, &AilSearchContext);

   // Allocate a result buffer.
   MmodAllocResult(AilSystem, M_DEFAULT, &AilResult);

   // Define the models.
   for (i=0; i<NUMBER_OF_MODELS; i++)
      {
      MmodDefine(AilSearchContext, M_IMAGE, AilImage,
                 (AIL_DOUBLE)ModelsOffsetX[i], (AIL_DOUBLE)ModelsOffsetY[i], 
                 (AIL_DOUBLE)ModelsSizeX[i], (AIL_DOUBLE)ModelsSizeY[i]);
      }

   // Set the desired search speed.
   MmodControl(AilSearchContext, M_CONTEXT, M_SPEED, MULTI_MODELS_SEARCH_SPEED);

   // Increase the smoothness for the edge extraction in the search context.
   MmodControl(AilSearchContext, M_CONTEXT, M_SMOOTHNESS, 75);

   // Modify the acceptance and the certainty for all the models that were defined.
   MmodControl(AilSearchContext, M_DEFAULT, M_ACCEPTANCE, 40);
   MmodControl(AilSearchContext, M_DEFAULT, M_CERTAINTY,  60);

   // Set the number of occurrences to 2 for all the models that were defined.
   MmodControl(AilSearchContext, M_DEFAULT, M_NUMBER, 2);

   #if (NUMBER_OF_MODELS>1)
   // Change the reference point of the second model.
   MmodControl(AilSearchContext, 1, M_REFERENCE_X, MODEL1_REFERENCEX);
   MmodControl(AilSearchContext, 1, M_REFERENCE_Y, MODEL1_REFERENCEY);

   #if (NUMBER_OF_MODELS>2)
   // Change the reference point of the third model.
   MmodControl(AilSearchContext, 2, M_REFERENCE_X, MODEL2_REFERENCEX);
   MmodControl(AilSearchContext, 2, M_REFERENCE_Y, MODEL2_REFERENCEY);
   #endif
   #endif

   // Preprocess the search context.
   MmodPreprocess(AilSearchContext, M_DEFAULT);   
   
   // Draw boxes and positions in the source image to identify the models.
   for (i=0; i<NUMBER_OF_MODELS; i++)
      {      
      MgraControl(M_DEFAULT, M_COLOR, ModelsDrawColor[i]);
      MmodDraw( M_DEFAULT, AilSearchContext, GraphicList,
                M_DRAW_BOX+M_DRAW_POSITION, i, M_ORIGINAL);
      }

   // Pause to show the models.
   MosPrintf(AIL_TEXT("A model context was defined with the ")
                  AIL_TEXT("models in the displayed image.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

    // Clear annotations.
   MgraClear(M_DEFAULT, GraphicList);


   // Load the complex target image.
   MbufLoad(MULTI_MODELS_TARGET_IMAGE, AilImage);  

   // Dummy first call for bench measure purpose only (bench stabilization, 
   // cache effect, etc...). This first call is NOT required by the application.
   MmodFind(AilSearchContext, AilImage, AilResult);

   // Reset the timer.
   MappTimer(M_DEFAULT, M_TIMER_RESET+M_SYNCHRONOUS, M_NULL);
   
   // Find the models.
   MmodFind(AilSearchContext, AilImage, AilResult);

   // Read the find time.
   MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &Time);

   // Get the number of models found.
   MmodGetResult(AilResult, M_DEFAULT, M_NUMBER+M_TYPE_AIL_INT, &NumResults);

   // If the models were found above the acceptance threshold.
   if( (NumResults >= 1) && (NumResults <= MODELS_MAX_OCCURRENCES) ) 
      {
      // Get the results for each model.
      MmodGetResult(AilResult, M_DEFAULT, M_INDEX+M_TYPE_AIL_INT, Models);
      MmodGetResult(AilResult, M_DEFAULT, M_POSITION_X, XPosition);
      MmodGetResult(AilResult, M_DEFAULT, M_POSITION_Y, YPosition);
      MmodGetResult(AilResult, M_DEFAULT, M_ANGLE, Angle);
      MmodGetResult(AilResult, M_DEFAULT, M_SCALE, Scale);
      MmodGetResult(AilResult, M_DEFAULT, M_SCORE, Score);

      // Print information about the target image.
      MosPrintf(AIL_TEXT("The models were found in the target "));
      MosPrintf(AIL_TEXT("image although there is:\n   "));
      MosPrintf(AIL_TEXT("Full rotation\n   Small scale change\n   "));
      MosPrintf(AIL_TEXT("Contrast variation\n   Specular reflection\n   "));
      MosPrintf(AIL_TEXT("Occlusion\n   Multiple models\n"));
      MosPrintf(AIL_TEXT("   Multiple occurrences\n\n"));

      // Print the results for the found models.
      MosPrintf(AIL_TEXT("Result   Model   X Position   Y Position   ")
                AIL_TEXT("Angle   Scale   Score\n\n"));      
      for (i=0; i<NumResults; i++)         
         {
         MosPrintf(AIL_TEXT("%-9d%-8d%-13.2f%-13.2f%-8.2f%-8.2f%-5.2f%%\n"), 
               i, (int) Models[i], XPosition[i], YPosition[i], 
               Angle[i], Scale[i], Score[i]);                 
         }
      MosPrintf(AIL_TEXT("\nThe search time is %.1f ms\n\n"), Time*1000.0);
      
      // Draw edges and positions over the occurrences that were found.
      for (i=0; i < NumResults; i++)
         {
         MgraControl(M_DEFAULT, M_COLOR, ModelsDrawColor[Models[i]]);
         MmodDraw(M_DEFAULT, AilResult,GraphicList,
                  M_DRAW_EDGES+M_DRAW_POSITION, i, M_DEFAULT);
         }
      }
   else
      {
      MosPrintf(AIL_TEXT("The models were not found or the number of ")
                             AIL_TEXT("models found is greater than\n"));
      MosPrintf(AIL_TEXT("the defined value of maximum occurrences !\n\n"));
      }

   // Wait for a key to be pressed.
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   // Free objects.
   MgraFree(GraphicList);
   MbufFree(AilImage);
   MmodFree(AilSearchContext);
   MmodFree(AilResult);
   }

```
