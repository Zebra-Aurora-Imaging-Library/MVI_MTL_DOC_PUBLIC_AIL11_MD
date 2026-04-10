---
title: "Mcol"
description: "This program uses the color module to perform color segmentation, color identification, color separation and color to grayscale conversion."
ms.language: "Python"
---

# Mcol

> This program uses the color module to perform color segmentation, color identification, color separation and color to grayscale conversion.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufChild2d`, `MbufChildColor2d`, `MbufClear`, `MbufCopy`, `MbufDiskInquire`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufPut2d`, `McolAlloc`, `McolAllocResult`, `McolControl`, `McolDefine`, `McolDraw`, `McolFree`, `McolGetResult`, `McolInquire`, `McolMatch`, `McolPreprocess`, `McolProject`, `McolSetMethod`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraArcFill`, `MgraFont`, `MgraRect`, `MgraRectFill`, `MgraText`, `MimConvert`, `MmodAllocResult`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodGetResult`, `MmodPreprocess`, `MmodRestore`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Food and Beverage, Electronics, Packaging, Applications, Inspecting/Proofing/Verifying, Identifying, Finding/Locating, Modules, Buffer, Display, Graphics, Image Processing, Geometric Model Finder, Color Analysis, What's New, AIL 10.0 SP4, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#**************************************************************************************
# 
# File name: Mcol.py  
#
# Synopsis:  This program contains 4 examples using the Color module.
#
#            The first example performs color segmentation of an image
#            by classifying each pixel with one out of 6 color samples.
#            The ratio of each color in the image is then calculated.
#
#            The second example performs color matching of circular regions
#            in objects located with model finder.
#
#            The third example performs color separation in order to
#            separate 2 types of ink on a piece of paper.
#
#            The fourth example performs a color to grayscale conversion
#            using the principal component projection functionality.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#

import ail11 as AIL
import os

# Display image margin 
DISPLAY_CENTER_MARGIN_X  = 5
DISPLAY_CENTER_MARGIN_Y  = 5

# Color patch sizes 
COLOR_PATCH_SIZEX        = 30
COLOR_PATCH_SIZEY        = 40

#****************************************************************************
# Main.
#****************************************************************************

def McolExample():
   
   # Allocate defaults. 
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Run the color segmentation example. 
   ColorSegmentationExample(AilSystem, AilDisplay)

   # Run the color matching example. 
   ColorMatchingExample(AilSystem, AilDisplay)

   # Run the color projection example. 
   ColorSeparationExample(AilSystem, AilDisplay)

   # Run the RGB to grayscale conversion example. 
   RGBtoGrayscaleExample(AilSystem, AilDisplay)

   # Free defaults.     
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0
   
#***************************************************************************
# Color Segmentation using color samples. 
#***************************************************************************

# Image filenames 
CANDY_SAMPLE_IMAGE_FILE   = os.path.join(AIL.M_IMAGE_PATH, "CandySamples.mim")
CANDY_TARGET_IMAGE_FILE   = os.path.join(AIL.M_IMAGE_PATH, "Candy.mim")

# Number of samples 
NUM_SAMPLES               = 6

# Draw spacing and offset 
CANDY_SAMPLES_XSPACING    = 35
CANDY_SAMPLES_YOFFSET     = 145

# Match parameters 
MATCH_MODE                = AIL.M_MIN_DIST_VOTE # Minimal distance vote mode.        
DISTANCE_TYPE             = AIL.M_MAHALANOBIS   # Mahalanobis distance.              
TOLERANCE_MODE            = AIL.M_SAMPLE_STDDEV # Standard deviation tolerance mode. 
TOLERANCE_VALUE           = 6.0             # Mahalanobis tolerance value.       
RED_TOLERANCE_VALUE       = 6.0
YELLOW_TOLERANCE_VALUE    = 12.0
PINK_TOLERANCE_VALUE      = 5.0

def ColorSegmentationExample(AilSystem, AilDisplay):

   # Blank spaces to align the samples names evenly. 
   Spaces = ["", " ", "  ", "   "] 

   # Color samples names. 
   SampleNames = ["Green", "Red", "Yellow", "Purple", "Blue", "Pink"]

   # Color samples position: OffsetX, OffsetY  
   SamplesROI = [[58, 143], [136, 148], [217, 144], [295, 142], [367, 143], [442, 147]]

   # Color samples size. 
   SampleSizeX = 36
   SampleSizeY = 32

   print("\nCOLOR SEGMENTATION:")
   print("-------------------")

   # Allocate the parent display image.   
   SourceSizeX = AIL.MbufDiskInquire(CANDY_SAMPLE_IMAGE_FILE, AIL.M_SIZE_X)
   SourceSizeY = AIL.MbufDiskInquire(CANDY_SAMPLE_IMAGE_FILE, AIL.M_SIZE_Y)
   DisplayImage = AIL.MbufAllocColor(AilSystem, 3, 2*SourceSizeX + DISPLAY_CENTER_MARGIN_X, SourceSizeY, 8+AIL.M_UNSIGNED, AIL.M_IMAGE+AIL.M_DISP+AIL.M_PROC)
   AIL.MbufClear(DisplayImage, AIL.M_COLOR_BLACK)

   # Create a source and dest child in the display image. 
   SourceChild = AIL.MbufChild2d(DisplayImage, 0, 0, SourceSizeX, SourceSizeY)
   DestChild = AIL.MbufChild2d(DisplayImage, SourceSizeX + DISPLAY_CENTER_MARGIN_X, 0, SourceSizeX, SourceSizeY)

   # Load the source image into the source child. 
   AIL.MbufLoad(CANDY_SAMPLE_IMAGE_FILE, SourceChild)
  
   # Allocate a color matching context. 
   MatchContext = AIL.McolAlloc(AilSystem, AIL.M_COLOR_MATCHING, AIL.M_RGB, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Define each color sample in the context. 
   for i in range(0, NUM_SAMPLES):
      AIL.McolDefine(MatchContext, SourceChild, AIL.M_SAMPLE_LABEL(i+1), AIL.M_IMAGE, SamplesROI[i][0], SamplesROI[i][1], SampleSizeX, SampleSizeY)

   # Set the color matching parameters. 
   AIL.McolSetMethod(MatchContext, MATCH_MODE, DISTANCE_TYPE, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AIL.McolControl(MatchContext, AIL.M_CONTEXT, AIL.M_DISTANCE_TOLERANCE_MODE, TOLERANCE_MODE)
   AIL.McolControl(MatchContext, AIL.M_ALL, AIL.M_DISTANCE_TOLERANCE, TOLERANCE_VALUE)

   # Adjust tolerances for the red, yellow and pink samples. 
   AIL.McolControl(MatchContext, AIL.M_SAMPLE_INDEX(1), AIL.M_DISTANCE_TOLERANCE, RED_TOLERANCE_VALUE)
   AIL.McolControl(MatchContext, AIL.M_SAMPLE_INDEX(2), AIL.M_DISTANCE_TOLERANCE, YELLOW_TOLERANCE_VALUE)
   AIL.McolControl(MatchContext, AIL.M_SAMPLE_INDEX(5), AIL.M_DISTANCE_TOLERANCE, PINK_TOLERANCE_VALUE)

   # Preprocess the context. 
   AIL.McolPreprocess(MatchContext, AIL.M_DEFAULT) 

   # Fill the samples colors array. 
   SampleMatchColor = []
   for i in range(0, NUM_SAMPLES):
      SampleColor = 3*[0]
      SampleColor[0] = AIL.McolInquire(MatchContext, AIL.M_SAMPLE_LABEL(i+1), AIL.M_MATCH_SAMPLE_COLOR_BAND_0 + AIL.M_TYPE_AIL_INT)
      SampleColor[1] = AIL.McolInquire(MatchContext, AIL.M_SAMPLE_LABEL(i+1), AIL.M_MATCH_SAMPLE_COLOR_BAND_1 + AIL.M_TYPE_AIL_INT)
      SampleColor[2] = AIL.McolInquire(MatchContext, AIL.M_SAMPLE_LABEL(i+1), AIL.M_MATCH_SAMPLE_COLOR_BAND_2 + AIL.M_TYPE_AIL_INT)
      SampleMatchColor.append(SampleColor)
      

   # Draw the samples. 
   DrawSampleColors(DestChild, SampleMatchColor, SampleNames, NUM_SAMPLES, CANDY_SAMPLES_XSPACING, CANDY_SAMPLES_YOFFSET)

   # Select the image buffer for display. 
   AIL.MdispSelect(AilDisplay, DisplayImage)

   # Pause to show the original image. 
   print("Color samples are defined for each possible candy color.")
   print("Press any key to do color matching.\n")
   AIL.MosGetch()

   # Load the target image.
   AIL.MbufClear(DisplayImage, AIL.M_COLOR_BLACK)
   AIL.MbufLoad(CANDY_TARGET_IMAGE_FILE, SourceChild)

   # Allocate a color matching result buffer. 
   MatchResult = AIL.McolAllocResult(AilSystem, AIL.M_COLOR_MATCHING_RESULT, AIL.M_DEFAULT)

   # Enable controls to draw the labeled color image. 
   AIL.McolControl(MatchContext, AIL.M_CONTEXT, AIL.M_GENERATE_PIXEL_MATCH, AIL.M_ENABLE)
   AIL.McolControl(MatchContext, AIL.M_CONTEXT, AIL.M_GENERATE_SAMPLE_COLOR_LUT, AIL.M_ENABLE)

   # Match with target image. 
   AIL.McolMatch(MatchContext, SourceChild, AIL.M_DEFAULT, AIL.M_NULL, MatchResult, AIL.M_DEFAULT)

   # Retrieve and display results.    
   print("Each pixel of the mixture is matched with one of the color samples.\n")
   print("Color segmentation results:")
   print("---------------------------")

   for SampleIndex in range(0, NUM_SAMPLES):      
      MatchScore = AIL.McolGetResult(MatchResult, AIL.M_DEFAULT, AIL.M_SAMPLE_INDEX(SampleIndex), AIL.M_SCORE)
      SpacesIndex = 6 - len(SampleNames[SampleIndex])
      print("Ratio of {:s}{:s} sample = {:5.2f}%".format(SampleNames[SampleIndex], Spaces[SpacesIndex], MatchScore[0]))
      
   print("\nResults reveal the low proportion of Blue candy.\n")

   # Draw the colored label image in the destination child. 
   AIL.McolDraw(AIL.M_DEFAULT, MatchResult, DestChild, AIL.M_DRAW_PIXEL_MATCH_USING_COLOR, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   # Pause to show the result image. 
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.MbufFree(DestChild)
   AIL.MbufFree(SourceChild)
   AIL.MbufFree(DisplayImage)
   AIL.McolFree(MatchResult)
   AIL.McolFree(MatchContext)
   

#****************************************************************************
# Color matching in labeled regions.
#****************************************************************************
# Image filenames 
FUSE_SAMPLES_IMAGE       = os.path.join(AIL.M_IMAGE_PATH, "FuseSamples.mim")
FUSE_TARGET_IMAGE        = os.path.join(AIL.M_IMAGE_PATH, "Fuse.mim")

# Model Finder context filename 
FINDER_CONTEXT           = os.path.join(AIL.M_IMAGE_PATH, "FuseModel.mmf")

# Number of fuse sample objects 
NUM_FUSES                = 4

# Draw spacing and offset 
FUSE_SAMPLES_XSPACING    = 40
FUSE_SAMPLES_YOFFSET     = 145

def ColorMatchingExample(AilSystem, AilDisplay):
   
   # Color sample names 
   SampleNames = ["Green", " Blue", " Red", "Yellow"]

   # Sample ROIs coordinates: OffsetX, OffsetY, SizeX, SizeY 
   SampleROIs =  [[54, 139, 28, 14],
                 [172, 137, 30, 23],
                 [296, 135, 31, 23],
                 [417, 134, 27, 22]]

   # Array of match sample colors. 
   SampleMatchColor = NUM_FUSES*3*[0]

   print("\nCOLOR IDENTIFICATION:")
   print("---------------------")

   # Allocate the parent display image. 
   SizeX = AIL.MbufDiskInquire(FUSE_TARGET_IMAGE, AIL.M_SIZE_X)
   SizeY = AIL.MbufDiskInquire(FUSE_TARGET_IMAGE, AIL.M_SIZE_Y)   
   DisplayImage = AIL.MbufAllocColor(AilSystem, 3, 2*SizeX + DISPLAY_CENTER_MARGIN_X, SizeY, 8+AIL.M_UNSIGNED, AIL.M_IMAGE+AIL.M_DISP+AIL.M_PROC)
   AIL.MbufClear(DisplayImage, AIL.M_COLOR_BLACK)

   # Allocate the model, area and label images. 
   ModelImage = AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8+AIL.M_UNSIGNED, AIL.M_IMAGE+AIL.M_PROC+AIL.M_DISP)
   AreaImage = AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8+AIL.M_UNSIGNED, AIL.M_IMAGE+AIL.M_PROC+AIL.M_DISP)

   # Create a source and destination child in the display image. 
   SourceChild = AIL.MbufChild2d(DisplayImage, 0, 0, SizeX, SizeY)
   DestChild = AIL.MbufChild2d(DisplayImage, SizeX + DISPLAY_CENTER_MARGIN_X, 0, SizeX, SizeY)

   # Load the sample source image. 
   AIL.MbufLoad(FUSE_SAMPLES_IMAGE, SourceChild)

   # Display the image buffer. 
   AIL.MdispSelect(AilDisplay, DisplayImage)

   # Prepare the overlay. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)
   OverlayID = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)
   OverlaySourceChild = AIL.MbufChild2d(OverlayID, 0, 0, SizeX, SizeY)
   OverlayDestChild = AIL.MbufChild2d(OverlayID, SizeX + DISPLAY_CENTER_MARGIN_X, 0, SizeX, SizeY)

   # Prepare the model finder context and result. 
   FuseFinderCtx = AIL.MmodRestore(FINDER_CONTEXT, AilSystem, AIL.M_DEFAULT)   
   AIL.MmodPreprocess(FuseFinderCtx, AIL.M_DEFAULT)
   FuseFinderRes = AIL.MmodAllocResult(AilSystem, AIL.M_DEFAULT)

   # Allocate a color match context and result. 
   ColMatchContext = AIL.McolAlloc(AilSystem, AIL.M_COLOR_MATCHING, AIL.M_RGB, AIL.M_DEFAULT, AIL.M_DEFAULT)
   ColMatchResult = AIL.McolAllocResult(AilSystem, AIL.M_COLOR_MATCHING_RESULT, AIL.M_DEFAULT)

   # Define the color samples in the context. 
   for i in range(0, NUM_FUSES):
      
      AIL.McolDefine(ColMatchContext, SourceChild, AIL.M_SAMPLE_LABEL(i+1), AIL.M_IMAGE, 
                  SampleROIs[i][0], 
                  SampleROIs[i][1], 
                  SampleROIs[i][2],
                  SampleROIs[i][3])
      

   # Preprocess the context. 
   AIL.McolPreprocess(ColMatchContext, AIL.M_DEFAULT)
   
   # Fill the samples colors array. 
   SampleMatchColor = []
   for i in range(0, NUM_FUSES):
      SampleColor = 3*[0]
      SampleColor[0] = AIL.McolInquire(ColMatchContext, AIL.M_SAMPLE_LABEL(i+1), AIL.M_MATCH_SAMPLE_COLOR_BAND_0 + AIL.M_TYPE_AIL_INT)
      SampleColor[1] = AIL.McolInquire(ColMatchContext, AIL.M_SAMPLE_LABEL(i+1), AIL.M_MATCH_SAMPLE_COLOR_BAND_1 + AIL.M_TYPE_AIL_INT)
      SampleColor[2] = AIL.McolInquire(ColMatchContext, AIL.M_SAMPLE_LABEL(i+1), AIL.M_MATCH_SAMPLE_COLOR_BAND_2 + AIL.M_TYPE_AIL_INT)
      SampleMatchColor.append(SampleColor)
      

   # Draw the color samples. 
   DrawSampleColors(DestChild, SampleMatchColor, SampleNames, 
                    NUM_FUSES, FUSE_SAMPLES_XSPACING, FUSE_SAMPLES_YOFFSET)
  
   # Draw the sample ROIs in the source image overlay. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED)
   for SampleIndex in range(0, NUM_FUSES):
      
      XEnd = SampleROIs[SampleIndex][0] + SampleROIs[SampleIndex][2] - 1
      YEnd = SampleROIs[SampleIndex][1] + SampleROIs[SampleIndex][3] - 1
      AIL.MgraRect(AIL.M_DEFAULT, OverlaySourceChild, SampleROIs[SampleIndex][0], 
                                              SampleROIs[SampleIndex][1], 
                                              XEnd, YEnd)
      

   # Pause to show the source image. 
   print("Colors are defined using one color sample region per fuse.")
   print("Press any key to process the target image.\n")
   AIL.MosGetch()

   # Clear the overlay. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)
   
   # Disable the display update. 
   AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_DISABLE)

   # Load the target image into the source child. 
   AIL.MbufLoad(FUSE_TARGET_IMAGE, SourceChild)

   # Get the grayscale model image and copy it into the display dest child. 
   AIL.MimConvert(SourceChild, ModelImage, AIL.M_RGB_TO_L)
   AIL.MbufCopy(ModelImage, DestChild)

   # Find the Model. 
   AIL.MmodFind(FuseFinderCtx, ModelImage, FuseFinderRes)   

   # Draw the blob image: labeled circular areas centered at each found fuse occurrence. 
   Number = AIL.MmodGetResult(FuseFinderRes, AIL.M_DEFAULT, AIL.M_NUMBER+AIL.M_TYPE_AIL_INT)
   AIL.MbufClear(AreaImage, 0)
   for i in range(0, Number):
      # Get the position 
      X = AIL.MmodGetResult(FuseFinderRes, i, AIL.M_POSITION_X)
      Y = AIL.MmodGetResult(FuseFinderRes, i, AIL.M_POSITION_Y)
      # Set the label color 
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, float(i+1))
      # Draw the filled circle 
      AIL.MgraArcFill(AIL.M_DEFAULT, AreaImage, X, Y, 20, 20, 0, 360)
      

   # Enable controls to draw the labeled color image. 
   AIL.McolControl(ColMatchContext, AIL.M_CONTEXT, AIL.M_SAVE_AREA_IMAGE, AIL.M_ENABLE)
   AIL.McolControl(ColMatchContext, AIL.M_CONTEXT, AIL.M_GENERATE_SAMPLE_COLOR_LUT, AIL.M_ENABLE)

   # Perform the color matching. 
   AIL.McolMatch(ColMatchContext, SourceChild, AIL.M_DEFAULT, AreaImage, ColMatchResult, AIL.M_DEFAULT)

   # Draw the label image into the overlay child. 
   AIL.McolDraw(AIL.M_DEFAULT, ColMatchResult, OverlayDestChild, 
                   AIL.M_DRAW_AREA_MATCH_USING_COLOR, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   # Draw the model position over the colored areas. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_BLUE)
   AIL.MmodDraw(AIL.M_DEFAULT, FuseFinderRes, OverlayDestChild, AIL.M_DRAW_BOX+AIL.M_DRAW_POSITION, 
            AIL.M_ALL, AIL.M_DEFAULT)

   # Enable the display update. 
   AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_ENABLE)
   
   # Pause to show the resulting image. 
   print("Fuses are located using the Model Finder tool.")
   print("The color of each target area is identified.")
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.MmodFree(FuseFinderRes)
   AIL.MmodFree(FuseFinderCtx)
   AIL.MbufFree(AreaImage)
   AIL.MbufFree(ModelImage)
   AIL.MbufFree(SourceChild)
   AIL.MbufFree(DestChild)
   AIL.MbufFree(OverlaySourceChild)
   AIL.MbufFree(OverlayDestChild)
   AIL.MbufFree(DisplayImage)   
   AIL.McolFree(ColMatchContext)
   AIL.McolFree(ColMatchResult)
   

#****************************************************************************
# Perform color separation of colored inks on a piece of paper.
#****************************************************************************
# Source image 
WRITING_IMAGE_FILE = os.path.join(AIL.M_IMAGE_PATH, "stamp.mim")

# Color triplets 
BACKGROUND_COLOR = [245, 234, 206]         
WRITING_COLOR    = [141, 174, 174]         
STAMP_COLOR      = [226, 150, 118]         

# Drawing spacing 
PATCHES_XSPACING   = 70

def ColorSeparationExample(AilSystem, AilDisplay):

   # Color samples' names 
   ColorNames = ["BACKGROUND", "WRITING", "STAMP"] 

   # Array with color patches to draw. 
   Colors =  [BACKGROUND_COLOR, WRITING_COLOR, STAMP_COLOR]

   # Samples' color coordinates 
   BackgroundColor = BACKGROUND_COLOR
   SelectedColor   = WRITING_COLOR
   RejectedColor   = STAMP_COLOR

   print("\nCOLOR SEPARATION:")
   print("-----------------")

   # Allocate an array buffer and fill it with the color coordinates. 
   ColorsArray = AIL.MbufAlloc2d(AilSystem, 3, 3, 8+AIL.M_UNSIGNED, AIL.M_ARRAY)
   AIL.MbufPut2d(ColorsArray, 0, 0, 3, 1, BackgroundColor)
   AIL.MbufPut2d(ColorsArray, 0, 1, 3, 1, SelectedColor)
   AIL.MbufPut2d(ColorsArray, 0, 2, 3, 1, RejectedColor)

   # Allocate the parent display image.    
   SourceSizeX = AIL.MbufDiskInquire(WRITING_IMAGE_FILE, AIL.M_SIZE_X)
   SourceSizeY = AIL.MbufDiskInquire(WRITING_IMAGE_FILE, AIL.M_SIZE_Y)
   DisplayImage = AIL.MbufAllocColor(AilSystem, 
                  3, 
                  2*SourceSizeX + DISPLAY_CENTER_MARGIN_X, 
                  SourceSizeY, 
                  8+AIL.M_UNSIGNED, 
                  AIL.M_IMAGE+AIL.M_DISP+AIL.M_PROC)
   AIL.MbufClear(DisplayImage, AIL.M_COLOR_BLACK)

   # Clear the overlay. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_DEFAULT)   

   # Create a source and dest child in the display image 
   SourceChild = AIL.MbufChild2d(DisplayImage, 0, 0, SourceSizeX, SourceSizeY)
   DestChild = AIL.MbufChild2d(DisplayImage, SourceSizeX + DISPLAY_CENTER_MARGIN_X, 0, SourceSizeX, SourceSizeY)

   # Load the source image into the display image source child. 
   AIL.MbufLoad(WRITING_IMAGE_FILE, SourceChild)
     
   # Draw the color patches. 
   DrawSampleColors(DestChild, Colors, ColorNames, 3, PATCHES_XSPACING, -1)

   # Display the image. 
   AIL.MdispSelect(AilDisplay, DisplayImage)

   # Pause to show the source image and color patches. 
   print("The writing will be separated from the stamp using the following triplets:")
   print("the background color: beige [{}, {}, {}],".format(int(BackgroundColor[0]), int(BackgroundColor[1]), int(BackgroundColor[2])))
   print("the writing color   : green [{}, {}, {}],".format(int(SelectedColor[0]), int(SelectedColor[1]), int(SelectedColor[2])))
   print("the stamp color     : red   [{}, {}, {}].\n".format(int(RejectedColor[0]), int(RejectedColor[1]), int(RejectedColor[2])))
   print("Press any key to extract the writing.\n")
   AIL.MosGetch()

   # Perform the color projection. 
   AIL.McolProject(SourceChild, ColorsArray, DestChild, AIL.M_NULL, AIL.M_COLOR_SEPARATION, AIL.M_DEFAULT, AIL.M_NULL)

   # Wait for a key. 
   print("Press any key to extract the stamp.\n")
   AIL.MosGetch()

   # Switch the order of the selected vs rejected colors in the color array. 
   AIL.MbufPut2d(ColorsArray, 0, 2, 3, 1, SelectedColor)
   AIL.MbufPut2d(ColorsArray, 0, 1, 3, 1, RejectedColor)   

   # Perform the color projection. 
   AIL.McolProject(SourceChild, ColorsArray, DestChild, AIL.M_NULL, AIL.M_COLOR_SEPARATION, AIL.M_DEFAULT, AIL.M_NULL)

   # Wait for a key. 
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.MbufFree(ColorsArray)
   AIL.MbufFree(SourceChild)
   AIL.MbufFree(DestChild)
   AIL.MbufFree(DisplayImage)   
   

#****************************************************************************
# Perform RGB to grayscale conversion
#****************************************************************************
# Source image 
RGB_IMAGE_FILE = os.path.join(AIL.M_IMAGE_PATH, "BinderClip.mim")

def RGBtoGrayscaleExample(AilSystem, AilDisplay):
   
   print("\nCONVERSION FROM RGB TO GRAYSCALE:")
   print("---------------------------------")
   print("The example compares principal component projection to luminance based\nconversion.\n")

   # Inquire size and type of the source image. 
   SourceSizeX = AIL.MbufDiskInquire(RGB_IMAGE_FILE, AIL.M_SIZE_X, AIL.M_NULL)
   SourceSizeY = AIL.MbufDiskInquire(RGB_IMAGE_FILE, AIL.M_SIZE_Y, AIL.M_NULL)
   Type        = AIL.MbufDiskInquire(RGB_IMAGE_FILE, AIL.M_TYPE, AIL.M_NULL)

   # Allocate buffer to display the input image and the results. 
   DisplayImage = AIL.MbufAllocColor(AilSystem, 3, SourceSizeX, 3 * SourceSizeY + 2 * DISPLAY_CENTER_MARGIN_Y, Type, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP)
   AIL.MbufClear(DisplayImage, 0)

   # Create a source child in the display image. 
   SourceChild = AIL.MbufChildColor2d(DisplayImage, AIL.M_ALL_BANDS, 0, 0, SourceSizeX, SourceSizeY)

   # Create a destination child in the display image for luminance based conversion. 
   LuminanceChild = AIL.MbufChildColor2d(DisplayImage, AIL.M_ALL_BANDS, 0, SourceSizeY + DISPLAY_CENTER_MARGIN_Y,
                    SourceSizeX, SourceSizeY)

   # Create a destination child in the display image for principal component projection. 
   PCPchild = AIL.MbufChildColor2d(DisplayImage, AIL.M_ALL_BANDS, 0, 2 * SourceSizeY + 2 * DISPLAY_CENTER_MARGIN_Y,
                    SourceSizeX, SourceSizeY)

   # Load the source image into the display image source child. 
   AIL.MbufLoad(RGB_IMAGE_FILE, SourceChild)
   AIL.MdispSelect(AilDisplay, DisplayImage)

   # Extract luminance channel from source image and copy the result in the display image. 
   LuminanceResult = AIL.MbufAlloc2d(AilSystem, SourceSizeX, SourceSizeY, Type, AIL.M_IMAGE + AIL.M_PROC)
   AIL.MimConvert(SourceChild, LuminanceResult, AIL.M_RGB_TO_L)
   AIL.MbufCopy(LuminanceResult, LuminanceChild)

   print("Color image converted to grayscale using luminance based conversion\ntechnique.\n")
   print("Press any key to convert the image using principal component projection\ntechnique.\n")
   AIL.MosGetch()

   # Create a mask to identify important colors in the source image. 
   Mask = AIL.MbufAlloc2d(AIL.M_DEFAULT_HOST, SourceSizeX, SourceSizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC)
   AIL.MbufClear(Mask, 0)

   # Define regions of interest (pixel colors with which to perform the color projection). 
   OffsetX   = 105
   OffsetY   = 45
   MaskSizeX = 60
   MaskSizeY = 20
   ChildMask = AIL.MbufChild2d(Mask, OffsetX, OffsetY, MaskSizeX, MaskSizeY)
   AIL.MbufClear(ChildMask, AIL.M_SOURCE_LABEL)
   AIL.MbufFree(ChildMask)

   OffsetX = 220
   ChildMask = AIL.MbufChild2d(Mask, OffsetX, OffsetY, MaskSizeX, MaskSizeY)
   AIL.MbufClear(ChildMask, AIL.M_SOURCE_LABEL)
   AIL.MbufFree(ChildMask)

   OffsetX = 335
   ChildMask = AIL.MbufChild2d(Mask, OffsetX, OffsetY, MaskSizeX, MaskSizeY)
   AIL.MbufClear(ChildMask, AIL.M_SOURCE_LABEL)

   # Perform principal component projection and copy the result in the display image. 
   PCPresult = AIL.MbufAlloc2d(AilSystem, SourceSizeX, SourceSizeY, Type, AIL.M_IMAGE + AIL.M_PROC)
   AIL.McolProject(SourceChild, Mask, PCPresult, AIL.M_NULL, AIL.M_PRINCIPAL_COMPONENT_PROJECTION, AIL.M_DEFAULT, AIL.M_NULL)
   AIL.MbufCopy(PCPresult, PCPchild)

   print("Color image converted to grayscale using principal component projection\ntechnique.")
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.MbufFree(ChildMask)
   AIL.MbufFree(Mask)
   AIL.MbufFree(PCPresult)
   AIL.MbufFree(LuminanceResult)
   AIL.MbufFree(PCPchild)
   AIL.MbufFree(LuminanceChild)
   AIL.MbufFree(SourceChild)
   AIL.MbufFree(DisplayImage)
   

#***************************************************************************
# Draw the samples as color patches.                                        
def DrawSampleColors(DestImage, pSamplesColors, pSampleNames, NumSamples, XSpacing, YOffset):
   
   DestSizeX = AIL.MbufInquire(DestImage, AIL.M_SIZE_X, AIL.M_NULL)
   DestSizeY = AIL.MbufInquire(DestImage, AIL.M_SIZE_Y, AIL.M_NULL)
   OffsetX = (DestSizeX - (NumSamples * COLOR_PATCH_SIZEX) - ((NumSamples - 1) * XSpacing)) /2.0
   OffsetY =  YOffset if YOffset > 0 else (DestSizeY - COLOR_PATCH_SIZEY)/2.0
   AIL.MgraFont(AIL.M_DEFAULT, AIL.M_FONT_DEFAULT_SMALL)

   for SampleIndex in range(0, NumSamples):
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_RGB888(pSamplesColors[SampleIndex][0], pSamplesColors[SampleIndex][1], pSamplesColors[SampleIndex][2]))
      AIL.MgraRectFill(AIL.M_DEFAULT, DestImage, OffsetX, OffsetY, OffsetX + COLOR_PATCH_SIZEX, OffsetY + COLOR_PATCH_SIZEY)
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_YELLOW)
      TextOffsetX = OffsetX + COLOR_PATCH_SIZEX / 2.0 - 4.0 * len(pSampleNames[SampleIndex]) + 0.5
      AIL.MgraText(AIL.M_DEFAULT, DestImage, TextOffsetX, OffsetY-20, pSampleNames[SampleIndex])
      OffsetX += (COLOR_PATCH_SIZEX + XSpacing)


if __name__ == "__main__":
   McolExample()

```
