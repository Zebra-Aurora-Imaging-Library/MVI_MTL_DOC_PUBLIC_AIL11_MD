---
title: "Mcal"
description: "This example demonstrates the usage of the Calibration module."
ms.language: "Python"
---

# Mcal

> This example demonstrates the usage of the Calibration module.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `McalAlloc`, `McalAssociate`, `McalControl`, `McalDraw`, `McalFree`, `McalGetCoordinateSystem`, `McalGrid`, `McalInquire`, `McalSetCoordinateSystem`, `McalTransformImage`, `McalUniform`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraControl`, `MgraFree`, `MgraText`, `MmeasAllocMarker`, `MmeasDraw`, `MmeasFindMarker`, `MmeasFree`, `MmeasGetResult`, `MmeasSetMarker`, `MmeasSetScore`, `MmetAddFeature`, `MmetAlloc`, `MmetAllocResult`, `MmetCalculate`, `MmetDraw`, `MmetFree`, `MmetGetResult`, `MmetSetRegion`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Machining, Applications, Measuring, Modules, Buffer, Display, Graphics, Measurement, Metrology, Calibration, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#*****************************************************************************
#
# File name: MCal.py
#
# Synopsis:  This program uses the Calibration module to:
#              - Remove distortion and then take measurements in world units using a 2D 
#                calibration.
#              - Perform a 3D calibration to take measurements at several known elevations.
#              - Calibrate a scene using a partial calibration grid that has a 2D code 
#                fiducial.
#
# Printable calibration grids in PDF format can be found in your
# "Aurora Imaging Library/##/Images/" directory.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved


import ail11 as AIL
import os

# Grid offset specifications. 
GRID_OFFSET_X          = 0
GRID_OFFSET_Y          = 0
GRID_OFFSET_Z          = 0

#  Main function. 
def McalExample():
   
   # Allocate defaults. 
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Print module name. 
   print("CALIBRATION MODULE:")
   print("-------------------\n\n")

   LinearInterpolationCalibration(AilSystem, AilDisplay)

   TsaiCalibration(AilSystem, AilDisplay)

   PartialGridCalibration(AilSystem, AilDisplay)

   # Free defaults.     
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0
   

#****************************************************************************
# Linear interpolation example. 
#****************************************************************************

# Source image files specification. 
GRID_IMAGE_FILE        = os.path.join(AIL.M_IMAGE_PATH, "CalGrid.mim")
BOARD_IMAGE_FILE       = os.path.join(AIL.M_IMAGE_PATH, "CalBoard.mim")

# World description of the calibration grid. 
GRID_ROW_SPACING       = 1
GRID_COLUMN_SPACING    = 1
GRID_ROW_NUMBER        = 18
GRID_COLUMN_NUMBER     = 25

# Measurement boxes specification. 
MEAS_BOX_POS_X1        = 55
MEAS_BOX_POS_Y1        = 24
MEAS_BOX_WIDTH1        = 7
MEAS_BOX_HEIGHT1       = 425

MEAS_BOX_POS_X2        = 225
MEAS_BOX_POS_Y2        = 11
MEAS_BOX_WIDTH2        = 7
MEAS_BOX_HEIGHT2       = 450

# Specification of the stripes' constraints. 
WIDTH_APPROXIMATION    = 410
WIDTH_VARIATION        = 25
MIN_EDGE_VALUE         = 5

def LinearInterpolationCalibration(AilSystem, AilDisplay):
                 
   # Clear the display 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)

   # Restore source image into an automatically allocated image buffer. 
   AilImage = AIL.MbufRestore(GRID_IMAGE_FILE, AilSystem)

   # Display the image buffer. 
   AIL.MdispSelect(AilDisplay, AilImage)

   # Prepare for overlay annotation. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)

   # Pause to show the original image. 
   print("LINEAR INTERPOLATION CALIBRATION:")
   print("---------------------------------\n")
   print("The displayed grid has been grabbed with a high distortion")
   print("camera and will be used to calibrate the camera.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Allocate a camera calibration context. 
   AilCalibration = AIL.McalAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Calibrate the camera with the image of the grid and its world description. 
   AIL.McalGrid(AilCalibration, AilImage,
            GRID_OFFSET_X, GRID_OFFSET_Y, GRID_OFFSET_Z,
            GRID_ROW_NUMBER, GRID_COLUMN_NUMBER,
            GRID_ROW_SPACING, GRID_COLUMN_SPACING,
            AIL.M_DEFAULT, AIL.M_DEFAULT)

   CalibrationStatus = AIL.McalInquire(AilCalibration, AIL.M_CALIBRATION_STATUS + AIL.M_TYPE_AIL_INT)
   if (CalibrationStatus == AIL.M_CALIBRATED):
      
      # Perform a first image transformation with the calibration grid. 
      AIL.McalTransformImage(AilImage, AilImage, AilCalibration,
                         AIL.M_BILINEAR + AIL.M_OVERSCAN_CLEAR, AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Pause to show the corrected image of the grid. 
      print("The camera has been calibrated and the image of the grid")
      print("has been transformed to remove its distortions.")
      print("Press any key to continue.\n")
      AIL.MosGetch()

      # Read the image of the board and associate the calibration to the image. 
      AIL.MbufLoad(BOARD_IMAGE_FILE, AilImage)
      AIL.McalAssociate(AilCalibration, AilImage, AIL.M_DEFAULT)

      # Allocate the measurement markers. 
      MeasMarker1 = AIL.MmeasAllocMarker(AilSystem, AIL.M_STRIPE, AIL.M_DEFAULT)
      MeasMarker2 = AIL.MmeasAllocMarker(AilSystem, AIL.M_STRIPE, AIL.M_DEFAULT)

      # Set the markers' measurement search region. 
      AIL.MmeasSetMarker(MeasMarker1, AIL.M_BOX_ORIGIN, MEAS_BOX_POS_X1, MEAS_BOX_POS_Y1)
      AIL.MmeasSetMarker(MeasMarker1, AIL.M_BOX_SIZE, MEAS_BOX_WIDTH1, MEAS_BOX_HEIGHT1)
      AIL.MmeasSetMarker(MeasMarker2, AIL.M_BOX_ORIGIN, MEAS_BOX_POS_X2, MEAS_BOX_POS_Y2)
      AIL.MmeasSetMarker(MeasMarker2, AIL.M_BOX_SIZE, MEAS_BOX_WIDTH2, MEAS_BOX_HEIGHT2)

      # Set markers' orientation. 
      AIL.MmeasSetMarker(MeasMarker1, AIL.M_ORIENTATION, AIL.M_HORIZONTAL, AIL.M_NULL)
      AIL.MmeasSetMarker(MeasMarker2, AIL.M_ORIENTATION, AIL.M_HORIZONTAL, AIL.M_NULL)

      # Set markers' settings to locate the largest stripe within the range
      #   [WIDTH_APPROXIMATION - WIDTH_VARIATION,
      #    WIDTH_APPROXIMATION + WIDTH_VARIATION],
      #   and with an edge strength over MIN_EDGE_VALUE. 
      AIL.MmeasSetMarker(MeasMarker1, AIL.M_EDGEVALUE_MIN, MIN_EDGE_VALUE, AIL.M_NULL)

      # Remove the default strength characteristic score AIL.Mapping. 
      AIL.MmeasSetScore(MeasMarker1, AIL.M_STRENGTH_SCORE,
                                 0.0,
                                 0.0,
                                 AIL.M_MAX_POSSIBLE_VALUE,
                                 AIL.M_MAX_POSSIBLE_VALUE,
                                 AIL.M_DEFAULT,
                                 AIL.M_DEFAULT,
                                 AIL.M_DEFAULT)

      # Add a width characteristic score AIL.Mapping (increasing ramp)
      # to find the largest stripe within a given range.

      # Width score AIL.Mapping to find the largest stripe within a given
      # width range ]Wmin, Wmax]:

      #    Score
      #       ^
      #       |         /|
      #       |       /  |
      #       |     /    |
      #       +---------------> Width
      #           Wmin  Wmax
      
      AIL.MmeasSetScore(MeasMarker1, AIL.M_STRIPE_WIDTH_SCORE,
                                 WIDTH_APPROXIMATION - WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 AIL.M_DEFAULT,
                                 AIL.M_PIXEL,
                                 AIL.M_DEFAULT)

      # Set the same settings for the second marker. 
      AIL.MmeasSetMarker(MeasMarker2, AIL.M_EDGEVALUE_MIN, MIN_EDGE_VALUE, AIL.M_NULL)

      AIL.MmeasSetScore(MeasMarker2, AIL.M_STRENGTH_SCORE,
                                 0.0,
                                 0.0,
                                 AIL.M_MAX_POSSIBLE_VALUE,
                                 AIL.M_MAX_POSSIBLE_VALUE,
                                 AIL.M_DEFAULT,
                                 AIL.M_DEFAULT,
                                 AIL.M_DEFAULT)

      AIL.MmeasSetScore(MeasMarker2, AIL.M_STRIPE_WIDTH_SCORE,
                                 WIDTH_APPROXIMATION - WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 AIL.M_DEFAULT,
                                 AIL.M_PIXEL,
                                 AIL.M_DEFAULT)

      # Find and measure the position and width of the board. 
      AIL.MmeasFindMarker(AIL.M_DEFAULT, AilImage, MeasMarker1, AIL.M_STRIPE_WIDTH+AIL.M_POSITION)
      AIL.MmeasFindMarker(AIL.M_DEFAULT, AilImage, MeasMarker2, AIL.M_STRIPE_WIDTH+AIL.M_POSITION)

      # Get the world width of the two markers. 
      WorldDistance1 = AIL.MmeasGetResult(MeasMarker1, AIL.M_STRIPE_WIDTH, None, AIL.M_NULL)
      WorldDistance2 = AIL.MmeasGetResult(MeasMarker2, AIL.M_STRIPE_WIDTH, None, AIL.M_NULL)

      # Get the pixel width of the two markers. 
      AIL.MmeasSetMarker(MeasMarker1, AIL.M_RESULT_OUTPUT_UNITS, AIL.M_PIXEL, AIL.M_NULL)
      AIL.MmeasSetMarker(MeasMarker2, AIL.M_RESULT_OUTPUT_UNITS, AIL.M_PIXEL, AIL.M_NULL)
      PixelDistance1 = AIL.MmeasGetResult(MeasMarker1, AIL.M_STRIPE_WIDTH, None, AIL.M_NULL)
      PixelDistance2 = AIL.MmeasGetResult(MeasMarker2, AIL.M_STRIPE_WIDTH, None, AIL.M_NULL)

      # Get the edges position in pixel to draw the annotations. 
      PosX1, PosY1 = AIL.MmeasGetResult(MeasMarker1, AIL.M_POSITION+AIL.M_EDGE_FIRST)
      PosX2, PosY2 = AIL.MmeasGetResult(MeasMarker1, AIL.M_POSITION+AIL.M_EDGE_SECOND)
      PosX3, PosY3 = AIL.MmeasGetResult(MeasMarker2, AIL.M_POSITION+AIL.M_EDGE_FIRST)
      PosX4, PosY4 = AIL.MmeasGetResult(MeasMarker2, AIL.M_POSITION+AIL.M_EDGE_SECOND)

      # Draw the measurement indicators on the image.  
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_YELLOW)
      AIL.MmeasDraw(AIL.M_DEFAULT, MeasMarker1, AilOverlayImage, AIL.M_DRAW_WIDTH, AIL.M_DEFAULT, AIL.M_RESULT)
      AIL.MmeasDraw(AIL.M_DEFAULT, MeasMarker2, AilOverlayImage, AIL.M_DRAW_WIDTH, AIL.M_DEFAULT, AIL.M_RESULT)

      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKCOLOR,  AIL.M_COLOR_BLACK)
      AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, int(PosX1[0]+0.5-40), int((PosY1[0]+0.5)+((PosY2[0] - PosY1[0])/2.0)), " Distance 1 ")
      AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, int(PosX3[0]+0.5-40), int((PosY3[0]+0.5)+((PosY4[0] - PosY3[0])/2.0)), " Distance 2 ")

      # Pause to show the original image and the measurement results. 
      print("A distorted image grabbed with the same camera was loaded and")
      print("calibrated measurements were done to evaluate the board dimensions.\n")
      print("========================================================")
      print("                      Distance 1          Distance 2 ")
      print("--------------------------------------------------------")
      print(" Calibrated unit:   {:8.2f} cm           {:6.2f} cm    ".format(WorldDistance1[0], WorldDistance2[0]))
      print(" Uncalibrated unit: {:8.2f} pixels       {:6.2f} pixels".format(PixelDistance1[0], PixelDistance2[0]))
      print("========================================================\n")
      print("Press any key to continue.\n")
      AIL.MosGetch()

      # Clear the display overlay. 
      AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)

      # Read the image of the PCB. 
      AIL.MbufLoad(BOARD_IMAGE_FILE, AilImage)

      # Transform the image of the board. 
      AIL.McalTransformImage(AilImage, AilImage, AilCalibration, AIL.M_BILINEAR+AIL.M_OVERSCAN_CLEAR, AIL.M_DEFAULT, AIL.M_DEFAULT)

      # show the transformed image of the board. 
      print("The image was corrected to remove its distortions.")

      # Free measurement markers. 
      AIL.MmeasFree(MeasMarker1)
      AIL.MmeasFree(MeasMarker2)
      
   else:
      
      print("Calibration generated an exception.")
      print("See User Guide to resolve the situation.\n")
      

   # Wait for a key to be pressed.  
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.McalFree(AilCalibration)
   AIL.MbufFree(AilImage)
   


#****************************************************************************
#Tsai example. 
#****************************************************************************

# Source image files specification. 
GRID_ORIGINAL_IMAGE_FILE     = os.path.join(AIL.M_IMAGE_PATH, "CalGridOriginal.mim")
OBJECT_ORIGINAL_IMAGE_FILE   = os.path.join(AIL.M_IMAGE_PATH, "CalObjOriginal.mim")
OBJECT_MOVED_IMAGE_FILE      = os.path.join(AIL.M_IMAGE_PATH, "CalObjMoved.mim")

# World description of the calibration grid. 
GRID_ORG_ROW_SPACING        = 1.5
GRID_ORG_COLUMN_SPACING     = 1.5
GRID_ORG_ROW_NUMBER         = 12
GRID_ORG_COLUMN_NUMBER      = 13
GRID_ORG_OFFSET_X           = 0
GRID_ORG_OFFSET_Y           = 0 
GRID_ORG_OFFSET_Z           = 0

# Region parameters for metrology 
MEASURED_CIRCLE_LABEL      = 1
RING1_POS_X                = 2.3
RING1_POS_Y                = 3.9
RING2_POS_X                = 10.7
RING2_POS_Y                = 11.1

RING_START_RADIUS          = 1.25
RING_END_RADIUS            = 2.3

# measured plane position 
RING_THICKNESS             = 0.175
STEP_THICKNESS             = 4.0

# Color definitions 
ABSOLUTE_COLOR   = AIL.M_RGB888(255,0,0)
RELATIVE_COLOR   = AIL.M_RGB888(0,255,0)
REGION_COLOR     = AIL.M_RGB888(0,100,255)
FEATURE_COLOR    = AIL.M_RGB888(255,0,255)


def TsaiCalibration(AilSystem, AilDisplay):        

   # Restore source image into an automatically allocated image buffer. 
   AilImage = AIL.MbufRestore(GRID_ORIGINAL_IMAGE_FILE, AilSystem)

   # Display the image buffer. 
   AIL.MdispSelect(AilDisplay, AilImage)
   # Prepare for overlay annotation. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)

   # Pause to show the original image. 
   print("TSAI BASED CALIBRATION:")
   print("-----------------------\n")
   print("The displayed grid has been grabbed with a high perspective")
   print("camera and will be used to calibrate the camera.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Allocate a camera calibration context. 
   AilCalibration = AIL.McalAlloc(AilSystem, AIL.M_TSAI_BASED, AIL.M_DEFAULT)

   # Calibrate the camera with the image of the grid and its world description. 
   AIL.McalGrid(AilCalibration, AilImage,
            GRID_ORG_OFFSET_X, GRID_ORG_OFFSET_Y, GRID_ORG_OFFSET_Z,
            GRID_ORG_ROW_NUMBER, GRID_ORG_COLUMN_NUMBER,
            GRID_ORG_ROW_SPACING, GRID_ORG_COLUMN_SPACING,
            AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Verify if the camera calibration is successful. 
   CalibrationStatus = AIL.McalInquire(AilCalibration, AIL.M_CALIBRATION_STATUS + AIL.M_TYPE_AIL_INT)
   if (CalibrationStatus == AIL.M_CALIBRATED):
      
      # Display the world absolute coordinate system 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, ABSOLUTE_COLOR)
      AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilOverlayImage, AIL.M_DRAW_ABSOLUTE_COORDINATE_SYSTEM+AIL.M_DRAW_AXES, AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Print camera information 
      print("The camera has been calibrated.")
      print("The world absolute coordinate system is shown in red.\n")
      ShowCameraInformation(AilCalibration)

      # Load source image into an image buffer. 
      AIL.MbufLoad(OBJECT_ORIGINAL_IMAGE_FILE, AilImage)

      # Associate the calibration to the image 
      AIL.McalAssociate(AilCalibration, AilImage, AIL.M_DEFAULT)
      
      # Set the offset of the camera calibration plane. 
      # This moves the relative origin at the top of the first metallic part 
      AIL.McalSetCoordinateSystem(AilImage,
         AIL.M_RELATIVE_COORDINATE_SYSTEM,
         AIL.M_ABSOLUTE_COORDINATE_SYSTEM,
         AIL.M_TRANSLATION + AIL.M_ASSIGN,
         AIL.M_NULL,
         0, 0, -RING_THICKNESS,
         AIL.M_DEFAULT)

      # Display the world relative coordinate system 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, RELATIVE_COLOR)
      AIL.McalDraw(AIL.M_DEFAULT, AilImage, AilOverlayImage, AIL.M_DRAW_RELATIVE_COORDINATE_SYSTEM + AIL.M_DRAW_FRAME, AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Measure the first circle. 
      print("The relative coordinate system (shown in green) was translated by {:.3f} cm".format(-RING_THICKNESS))
      print("in z to align it with the top of the first metallic part.")
      MeasureRing(AilSystem, AilOverlayImage, AilImage, RING1_POS_X, RING1_POS_Y )
      print("Press any key to continue.\n")
      AIL.MosGetch()

      # Modify the offset of the camera calibration plane 
      # This moves the relative origin at the top of the second metallic part  
      AIL.McalSetCoordinateSystem(AilImage,
         AIL.M_RELATIVE_COORDINATE_SYSTEM,
         AIL.M_ABSOLUTE_COORDINATE_SYSTEM,
         AIL.M_TRANSLATION + AIL.M_COMPOSE_WITH_CURRENT,
         AIL.M_NULL,
         0, 0, -STEP_THICKNESS,
         AIL.M_DEFAULT)

      # Clear the overlay 
      AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)
      # Display the world absolute coordinate system 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, ABSOLUTE_COLOR)
      AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilOverlayImage, AIL.M_DRAW_ABSOLUTE_COORDINATE_SYSTEM + AIL.M_DRAW_AXES, AIL.M_DEFAULT, AIL.M_DEFAULT)
      # Display the world relative coordinate system 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, RELATIVE_COLOR)
      AIL.McalDraw(AIL.M_DEFAULT, AilImage, AilOverlayImage, AIL.M_DRAW_RELATIVE_COORDINATE_SYSTEM + AIL.M_DRAW_FRAME, AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Measure the second circle. 
      print("The relative coordinate system was translated by another {:.3f} cm".format(-STEP_THICKNESS))
      print("in z to align it with the top of the second metallic part.")
      MeasureRing(AilSystem, AilOverlayImage, AilImage, RING2_POS_X, RING2_POS_Y )
      print("Press any key to continue.\n")
      AIL.MosGetch()
      
   else:
      
      print("Calibration generated an exception.")
      print("See User Guide to resolve the situation.\n")
      

   # Free all allocations. 
   AIL.McalFree(AilCalibration)
   AIL.MbufFree(AilImage)
   

# Measuring function with AilMetrology module 
def MeasureRing(AilSystem, AilOverlayImage, AilImage, MeasureRingX, MeasureRingY):

   # Allocate metrology context and result. 
   AilMetrolContext = AIL.MmetAlloc(AilSystem, AIL.M_DEFAULT)
   AilMetrolResult = AIL.MmetAllocResult(AilSystem, AIL.M_DEFAULT)

   # Add a first measured segment feature to context and set its search region 
   AIL.MmetAddFeature(AilMetrolContext, AIL.M_MEASURED, AIL.M_CIRCLE, MEASURED_CIRCLE_LABEL,
                  AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, 0, AIL.M_DEFAULT)

   AIL.MmetSetRegion(AilMetrolContext, AIL.M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), 
                 AIL.M_DEFAULT, AIL.M_RING,
                 MeasureRingX, MeasureRingY,
                 RING_START_RADIUS, RING_END_RADIUS,
                 AIL.M_NULL, AIL.M_NULL )

   # Calculate 
   AIL.MmetCalculate(AilMetrolContext, AilImage, AilMetrolResult, AIL.M_DEFAULT)

   # Draw region 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, REGION_COLOR)
   AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, AilOverlayImage, AIL.M_DRAW_REGION, AIL.M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), AIL.M_DEFAULT)

   # Draw the circle 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FEATURE_COLOR)
   AIL.MmetDraw(AIL.M_DEFAULT, AilMetrolResult, AilOverlayImage, AIL.M_DRAW_FEATURE, AIL.M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), AIL.M_DEFAULT)

   Value = AIL.MmetGetResult(AilMetrolResult, AIL.M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), AIL.M_RADIUS)
   print("The large circle's radius was measured: {:.3f} cm.".format(Value))

   # Free all allocations. 
   AIL.MmetFree(AilMetrolResult)
   AIL.MmetFree(AilMetrolContext)
   

# Print the current camera position and orientation  
def ShowCameraInformation(AilCalibration):

   CameraPosX, CameraPosY,CameraPosZ = AIL.McalGetCoordinateSystem(AilCalibration,
                                             AIL.M_CAMERA_COORDINATE_SYSTEM,
                                             AIL.M_ABSOLUTE_COORDINATE_SYSTEM,
                                             AIL.M_TRANSLATION,
                                             AIL.M_NULL,
                                             None, None, None, 
                                             AIL.M_NULL)

   CameraYaw, CameraPitch, CameraRoll = AIL.McalGetCoordinateSystem(AilCalibration,
                                             AIL.M_CAMERA_COORDINATE_SYSTEM,
                                             AIL.M_ABSOLUTE_COORDINATE_SYSTEM,
                                             AIL.M_ROTATION_YXZ,
                                             AIL.M_NULL,
                                             None, None, None, 
                                             AIL.M_NULL)

   # Pause to show the camera position and orientation. 
   print("Camera position in cm:          (x, y, z)           " + "({:.2f}, {:.2f}, {:.2f})".format(CameraPosX, CameraPosY, CameraPosZ))
   print("Camera orientation in degrees:  (yaw, pitch, roll)  " + "({:.2f}, {:.2f}, {:.2f})".format(CameraYaw, CameraPitch, CameraRoll))
   print("Press any key to continue.\n")
   AIL.MosGetch()
   

#****************************************************************************
#Partial grid example. 
#****************************************************************************
# Source image files specification. 
PARTIAL_GRID_IMAGE_FILE = os.path.join(AIL.M_IMAGE_PATH, "PartialGrid.mim")

# Definition of the region to correct. 
CORRECTED_SIZE_X          = 60.0
CORRECTED_SIZE_Y          = 50.0
CORRECTED_OFFSET_X        = -35.0
CORRECTED_OFFSET_Y        = -5.0
CORRECTED_IMAGE_SIZE_X    = 512

def PartialGridCalibration(AilSystem, AilDisplay):
            
   # Clear the display 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)

   # Allocate a graphic list and associate it to the display. 
   AilGraList = AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT)
   AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraList)

   # Restore source image into an automatically allocated image buffer. 
   AilImage = AIL.MbufRestore(PARTIAL_GRID_IMAGE_FILE, AilSystem)
   ImageType = AIL.MbufInquire(AilImage, AIL.M_TYPE)

   # Display the image buffer. 
   AIL.MdispSelect(AilDisplay, AilImage)

   # Pause to show the partial grid image. 
   print("PARTIAL GRID CALIBRATION:")
   print("-------------------------\n")
   print("A camera will be calibrated using a rectangular grid that")
   print("is only partially visible in the camera's field of view.")
   print("The 2D code in the center is used as a fiducial to retrieve")
   print("the characteristics of the calibration grid.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Allocate the calibration object. 
   AilCalibration = AIL.McalAlloc(AilSystem, AIL.M_TSAI_BASED, AIL.M_DEFAULT)

   # Set the calibration to calibrate a partial grid with fiducial. 
   AIL.McalControl(AilCalibration, AIL.M_GRID_PARTIAL, AIL.M_ENABLE)
   AIL.McalControl(AilCalibration, AIL.M_GRID_FIDUCIAL, AIL.M_DATAMATRIX)

   # Calibrate the camera with the partial grid with fiducial. 
   AIL.McalGrid(AilCalibration, AilImage,
            GRID_OFFSET_X, GRID_OFFSET_Y, GRID_OFFSET_Z,
            AIL.M_UNKNOWN, AIL.M_UNKNOWN, AIL.M_FROM_FIDUCIAL, AIL.M_FROM_FIDUCIAL,
            AIL.M_DEFAULT, AIL.M_CHESSBOARD_GRID)

   CalibrationStatus = AIL.McalInquire(AilCalibration, AIL.M_CALIBRATION_STATUS + AIL.M_TYPE_AIL_INT)
   if(CalibrationStatus == AIL.M_CALIBRATED):
      
      # Draw the absolute coordinate system. 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED)
      AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilGraList, AIL.M_DRAW_ABSOLUTE_COORDINATE_SYSTEM,
         AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Draw a box around the fiducial. 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_CYAN)
      AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilGraList, AIL.M_DRAW_FIDUCIAL_BOX,
         AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Get the information of the grid read from the fiducial. 
      RowSpacing = AIL.McalInquire(AilCalibration, AIL.M_ROW_SPACING)
      ColumnSpacing = AIL.McalInquire(AilCalibration, AIL.M_COLUMN_SPACING)
      UnitName = AIL.McalInquire(AilCalibration, AIL.M_GRID_UNIT_SHORT_NAME)

      # Draw the information of the grid read from the fiducial. 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED)
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_INPUT_UNITS, AIL.M_DISPLAY)
      DrawGridInfo(AilGraList, "Row spacing", RowSpacing, 0, UnitName)
      DrawGridInfo(AilGraList, "Col spacing", ColumnSpacing, 1, UnitName)

      # Pause to show the calibration result. 
      print("The camera has been calibrated.\n")
      print("The grid information read is displayed.")
      print("Press any key to continue.\n")
      AIL.MosGetch()

      # Calculate the pixel size and size Y of the corrected image. 
      CorrectedPixelSize = CORRECTED_SIZE_X / CORRECTED_IMAGE_SIZE_X
      CorrectedImageSizeY = int(CORRECTED_SIZE_Y / CorrectedPixelSize)

      # Allocate the corrected image. 
      AilCorrectedImage = AIL.MbufAlloc2d(AilSystem, CORRECTED_IMAGE_SIZE_X, CorrectedImageSizeY, ImageType,
         AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP)

      # Calibrate the corrected image. 
      AIL.McalUniform(AilCorrectedImage, CORRECTED_OFFSET_X, CORRECTED_OFFSET_Y,
         CorrectedPixelSize, CorrectedPixelSize, 0.0, AIL.M_DEFAULT)

      # Correct the calibrated image. 
      AIL.McalTransformImage(AilImage, AilCorrectedImage, AilCalibration,
         AIL.M_BILINEAR + AIL.M_OVERSCAN_CLEAR, AIL.M_DEFAULT,
         AIL.M_WARP_IMAGE + AIL.M_USE_DESTINATION_CALIBRATION)

      # Select the corrected image on the display. 
      AIL.MgraClear(AIL.M_DEFAULT, AilGraList)
      AIL.MdispSelect(AilDisplay, AilCorrectedImage)

      # Pause to show the corrected image. 
      print("A sub-region of the grid was selected and transformed")
      print("to remove the distortions.")
      print("The sub-region dimensions and position are:")
      print("   Size X  : {:3.3g} {:s}".format(CORRECTED_SIZE_X, UnitName))
      print("   Size Y  : {:3.3g} {:s}".format(CORRECTED_SIZE_Y, UnitName))
      print("   Offset X: {:3.3g} {:s}".format(CORRECTED_OFFSET_X, UnitName))
      print("   Offset Y: {:3.3g} {:s}".format(CORRECTED_OFFSET_Y, UnitName))

      # Wait for a key to be pressed. 
      print("Press any key to quit.\n")
      AIL.MosGetch()

      AIL.MbufFree(AilCorrectedImage)
      
   else:
      
      print("Calibration generated an exception.")
      print("See User Guide to resolve the situation.\n")
      print("Press any key to quit.\n")
      AIL.MosGetch()
      

   # Free all allocations. 
   AIL.McalFree(AilCalibration)
   AIL.MbufFree(AilImage)
   AIL.MgraFree(AilGraList)
   

# Definition of the parameters for the drawing of the grid info 
LINE_HEIGHT      = 16
MAX_INFO_SIZE    = 64

# Draw an information of the grid. 
def DrawGridInfo(AilGraList, InfoTag, Value, LineOffsetY, Units):
   Info = "{}: {:.3g} {}".format(InfoTag, Value, Units)
   AIL.MgraText(AIL.M_DEFAULT, AilGraList, 0, LineOffsetY * LINE_HEIGHT, Info)

if __name__ == "__main__":
   McalExample()

```
