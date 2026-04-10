---
title: "M3dblob"
description: "This program demonstrates how to use the 3d segmentation module to identify objects in a scene and separate them into categories."
ms.language: "Python"
---

# M3dblob

> This program demonstrates how to use the 3d segmentation module to identify objects in a scene and separate them into categories.

**Language:** Python

**Functions used:** `M3dblobAlloc`, `M3dblobAllocResult`, `M3dblobCalculate`, `M3dblobCombine`, `M3dblobControl`, `M3dblobDraw3d`, `M3dblobFree`, `M3dblobGetResult`, `M3dblobSegment`, `M3dblobSelect`, `M3dgeoAlloc`, `M3dgeoFree`, `M3dgeoMatrixGetTransform`, `M3ddispAlloc`, `M3ddispFree`, `M3ddispInquire`, `M3ddispSelect`, `M3dgraAlloc`, `M3dgraControl`, `M3dgraCopy`, `M3dgraFree`, `M3dgraInquire`, `M3dgraRemove`, `M3dimCalibrateDepthMap`, `M3dimProject`, `MappAlloc`, `MappControl`, `MappFree`, `MappHookFunction`, `MbufAlloc2d`, `MbufAllocColor`, `MbufControl`, `MbufFree`, `MbufImport`, `MbufInquireContainer`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `M3ddispCopy`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MdispZoom`, `MgenLutFunction`, `MgraAllocList`, `MgraControl`, `MgraControlList`, `MgraDots`, `MgraFree`, `MgraRectAngle`, `MobjInquire`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Automation, Applications, Finding/Locating, Counting, 3D profiling, Modules, 3D Blob Analysis, 3D Geometry, 3D Image Processing, 3D Display, 3D Graphics, Display, Graphics, Buffer, What's New, AIL 10.0 SP6, AIL 10.0 SP5

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: M3dblob.py
#
# Synopsis:  This program demonstrates how to use the 3d blob analysis module to
#            identify objects in a scene and separate them into categories.
#            See the PrintHeader() function below for a detailed description.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################


import ail11 as AIL
import os;

# Source file specification.
CONNECTORS_AND_WASHERS_FILE = os.path.join(AIL.M_IMAGE_PATH, "ConnectorsAndWashers.mbufc")
CONNECTORS_AND_WASHERS_ILLUSTRATION_FILE = os.path.join(AIL.M_IMAGE_PATH, "ConnectorsAndWashers.png")

TWISTY_PUZZLES_FILE = os.path.join(AIL.M_IMAGE_PATH, "TwistyPuzzles.mbufc")

# Segmentation thresholds.
LOCAL_SEGMENTATION_MIN_NB_POINTS = 100
LOCAL_SEGMENTATION_MAX_NB_POINTS = 10000
LOCAL_SEGMENTATION_DISTANCE_THRESHOLD = 0.75 # in mm

PLANAR_SEGMENTATION_MIN_NB_POINTS = 5000
PLANAR_SEGMENTATION_NORMAL_THRESHOLD = 20    # in deg

# --------------------------------------------------------------
# First example.
# --------------------------------------------------------------
def IdentificationAndSortingExample(SceneDisplay, IllustrationDisplay):

   AilSystem = AIL.MobjInquire(SceneDisplay, AIL.M_OWNER_SYSTEM)
   SceneGraphicList = AIL.M3ddispInquire(SceneDisplay, AIL.M_3D_GRAPHIC_LIST_ID)

   # Restore the point cloud and display it.
   AilPointCloud = AIL.MbufImport(CONNECTORS_AND_WASHERS_FILE, AIL.M_DEFAULT, AIL.M_RESTORE, AilSystem)

   AIL.M3dgraRemove(SceneGraphicList, AIL.M_ALL, AIL.M_DEFAULT)
   AIL.M3dgraControl(SceneGraphicList, AIL.M_DEFAULT_SETTINGS, AIL.M_THICKNESS, 3)
   AIL.M3dgraControl(SceneGraphicList, AIL.M_DEFAULT_SETTINGS, AIL.M_FONT_SIZE, 4)

   AIL.M3ddispSelect(SceneDisplay, AilPointCloud, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AIL.M3ddispSetView(SceneDisplay, AIL.M_AUTO, AIL.M_TOP_TILTED, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Set the display's rotation axis center. This will keep the
   # behaviour of auto rotate consistent as we move its interest point.
   AIL.M3ddispCopy(AIL.M_VIEW_INTEREST_POINT, SceneDisplay, AIL.M_ROTATION_AXIS_CENTER, AIL.M_DEFAULT)

   # Show an illustration of the objects in the scene.
   IllustrationImage = AIL.MbufRestore(CONNECTORS_AND_WASHERS_ILLUSTRATION_FILE, AilSystem)
   AIL.MdispSelect(IllustrationDisplay, IllustrationImage)

   print("A 3D point cloud consisting of wire connectors and washers")
   print("is restored from a file and displayed.")
   print()
   print("Press any key to segment it into separate objects.")
   print()
   AIL.MosGetch()

   # Allocate the segmentation contexts.
   SegmentationContext = AIL.M3dblobAlloc(AilSystem, AIL.M_SEGMENTATION_CONTEXT, AIL.M_DEFAULT)
   CalculateContext = AIL.M3dblobAlloc(AilSystem, AIL.M_CALCULATE_CONTEXT, AIL.M_DEFAULT)
   Draw3dContext = AIL.M3dblobAlloc(AilSystem, AIL.M_DRAW_3D_CONTEXT, AIL.M_DEFAULT)

   # Allocate the segmentation results. One result is used for each category.
   AllBlobs = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT)
   Connectors = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT)
   Washers = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT)
   UnknownBlobs = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT)

   # Segment the point cloud into several blobs.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NEIGHBOR_SEARCH_MODE, AIL.M_ORGANIZED)                  # Take advantage of the 2d organization.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NEIGHBORHOOD_ORGANIZED_SIZE, 5)                         # Look for neighbors in a 5x5 square kernel.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NUMBER_OF_POINTS_MIN, LOCAL_SEGMENTATION_MIN_NB_POINTS) # Exclude small isolated clusters.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NUMBER_OF_POINTS_MAX, LOCAL_SEGMENTATION_MAX_NB_POINTS) # Exclude extremely large clusters which make up the background.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_MAX_DISTANCE, LOCAL_SEGMENTATION_DISTANCE_THRESHOLD)    # Set the distance between points to be blobbed together.

   AIL.M3dblobSegment(SegmentationContext, AilPointCloud, AllBlobs, AIL.M_DEFAULT)

   # Draw all blobs in the 3d display.
   AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_LABEL, AIL.M_ACTIVE, AIL.M_ENABLE)
   AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_LABEL, AIL.M_FONT_SIZE, AIL.M_GRAPHIC_LIST_DEFAULT)
   AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_ACTIVE, AIL.M_ENABLE)
   AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_THICKNESS, AIL.M_GRAPHIC_LIST_DEFAULT)
   AllBlobsLabel = AIL.M3dblobDraw3d(Draw3dContext, AilPointCloud, AllBlobs, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT)

   print("The point cloud is segmented based on the distance between points.")
   print("Points belonging to the background plane or small isolated clusters")
   print("are excluded.")
   print()
   print("Press any key to continue.")
   print()
   AIL.MosGetch()

   # Calculate features on the blobs and use them to determine the type of object they represent.
   AIL.M3dblobControl(CalculateContext, AIL.M_DEFAULT, AIL.M_PCA_BOX, AIL.M_ENABLE)
   AIL.M3dblobControl(CalculateContext, AIL.M_DEFAULT, AIL.M_LINEARITY, AIL.M_ENABLE)
   AIL.M3dblobControl(CalculateContext, AIL.M_DEFAULT, AIL.M_PLANARITY, AIL.M_ENABLE)

   AIL.M3dblobCalculate(CalculateContext, AilPointCloud, AllBlobs, AIL.M_ALL, AIL.M_DEFAULT)

   # Connectors are more elongated than other blobs.
   # Use the feature M_LINEARITY, which is a value from 0 (perfect sphere/plane) to 1 (perfect line).
   AIL.M3dblobSelect(AllBlobs, Connectors, AIL.M_LINEARITY, AIL.M_GREATER, 0.5, AIL.M_NULL, AIL.M_DEFAULT)

   # Washers are flat and circular.
   # Use the feature M_PLANARITY, which is a value from 0 (perfect sphere) to 1 (perfect plane).
   AIL.M3dblobSelect(AllBlobs, Washers, AIL.M_LINEARITY, AIL.M_LESS, 0.2, AIL.M_NULL, AIL.M_DEFAULT)
   AIL.M3dblobSelect(Washers, Washers, AIL.M_PLANARITY, AIL.M_GREATER, 0.8, AIL.M_NULL, AIL.M_DEFAULT)

   # Blobs that are neither connectors nor washers are unknown objects.
   # Use M3dblobCombine to subtract already identified blobs from AllBlobs.
   AIL.M3dblobCombine(AllBlobs, Connectors, UnknownBlobs, AIL.M_SUB, AIL.M_DEFAULT)
   AIL.M3dblobCombine(UnknownBlobs, Washers, UnknownBlobs, AIL.M_SUB, AIL.M_DEFAULT)

   # Print the number of blobs in each category.
   NbConnectors = AIL.M3dblobGetResult(Connectors,   AIL.M_DEFAULT, AIL.M_NUMBER)
   NbWashers    = AIL.M3dblobGetResult(Washers,      AIL.M_DEFAULT, AIL.M_NUMBER)
   NbUnknown    = AIL.M3dblobGetResult(UnknownBlobs, AIL.M_DEFAULT, AIL.M_NUMBER)

   print("Simple 3D features (planarity, linearity) are calculated on the")
   print("blobs and used to identify them.")
   print()
   print("The relevant objects (connectors and washers) have their")
   print("bounding box displayed.")
   print("Connectors (in red):     " + str(NbConnectors))
   print("Washers (in green) :     " + str(NbWashers))
   print("Unknown (in yellow):     " + str(NbUnknown))
   print()

   # Draw the blobs in the 3d display.
   AIL.M3dgraRemove(SceneGraphicList, AllBlobsLabel, AIL.M_DEFAULT)

   AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_COLOR, AIL.M_COLOR_YELLOW)
   AIL.M3dblobDraw3d(Draw3dContext, AilPointCloud, UnknownBlobs, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT)

   AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_PCA_BOX, AIL.M_ACTIVE, AIL.M_ENABLE)
   AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_COLOR, AIL.M_COLOR_RED)
   AIL.M3dblobDraw3d(Draw3dContext, AilPointCloud, Connectors, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT)

   AIL.M3dblobControlDraw(Draw3dContext, AIL.M_DRAW_BLOBS, AIL.M_COLOR, AIL.M_COLOR_GREEN)
   AIL.M3dblobDraw3d(Draw3dContext, AilPointCloud, Washers, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT)

   print("Press any key to continue.")
   print()
   AIL.MosGetch()

   AIL.M3dblobFree(UnknownBlobs)
   AIL.M3dblobFree(Washers)
   AIL.M3dblobFree(Connectors)
   AIL.M3dblobFree(AllBlobs)
   AIL.M3dblobFree(Draw3dContext)
   AIL.M3dblobFree(CalculateContext)
   AIL.M3dblobFree(SegmentationContext)

   AIL.MbufFree(IllustrationImage)
   AIL.MbufFree(AilPointCloud)


# --------------------------------------------------------------
# Second example.
# --------------------------------------------------------------
def PlanarSegmentationExample(SceneDisplay, IllustrationDisplay):

   AilSystem = AIL.MobjInquire(SceneDisplay, AIL.M_OWNER_SYSTEM)
   SceneGraphicList = AIL.M3ddispInquire(SceneDisplay, AIL.M_3D_GRAPHIC_LIST_ID)

   # Restore the point cloud and display it.
   AilPointCloud = AIL.MbufImport(TWISTY_PUZZLES_FILE, AIL.M_DEFAULT, AIL.M_RESTORE, AilSystem)

   AIL.M3dgraRemove(SceneGraphicList, AIL.M_ALL, AIL.M_DEFAULT)
   AIL.M3dgraControl(SceneGraphicList, AIL.M_DEFAULT_SETTINGS, AIL.M_THICKNESS, 1)

   AIL.M3ddispSelect(SceneDisplay, AilPointCloud, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AIL.M3ddispSetView(SceneDisplay, AIL.M_AUTO, AIL.M_TOP_TILTED, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Set the display's rotation axis center. This will keep the
   # behaviour of auto rotate consistent as we move its interest point.
   AIL.M3ddispCopy(AIL.M_VIEW_INTEREST_POINT, SceneDisplay, AIL.M_ROTATION_AXIS_CENTER, AIL.M_DEFAULT)

   print("Another point cloud containing various twisty puzzles is restored.")
   print()
   print("Press any key to segment it into separate objects.")
   print()
   AIL.MosGetch()

   # Allocate the segmentation objects.
   SegmentationContext = AIL.M3dblobAlloc(AilSystem, AIL.M_SEGMENTATION_CONTEXT, AIL.M_DEFAULT)
   SegmentationResult = AIL.M3dblobAllocResult(AilSystem, AIL.M_SEGMENTATION_RESULT, AIL.M_DEFAULT)

   # First segment the point cloud with only local thresholds.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NEIGHBOR_SEARCH_MODE, AIL.M_ORGANIZED)                    # Take advantage of the 2d organization.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NEIGHBORHOOD_ORGANIZED_SIZE, 5)                           # Look for neighbors in a 5x5 square kernel.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NUMBER_OF_POINTS_MIN, PLANAR_SEGMENTATION_MIN_NB_POINTS)  # Exclude small isolated clusters.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_MAX_DISTANCE_MODE, AIL.M_AUTO)                            # Use an automatic local distance threshold.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NORMAL_DISTANCE_MAX_MODE, AIL.M_AUTO)                     # Use an automatic local normal threshold.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NORMAL_DISTANCE_MODE, AIL.M_ORIENTATION)                  # Consider flipped normals to be the same.

   AIL.M3dblobSegment(SegmentationContext, AilPointCloud, SegmentationResult, AIL.M_DEFAULT)

   AnnotationLabel = AIL.M3dblobDraw3d(AIL.M_DEFAULT, AilPointCloud, SegmentationResult, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT)

   print("The point cloud is segmented based on local thresholds (distance, normals).")
   print()
   print("Local thresholds can separate distinct objects due to camera occlusions,")
   print("but are often not enough to segment a single object into subparts.")
   print()
   print("Press any key to use global thresholds instead.")
   print()
   AIL.MosGetch()

   # Then segment again with global thresholds.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_NORMAL_DISTANCE_MAX_MODE, AIL.M_USER_DEFINED);                      # Remove the local normal threshold.
   AIL.M3dblobControl(SegmentationContext, AIL.M_DEFAULT, AIL.M_GLOBAL_NORMAL_DISTANCE_MAX, PLANAR_SEGMENTATION_NORMAL_THRESHOLD);  # Use a global normal threshold instead.

   AIL.M3dblobSegment(SegmentationContext, AilPointCloud, SegmentationResult, AIL.M_DEFAULT)

   AIL.M3dgraRemove(SceneGraphicList, AnnotationLabel, AIL.M_DEFAULT)
   AnnotationLabel = AIL.M3dblobDraw3d(AIL.M_DEFAULT, AilPointCloud, SegmentationResult, AIL.M_ALL, SceneGraphicList, AIL.M_ROOT_NODE, AIL.M_DEFAULT)

   print("The puzzles' sides are now separated.")
   print()
   print("Press any key to end.")
   print()
   AIL.MosGetch()

   AIL.M3dblobFree(SegmentationContext)
   AIL.M3dblobFree(SegmentationResult)

   AIL.MbufFree(AilPointCloud)


# --------------------------------------------------------------
# Allocates a 3D display if it is supported.
# --------------------------------------------------------------
def Alloc3dDisplayId(AilSystem):
   AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE)
   AilDisplay = AIL.M3ddispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT)
   AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE)

   if AilDisplay == AIL.M_NULL:
      print("The current system does not support the 3D display.")
      print("Press any key to exit.")
      AIL.MosGetch()

   return AilDisplay

# --------------------------------------------------------------
# Main.
# --------------------------------------------------------------
def M3dblobExample():
   print("[EXAMPLE NAME]")
   print("M3dblob")
   print()
   print("[SYNOPSIS]")
   print("This program demonstrates how to use the 3d blob analysis module to")
   print("identify objects in a scene and separate them into categories.")
   print()
   print("[MODULES USED]")
   print("Modules used: 3D Blob Analysis, 3D Image Processing,")
   print("3D Display, Display, Buffer, and 3D Graphics.")
   print()

   # Allocate defaults.
   AilApplication, AilSystem = AIL.MappAllocDefault(AIL.M_DEFAULT, DispIdPtr=AIL.M_NULL, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Allocate the displays.
   SceneDisplay = Alloc3dDisplayId(AilSystem)
   if SceneDisplay == AIL.M_NULL:
      AIL.MappFreeDefault(AilApplication, AilSystem, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL)
      return -1
   AIL.M3ddispControl(SceneDisplay, AIL.M_TITLE, "Scene")

   IllustrationDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_WINDOWED)
   IllustrationOffsetX = AIL.M3ddispInquire(SceneDisplay, AIL.M_SIZE_X)
   AIL.MdispControl(IllustrationDisplay, AIL.M_TITLE, "Objects to inspect")
   AIL.MdispControl(IllustrationDisplay, AIL.M_WINDOW_INITIAL_POSITION_X, IllustrationOffsetX)

   # Run the examples.
   IdentificationAndSortingExample(SceneDisplay, IllustrationDisplay)
   PlanarSegmentationExample(SceneDisplay, IllustrationDisplay)

   AIL.MdispFree(IllustrationDisplay)
   AIL.M3ddispFree(SceneDisplay)

   AIL.MappFreeDefault(AilApplication, AilSystem, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL)

   return 0

if __name__ == "__main__":
   M3dblobExample()

```
