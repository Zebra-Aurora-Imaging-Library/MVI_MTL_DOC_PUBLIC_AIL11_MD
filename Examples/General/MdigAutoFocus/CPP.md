---
title: "MdigAutoFocus"
description: "This program performs an autofocus operation using the MdigFocus() function. Since the way to move a motorized camera lens is device-specific, we will not include real lens movement control and image grab but will simulate the lens focus with a smooth operation."
ms.language: "C++"
---

# MdigAutoFocus

> This program performs an autofocus operation using the MdigFocus() function. Since the way to move a motorized camera lens is device-specific, we will not include real lens movement control and image grab but will simulate the lens focus with a smooth operation.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufClear`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdigFocus`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraLine`, `MimConvolve`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdigAutoFocus.cpp
//
// Description: This program performs an autofocus operation using the 
//              MdigFocus() function. Since the way to move a motorized
//              camera lens is device-specific, we will not include real
//              lens movement control and image grab but will simulate 
//              the lens focus with a smooth operation. 
//
// Note:        Under AIL-Lite, the out of focus lens simulation is not supported.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Source image file specification.  
#define IMAGE_FILE                     M_IMAGE_PATH AIL_TEXT("BaboonMono.mim")

// Lens mechanical characteristics. 
#define FOCUS_MAX_NB_POSITIONS         100
#define FOCUS_MIN_POSITION             0
#define FOCUS_MAX_POSITION             (FOCUS_MAX_NB_POSITIONS - 1)
#define FOCUS_START_POSITION           10

// Autofocus search properties. 
#define FOCUS_MAX_POSITION_VARIATION   M_DEFAULT
#define FOCUS_MODE                     M_SMART_SCAN
#define FOCUS_SENSITIVITY              1

// User Data structure definition. 
typedef struct 
{  AIL_ID      SourceImage;
   AIL_ID      FocusImage;
   AIL_ID      Display;
   long        Iteration;
}  DigHookUserData;

// Autofocus callback function responsible for moving the lens. 
AIL_INT MFTYPE MoveLensHookFunction(AIL_INT HookType,
                                    AIL_INT Position,
                                    void*   UserDataHookPtr);

// Simulate a grab from a camera at different lens positions. 
void SimulateGrabFromCamera(AIL_ID SourceImage, 
                            AIL_ID FocusImage, 
                            AIL_INT Iteration, 
                            AIL_ID AnnotationDisplay);

// Draw position of the lens. 
void DrawCursor(AIL_ID AnnotationImage, AIL_INT Position);

//***************************************************************************
//  Main application function.                                               
int MosMain(void)
{
   AIL_ID  AilApplication,                  // Application identifier.      
           AilSystem,                       // System identifier.           
           AilDisplay,                      // Display identifier.          
           AilSource,                       // Source image.                 
           AilCameraFocus;                  // Focus simulated image.        
   AIL_INT FocusPos;                        // Best focus position.          
   DigHookUserData UserData;                // User data passed to the hook. 

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Load the source image. 
   MbufRestore(IMAGE_FILE, AilSystem, &AilSource);
   MbufRestore(IMAGE_FILE, AilSystem, &AilCameraFocus);
   MbufClear(AilCameraFocus, 0);

   // Select image on the display.  
   MdispSelect(AilDisplay, AilCameraFocus);

   // Simulate the first image grab. 
   SimulateGrabFromCamera(AilSource, AilCameraFocus, FOCUS_START_POSITION, AilDisplay);

   // Initialize user data needed within the hook function. 
   UserData.SourceImage = AilSource;
   UserData.FocusImage  = AilCameraFocus;
   UserData.Iteration   = 0L;
   UserData.Display     = AilDisplay;

   // Pause to show the original image. 
   MosPrintf(AIL_TEXT("\nAUTOFOCUS:\n"));
   MosPrintf(AIL_TEXT("----------\n\n"));
   MosPrintf(AIL_TEXT("Automatic focusing operation will be done on this image.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));  
   MosGetch();
   MosPrintf(AIL_TEXT("Autofocusing...\n\n"));  
  
   // Perform Autofocus. 
   // Since lens movement is hardware specific, no digitizer is used here.
   // We simulate the lens movement with by smoothing the image data in 
   // the hook function instead.
   MdigFocus(M_NULL,
             AilCameraFocus,
             M_DEFAULT,
             MoveLensHookFunction,
             &UserData,
             FOCUS_MIN_POSITION,
             FOCUS_START_POSITION,
             FOCUS_MAX_POSITION,
             FOCUS_MAX_POSITION_VARIATION,
             FOCUS_MODE + FOCUS_SENSITIVITY,
             &FocusPos); 

   // Print the best focus position and number of iterations. 
   MosPrintf(AIL_TEXT("The best focus position is %d.\n"), (int)FocusPos);
   MosPrintf(AIL_TEXT("The best focus position found in %d iterations.\n\n"), 
                                                         (int)UserData.Iteration);
   MosPrintf(AIL_TEXT("Press any key to end.\n"));  
   MosGetch();

  // Free all allocations. 
   MbufFree(AilSource);
   MbufFree(AilCameraFocus);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}

//******************************************************************************
// Autofocus hook function responsible to move the lens.                        

AIL_INT MFTYPE MoveLensHookFunction(AIL_INT HookType,
                                    AIL_INT Position,
                                    void*   UserDataHookPtr)
   {
   DigHookUserData *UserData = (DigHookUserData *)UserDataHookPtr;

   // Here, the lens position must be changed according to the Position parameter.
   // In that case, we simulate the lens position change followed by a grab.
   if(HookType == M_CHANGE || HookType == M_ON_FOCUS)
      {
      SimulateGrabFromCamera( UserData->SourceImage, 
                              UserData->FocusImage, 
                              Position,
                              UserData->Display 
                              );
      UserData->Iteration++;
      }

   return 0;
   }

//*********************************************************************************
// Utility function to simulate a grab from a camera at different lens position    
// by smoothing the original image. It should be replaced with a true camera grab. 
//                                                                                 
// Note that this lens simulation will not work under AIL-lite because it uses     
// MimConvolve().                                                                  

// Lens simulation characteristics. 
#define FOCUS_BEST_POSITION            (FOCUS_MAX_NB_POSITIONS/2)

void SimulateGrabFromCamera(AIL_ID SourceImage, AIL_ID FocusImage, 
                            AIL_INT Iteration, AIL_ID AnnotationDisplay)
{
   AIL_INT NbSmoothNeeded;   // Number of smooths needed          
   AIL_INT BufType;          // Buffer type                       
   AIL_INT BufSizeX;         // Buffer size X                     
   AIL_INT BufSizeY;         // Buffer size Y                     
   AIL_INT Smooth;           // Smooth index                      
   AIL_ID TempBuffer;        // Temporary buffer                  
   AIL_ID SourceOwnerSystem; // Owner system of the source buffer 

   // Throw an error under AIL-lite since lens simulation cannot be used. 
   #if (M_AIL_LITE)
      #error "Replace the SimulateGrabFromCamera()function with a true image grab."
   #endif

   // Compute number of smooths needed to simulate focus. 
   NbSmoothNeeded = MosAbs(Iteration - FOCUS_BEST_POSITION);

   // Buffer inquires. 
   BufType        = MbufInquire(FocusImage, M_TYPE,   M_NULL);
   BufSizeX       = MbufInquire(FocusImage, M_SIZE_X, M_NULL);
   BufSizeY       = MbufInquire(FocusImage, M_SIZE_Y, M_NULL);

   if(NbSmoothNeeded == 0)
      {
      // Directly copy image source to destination. 
      MbufCopy(SourceImage, FocusImage);
      }
   else if(NbSmoothNeeded == 1)
      {
      // Directly convolve image from source to destination. 
      MimConvolve(SourceImage, FocusImage, M_SMOOTH);
      }
   else 
      {
      SourceOwnerSystem = (AIL_ID)MbufInquire(SourceImage, M_OWNER_SYSTEM, M_NULL);

      // Allocate temporary buffer.    
      MbufAlloc2d(SourceOwnerSystem, BufSizeX, BufSizeY, 
                  BufType, M_IMAGE+M_PROC, &TempBuffer);
   
      // Perform first smooth. 
      MimConvolve(SourceImage, TempBuffer, M_SMOOTH);
         
      // Perform smooths. 
      for(Smooth=1;Smooth<NbSmoothNeeded-1;Smooth++)
         {
         MimConvolve(TempBuffer, TempBuffer, M_SMOOTH);
         }

      // Perform last smooth. 
      MimConvolve(TempBuffer, FocusImage, M_SMOOTH);

      // Free temporary buffer. 
      MbufFree(TempBuffer);
      }

   // Draw position cursor. 
   DrawCursor(AnnotationDisplay, Iteration);
}

//**************************************************************
// Draw position of the focus lens.                             

// Cursor specifications. 
#define CURSOR_POSITION                ((BufSizeY*7)/8)
#define CURSOR_SIZE                    14
#define CURSOR_COLOR                   M_COLOR_GREEN

void DrawCursor(AIL_ID AnnotationDisplay, AIL_INT Position)
{
   AIL_ID AnnotationImage;
   AIL_INT BufSizeX, BufSizeY, n;
   AIL_DOUBLE CursorColor;

   // Prepare for overlay annotations. 
   MdispControl(AnnotationDisplay, M_OVERLAY, M_ENABLE);
   MdispControl(AnnotationDisplay, M_OVERLAY_CLEAR, M_DEFAULT);
   MdispInquire(AnnotationDisplay, M_OVERLAY_ID, &AnnotationImage);
   MbufInquire(AnnotationImage, M_SIZE_X, &BufSizeX);
   MbufInquire(AnnotationImage, M_SIZE_Y, &BufSizeY);
   CursorColor = CURSOR_COLOR;
   MgraControl(M_DEFAULT, M_COLOR, CursorColor);

   // Write annotations. 
   n = (BufSizeX/FOCUS_MAX_NB_POSITIONS);
   MgraLine(M_DEFAULT, AnnotationImage, 0, CURSOR_POSITION+CURSOR_SIZE, 
                                        BufSizeX-1, CURSOR_POSITION+CURSOR_SIZE);
   MgraLine(M_DEFAULT, AnnotationImage, Position*n, CURSOR_POSITION+CURSOR_SIZE,
                                        Position*n-CURSOR_SIZE, CURSOR_POSITION);
   MgraLine(M_DEFAULT, AnnotationImage, Position*n, CURSOR_POSITION+CURSOR_SIZE, 
                                        Position*n+CURSOR_SIZE, CURSOR_POSITION);
   MgraLine(M_DEFAULT, AnnotationImage, Position*n-CURSOR_SIZE, CURSOR_POSITION, 
                                        Position*n+CURSOR_SIZE, CURSOR_POSITION);
}

```
