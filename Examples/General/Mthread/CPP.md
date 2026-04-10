---
title: "Mthread"
description: "This program shows how to use threads in an AIL application and synchronize them with events. It creates 4 processing threads that are used to work in 4 different regions of a display buffer."
ms.language: "C++"
---

# Mthread

> This program shows how to use threads in an AIL application and synchronize them with events. It creates 4 processing threads that are used to work in 4 different regions of a display buffer.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MappInquire`, `MappTimer`, `MbufAlloc2d`, `MbufClear`, `MbufCopy`, `MbufCopyColor2d`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MdispInquire`, `MgraArcFill`, `MgraRectFill`, `MgraText`, `MimArith`, `MimConvolve`, `MimRotate`, `MsysAlloc`, `MsysFree`, `MsysInquire`, `MthrAlloc`, `MthrControl`, `MthrFree`, `MthrWait`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MThread.cpp 
//
// Description: This program shows how to use threads in an application and
//              synchronize them with event. It creates 4 processing threads that
//              are used to work in 4 different regions of a display buffer.
//      
//              Thread usage:
//              - The main thread starts a processing thread in each of the 4 different
//                quarters of a display buffer. The main thread then waits for a key to
//                be pressed to stop them.
//              - The top-left and bottom-left threads work in a loop, as follows: the
//                top-left thread adds a constant to its buffer, then sends an event to
//                the bottom-left thread. The bottom-left thread waits for the event
//                from the top-left thread, rotates the top-left buffer image, then sends an
//                event to the top-left thread to start a new loop.
//              - The top-right and bottom-right threads work the same way as the
//                top-left and bottom-left threads, except that the bottom-right thread
//                performs an edge detection operation, rather than a rotation.
//
// Note:        - Under AIL-Lite, the threads will do graphic annotations instead.
//              - Comment out the MdispSelect() if you wish to avoid benchmarking
//                the display update overhead on CPU usage and processing rate.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////
 
#include <AIL.h>

// Local defines. 
#define IMAGE_FILE         M_IMAGE_PATH AIL_TEXT("Bird.mim")
#define IMAGE_WIDTH        256L
#define IMAGE_HEIGHT       240L
#define STRING_LENGTH_MAX  40
#define STRING_POS_X       10
#define STRING_POS_Y       220
#define DRAW_RADIUS_NUMBER 5
#define DRAW_RADIUS_STEP   10
#define DRAW_CENTER_POSX   196
#define DRAW_CENTER_POSY   180

// Function prototypes. 
AIL_UINT32 MFTYPE TopThread(void *TPar);
AIL_UINT32 MFTYPE BotLeftThread(void *TPar);
AIL_UINT32 MFTYPE BotRightThread(void *TPar);

// Thread parameters structure. 
typedef struct ThreadParam
   {
   AIL_ID Id;
   AIL_ID System;
   AIL_ID OrgImage;
   AIL_ID SrcImage;
   AIL_ID DstImage;
   AIL_ID DispImage;
   AIL_INT DispOffsetX;
   AIL_INT DispOffsetY;
   AIL_ID ReadyEvent;
   AIL_ID DoneEvent;
   AIL_INT NumberOfIteration;
   AIL_INT Radius;
   AIL_INT Exit;
   AIL_INT LicenseModules;
   struct ThreadParam *LinkThreadParam;
   } THREAD_PARAM;


// Main function: 
// -------------- 
int MosMain(void)
   { 
   AIL_ID AilApplication,        // Application identifier.                  
          AilRemoteApplication,  // Remote application identifier.           
          AilSystem,             // System identifier.                       
          AilDisplay,            // Display identifier.                      
          AilImage, AilOrgImage; // Image buffer identifiers.                
   THREAD_PARAM TParTopLeft,     // Parameters passed to top-left thread.    
                TParBotLeft,     // Parameters passed to bottom-left thread. 
                TParTopRight,    // Parameters passed to top-right thread.   
                TParBotRight;    // Parameters passed to bottom-right thread.
   AIL_DOUBLE Time, FramesPerSecond; // Timer variables.                     
   AIL_INT LicenseModules;       // List of available modules.             

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Allocate and display the main image buffer. 
   MbufAlloc2d(AilSystem, IMAGE_WIDTH*2, IMAGE_HEIGHT*2, 8+M_UNSIGNED,
                                         M_IMAGE+M_PROC+M_DISP, &AilImage);
   MbufClear(AilImage, 0);
   MdispSelect(AilDisplay,AilImage);
   MdispInquire(AilDisplay, M_SELECTED, &TParTopLeft.DispImage);

   // Allocate an image buffer to keep the original. 
   MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8+M_UNSIGNED,
                                       M_IMAGE+M_PROC, &AilOrgImage);

   // Allocate a processing buffer for each thread. 
   MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8+M_UNSIGNED, M_IMAGE+M_PROC, &TParTopLeft.SrcImage);
   MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8+M_UNSIGNED, M_IMAGE+M_PROC, &TParBotLeft.DstImage);
   MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8+M_UNSIGNED, M_IMAGE+M_PROC, &TParTopRight.SrcImage);
   MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8+M_UNSIGNED, M_IMAGE+M_PROC, &TParBotRight.DstImage);
 
   // Allocate synchronization events. 
   MthrAlloc(AilSystem, M_EVENT, M_DEFAULT, M_NULL, M_NULL, &TParTopLeft.DoneEvent);
   MthrAlloc(AilSystem, M_EVENT, M_DEFAULT, M_NULL, M_NULL, &TParBotLeft.DoneEvent);
   MthrAlloc(AilSystem, M_EVENT, M_DEFAULT, M_NULL, M_NULL, &TParTopRight.DoneEvent);
   MthrAlloc(AilSystem, M_EVENT, M_DEFAULT, M_NULL, M_NULL, &TParBotRight.DoneEvent);

   // Inquire licenses. 
   MsysInquire(AilSystem, M_OWNER_APPLICATION, &AilRemoteApplication);
   MappInquire(AilRemoteApplication, M_LICENSE_MODULES, &LicenseModules);

   // Initialize remaining thread parameters. 
   TParTopLeft.System               = AilSystem;
   TParTopLeft.OrgImage             = AilOrgImage;
   TParTopLeft.DstImage             = TParTopLeft.SrcImage;
   TParTopLeft.DispOffsetX          = 0;
   TParTopLeft.DispOffsetY          = 0;
   TParTopLeft.ReadyEvent           = TParBotLeft.DoneEvent;
   TParTopLeft.NumberOfIteration    = 0;
   TParTopLeft.Radius               = 0;
   TParTopLeft.Exit                 = 0;
   TParTopLeft.LicenseModules       = LicenseModules;
   TParTopLeft.LinkThreadParam     = &TParBotLeft;

   TParBotLeft.System               = AilSystem;
   TParBotLeft.OrgImage             = 0;
   TParBotLeft.SrcImage             = TParTopLeft.DstImage;
   TParBotLeft.DispImage            = TParTopLeft.DispImage;
   TParBotLeft.DispOffsetX          = 0;
   TParBotLeft.DispOffsetY          = IMAGE_HEIGHT;
   TParBotLeft.ReadyEvent           = TParTopLeft.DoneEvent;
   TParBotLeft.NumberOfIteration    = 0;
   TParBotLeft.Radius               = 0;
   TParBotLeft.Exit                 = 0;
   TParBotLeft.LicenseModules       = LicenseModules;
   TParBotLeft.LinkThreadParam     = 0;

   TParTopRight.System              = AilSystem;
   TParTopRight.OrgImage            = AilOrgImage;
   TParTopRight.DstImage            = TParTopRight.SrcImage;
   TParTopRight.DispImage           = TParTopLeft.DispImage;
   TParTopRight.DispOffsetX         = IMAGE_WIDTH;
   TParTopRight.DispOffsetY         = 0;
   TParTopRight.ReadyEvent          = TParBotRight.DoneEvent;
   TParTopRight.NumberOfIteration   = 0;
   TParTopRight.Radius              = 0;
   TParTopRight.Exit                = 0;
   TParTopRight.LicenseModules      = LicenseModules;
   TParTopRight.LinkThreadParam    = &TParBotRight;

   TParBotRight.System              = AilSystem;
   TParBotRight.OrgImage            = 0;
   TParBotRight.SrcImage            = TParTopRight.DstImage;
   TParBotRight.DispImage           = TParTopLeft.DispImage;
   TParBotRight.DispOffsetX         = IMAGE_WIDTH;
   TParBotRight.DispOffsetY         = IMAGE_HEIGHT;
   TParBotRight.ReadyEvent          = TParTopRight.DoneEvent;
   TParBotRight.NumberOfIteration   = 0;
   TParBotRight.Radius              = 0;
   TParBotRight.Exit                = 0;
   TParBotRight.LicenseModules      = LicenseModules;
   TParBotRight.LinkThreadParam    = 0;

   // Initialize the original image to process. 
   MbufLoad(IMAGE_FILE, AilOrgImage);

   // Start the 4 threads. 
   MthrAlloc(AilSystem, M_THREAD, M_DEFAULT, &TopThread     , 
                              &TParTopLeft,  &TParTopLeft.Id);
   MthrAlloc(AilSystem, M_THREAD, M_DEFAULT, &BotLeftThread , 
                              &TParBotLeft,  &TParBotLeft.Id);
   MthrAlloc(AilSystem, M_THREAD, M_DEFAULT, &TopThread     , 
                              &TParTopRight, &TParTopRight.Id);
   MthrAlloc(AilSystem, M_THREAD, M_DEFAULT, &BotRightThread, 
                              &TParBotRight, &TParBotRight.Id);

   // Start the timer. 
   MappTimer(M_DEFAULT, M_TIMER_RESET+M_SYNCHRONOUS, M_NULL);

   // Set events to start operation of top-left and top-right threads. 
   MthrControl(TParTopLeft.ReadyEvent,  M_EVENT_SET, M_SIGNALED);
   MthrControl(TParTopRight.ReadyEvent, M_EVENT_SET, M_SIGNALED);

   // Report that the threads are started and wait for a key press to stop them. 
   MosPrintf(AIL_TEXT("\nMULTI-THREADING:\n"));
   MosPrintf(AIL_TEXT("----------------\n\n"));
   MosPrintf(AIL_TEXT("4 threads running...\n"));
   MosPrintf(AIL_TEXT("Press any key to stop.\n\n"));
   MosGetch();

   // Signal the threads to exit. 
   TParTopLeft.Exit  = 1;
   TParTopRight.Exit = 1;

   // Wait for all threads to terminate. 
   MthrWait(TParTopLeft.Id , M_THREAD_END_WAIT, M_NULL);
   MthrWait(TParBotLeft.Id , M_THREAD_END_WAIT, M_NULL);
   MthrWait(TParTopRight.Id, M_THREAD_END_WAIT, M_NULL);
   MthrWait(TParBotRight.Id, M_THREAD_END_WAIT, M_NULL);

   // Stop the timer and calculate the number of frames per second processed. 
   MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &Time);
   FramesPerSecond = (TParTopLeft.NumberOfIteration + TParBotLeft.NumberOfIteration +
                     TParTopRight.NumberOfIteration + TParBotRight.NumberOfIteration)/Time;

   // Print statistics. 
   MosPrintf(AIL_TEXT("Top-left iterations done:     %4d.\n"), 
                                 (int)TParTopLeft.NumberOfIteration);
   MosPrintf(AIL_TEXT("Bottom-left iterations done:  %4d.\n"), 
                                 (int)TParBotLeft.NumberOfIteration);
   MosPrintf(AIL_TEXT("Top-right iterations done:    %4d.\n"), 
                                 (int)TParTopRight.NumberOfIteration);
   MosPrintf(AIL_TEXT("Bottom-right iterations done: %4d.\n\n"), 
                                 (int)TParBotRight.NumberOfIteration);
   MosPrintf(AIL_TEXT("Processing speed for the 4 threads: ")
                             AIL_TEXT("%.0f Images/Sec.\n\n"), FramesPerSecond);
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   // Free threads. 
   MthrFree(TParTopLeft.Id);
   MthrFree(TParBotLeft.Id);
   MthrFree(TParTopRight.Id);
   MthrFree(TParBotRight.Id);

   // Free events. 
   MthrFree(TParTopLeft.DoneEvent);
   MthrFree(TParBotLeft.DoneEvent);
   MthrFree(TParTopRight.DoneEvent);
   MthrFree(TParBotRight.DoneEvent);

   // Free buffers. 
   MbufFree(TParTopLeft.SrcImage);
   MbufFree(TParTopRight.SrcImage);
   MbufFree(TParBotLeft.DstImage);
   MbufFree(TParBotRight.DstImage);
   MbufFree(AilOrgImage);
   MbufFree(AilImage);

   // Free defaults. 
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }


// Top-left and top-right threads' function (Add an offset): 
// --------------------------------------------------------- 
AIL_UINT32 MFTYPE TopThread(void *ThreadParameters)
{
   THREAD_PARAM  *TPar = (THREAD_PARAM *)ThreadParameters;
   AIL_TEXT_CHAR Text[STRING_LENGTH_MAX];

   while (!TPar->Exit)
      {
      // Wait for bottom ready event before proceeding. 
      MthrWait(TPar->ReadyEvent, M_EVENT_WAIT, M_NULL);

      // For better visual effect, reset SrcImage to the original image regularly. 
      if ((TPar->NumberOfIteration % 192) == 0)
         MbufCopy(TPar->OrgImage, TPar->SrcImage);

   #if (!M_AIL_LITE)
      if (TPar->LicenseModules & M_LICENSE_IM)
         {
         // Add a constant to the image. 
         MimArith(TPar->SrcImage, 1L, TPar->DstImage, M_ADD_CONST+M_SATURATION);
         }
      else
   #endif
         {
         // Under AIL-Lite draw a variable size rectangle in the image. 
         TPar->Radius = TPar->LinkThreadParam->Radius = 
                      (TPar->NumberOfIteration % DRAW_RADIUS_NUMBER) * DRAW_RADIUS_STEP;
         MgraControl(M_DEFAULT, M_COLOR, 0xff);
         MgraRectFill(M_DEFAULT, TPar->DstImage, 
                     DRAW_CENTER_POSX - TPar->Radius, DRAW_CENTER_POSY - TPar->Radius, 
                     DRAW_CENTER_POSX + TPar->Radius, DRAW_CENTER_POSY + TPar->Radius);
        }

      // Increment iteration count and draw text. 
      TPar->NumberOfIteration++;
      MgraControl(M_DEFAULT, M_COLOR, 0xFF);
      MosSprintf(Text, STRING_LENGTH_MAX, AIL_TEXT("%d"), (int)TPar->NumberOfIteration);
      MgraText(M_DEFAULT, TPar->DstImage, STRING_POS_X, STRING_POS_Y, Text);

      // Update the display. 
      if (TPar->DispImage)
         {
         MbufCopyColor2d(TPar->DstImage,
                         TPar->DispImage,
                         M_ALL_BANDS, 0, 0,
                         M_ALL_BANDS,
                         TPar->DispOffsetX,
                         TPar->DispOffsetY,
                         IMAGE_WIDTH,
                         IMAGE_HEIGHT);
         }

      // Signal to the bottom thread that the first part of the processing is completed. 
      MthrControl(TPar->DoneEvent, M_EVENT_SET, M_SIGNALED);
      }

   // Require the bottom thread to exit. 
   TPar->LinkThreadParam->Exit = 1;

   // Signal the bottom thread to wake up. 
   MthrControl(TPar->DoneEvent, M_EVENT_SET, M_SIGNALED);

   // Before exiting the thread, make sure that all the commands are executed. 
   MthrWait(TPar->System, M_THREAD_WAIT, M_NULL);
   return(1L);
}


// Bottom-left thread function (Rotate): 
// ------------------------------------- 
AIL_UINT32 MFTYPE BotLeftThread(void *ThreadParameters)
{
   THREAD_PARAM  *TPar = (THREAD_PARAM *)ThreadParameters;
   AIL_TEXT_CHAR Text[STRING_LENGTH_MAX];
   AIL_DOUBLE Angle = 0, AngleIncrement = 0.5;

   while (!TPar->Exit)
      {
      // Wait for the event in top-left function to be ready before proceeding. 
      MthrWait(TPar->ReadyEvent, M_EVENT_WAIT, M_NULL);

   #if (!M_AIL_LITE)
      if (TPar->LicenseModules & M_LICENSE_IM)
         {
         // Rotate the image. 
         MimRotate(TPar->SrcImage, TPar->DstImage, Angle,
                   M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT,
                   M_NEAREST_NEIGHBOR+M_OVERSCAN_CLEAR);

         Angle += AngleIncrement;

         if (Angle >= 360)
            {
            Angle -= 360;
            }
         }
      else
   #endif
         {
         // Under AIL-Lite copy the top-left image and draw 
         // a variable size filled circle in the image. 
         MbufCopy(TPar->SrcImage,TPar->DstImage);
         MgraControl(M_DEFAULT, M_COLOR, 0x80);
         MgraArcFill(M_DEFAULT, TPar->DstImage, DRAW_CENTER_POSX, DRAW_CENTER_POSY,
                                                TPar->Radius, TPar->Radius, 0, 360);
         }

      // Increment iteration count and draw text. 
      TPar->NumberOfIteration++;
      MgraControl(M_DEFAULT, M_COLOR, 0xFF);
      MosSprintf(Text, STRING_LENGTH_MAX, AIL_TEXT("%d"), (int)TPar->NumberOfIteration);
      MgraText(M_DEFAULT, TPar->DstImage, STRING_POS_X, STRING_POS_Y, Text);

      // Update the display. 
      if (TPar->DispImage)
         {
         MbufCopyColor2d(TPar->DstImage,
                         TPar->DispImage,
                         M_ALL_BANDS, 0, 0,
                         M_ALL_BANDS,
                         TPar->DispOffsetX,
                         TPar->DispOffsetY,
                         IMAGE_WIDTH,
                         IMAGE_HEIGHT);
         }

      // Signal to the top-left thread that the last part of the processing is completed. 
      MthrControl(TPar->DoneEvent, M_EVENT_SET, M_SIGNALED);
      }
      
   // Before exiting the thread, make sure that all the commands are executed. 
   MthrWait(TPar->System, M_THREAD_WAIT, M_NULL);
   return(1L);
}

// Bottom-right thread function (Edge Detect): 
// ------------------------------------------- 
AIL_UINT32 MFTYPE BotRightThread(void *ThreadParameters)
{
   THREAD_PARAM  *TPar = (THREAD_PARAM *)ThreadParameters;
   AIL_TEXT_CHAR Text[STRING_LENGTH_MAX];

   while (!TPar->Exit)
      {
      // Wait for the event in top-right function to be ready before proceeding. 
      MthrWait(TPar->ReadyEvent, M_EVENT_WAIT, M_NULL);

   #if (!M_AIL_LITE)
      if (TPar->LicenseModules & M_LICENSE_IM)
         {
         // Perform an edge detection operation on the image. 
         MimConvolve(TPar->SrcImage, TPar->DstImage, M_EDGE_DETECT_SOBEL_FAST);
         }
      else
   #endif
         {
         // Under AIL-Lite copy the top-right image and draw 
         // a variable size filled circle in the image. 
         MbufCopy(TPar->SrcImage,TPar->DstImage);
         MgraControl(M_DEFAULT, M_COLOR, 0x40);
         MgraArcFill(M_DEFAULT, TPar->DstImage, DRAW_CENTER_POSX, DRAW_CENTER_POSY,
                     TPar->Radius/2, TPar->Radius/2, 0, 360);
         }

      // Increment iteration count and draw text. 
      TPar->NumberOfIteration++;
      MgraControl(M_DEFAULT, M_COLOR, 0xFF);
      MosSprintf(Text, STRING_LENGTH_MAX, AIL_TEXT("%d"), (int)TPar->NumberOfIteration);
      MgraText(M_DEFAULT, TPar->DstImage, STRING_POS_X, STRING_POS_Y, Text);

      // Update the display. 
      if (TPar->DispImage)
         {
         MbufCopyColor2d(TPar->DstImage,
                         TPar->DispImage,
                         M_ALL_BANDS, 0, 0,
                         M_ALL_BANDS,
                         TPar->DispOffsetX,
                         TPar->DispOffsetY,
                         IMAGE_WIDTH,
                         IMAGE_HEIGHT);
         }

      // Signal to the top-right thread that the last part of the processing is completed. 
      MthrControl(TPar->DoneEvent, M_EVENT_SET, M_SIGNALED);
      }
      
   // Before exiting the thread, make sure that all the commands are executed. 
   MthrWait(TPar->System, M_THREAD_WAIT, M_NULL);
   return(1L);
}

```
