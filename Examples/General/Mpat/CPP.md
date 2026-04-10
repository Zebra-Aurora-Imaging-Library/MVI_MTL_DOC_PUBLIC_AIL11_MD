---
title: "Mpat"
description: "This example demonstrates the usage of the Pattern Matching module."
ms.language: "C++"
---

# Mpat

> This example demonstrates the usage of the Pattern Matching module.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MappGetError`, `MappTimer`, `MbufAlloc2d`, `MbufChild2d`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraControl`, `MgraFree`, `MimRotate`, `MimTranslate`, `MpatAlloc`, `MpatAllocResult`, `MpatControl`, `MpatDefine`, `MpatDraw`, `MpatFind`, `MpatFree`, `MpatGetResult`, `MpatInquire`, `MpatPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Semiconductor, Applications, Finding/Locating, Modules, Buffer, Display, Graphics, Image Processing, Pattern Matching, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MPat.cpp
//
// Description: This program contains 4 examples of the pattern matching module:
//
//              The first example defines a model and then searches for it in a shifted
//              version of the image (without rotation).
//           
//              The second example defines a regular model and then searches for it in a
//              rotated version of the image using search angle range.
//           
//              The third example defines a rotated model at certain angle and then
//              searches for it in a rotated version of the image.
//           
//              The fourth example automatically allocates a model in a wafer image and 
//              finds its horizontal and vertical displacement.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>
#include <math.h>

// Example functions declarations.
void SearchModelExample(AIL_ID AilSystem, AIL_ID AilDisplay);
void SearchModelAngleRangeExample(AIL_ID AilSystem, AIL_ID AilDisplay);
void SearchModelAtAngleExample(AIL_ID AilSystem, AIL_ID AilDisplay);
void AutoAllocationModelExample(AIL_ID AilSystem, AIL_ID AilDisplay);

// *****************************************************************************
// Main.
// *****************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication,     // Application identifier.
          AilSystem,          // System identifier.     
          AilDisplay;         // Display identifier.    

   MosPrintf(AIL_TEXT("\nGRAYSCALE PATTERN MATCHING:\n"));
   MosPrintf(AIL_TEXT("---------------------------\n\n"));

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Run the search at 0 degree example.
   SearchModelExample(AilSystem, AilDisplay);

   // Run the search over 360 degrees example.
   SearchModelAngleRangeExample(AilSystem, AilDisplay);

   // Run the search rotated model example.
   SearchModelAtAngleExample(AilSystem, AilDisplay);

   // Run the automatic model allocation example.
   AutoAllocationModelExample(AilSystem, AilDisplay);

   // Free defaults.
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }


//****************************************************************************
// Find model in shifted version of the image example.

// Source image file name.
#define FIND_IMAGE_FILE    M_IMAGE_PATH AIL_TEXT("CircuitsBoard.mim")

// Model position and size.
#define FIND_MODEL_X_POS   153L
#define FIND_MODEL_Y_POS   132L
#define FIND_MODEL_WIDTH   128L
#define FIND_MODEL_HEIGHT  128L
#define FIND_MODEL_X_CENTER   (FIND_MODEL_X_POS+(FIND_MODEL_WIDTH -1)/2.0)
#define FIND_MODEL_Y_CENTER   (FIND_MODEL_Y_POS+(FIND_MODEL_HEIGHT-1)/2.0)

// Target image shifting values.
#define FIND_SHIFT_X        4.5
#define FIND_SHIFT_Y        7.5

// Minimum match score to determine acceptability of model (default).
#define FIND_MODEL_MIN_MATCH_SCORE 70.0

// Minimum accuracy for the search.
#define FIND_MODEL_MIN_ACCURACY    0.1

// Absolute value macro.
#define AbsoluteValue(x) (((x) < 0.0) ? -(x) : (x))

void SearchModelExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID     AilImage,                // Image buffer identifier.
              GraphicList,             // Graphic list identifier.
              ContextId,               // ContextId identifier.   
              Result;                  // Result identifier.      
   AIL_INT    NumResults;              // Number of results found 
   AIL_DOUBLE XOrg = 0.0, YOrg = 0.0;  // Original model position.
   AIL_DOUBLE x    = 0.0, y    = 0.0;  // Model position.         
   AIL_DOUBLE ErrX = 0.0, ErrY = 0.0;  // Model error position.   
   AIL_DOUBLE Score = 0.0;             // Model correlation score.
   AIL_DOUBLE Time  = 0.0;             // Model search time.      
   AIL_DOUBLE AnnotationColor = M_COLOR_GREEN; // Drawing color.  

   // Restore source image into an automatically allocated image buffer.
   MbufRestore(FIND_IMAGE_FILE, AilSystem, &AilImage);

   // Display the image buffer.
   MdispSelect(AilDisplay, AilImage);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &GraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

   // Allocate a normalized pattern matching context.
   MpatAlloc(AilSystem, M_NORMALIZED, M_DEFAULT, &ContextId);

   // Define a regular model.
   MpatDefine(ContextId, M_REGULAR_MODEL, AilImage, FIND_MODEL_X_POS,
              FIND_MODEL_Y_POS, FIND_MODEL_WIDTH, FIND_MODEL_HEIGHT, M_DEFAULT);

   // Set the search accuracy to high.
   MpatControl(ContextId, M_DEFAULT, M_ACCURACY, M_HIGH);

   // Set the search model speed to high.
   MpatControl(ContextId, M_DEFAULT, M_SPEED, M_HIGH);

   // Preprocess the model.
   MpatPreprocess(ContextId, M_DEFAULT, AilImage);

   // Draw a box around the model in the model image.
   MgraControl(M_DEFAULT, M_COLOR, AnnotationColor);
   MpatDraw(M_DEFAULT, ContextId, GraphicList, M_DRAW_BOX + M_DRAW_POSITION,
                                                                  M_DEFAULT, M_ORIGINAL);
   // Pause to show the original image and model position.
   MosPrintf(AIL_TEXT("\nA %ldx%ld model was defined in the source image.\n"),
                             FIND_MODEL_WIDTH, FIND_MODEL_HEIGHT);
   MosPrintf(AIL_TEXT("It will be found in an image shifted ")
             AIL_TEXT("by %.2f in X and %.2f in Y.\n"), FIND_SHIFT_X, FIND_SHIFT_Y);
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Clear annotations.
   MgraClear(M_DEFAULT, GraphicList);

   // Translate the image on a subpixel level.
   MimTranslate(AilImage, AilImage, FIND_SHIFT_X, FIND_SHIFT_Y, M_DEFAULT);

   // Allocate result buffer.
   MpatAllocResult(AilSystem, M_DEFAULT, &Result);

   // Dummy first call for bench measure purpose only (bench stabilization,
   // cache effect, etc...). This first call is NOT required by the application.
   MpatFind(ContextId, AilImage, Result);
   MappTimer(M_DEFAULT, M_TIMER_RESET + M_SYNCHRONOUS, M_NULL);

   // Find the model in the target buffer.
   MpatFind(ContextId, AilImage, Result);

   // Read the time spent in MpatFind.
   MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &Time);

   // If one model was found above the acceptance threshold.
   MpatGetResult(Result, M_GENERAL, M_NUMBER+M_TYPE_AIL_INT, &NumResults);
   if(NumResults == 1L)
      {
      // Read results and draw a box around the model occurrence.
      MpatGetResult(Result, M_DEFAULT, M_POSITION_X, &x);
      MpatGetResult(Result, M_DEFAULT, M_POSITION_Y, &y);
      MpatGetResult(Result, M_DEFAULT, M_SCORE, &Score);
      MgraControl(M_DEFAULT, M_COLOR, AnnotationColor);
      MpatDraw(M_DEFAULT, Result, GraphicList, M_DRAW_BOX + M_DRAW_POSITION,
                                                              M_DEFAULT, M_DEFAULT);

      // Calculate the position errors in X and Y and inquire original model position.
      ErrX = fabs((FIND_MODEL_X_CENTER + FIND_SHIFT_X) - x);
      ErrY = fabs((FIND_MODEL_Y_CENTER + FIND_SHIFT_Y) - y);
      MpatInquire(ContextId, M_DEFAULT, M_ORIGINAL_X, &XOrg);
      MpatInquire(ContextId, M_DEFAULT, M_ORIGINAL_Y, &YOrg);

      // Print out the search result of the model in the original image.
      MosPrintf(AIL_TEXT("Search results:\n"));
      MosPrintf(AIL_TEXT("---------------------------------------------------\n"));
      MosPrintf(AIL_TEXT("The model is found to be shifted by \tX:%.2f, Y:%.2f.\n"),
                                                                    x - XOrg, y - YOrg);
      MosPrintf(AIL_TEXT("The model position error is \t\tX:%.2f, Y:%.2f\n"),
                                                                 ErrX, ErrY);
      MosPrintf(AIL_TEXT("The model match score is \t\t%.1f\n"), Score);
      MosPrintf(AIL_TEXT("The search time is \t\t\t%.3f ms\n\n"), Time*1000.0);

      // Verify the results.
      if (
          (AbsoluteValue((x - XOrg) - FIND_SHIFT_X) > FIND_MODEL_MIN_ACCURACY) ||
          (AbsoluteValue((y - YOrg) - FIND_SHIFT_Y) > FIND_MODEL_MIN_ACCURACY) ||
          (Score                              < FIND_MODEL_MIN_MATCH_SCORE)
         )
         MosPrintf(AIL_TEXT("Results verification error !\n"));
      }
   else
      MosPrintf(AIL_TEXT("Model not found !\n"));

   // Wait for a key to be pressed.
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Clear annotations.
   MgraClear(M_DEFAULT, GraphicList);

   // Free all allocations.
   MgraFree(GraphicList);
   MpatFree(Result);
   MpatFree(ContextId);
   MbufFree(AilImage);
   }


//****************************************************************************
// Find rotated model example.

// Source image file name.
#define ROTATED_FIND_IMAGE_FILE      M_IMAGE_PATH AIL_TEXT("CircuitsBoard.mim")

// Image rotation values.
#define ROTATED_FIND_ROTATION_DELTA_ANGLE  10
#define ROTATED_FIND_ROTATION_ANGLE_STEP   1
#define ROTATED_FIND_RAD_PER_DEG           0.01745329251

// Model position and size.
#define ROTATED_FIND_MODEL_X_POS           153L
#define ROTATED_FIND_MODEL_Y_POS           132L
#define ROTATED_FIND_MODEL_WIDTH           128L
#define ROTATED_FIND_MODEL_HEIGHT          128L

#define ROTATED_FIND_MODEL_X_CENTER           ROTATED_FIND_MODEL_X_POS+ \
                                             (ROTATED_FIND_MODEL_WIDTH -1)/2.0
#define ROTATED_FIND_MODEL_Y_CENTER           ROTATED_FIND_MODEL_Y_POS+ \
                                             (ROTATED_FIND_MODEL_HEIGHT-1)/2.0


// Minimum accuracy for the search position.
#define ROTATED_FIND_MIN_POSITION_ACCURACY     0.10

// Minimum accuracy for the search angle.
#define ROTATED_FIND_MIN_ANGLE_ACCURACY        0.25

// Angle range to search.
#define ROTATED_FIND_ANGLE_DELTA_POS           ROTATED_FIND_ROTATION_DELTA_ANGLE
#define ROTATED_FIND_ANGLE_DELTA_NEG           ROTATED_FIND_ROTATION_DELTA_ANGLE

// Prototypes of utility functions.
void RotateModelCenter(AIL_ID Buffer,
                       AIL_DOUBLE *X,
                       AIL_DOUBLE *Y,
                       AIL_DOUBLE Angle);

AIL_DOUBLE CalculateAngleDist(AIL_DOUBLE Angle1,
                              AIL_DOUBLE Angle2);


void SearchModelAngleRangeExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID      AilSourceImage,              // Model  image buffer identifier.  
               AilTargetImage,              // Target image buffer identifier.  
               AilDisplayImage,             // Target image buffer identifier.  
               GraphicList,                 // Graphic list.                    
               AilContextId,                // Model identifier.                
               AilResult;                   // Result identifier.               
   AIL_DOUBLE  RealX       = 0.0,           // Model real position in x.        
               RealY       = 0.0,           // Model real position in y.        
               RealAngle   = 0.0,           // Model real angle.                
               X           = 0.0,           // Model position in x found.       
               Y           = 0.0,           // Model position in y found.       
               Angle       = 0.0,           // Model angle found.               
               Score       = 0.0,           // Model correlation score.         
               Time        = 0.0,           // Model search time.               
               ErrX        = 0.0,           // Model error position in x.       
               ErrY        = 0.0,           // Model error position in y.       
               ErrAngle    = 0.0,           // Model error angle.               
               SumErrX     = 0.0,           // Model total error position in x. 
               SumErrY      = 0.0,          // Model total error position in y. 
               SumErrAngle  = 0.0,          // Model total error angle.         
               SumTime      = 0.0;          // Model total search time.         
   AIL_INT     NumResults;                  // Number of results found          
   AIL_INT     NbFound      = 0;            // Number of models found.          
   AIL_DOUBLE  AnnotationColor = M_COLOR_GREEN; // Drawing color.               

   // Load target image into image buffers and display it.
   MbufRestore(ROTATED_FIND_IMAGE_FILE, AilSystem, &AilSourceImage);
   MbufRestore(ROTATED_FIND_IMAGE_FILE, AilSystem, &AilTargetImage);
   MbufRestore(ROTATED_FIND_IMAGE_FILE, AilSystem, &AilDisplayImage);
   MdispSelect(AilDisplay, AilDisplayImage);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &GraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

   // Allocate a normalized pattern matching context.
   MpatAlloc(AilSystem, M_NORMALIZED, M_DEFAULT, &AilContextId);

   // Define a regular model.
   MpatDefine(AilContextId, M_REGULAR_MODEL + M_CIRCULAR_OVERSCAN, AilSourceImage,
              ROTATED_FIND_MODEL_X_POS, ROTATED_FIND_MODEL_Y_POS,
              ROTATED_FIND_MODEL_WIDTH, ROTATED_FIND_MODEL_HEIGHT, M_DEFAULT);

   // Set the search model speed.
   MpatControl(AilContextId, M_DEFAULT, M_SPEED, M_MEDIUM);

   // Set the position search accuracy.
   MpatControl(AilContextId, M_DEFAULT, M_ACCURACY, M_HIGH);

   // Activate the search model angle mode.
   MpatControl(AilContextId, M_DEFAULT, M_SEARCH_ANGLE_MODE, M_ENABLE);

   // Set the search model range angle.
   MpatControl(AilContextId, M_DEFAULT, M_SEARCH_ANGLE_DELTA_NEG, ROTATED_FIND_ANGLE_DELTA_NEG);
   MpatControl(AilContextId, M_DEFAULT, M_SEARCH_ANGLE_DELTA_POS, ROTATED_FIND_ANGLE_DELTA_POS);

   // Set the search model angle accuracy.
   MpatControl(AilContextId, M_DEFAULT, M_SEARCH_ANGLE_ACCURACY, ROTATED_FIND_MIN_ANGLE_ACCURACY);

   // Set the search model angle interpolation mode to bilinear.
   MpatControl(AilContextId, M_DEFAULT, M_SEARCH_ANGLE_INTERPOLATION_MODE, M_BILINEAR);

   // Preprocess the model.
   MpatPreprocess(AilContextId, M_DEFAULT, AilSourceImage);

   // Allocate a result buffer.
   MpatAllocResult(AilSystem, M_DEFAULT, &AilResult);

   // Draw the original model position.
   MpatDraw(M_DEFAULT, AilContextId, GraphicList, M_DRAW_BOX + M_DRAW_POSITION,
                                                          M_DEFAULT, M_ORIGINAL);

   // Pause to show the original image and model position.
   MosPrintf(AIL_TEXT("\nA %ldx%ld model was defined in the source image.\n"),
                          ROTATED_FIND_MODEL_WIDTH, ROTATED_FIND_MODEL_HEIGHT);
   MosPrintf(AIL_TEXT("It will be searched in images rotated from %d degree ")
             AIL_TEXT("to %d degree.\n"), -ROTATED_FIND_ROTATION_DELTA_ANGLE,
                                            ROTATED_FIND_ROTATION_DELTA_ANGLE);
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Dummy first call for bench measure purpose only (bench stabilization,
   // cache effect, etc...). This first call is NOT required by the application.
   MpatFind(AilContextId, AilSourceImage, AilResult);

   // If the model was found above the acceptance threshold.
   MpatGetResult(AilResult, M_DEFAULT, M_NUMBER+M_TYPE_AIL_INT, &NumResults);
   if(NumResults == 1L)
      {
      // Search for the model in images at different angles.
      RealAngle = ROTATED_FIND_ROTATION_DELTA_ANGLE;
      while(RealAngle >= -ROTATED_FIND_ROTATION_DELTA_ANGLE)
         {
         // Rotate the image from the model image to target image.
         MimRotate(AilSourceImage, AilTargetImage, RealAngle, M_DEFAULT,
                   M_DEFAULT, M_DEFAULT, M_DEFAULT, M_BILINEAR + M_OVERSCAN_CLEAR);

         // Reset the timer.
         MappTimer(M_DEFAULT, M_TIMER_RESET + M_SYNCHRONOUS, M_NULL);

         // Find the model in the target image.
         MpatFind(AilContextId, AilTargetImage, AilResult);
         
         // Read the time spent in MpatFind.
         MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &Time);

         // Clear the annotations.
         MgraClear(M_DEFAULT, GraphicList);

         // If one model was found above the acceptance threshold.
         MpatGetResult(AilResult, M_DEFAULT, M_NUMBER+M_TYPE_AIL_INT, &NumResults);
         if(NumResults == 1L)
            {
            // Read results and draw a box around the model occurrence.
            MpatGetResult(AilResult, M_DEFAULT, M_POSITION_X, &X);
            MpatGetResult(AilResult, M_DEFAULT, M_POSITION_Y, &Y);
            MpatGetResult(AilResult, M_DEFAULT, M_ANGLE, &Angle);
            MpatGetResult(AilResult, M_DEFAULT, M_SCORE, &Score);

            MgraControl(M_DEFAULT, M_COLOR, AnnotationColor);
            MpatDraw(M_DEFAULT, AilResult, GraphicList,
                     M_DRAW_BOX + M_DRAW_POSITION, M_DEFAULT, M_DEFAULT);

            MbufCopy(AilTargetImage, AilDisplayImage);

            // Calculate the angle error and the position errors for statistics.
            ErrAngle = CalculateAngleDist(Angle, RealAngle);

            RotateModelCenter(AilSourceImage, &RealX, &RealY, RealAngle);
            ErrX = fabs(X - RealX);
            ErrY = fabs(Y - RealY);

            SumErrAngle += ErrAngle;
            SumErrX += ErrX;
            SumErrY += ErrY;
            SumTime += Time;
            NbFound++;

            // Verify the precision for the position and the angle.
            if ((ErrX > ROTATED_FIND_MIN_POSITION_ACCURACY) ||
                (ErrY > ROTATED_FIND_MIN_POSITION_ACCURACY) ||
                (ErrAngle > ROTATED_FIND_MIN_ANGLE_ACCURACY))
               {
               MosPrintf(AIL_TEXT("Model accuracy error at angle %.1f !\n\n"), RealAngle);
               MosPrintf(AIL_TEXT("Errors are X:%.3f, Y:%.3f and Angle:%.2f\n\n"),
                                                           ErrX, ErrY, ErrAngle);
               MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
               MosGetch();
               }
            }
         else
            {
            MosPrintf(AIL_TEXT("Model was not found at angle %.1f !\n\n"), RealAngle);
            MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
            MosGetch();
            }

         RealAngle -= ROTATED_FIND_ROTATION_ANGLE_STEP;
         }

      // Print out the search result statistics
      // of the models found in rotated images.
      MosPrintf(AIL_TEXT("\nSearch statistics for the model ")
                AIL_TEXT("found in the rotated images.\n"));
      MosPrintf(AIL_TEXT("------------------------------")
                AIL_TEXT("------------------------------\n"));
      MosPrintf(AIL_TEXT("The average position error is \t\tX:%.3f, Y:%.3f\n"),
                                                      SumErrX / NbFound, SumErrY / NbFound);
      MosPrintf(AIL_TEXT("The average angle error is \t\t%.3f\n"), SumErrAngle / NbFound);
      MosPrintf(AIL_TEXT("The average search time is \t\t%.3f ms\n\n"),
                                                          SumTime*1000.0 / NbFound);
      }
   else
      {
      MosPrintf(AIL_TEXT("Model was not found!\n\n"));
      }

   // Wait for a key to be pressed.
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Free all allocations.
   MgraFree(GraphicList);
   MpatFree(AilResult);
   MpatFree(AilContextId);
   MbufFree(AilTargetImage);
   MbufFree(AilSourceImage);
   MbufFree(AilDisplayImage);
   }


// Calculate the rotated center of the model to compare the accuracy with
// the center of the occurrence found during pattern matching.
void RotateModelCenter(AIL_ID Buffer,
                       AIL_DOUBLE *X,
                       AIL_DOUBLE *Y,
                       AIL_DOUBLE Angle)
   {
   AIL_INT BufSizeX = MbufInquire(Buffer, M_SIZE_X, M_NULL);
   AIL_INT BufSizeY = MbufInquire(Buffer, M_SIZE_Y, M_NULL);
   AIL_DOUBLE RadAngle = Angle * ROTATED_FIND_RAD_PER_DEG;
   AIL_DOUBLE CosAngle = cos(RadAngle);
   AIL_DOUBLE SinAngle = sin(RadAngle);

   AIL_DOUBLE OffSetX = (BufSizeX - 1) / 2.0F;
   AIL_DOUBLE OffSetY = (BufSizeY - 1) / 2.0F;

   *X = (ROTATED_FIND_MODEL_X_CENTER - OffSetX)*CosAngle +
        (ROTATED_FIND_MODEL_Y_CENTER - OffSetY)*SinAngle + OffSetX;
   *Y = (ROTATED_FIND_MODEL_Y_CENTER - OffSetY)*CosAngle -
        (ROTATED_FIND_MODEL_X_CENTER - OffSetX)*SinAngle + OffSetY;
   }


// Calculate the absolute difference between the real angle
// and the angle found.
AIL_DOUBLE CalculateAngleDist(AIL_DOUBLE Angle1, AIL_DOUBLE Angle2)
   {
   AIL_DOUBLE dist = fabs(Angle1 - Angle2);

   while(dist >= 360.0)
      dist -= 360.0;

   if(dist > 180.0)
      dist = 360.0 - dist;

   return dist;
   }

//*****************************************************
// Find the rotated model in a rotated image example. *
//*****************************************************
void SearchModelAtAngleExample(AIL_ID AilSystem, AIL_ID AilDisplay)
{
   AIL_ID       AilSourceImage,                    // Model image buffer identifier.  
                AilTargetImage,                    // Target image buffer identifier. 
                ContextId,                         // Context identifier.             
                GraphicList,                       // Graphic list.                   
                AilResult;                         // Result identifier.              
   AIL_DOUBLE   Time        = 0.0;                 // Model search time.              
   AIL_INT      NbFound     = 0;                   // Number of models found.         
   AIL_DOUBLE   AnnotationColor = M_COLOR_GREEN;   // Drawing color.                  

   // Load the source image and display it.
   MbufRestore(ROTATED_FIND_IMAGE_FILE, AilSystem, &AilSourceImage);
   MdispSelect(AilDisplay, AilSourceImage);

   // Allocate the target image.
   MbufAlloc2d(AilSystem, MbufInquire(AilSourceImage, M_SIZE_X, M_NULL),
      MbufInquire(AilSourceImage, M_SIZE_Y, M_NULL), 
      8+M_UNSIGNED, M_IMAGE+M_PROC+M_DISP, &AilTargetImage);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &GraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

   // Allocate a normalized grayscale model.
   MpatAlloc(AilSystem, M_NORMALIZED, M_DEFAULT, &ContextId);

   // Define a regular model.
   MpatDefine(ContextId, M_REGULAR_MODEL, AilSourceImage,
              ROTATED_FIND_MODEL_X_POS, ROTATED_FIND_MODEL_Y_POS,
              ROTATED_FIND_MODEL_WIDTH, ROTATED_FIND_MODEL_HEIGHT, M_DEFAULT);

   // Activate the search model angle mode.
   MpatControl(ContextId, M_DEFAULT, M_SEARCH_ANGLE_MODE, M_ENABLE);

   // Disable the search model range angle.
   MpatControl(ContextId, M_DEFAULT, M_SEARCH_ANGLE_DELTA_NEG, 0);
   MpatControl(ContextId, M_DEFAULT, M_SEARCH_ANGLE_DELTA_POS, 0);

   // Set a specific angle.
   MpatControl(ContextId, 0, M_SEARCH_ANGLE, ROTATED_FIND_ROTATION_DELTA_ANGLE);

   // Preprocess the model.
   MpatPreprocess(ContextId, M_DEFAULT, AilSourceImage);

   // Allocate a result buffer.
   MpatAllocResult(AilSystem, M_DEFAULT, &AilResult);

   // Draw the original model position.
   MpatDraw(M_DEFAULT, ContextId, GraphicList, M_DRAW_BOX + M_DRAW_POSITION,
            M_DEFAULT, M_ORIGINAL);

   // Pause to show the original image and model position.
   MosPrintf(AIL_TEXT("\nA %ldx%ld model was defined in the source image.\n"),
             ROTATED_FIND_MODEL_WIDTH, ROTATED_FIND_MODEL_HEIGHT);
   MosPrintf(AIL_TEXT("It will be searched in an image rotated at %d degrees.\n"),
             -ROTATED_FIND_ROTATION_DELTA_ANGLE);
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Rotate the source image -10 degrees.
   MimRotate(AilSourceImage, AilTargetImage, ROTATED_FIND_ROTATION_DELTA_ANGLE, M_DEFAULT, 
             M_DEFAULT, M_DEFAULT, M_DEFAULT, M_BILINEAR + M_OVERSCAN_CLEAR);

   MdispSelect(AilDisplay, AilTargetImage);

   // Dummy first call for bench measure purpose only (bench stabilization, 
   // cache effect, etc...). This first call is NOT required by the application.
   MpatFind(ContextId, AilTargetImage, AilResult);

   // Reset the timer.
   MappTimer(M_DEFAULT, M_TIMER_RESET+M_SYNCHRONOUS, M_NULL);

   MpatFind(ContextId, AilTargetImage, AilResult);
 
   // Read the time spent in MpatFind().
   MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &Time);

   // Clear the annotations.
   MgraClear(M_DEFAULT, GraphicList);

   MpatGetResult(AilResult, M_DEFAULT, M_NUMBER+M_TYPE_AIL_INT, &NbFound);
   if (NbFound== 1L)
      {
      MgraControl(M_DEFAULT, M_COLOR, AnnotationColor);
      MpatDraw(M_DEFAULT, AilResult, GraphicList, M_DRAW_BOX+M_DRAW_POSITION, M_DEFAULT, M_DEFAULT);
      MosPrintf(AIL_TEXT("A search model at a specific angle has been found in the rotated image.\n"));
      MosPrintf(AIL_TEXT("The search time is %.3f ms.\n\n"), Time*1000.0);
      }
   else
      {
      MosPrintf(AIL_TEXT("Model was not found!\n\n"));
      }

   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Disable the overlay display.
   MdispControl(AilDisplay, M_OVERLAY_SHOW, M_DISABLE);

   // Free all allocations.
   MpatFree(AilResult);
   MpatFree(ContextId);
   MgraFree(GraphicList);
   MbufFree(AilTargetImage);
   MbufFree(AilSourceImage);
}

//****************************************************************************
// Automatic model allocation example.

// Source and target images file specifications.
#define AUTO_MODEL_IMAGE_FILE         M_IMAGE_PATH AIL_TEXT("Wafer.mim")
#define AUTO_MODEL_TARGET_IMAGE_FILE  M_IMAGE_PATH AIL_TEXT("WaferShifted.mim")

// Model width and height.
#define AUTO_MODEL_WIDTH   64L
#define AUTO_MODEL_HEIGHT  64L

void AutoAllocationModelExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID     AilImage,                        // Image buffer identifier.    
              AilSubImage,                     // Sub-image buffer identifier.
              GraphicList,                     // Graphic list.               
              ContextId,                       // Model identifier.           
              Result;                          // Result buffer identifier.   
   AIL_INT    AllocError;                      // Allocation error variable.  
   AIL_INT    NumResults;                      // Number of results found     
   AIL_INT    ImageWidth, ImageHeight;         // Target image dimensions     
   AIL_DOUBLE OrgX = 0.0, OrgY = 0.0;          // Original center of model.   
   AIL_DOUBLE x = 0.0, y = 0.0, Score = 0.0;   // Result variables.           
   AIL_DOUBLE AnnotationColor = M_COLOR_GREEN; // Drawing color.               

   // Load model image into an image buffer.
   MbufRestore(AUTO_MODEL_IMAGE_FILE, AilSystem, &AilImage);

   // Display the image.
   MdispSelect(AilDisplay, AilImage);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &GraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

   // Restrict the region to be processed to the bottom right corner of the image.
   MbufInquire(AilImage, M_SIZE_X, &ImageWidth);
   MbufInquire(AilImage, M_SIZE_Y, &ImageHeight);
   MbufChild2d(AilImage, ImageWidth/2, ImageHeight/2,
                         ImageWidth/2, ImageHeight/2, &AilSubImage);

   // Add an offset to the drawings so they are aligned with
   // the processed child image.
   MgraControl(M_DEFAULT, M_DRAW_OFFSET_X, (AIL_DOUBLE)-(ImageWidth/2));
   MgraControl(M_DEFAULT, M_DRAW_OFFSET_Y, (AIL_DOUBLE)-(ImageHeight/2));

   // Automatically allocate a normalized grayscale type pattern matching context.
   MpatAlloc(AilSystem, M_NORMALIZED, M_DEFAULT, &ContextId);

   // Define a unique model.
   MpatDefine(ContextId, M_AUTO_MODEL, AilSubImage, M_DEFAULT, M_DEFAULT,
              AUTO_MODEL_WIDTH, AUTO_MODEL_HEIGHT, M_DEFAULT);

   // Set the search accuracy to high.
   MpatControl(ContextId, M_DEFAULT, M_ACCURACY, M_HIGH);

   // Check for that model allocation was successful.
   MappGetError(M_DEFAULT, M_CURRENT, &AllocError);
   if(!AllocError)
      {
      // Draw a box around the model.
      MgraControl(M_DEFAULT, M_COLOR, AnnotationColor);
      MpatDraw(M_DEFAULT, ContextId, GraphicList, M_DRAW_BOX + M_DRAW_POSITION,
                                                                  M_DEFAULT, M_ORIGINAL);
      MosPrintf(AIL_TEXT("A model was automatically defined in the image.\n"));
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Clear the annotations.
      MgraClear(M_DEFAULT, GraphicList);

      // Load target image into an image buffer.
      MbufLoad(AUTO_MODEL_TARGET_IMAGE_FILE, AilImage);

      // Allocate result.
      MpatAllocResult(AilSystem, M_DEFAULT, &Result);

      // Preprocess the model.
      MpatPreprocess(ContextId, M_DEFAULT, AilSubImage);

      // Find model.
      MpatFind(ContextId, AilSubImage, Result);

      // If one model was found above the acceptance threshold set.
      MpatGetResult(Result, M_DEFAULT, M_NUMBER+M_TYPE_AIL_INT, &NumResults);
      if(NumResults == 1L)
         {
         // Get results.
         MpatGetResult(Result, M_DEFAULT, M_POSITION_X, &x);
         MpatGetResult(Result, M_DEFAULT, M_POSITION_Y, &y);
         MpatGetResult(Result, M_DEFAULT, M_SCORE, &Score);

         // Draw a box around the occurrence.
         MgraControl(M_DEFAULT, M_COLOR, AnnotationColor);
         MpatDraw(M_DEFAULT, Result, GraphicList, M_DRAW_BOX + M_DRAW_POSITION,
                                                                        M_DEFAULT, M_DEFAULT);

         // Analyze and print results.
         MpatInquire(ContextId, M_DEFAULT, M_ORIGINAL_X, &OrgX);
         MpatInquire(ContextId, M_DEFAULT, M_ORIGINAL_Y, &OrgY);
         MosPrintf(AIL_TEXT("An image misaligned by 50 pixels in X and 20 pixels ")
                                                   AIL_TEXT("in Y was loaded.\n\n"));
         MosPrintf(AIL_TEXT("The image is found to be shifted by %.2f in X, ")
                                    AIL_TEXT("and %.2f in Y.\n"), x - OrgX, y - OrgY);
         MosPrintf(AIL_TEXT("Model match score is %.1f percent.\n"), Score);
         MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
         MosGetch();
         }
      else
         {
         MosPrintf(AIL_TEXT("Error: Pattern not found properly.\n"));
         MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
         MosGetch();
         }

      // Free result buffer and model.
      MpatFree(Result);
      MpatFree(ContextId);
      }
   else
      {
      MosPrintf(AIL_TEXT("Error: Automatic model definition failed.\n"));
      MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
      MosGetch();
      }

   // Remove the drawing offset.
   MgraControl(M_DEFAULT, M_DRAW_OFFSET_X, 0.0);
   MgraControl(M_DEFAULT, M_DRAW_OFFSET_Y, 0.0);

   // Free the graphic list.
   MgraFree(GraphicList);

   // Free child buffer and defaults.
   MbufFree(AilSubImage);
   MbufFree(AilImage);
   }


```
