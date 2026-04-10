---
title: "M3dmod"
description: "This example demonstrates how to use the 3D model finder module to define surface models and search for them in 3D scenes."
ms.language: "Python"
---

# M3dmod

> This example demonstrates how to use the 3D model finder module to define surface models and search for them in 3D scenes.

**Language:** Python

**Functions used:** `M3ddispAlloc`, `M3ddispControl`, `M3ddispCopy`, `M3ddispFree`, `M3ddispInquire`, `M3ddispSelect`, `M3ddispSetView`, `M3dgraControl`, `M3dgraCopy`, `M3dgraRemove`, `M3dmodAlloc`, `M3dmodAllocResult`, `M3dmodControl`, `M3dmodDefine`, `M3dmodDraw3d`, `M3dmodFind`, `M3dmodFree`, `M3dmodGetResult`, `M3dmodPreprocess`, `MappAlloc`, `MappControl`, `MappFree`, `MappTimer`, `MbufFree`, `MbufInquireContainer`, `MbufRestore`, `MobjInquire`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Automation, Applications, Finding/Locating, Identifying, Modules, 3D Display, 3D Graphics, 3D Model Finder, Buffer, What's New, AIL 10.0 SP6

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: M3dmod.py
#
# Synopsis: This example demonstrates how to use the 3D model finder module
#           to define surface models and search for them in 3D scenes.
#           A simple single model search is presented first followed by a more
#           complex example of multiple occurrences in a complex scene.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################


import ail11 as AIL
import os

# --------------------------------------------------------------
# CDisplay
# Class that manages the 2D/3D displays for 3D examples.
# --------------------------------------------------------------
class CDisplay:
   def __init__(self, AilSystem):
      self.m_AilSystem = AilSystem
      self.m_AilDisplay     = AIL.M_NULL
      self.m_AilGraphicList = AIL.M_NULL
      self.m_DisplayType    = AIL.M_NULL
      self.m_Lut            = AIL.M_NULL
      self.m_AilDepthMap    = AIL.M_NULL
      self.m_IntensityMap   = AIL.M_NULL

   # --------------------------------------------------------------
   # Allocates a 3D display and returns its identifier.
   # --------------------------------------------------------------
   def Alloc3dDisplayId(self):
      # Try to allocate a 3d display.
      AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE)
      self.m_AilDisplay = AIL.M3ddispAlloc(self.m_AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT)
      AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE)

      if self.m_AilDisplay == AIL.M_NULL:
         print()
         print("The current system does not support the 3D display.")
         print("A 2D display will be used instead.")

         # Allocate a 2d display instead.
         self.m_AilDisplay = AIL.MdispAlloc(self.m_AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT)
         self.m_Lut        = AIL.MbufAllocColor(self.m_AilSystem, 3, 256, 1, AIL.M_UNSIGNED + 8, AIL.M_LUT)
         AIL.MgenLutFunction(self.m_Lut, AIL.M_COLORMAP_TURBO + AIL.M_FLIP, AIL.M_DEFAULT,
                             AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT,
                             AIL.M_DEFAULT)

      self.m_DisplayType = AIL.MobjInquire(self.m_AilDisplay, AIL.M_OBJECT_TYPE)
      self.GetGraphicListId()

   # --------------------------------------------------------------
   # Sets the window size.
   # --------------------------------------------------------------
   def Size(self, SizeX, SizeY):

      if (self.m_DisplayType == AIL.M_3D_DISPLAY):
         AIL.M3ddispControl(self.m_AilDisplay, AIL.M_SIZE_X, SizeX)
         AIL.M3ddispControl(self.m_AilDisplay, AIL.M_SIZE_Y, SizeY)

      else:
         self.m_AilDepthMap  = AIL.MbufAlloc2d(self.m_AilSystem, int(SizeX), int(SizeY), AIL.M_UNSIGNED + 8, AIL.M_IMAGE | AIL.M_PROC | AIL.M_DISP)
         self.m_IntensityMap = AIL.MbufAllocColor(self.m_AilSystem, 3, int(SizeX), int(SizeY), AIL.M_UNSIGNED + 8, AIL.M_IMAGE | AIL.M_PROC | AIL.M_DISP)

   # --------------------------------------------------------------
   # Sets the window position x.
   # --------------------------------------------------------------
   def PositionX(self, PositionX):
      if (self.m_DisplayType == AIL.M_3D_DISPLAY):
         AIL.M3ddispControl(self.m_AilDisplay, AIL.M_WINDOW_INITIAL_POSITION_X, PositionX)

      else:
         AIL.MdispControl(self.m_AilDisplay, AIL.M_WINDOW_INITIAL_POSITION_X, PositionX)

   # --------------------------------------------------------------
   # Displays the container in the 3D or 2D display.
   # --------------------------------------------------------------
   def DisplayContainer(self, AilContainer, UseLut):
      if (self.m_DisplayType == AIL.M_3D_DISPLAY):
         Label = AIL.M3ddispSelect(self.m_AilDisplay, AilContainer, AIL.M_DEFAULT, AIL.M_DEFAULT)
         if (UseLut):
            AIL.M3dgraCopy(AIL.M_COLORMAP_TURBO + AIL.M_FLIP, AIL.M_DEFAULT, self.m_AilGraphicList, Label, AIL.M_COLOR_LUT, AIL.M_DEFAULT)
            AIL.M3dgraControl(self.m_AilGraphicList, Label, AIL.M_COLOR_USE_LUT, AIL.M_TRUE)
            AIL.M3dgraControl(self.m_AilGraphicList, Label, AIL.M_COLOR_COMPONENT_BAND, 2)
            AIL.M3dgraControl(self.m_AilGraphicList, Label, AIL.M_COLOR_COMPONENT, AIL.M_COMPONENT_RANGE)

         # Set the display's rotation axis center. This will keep the
         # behaviour of auto rotate consistent as we move its interest point.
         AIL.M3ddispCopy(AIL.M_VIEW_INTEREST_POINT, self.m_AilDisplay, AIL.M_ROTATION_AXIS_CENTER, AIL.M_DEFAULT)
      else:
         # Project into a depthmap.
         AIL.M3dimCalibrateDepthMap(AilContainer, self.m_AilDepthMap, self.m_IntensityMap, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_CENTER)

         if (UseLut):
            # Associate a LUT.
            AIL.MbufControl(self.m_AilDepthMap, AIL.M_ASSOCIATED_LUT, self.m_Lut)
            AIL.M3dimProject(AilContainer, self.m_AilDepthMap, AIL.M_NULL, AIL.M_POINT_BASED, AIL.M_MAX_Z, AIL.M_DEFAULT, AIL.M_DEFAULT)
            AIL.MdispSelect(self.m_AilDisplay, self.m_AilDepthMap)

         else:
            HasReflectanceColor = AIL.MbufInquireContainer(AilContainer, AIL.M_COMPONENT_REFLECTANCE, AIL.M_COMPONENT_ID) != AIL.M_NULL
            HasIntensityColor = AIL.MbufInquireContainer(AilContainer, AIL.M_COMPONENT_INTENSITY, AIL.M_COMPONENT_ID)  != AIL.M_NULL

            if (HasReflectanceColor or HasIntensityColor):
               AIL.M3dimProject(AilContainer, self.m_AilDepthMap, self.m_IntensityMap, AIL.M_POINT_BASED, AIL.M_MAX_Z, AIL.M_DEFAULT, AIL.M_DEFAULT)
               AIL.MdispSelect(self.m_AilDisplay, self.m_IntensityMap)
            else:
               AIL.M3dimProject(AilContainer, self.m_AilDepthMap, AIL.M_NULL, AIL.M_POINT_BASED, AIL.M_MAX_Z, AIL.M_DEFAULT, AIL.M_DEFAULT)
               AIL.MdispSelect(self.m_AilDisplay, self.m_AilDepthMap)

   # --------------------------------------------------------------
   #  Set the title.
   # --------------------------------------------------------------
   def Title(self, Title):
      if (self.m_DisplayType == AIL.M_3D_DISPLAY):
         AIL.M3ddispControl(self.m_AilDisplay, AIL.M_TITLE, Title)
      else:
         AIL.MdispControl(self.m_AilDisplay, AIL.M_TITLE, Title)

   # --------------------------------------------------------------
   #  Set the 3D disply view.
   # --------------------------------------------------------------
   def SetView(self, Mode, Param1, Param2, Param3):
      if (self.m_DisplayType == AIL.M_3D_DISPLAY):
         AIL.M3ddispSetView(self.m_AilDisplay, Mode, Param1, Param2, Param3, AIL.M_DEFAULT)

         # Set the display's rotation axis center. This will keep the
         # behaviour of auto rotate consistent as we move its interest point.
         AIL.M3ddispCopy(AIL.M_VIEW_INTEREST_POINT, self.m_AilDisplay, AIL.M_ROTATION_AXIS_CENTER, AIL.M_DEFAULT)

   # --------------------------------------------------------------
   # Draw the 3d model occurrences found.
   # --------------------------------------------------------------
   def Draw(self, AilResult):
      if (self.m_DisplayType == AIL.M_3D_DISPLAY):
         return AIL.M3dmodDraw3d(AIL.M_DEFAULT, AilResult, AIL.M_ALL, self.m_AilGraphicList, AIL.M_DEFAULT, AIL.M_DEFAULT)

      else:
         Ail3dGraphicList  = AIL.M3dgraAlloc(self.m_AilSystem, AIL.M_DEFAULT)
         AIL.M3dmodDraw3d(AIL.M_DEFAULT, AilResult, AIL.M_ALL, Ail3dGraphicList, AIL.M_DEFAULT, AIL.M_DEFAULT)

         # Clear the graphic list.
         AIL.MgraControlList(self.m_AilGraphicList, AIL.M_ALL, AIL.M_DEFAULT, AIL.M_DELETE, AIL.M_DEFAULT)

         # Get all 3d graphics.
         Labels = AIL.M3dgraInquire(Ail3dGraphicList, AIL.M_ROOT_NODE, AIL.M_CHILDREN + AIL.M_RECURSIVE)

         Matrix = AIL.M3dgeoAlloc(self.m_AilSystem, AIL.M_TRANSFORMATION_MATRIX, AIL.M_DEFAULT)

         AilContainer = AIL.MbufAllocContainer(self.m_AilSystem, AIL.M_PROC | AIL.M_DISP, AIL.M_DEFAULT)

         # Draw all 3d boxes and dots in the 2d display.
         for Label in Labels:
            GraphicType = AIL.M3dgraInquire(Ail3dGraphicList, Label, AIL.M_GRAPHIC_TYPE)

            if (GraphicType == AIL.M_GRAPHIC_TYPE_DOTS):
               Color = AIL.M3dgraInquire(Ail3dGraphicList, Label, AIL.M_COLOR)
               PointsX = AIL.M3dgraInquire(Ail3dGraphicList, Label, AIL.M_POINTS_X)
               PointsY = AIL.M3dgraInquire(Ail3dGraphicList, Label, AIL.M_POINTS_Y)

               AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, Color)
               AIL.MgraControl(AIL.M_DEFAULT, AIL.M_INPUT_UNITS, AIL.M_WORLD)
               AIL.MgraDots(AIL.M_DEFAULT, self.m_AilGraphicList, AIL.M_DEFAULT, PointsX, PointsY, AIL.M_DEFAULT)

            elif (GraphicType == AIL.M_GRAPHIC_TYPE_BOX):
               CenterX = AIL.M3dgraInquire(Ail3dGraphicList, Label, AIL.M_CENTER_X)
               CenterY = AIL.M3dgraInquire(Ail3dGraphicList, Label, AIL.M_CENTER_Y)
               SizeX =  AIL.M3dgraInquire(Ail3dGraphicList, Label, AIL.M_SIZE_X)
               SizeY = AIL.M3dgraInquire(Ail3dGraphicList, Label, AIL.M_SIZE_Y)
               AIL.M3dgraCopy(Ail3dGraphicList, Label, Matrix, AIL.M_DEFAULT, AIL.M_TRANSFORMATION_MATRIX, AIL.M_DEFAULT)
               (RotZ, RotX, RotY) = AIL.M3dgeoMatrixGetTransform(Matrix, AIL.M_ROTATION_ZXY, None, None, None, AIL.M_NULL, AIL.M_DEFAULT)

               AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_WHITE)
               AIL.MgraControl(AIL.M_DEFAULT, AIL.M_INPUT_UNITS, AIL.M_WORLD)
               AIL.MgraRectAngle(AIL.M_DEFAULT, self.m_AilGraphicList, CenterX, CenterY, SizeX, SizeY, -RotZ, AIL.M_CENTER_AND_DIMENSION)

         AIL.M3dgeoFree(Matrix)
         AIL.M3dgraFree(Ail3dGraphicList)
         AIL.MbufFree(AilContainer)
      return 0

   # --------------------------------------------------------------
   # Updates the displayed image.
   # --------------------------------------------------------------
   def UpdateDisplay(self, AilContainer, UseLut):
      if (self.m_DisplayType == AIL.M_3D_DISPLAY):
            return; # Containers are updated automatically in the 3D display
      else:
            self.DisplayContainer(AilContainer, UseLut)

   def Clear(self, Label):
      if (self.m_DisplayType == AIL.M_3D_DISPLAY):
            AIL.M3dgraRemove(self.m_AilGraphicList, Label, AIL.M_DEFAULT)
      else:
            AIL.MgraControlList(self.m_AilGraphicList, AIL.M_ALL, AIL.M_DEFAULT, AIL.M_DELETE, AIL.M_DEFAULT)

   # --------------------------------------------------------------
   # Free the display.
   # --------------------------------------------------------------
   def FreeDisplay(self):
      if (self.m_DisplayType == AIL.M_DISPLAY):
            AIL.MdispFree(self.m_AilDisplay)
            AIL.MbufFree(self.m_Lut)
            AIL.MbufFree(self.m_AilDepthMap)
            AIL.MbufFree(self.m_IntensityMap)
            AIL.MgraFree(self.m_AilGraphicList)
      else:
            AIL.M3ddispFree(self.m_AilDisplay)

   # --------------------------------------------------------------
   # Gets the display's graphic list, or allocates a standalone one.
   # --------------------------------------------------------------
   def GetGraphicListId(self):
      if (self.m_DisplayType == AIL.M_3D_DISPLAY):
         self.m_AilGraphicList = AIL.M3ddispInquire(self.m_AilDisplay, AIL.M_3D_GRAPHIC_LIST_ID)
      else:
         # Associate a graphic list.
         self.m_AilGraphicList = AIL.MgraAllocList(self.m_AilSystem, AIL.M_DEFAULT)
         AIL.MdispControl(self.m_AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, self.m_AilGraphicList)



# Input scanned point cloud files.
SINGLE_MODEL   = os.path.join(AIL.M_IMAGE_PATH, "SimpleModel.mbufc")
SINGLE_SCENE   = os.path.join(AIL.M_IMAGE_PATH, "SimpleScene.mbufc")
COMPLEX_MODEL1 = os.path.join(AIL.M_IMAGE_PATH, "ComplexModel1.ply")
COMPLEX_MODEL2 = os.path.join(AIL.M_IMAGE_PATH, "ComplexModel2.ply")
COMPLEX_SCENE  = os.path.join(AIL.M_IMAGE_PATH, "ComplexScene.ply")

# Constants.
DISP_SIZE_X = 480
DISP_SIZE_Y = 420

# --------------------------------------------------------------
# Simple scene with a single occurrence.
# --------------------------------------------------------------
def SimpleSceneSurfaceFinder(AilSystem, DisplayModel : CDisplay, DisplayScene : CDisplay):
   # Allocate a surface Model Finder context.
   AilContext = AIL.M3dmodAlloc(AilSystem, AIL.M_FIND_SURFACE_CONTEXT, AIL.M_DEFAULT)

   # Allocate a surface Model Finder result.
   AilResult = AIL.M3dmodAllocResult(AilSystem, AIL.M_FIND_SURFACE_RESULT, AIL.M_DEFAULT)

   # Restore the model container and display it.
   AilModelContainer = AIL.MbufRestore(SINGLE_MODEL, AilSystem)
   DisplayModel.SetView(AIL.M_AZIM_ELEV_ROLL, 45, -35, 180)
   DisplayModel.DisplayContainer(AilModelContainer, True)
   print("The 3D point cloud of the model is restored from a file and displayed.")

   # Load the single model scene point cloud.
   AilSceneContainer = AIL.MbufRestore(SINGLE_SCENE, AilSystem)

   DisplayScene.SetView(AIL.M_AZIM_ELEV_ROLL, 202, -20.0, 182.0)
   DisplayScene.DisplayContainer(AilSceneContainer, True)

   print("The 3D point cloud of the scene is restored from a file and displayed.\n")
   print("Press any key to start.\n");
   AIL.MosGetch()

   # Define the surface model.
   AIL.M3dmodDefine(AilContext, AIL.M_ADD_FROM_POINT_CLOUD, AIL.M_SURFACE,
                     AilModelContainer, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT,
                     AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
   print("Define the model using the given model point cloud.\n")

   # Set the search perseverance.
   print("Set the lowest perseverance to increase the search speed for a simple scene.\n")
   AIL.M3dmodControl(AilContext, AIL.M_DEFAULT, AIL.M_PERSEVERANCE, 0.0)

   print("Set the scene complexity to low to increase the search speed for a simple scene.\n")
   AIL.M3dmodControl(AilContext, AIL.M_DEFAULT, AIL.M_SCENE_COMPLEXITY, AIL.M_LOW)

   # Preprocess the search context.
   AIL.M3dmodPreprocess(AilContext, AIL.M_DEFAULT)

   print("M_COMPONENT_NORMALS_AIL is added to the point cloud if not present.\n")
   # The surface finder requires the existence of M_COMPONENT_NORMALS_AIL in the point cloud.
   AddComponentNormalsIfMissing(AilSceneContainer)

   print("3D surface finder is running..\n")

   # Reset the timer.
   AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS)

   # Find the model.
   AIL.M3dmodFind(AilContext, AilSceneContainer, AilResult, AIL.M_DEFAULT)

   # Read the find time.
   ComputationTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)

   ShowResults(AilResult, ComputationTime)
   DisplayScene.Draw(AilResult)

   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Free objects.
   AIL.M3dmodFree(AilContext)
   AIL.M3dmodFree(AilResult)
   AIL.MbufFree(AilModelContainer)
   AIL.MbufFree(AilSceneContainer)

# --------------------------------------------------------------
# Complex scene with multiple occurrences.
# --------------------------------------------------------------
def ComplexSceneSurfaceFinder(AilSystem, DisplayModel : CDisplay, DisplayScene : CDisplay):
   # Allocate a surface 3D Model Finder context.
   AilContext = AIL.M3dmodAlloc(AilSystem, AIL.M_FIND_SURFACE_CONTEXT, AIL.M_DEFAULT)

   # Allocate a surface 3D Model Finder result.
   AilResult = AIL.M3dmodAllocResult(AilSystem, AIL.M_FIND_SURFACE_RESULT, AIL.M_DEFAULT)

   DisplayModel.Clear(AIL.M_ALL)
   DisplayScene.Clear(AIL.M_ALL)

   # Restore the first model container and display it.
   AilModelContainer = AIL.MbufRestore(COMPLEX_MODEL1, AilSystem)
   DisplayModel.SetView(AIL.M_AZIM_ELEV_ROLL, 290, -67, 265)
   DisplayModel.DisplayContainer(AilModelContainer, False)
   DisplayModel.SetView(AIL.M_AUTO, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
   print("The 3D point cloud of the first model is restored from a file and displayed.")

   # Load the complex scene point cloud.
   AilSceneContainer = AIL.MbufRestore(COMPLEX_SCENE, AilSystem)

   DisplayScene.SetView(AIL.M_AZIM_ELEV_ROLL, 260, -72, 142)
   DisplayScene.DisplayContainer(AilSceneContainer, False)
   DisplayScene.SetView(AIL.M_AUTO, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
   DisplayScene.SetView(AIL.M_ZOOM, 1.2, AIL.M_DEFAULT, AIL.M_DEFAULT)

   print("The 3D point cloud of the scene is restored from a file and displayed.\n")
   print("Press any key to start.\n")
   AIL.MosGetch()

   print("M_COMPONENT_NORMALS_AIL is added to the point cloud if not present.\n")

   # The surface finder requires the existence of M_COMPONENT_NORMALS_AIL in the point cloud.
   AddComponentNormalsIfMissing(AilSceneContainer)

   # Define the surface model.
   AIL.M3dmodDefine(AilContext, AIL.M_ADD_FROM_POINT_CLOUD, AIL.M_SURFACE,
                     AilModelContainer, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT,
                     AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Find all ocurrences.
   AIL.M3dmodControl(AilContext, 0, AIL.M_NUMBER, AIL.M_ALL)
   AIL.M3dmodControl(AilContext, 0, AIL.M_COVERAGE_MAX, 75)

   AIL.M3dmodPreprocess(AilContext, AIL.M_DEFAULT)
   print("3D surface finder is running..\n")

   # Reset the timer.
   AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS)

   # Find the model.
   AIL.M3dmodFind(AilContext, AilSceneContainer, AilResult, AIL.M_DEFAULT)

   # Read the find time.
   ComputationTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)

   ShowResults(AilResult, ComputationTime)
   Label = DisplayScene.Draw(AilResult)

   print("Press any key to continue.\n")
   AIL.MosGetch()

   DisplayScene.Clear(Label)

   AIL.MbufFree(AilModelContainer)
   AilModelContainer = AIL.MbufRestore(COMPLEX_MODEL2, AilSystem)
   DisplayModel.DisplayContainer(AilModelContainer, False)
   DisplayModel.SetView(AIL.M_AUTO, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   print("The 3D point cloud of the second model is restored from file and displayed.\n")

   # Delete the previous model.
   AIL.M3dmodDefine(AilContext, AIL.M_DELETE, AIL.M_DEFAULT, 0, AIL.M_DEFAULT,
                     AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT,
                     AIL.M_DEFAULT)

   # Define the surface model.
   AIL.M3dmodDefine(AilContext, AIL.M_ADD_FROM_POINT_CLOUD, AIL.M_SURFACE,
                     AilModelContainer, AIL.M_DEFAULT, AIL.M_DEFAULT,
                     AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Find all ocurrences.
   AIL.M3dmodControl(AilContext, 0, AIL.M_NUMBER, AIL.M_ALL)
   AIL.M3dmodControl(AilContext, 0, AIL.M_COVERAGE_MAX, 95)

   AIL.M3dmodPreprocess(AilContext, AIL.M_DEFAULT)
   print("3D surface finder is running..\n")

   # Reset the timer.
   AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS)

   # Find the model.
   AIL.M3dmodFind(AilContext, AilSceneContainer, AilResult, AIL.M_DEFAULT)

   # Read the find time.
   ComputationTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)

   ShowResults(AilResult, ComputationTime)
   DisplayScene.Draw(AilResult)

   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free objects.
   AIL.M3dmodFree(AilContext)
   AIL.M3dmodFree(AilResult)
   AIL.MbufFree(AilModelContainer)
   AIL.MbufFree(AilSceneContainer)

# --------------------------------------------------------------
# Adds the component M_COMPONENT_NORMALS_AIL if it's not found.
# --------------------------------------------------------------
def AddComponentNormalsIfMissing(AilContainer):
   AilNormals = AIL.MbufInquireContainer(AilContainer, AIL.M_COMPONENT_NORMALS_AIL, AIL.M_COMPONENT_ID)

   if (AilNormals != AIL.M_NULL):
         return
   SizeX = AIL.MbufInquireContainer(AilContainer, AIL.M_COMPONENT_RANGE, AIL.M_SIZE_X)
   SizeY = AIL.MbufInquireContainer(AilContainer, AIL.M_COMPONENT_RANGE, AIL.M_SIZE_Y)
   if (SizeX < 50 or SizeY < 50):
      AIL.M3dimNormals(AIL.M_NORMALS_CONTEXT_TREE, AilContainer, AilContainer, AIL.M_DEFAULT)
   else:
      AIL.M3dimNormals(AIL.M_NORMALS_CONTEXT_ORGANIZED, AilContainer, AilContainer, AIL.M_DEFAULT)

# --------------------------------------------------------------
# Shows the surface finder results.
# --------------------------------------------------------------
def ShowResults(AilResult, ComputationTime):
   Status = AIL.M3dmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_STATUS)
   if (Status != AIL.M_COMPLETE):
      print("The find process is not completed.")

   NbOcc = int(AIL.M3dmodGetResult(AilResult, AIL.M_DEFAULT, AIL.M_NUMBER))
   print("Found {0} occurrence(s) in {1:.2f} s.".format(NbOcc, ComputationTime))
   print()

   if (NbOcc == 0):
         return

   print("Index        Score        Score_Target");
   print("------------------------------------------------------");

   for i in range(NbOcc):
      ScoreTarget = AIL.M3dmodGetResult(AilResult, i, AIL.M_SCORE_TARGET)
      Score = AIL.M3dmodGetResult(AilResult, i, AIL.M_SCORE)
      print("  {0}          {1:.4f}      {2:6.2f}          ".format( i, Score, ScoreTarget))

   print()

# --------------------------------------------------------------
# Main.
# --------------------------------------------------------------
def M3dmodExample():
   print("[EXAMPLE NAME]")
   print("M3dmod")
   print()
   print("[SYNOPSIS]")
   print("This example demonstrates how to use the 3D model finder module")
   print("to define surface models and search for them in 3D scenes.")
   print()

   print("[MODULES USED]")
   print("Modules used: 3D Model Finder, 3D Display, 3D Graphics, and 3D Image")
   print("Processing.")
   print()

   # Allocate objects.
   AilApplication = AIL.MappAlloc(AIL.M_NULL, AIL.M_DEFAULT)
   AilSystem = AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Allocate the display.
   DisplayModel = CDisplay(AilSystem)
   DisplayModel.Alloc3dDisplayId()
   DisplayModel.Size(DISP_SIZE_X / 2, DISP_SIZE_Y / 2)
   DisplayModel.Title("Model Cloud")

   DisplayScene = CDisplay(AilSystem)
   DisplayScene.Alloc3dDisplayId()
   DisplayScene.Size(DISP_SIZE_X, DISP_SIZE_Y)
   DisplayScene.PositionX((1.04 * 0.5 * DISP_SIZE_X))
   DisplayScene.Title("Scene Cloud")

   SimpleSceneSurfaceFinder(AilSystem, DisplayModel, DisplayScene)

   ComplexSceneSurfaceFinder(AilSystem, DisplayModel, DisplayScene)

   # Free objects.
   DisplayModel.FreeDisplay()
   DisplayScene.FreeDisplay()
   AIL.MsysFree(AilSystem)
   AIL.MappFree(AilApplication)

   return 0

if __name__ == "__main__":
   M3dmodExample()

```
