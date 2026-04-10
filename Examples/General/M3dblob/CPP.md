---
title: "M3dblob"
description: "This program demonstrates how to use the 3d segmentation module to identify objects in a scene and separate them into categories."
ms.language: "C++"
---

# M3dblob

> This program demonstrates how to use the 3d segmentation module to identify objects in a scene and separate them into categories.

**Language:** C++

**Functions used:** `M3dblobAlloc`, `M3dblobAllocResult`, `M3dblobCalculate`, `M3dblobCombine`, `M3dblobControl`, `M3dblobDraw3d`, `M3dblobFree`, `M3dblobGetResult`, `M3dblobSegment`, `M3dblobSelect`, `M3dgeoAlloc`, `M3dgeoFree`, `M3dgeoMatrixGetTransform`, `M3ddispAlloc`, `M3ddispFree`, `M3ddispInquire`, `M3ddispSelect`, `M3dgraAlloc`, `M3dgraControl`, `M3dgraCopy`, `M3dgraFree`, `M3dgraInquire`, `M3dgraRemove`, `M3dimCalibrateDepthMap`, `M3dimProject`, `MappAlloc`, `MappControl`, `MappFree`, `MappHookFunction`, `MbufAlloc2d`, `MbufAllocColor`, `MbufControl`, `MbufFree`, `MbufImport`, `MbufInquireContainer`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `M3ddispCopy`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MdispZoom`, `MgenLutFunction`, `MgraAllocList`, `MgraControl`, `MgraControlList`, `MgraDots`, `MgraFree`, `MgraRectAngle`, `MobjInquire`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Automation, Applications, Finding/Locating, Counting, 3D profiling, Modules, 3D Blob Analysis, 3D Geometry, 3D Image Processing, 3D Display, 3D Graphics, Display, Graphics, Buffer, What's New, AIL 10.0 SP6, AIL 10.0 SP5

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    M3dblob.cpp
//
// Description: This program demonstrates how to use the 3d blob module to
//              identify objects in a scene and separate them into categories.
//              See the PrintHeader() function below for a detailed description.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>
#include <cstdlib>

// Source file specification.
static const AIL_STRING CONNECTORS_AND_WASHERS_FILE =
   M_IMAGE_PATH AIL_TEXT("ConnectorsAndWashers.mbufc");
static const AIL_STRING CONNECTORS_AND_WASHERS_ILLUSTRATION_FILE =
   M_IMAGE_PATH AIL_TEXT("ConnectorsAndWashers.png");

static const AIL_STRING TWISTY_PUZZLES_FILE = M_IMAGE_PATH AIL_TEXT("TwistyPuzzles.mbufc");

// Segmentation thresholds.
static const AIL_INT    LOCAL_SEGMENTATION_MIN_NB_POINTS = 100;
static const AIL_INT    LOCAL_SEGMENTATION_MAX_NB_POINTS = 10000;
static const AIL_DOUBLE LOCAL_SEGMENTATION_DISTANCE_THRESHOLD = 0.75;   // in mm

static const AIL_INT    PLANAR_SEGMENTATION_MIN_NB_POINTS = 5000;
static const AIL_DOUBLE PLANAR_SEGMENTATION_NORMAL_THRESHOLD = 20;      // in deg

// Function declarations.
AIL_ID Alloc3dDisplayId(AIL_ID AilSystem);

void   IdentificationAndSortingExample(AIL_ID SceneDisplay, AIL_ID IllustrationDisplay);
void   PlanarSegmentationExample(AIL_ID SceneDisplay, AIL_ID IllustrationDisplay);

//*****************************************************************************
// Main.
//*****************************************************************************
int MosMain(void)
   {
   MosPrintf(AIL_TEXT("[EXAMPLE NAME]\n")
      AIL_TEXT("M3dblob\n\n")

      AIL_TEXT("[SYNOPSIS]\n")
      AIL_TEXT("This program demonstrates how to use the 3d blob analysis module to\n")
      AIL_TEXT("identify objects in a scene and separate them into categories.\n\n")

      AIL_TEXT("[MODULES USED]\n")
      AIL_TEXT("Modules used: 3D Blob Analysis, 3D Image Processing,\n")
      AIL_TEXT("3D Display, Display, Buffer, and 3D Graphics.\n\n"));

   AIL_ID AilApplication,  // Application identifier.
          AilSystem;       // System identifier.

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, M_NULL, M_NULL, M_NULL);

   // Allocate the displays.
   AIL_ID SceneDisplay = Alloc3dDisplayId(AilSystem);
   if(SceneDisplay == M_NULL)
      {
      MappFreeDefault(AilApplication, AilSystem, M_NULL, M_NULL, M_NULL);
      return EXIT_FAILURE;
      }
   M3ddispControl(SceneDisplay, M_TITLE, AIL_TEXT("Scene"));

   AIL_ID IllustrationDisplay = MdispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"),
                                           M_WINDOWED, M_NULL);
   AIL_INT IllustrationOffsetX = M3ddispInquire(SceneDisplay, M_SIZE_X, M_NULL);
   MdispControl(IllustrationDisplay, M_TITLE, AIL_TEXT("Objects to inspect"));
   MdispControl(IllustrationDisplay, M_WINDOW_INITIAL_POSITION_X, IllustrationOffsetX);

   // Run the examples.
   IdentificationAndSortingExample(SceneDisplay, IllustrationDisplay);
   PlanarSegmentationExample(SceneDisplay, IllustrationDisplay);

   MdispFree(IllustrationDisplay);
   M3ddispFree(SceneDisplay);

   MappFreeDefault(AilApplication, AilSystem, M_NULL, M_NULL, M_NULL);

   return 0;
   }


//*****************************************************************************
// First example.
//*****************************************************************************
void IdentificationAndSortingExample(AIL_ID SceneDisplay, AIL_ID IllustrationDisplay)
   {
   AIL_ID AilSystem = MobjInquire(SceneDisplay, M_OWNER_SYSTEM, M_NULL);
   AIL_ID SceneGraList = (AIL_ID)M3ddispInquire(SceneDisplay, M_3D_GRAPHIC_LIST_ID, M_NULL);

   // Restore the point cloud and display it.
   AIL_ID AilPointCloud = MbufImport(CONNECTORS_AND_WASHERS_FILE, M_DEFAULT,
                                     M_RESTORE, AilSystem, M_NULL);

   M3dgraRemove(SceneGraList, M_ALL, M_DEFAULT);
   M3dgraControl(SceneGraList, M_DEFAULT_SETTINGS, M_THICKNESS, 3);
   M3dgraControl(SceneGraList, M_DEFAULT_SETTINGS, M_FONT_SIZE, 4);

   M3ddispSelect(SceneDisplay, AilPointCloud, M_DEFAULT, M_DEFAULT);
   M3ddispSetView(SceneDisplay, M_AUTO, M_TOP_TILTED, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   // Set the display's rotation axis center. This will keep the
   // behaviour of auto rotate consistent as we move its interest point.
   M3ddispCopy(M_VIEW_INTEREST_POINT, SceneDisplay, M_ROTATION_AXIS_CENTER, M_DEFAULT);

   // Show an illustration of the objects in the scene.
   AIL_ID IllustrationImage = MbufRestore(CONNECTORS_AND_WASHERS_ILLUSTRATION_FILE,
                                          AilSystem, M_NULL);
   MdispSelect(IllustrationDisplay, IllustrationImage);

   MosPrintf(AIL_TEXT("A 3D point cloud consisting of wire connectors and washers\n")
             AIL_TEXT("is restored from a file and displayed.\n\n")
             AIL_TEXT("Press any key to segment it into separate objects.\n\n"));
   MosGetch();

   // Allocate the segmentation contexts.
   AIL_ID SegmentationContext = M3dblobAlloc(AilSystem, M_SEGMENTATION_CONTEXT,
                                             M_DEFAULT, M_NULL);
   AIL_ID CalculateContext = M3dblobAlloc(AilSystem, M_CALCULATE_CONTEXT, M_DEFAULT, M_NULL);
   AIL_ID Draw3dContext = M3dblobAlloc(AilSystem, M_DRAW_3D_CONTEXT, M_DEFAULT, M_NULL);

   // Allocate the segmentation results. One result is used for each category.
   AIL_ID AllBlobs = M3dblobAllocResult(AilSystem, M_SEGMENTATION_RESULT, M_DEFAULT, M_NULL);
   AIL_ID Connectors = M3dblobAllocResult(AilSystem, M_SEGMENTATION_RESULT, M_DEFAULT, M_NULL);
   AIL_ID Washers = M3dblobAllocResult(AilSystem, M_SEGMENTATION_RESULT, M_DEFAULT, M_NULL);
   AIL_ID UnknownBlobs = M3dblobAllocResult(AilSystem, M_SEGMENTATION_RESULT,
                                            M_DEFAULT, M_NULL);

   // Take advantage of the 2d organization.
   M3dblobControl(SegmentationContext, M_DEFAULT, M_NEIGHBOR_SEARCH_MODE, M_ORGANIZED);

   // Look for neighbors in a 5x5 square kernel.
   M3dblobControl(SegmentationContext, M_DEFAULT, M_NEIGHBORHOOD_ORGANIZED_SIZE, 5);

   // Exclude small isolated clusters.
   M3dblobControl(SegmentationContext, M_DEFAULT,
                  M_NUMBER_OF_POINTS_MIN, LOCAL_SEGMENTATION_MIN_NB_POINTS);

   // Exclude extremely large clusters which make up the background.
   M3dblobControl(SegmentationContext, M_DEFAULT,
                  M_NUMBER_OF_POINTS_MAX, LOCAL_SEGMENTATION_MAX_NB_POINTS);

   // Set the distance between points to be blobbed together.
   M3dblobControl(SegmentationContext, M_DEFAULT,
                  M_MAX_DISTANCE, LOCAL_SEGMENTATION_DISTANCE_THRESHOLD);

   // Segment the point cloud into several blobs.
   M3dblobSegment(SegmentationContext, AilPointCloud, AllBlobs, M_DEFAULT);

   // Draw all blobs in the 3d display.
   M3dblobControlDraw(Draw3dContext, M_DRAW_LABEL, M_ACTIVE, M_ENABLE);
   M3dblobControlDraw(Draw3dContext, M_DRAW_LABEL, M_FONT_SIZE, M_GRAPHIC_LIST_DEFAULT);

   M3dblobControlDraw(Draw3dContext, M_DRAW_BLOBS, M_ACTIVE, M_ENABLE);
   M3dblobControlDraw(Draw3dContext, M_DRAW_BLOBS, M_THICKNESS, M_GRAPHIC_LIST_DEFAULT);

   AIL_INT64 AllBlobsLabel = M3dblobDraw3d(Draw3dContext, AilPointCloud, AllBlobs,
                                           M_ALL, SceneGraList, M_ROOT_NODE, M_DEFAULT);

   MosPrintf(AIL_TEXT("The point cloud is segmented based on the distance between points.\n"));
   MosPrintf(AIL_TEXT("Points belonging to the background plane ")
             AIL_TEXT("or small isolated clusters\n"));
   MosPrintf(AIL_TEXT("are excluded.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Calculate features on the blobs and use them to determine
   // the type of object they represent.
   M3dblobControl(CalculateContext, M_DEFAULT, M_PCA_BOX, M_ENABLE);
   M3dblobControl(CalculateContext, M_DEFAULT, M_LINEARITY, M_ENABLE);
   M3dblobControl(CalculateContext, M_DEFAULT, M_PLANARITY, M_ENABLE);

   M3dblobCalculate(CalculateContext, AilPointCloud, AllBlobs, M_ALL, M_DEFAULT);

   // Connectors are more elongated than other blobs.
   // Use the feature M_LINEARITY, which is a value from 0 (perfect sphere/plane)
   // to 1 (perfect line).
   M3dblobSelect(AllBlobs, Connectors, M_LINEARITY, M_GREATER, 0.5, M_NULL, M_DEFAULT);

   // Washers are flat and circular.
   // Use the feature M_PLANARITY, which is a value from 0 (perfect sphere)
   // to 1 (perfect plane).
   M3dblobSelect(AllBlobs, Washers, M_LINEARITY, M_LESS, 0.2, M_NULL, M_DEFAULT);
   M3dblobSelect(Washers, Washers, M_PLANARITY, M_GREATER, 0.8, M_NULL, M_DEFAULT);

   // Blobs that are neither connectors nor washers are unknown objects.
   // Use M3dblobCombine to subtract already identified blobs from AllBlobs.
   M3dblobCombine(AllBlobs, Connectors, UnknownBlobs, M_SUB, M_DEFAULT);
   M3dblobCombine(UnknownBlobs, Washers, UnknownBlobs, M_SUB, M_DEFAULT);

   // Print the number of blobs in each category.
   AIL_INT NbConnectors = (AIL_INT)M3dblobGetResult(Connectors,   M_DEFAULT, M_NUMBER, M_NULL);
   AIL_INT NbWashers    = (AIL_INT)M3dblobGetResult(Washers,      M_DEFAULT, M_NUMBER, M_NULL);
   AIL_INT NbUnknown    = (AIL_INT)M3dblobGetResult(UnknownBlobs, M_DEFAULT, M_NUMBER, M_NULL);

   MosPrintf(AIL_TEXT("Simple 3D features (planarity, linearity) are calculated on the\n"));
   MosPrintf(AIL_TEXT("blobs and used to identify them.\n\n"));

   MosPrintf(AIL_TEXT("The relevant objects (connectors and washers) have their\n"));
   MosPrintf(AIL_TEXT("bounding box displayed.\n"));
   MosPrintf(AIL_TEXT("Connectors (in red):     %i\n"), NbConnectors);
   MosPrintf(AIL_TEXT("Washers (in green) :     %i\n"), NbWashers);
   MosPrintf(AIL_TEXT("Unknown (in yellow):     %i\n"), NbUnknown);

   // Draw the blobs in the 3d display.
   M3dgraRemove(SceneGraList, AllBlobsLabel, M_DEFAULT);

   M3dblobControlDraw(Draw3dContext, M_DRAW_BLOBS, M_COLOR, M_COLOR_YELLOW);
   M3dblobDraw3d(Draw3dContext, AilPointCloud, UnknownBlobs, M_ALL, SceneGraList,
                 M_ROOT_NODE, M_DEFAULT);

   M3dblobControlDraw(Draw3dContext, M_DRAW_PCA_BOX, M_ACTIVE, M_ENABLE);
   M3dblobControlDraw(Draw3dContext, M_DRAW_BLOBS, M_COLOR, M_COLOR_RED);
   M3dblobDraw3d(Draw3dContext, AilPointCloud, Connectors, M_ALL, SceneGraList,
                 M_ROOT_NODE, M_DEFAULT);

   M3dblobControlDraw(Draw3dContext, M_DRAW_BLOBS, M_COLOR, M_COLOR_GREEN);
   M3dblobDraw3d(Draw3dContext, AilPointCloud, Washers, M_ALL, SceneGraList,
                 M_ROOT_NODE, M_DEFAULT);

   MosPrintf(AIL_TEXT("\nPress any key to continue.\n\n"));
   MosGetch();

   M3dblobFree(UnknownBlobs);
   M3dblobFree(Washers);
   M3dblobFree(Connectors);
   M3dblobFree(AllBlobs);
   M3dblobFree(Draw3dContext);
   M3dblobFree(CalculateContext);
   M3dblobFree(SegmentationContext);

   MbufFree(IllustrationImage);
   MbufFree(AilPointCloud);
   }


//*****************************************************************************
// Second example.
//*****************************************************************************
void PlanarSegmentationExample(AIL_ID SceneDisplay, AIL_ID /*IllustrationDisplay*/)
   {
   AIL_ID AilSystem = MobjInquire(SceneDisplay, M_OWNER_SYSTEM, M_NULL);
   AIL_ID SceneGraList = (AIL_ID)M3ddispInquire(SceneDisplay, M_3D_GRAPHIC_LIST_ID, M_NULL);

   // Restore the point cloud and display it.
   AIL_ID AilPointCloud = MbufImport(TWISTY_PUZZLES_FILE, M_DEFAULT,
                                     M_RESTORE, AilSystem, M_NULL);

   M3dgraRemove(SceneGraList, M_ALL, M_DEFAULT);
   M3dgraControl(SceneGraList, M_DEFAULT_SETTINGS, M_THICKNESS, 1);

   M3ddispSelect(SceneDisplay, AilPointCloud, M_DEFAULT, M_DEFAULT);
   M3ddispSetView(SceneDisplay, M_AUTO, M_TOP_TILTED, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   // Set the display's rotation axis center. This will keep the
   // behaviour of auto rotate consistent as we move its interest point.
   M3ddispCopy(M_VIEW_INTEREST_POINT, SceneDisplay, M_ROTATION_AXIS_CENTER, M_DEFAULT);

   MosPrintf(AIL_TEXT("Another point cloud containing various twisty puzzles is restored.\n\n")
             AIL_TEXT("Press any key to segment it into separate objects.\n\n"));
   MosGetch();

   // Allocate the segmentation objects.
   AIL_ID SegmentationContext = M3dblobAlloc(AilSystem, M_SEGMENTATION_CONTEXT,
                                             M_DEFAULT, M_NULL);
   AIL_ID SegmentationResult = M3dblobAllocResult(AilSystem, M_SEGMENTATION_RESULT,
                                             M_DEFAULT, M_NULL);

   // Take advantage of the 2d organization.
   M3dblobControl(SegmentationContext, M_DEFAULT, M_NEIGHBOR_SEARCH_MODE, M_ORGANIZED);

   // Look for neighbors in a 5x5 square kernel.
   M3dblobControl(SegmentationContext, M_DEFAULT, M_NEIGHBORHOOD_ORGANIZED_SIZE, 5);

   // Exclude small isolated clusters.
   M3dblobControl(SegmentationContext, M_DEFAULT,
                  M_NUMBER_OF_POINTS_MIN, PLANAR_SEGMENTATION_MIN_NB_POINTS);

   // Use an automatic local distance threshold.
   M3dblobControl(SegmentationContext, M_DEFAULT, M_MAX_DISTANCE_MODE, M_AUTO);

   // Use an automatic local normal threshold.
   M3dblobControl(SegmentationContext, M_DEFAULT, M_NORMAL_DISTANCE_MAX_MODE, M_AUTO);

   // Consider flipped normals to be the same.
   M3dblobControl(SegmentationContext, M_DEFAULT, M_NORMAL_DISTANCE_MODE, M_ORIENTATION);

   // First segment the point cloud with only local thresholds.
   M3dblobSegment(SegmentationContext, AilPointCloud, SegmentationResult, M_DEFAULT);

   AIL_INT64 AnnotationLabel = M3dblobDraw3d(M_DEFAULT, AilPointCloud, SegmentationResult,
                                             M_ALL, SceneGraList, M_ROOT_NODE, M_DEFAULT);

   MosPrintf(AIL_TEXT("The point cloud is segmented based on local ")
             AIL_TEXT("thresholds (distance, normals).\n\n"));
   MosPrintf(AIL_TEXT("Local thresholds can separate distinct objects due ")
             AIL_TEXT("to camera occlusions,\n"));
   MosPrintf(AIL_TEXT("but are often not enough to segment a single ")
             AIL_TEXT("object into subparts.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to use global thresholds instead.\n\n"));
   MosGetch();

   // Remove the local normal threshold.
   M3dblobControl(SegmentationContext, M_DEFAULT, M_NORMAL_DISTANCE_MAX_MODE, M_USER_DEFINED);

   // Use a global normal threshold instead.
   M3dblobControl(SegmentationContext, M_DEFAULT,
                  M_GLOBAL_NORMAL_DISTANCE_MAX, PLANAR_SEGMENTATION_NORMAL_THRESHOLD);

   // Segment again with global thresholds.
   M3dblobSegment(SegmentationContext, AilPointCloud, SegmentationResult, M_DEFAULT);

   M3dgraRemove(SceneGraList, AnnotationLabel, M_DEFAULT);
   AnnotationLabel = M3dblobDraw3d(M_DEFAULT, AilPointCloud, SegmentationResult,
                                   M_ALL, SceneGraList, M_ROOT_NODE, M_DEFAULT);

   MosPrintf(AIL_TEXT("The puzzles' sides are now separated.\n\n")
             AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   M3dblobFree(SegmentationContext);
   M3dblobFree(SegmentationResult);

   MbufFree(AilPointCloud);
   }

//*****************************************************************************
// Allocates a 3D display if it is supported.
//*****************************************************************************
AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
   {
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_DISABLE);
   AIL_ID AilDisplay = M3ddispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"),
                                    M_DEFAULT, M_NULL);
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_ENABLE);

   if(AilDisplay == M_NULL)
      {
      MosPrintf(AIL_TEXT("The current system does not support the 3D display.\n")
                AIL_TEXT("Press any key to exit.\n"));
      MosGetch();
      }

   return AilDisplay;
   }

```
