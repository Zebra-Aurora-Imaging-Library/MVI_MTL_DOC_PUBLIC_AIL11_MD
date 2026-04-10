---
title: "M3dMap"
description: "This program inspects a wood surface using laser profiling to find any depth defects and computes the volume of a cookie. Note: Printable calibration grids in PDF format can be found in your \"Zebra Imaging/Images/\" directory. Note: When considering a laser-based 3D reconstruction system, the file \"3D Setup Helper.xls\" can be used to accelerate prototyping by choosing an adequate hardware configuration (angle, distance, lens, camera, ...). The file is located in your \"Zebra Imaging/Tools/\" directory."
ms.language: "Python"
---

# M3dMap

> This program inspects a wood surface using laser profiling to find any depth defects and computes the volume of a cookie. Note: Printable calibration grids in PDF format can be found in your "Zebra Imaging/Images/" directory. Note: When considering a laser-based 3D reconstruction system, the file "3D Setup Helper.xls" can be used to accelerate prototyping by choosing an adequate hardware configuration (angle, distance, lens, camera, ...). The file is located in your "Zebra Imaging/Tools/" directory.

**Language:** Python

**Functions used:** `M3dmapAddScan`, `M3dmapAlloc`, `M3dmapAllocResult`, `M3dmapCalibrate`, `M3dmapControl`, `M3dmapCopyResult`, `M3dmapFree`, `M3dmapInquire`, `M3ddispControl`, `MappAlloc`, `MappFree`, `MappTimer`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobGetResult`, `MbufAlloc1d`, `MbufAlloc2d`, `MbufAllocColor`, `MbufAllocContainer`, `MbufClear`, `MbufCopy`, `MbufCopyColor2d`, `MbufDiskInquire`, `MbufFree`, `MbufControlContainer`, `MbufImportSequence`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `McalAlloc`, `McalDraw`, `McalFree`, `McalGrid`, `McalInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispLut`, `MdispSelect`, `MgenLutRamp`, `MgraControl`, `MgraText`, `MimBinarize`, `MimControl`, `MimConvert`, `MimOpen`, `MsysAlloc`, `MsysFree`, `M3ddispAlloc`, `M3ddispFree`, `M3ddispSelect`, `M3ddispSetView`, `M3dgeoAlloc`, `M3dgeoFree`, `M3dgeoPlane`, `M3dimAlloc`, `M3dimCalibrateDepthMap`, `M3dimControl`, `M3dimCrop`, `M3dimFillGaps`, `M3dimFree`, `M3dimProject`, `M3dmetVolume`, `MappControl`

**Categories:** Overview, General, Industries, Machining, Food and Beverage, Applications, Inspecting/Proofing/Verifying, 3D profiling, Modules, Buffer, Display, Graphics, Image Processing, Calibration, Blob Analysis, 3D Map, 3D Display, 3D Graphics, 3D Image Processing, 3D Geometry, 3D Metrology, What's New, AIL 10.0 SP4, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: m3dmap.py 
#
# Synopsis: This program inspects a wood surface using 
#           sheet-of-light profiling (laser) to find any depth defects.
#
# Printable calibration grids in PDF format can be found in your
# "Aurora Imaging Library/##/Images/" directory.
#
# When considering a laser-based 3D reconstruction system, the file "3D Setup Helper.xls"
# can be used to accelerate prototyping by choosing an adequate hardware configuration
# (angle, distance, lens, camera, ...). The file is located in your
# "Aurora Imaging Library/##/Tools/" directory.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################

 
import ail11 as AIL
import math
import os

def M3dMapExample():
   # Allocate defaults.
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Run the depth correction example.
   DepthCorrectionExample(AilSystem, AilDisplay)

   # Run the calibrated camera example.
   CalibratedCameraExample(AilSystem, AilDisplay)

   # Free defaults.    
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)
   
   return 0

#****************************************************************************
# Depth correction example.
#****************************************************************************

# Input sequence specifications.
REFERENCE_PLANES_SEQUENCE_FILE = os.path.join(AIL.M_IMAGE_PATH, "ReferencePlanes.avi")
OBJECT_SEQUENCE_FILE           = os.path.join(AIL.M_IMAGE_PATH, "ScannedObject.avi")

# Peak detection parameters.
PEAK_WIDTH_NOMINAL        =  10
PEAK_WIDTH_DELTA          =   8
MIN_CONTRAST              = 140

# Calibration heights in mm.
CORRECTED_DEPTHS = [1.25, 2.50, 3.75, 5.00]

SCALE_FACTOR = 10000.0 # (depth in world units) * SCALE_FACTOR gives gray levels

# Annotation position.
CALIB_TEXT_POS_X        = 400   
CALIB_TEXT_POS_Y        =  15

def DepthCorrectionExample(AilSystem, AilDisplay):
   # Inquire characteristics of the input sequences.
   SizeX = AIL.MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_SIZE_X)
   SizeY = AIL.MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_SIZE_Y)
   NbReferencePlanes = AIL.MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_NUMBER_OF_IMAGES)
   FrameRate = AIL.MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_FRAME_RATE)
   NbObjectImages = AIL.MbufDiskInquire(OBJECT_SEQUENCE_FILE, AIL.M_NUMBER_OF_IMAGES)

   # Allocate buffer to hold images. 
   AilImage = AIL.MbufAlloc2d(AilSystem, SizeX, SizeY,  8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC)
   AIL.MbufClear(AilImage, 0.0)

   print("\nDEPTH ANALYSIS:")
   print("---------------\n")
   print("This program performs a surface inspection to detect depth defects ")
   print("on a wood surface using a laser (sheet-of-light) profiling system.\n")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Select display. 
   AIL.MdispSelect(AilDisplay, AilImage)

   # Prepare for overlay annotations. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_BACKGROUND_MODE, AIL.M_TRANSPARENT)
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_WHITE)

   # Allocate 3dmap objects. 
   AilLaser = AIL.M3dmapAlloc(AilSystem, AIL.M_LASER, AIL.M_DEPTH_CORRECTION)
   AilCalibScan = AIL.M3dmapAllocResult(AilSystem, AIL.M_LASER_CALIBRATION_DATA, AIL.M_DEFAULT)

   # Set laser line extraction options.
   AilPeakLocator = AIL.M3dmapInquire(AilLaser, AIL.M_DEFAULT, AIL.M_LOCATE_PEAK_1D_CONTEXT_ID + AIL.M_TYPE_AIL_ID)
   AIL.MimControl(AilPeakLocator, AIL.M_PEAK_WIDTH_NOMINAL, PEAK_WIDTH_NOMINAL)
   AIL.MimControl(AilPeakLocator, AIL.M_PEAK_WIDTH_DELTA  , PEAK_WIDTH_DELTA  )
   AIL.MimControl(AilPeakLocator, AIL.M_MINIMUM_CONTRAST  , MIN_CONTRAST      )

   # Open the calibration sequence file for reading.
   AIL.MbufImportSequence(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                   AIL.M_NULL, AIL.M_NULL, AIL.M_OPEN)

   # Read and process all images in the input sequence.
   StartTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)

   for n in range(NbReferencePlanes):

      # Read image from sequence. 
      AIL.MbufImportSequence(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_LOAD, AIL.M_NULL,
                                                             AilImage, AIL.M_DEFAULT, 1, AIL.M_READ)

      # Annotate the image with the calibration height. 
      AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)
      CalibString = "Reference plane {idx}: {height:.2f} mm".format(idx=(n+1), height=round(CORRECTED_DEPTHS[n], 2))
      AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, CALIB_TEXT_POS_X, CALIB_TEXT_POS_Y, CalibString)

      # Set desired corrected depth of next reference plane. 
      AIL.M3dmapControl(AilLaser, AIL.M_DEFAULT, AIL.M_CORRECTED_DEPTH, CORRECTED_DEPTHS[n] * SCALE_FACTOR)

      # Analyze the image to extract laser line.
      AIL.M3dmapAddScan(AilLaser, AilCalibScan, AilImage, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Wait to have a proper frame rate, if necessary. 
      EndTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)
      WaitTime = (1.0 / FrameRate) - (EndTime - StartTime)
      if WaitTime > 0:
         AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_WAIT, WaitTime)
      StartTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)
   
   # Close the calibration sequence file.
   AIL.MbufImportSequence(REFERENCE_PLANES_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                  AIL.M_NULL, AIL.M_NULL, AIL.M_CLOSE)

   # Calibrate the laser profiling context using reference planes of known heights.
   AIL.M3dmapCalibrate(AilLaser, AilCalibScan, AIL.M_NULL, AIL.M_DEFAULT)

   print("The laser profiling system has been calibrated using 4 reference")
   print("planes of known heights.\n")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   print("The wood surface is being scanned.\n")

   # Free the result buffer used for calibration because it will not be used anymore.
   AIL.M3dmapFree(AilCalibScan)
   AilCalibScan = AIL.M_NULL

   # Allocate the result buffer for the scanned depth corrected data.
   AilScan = AIL.M3dmapAllocResult(AilSystem, AIL.M_DEPTH_CORRECTED_DATA, AIL.M_DEFAULT)

   # Open the object sequence file for reading.
   FrameRate = AIL.MbufDiskInquire(OBJECT_SEQUENCE_FILE, AIL.M_FRAME_RATE)
   AIL.MbufImportSequence(OBJECT_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                           AIL.M_NULL, AIL.M_OPEN)

   # Read and process all images in the input sequence.
   StartTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)

   for n in range(NbObjectImages):
      # Read image from sequence.
      AIL.MbufImportSequence(OBJECT_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_LOAD, AIL.M_NULL, AilImage,
                                                                     AIL.M_DEFAULT, 1, AIL.M_READ)

      # Analyze the image to extract laser line and correct its depth. 
      AIL.M3dmapAddScan(AilLaser, AilScan, AilImage, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT)

      # Wait to have a proper frame rate, if necessary. 
      EndTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)
      WaitTime = (1.0/FrameRate) - (EndTime - StartTime)
      if WaitTime > 0:
         AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_WAIT, WaitTime)
      StartTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)

   # Close the object sequence file.
   AIL.MbufImportSequence(OBJECT_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                    AIL.M_NULL, AIL.M_CLOSE)

   # Allocate the image for a partially corrected depth map.
   AilDepthMap = AIL.MbufAlloc2d(AilSystem, SizeX, NbObjectImages, 16 + AIL.M_UNSIGNED,
                                                       AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP)

   # Get partially corrected depth map from accumulated information in the result buffer.
   AIL.M3dmapCopyResult(AilScan, AIL.M_DEFAULT, AilDepthMap, AIL.M_PARTIALLY_CORRECTED_DEPTH_MAP, AIL.M_DEFAULT)

   # Disable display updates.
   AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_DISABLE);

   # Show partially corrected depth map and find defects.
   SetupColorDisplay(AilSystem, AilDisplay, AIL.MbufInquire(AilDepthMap, AIL.M_SIZE_BIT, AIL.M_NULL))
   
   # Display partially corrected depth map.
   AIL.MdispSelect(AilDisplay, AilDepthMap)
   AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_ENABLE);
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)

   print("The pseudo-color depth map of the surface is displayed.\n")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   PerformBlobAnalysis(AilSystem, AilDisplay, AilOverlayImage, AilDepthMap)

   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Disassociates display LUT and clear overlay.
   AIL.MdispSelect(AilDisplay, AIL.M_NULL)
   AIL.MdispLut(AilDisplay, AIL.M_DEFAULT)
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)

   # Free all allocations.
   AIL.M3dmapFree(AilScan)
   AIL.M3dmapFree(AilLaser)
   AIL.MbufFree(AilDepthMap)
   AIL.MbufFree(AilImage)

# Values used for binarization. 
EXPECTED_HEIGHT    = 3.4   # Inspected surface should be at this height (in mm)   
DEFECT_THRESHOLD   = 0.2   # Max acceptable deviation from expected height (mm)   
SATURATED_DEFECT   = 1.0   # Deviation at which defect will appear red (in mm)    

# Radius of the smallest particles to keep. 
MIN_BLOB_RADIUS            =  3

# Pixel offset for drawing text. 
TEXT_H_OFFSET_1           = -50
TEXT_V_OFFSET_1           =  -6
TEXT_H_OFFSET_2           = -30
TEXT_V_OFFSET_2           =   6

def PerformBlobAnalysis(AilSystem, AilDisplay, AilOverlayImage, AilDepthMap):
   # Get size of depth map. 
   SizeX = AIL.MbufInquire(AilDepthMap, AIL.M_SIZE_X)
   SizeY = AIL.MbufInquire(AilDepthMap, AIL.M_SIZE_Y)

   # Allocate a binary image buffer for fast processing. 
   AilBinImage = AIL.MbufAlloc2d(AilSystem, SizeX, SizeY,  1 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC)

   # Binarize image. 
   DefectThreshold = (EXPECTED_HEIGHT - DEFECT_THRESHOLD) * SCALE_FACTOR
   AIL.MimBinarize(AilDepthMap, AilBinImage, AIL.M_FIXED + AIL.M_LESS_OR_EQUAL, DefectThreshold, AIL.M_NULL)

   # Remove small particles. 
   AIL.MimOpen(AilBinImage, AilBinImage, MIN_BLOB_RADIUS, AIL.M_BINARY)

   # Allocate a blob context. *
   AilBlobContext = AIL.MblobAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT)
  
   # Enable the Center Of Gravity and Min Pixel features calculation. *
   AIL.MblobControl(AilBlobContext, AIL.M_CENTER_OF_GRAVITY + AIL.M_GRAYSCALE, AIL.M_ENABLE)
   AIL.MblobControl(AilBlobContext, AIL.M_MIN_PIXEL, AIL.M_ENABLE)
 
   # Allocate a blob result buffer. 
   AilBlobResult = AIL.MblobAllocResult(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT)
 
   # Calculate selected features for each blob. *
   AIL.MblobCalculate(AilBlobContext, AilBinImage, AilDepthMap, AilBlobResult)
 
   # Get the total number of selected blobs. *

   TotalBlobs = AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT)

   print("Number of defects: {TotalBlobs}".format(TotalBlobs=TotalBlobs))

   # Read and print the blob characteristics. *
   # Get the results. 
   CogX = AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_CENTER_OF_GRAVITY_X + AIL.M_GRAYSCALE)
   CogY = AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_CENTER_OF_GRAVITY_Y + AIL.M_GRAYSCALE)
   MinPixels = AIL.MblobGetResult(AilBlobResult, AIL.M_DEFAULT, AIL.M_MIN_PIXEL + AIL.M_TYPE_AIL_INT)

   # Draw the defects. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED)
   AIL.MblobDraw(AIL.M_DEFAULT, AilBlobResult, AilOverlayImage,
               AIL.M_DRAW_BLOBS, AIL.M_INCLUDED_BLOBS, AIL.M_DEFAULT)
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_WHITE)

   # Print the depth of each blob. 
   if TotalBlobs > 1:
      for n in range(TotalBlobs):
         # Write the depth of the defect in the overlay. 
         DepthOfDefect = EXPECTED_HEIGHT - (MinPixels[n]/SCALE_FACTOR)
         DepthString = "{DepthOfDefect:.2f} mm".format(DepthOfDefect=DepthOfDefect)
         print("Defect #{n}: depth = {DepthOfDefect:.2f} mm\n".format(n=n, DepthOfDefect=DepthOfDefect))
         AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, CogX[n] + TEXT_H_OFFSET_1,
                                          CogY[n] + TEXT_V_OFFSET_1, "Defect depth")
         AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, CogX[n] + TEXT_H_OFFSET_2,
                                                      CogY[n] + TEXT_V_OFFSET_2, DepthString)
   else:
      DepthOfDefect = EXPECTED_HEIGHT - (MinPixels/SCALE_FACTOR)
      DepthString = "{DepthOfDefect:.2f} mm".format(DepthOfDefect=DepthOfDefect)
      print("Defect #0: depth = {DepthOfDefect:.2f} mm\n".format(DepthOfDefect=DepthOfDefect))
      AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, CogX + TEXT_H_OFFSET_1,
                                       CogY + TEXT_V_OFFSET_1, "Defect depth")
      AIL.MgraText(AIL.M_DEFAULT, AilOverlayImage, CogX + TEXT_H_OFFSET_2,
                                                   CogY + TEXT_V_OFFSET_2, DepthString)

   # Free all allocations. 
   AIL.MblobFree(AilBlobResult)
   AIL.MblobFree(AilBlobContext)
   AIL.MbufFree(AilBinImage)

# Color constants for display LUT. 
BLUE_HUE = 171.0           # Expected depths will be blue.   
RED_HUE  = 0.0             # Worst defects will be red.      
FULL_SATURATION = 255      # All colors are fully saturated. 
HALF_LUMINANCE  = 128      # All colors have half luminance. 

# Creates a color display LUT to show defects in red. 
def SetupColorDisplay(AilSystem, AilDisplay, SizeBit):
   # Number of possible gray levels in corrected depth map. 
   NbGrayLevels = 1 << SizeBit

   # Allocate 1-band LUT that will contain hue values. 
   AilRampLut1Band = AIL.MbufAlloc1d(AilSystem, NbGrayLevels, 8 + AIL.M_UNSIGNED, AIL.M_LUT)

   # Compute limit gray values. 
   DefectGrayLevel   = int((EXPECTED_HEIGHT - SATURATED_DEFECT) * SCALE_FACTOR)
   ExpectedGrayLevel = int(EXPECTED_HEIGHT * SCALE_FACTOR)

   # Create hue values for each possible gray level. 
   AIL.MgenLutRamp(AilRampLut1Band, 0, RED_HUE, DefectGrayLevel, RED_HUE)
   AIL.MgenLutRamp(AilRampLut1Band, DefectGrayLevel, RED_HUE, ExpectedGrayLevel, BLUE_HUE)
   AIL.MgenLutRamp(AilRampLut1Band, ExpectedGrayLevel, BLUE_HUE, NbGrayLevels-1, BLUE_HUE)

   # Create a HSL image buffer. 
   AilColorImage = AIL.MbufAllocColor(AilSystem, 3, NbGrayLevels, 1, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE)
   AIL.MbufClear(AilColorImage, AIL.M_RGB888(0, FULL_SATURATION, HALF_LUMINANCE))

   # Set its H band (hue) to the LUT contents and convert the image to RGB. 
   AIL.MbufCopyColor2d(AilRampLut1Band, AilColorImage, 0, 0, 0, 0, 0, 0, NbGrayLevels, 1)
   AIL.MimConvert(AilColorImage, AilColorImage, AIL.M_HSL_TO_RGB)

   # Create RGB LUT to give to display and copy image contents. 
   AilRampLut3Band = AIL.MbufAllocColor(AilSystem, 3, NbGrayLevels, 1, 8 + AIL.M_UNSIGNED, AIL.M_LUT)
   AIL.MbufCopy(AilColorImage, AilRampLut3Band)

   # Associates LUT to display. 
   AIL.MdispLut(AilDisplay, AilRampLut3Band)

   # Free all allocations. 
   AIL.MbufFree(AilRampLut1Band)
   AIL.MbufFree(AilRampLut3Band)
   AIL.MbufFree(AilColorImage)
   
#***************************************************************************
# Calibrated camera example.
#***************************************************************************

# Input sequence specifications. 
GRID_FILENAME          = os.path.join(AIL.M_IMAGE_PATH, "GridForLaser.mim")
LASERLINE_FILENAME     = os.path.join(AIL.M_IMAGE_PATH, "LaserLine.mim")
OBJECT2_SEQUENCE_FILE  = os.path.join(AIL.M_IMAGE_PATH, "Cookie.avi")

# Camera calibration grid parameters. 
GRID_NB_ROWS           =  13
GRID_NB_COLS           =  12
GRID_ROW_SPACING       =  5.0     # in mm                
GRID_COL_SPACING       =  5.0     # in mm                

# Laser device setup parameters. 
CONVEYOR_SPEED         =  -0.2     # in mm/frame          

# Fully corrected depth map generation parameters. 
DEPTH_MAP_SIZE_X       =  480      # in pixels            
DEPTH_MAP_SIZE_Y       =  480      # in pixels            
GAP_DEPTH              =  1.5      # in mm                

# Peak detection parameters. 
PEAK_WIDTH_NOMINAL_2      =  9
PEAK_WIDTH_DELTA_2        =  7
MIN_CONTRAST_2            = 75

# Everything below this is considered as noise. 
MIN_HEIGHT_THRESHOLD = 1.0 # in mm 

def CalibratedCameraExample(AilSystem, AilDisplay):
   print("\n3D PROFILING AND VOLUME ANALYSIS:")
   print("---------------------------------\n")
   print("This program generates fully corrected 3D data of a")
   print("scanned cookie and computes its volume.")
   print("The laser (sheet-of-light) profiling system uses a")
   print("3d-calibrated camera.\n")

   # Load grid image for camera calibration.
   AilImage = AIL.MbufRestore(GRID_FILENAME, AilSystem)

   # Select display.
   AIL.MdispSelect(AilDisplay, AilImage)

   print("Calibrating the camera...\n")

   SizeX = AIL.MbufInquire(AilImage, AIL.M_SIZE_X)
   SizeY = AIL.MbufInquire(AilImage, AIL.M_SIZE_Y)

   # Allocate calibration context in 3D mode.
   AilCalibration = AIL.McalAlloc(AilSystem, AIL.M_TSAI_BASED, AIL.M_DEFAULT)

   # Calibrate the camera.
   AIL.McalGrid(AilCalibration, AilImage, 0.0, 0.0, 0.0, GRID_NB_ROWS, GRID_NB_COLS,
            GRID_ROW_SPACING, GRID_COL_SPACING, AIL.M_DEFAULT, AIL.M_CHESSBOARD_GRID)

   CalibrationStatus = AIL.McalInquire(AilCalibration, AIL.M_CALIBRATION_STATUS + AIL.M_TYPE_AIL_INT)
   
   if CalibrationStatus != AIL.M_CALIBRATED:
      AIL.McalFree(AilCalibration)
      AIL.MbufFree(AilImage)
      print("Camera calibration failed.")
      print("Press any key to end.\n")
      AIL.MosGetch()
      return

   # Prepare for overlay annotations.
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN)

   # Draw camera calibration points.
   AIL.McalDraw(AIL.M_DEFAULT, AilCalibration, AilOverlayImage, AIL.M_DRAW_IMAGE_POINTS,
                                                                     AIL.M_DEFAULT, AIL.M_DEFAULT)

   print("The camera was calibrated using a chessboard grid.\n")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Disable overlay.
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_DISABLE)

   # Load laser line image.
   AIL.MbufLoad(LASERLINE_FILENAME, AilImage)

   # Allocate 3dmap objects.
   AilLaser = AIL.M3dmapAlloc(AilSystem, AIL.M_LASER, AIL.M_CALIBRATED_CAMERA_LINEAR_MOTION)
   AilCalibScan = AIL.M3dmapAllocResult(AilSystem, AIL.M_LASER_CALIBRATION_DATA, AIL.M_DEFAULT)

   # Set laser line extraction options.
   AilPeakLocator = AIL.M3dmapInquire(AilLaser, AIL.M_DEFAULT, AIL.M_LOCATE_PEAK_1D_CONTEXT_ID + AIL.M_TYPE_AIL_ID)
   AIL.MimControl(AilPeakLocator, AIL.M_PEAK_WIDTH_NOMINAL, PEAK_WIDTH_NOMINAL_2)
   AIL.MimControl(AilPeakLocator, AIL.M_PEAK_WIDTH_DELTA  , PEAK_WIDTH_DELTA_2  )
   AIL.MimControl(AilPeakLocator, AIL.M_MINIMUM_CONTRAST  , MIN_CONTRAST_2      )

   # Calibrate laser profiling context.
   AIL.M3dmapAddScan(AilLaser, AilCalibScan, AilImage, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AIL.M3dmapCalibrate(AilLaser, AilCalibScan, AilCalibration, AIL.M_DEFAULT)

   print("The laser profiling system has been calibrated using the image")
   print("of one laser line.\n")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Free the result buffer use for calibration as it will not be used anymore.
   AIL.M3dmapFree(AilCalibScan)
   AilCalibScan = AIL.M_NULL

   # Allocate the result buffer to hold the scanned 3D points.
   AilScan = AIL.M3dmapAllocResult(AilSystem, AIL.M_POINT_CLOUD_RESULT, AIL.M_DEFAULT)

   # Set speed of scanned object (speed in mm/frame is constant).
   AIL.M3dmapControl(AilLaser, AIL.M_DEFAULT, AIL.M_SCAN_SPEED, CONVEYOR_SPEED)

   # Inquire characteristics of the input sequence.
   NumberOfImages = AIL.MbufDiskInquire(OBJECT2_SEQUENCE_FILE, AIL.M_NUMBER_OF_IMAGES)
   FrameRate = AIL.MbufDiskInquire(OBJECT2_SEQUENCE_FILE, AIL.M_FRAME_RATE)

   # Open the object sequence file for reading.
   AIL.MbufImportSequence(OBJECT2_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                           AIL.M_NULL, AIL.M_OPEN)

   print("The cookie is being scanned to generate 3D data.\n")

   # Read and process all images in the input sequence.
   StartTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)

   for n in range(NumberOfImages):
      # Read image from sequence. 
      AIL.MbufImportSequence(OBJECT2_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_LOAD, AIL.M_NULL, AilImage,
                                                                     AIL.M_DEFAULT, 1, AIL.M_READ)

      # Analyze the image to extract laser line and correct its depth. 
      AIL.M3dmapAddScan(AilLaser, AilScan, AilImage, AIL.M_NULL, AIL.M_NULL, AIL.M_POINT_CLOUD_LABEL(1), AIL.M_DEFAULT)

      # Wait to have a proper frame rate, if necessary. 
      EndTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)
      WaitTime = (1.0/FrameRate) - (EndTime - StartTime)
      if WaitTime > 0:
         AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_WAIT, WaitTime)
      StartTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)

   # Close the object sequence file. 
   AIL.MbufImportSequence(OBJECT2_SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL,
                                                                          AIL.M_NULL, AIL.M_CLOSE)

   # Convert to M_CONTAINER for 3D processing. 
   AilContainerId = AIL.MbufAllocContainer(AilSystem, AIL.M_PROC | AIL.M_DISP, AIL.M_DEFAULT)
   AIL.M3dmapCopyResult(AilScan, AIL.M_ALL, AilContainerId, AIL.M_POINT_CLOUD_UNORGANIZED, AIL.M_DEFAULT)

   # The container's reflectance is 16bits, but only uses the bottom 8.Set the maximum value to display it properly.
   AIL.MbufControlContainer(AilContainerId, AIL.M_COMPONENT_REFLECTANCE, AIL.M_MAX, 255)

   # Allocate image for the fully corrected depth map. 
   AilDepthMap = AIL.MbufAlloc2d(AilSystem, DEPTH_MAP_SIZE_X, DEPTH_MAP_SIZE_Y, 16 + AIL.M_UNSIGNED,
               AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP)

   # Include all points during depth map generation. 
   AIL.M3dimCalibrateDepthMap(AilContainerId, AilDepthMap, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_NEGATIVE, AIL.M_DEFAULT)

   # Remove noise in the container close to the Z = 0. 
   AilPlane = AIL.M3dgeoAlloc(AilSystem, AIL.M_GEOMETRY, AIL.M_DEFAULT)
   AIL.M3dgeoPlane(AilPlane, AIL.M_COEFFICIENTS, 0.0, 0.0, 1.0, MIN_HEIGHT_THRESHOLD, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # M_INVERSE remove what is above the plane. 
   AIL.M3dimCrop(AilContainerId, AilContainerId, AilPlane, AIL.M_NULL, AIL.M_SAME, AIL.M_INVERSE)
   AIL.M3dgeoFree(AilPlane)

   print("Fully corrected 3D data of the cookie is displayed.\n")

   M3dDisplay = Alloc3dDisplayId(AilSystem)
   if M3dDisplay:
      print("Press <R> on the display window to stop/start the rotation.\n")
      AIL.M3ddispSelect(M3dDisplay, AilContainerId, AIL.M_SELECT, AIL.M_DEFAULT)
      AIL.M3ddispSetView(M3dDisplay, AIL.M_AUTO, AIL.M_BOTTOM_TILTED, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
      AIL.M3ddispControl(M3dDisplay, AIL.M_AUTO_ROTATE, AIL.M_ENABLE)

   # Get fully corrected depth map from accumulated information in the result buffer. 
   AIL.M3dimProject(AilContainerId, AilDepthMap, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_MIN_Z, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Set fill gaps parameters. 
   FillGapsContext = AIL.M3dimAlloc(AilSystem, AIL.M_FILL_GAPS_CONTEXT, AIL.M_DEFAULT)
   AIL.M3dimControl(FillGapsContext, AIL.M_FILL_MODE,                  AIL.M_X_THEN_Y)
   AIL.M3dimControl(FillGapsContext, AIL.M_FILL_SHARP_ELEVATION,       AIL.M_MIN)
   AIL.M3dimControl(FillGapsContext, AIL.M_FILL_SHARP_ELEVATION_DEPTH, GAP_DEPTH)
   AIL.M3dimControl(FillGapsContext, AIL.M_FILL_BORDER,                AIL.M_DISABLE)

   AIL.M3dimFillGaps(FillGapsContext, AilDepthMap, AIL.M_NULL, AIL.M_DEFAULT)

   # Compute the volume of the depth map. 
   Volume, StatusPtr = AIL.M3dmetVolume(AilDepthMap, AIL.M_XY_PLANE, AIL.M_TOTAL,  AIL.M_DEFAULT)

   print("Volume of the cookie is {Volume:.1f} cm^3.\n".format(Volume=(Volume / 1000.0)))
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free all allocations. 
   if (M3dDisplay):
      AIL.M3ddispFree(M3dDisplay)
   AIL.M3dimFree(FillGapsContext)
   AIL.MbufFree(AilContainerId)
   AIL.M3dmapFree(AilScan)
   AIL.M3dmapFree(AilLaser)
   AIL.McalFree(AilCalibration)
   AIL.MbufFree(AilDepthMap)
   AIL.MbufFree(AilImage)

#*****************************************************************************
# Allocates a 3D display and returns its identifier.  
#*****************************************************************************
def Alloc3dDisplayId(AilSystem):
   AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE)
   AilDisplay3D = AIL.M3ddispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, AIL.M_NULL)
   AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE)

   if not AilDisplay3D:
      print("\nThe current system does not support the 3D display.\n")
   return AilDisplay3D


if __name__ == "__main__":
   M3dMapExample()

```
