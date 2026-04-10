---
title: "M3dblob"
description: "This program demonstrates how to use the 3d segmentation module to identify objects in a scene and separate them into categories."
ms.language: "C#"
---

# M3dblob

> This program demonstrates how to use the 3d segmentation module to identify objects in a scene and separate them into categories.

**Language:** C#

**Functions used:** `M3dblobAlloc`, `M3dblobAllocResult`, `M3dblobCalculate`, `M3dblobCombine`, `M3dblobControl`, `M3dblobDraw3d`, `M3dblobFree`, `M3dblobGetResult`, `M3dblobSegment`, `M3dblobSelect`, `M3dgeoAlloc`, `M3dgeoFree`, `M3dgeoMatrixGetTransform`, `M3ddispAlloc`, `M3ddispFree`, `M3ddispInquire`, `M3ddispSelect`, `M3dgraAlloc`, `M3dgraControl`, `M3dgraCopy`, `M3dgraFree`, `M3dgraInquire`, `M3dgraRemove`, `M3dimCalibrateDepthMap`, `M3dimProject`, `MappAlloc`, `MappControl`, `MappFree`, `MappHookFunction`, `MbufAlloc2d`, `MbufAllocColor`, `MbufControl`, `MbufFree`, `MbufImport`, `MbufInquireContainer`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `M3ddispCopy`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MdispZoom`, `MgenLutFunction`, `MgraAllocList`, `MgraControl`, `MgraControlList`, `MgraDots`, `MgraFree`, `MgraRectAngle`, `MobjInquire`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Automation, Applications, Finding/Locating, Counting, 3D profiling, Modules, 3D Blob Analysis, 3D Geometry, 3D Image Processing, 3D Display, 3D Graphics, Display, Graphics, Buffer, What's New, AIL 10.0 SP6, AIL 10.0 SP5

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    M3dblob.cs
//
// Description: This program demonstrates how to use the 3d blob module to
//              identify objects in a scene and separate them into categories.
//              See the PrintHeader() function below for a detailed description.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using Zebra.AuroraImagingLibrary;

namespace M3dblob
{
    class Program
    {
        // Source file specification.
        private static readonly string  CONNECTORS_AND_WASHERS_FILE = AIL.M_IMAGE_PATH + "ConnectorsAndWashers.mbufc";
        private static readonly string  CONNECTORS_AND_WASHERS_ILLUSTRATION_FILE = AIL.M_IMAGE_PATH + "ConnectorsAndWashers.png";

        private static readonly string  TWISTY_PUZZLES_FILE = AIL.M_IMAGE_PATH + "TwistyPuzzles.mbufc";

        // Segmentation thresholds.
        private static readonly AIL_INT LOCAL_SEGMENTATION_MIN_NB_POINTS = 100;
        private static readonly AIL_INT LOCAL_SEGMENTATION_MAX_NB_POINTS = 10000;
        private static readonly double  LOCAL_SEGMENTATION_DISTANCE_THRESHOLD = 0.75; // in mm

        private static readonly AIL_INT PLANAR_SEGMENTATION_MIN_NB_POINTS = 5000;
        private static readonly double  PLANAR_SEGMENTATION_NORMAL_THRESHOLD = 20;    // in deg

        // --------------------------------------------------------------
        private static void Main(string[] args)
        {
            Console.WriteLine("[EXAMPLE NAME]");
            Console.WriteLine("M3dblob");
            Console.WriteLine();
            Console.WriteLine("[SYNOPSIS]");
            Console.WriteLine("This program demonstrates how to use the 3d blob analysis module to");
            Console.WriteLine("identify objects in a scene and separate them into categories.");
            Console.WriteLine();

            Console.WriteLine("[MODULES USED]");
            Console.WriteLine("Modules used: 3D Blob Analysis, 3D Image Processing,");
            Console.WriteLine("3D Display, Display, Buffer, and 3D Graphics.");
            Console.WriteLine();

            // Allocate defaults.
            AIL_ID AilApplication = AIL.M_NULL;
            AIL_ID AilSystem = AIL.M_NULL;
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL);

            // Allocate the displays.
            AIL_ID SceneDisplay = Alloc3dDisplayId(AilSystem);
            if (SceneDisplay == AIL.M_NULL)
                {
                AIL.MappFreeDefault(AilApplication, AilSystem, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL);
                return;
                }
            AIL.M3ddispControl(SceneDisplay, AIL.M_TITLE, "Scene");

            AIL_ID IllustrationDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_WINDOWED, AIL.M_NULL);
            AIL_INT IllustrationOffsetX = AIL.M3ddispInquire(SceneDisplay, AIL.M_SIZE_X);
            AIL.MdispControl(IllustrationDisplay, AIL.M_TITLE, "Objects to inspect");
            AIL.MdispControl(IllustrationDisplay, AIL.M_WINDOW_INITIAL_POSITION_X, IllustrationOffsetX);

            // Run the examples.
            IdentificationAndSortingExample(SceneDisplay, IllustrationDisplay);
            PlanarSegmentationExample(SceneDisplay, IllustrationDisplay);

            AIL.MdispFree(IllustrationDisplay);
            AIL.M3ddispFree(SceneDisplay);

            AIL.MappFreeDefault(AilApplication, AilSystem, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL);

            return;
        }

        //*****************************************************************************
        // Allocates a 3D or 2D display depending on which one is supported.
        //*****************************************************************************
        private static AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
        {
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);
            AIL_ID AilDisplay = AIL.M3ddispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, AIL.M_NULL);
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE);

           if (AilDisplay == AIL.M_NULL)
                {
                Console.WriteLine("The current system does not support the 3D display.");
                Console.WriteLine("Press any key to exit.");
                Console.ReadKey(true);
                }

            return AilDisplay;
        }

        //*****************************************************************************
        // First example.
        //*****************************************************************************
        private static void IdentificationAndSortingExample(AIL_ID SceneDisplay, AIL_ID IllustrationDisplay)
            {
            AIL_ID AilSystem = (AIL_ID)AIL.MobjInquire(SceneDisplay, AIL.M_OWNER_SYSTEM, AIL.M_NULL);
            AIL_ID SceneGraphicList = AIL.M3ddispInquire(SceneDisplay, AIL.M_3D_GRAPHIC_LIST_ID, AIL.M_NULL);

            // Restore the point cloud and display it.
            AIL_ID AilPointCloud = AIL.MbufImport(CONNECTORS_AND_WASHERS_FILE, AIL.M_DEFAULT, AIL.M_RESTORE, AilSystem, AIL.M_NULL);

            AIL.M3dgraRemove(SceneGraphicList, AIL.M_ALL, AIL.M_DEFAULT);
            AIL.M3dgraControl(SceneGraphicList, AIL.M_DEFAULT_SETTINGS, AIL.M_THICKNESS, 3);
            AIL.M3dgraControl(SceneGraphicList, AIL.M_DEFAULT_SETTINGS, AIL.M_FONT_SIZE, 4);

            AIL.M3ddispSelect(SceneDisplay, AilPointCloud, AIL.M_DEFAULT, AIL.M_DEFAULT);
            AIL.M3ddispSetView(SceneDisplay, AIL.M_AUTO, AIL.M_TOP_TILTED, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Set the display's rotation axis center. This will keep the
            // behaviour of auto rotate consistent as we move its interest point.
            AIL.M3ddispCopy(AIL.M_VIEW_INTEREST_POINT, SceneDisplay, AIL.M_ROTATION_AXIS_CENTER, AIL.M_DEFAULT);

            // Show an illustration of the objects in the scene.
            AIL_ID IllustrationImage = AIL.MbufRestore(CONNECTORS_AND_WASHERS_ILLUSTRATION_FILE, AilSystem, AIL.M_NULL);
            AIL.MdispSelect(IllustrationDisplay, IllustrationImage);

            Console.WriteLine("A 3D point cloud consisting of wire connectors and washers");
            Console.WriteLine("is restored from a file and displayed.");
            Console.WriteLine();
            Console.WriteLine("Press any key to segment it into separate objects.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Allocate the segmentation contexts.
            AIL_ID SegmentationContext = AIL.M3dblobAlloc(AilSystem, AIL.M_SEGMENTATION_CONTEXT, AIL.M_DEFAULT, AIL.M_NULL);
            AIL_ID CalculateContext = AIL.M3dblobAlloc(AilSystem, AIL.M_CALCULATE_CONTEXT, AIL.M_DEFAULT, AIL.M_NULL);
            AIL_ID Draw3dContext = AIL.M3dblobAlloc(AilSystem, AIL.M_DRAW_3D_CONTEXT, AIL.M_DEFAULT, AIL.M_NULL);

            // Allocate the segmentation results. One result is used for each category.
            AIL_ID AllBlobs = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT, AIL.M_NULL);
            AIL_ID Connectors = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT, AIL.M_NULL);
            AIL_ID Washers = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT, AIL.M_NULL);
            AIL_ID UnknownBlobs = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT, AIL.M_NULL);

            // Segment the point cloud into several blobs.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NEIGHBOR_SEARCH_MODE, AIL.M_ORGANIZED);                  // Take advantage of the 2d organization.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NEIGHBORHOOD_ORGANIZED_SIZE, 5);                         // Look for neighbors in a 5x5 square kernel.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NUMBER_OF_POINTS_MIN, LOCAL_SEGMENTATION_MIN_NB_POINTS); // Exclude small isolated clusters.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NUMBER_OF_POINTS_MAX, LOCAL_SEGMENTATION_MAX_NB_POINTS); // Exclude extremely large clusters which make up the background.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_MAX_DISTANCE, LOCAL_SEGMENTATION_DISTANCE_THRESHOLD);    // Set the distance between points to be blobbed together.

            AIL.M3dblobSegment(SegmentationContext, AilPointCloud, AllBlobs, AIL.M_DEFAULT);

            // Draw all blobs in the 3d display.
            AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_LABEL, AIL.M_ACTIVE, AIL.M_ENABLE);
            AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_LABEL, AIL.M_FONT_SIZE, AIL.M_GRAPHIC_LIST_DEFAULT);
            AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_ACTIVE, AIL.M_ENABLE);
            AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_THICKNESS, AIL.M_GRAPHIC_LIST_DEFAULT);
            AIL_INT AllBlobsLabel = AIL.M3dblobDraw3d(Draw3dContext, AilPointCloud, AllBlobs, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT);

            Console.WriteLine("The point cloud is segmented based on the distance between points.");
            Console.WriteLine("Points belonging to the background plane or small isolated clusters");
            Console.WriteLine("are excluded.");
            Console.WriteLine();
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Calculate features on the blobs and use them to determine the type of object they represent.
            AIL.M3dblobControl(CalculateContext, AIL.M_DEFAULT, AIL.M_PCA_BOX, AIL.M_ENABLE);
            AIL.M3dblobControl(CalculateContext, AIL.M_DEFAULT, AIL.M_LINEARITY, AIL.M_ENABLE);
            AIL.M3dblobControl(CalculateContext, AIL.M_DEFAULT, AIL.M_PLANARITY, AIL.M_ENABLE);

            AIL.M3dblobCalculate(CalculateContext, AilPointCloud, AllBlobs, AIL.M_ALL, AIL.M_DEFAULT);

            // Connectors are more elongated than other blobs.
            // Use the feature M_LINEARITY, which is a value from 0 (perfect sphere/plane) to 1 (perfect line).
            AIL.M3dblobSelect(AllBlobs, Connectors, AIL.M_LINEARITY, AIL.M_GREATER, 0.5, AIL.M_NULL, AIL.M_DEFAULT);

            // Washers are flat and circular.
            // Use the feature M_PLANARITY, which is a value from 0 (perfect sphere) to 1 (perfect plane).
            AIL.M3dblobSelect(AllBlobs, Washers, AIL.M_LINEARITY, AIL.M_LESS, 0.2, AIL.M_NULL, AIL.M_DEFAULT);
            AIL.M3dblobSelect(Washers, Washers, AIL.M_PLANARITY, AIL.M_GREATER, 0.8, AIL.M_NULL, AIL.M_DEFAULT);

            // Blobs that are neither connectors nor washers are unknown objects.
            // Use M3dblobCombine to subtract already identified blobs from AllBlobs.
            AIL.M3dblobCombine(AllBlobs, Connectors, UnknownBlobs, AIL.M_SUB, AIL.M_DEFAULT);
            AIL.M3dblobCombine(UnknownBlobs, Washers, UnknownBlobs, AIL.M_SUB, AIL.M_DEFAULT);

            // Print the number of blobs in each category.
            AIL_INT NbConnectors = 0, NbWashers = 0, NbUnknown = 0;
            AIL.M3dblobGetResult(Connectors,   AIL.M_DEFAULT, AIL.M_NUMBER, ref NbConnectors);
            AIL.M3dblobGetResult(Washers,      AIL.M_DEFAULT, AIL.M_NUMBER, ref NbWashers);
            AIL.M3dblobGetResult(UnknownBlobs, AIL.M_DEFAULT, AIL.M_NUMBER, ref NbUnknown);

            Console.WriteLine("Simple 3D features (planarity, linearity) are calculated on the");
            Console.WriteLine("blobs and used to identify them.");
            Console.WriteLine();
            Console.WriteLine("The relevant objects (connectors and washers) have their");
            Console.WriteLine("bounding box displayed.");
            Console.WriteLine("Connectors (in red):     {0}", NbConnectors);
            Console.WriteLine("Washers (in green) :     {0}", NbWashers);
            Console.WriteLine("Unknown (in yellow):     {0}", NbUnknown);
            Console.WriteLine();

            // Draw the blobs in the 3d display.
            AIL.M3dgraRemove(SceneGraphicList, AllBlobsLabel, AIL.M_DEFAULT);

            AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_COLOR, AIL.M_COLOR_YELLOW);
            AIL.M3dblobDraw3d(Draw3dContext, AilPointCloud, UnknownBlobs, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT);

            AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_PCA_BOX, AIL.M_ACTIVE, AIL.M_ENABLE);
            AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_COLOR, AIL.M_COLOR_RED);
            AIL.M3dblobDraw3d(Draw3dContext, AilPointCloud, Connectors, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT);

            AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_COLOR, AIL.M_COLOR_GREEN);
            AIL.M3dblobDraw3d(Draw3dContext, AilPointCloud, Washers, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT);

            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            AIL.M3dblobFree(UnknownBlobs);
            AIL.M3dblobFree(Washers);
            AIL.M3dblobFree(Connectors);
            AIL.M3dblobFree(AllBlobs);
            AIL.M3dblobFree(Draw3dContext);
            AIL.M3dblobFree(CalculateContext);
            AIL.M3dblobFree(SegmentationContext);

            AIL.MbufFree(IllustrationImage);
            AIL.MbufFree(AilPointCloud);

            }

        //*****************************************************************************
        // Second example.
        //*****************************************************************************
        private static void PlanarSegmentationExample(AIL_ID SceneDisplay, AIL_ID IllustrationDisplay)
            {
            AIL_ID AilSystem = (AIL_ID)AIL.MobjInquire(SceneDisplay, AIL.M_OWNER_SYSTEM, AIL.M_NULL);
            AIL_ID SceneGraphicList = AIL.M3ddispInquire(SceneDisplay, AIL.M_3D_GRAPHIC_LIST_ID, AIL.M_NULL);

            // Restore the point cloud and display it.
            AIL_ID AilPointCloud = AIL.MbufImport(TWISTY_PUZZLES_FILE, AIL.M_DEFAULT, AIL.M_RESTORE, AilSystem, AIL.M_NULL);

            AIL.M3dgraRemove(SceneGraphicList, AIL.M_ALL, AIL.M_DEFAULT);
            AIL.M3dgraControl(SceneGraphicList, AIL.M_DEFAULT_SETTINGS, AIL.M_THICKNESS, 1);

            AIL.M3ddispSelect(SceneDisplay, AilPointCloud, AIL.M_DEFAULT, AIL.M_DEFAULT);
            AIL.M3ddispSetView(SceneDisplay, AIL.M_AUTO, AIL.M_TOP_TILTED, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Set the display's rotation axis center. This will keep the
            // behaviour of auto rotate consistent as we move its interest point.
            AIL.M3ddispCopy(AIL.M_VIEW_INTEREST_POINT, SceneDisplay, AIL.M_ROTATION_AXIS_CENTER, AIL.M_DEFAULT);

            Console.WriteLine("Another point cloud containing various twisty puzzles is restored.");
            Console.WriteLine();
            Console.WriteLine("Press any key to segment it into separate objects.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Allocate the segmentation objects.
            AIL_ID SegmentationContext = AIL.M3dblobAlloc(AilSystem, AIL.M_SEGMENTATION_CONTEXT, AIL.M_DEFAULT, AIL.M_NULL);
            AIL_ID SegmentationResult = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT, AIL.M_NULL);

            // First segment the point cloud with only local thresholds.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NEIGHBOR_SEARCH_MODE, AIL.M_ORGANIZED);                    // Take advantage of the 2d organization.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NEIGHBORHOOD_ORGANIZED_SIZE, 5);                           // Look for neighbors in a 5x5 square kernel.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NUMBER_OF_POINTS_MIN, PLANAR_SEGMENTATION_MIN_NB_POINTS);  // Exclude small isolated clusters.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_MAX_DISTANCE_MODE, AIL.M_AUTO);                            // Use an automatic local distance threshold.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NORMAL_DISTANCE_MAX_MODE, AIL.M_AUTO);                     // Use an automatic local normal threshold.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NORMAL_DISTANCE_MODE, AIL.M_ORIENTATION);                  // Consider flipped normals to be the same.

            AIL.M3dblobSegment(SegmentationContext, AilPointCloud, SegmentationResult, AIL.M_DEFAULT);

            AIL_INT AnnotationLabel = AIL.M3dblobDraw3d(AIL.M_DEFAULT, AilPointCloud, SegmentationResult, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT);

            Console.WriteLine("The point cloud is segmented based on local thresholds (distance, normals).");
            Console.WriteLine();
            Console.WriteLine("Local thresholds can separate distinct objects due to camera occlusions,");
            Console.WriteLine("but are often not enough to segment a single object into subparts.");
            Console.WriteLine();
            Console.WriteLine("Press any key to use global thresholds instead.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Then segment again with global thresholds.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NORMAL_DISTANCE_MAX_MODE, AIL.M_USER_DEFINED);                      // Remove the local normal threshold.
            AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_GLOBAL_NORMAL_DISTANCE_MAX, PLANAR_SEGMENTATION_NORMAL_THRESHOLD);  // Use a global normal threshold instead.

            AIL.M3dblobSegment(SegmentationContext, AilPointCloud, SegmentationResult, AIL.M_DEFAULT);

            AIL.M3dgraRemove(SceneGraphicList, AnnotationLabel, AIL.M_DEFAULT);
            AnnotationLabel = AIL.M3dblobDraw3d(AIL.M_DEFAULT, AilPointCloud, SegmentationResult, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT);

            Console.WriteLine("The puzzles' sides are now separated.");
            Console.WriteLine();
            Console.WriteLine("Press any key to end.");
            Console.WriteLine();
            Console.ReadKey(true);

            AIL.M3dblobFree(SegmentationContext);
            AIL.M3dblobFree(SegmentationResult);

            AIL.MbufFree(AilPointCloud);
            }
        }
    }

```
