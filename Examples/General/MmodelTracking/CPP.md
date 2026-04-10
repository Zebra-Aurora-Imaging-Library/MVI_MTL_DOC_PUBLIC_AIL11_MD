---
title: "MmodelTracking"
description: "This program shows how to track a unique object using pattern recognition. It allocates a model in the field of view of the camera and finds it in a loop. It also prints the coordinates of the found model and draws a box around it. It searches using 2 methods, the normalized grayscale correlation (Mpat), which is very fast and with the Geometric Model Finder (Mmod), which is independent of the model rotation and scale but slower. Note: Display update and annotations drawing can require significant CPU usage."
ms.language: "C++"
---

# MmodelTracking

> This program shows how to track a unique object using pattern recognition. It allocates a model in the field of view of the camera and finds it in a loop. It also prints the coordinates of the found model and draws a box around it. It searches using 2 methods, the normalized grayscale correlation (Mpat), which is very fast and with the Geometric Model Finder (Mmod), which is independent of the model rotation and scale but slower. Note: Display update and annotations drawing can require significant CPU usage.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufAlloc2d`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MdigAlloc`, `MdigControl`, `MdigFree`, `MdigGrab`, `MdigGrabContinuous`, `MdigGrabWait`, `MdigHalt`, `MdigInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraRect`, `MmodAlloc`, `MmodAllocResult`, `MmodControl`, `MmodDefine`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodInquire`, `MmodPreprocess`, `MpatAlloc`, `MpatAllocResult`, `MpatControl`, `MpatDefine`, `MpatDraw`, `MpatFind`, `MpatFree`, `MpatGetResult`, `MpatInquire`, `MpatPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Finding/Locating, Modules, Buffer, Display, Graphics, Digitizer, Pattern Matching, Geometric Model Finder, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MmodelTracking.cpp
//
// Description: This program shows how to track a unique object using 
//              pattern recognition. It allocates a model in the field of 
//              view of the camera and finds it in a loop. It also prints 
//              the coordinates of the found model and draws a box around it.
//              It searches using 2 methods, the normalized grayscale 
//              correlation (AIL.Mpat), which is very fast and with the Geometric 
//              Model Finder (AIL.Mmod), which is independent of the model rotation 
//              and scale but slower.
//
// Note:        Display update and annotations drawing can require
//              significant CPU usage.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Model specification.
#define MODEL_WIDTH                   128L
#define MODEL_HEIGHT                  128L
#define MODEL_POS_X_INIT(TargetImage) (MbufInquire(TargetImage, M_SIZE_X, M_NULL)/2)
#define MODEL_POS_Y_INIT(TargetImage) (MbufInquire(TargetImage, M_SIZE_Y, M_NULL)/2)

// Minimum score to consider the object found (in percent).
#define MODEL_MIN_MATCH_SCORE       50.0

// Drawing color
#define DRAW_COLOR                  0xFF // White

// Example selection.
#define RUN_PAT_TRACKING_EXAMPLE    M_YES
#define RUN_MOD_TRACKING_EXAMPLE    M_YES 

// Example functions declarations.
void MpatTrackingExample(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilDigitizer,
                         AIL_ID AilDisplayImage, AIL_ID AilModelImage);
void MmodTrackingExample(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilDigitizer,
                         AIL_ID AilDisplayImage, AIL_ID AilModelImage);
void GetModelImage(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilDigitizer,
                   AIL_ID AilDisplayImage, AIL_ID AilModelImage);


// *****************************************************************************
// Main.
// *****************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication,   // Application identifier.   
          AilSystem,        // System identifier.        
          AilDisplay,       // Display identifier.       
          AilDigitizer,     // Digitizer identifier.     
          AilDisplayImage,  // Display image identifier. 
          AilModelImage;    // Model image identifier.   

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay,
                    &AilDigitizer, &AilDisplayImage);

   // Allocate a model image buffer.
   MbufAlloc2d(AilSystem,
               MbufInquire(AilDisplayImage, M_SIZE_X, M_NULL),
               MbufInquire(AilDisplayImage, M_SIZE_Y, M_NULL),
               8, M_IMAGE+M_PROC, &AilModelImage);

   MosPrintf(AIL_TEXT("\nMODEL TRACKING:\n"));
   MosPrintf(AIL_TEXT("---------------\n\n"));

   // Get the model image.
   GetModelImage(AilSystem, AilDisplay, AilDigitizer, AilDisplayImage, AilModelImage);

#if (RUN_PAT_TRACKING_EXAMPLE)
   // Finds the model using pattern matching.
   MpatTrackingExample(AilSystem, AilDisplay, AilDigitizer, AilDisplayImage, AilModelImage);
#endif

#if (RUN_MOD_TRACKING_EXAMPLE)
   // Finds the model using geometric model finder.
   MmodTrackingExample(AilSystem, AilDisplay, AilDigitizer, AilDisplayImage, AilModelImage);
#endif

   // Free allocated buffers.

   MbufFree(AilModelImage);

   // Free defaults.
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, AilDisplayImage);

   return 0;
   }


// *****************************************************************************
// Get Model Image Function.
// *****************************************************************************

void GetModelImage(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilDigitizer,
                   AIL_ID AilDisplayImage, AIL_ID AilModelImage)
   {
   AIL_ID     AilOverlayImage;         // Overlay image.
   AIL_DOUBLE DrawColor = DRAW_COLOR;  // Drawing color.

   // Prepare for overlay annotations.
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);

   // Draw the position of the model to define in the overlay.
   MgraControl(M_DEFAULT, M_COLOR, DrawColor);
   MgraRect(M_DEFAULT, AilOverlayImage,
            MODEL_POS_X_INIT(AilOverlayImage) - (MODEL_WIDTH/2),
            MODEL_POS_Y_INIT(AilOverlayImage) - (MODEL_HEIGHT/2),
            MODEL_POS_X_INIT(AilOverlayImage) + (MODEL_WIDTH/2),
            MODEL_POS_Y_INIT(AilOverlayImage) + (MODEL_HEIGHT/2));

   // Grab continuously.
   MosPrintf(AIL_TEXT("Model definition:\n\n"));
   MosPrintf(AIL_TEXT("Place a unique model to find in the marked rectangle.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));

   // Grab a reference model image.
   MdigGrabContinuous(AilDigitizer, AilDisplayImage);
   MosGetch();
   MdigHalt(AilDigitizer);

   // Copy the grabbed image to the Model image to keep it.
   MbufCopy(AilDisplayImage, AilModelImage);

   // Clear and disable the overlay.
   MdispControl(AilDisplay, M_OVERLAY, M_DISABLE);
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);
   }


// *****************************************************************************
// Tracking object with pattern matching module.
// *****************************************************************************

void MpatTrackingExample(AIL_ID AilSystem, AIL_ID AilDisplay,
                         AIL_ID AilDigitizer, AIL_ID AilDisplayImage, AIL_ID AilModelImage)
   {
   AIL_ID      AilImage[2],             // Processing image buffer identifiers.
               ContextId,               // Model identifier.                   
               Result;                  // Result identifier.                  
   AIL_DOUBLE  DrawColor = DRAW_COLOR;  // Model drawing color.                
   AIL_INT     Found;                   // Number of found models.             
   AIL_INT     NbFindDone = 0;          // Number of loops to find model done. 
   AIL_DOUBLE  OrgX = 0.0, OrgY = 0.0;  // Original center of model.           
   AIL_DOUBLE  x, y, Score = 0.0;       // Result variables.                   
   AIL_DOUBLE  Time = 0.0;              // Timer.                              

   // Print a start message.
   MosPrintf(AIL_TEXT("\nGRAYSCALE PATTERN MATCHING:\n"));
   MosPrintf(AIL_TEXT("---------------------------\n\n"));

   // Display the model image.
   MbufCopy(AilModelImage, AilDisplayImage);

   // Allocate normalized grayscale type model.
   MpatAlloc(AilSystem, M_NORMALIZED, M_DEFAULT, &ContextId);
   MpatDefine(ContextId, M_REGULAR_MODEL, AilModelImage,
              MODEL_POS_X_INIT(AilModelImage)-(MODEL_WIDTH/2),
              MODEL_POS_Y_INIT(AilModelImage)-(MODEL_HEIGHT/2),
              MODEL_WIDTH, MODEL_HEIGHT, M_DEFAULT);


   // Allocate result.
   MpatAllocResult(AilSystem, M_DEFAULT, &Result);

   // Draw box around the model.
   MgraControl(M_DEFAULT, M_COLOR, DrawColor);
   MpatDraw(M_DEFAULT, ContextId, AilDisplayImage, M_DRAW_BOX, M_DEFAULT, M_ORIGINAL);

   // Set minimum acceptance for search.
   MpatControl(ContextId, 0, M_ACCEPTANCE, MODEL_MIN_MATCH_SCORE);

   // Set speed.
   MpatControl(ContextId, 0, M_SPEED, M_HIGH);

   // Set accuracy.
   MpatControl(ContextId, 0, M_ACCURACY, M_LOW);

   // Preprocess model.
   MpatPreprocess(ContextId, M_DEFAULT, AilModelImage);

   // Inquire about center of model.
   MpatInquire(ContextId, 0, M_ORIGINAL_X, &OrgX);
   MpatInquire(ContextId, 0, M_ORIGINAL_Y, &OrgY);

   // Print the original position.
   MosPrintf(AIL_TEXT("A Grayscale Model was defined.\n"));
   MosPrintf(AIL_TEXT("Model dimensions:  %ld x %ld.\n"), MODEL_WIDTH, MODEL_HEIGHT);
   MosPrintf(AIL_TEXT("Model center:      X=%.2f, Y=%.2f.\n"), OrgX, OrgY);
   MosPrintf(AIL_TEXT("Model is scale and rotation dependant.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate 2 grab buffers.
   MbufAlloc2d(AilSystem,
               MbufInquire(AilModelImage, M_SIZE_X, M_NULL),
               MbufInquire(AilModelImage, M_SIZE_Y, M_NULL),
               8,
               M_IMAGE+M_GRAB+M_PROC,
               &AilImage[0]);
   MbufAlloc2d(AilSystem,
               MbufInquire(AilModelImage, M_SIZE_X, M_NULL),
               MbufInquire(AilModelImage, M_SIZE_Y, M_NULL),
               8,
               M_IMAGE+M_GRAB+M_PROC,
               &AilImage[1]);

   // Grab continuously and perform the find operation using double buffering.
   MosPrintf(AIL_TEXT("\nContinuously finding the Grayscale model.\n"));
   MosPrintf(AIL_TEXT("Press any key to stop.\n\n"));

   // Grab a first target image into first buffer (done twice for timer reset accuracy).
   MdigControl(AilDigitizer, M_GRAB_MODE, M_ASYNCHRONOUS);
   MdigGrab(AilDigitizer, AilImage[0]);
   MdigGrab(AilDigitizer, AilImage[1]);
   MappTimer(M_DEFAULT, M_TIMER_RESET, &Time);

   // Loop, processing one buffer while grabbing the other.
   do
      {
      // Grab a target image into the other buffer.
      MdigGrab(AilDigitizer, AilImage[NbFindDone%2]);

      // Read the time.
      MappTimer(M_DEFAULT, M_TIMER_READ, &Time);

      // Find model.
      MpatFind(ContextId, AilImage[(NbFindDone+1)%2], Result);

      // Get results.
      MpatGetResult(Result, M_GENERAL, M_NUMBER+M_TYPE_AIL_INT, &Found);
      MpatGetResult(Result, M_DEFAULT, M_POSITION_X, &x);
      MpatGetResult(Result, M_DEFAULT, M_POSITION_Y, &y);
      MpatGetResult(Result, M_DEFAULT, M_SCORE, &Score);

      // Print a message based upon the score.
      if(Found)
         {
         // Draw a box around the model and print the results.
         MpatDraw(M_DEFAULT, Result, AilImage[(NbFindDone+1)%2],
                  M_DRAW_BOX+M_DRAW_POSITION, M_DEFAULT, M_DEFAULT);
         MosPrintf(AIL_TEXT("Found: X=%7.2f, Y=%7.2f, Score=%5.1f%% (%.1f fps).    \r"),
                   x, y, Score, (NbFindDone+1)/Time);
         }
      else
         {
         // Print the "not found" message.
         MosPrintf(AIL_TEXT("Not found ! (score<%5.1f%%)                (%.1f fps).     \r"),
                   MODEL_MIN_MATCH_SCORE, (NbFindDone+1)/Time);
         }

      // Copy target image to the display.
      MbufCopy(AilImage[(NbFindDone+1)%2], AilDisplayImage);

      // Increment find count
      NbFindDone++;
      } 
   while (!MosKbhit());

   MosGetch();
   MosPrintf(AIL_TEXT("\n\n"));

   // Wait for end of last grab.
   MdigGrabWait(AilDigitizer, M_GRAB_END);

   // Free all allocated objects.
   MpatFree(Result);
   MpatFree(ContextId);
   MbufFree(AilImage[1]);
   MbufFree(AilImage[0]);
   }


//*****************************************************************************
//Tracking object with Geometric Model Finder module
//*****************************************************************************

#define MODEL_MAX_OCCURRENCES       16L

void MmodTrackingExample(AIL_ID AilSystem,    AIL_ID AilDisplay,
                         AIL_ID AilDigitizer, AIL_ID AilDisplayImage, AIL_ID AilModelImage)
   {
   AIL_ID     AilImage[2],                  // Processing image buffer identifiers.
              SearchContext,                // Search context identifier.          
              Result;                       // Result identifier.                  
   AIL_DOUBLE DrawColor = DRAW_COLOR;       // Model drawing color.                
   AIL_INT    Found;                        // Number of models found.             
   AIL_INT    NbFindDone = 0;               // Number of loops to find model done. 
   AIL_DOUBLE OrgX = 0.0, OrgY = 0.0;       // Original center of model.           
   AIL_DOUBLE Score[MODEL_MAX_OCCURRENCES], // Model correlation score.            
              x[MODEL_MAX_OCCURRENCES],     // Model X position.                   
              y[MODEL_MAX_OCCURRENCES],     // Model Y position.                   
              Angle[MODEL_MAX_OCCURRENCES], // Model occurrence angle.             
              Scale[MODEL_MAX_OCCURRENCES]; // Model occurrence scale.             
   AIL_DOUBLE Time = 0.0;                   // Timer.                              

   // Print a start message.
   MosPrintf(AIL_TEXT("\nGEOMETRIC MODEL FINDER (scale and rotation independent):\n"));
   MosPrintf(AIL_TEXT("--------------------------------------------------------\n\n"));

   // Display model image.
   MbufCopy(AilModelImage, AilDisplayImage);

   // Allocate a context and define a geometric model.
   MmodAlloc(AilSystem, M_GEOMETRIC, M_DEFAULT, &SearchContext);
   MmodDefine(SearchContext, M_IMAGE, AilModelImage,
              (AIL_DOUBLE)MODEL_POS_X_INIT(AilModelImage) - (MODEL_WIDTH/2),
              (AIL_DOUBLE)MODEL_POS_Y_INIT(AilModelImage) - (MODEL_HEIGHT/2),
              MODEL_WIDTH, MODEL_HEIGHT);

   // Allocate result.
   MmodAllocResult(AilSystem, M_DEFAULT, &Result);

   // Draw a box around the model.
   MgraControl(M_DEFAULT, M_COLOR, DrawColor);
   MmodDraw(M_DEFAULT, SearchContext, AilDisplayImage, M_DRAW_BOX, M_DEFAULT, M_ORIGINAL);

   // Set speed to VERY HIGH for fast but less precise search.
   MmodControl(SearchContext, M_CONTEXT, M_SPEED, M_VERY_HIGH);

   // Set minimum acceptance for the search.
   MmodControl(SearchContext, M_DEFAULT, M_ACCEPTANCE, MODEL_MIN_MATCH_SCORE);

   // Preprocess model.
   MmodPreprocess(SearchContext, M_DEFAULT);

   // Inquire about center of model.
   MmodInquire(SearchContext, M_DEFAULT, M_ORIGINAL_X, &OrgX);
   MmodInquire(SearchContext, M_DEFAULT, M_ORIGINAL_Y, &OrgY);

   // Print the original position.
   MosPrintf(AIL_TEXT("The Geometric target model was defined.\n"));
   MosPrintf(AIL_TEXT("Model dimensions: %ld x %ld.\n"), MODEL_WIDTH, MODEL_HEIGHT);
   MosPrintf(AIL_TEXT("Model center:     X=%.2f, Y=%.2f.\n"), OrgX, OrgY);
   MosPrintf(AIL_TEXT("Model is scale and rotation independent.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate 2 grab buffers.
   MbufAlloc2d(AilSystem,
               MbufInquire(AilModelImage, M_SIZE_X, M_NULL),
               MbufInquire(AilModelImage, M_SIZE_Y, M_NULL),
               8,
               M_IMAGE+M_GRAB+M_PROC,
               &AilImage[0]);
   MbufAlloc2d(AilSystem,
               MbufInquire(AilModelImage, M_SIZE_X, M_NULL),
               MbufInquire(AilModelImage, M_SIZE_Y, M_NULL),
               8,
               M_IMAGE+M_GRAB+M_PROC,
               &AilImage[1]);

   // Grab continuously grab and perform the find operation using double buffering.
   MosPrintf(AIL_TEXT("\nContinuously finding the Geometric Model.\n"));
   MosPrintf(AIL_TEXT("Press any key to stop.\n\n"));

   // Grab a first target image into first buffer (done twice for timer reset accuracy).
   MdigControl(AilDigitizer, M_GRAB_MODE, M_ASYNCHRONOUS);
   MdigGrab(AilDigitizer, AilImage[0]);
   MdigGrab(AilDigitizer, AilImage[1]);
   MappTimer(M_DEFAULT, M_TIMER_RESET, &Time);

   // Loop, processing one buffer while grabbing the other.
   do
      {
      // Grab a target image into the other buffer.
      MdigGrab(AilDigitizer, AilImage[NbFindDone%2]);

      // Read the time.
      MappTimer(M_DEFAULT, M_TIMER_READ, &Time);

      // Find model.
      MmodFind(SearchContext, AilImage[(NbFindDone+1)%2], Result);

      // Get the number of occurrences found.
      MmodGetResult(Result, M_DEFAULT, M_NUMBER+M_TYPE_AIL_INT, &Found);

      // Print a message based on the score.
      if ( (Found >= 1) && (Found < MODEL_MAX_OCCURRENCES) )
         {
         // Get results.
         MmodGetResult(Result, M_DEFAULT, M_POSITION_X, x);
         MmodGetResult(Result, M_DEFAULT, M_POSITION_Y, y);
         MmodGetResult(Result, M_DEFAULT, M_SCALE, Scale);
         MmodGetResult(Result, M_DEFAULT, M_ANGLE, Angle);
         MmodGetResult(Result, M_DEFAULT, M_SCORE, Score);

         // Draw a box and a cross where the model was found and print the results.
         MmodDraw(M_DEFAULT, Result, AilImage[(NbFindDone+1)%2],
                  M_DRAW_BOX+M_DRAW_POSITION+M_DRAW_EDGES, M_DEFAULT, M_DEFAULT);
         MosPrintf(AIL_TEXT("Found: X=%6.1f, Y=%6.1f, Angle=%6.1f, Scale=%5.2f, ")
                   AIL_TEXT("Score=%5.1f%% (%5.1f fps).\r"),
                   x[0], y[0], Angle[0], Scale[0], Score[0], (NbFindDone+1)/Time);
         }
      else
         {
         // Print the "not found" message.
         MosPrintf(AIL_TEXT("Not found! (score<%5.1f%%)                          ")
                   AIL_TEXT("(%5.1f fps).\r"), MODEL_MIN_MATCH_SCORE, (NbFindDone+1)/Time);
         }

      // Copy target image to the display.
      MbufCopy(AilImage[(NbFindDone+1)%2], AilDisplayImage);

      // Increment the counter.
      NbFindDone++;
      } 
   while (!MosKbhit());

   MosGetch();
   MosPrintf(AIL_TEXT("\n\n"));

   // Wait for the end of last grab.
   MdigGrabWait(AilDigitizer, M_GRAB_END);

   // Free all allocations.
   MmodFree(Result);
   MmodFree(SearchContext);
   MbufFree(AilImage[1]);
   MbufFree(AilImage[0]);
   }

```
