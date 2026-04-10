---
title: "M3dreg"
description: "This program demonstrates how to use the 3D surface registration to register several partial point clouds of a 3D object and stitch them into a single complete point cloud."
ms.language: "C++"
---

# M3dreg

> This program demonstrates how to use the 3D surface registration to register several partial point clouds of a 3D object and stitch them into a single complete point cloud.

**Language:** C++

**Functions used:** `M3dregAlloc`, `M3dregAllocResult`, `M3dregControl`, `M3dregInquire`, `M3dregSetLocation`, `M3dregCalculate`, `M3dregMerge`, `M3dregFree`, `M3dimAlloc`, `M3dimAllocResult`, `M3dimControl`, `M3dimStat`, `M3dimGetResult`, `M3dimFree`, `M3ddispCopy`, `M3dgeoInquire`, `M3dgeoLine`, `MappAlloc`, `MappFree`, `MappTimer`, `MbufAllocContainer`, `MbufInquireContainer`, `MbufImport`, `MbufAllocColor`, `MbufAlloc2d`, `MbufClear`, `MbufFree`, `M3ddispAlloc`, `M3ddispControl`, `M3ddispSelect`, `M3ddispFree`, `MsysAlloc`, `MsysFree`, `M3ddispSetView`, `MappControl`, `MbufAllocComponent`, `MobjInquire`, `M3dimCalculateMapSize`, `M3dimCalibrateDepthMap`, `M3dimProject`, `M3dimRotate`, `MdispAlloc`, `MdispFree`, `MdispSelect`

**Categories:** Overview, General, Industries, Machining, Automation, Applications, Stitching, 3D profiling, Modules, 3D Registration, Buffer, 3D Display, 3D Graphics, Image Processing, 3D Image Processing, 3D Geometry, What's New, AIL 10.0 SP4

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    M3dreg.cpp
//
// Description: This example demonstrates how to use the 3D registration module
//              to stitch several partial point clouds of a 3D object together
//              into a single complete point cloud.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// -------------------------------------------------------------- 
// Example description.                                           
// -------------------------------------------------------------- 
void PrintHeader()
   {
   MosPrintf(AIL_TEXT("[EXAMPLE NAME]\n"));
   MosPrintf(AIL_TEXT("M3dreg\n\n"));

   MosPrintf(AIL_TEXT("[SYNOPSIS]\n"));
   MosPrintf(AIL_TEXT("This example demonstrates how to use the 3D Registration module \n"));
   MosPrintf(AIL_TEXT("to stitch several partial point clouds of a 3D object together \n"));

   MosPrintf(AIL_TEXT("into a single complete point cloud.\n"));
   MosPrintf(AIL_TEXT("\n"));

   MosPrintf(AIL_TEXT("[MODULES USED]\n"));
   MosPrintf(AIL_TEXT("Modules used: 3D Registration, 3D Display, 3D Graphics, and 3D Image\nProcessing.\n\n"));
   }

// Input scanned point cloud (PLY) files. 
static const AIL_INT NUM_SCANS = 6;
static const AIL_TEXT_CHAR* const FILE_POINT_CLOUD[NUM_SCANS] =
   {
   M_IMAGE_PATH AIL_TEXT("Cloud1.ply"),
   M_IMAGE_PATH AIL_TEXT("Cloud2.ply"),
   M_IMAGE_PATH AIL_TEXT("Cloud3.ply"),
   M_IMAGE_PATH AIL_TEXT("Cloud4.ply"),
   M_IMAGE_PATH AIL_TEXT("Cloud5.ply"),
   M_IMAGE_PATH AIL_TEXT("Cloud6.ply"),
   }; 

// The colors assigned for each cloud. 
const AIL_INT Color[NUM_SCANS + 1] =
   {
   M_RGB888(0,   159, 255),
   M_RGB888(154,  77,  66),
   M_RGB888(0,   255, 190),
   M_RGB888(120,  63, 193),
   M_RGB888(31,  150, 152),
   M_RGB888(255, 172, 253),
   M_RGB888(177, 204, 113)
   };

// Utility functions. 
void   ColorCloud(AIL_ID AilPointCloud, AIL_INT Col);
void   AutoRotateDisplay(AIL_ID AilSystem, AIL_ID AilDisplay);
AIL_ID Alloc3dDisplayId(AIL_ID AilSystem);
void   DisplayContainer(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilContainer, AIL_ID* pAilDepthMap, AIL_ID* pAilIntensityMap);
void   UpdateDisplay(AIL_ID AilSystem, AIL_ID AilContainer, AIL_ID AilDepthMap, AIL_ID AilIntensityMap);
void   FreeDisplay(AIL_ID AilDisplay);
// -------------------------------------------------------------- 

int MosMain()
   {
   // Print example information in console. 
   PrintHeader();

   AIL_ID AilApplication,   // application identifier 
          AilSystem,        // System identifier      
          AilDisplay;       // Display identifier     
   
   // Allocate objects. 
   MappAlloc(M_NULL, M_DEFAULT, &AilApplication);
   MsysAlloc(M_DEFAULT, M_SYSTEM_DEFAULT, M_DEFAULT, M_DEFAULT, &AilSystem);

   AIL_ID AilContainerIds[NUM_SCANS];
   MosPrintf(AIL_TEXT("Reading the PLY files of 6 partial point clouds"));
   for(AIL_INT i = 0; i < NUM_SCANS; ++i)
      {
      MosPrintf(AIL_TEXT("."));
      AilContainerIds[i] = MbufImport(FILE_POINT_CLOUD[i], M_DEFAULT, M_RESTORE, AilSystem, M_NULL);
      ColorCloud(AilContainerIds[i], Color[i]);
      }
   MosPrintf(AIL_TEXT("\n\n"));

   AilDisplay = Alloc3dDisplayId(AilSystem);

   AIL_ID AilDisplayImage = M_NULL; // Used for 2D display if needed.
   AIL_ID AilDepthMap = M_NULL;     // Used for 2D display if needed.

   // Display the first point cloud container. 
   DisplayContainer(AilSystem, AilDisplay, AilContainerIds[0], &AilDepthMap, &AilDisplayImage);
   AutoRotateDisplay(AilSystem, AilDisplay);

   MosPrintf(AIL_TEXT("Showing the first partial point cloud of the object.\n"));
   MosPrintf(AIL_TEXT("Press any key to start.\n\n"));
   MosGetch();
  
   // Allocate context and result for 3D registration (stitching). 
   AIL_ID AilContext = M3dregAlloc(AilSystem, M_PAIRWISE_REGISTRATION_CONTEXT, M_DEFAULT, M_NULL);
   AIL_ID AilResult  = M3dregAllocResult(AilSystem, M_PAIRWISE_REGISTRATION_RESULT, M_DEFAULT, M_NULL);

   M3dregControl(AilContext, M_DEFAULT, M_NUMBER_OF_REGISTRATION_ELEMENTS, NUM_SCANS);
   M3dregControl(AilContext, M_DEFAULT, M_MAX_ITERATIONS, 40);

   // Pairwise registration context controls.                                 
   // Use normal subsampling to preserve edges and yield faster registration. 
   AIL_ID AilSubsampleContext = M_NULL;
   M3dregInquire(AilContext, M_DEFAULT, M_SUBSAMPLE_CONTEXT_ID, &AilSubsampleContext);
   M3dregControl(AilContext, M_DEFAULT, M_SUBSAMPLE, M_ENABLE);

   // Keep edge points. 
   M3dimControl(AilSubsampleContext, M_SUBSAMPLE_MODE, M_SUBSAMPLE_NORMAL);
   M3dimControl(AilSubsampleContext, M_NEIGHBORHOOD_DISTANCE, 10);
 
   // Chain of set location, i==0 is referencing to the GLOBAL. 
   for(AIL_INT i = 1; i < NUM_SCANS; ++i)
      { M3dregSetLocation(AilContext, i, i-1, M_IDENTITY_MATRIX, M_DEFAULT, M_DEFAULT, M_DEFAULT); }

   MosPrintf(AIL_TEXT("The 3D stitching between partial point clouds has been performed with\n")
             AIL_TEXT("the help of the points within the expected common overlap regions.\n\n"));

   // Calculate the time to perform the registration. 
   AIL_DOUBLE ComputationTimeMS;
   MappTimer(M_TIMER_RESET, M_NULL);

   // Perform the registration (stitching). 
   M3dregCalculate(AilContext, AilContainerIds, NUM_SCANS, AilResult, M_DEFAULT);

   ComputationTimeMS = MappTimer(M_TIMER_READ, M_NULL) * 1000.0;

   MosPrintf(AIL_TEXT("The registration of the 6 partial point clouds succeeded in %.2f ms.\n\n"),
             ComputationTimeMS);

   // Merging overlapping point clouds will result into unneeded large number of points at 
   // the overlapping regions.                                                             
   // During merging, subsampling is used to ensure that the density of points             
   // is fairly dense, but without replications.                                           
   AIL_DOUBLE GridSize;
   AIL_ID StatResultId = M3dimAllocResult(AilSystem, M_STATISTICS_RESULT, M_DEFAULT, M_NULL);
   M3dimStat(M_STAT_CONTEXT_DISTANCE_TO_NEAREST_NEIGHBOR, AilContainerIds[0], StatResultId, M_DEFAULT);

   // Nearest neighbour distances gives a measure of the point cloud density. 
   M3dimGetResult(StatResultId, M_DISTANCE_TO_NEAREST_NEIGHBOR_MIN, &GridSize);

   // Use the measured point cloud density as guide for the subsampling. 
   AIL_ID AilMergeSubsampleContext = M3dimAlloc(AilSystem , M_SUBSAMPLE_CONTEXT, M_DEFAULT, M_NULL);
   M3dimControl(AilMergeSubsampleContext, M_SUBSAMPLE_MODE, M_SUBSAMPLE_GRID);
   M3dimControl(AilMergeSubsampleContext, M_GRID_SIZE_X, GridSize);
   M3dimControl(AilMergeSubsampleContext, M_GRID_SIZE_Y, GridSize);
   M3dimControl(AilMergeSubsampleContext, M_GRID_SIZE_Z, GridSize);

   // Allocate the point cloud for the final stitched clouds. 
   AIL_ID AilStitchedId = MbufAllocContainer(AilSystem, M_PROC + M_DISP, M_DEFAULT, M_NULL);

   MosPrintf(AIL_TEXT("The merging of point clouds will be shown incrementally.\n"));
   MosPrintf(AIL_TEXT("Press any key to show 2 merged point clouds of 6.\n\n"));
   MosGetch();

   // Merge can merge all clouds at once, but here it is done incrementally for the display. 
   for(AIL_INT i = 2; i <= NUM_SCANS; ++i)
      {
      M3dregMerge(AilResult, AilContainerIds, i, AilStitchedId, AilMergeSubsampleContext, M_DEFAULT);

      if(i == 2)
         { DisplayContainer(AilSystem, AilDisplay, AilStitchedId, &AilDepthMap, &AilDisplayImage); }
      else
         { UpdateDisplay(AilSystem, AilStitchedId, AilDepthMap, AilDisplayImage); }

      if(i < NUM_SCANS)
         {
         MosPrintf(AIL_TEXT("Press any key to show %d merged point clouds of 6.\n\n"), i + 1);
         }
      else
         {
         MosPrintf(AIL_TEXT("Press any key to end.\n"));
         }

      AutoRotateDisplay(AilSystem, AilDisplay);
      MosGetch();
      }
    
   // Free Objects. 
   for(AIL_INT i = 0; i < NUM_SCANS; ++i)
      { MbufFree(AilContainerIds[i]); }

   MbufFree(AilStitchedId);
   M3dimFree(StatResultId);
   M3dimFree(AilMergeSubsampleContext);
   M3dregFree(AilContext);
   M3dregFree(AilResult);
   FreeDisplay(AilDisplay);
   if(AilDisplayImage)
      MbufFree(AilDisplayImage);
   if(AilDepthMap)
      MbufFree(AilDepthMap);
   MsysFree(AilSystem);
   MappFree(AilApplication);

   return 0;
   }

// -------------------------------------------------------------- 
// Color the container.                                           
// -------------------------------------------------------------- 
void ColorCloud(AIL_ID AilPointCloud, AIL_INT Col)
   {
   AIL_INT SizeX = MbufInquireContainer(AilPointCloud, M_COMPONENT_RANGE, M_SIZE_X, M_NULL);
   AIL_INT SizeY = MbufInquireContainer(AilPointCloud, M_COMPONENT_RANGE, M_SIZE_Y, M_NULL);
   AIL_ID ReflectanceId = MbufAllocComponent(AilPointCloud, 3, SizeX, SizeY, 8 + M_UNSIGNED, M_IMAGE, M_COMPONENT_REFLECTANCE, M_NULL);
   MbufClear(ReflectanceId, static_cast<AIL_DOUBLE>(Col));
   }

// -------------------------------------------------------------- 
// Auto rotate the 3D object.                                     
// -------------------------------------------------------------- 
void AutoRotateDisplay(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_INT64 DisplayType;
   MobjInquire(AilDisplay, M_OBJECT_TYPE, &DisplayType);

   // AutoRotate available only for the 3D display. 
   if(DisplayType != M_3D_DISPLAY)
      return;

   // By default the display rotates around the Z axis, but the robot is oriented along the Y axis. 
   AIL_ID Geometry = M3dgeoAlloc(AilSystem, M_GEOMETRY, M_DEFAULT, M_NULL);
   M3ddispCopy(AilDisplay, Geometry, M_ROTATION_AXIS, M_DEFAULT);
   AIL_DOUBLE CenterX = M3dgeoInquire(Geometry, M_START_POINT_X, M_NULL);
   AIL_DOUBLE CenterY = M3dgeoInquire(Geometry, M_START_POINT_Y, M_NULL);
   AIL_DOUBLE CenterZ = M3dgeoInquire(Geometry, M_START_POINT_Z, M_NULL);
   M3dgeoLine(Geometry, M_POINT_AND_VECTOR, M_UNCHANGED, M_UNCHANGED, M_UNCHANGED, 0, 1, 0, M_UNCHANGED, M_DEFAULT);
   M3ddispCopy(Geometry, AilDisplay, M_ROTATION_AXIS, M_DEFAULT);
   M3ddispControl(AilDisplay, M_AUTO_ROTATE, M_ENABLE);
   M3dgeoFree(Geometry);
   }

// -------------------------------------------------------------- 
// Allocates a 3D display and returns its identifier.           
// -------------------------------------------------------------- 
AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
   {
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_DISABLE);
   AIL_ID AilDisplay = M3ddispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_NULL);
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_ENABLE);

   if(!AilDisplay)
      {
      MosPrintf(AIL_TEXT("\n")
                AIL_TEXT("The current system does not support the 3D display.\n")
                AIL_TEXT("A 2D display will be used instead.\n")
                AIL_TEXT("Press any key to continue.\n"));
      MosGetch();

      // Allocate a 2D Display instead. 
      AilDisplay = MdispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_WINDOWED, M_NULL);
      }
   else
      {
      // Adjust the viewpoint of the 3D display. 
      M3ddispSetView(AilDisplay, M_AUTO, M_BOTTOM_VIEW, M_DEFAULT, M_DEFAULT, M_DEFAULT);
      MosPrintf(AIL_TEXT("Press <R> on the display window to stop/start the rotation.\n\n"));
      }

   return AilDisplay;
   }

// -------------------------------------------------------------- 
// Display the received container.                                
// -------------------------------------------------------------- 
void DisplayContainer(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilContainer, AIL_ID* pAilDepthMap, AIL_ID* pAilIntensityMap)
   {
   AIL_INT64 DisplayType;
   MobjInquire(AilDisplay, M_OBJECT_TYPE, &DisplayType);

   bool Use3D = (DisplayType == M_3D_DISPLAY);
   if(Use3D)
      {
      M3ddispSelect(AilDisplay, AilContainer, M_ADD, M_DEFAULT);
      M3ddispSelect(AilDisplay, M_NULL, M_OPEN, M_DEFAULT);
      }
   else
      {
      if(*pAilDepthMap == M_NULL)
         {
         AIL_INT SizeX = 0; // Image size X for 2d display. 
         AIL_INT SizeY = 0; // Image size Y for 2d display. 

         M3dimCalculateMapSize(M_DEFAULT, AilContainer, M_NULL, M_DEFAULT, &SizeX, &SizeY);

         *pAilIntensityMap = MbufAllocColor(AilSystem, 3, SizeX, SizeY, M_UNSIGNED + 8, M_IMAGE | M_PROC | M_DISP, M_NULL);
         *pAilDepthMap     = MbufAlloc2d   (AilSystem,    SizeX, SizeY, M_UNSIGNED + 8, M_IMAGE | M_PROC | M_DISP, M_NULL);

         M3dimCalibrateDepthMap(AilContainer, *pAilDepthMap, *pAilIntensityMap, M_NULL, M_DEFAULT, M_DEFAULT, M_CENTER);
         }

      // Rotate the point cloud container to be in the xy plane before projecting. 
      AIL_ID RotatedContainer = MbufAllocContainer(AilSystem, M_PROC, M_DEFAULT, M_NULL);

      M3dimRotate(AilContainer, RotatedContainer, M_ROTATION_XYZ, 90, 60, 0, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT);     
      M3dimProject(AilContainer, *pAilDepthMap, *pAilIntensityMap, M_DEFAULT, M_MIN_Z, M_DEFAULT, M_DEFAULT);

      // Display the projected point cloud container. 
      MdispSelect(AilDisplay, *pAilIntensityMap);
      MbufFree(RotatedContainer);
      }
   }

// -------------------------------------------------------------- 
// Updated the displayed image.                                   
// -------------------------------------------------------------- 
void UpdateDisplay(AIL_ID AilSystem, AIL_ID AilContainer, AIL_ID AilDepthMap, AIL_ID AilIntensityMap)
   {
   if(!AilDepthMap)
      { return; }

   AIL_ID RotatedContainer = MbufAllocContainer(AilSystem, M_PROC, M_DEFAULT, M_NULL);

   M3dimRotate(AilContainer, RotatedContainer, M_ROTATION_XYZ, 90, 70, 0, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT);
   M3dimProject(AilContainer, AilDepthMap, AilIntensityMap, M_DEFAULT, M_MIN_Z, M_DEFAULT, M_DEFAULT);

   MbufFree(RotatedContainer);
   }

// -------------------------------------------------------------- 
// Free the display.                                              
// -------------------------------------------------------------- 
void FreeDisplay(AIL_ID AilDisplay)
   {
   AIL_INT64 DisplayType;
   MobjInquire(AilDisplay, M_OBJECT_TYPE, &DisplayType);

   if(DisplayType == M_DISPLAY)
      { MdispFree(AilDisplay); }
   else
      { M3ddispFree(AilDisplay); }
   }

```
