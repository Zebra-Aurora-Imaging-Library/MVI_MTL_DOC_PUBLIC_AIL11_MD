---
title: "M3dmod"
description: "This example demonstrates how to use the 3D model finder module to define surface models and search for them in 3D scenes."
ms.language: "C++"
---

# M3dmod

> This example demonstrates how to use the 3D model finder module to define surface models and search for them in 3D scenes.

**Language:** C++

**Functions used:** `M3ddispAlloc`, `M3ddispControl`, `M3ddispCopy`, `M3ddispFree`, `M3ddispInquire`, `M3ddispSelect`, `M3ddispSetView`, `M3dgraControl`, `M3dgraCopy`, `M3dgraRemove`, `M3dmodAlloc`, `M3dmodAllocResult`, `M3dmodControl`, `M3dmodDefine`, `M3dmodDraw3d`, `M3dmodFind`, `M3dmodFree`, `M3dmodGetResult`, `M3dmodPreprocess`, `MappAlloc`, `MappControl`, `MappFree`, `MappTimer`, `MbufFree`, `MbufInquireContainer`, `MbufRestore`, `MobjInquire`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Automation, Applications, Finding/Locating, Identifying, Modules, 3D Display, 3D Graphics, 3D Model Finder, Buffer, What's New, AIL 10.0 SP6

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    M3dmod.cpp
//
// Description: This example demonstrates how to use the 3D model finder module
//              to define surface models and search for them in 3D scenes.
//              A simple single model search is presented first followed by a more
//              complex example of multiple occurrences in a complex scene.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include "CDisplay.h"
// -------------------------------------------------------------- 
// Example description.                                           
// -------------------------------------------------------------- 
void PrintHeader()
   {
   MosPrintf(AIL_TEXT("[EXAMPLE NAME]\n"));
   MosPrintf(AIL_TEXT("M3dmod\n\n"));

   MosPrintf(AIL_TEXT("[SYNOPSIS]\n"));
   MosPrintf(AIL_TEXT("This example demonstrates how to use the 3D model finder module\n"));
   MosPrintf(AIL_TEXT("to define surface models and search for them in 3D scenes.\n"));
   MosPrintf(AIL_TEXT("\n"));

   MosPrintf(AIL_TEXT("[MODULES USED]\n"));
   MosPrintf(AIL_TEXT("Modules used: 3D Model Finder, 3D Display, 3D Graphics, and 3D Image\n"
                      "Processing.\n\n"));
   }

// Input scanned point cloud files. 
static const AIL_STRING SINGLE_MODEL   = M_IMAGE_PATH AIL_TEXT("SimpleModel.mbufc");
static const AIL_STRING SINGLE_SCENE   = M_IMAGE_PATH AIL_TEXT("SimpleScene.mbufc");
static const AIL_STRING COMPLEX_MODEL1 = M_IMAGE_PATH AIL_TEXT("ComplexModel1.ply");
static const AIL_STRING COMPLEX_MODEL2 = M_IMAGE_PATH AIL_TEXT("ComplexModel2.ply");
static const AIL_STRING COMPLEX_SCENE  = M_IMAGE_PATH AIL_TEXT("ComplexScene.ply");

// Constants. 
static const AIL_INT    DISP_SIZE_X = 480;
static const AIL_INT    DISP_SIZE_Y = 420;

// functions. 
void SimpleSceneSurfaceFinder (AIL_ID    AilSystem,
                               CDisplay& DisplayModel,
                               CDisplay& DisplayScene);
void ComplexSceneSurfaceFinder(AIL_ID    AilSystem,
                               CDisplay& DisplayModel,
                               CDisplay& DisplayScene);
void AddComponentNormalsIfMissing(AIL_ID AilContainer);
void ShowResults(AIL_ID AilResult, AIL_DOUBLE ComputationTime);
// -------------------------------------------------------------- 

int MosMain()
   {
   // Print example information in console. 
   PrintHeader();

   AIL_UNIQUE_APP_ID    AilApplication;    // application identifier  
   AIL_UNIQUE_SYS_ID    AilSystem;         // System identifier       

   // Allocate objects. 
   AilApplication = MappAlloc(M_NULL, M_DEFAULT, M_UNIQUE_ID);
   AilSystem      = MsysAlloc(M_DEFAULT, M_SYSTEM_DEFAULT, M_DEFAULT, M_DEFAULT, M_UNIQUE_ID);

   // Allocate the display.  
   CDisplay DisplayModel(AilSystem);
   DisplayModel.Alloc3dDisplayId();
   DisplayModel.Size(DISP_SIZE_X/2, DISP_SIZE_Y/2);
   DisplayModel.Title(AIL_TEXT("Model Cloud"));

   CDisplay DisplayScene(AilSystem);
   DisplayScene.Alloc3dDisplayId();
   DisplayScene.Size(DISP_SIZE_X, DISP_SIZE_Y);
   DisplayScene.PositionX((AIL_INT)(1.04 * 0.5*DISP_SIZE_X));
   DisplayScene.Title(AIL_TEXT("Scene Cloud"));

   SimpleSceneSurfaceFinder(AilSystem, DisplayModel, DisplayScene);

   ComplexSceneSurfaceFinder(AilSystem, DisplayModel, DisplayScene);

   DisplayModel.FreeDisplay();
   DisplayScene.FreeDisplay();
   return 0;
   }
// -------------------------------------------------------------- 
// Simple scene with a single occurrence.                         
// -------------------------------------------------------------- 
void SimpleSceneSurfaceFinder(AIL_ID AilSystem, CDisplay& DisplayModel, CDisplay& DisplayScene)
   {
   // Allocate a surface Model Finder context. 
   auto AilContext = M3dmodAlloc(AilSystem, M_FIND_SURFACE_CONTEXT, M_DEFAULT, M_UNIQUE_ID);

   // Allocate a surface Model Finder result. 
   auto AilResult = M3dmodAllocResult(AilSystem, M_FIND_SURFACE_RESULT,
                                      M_DEFAULT, M_UNIQUE_ID);

   // Restore the model container and display it 
   auto AilModelContainer = MbufRestore(SINGLE_MODEL, AilSystem, M_UNIQUE_ID);
   DisplayModel.SetView(M_AZIM_ELEV_ROLL, 45, -35, 180);
   DisplayModel.DisplayContainer(AilModelContainer, true);
   MosPrintf(AIL_TEXT("The 3D point cloud of the model is restored from a file and"
                      " displayed.\n"));

   // Load the single model scene point cloud. 
   auto AilSceneContainer = MbufRestore(SINGLE_SCENE, AilSystem, M_UNIQUE_ID);

   DisplayScene.SetView(M_AZIM_ELEV_ROLL, 202, -20.0, 182.0);
   DisplayScene.DisplayContainer(AilSceneContainer, true);

   MosPrintf(AIL_TEXT("The 3D point cloud of the scene is restored from a file and"
                      " displayed."));
   MosPrintf(AIL_TEXT("\n\nPress any key to start.\n\n"));
   MosGetch();

  
   // Define the surface model. 
   M3dmodDefine(AilContext, M_ADD_FROM_POINT_CLOUD, M_SURFACE, (AIL_DOUBLE)AilModelContainer,
                M_DEFAULT, M_DEFAULT,M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT);
   MosPrintf(AIL_TEXT("Define the model using the given model point cloud.\n\n"));

   // Set the search perseverance. 
   MosPrintf(AIL_TEXT("Set the lowest perseverance to increase the search speed for a simple"
                      " scene.\n\n"));
   M3dmodControl(AilContext, M_DEFAULT, M_PERSEVERANCE, 0.0);

   MosPrintf(AIL_TEXT("Set the scene complexity to low to increase the search speed for a "
                      "simple scene.\n\n"));
   M3dmodControl(AilContext, M_DEFAULT, M_SCENE_COMPLEXITY, M_LOW);
  
   // Preprocess the search context. 
   M3dmodPreprocess(AilContext, M_DEFAULT);

   MosPrintf(AIL_TEXT("M_COMPONENT_NORMALS_AIL is added to the point cloud if not"
                      " present.\n\n"));
   // The surface finder requires the existence of M_COMPONENT_NORMALS_AIL in the point cloud. 
   AddComponentNormalsIfMissing(AilSceneContainer);

   MosPrintf(AIL_TEXT("3D surface finder is running..\n\n"));

   // Reset the timer. 
   AIL_DOUBLE ComputationTime = 0.0;
   MappTimer(M_DEFAULT, M_TIMER_RESET + M_SYNCHRONOUS, M_NULL);

   // Find the model. 
   M3dmodFind(AilContext, AilSceneContainer, AilResult, M_DEFAULT);

   // Read the find time. 
   MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &ComputationTime);

   ShowResults(AilResult, ComputationTime);
   DisplayScene.Draw(AilResult);

   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();
   }
// -------------------------------------------------------------- 
// Complex scene with multiple occurrences.                       
// -------------------------------------------------------------- 
void ComplexSceneSurfaceFinder(AIL_ID    AilSystem,
                               CDisplay& DisplayModel,
                               CDisplay& DisplayScene)
   {
   // Allocate a surface 3D Model Finder context. 
   auto AilContext = M3dmodAlloc(AilSystem, M_FIND_SURFACE_CONTEXT, M_DEFAULT, M_UNIQUE_ID);

   // Allocate a surface 3D Model Finder result. 
   auto AilResult = M3dmodAllocResult(AilSystem, M_FIND_SURFACE_RESULT,
                                      M_DEFAULT, M_UNIQUE_ID);

   DisplayModel.Clear(M_ALL);
   DisplayScene.Clear(M_ALL);

   // Restore the first model container and display it. 
   auto AilModelContainer = MbufRestore(COMPLEX_MODEL1, AilSystem, M_UNIQUE_ID);
   DisplayModel.SetView(M_AZIM_ELEV_ROLL, 290, -67, 265);
   DisplayModel.DisplayContainer(AilModelContainer, false);
   DisplayModel.SetView(M_AUTO, M_DEFAULT, M_DEFAULT, M_DEFAULT);
   MosPrintf(AIL_TEXT("The 3D point cloud of the first model is restored from a file and"
                      " displayed.\n"));

   // Load the complex scene point cloud. 
   auto AilSceneContainer = MbufRestore(COMPLEX_SCENE, AilSystem, M_UNIQUE_ID);

   DisplayScene.SetView(M_AZIM_ELEV_ROLL, 260, -72, 142);
   DisplayScene.DisplayContainer(AilSceneContainer, false);
   DisplayScene.SetView(M_AUTO, M_DEFAULT, M_DEFAULT, M_DEFAULT);
   DisplayScene.SetView(M_ZOOM, 1.2, M_DEFAULT, M_DEFAULT);

   MosPrintf(AIL_TEXT("The 3D point cloud of the scene is restored from a file and"
                      " displayed.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to start.\n\n"));
   MosGetch();

   MosPrintf(AIL_TEXT("M_COMPONENT_NORMALS_AIL is added to the point cloud if"
                      " not present.\n\n"));
   // The surface finder requires the existence of M_COMPONENT_NORMALS_AIL
   // in the point cloud.
   AddComponentNormalsIfMissing(AilSceneContainer);
                                                                              
   // Define the surface model. 
   M3dmodDefine(AilContext, M_ADD_FROM_POINT_CLOUD, M_SURFACE, (AIL_DOUBLE)AilModelContainer,
                M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   // Find all ocurrences.  
   M3dmodControl(AilContext, 0, M_NUMBER, M_ALL);
   M3dmodControl(AilContext, 0, M_COVERAGE_MAX, 75);

   M3dmodPreprocess(AilContext, M_DEFAULT);
   MosPrintf(AIL_TEXT("3D surface finder is running..\n\n"));

   // Reset the timer. 
   AIL_DOUBLE ComputationTime = 0.0;
   MappTimer(M_DEFAULT, M_TIMER_RESET + M_SYNCHRONOUS, M_NULL);

   // Find the model. 
   M3dmodFind(AilContext, AilSceneContainer, AilResult, M_DEFAULT);

   // Read the find time. 
   MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &ComputationTime);

   ShowResults(AilResult, ComputationTime);
   AIL_INT64 Label = DisplayScene.Draw(AilResult);

   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   DisplayScene.Clear(Label);
  
   AilModelContainer = MbufRestore(COMPLEX_MODEL2, AilSystem, M_UNIQUE_ID);
   DisplayModel.DisplayContainer(AilModelContainer, false);
   DisplayModel.SetView(M_AUTO, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   MosPrintf(AIL_TEXT("The 3D point cloud of the second model is restored from file and"
                      " displayed.\n\n"));

   // Delete the previous model. 
   M3dmodDefine(AilContext, M_DELETE, M_DEFAULT, 0, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT,
                M_DEFAULT, M_DEFAULT);

   // Define the surface model. 
   M3dmodDefine(AilContext, M_ADD_FROM_POINT_CLOUD, M_SURFACE, (AIL_DOUBLE)AilModelContainer,
                M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   // Find all ocurrences.  
   M3dmodControl(AilContext, 0, M_NUMBER, M_ALL);
   M3dmodControl(AilContext, 0, M_COVERAGE_MAX, 95);

   M3dmodPreprocess(AilContext, M_DEFAULT);
   MosPrintf(AIL_TEXT("3D surface finder is running..\n\n"));

   // Reset the timer. 
   MappTimer(M_DEFAULT, M_TIMER_RESET + M_SYNCHRONOUS, M_NULL);

   // Find the model. 
   M3dmodFind(AilContext, AilSceneContainer, AilResult, M_DEFAULT);

   // Read the find time. 
   MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &ComputationTime);

   ShowResults(AilResult, ComputationTime);
   DisplayScene.Draw(AilResult);

   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();
   }
// -------------------------------------------------------------- 
// Adds the component M_COMPONENT_NORMALS_AIL if it's not found.  
// -------------------------------------------------------------- 
void AddComponentNormalsIfMissing(AIL_ID AilContainer)
   {
   AIL_ID AilNormals =
      MbufInquireContainer(AilContainer, M_COMPONENT_NORMALS_AIL, M_COMPONENT_ID, M_NULL);

   if(AilNormals != M_NULL)
      return;
   AIL_INT SizeX = MbufInquireContainer(AilContainer, M_COMPONENT_RANGE, M_SIZE_X, M_NULL);
   AIL_INT SizeY = MbufInquireContainer(AilContainer, M_COMPONENT_RANGE, M_SIZE_Y, M_NULL);
   if(SizeX < 50 || SizeY < 50)
      M3dimNormals(M_NORMALS_CONTEXT_TREE, AilContainer, AilContainer, M_DEFAULT);
   else
      M3dimNormals(M_NORMALS_CONTEXT_ORGANIZED, AilContainer, AilContainer, M_DEFAULT);
   }
// --------------------------------------------------------- 
// Shows the surface finder results.                         
// --------------------------------------------------------- 
void ShowResults(AIL_ID AilResult, AIL_DOUBLE ComputationTime)
   {
   AIL_INT Status;
   M3dmodGetResult(AilResult, M_DEFAULT, M_STATUS, &Status);

   if(Status != M_COMPLETE)
      {
      MosPrintf(AIL_TEXT("The find process is not completed.\n"));
      }

   AIL_INT NbOcc = 0;
   M3dmodGetResult(AilResult, M_DEFAULT, M_NUMBER, &NbOcc);
   MosPrintf(AIL_TEXT("Found %i occurrence(s) in %.2f s.\n\n"), NbOcc, ComputationTime);

   if(NbOcc == 0)
      return;

   MosPrintf(AIL_TEXT("Index        Score        Score_Target\n"));
   MosPrintf(AIL_TEXT("------------------------------------------------------\n"));

   for(AIL_INT i = 0; i < NbOcc; ++i)
      {
      AIL_DOUBLE ScoreTarget = M3dmodGetResult(AilResult, i, M_SCORE_TARGET, M_NULL);
      AIL_DOUBLE Score       = M3dmodGetResult(AilResult, i, M_SCORE       , M_NULL);
     

      MosPrintf(AIL_TEXT("  %i          %.4f      %6.2f          \n"),
                i, Score, ScoreTarget);
      }
   MosPrintf(AIL_TEXT("\n"));
   }


```
