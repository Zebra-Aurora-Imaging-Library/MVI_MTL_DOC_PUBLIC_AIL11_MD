---
title: "MimLocatePeak1d"
description: "This program finds the peak in each column of an input sequence and reconstruct the height of a 3D object using it."
ms.language: "C++"
---

# MimLocatePeak1d

> This program finds the peak in each column of an input sequence and reconstruct the height of a 3D object using it.

**Language:** C++

**Functions used:** `M3ddispAlloc`, `M3ddispControl`, `M3ddispFree`, `M3ddispSelect`, `MappControl`, `MbufAllocComponent`, `MbufAllocContainer`, `MbufConvert3d`, `McalControl`, `McalUniform`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MimAlloc`, `MappAlloc`, `MappFree`, `MappTimer`, `MbufAlloc2d`, `MbufClear`, `MbufCopy`, `MbufDiskInquire`, `MbufFree`, `MbufImportSequence`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MimAllocResult`, `MimDraw`, `MimFree`, `MimGetResult`, `MimLocatePeak1d`, `MimStatCalculate`, `MimArith`, `MimClip`, `MimControl`, `MimResize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Image Processing, 3D Display, Calibration, Graphics, What's New, AIL 10.0 SP4, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImLocatePeak1d.cpp
//
// Description: This program finds the peak in each column of an input sequence
//              and reconstruct the height of a 3D object using it.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Input sequence specifications.
#define SEQUENCE_FILE   M_IMAGE_PATH AIL_TEXT("HandWithLaser.avi")

//     ^            +
//     |        +       +
//     |      + <-Width-> + <------------
//     |     +             +             | Min contrast
//     | ++++               ++++++++ <---
//     |
//     |
//     ------------------------------>
//        Peak intensity profile

// Peak detection parameters.
#define LINE_WIDTH_AVERAGE      20
#define LINE_WIDTH_DELTA        20
#define MIN_CONTRAST           100.0
#define NB_FIXED_POINT           4

// 3D display parameters.
#define M3D_MESH_SCALING_X     1.0
#define M3D_MESH_SCALING_Y     4.0
#define M3D_MESH_SCALING_Z    -0.13

// Utility functions.
AIL_ID Alloc3dDisplayId(AIL_ID AilSystem);
void AutoRotate3dDisplay(AIL_ID AilDisplay);

//*****************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication,          // Application identifier.        
          AilSystem,               // System identifier.             
          AilDisplay,              // Display identifier.            
          AilDisplayImage,         // Image buffer identifier.       
          AilGraList,              // Graphic list identifier.       
          AilImage,                // Image buffer identifier.       
          AilPosYImage,            // Image buffer identifier.       
          AilValImage,             // Image buffer identifier.       
          AilContext,              // Processing context identifier. 
          AilLocatePeak,           // Processing result identifier.  
          AilStatContext = M_NULL, // Statistics context identifier. 
          AilExtreme = M_NULL;     // Result buffer identifier.      

   AIL_INT SizeX,
           SizeY,
           NumberOfImages,  
           n;
   AIL_INT ExtremePosY[2] = { 0, 0 };
   AIL_DOUBLE FrameRate,
              PreviousTime,
              StartTime,
              EndTime,
              TotalProcessTime,
              WaitTime;

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem,
      &AilDisplay, M_NULL, M_NULL);

   // Inquire characteristics of the input sequence.
   MbufDiskInquire(SEQUENCE_FILE, M_SIZE_X, &SizeX);
   MbufDiskInquire(SEQUENCE_FILE, M_SIZE_Y, &SizeY);
   MbufDiskInquire(SEQUENCE_FILE, M_NUMBER_OF_IMAGES, &NumberOfImages);
   MbufDiskInquire(SEQUENCE_FILE, M_FRAME_RATE, &FrameRate);

   // Allocate buffers to hold images.
   MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + M_UNSIGNED, M_IMAGE + M_PROC, &AilImage);
   MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + M_UNSIGNED, M_IMAGE + M_DISP, &AilDisplayImage);
   MbufAlloc2d(AilSystem, SizeX, NumberOfImages, 16 + M_UNSIGNED, M_IMAGE + M_PROC, &AilPosYImage);
   MbufAlloc2d(AilSystem, SizeX, NumberOfImages, 8 + M_UNSIGNED, M_IMAGE + M_PROC, &AilValImage);

   // Allocate context for MimLocatePeak1D.
   MimAlloc(AilSystem, M_LOCATE_PEAK_1D_CONTEXT, M_DEFAULT, &AilContext);

   // Allocate result for MimLocatePeak1D.
   MimAllocResult(AilSystem, M_DEFAULT, M_LOCATE_PEAK_1D_RESULT, &AilLocatePeak);

   // Allocate graphic list.
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraList);

   // Select display.
   MdispSelect(AilDisplay, AilDisplayImage);
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraList);

   // Print a message.
   MosPrintf(AIL_TEXT("\nEXTRACTING 3D IMAGE FROM A LASER LINE (SHEET-OF-LIGHT):\n"));
   MosPrintf(AIL_TEXT("--------------------------------------------------------\n\n"));
   MosPrintf(AIL_TEXT("The position of a laser line is being extracted from ")
      AIL_TEXT("an image\n"));
   MosPrintf(AIL_TEXT("to generate a depth image.\n\n"));

   // Open the sequence file for reading.
   MbufImportSequence(SEQUENCE_FILE, M_DEFAULT, M_NULL, M_NULL, NULL, M_NULL, M_NULL, M_OPEN);

   // Preprocess the context.
   MimLocatePeak1d(AilContext,
      AilImage,
      AilLocatePeak,
      M_NULL,
      M_NULL,
      M_NULL,
      M_PREPROCESS,
      M_DEFAULT);

   // Read and process all images in the input sequence.
   MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &PreviousTime);
   TotalProcessTime = 0.0;

   for (n = 0; n < NumberOfImages; n++)
      {
      // Read image from sequence.
      MbufImportSequence(SEQUENCE_FILE, M_DEFAULT, M_LOAD, M_NULL,
         &AilImage, M_DEFAULT, 1, M_READ);

      // Display the image.
      MbufCopy(AilImage, AilDisplayImage);

      // Locate the peak in each column of the image.
      MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &StartTime);

      MimLocatePeak1d(AilContext,
         AilImage,
         AilLocatePeak,
         LINE_WIDTH_AVERAGE,
         LINE_WIDTH_DELTA,
         MIN_CONTRAST,
         M_DEFAULT,
         M_DEFAULT);

      // Draw extracted peaks.
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);
      MgraClear(M_DEFAULT, AilGraList);
      MimDraw(M_DEFAULT, AilLocatePeak, M_NULL, AilGraList, M_DRAW_PEAKS + M_CROSS, M_ALL, M_DEFAULT, M_DEFAULT);

      // Draw peak's data to depth map.
      MimDraw(M_DEFAULT, AilLocatePeak, M_NULL, AilPosYImage, M_DRAW_DEPTH_MAP_ROW, AIL_DOUBLE(n), M_NULL, M_FIXED_POINT + NB_FIXED_POINT);
      MimDraw(M_DEFAULT, AilLocatePeak, M_NULL, AilValImage, M_DRAW_INTENSITY_MAP_ROW, AIL_DOUBLE(n), M_NULL, M_DEFAULT);

      MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &EndTime);
      TotalProcessTime += EndTime - StartTime;

      // Wait to have a proper frame rate.
      WaitTime = (1.0 / FrameRate) - (EndTime - PreviousTime);
      if (WaitTime > 0)
         MappTimer(M_DEFAULT, M_TIMER_WAIT, &WaitTime);
      MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &PreviousTime);
      }

   MgraClear(M_DEFAULT, AilGraList);

   // Close the sequence file.
   MbufImportSequence(SEQUENCE_FILE, M_DEFAULT, M_NULL, M_NULL, NULL, M_NULL, M_NULL, M_CLOSE);

   MosPrintf(AIL_TEXT("%d images processed in %7.2lf s (%7.2lf ms/image).\n"),
      (int)NumberOfImages, TotalProcessTime,
      TotalProcessTime / (AIL_DOUBLE)NumberOfImages * 1000.0);

   // Pause to show the result.
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   MosPrintf(AIL_TEXT("The reconstructed images are being displayed.\n"));

   // Draw extracted peak position in each column of each image.
   const AIL_INT VisualizationDelayMsec = 10;
   for (n = 0; n < NumberOfImages; n++)
      {
      MbufClear(AilImage, 0);
      MimDraw(M_DEFAULT, AilPosYImage, AilValImage, AilImage,
         M_DRAW_PEAKS + M_VERTICAL + M_LINES, (AIL_DOUBLE)n,
         1, M_FIXED_POINT + NB_FIXED_POINT);

      // Display the result image.
      MbufCopy(AilImage, AilDisplayImage);

      MosSleep(VisualizationDelayMsec);
      }

   // Pause to show the result.
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Try to allocate 3D display.
   AIL_ID AilDisplay3D = Alloc3dDisplayId(AilSystem);
   if(AilDisplay3D)
      {
      McalUniform(AilPosYImage, 0.0, 0.0, M3D_MESH_SCALING_X, M3D_MESH_SCALING_Y, 0.0, M_DEFAULT);
      McalControl(AilPosYImage, M_GRAY_LEVEL_SIZE_Z, M3D_MESH_SCALING_Z);

      AIL_ID ContainerId = MbufAllocContainer(AilSystem, M_PROC | M_DISP, M_DEFAULT, M_NULL);
      MbufConvert3d(AilPosYImage, ContainerId, M_NULL, M_DEFAULT, M_DEFAULT);
      AIL_ID Reflectance = MbufAllocComponent(ContainerId, 1, SizeX, NumberOfImages, 8 + M_UNSIGNED, M_IMAGE, M_COMPONENT_REFLECTANCE, M_NULL);
      MbufCopy(AilValImage, Reflectance);
      MosPrintf(AIL_TEXT("The depth buffer is displayed using a 3D display.\n"));
      MosPrintf(AIL_TEXT("Press <R> on the display window to stop/start the rotation.\n\n"));

      // Hide display.
      MdispControl(AilDisplay, M_WINDOW_SHOW, M_DISABLE);

      M3ddispSelect(AilDisplay3D, ContainerId, M_SELECT, M_DEFAULT);
      AutoRotate3dDisplay(AilDisplay3D);

      // Pause to show the result.
      MosPrintf(AIL_TEXT("Press any key to end.\n"));
      MosGetch();
      M3ddispFree(AilDisplay3D);
      MbufFree(ContainerId);      
      }
   else
      {
      MosPrintf(AIL_TEXT("The depth buffer is displayed using a 2D display.\n"));

      // Find the remapping for result buffers.
      MimAlloc(AilSystem, M_STATISTICS_CONTEXT, M_DEFAULT, &AilStatContext);
      MimAllocResult(AilSystem, M_DEFAULT, M_STATISTICS_RESULT, &AilExtreme);

      MimControl(AilStatContext, M_STAT_MIN, M_ENABLE);
      MimControl(AilStatContext, M_STAT_MAX, M_ENABLE);
      MimControl(AilStatContext, M_CONDITION, M_NOT_EQUAL);
      MimControl(AilStatContext, M_COND_LOW, 0xFFFF);

      MimStatCalculate(AilStatContext, AilPosYImage, AilExtreme, M_DEFAULT);
      MimGetResult(AilExtreme, M_STAT_MIN + M_TYPE_AIL_INT, &ExtremePosY[0]);
      MimGetResult(AilExtreme, M_STAT_MAX + M_TYPE_AIL_INT, &ExtremePosY[1]);

      MimFree(AilExtreme);
      MimFree(AilStatContext);

      // Free the display and reallocate a new one of the proper dimension for results.
      MbufFree(AilDisplayImage);
      MbufAlloc2d(AilSystem,
         (AIL_INT) ( (AIL_DOUBLE) SizeX * (M3D_MESH_SCALING_X > 0 ? M3D_MESH_SCALING_X : -M3D_MESH_SCALING_X) ),
         (AIL_INT) ( (AIL_DOUBLE) NumberOfImages * M3D_MESH_SCALING_Y),
         8 + M_UNSIGNED,
         M_IMAGE + M_PROC + M_DISP,
         &AilDisplayImage);

      MdispSelect(AilDisplay, AilDisplayImage);

      // Display the height buffer.
      MimClip(AilPosYImage, AilPosYImage, M_GREATER,
         (AIL_DOUBLE) ExtremePosY[1], M_NULL,
         (AIL_DOUBLE) ExtremePosY[1], M_NULL);
      MimArith(AilPosYImage, (AIL_DOUBLE) ExtremePosY[0],
         AilPosYImage, M_SUB_CONST);
      MimArith(AilPosYImage, ((ExtremePosY[1] - ExtremePosY[0]) / 255.0) + 1,
         AilPosYImage, M_DIV_CONST);
      MimResize(AilPosYImage, AilDisplayImage,
         M_FILL_DESTINATION,
         M_FILL_DESTINATION,
         M_BILINEAR);

      // Pause to show the result.
      MosPrintf(AIL_TEXT("Press any key to end.\n"));
      MosGetch();
      }

   // Free all allocations.
   MimFree(AilLocatePeak);
   MimFree(AilContext);
   MbufFree(AilImage);
   MgraFree(AilGraList);
   MbufFree(AilDisplayImage);
   MbufFree(AilPosYImage);
   MbufFree(AilValImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }

//*****************************************************************************
// Allocates a 3D display and returns its identifier. 
//*****************************************************************************
AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
   {
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_DISABLE);
   AIL_ID AilDisplay3D = M3ddispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_NULL);
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_ENABLE);

   if(!AilDisplay3D)
      {
      MosPrintf(AIL_TEXT("\n")
                AIL_TEXT("The current system does not support the 3D display.\n\n"));
      }
   return AilDisplay3D;
   }

//****************************************************************************
// Auto rotate the 3D object.
//****************************************************************************
void AutoRotate3dDisplay(AIL_ID AilDisplay)
   {
   M3ddispControl(AilDisplay, M_AUTO_ROTATE, M_ENABLE);
   }

```
