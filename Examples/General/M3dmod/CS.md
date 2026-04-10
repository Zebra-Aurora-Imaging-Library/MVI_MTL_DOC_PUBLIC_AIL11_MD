---
title: "M3dmod"
description: "This example demonstrates how to use the 3D model finder module to define surface models and search for them in 3D scenes."
ms.language: "C#"
---

# M3dmod

> This example demonstrates how to use the 3D model finder module to define surface models and search for them in 3D scenes.

**Language:** C#

**Functions used:** `M3ddispAlloc`, `M3ddispControl`, `M3ddispCopy`, `M3ddispFree`, `M3ddispInquire`, `M3ddispSelect`, `M3ddispSetView`, `M3dgraControl`, `M3dgraCopy`, `M3dgraRemove`, `M3dmodAlloc`, `M3dmodAllocResult`, `M3dmodControl`, `M3dmodDefine`, `M3dmodDraw3d`, `M3dmodFind`, `M3dmodFree`, `M3dmodGetResult`, `M3dmodPreprocess`, `MappAlloc`, `MappControl`, `MappFree`, `MappTimer`, `MbufFree`, `MbufInquireContainer`, `MbufRestore`, `MobjInquire`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Automation, Applications, Finding/Locating, Identifying, Modules, 3D Display, 3D Graphics, 3D Model Finder, Buffer, What's New, AIL 10.0 SP6

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    M3dmod.cs
//
// Description: This example demonstrates how to use the 3D model finder module
//              to define surface models and search for them in 3D scenes.
//              A simple single model search is presented first followed by a more
//              complex example of multiple occurrences in a complex scene.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////


using System;
using Zebra.AuroraImagingLibrary;

namespace M3dmod
{
    class Program
    {
        // -------------------------------------------------------------- 
        // Example description.                                           
        // -------------------------------------------------------------- 
        private static void PrintHeader()
        {
            Console.WriteLine("[EXAMPLE NAME]");
            Console.WriteLine("M3dmod");
            Console.WriteLine();
            Console.WriteLine("[SYNOPSIS]");
            Console.WriteLine("This example demonstrates how to use the 3D model finder module");
            Console.WriteLine("to define surface models and search for them in 3D scenes.");
            Console.WriteLine();

            Console.WriteLine("[MODULES USED]");
            Console.WriteLine("Modules used: 3D Model Finder, 3D Display, 3D Graphics, and 3D"+
                              " Image\nProcessing.");
            Console.WriteLine();
        }

        // Input scanned point cloud files. 
        const string SINGLE_MODEL   = AIL.M_IMAGE_PATH + "SimpleModel.mbufc";
        const string SINGLE_SCENE   = AIL.M_IMAGE_PATH + "SimpleScene.mbufc";
        const string COMPLEX_MODEL1 = AIL.M_IMAGE_PATH + "ComplexModel1.ply";
        const string COMPLEX_MODEL2 = AIL.M_IMAGE_PATH + "ComplexModel2.ply";
        const string COMPLEX_SCENE  = AIL.M_IMAGE_PATH + "ComplexScene.ply";

        // Constants. 
        private static readonly AIL_INT    DISP_SIZE_X = 480;
        private static readonly AIL_INT    DISP_SIZE_Y = 420;

        // --------------------------------------------------------------
        private static void Main(string[] args)
        {
            // Print example information in console. 
            PrintHeader();

            AIL_ID AilApplication = AIL.M_NULL;   // Application identifier
            AIL_ID AilSystem      = AIL.M_NULL;   // System identifier     

            // Allocate objects. 
            AIL.MappAlloc(AIL.M_NULL, AIL.M_DEFAULT, ref AilApplication);
            AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT,
                          ref AilSystem);

            // Allocate the display. 
            CDisplay DisplayModel = new CDisplay(AilSystem);
            DisplayModel.Alloc3dDisplayId();
            DisplayModel.Size(DISP_SIZE_X / 2, DISP_SIZE_Y / 2);
            DisplayModel.Title("Model Cloud");

            CDisplay DisplayScene = new CDisplay(AilSystem);
            DisplayScene.Alloc3dDisplayId();
            DisplayScene.Size(DISP_SIZE_X, DISP_SIZE_Y);
            DisplayScene.PositionX((AIL_INT)(1.04 * 0.5 * DISP_SIZE_X));
            DisplayScene.Title("Scene Cloud");

            SimpleSceneSurfaceFinder(AilSystem, ref DisplayModel,ref DisplayScene);

            ComplexSceneSurfaceFinder(AilSystem, ref DisplayModel, ref DisplayScene);

            // Free objects. 
            DisplayModel.FreeDisplay();
            DisplayScene.FreeDisplay();
            AIL.MsysFree(AilSystem);
            AIL.MappFree(AilApplication);
            }
        // -------------------------------------------------------------- 
        // Simple scene with a single occurrence                          
        // -------------------------------------------------------------- 
        static void SimpleSceneSurfaceFinder(AIL_ID       AilSystem   ,
                                             ref CDisplay DisplayModel,
                                             ref CDisplay DisplayScene)
            {
            // Allocate a surface Model Finder context. 
            AIL_ID AilContext = AIL.M3dmodAlloc(AilSystem, AIL.M_FIND_SURFACE_CONTEXT,
                                                AIL.M_DEFAULT, AIL.M_NULL);

            // Allocate a surface Model Finder result. 
            AIL_ID AilResult = AIL.M3dmodAllocResult(AilSystem, AIL.M_FIND_SURFACE_RESULT,
                                                     AIL.M_DEFAULT, AIL.M_NULL);

            // Restore the model container and display it 
            AIL_ID AilModelContainer = AIL.MbufRestore(SINGLE_MODEL, AilSystem, AIL.M_NULL);
            DisplayModel.SetView(AIL.M_AZIM_ELEV_ROLL, 45, -35, 180);
            DisplayModel.DisplayContainer(AilModelContainer, true);
            Console.WriteLine("The 3D point cloud of the model is restored from a file and "+
                               "displayed.");

            // Load the single model scene point cloud. 
            AIL_ID AilSceneContainer = AIL.MbufRestore(SINGLE_SCENE, AilSystem, AIL.M_NULL);

            DisplayScene.SetView(AIL.M_AZIM_ELEV_ROLL, 202, -20.0, 182.0);
            DisplayScene.DisplayContainer(AilSceneContainer, true);

            Console.WriteLine("The 3D point cloud of the scene is restored from a file and"+
                              " displayed.\n");
            Console.WriteLine("Press any key to start.\n");
            Console.ReadKey(true);


            // Define the surface model. 
            AIL.M3dmodDefine(AilContext, AIL.M_ADD_FROM_POINT_CLOUD, AIL.M_SURFACE,
                             AilModelContainer, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT,
                             AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
            Console.WriteLine("Define the model using the given model point cloud.\n");

            // Set the search perseverance. 
            Console.WriteLine("Set the lowest perseverance to increase the search speed for" +
                              " a simple scene.\n");
            AIL.M3dmodControl(AilContext, AIL.M_DEFAULT, AIL.M_PERSEVERANCE, 0.0);

            Console.WriteLine("Set the scene complexity to low to increase the search speed " +
                              "for a simple scene.\n");
            AIL.M3dmodControl(AilContext, AIL.M_DEFAULT, AIL.M_SCENE_COMPLEXITY, AIL.M_LOW);

            // Preprocess the search context. 
            AIL.M3dmodPreprocess(AilContext, AIL.M_DEFAULT);

            Console.WriteLine("M_COMPONENT_NORMALS_AIL is added to the point cloud if not " +
                              "present.\n");
            // The surface finder requires the existence of M_COMPONENT_NORMALS_AIL in the 
            // point cloud. 
            AddComponentNormalsIfMissing(AilSceneContainer);

            Console.WriteLine("3D surface finder is running..\n");

            // Reset the timer. 
            double ComputationTime = 0.0;
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            // Find the model. 
            AIL.M3dmodFind(AilContext, AilSceneContainer, AilResult, AIL.M_DEFAULT);

            // Read the find time. 
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS,
                          ref ComputationTime);

            ShowResults(AilResult, ComputationTime);
            DisplayScene.Draw(AilResult);

            Console.WriteLine("Press any key to continue.\n");
            Console.ReadKey(true);

            // Free objects
            AIL.M3dmodFree(AilContext);
            AIL.M3dmodFree(AilResult);
            AIL.MbufFree(AilModelContainer);
            AIL.MbufFree(AilSceneContainer);
            }
        // -------------------------------------------------------------- 
        // Complex scene with multiple occurrences                        
        // -------------------------------------------------------------- 
        static void ComplexSceneSurfaceFinder(AIL_ID       AilSystem   ,
                                              ref CDisplay DisplayModel,
                                              ref CDisplay DisplayScene)
            {
            // Allocate a surface 3D Model Finder context. 
            AIL_ID AilContext = AIL.M3dmodAlloc(AilSystem, AIL.M_FIND_SURFACE_CONTEXT,
                                                AIL.M_DEFAULT, AIL.M_NULL);

            // Allocate a surface 3D Model Finder result. 
            AIL_ID AilResult = AIL.M3dmodAllocResult(AilSystem, AIL.M_FIND_SURFACE_RESULT,
                                                     AIL.M_DEFAULT, AIL.M_NULL);

            DisplayModel.Clear(AIL.M_ALL);
            DisplayScene.Clear(AIL.M_ALL);

            // Restore the first model container and display it. 
            AIL_ID AilModelContainer = AIL.MbufRestore(COMPLEX_MODEL1, AilSystem,
                                                       AIL.M_NULL);
            DisplayModel.SetView(AIL.M_AZIM_ELEV_ROLL, 290, -67, 265);
            DisplayModel.DisplayContainer(AilModelContainer, false);
            DisplayModel.SetView(AIL.M_AUTO, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
            Console.WriteLine("The 3D point cloud of the first model is restored from a file" +
                " and displayed.");

            // Load the complex scene point cloud. 
            AIL_ID AilSceneContainer = AIL.MbufRestore(COMPLEX_SCENE, AilSystem, AIL.M_NULL);

      
            DisplayScene.SetView(AIL.M_AZIM_ELEV_ROLL, 260, -72, 142);
            DisplayScene.DisplayContainer(AilSceneContainer, false);
            DisplayScene.SetView(AIL.M_AUTO, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
            DisplayScene.SetView(AIL.M_ZOOM, 1.2, AIL.M_DEFAULT, AIL.M_DEFAULT);

            Console.WriteLine("The 3D point cloud of the scene is restored from a file and" +
                              " displayed.\n");
            Console.WriteLine("Press any key to start.\n");
            Console.ReadKey(true);

            Console.WriteLine("M_COMPONENT_NORMALS_AIL is added to the point cloud if not " +
                              "present.\n");
            // The surface finder requires the existence of M_COMPONENT_NORMALS_AIL in the 
            // point cloud. 
            AddComponentNormalsIfMissing(AilSceneContainer);

            // Define the surface model. 
            AIL.M3dmodDefine(AilContext, AIL.M_ADD_FROM_POINT_CLOUD, AIL.M_SURFACE,
                             AilModelContainer, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT,
                             AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Find all ocurrences. 
            AIL.M3dmodControl(AilContext, 0, AIL.M_NUMBER, AIL.M_ALL);
            AIL.M3dmodControl(AilContext, 0, AIL.M_COVERAGE_MAX, 75);

            AIL.M3dmodPreprocess(AilContext, AIL.M_DEFAULT);
            Console.WriteLine("3D surface finder is running..\n");

            // Reset the timer. 
            double ComputationTime = 0.0;
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            // Find the model. 
            AIL.M3dmodFind(AilContext, AilSceneContainer, AilResult, AIL.M_DEFAULT);

            // Read the find time. 
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS,
                          ref ComputationTime);

            ShowResults(AilResult, ComputationTime);
            AIL_INT Label = DisplayScene.Draw(AilResult);

            Console.WriteLine("Press any key to continue.\n");
            Console.ReadKey(true);

            DisplayScene.Clear(Label);

            AIL.MbufFree(AilModelContainer);
            AilModelContainer = AIL.MbufRestore(COMPLEX_MODEL2, AilSystem, AIL.M_NULL);
            DisplayModel.DisplayContainer(AilModelContainer, false);
            DisplayModel.SetView(AIL.M_AUTO, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            Console.WriteLine("The 3D point cloud of the second model is restored from file" +
                              " and displayed.\n");

            // Delete the previous model. 
            AIL.M3dmodDefine(AilContext, AIL.M_DELETE, AIL.M_DEFAULT, 0, AIL.M_DEFAULT,
                             AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT,
                             AIL.M_DEFAULT);

            // Define the surface model. 
            AIL.M3dmodDefine(AilContext, AIL.M_ADD_FROM_POINT_CLOUD, AIL.M_SURFACE,
                             AilModelContainer, AIL.M_DEFAULT, AIL.M_DEFAULT,
                             AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Find all ocurrences. 
            AIL.M3dmodControl(AilContext, 0, AIL.M_NUMBER, AIL.M_ALL);
            AIL.M3dmodControl(AilContext, 0, AIL.M_COVERAGE_MAX, 95);

            AIL.M3dmodPreprocess(AilContext, AIL.M_DEFAULT);
            Console.WriteLine("3D surface finder is running..\n");

            // Reset the timer. 
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            // Find the model. 
            AIL.M3dmodFind(AilContext, AilSceneContainer, AilResult, AIL.M_DEFAULT);

            // Read the find time. 
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS,
                          ref ComputationTime);

            ShowResults(AilResult, ComputationTime);
            DisplayScene.Draw(AilResult);

            Console.WriteLine("Press any key to end.\n");
            Console.ReadKey(true);

            // Free objects
            AIL.M3dmodFree(AilContext);
            AIL.M3dmodFree(AilResult);
            AIL.MbufFree(AilModelContainer);
            AIL.MbufFree(AilSceneContainer);
            }
        // -------------------------------------------------------------- 
        // Adds the component M_COMPONENT_NORMALS_AIL if it's not found.  
        // -------------------------------------------------------------- 
        static void AddComponentNormalsIfMissing(AIL_ID AilContainer)
            {
            AIL_ID AilNormals =
            (AIL_ID)AIL.MbufInquireContainer(AilContainer, AIL.M_COMPONENT_NORMALS_AIL,
                                             AIL.M_COMPONENT_ID, AIL.M_NULL);

            if (AilNormals != AIL.M_NULL)
                return;
            AIL_INT SizeX = AIL.MbufInquireContainer(AilContainer, AIL.M_COMPONENT_RANGE,
                                                     AIL.M_SIZE_X, AIL.M_NULL);
            AIL_INT SizeY = AIL.MbufInquireContainer(AilContainer, AIL.M_COMPONENT_RANGE,
                                                     AIL.M_SIZE_Y, AIL.M_NULL);
            if (SizeX < 50 || SizeY < 50)
                AIL.M3dimNormals(AIL.M_NORMALS_CONTEXT_TREE, AilContainer, AilContainer,
                                 AIL.M_DEFAULT);
            else
                AIL.M3dimNormals(AIL.M_NORMALS_CONTEXT_ORGANIZED, AilContainer, AilContainer,
                                 AIL.M_DEFAULT);
            }
        // --------------------------------------------------------- 
        // Shows the surface finder results.                         
        // --------------------------------------------------------- 
        static void ShowResults(AIL_ID AilResult, double ComputationTime)
            {
            AIL_INT Status = 0 ;
            AIL.M3dmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_STATUS,  ref Status);
            if (Status != AIL.M_COMPLETE)
                {
                Console.WriteLine("The find process is not completed.");
                }

            AIL_INT NbOcc = 0;
            AIL.M3dmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_NUMBER,  ref NbOcc);
            Console.WriteLine("Found {0} occurrence(s) in "+String.Format("{0:F2}",
                              ComputationTime)+" s.\n", NbOcc);

            if (NbOcc == 0)
                return;

            Console.WriteLine("Index        Score        Score_Target");
            Console.WriteLine("------------------------------------------------------");

            double ScoreTarget = 0.0;
            double Score       = 0.0;
            for (AIL_INT i = 0; i < NbOcc; ++i)
                {
                AIL.M3dmodGetResult(AilResult, i, AIL.M_SCORE_TARGET, ref ScoreTarget);
                AIL.M3dmodGetResult(AilResult, i, AIL.M_SCORE       , ref Score);


                Console.WriteLine("  {0}          "+ String.Format("{0:F4}", Score) + "       "
                                  + String.Format("{0:F2}", ScoreTarget) +"          ", i);
                }
            Console.WriteLine();
            }
    }
}

```
