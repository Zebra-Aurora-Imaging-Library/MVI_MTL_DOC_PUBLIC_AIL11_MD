---
title: "M3dreg"
description: "This program demonstrates how to use the 3D surface registration to register several partial point clouds of a 3D object and stitch them into a single complete point cloud."
ms.language: "Python"
---

# M3dreg

> This program demonstrates how to use the 3D surface registration to register several partial point clouds of a 3D object and stitch them into a single complete point cloud.

**Language:** Python

**Functions used:** `M3dregAlloc`, `M3dregAllocResult`, `M3dregControl`, `M3dregInquire`, `M3dregSetLocation`, `M3dregCalculate`, `M3dregMerge`, `M3dregFree`, `M3dimAlloc`, `M3dimAllocResult`, `M3dimControl`, `M3dimStat`, `M3dimGetResult`, `M3dimFree`, `M3ddispCopy`, `M3dgeoInquire`, `M3dgeoLine`, `MappAlloc`, `MappFree`, `MappTimer`, `MbufAllocContainer`, `MbufInquireContainer`, `MbufImport`, `MbufAllocColor`, `MbufAlloc2d`, `MbufClear`, `MbufFree`, `M3ddispAlloc`, `M3ddispControl`, `M3ddispSelect`, `M3ddispFree`, `MsysAlloc`, `MsysFree`, `M3ddispSetView`, `MappControl`, `MbufAllocComponent`, `MobjInquire`, `M3dimCalculateMapSize`, `M3dimCalibrateDepthMap`, `M3dimProject`, `M3dimRotate`, `MdispAlloc`, `MdispFree`, `MdispSelect`

**Categories:** Overview, General, Industries, Machining, Automation, Applications, Stitching, 3D profiling, Modules, 3D Registration, Buffer, 3D Display, 3D Graphics, Image Processing, 3D Image Processing, 3D Geometry, What's New, AIL 10.0 SP4

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: M3dreg.py
#
# Synopsis: This example demonstrates how to use the 3D registration module
#           to stitch several partial point clouds of a 3D object together
#           into a single complete point cloud.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################

 
import ail11 as AIL
import os

# --------------------------------------------------------------
# Example description.                                          
# --------------------------------------------------------------
def PrintHeader():
   print("[EXAMPLE NAME]")
   print("M3dreg\n")

   print("[SYNOPSIS]")
   print("This example demonstrates how to use the 3D Registration module ")
   print("to stitch several partial point clouds of a 3D object together ")

   print("into a single complete point cloud.\n")

   print("[MODULES USED]")
   print("Modules used: 3D Registration, 3D Display, 3D Graphics, and 3D Image\nProcessing.\n")

# Input scanned point cloud (PLY) files. 
NUM_SCANS = 6
FILE_POINT_CLOUD = [
   os.path.join(AIL.M_IMAGE_PATH, "Cloud1.ply"),
   os.path.join(AIL.M_IMAGE_PATH, "Cloud2.ply"),
   os.path.join(AIL.M_IMAGE_PATH, "Cloud3.ply"),
   os.path.join(AIL.M_IMAGE_PATH, "Cloud4.ply"),
   os.path.join(AIL.M_IMAGE_PATH, "Cloud5.ply"),
   os.path.join(AIL.M_IMAGE_PATH, "Cloud6.ply")
]

# The colors assigned for each cloud. 
Color = [
   AIL.M_RGB888(0,   159, 255),
   AIL.M_RGB888(154,  77,  66),
   AIL.M_RGB888(0,   255, 190),
   AIL.M_RGB888(120,  63, 193),
   AIL.M_RGB888(31,  150, 152),
   AIL.M_RGB888(255, 172, 253),
   AIL.M_RGB888(177, 204, 113)
]

def M3dregExample():
   # Print example information in console. 
   PrintHeader()
   
   # Allocate objects. 
   AilApplication = AIL.MappAlloc(AIL.M_NULL, AIL.M_DEFAULT)
   AilSystem = AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   AilContainerIds = [None] * NUM_SCANS
   print("Reading the PLY files of 6 partial point clouds", end="")
   for i in range(NUM_SCANS):
      print(".", end="")
      AilContainerIds[i] = AIL.MbufImport(FILE_POINT_CLOUD[i], AIL.M_DEFAULT, AIL.M_RESTORE, AilSystem, AIL.M_NULL)
      ColorCloud(AilContainerIds[i], Color[i])

   print("\n")

   AilDisplay = Alloc3dDisplayId(AilSystem)

   AilDisplayImage = AIL.M_NULL # Used for 2D display if needed. 
   AilDepthMap = AIL.M_NULL     # Used for 2D display if needed. 

   # Display the first point cloud container. 
   AilDepthMap, AilDisplayImage = DisplayContainer(AilSystem, AilDisplay, AilContainerIds[0], AilDepthMap, AilDisplayImage)
   AutoRotateDisplay(AilSystem, AilDisplay)

   print("Showing the first partial point cloud of the object.")
   print("Press any key to start.\n")
   AIL.MosGetch()
  
   # Allocate context and result for 3D registration (stitching). 
   AilContext = AIL.M3dregAlloc(AilSystem, AIL.M_PAIRWISE_REGISTRATION_CONTEXT, AIL.M_DEFAULT, AIL.M_NULL)
   AilResult  = AIL.M3dregAllocResult(AilSystem, AIL.M_PAIRWISE_REGISTRATION_RESULT, AIL.M_DEFAULT, AIL.M_NULL)

   AIL.M3dregControl(AilContext, AIL.M_DEFAULT, AIL.M_NUMBER_OF_REGISTRATION_ELEMENTS, NUM_SCANS)
   AIL.M3dregControl(AilContext, AIL.M_DEFAULT, AIL.M_MAX_ITERATIONS, 40)

   # Pairwise registration context controls.                                 
   # Use normal subsampling to preserve edges and yield faster registration. 
   AilSubsampleContext = AIL.M_NULL
   AilSubsampleContext = AIL.M3dregInquire(AilContext, AIL.M_DEFAULT, AIL.M_SUBSAMPLE_CONTEXT_ID)
   AIL.M3dregControl(AilContext, AIL.M_DEFAULT, AIL.M_SUBSAMPLE, AIL.M_ENABLE)

   # Keep edge points. 
   AIL.M3dimControl(AilSubsampleContext, AIL.M_SUBSAMPLE_MODE, AIL.M_SUBSAMPLE_NORMAL)
   AIL.M3dimControl(AilSubsampleContext, AIL.M_NEIGHBORHOOD_DISTANCE, 10)
 
   # Chain of set location, i==0 is referencing to the GLOBAL. 
   for i in range(1, NUM_SCANS):
      AIL.M3dregSetLocation(AilContext, i, i-1, AIL.M_IDENTITY_MATRIX, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   print("The 3D stitching between partial point clouds has been performed with")
   print("the help of the points within the expected common overlap regions.\n")

   # Calculate the time to perform the registration. 
   AIL.MappTimer(AilApplication, AIL.M_TIMER_RESET, AIL.M_NULL)

   # Perform the registration (stitching). 
   AIL.M3dregCalculate(AilContext, AilContainerIds, NUM_SCANS, AilResult, AIL.M_DEFAULT)

   ComputationTimeMS = AIL.MappTimer(AilApplication, AIL.M_TIMER_READ, AIL.M_NULL) * 1000.0

   print("The registration of the 6 partial point clouds succeeded in {ComputationTimeMS:.2f} ms.\n".format(ComputationTimeMS=ComputationTimeMS))

   # Merging overlapping point clouds will result into unneeded large number of points at 
   # the overlapping regions.                                                             
   # During merging, subsampling is used to ensure that the density of points             
   # is fairly dense, but without replications.                                           
   StatResultId = AIL.M3dimAllocResult(AilSystem, AIL.M_STATISTICS_RESULT, AIL.M_DEFAULT)
   AIL.M3dimStat(AIL.M_STAT_CONTEXT_DISTANCE_TO_NEAREST_NEIGHBOR, AilContainerIds[0], StatResultId, AIL.M_DEFAULT)

   # Nearest neighbour distances gives a measure of the point cloud density. 
   GridSize = AIL.M3dimGetResult(StatResultId, AIL.M_MIN_DISTANCE_TO_NEAREST_NEIGHBOR)

   # Use the measured point cloud density as guide for the subsampling. 
   AilMergeSubsampleContext = AIL.M3dimAlloc(AilSystem , AIL.M_SUBSAMPLE_CONTEXT, AIL.M_DEFAULT)
   AIL.M3dimControl(AilMergeSubsampleContext, AIL.M_SUBSAMPLE_MODE, AIL.M_SUBSAMPLE_GRID)
   AIL.M3dimControl(AilMergeSubsampleContext, AIL.M_GRID_SIZE_X, GridSize)
   AIL.M3dimControl(AilMergeSubsampleContext, AIL.M_GRID_SIZE_Y, GridSize)
   AIL.M3dimControl(AilMergeSubsampleContext, AIL.M_GRID_SIZE_Z, GridSize)

   # Allocate the point cloud for the final stitched clouds. 
   AilStitchedId = AIL.MbufAllocContainer(AilSystem, AIL.M_PROC + AIL.M_DISP, AIL.M_DEFAULT)

   print("The merging of point clouds will be shown incrementally.")
   print("Press any key to show 2 merged point clouds of 6.\n")
   AIL.MosGetch()

   # Merge can merge all clouds at once, but here it is done incrementally for the display. 
   i = 2
   while i <= NUM_SCANS:
      AIL.M3dregMerge(AilResult, AilContainerIds, i, AilStitchedId, AilMergeSubsampleContext, AIL.M_DEFAULT)

      if i == 2:
         DisplayContainer(AilSystem, AilDisplay, AilStitchedId, AilDepthMap, AilDisplayImage)
      else:
         UpdateDisplay(AilSystem, AilStitchedId, AilDepthMap, AilDisplayImage) 

      if i < NUM_SCANS:
         print("Press any key to show {i} merged point clouds of 6.\n".format(i=(i+1)))
      else:
         print("Press any key to end.")
      i += 1

      AutoRotateDisplay(AilSystem, AilDisplay)
      AIL.MosGetch()
    
   # Free Objects. 
   for i in range(NUM_SCANS):
      AIL.MbufFree(AilContainerIds[i])

   AIL.MbufFree(AilStitchedId)
   AIL.M3dimFree(StatResultId)
   AIL.M3dimFree(AilMergeSubsampleContext)
   AIL.M3dregFree(AilContext)
   AIL.M3dregFree(AilResult)
   FreeDisplay(AilDisplay)
   if AilDisplayImage:
      AIL.MbufFree(AilDisplayImage)
   if AilDepthMap:
      AIL.MbufFree(AilDepthMap)
   AIL.MsysFree(AilSystem)
   AIL.MappFree(AilApplication)
   
   return 0

# -------------------------------------------------------------- 
# Color the container.                                           
# -------------------------------------------------------------- 
def ColorCloud(AilPointCloud, Col):
   SizeX = AIL.MbufInquireContainer(AilPointCloud, AIL.M_COMPONENT_RANGE, AIL.M_SIZE_X, AIL.M_NULL)
   SizeY = AIL.MbufInquireContainer(AilPointCloud, AIL.M_COMPONENT_RANGE, AIL.M_SIZE_Y, AIL.M_NULL)
   ReflectanceId = AIL.MbufAllocComponent(AilPointCloud, 3, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE, AIL.M_COMPONENT_REFLECTANCE)
   AIL.MbufClear(ReflectanceId, Col)

# -------------------------------------------------------------- 
# Auto rotate the 3D object.                                     
# -------------------------------------------------------------- 
def AutoRotateDisplay(AilSystem, AilDisplay):
   DisplayType = AIL.MobjInquire(AilDisplay, AIL.M_OBJECT_TYPE)

   # AutoRotate available only for the 3D display. 
   if DisplayType != AIL.M_3D_DISPLAY:
      return

   # By default the display rotates around the Z axis, but the robot is oriented along the Y axis. 
   Geometry = AIL.M3dgeoAlloc(AilSystem, AIL.M_GEOMETRY, AIL.M_DEFAULT)
   AIL.M3ddispCopy(AilDisplay, Geometry, AIL.M_ROTATION_AXIS, AIL.M_DEFAULT)
   CenterX = AIL.M3dgeoInquire(Geometry, AIL.M_START_POINT_X)
   CenterY = AIL.M3dgeoInquire(Geometry, AIL.M_START_POINT_Y)
   CenterZ = AIL.M3dgeoInquire(Geometry, AIL.M_START_POINT_Z)
   AIL.M3dgeoLine(Geometry, AIL.M_POINT_AND_VECTOR, AIL.M_UNCHANGED, AIL.M_UNCHANGED, AIL.M_UNCHANGED, 0, 1, 0, AIL.M_UNCHANGED, AIL.M_DEFAULT)
   AIL.M3ddispCopy(Geometry, AilDisplay, AIL.M_ROTATION_AXIS, AIL.M_DEFAULT)
   AIL.M3ddispControl(AilDisplay, AIL.M_AUTO_ROTATE, AIL.M_ENABLE)
   AIL.M3dgeoFree(Geometry)

# -------------------------------------------------------------- 
# Allocates a 3D display and returns its identifier.           
# -------------------------------------------------------------- 
def Alloc3dDisplayId(AilSystem):
   AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE)
   AilDisplay = AIL.M3ddispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT)
   AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE)

   if not AilDisplay:
      print("\nThe current system does not support the 3D display.")
      print("A 2D display will be used instead.")
      print("Press any key to continue.")
      AIL.MosGetch()

      # Allocate a 2D Display instead. 
      AilDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_WINDOWED)
   else:
      # Adjust the viewpoint of the 3D display. 
      AIL.M3ddispSetView(AilDisplay, AIL.M_AUTO, AIL.M_BOTTOM_VIEW, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
      print("Press <R> on the display window to stop/start the rotation.\n")

   return AilDisplay

# -------------------------------------------------------------- 
# Display the received container.                                
# -------------------------------------------------------------- 
def DisplayContainer(AilSystem, AilDisplay, AilContainer, AilDepthMap, AilIntensityMap):
   DisplayType = AIL.MobjInquire(AilDisplay, AIL.M_OBJECT_TYPE)

   Use3D = (DisplayType == AIL.M_3D_DISPLAY)
   if Use3D:
      AIL.M3ddispSelect(AilDisplay, AilContainer, AIL.M_ADD, AIL.M_DEFAULT)
      AIL.M3ddispSelect(AilDisplay, AIL.M_NULL, AIL.M_OPEN, AIL.M_DEFAULT)
   else:
      if AilDepthMap == AIL.M_NULL:
         SizeX = 0 # Image size X for 2d display. 
         SizeY = 0 # Image size Y for 2d display. 

         SizeX, SizeY = AIL.M3dimCalculateMapSize(AIL.M_DEFAULT, AilContainer, AIL.M_NULL, AIL.M_DEFAULT)

         AilIntensityMap = AIL.MbufAllocColor(AilSystem, 3, SizeX, SizeY, AIL.M_UNSIGNED + 8, AIL.M_IMAGE | AIL.M_PROC | AIL.M_DISP)
         AilDepthMap     = AIL.MbufAlloc2d   (AilSystem,    SizeX, SizeY, AIL.M_UNSIGNED + 8, AIL.M_IMAGE | AIL.M_PROC | AIL.M_DISP)

         AIL.M3dimCalibrateDepthMap(AilContainer, AilDepthMap, AilIntensityMap, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_CENTER)

      # Rotate the point cloud container to be in the xy plane before projecting. 
      RotatedContainer = AIL.MbufAllocContainer(AilSystem, AIL.M_PROC, AIL.M_DEFAULT)

      AIL.M3dimRotate(AilContainer, RotatedContainer, AIL.M_ROTATION_XYZ, 90, 60, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)     
      AIL.M3dimProject(AilContainer, AilDepthMap, AilIntensityMap, AIL.M_DEFAULT, AIL.M_MIN_Z, AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Display the projected point cloud container. 
      AIL.MdispSelect(AilDisplay, AilIntensityMap)
      AIL.MbufFree(RotatedContainer)

   return AilDepthMap, AilIntensityMap

# -------------------------------------------------------------- 
# Updated the displayed image.                                   
# -------------------------------------------------------------- 
def UpdateDisplay(AilSystem, AilContainer, AilDepthMap, AilIntensityMap):
   if not AilDepthMap:
      return 

   RotatedContainer = AIL.MbufAllocContainer(AilSystem, AIL.M_PROC, AIL.M_DEFAULT)

   AIL.M3dimRotate(AilContainer, RotatedContainer, AIL.M_ROTATION_XYZ, 90, 70, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AIL.M3dimProject(AilContainer, AilDepthMap, AilIntensityMap, AIL.M_DEFAULT, AIL.M_MIN_Z, AIL.M_DEFAULT, AIL.M_DEFAULT)

   AIL.MbufFree(RotatedContainer)

# -------------------------------------------------------------- 
# Free the display.                                              
# -------------------------------------------------------------- 
def FreeDisplay(AilDisplay):
   DisplayType = AIL.MobjInquire(AilDisplay, AIL.M_OBJECT_TYPE)

   if DisplayType == AIL.M_DISPLAY:
      AIL.MdispFree(AilDisplay) 
   else:
      AIL.M3ddispFree(AilDisplay) 

if __name__ == "__main__":
   M3dregExample()

```
