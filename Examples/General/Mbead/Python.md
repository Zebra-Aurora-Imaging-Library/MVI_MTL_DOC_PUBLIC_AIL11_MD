---
title: "Mbead"
description: "This program performs several bead inspections on mechanical parts."
ms.language: "Python"
---

# Mbead

> This program performs several bead inspections on mechanical parts.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MbeadAlloc`, `MbeadAllocResult`, `MbeadControl`, `MbeadDraw`, `MbeadFree`, `MbeadGetResult`, `MbeadInquire`, `MbeadTemplate`, `MbeadTrain`, `MbeadVerify`, `MbufFree`, `MbufRestore`, `McalAlloc`, `McalDraw`, `McalFixture`, `McalFree`, `McalUniform`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MmodAlloc`, `MmodAllocResult`, `MmodDefine`, `MmodDraw`, `MmodFind`, `MmodFree`, `MmodPreprocess`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Measuring, Modules, Buffer, Display, Graphics, Calibration, Geometric Model Finder, Bead Inspection, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#****************************************************************************
# 
# File name: AIL.Mbead.py
#
# Synopsis:  This program uses the Bead module to train a bead template
#            and then to inspect a defective bead in a target image.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#

import ail11 as AIL
import os

#***************************************************************************
# Main. 
#*****************************************************************************

def MbeadExample():
   
   # Allocate defaults. 
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)    

   # Run fixtured bead example. 
   FixturedBeadExample(AilSystem, AilDisplay)

   # Run predefined bead example. 
   PredefinedBeadExample(AilSystem, AilDisplay)

   # Free defaults.     
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)

   return 0

# Utility definitions.
USER_POSITION_COLOR      = AIL.M_COLOR_RED
USER_TEMPLATE_COLOR      = AIL.M_COLOR_CYAN
TRAINED_BEAD_WIDTH_COLOR = AIL.M_RGB888(255, 128, 0)
MODEL_FINDER_COLOR       = AIL.M_COLOR_GREEN
COORDINATE_SYSTEM_COLOR  = AIL.M_RGB888(164, 164, 0)
RESULT_SEARCH_BOX_COLOR  = AIL.M_COLOR_CYAN
PASS_BEAD_WIDTH_COLOR    = AIL.M_COLOR_GREEN
PASS_BEAD_POSITION_COLOR = AIL.M_COLOR_GREEN
FAIL_NOT_FOUND_COLOR     = AIL.M_COLOR_RED
FAIL_SMALL_WIDTH_COLOR   = AIL.M_RGB888(255, 128, 0)
FAIL_EDGE_OFFSET_COLOR   = AIL.M_COLOR_RED
   
#***************************************************************************
# Fixtured 'stripe' bead example. 

# Target image specifications. 
IMAGE_FILE_TRAINING = os.path.join(AIL.M_IMAGE_PATH, "BeadTraining.mim")
IMAGE_FILE_TARGET   = os.path.join(AIL.M_IMAGE_PATH, "BeadTarget.mim")

# Bead stripe training data definition. 
NUMBER_OF_TRAINING_POINTS = 13

TrainingPointsX = [180, 280, 400, 430, 455, 415, 370, 275, 185, 125, 105, 130, 180]
TrainingPointsY = [190, 215, 190, 200, 260, 330, 345, 310, 340, 305, 265, 200, 190]

# Max angle correction. 
MAX_ANGLE_CORRECTION_VALUE = 20.0

# Max offset deviation. 
MAX_DEVIATION_OFFSET_VALUE = 25.0

# Maximum negative width variation. 
WIDTH_DELTA_NEG_VALUE = 2.0

# Model region  definition. 
MODEL_ORIGIN_X = 145
MODEL_ORIGIN_Y = 115
MODEL_SIZE_X   = 275
MODEL_SIZE_Y   = 60

#***************************************************************************
# Predefined 'edge' bead example. 

# Target image specifications. 
CAP_FILE_TARGET = os.path.join(AIL.M_IMAGE_PATH, "Cap.mim")

# Template attributes definition. 
CIRCLE_CENTER_X = 330.0
CIRCLE_CENTER_Y = 230.0
CIRCLE_RADIUS   = 120.0

# Edge threshold value. 
EDGE_THRESHOLD_VALUE = 25.0

# Max offset found and deviation tolerance. 
MAX_CONTOUR_DEVIATION_OFFSET = 5.0
MAX_CONTOUR_FOUND_OFFSET     = 25.0

def FixturedBeadExample(AilSystem, AilDisplay):
                                      
   # Restore source images into automatically allocated image buffers. 
   AilImageTraining = AIL.MbufRestore(IMAGE_FILE_TRAINING, AilSystem)
   AilImageTarget = AIL.MbufRestore(IMAGE_FILE_TARGET, AilSystem)

   # Display the training image buffer. 
   AIL.MdispSelect(AilDisplay, AilImageTraining)

   # Allocate a graphic list to hold the subpixel annotations to draw. 
   AilGraList = AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT)

   # Associate the graphic list to the display for annotations. 
   AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraList)

   # Original template image. 
   print("\nFIXTURED BEAD INSPECTION:")
   print("-------------------------\n")
   print("This program performs a bead inspection on a mechanical part.")
   print("In the first step, a bead template context is trained using an image.")
   print("In the second step, a mechanical part, at an arbitrary angle and with")
   print("a defective bead, is inspected.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Allocate a bead context. 
   AilBeadContext = AIL.MbeadAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Allocate a bead result. 
   AilBeadResult = AIL.MbeadAllocResult(AilSystem, AIL.M_DEFAULT)

   # Add a bead template. 
   AIL.MbeadTemplate(AilBeadContext, AIL.M_ADD, AIL.M_DEFAULT, AIL.M_TEMPLATE_LABEL(1), NUMBER_OF_TRAINING_POINTS, TrainingPointsX, TrainingPointsY, AIL.M_NULL, AIL.M_DEFAULT)

   # Set template input units to world units.    
   AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TEMPLATE_INPUT_UNITS, AIL.M_WORLD)

   # Set the bead 'edge type' search properties. 
   AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_ANGLE_ACCURACY_MAX_DEVIATION, MAX_ANGLE_CORRECTION_VALUE)

   # Set the maximum valid bead deformation. 
   AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_OFFSET_MAX, MAX_DEVIATION_OFFSET_VALUE)

   # Set the valid bead minimum width criterion. 
   AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_WIDTH_DELTA_NEG, WIDTH_DELTA_NEG_VALUE)

   # Display the bead polyline. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, USER_TEMPLATE_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilGraList, AIL.M_DRAW_POSITION_POLYLINE, AIL.M_USER, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   # Display the bead training points. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, USER_POSITION_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilGraList, AIL.M_DRAW_POSITION, AIL.M_USER, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   # Pause to show the template image and user points. 
   print("The initial points specified by the user (in red) are")
   print("used to train the bead information from an image.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Set a 1:1 calibration to the training image. 
   AIL.McalUniform(AilImageTraining, 0, 0, 1, 1, 0, AIL.M_DEFAULT)

   # Train the bead context. 
   AIL.MbeadTrain(AilBeadContext, AilImageTraining, AIL.M_DEFAULT)

   # Display the trained bead.    
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, TRAINED_BEAD_WIDTH_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilGraList, AIL.M_DRAW_WIDTH, AIL.M_TRAINED, AIL.M_TEMPLATE_LABEL(1), AIL.M_ALL, AIL.M_DEFAULT)

   # Retrieve the trained nominal width. 
   NominalWidth = AIL.MbeadInquire(AilBeadContext,AIL.M_TEMPLATE_LABEL(1), AIL.M_TRAINED_WIDTH_NOMINAL)

   print("The template has been trained and is displayed in orange.")
   print("Its nominal trained width is {:.2f} pixels.\n".format(NominalWidth))

   # model to further fixture the bead template. 
   AilModelFinderContext = AIL.MmodAlloc(AilSystem, AIL.M_GEOMETRIC, AIL.M_DEFAULT)

   AilModelFinderResult = AIL.MmodAllocResult(AilSystem, AIL.M_DEFAULT)

   AIL.MmodDefine(AilModelFinderContext, AIL.M_IMAGE, AilImageTraining, MODEL_ORIGIN_X, MODEL_ORIGIN_Y, MODEL_SIZE_X, MODEL_SIZE_Y)

   # Preprocess the model. 
   AIL.MmodPreprocess(AilModelFinderContext, AIL.M_DEFAULT)   

   # Allocate a fixture object. 
   AilFixturingOffset = AIL.McalAlloc(AilSystem, AIL.M_FIXTURING_OFFSET, AIL.M_DEFAULT)

   # Learn the relative offset of the model. 
   AIL.McalFixture(AIL.M_NULL, AilFixturingOffset, AIL.M_LEARN_OFFSET, AIL.M_MODEL_MOD, AilModelFinderContext, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Display the model. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, MODEL_FINDER_COLOR)
   AIL.MmodDraw(AIL.M_DEFAULT, AilModelFinderContext, AilGraList, AIL.M_DRAW_BOX+AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_ORIGINAL) 

   print("A Model Finder model (in green) is also defined to")
   print("further fixture the bead verification operation.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Clear the overlay annotation. 
   AIL.MgraClear(AIL.M_DEFAULT, AilGraList)

   # Display the target image buffer. 
   AIL.MdispSelect(AilDisplay, AilImageTarget)

   # Find the location of the fixture using Model Finder. 
   AIL.MmodFind(AilModelFinderContext, AilImageTarget, AilModelFinderResult)

   # Display the found model occurrence. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, MODEL_FINDER_COLOR)
   AIL.MmodDraw(AIL.M_DEFAULT, AilModelFinderResult, AilGraList, AIL.M_DRAW_BOX+AIL.M_DRAW_POSITION, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Apply fixture offset to the target image. 
   AIL.McalFixture(AilImageTarget, AilFixturingOffset, AIL.M_MOVE_RELATIVE, AIL.M_RESULT_MOD, AilModelFinderResult, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Display the relative coordinate system. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, COORDINATE_SYSTEM_COLOR)
   AIL.McalDraw(AIL.M_DEFAULT, AIL.M_NULL, AilGraList, AIL.M_DRAW_RELATIVE_COORDINATE_SYSTEM, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Perform the inspection of the bead in the fixtured target image. 
   AIL.MbeadVerify(AilBeadContext, AilImageTarget, AilBeadResult, AIL.M_DEFAULT)

   # Display the result search boxes. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, RESULT_SEARCH_BOX_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilGraList, AIL.M_DRAW_SEARCH_BOX, AIL.M_ALL, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   print("The mechanical part's position and angle (in green) were located")
   print("using Model Finder, and the bead's search boxes (in cyan) were")
   print("positioned accordingly.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Clear the overlay annotation. 
   AIL.MgraClear(AIL.M_DEFAULT, AilGraList)

   # Display the moved relative coordinate system. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, COORDINATE_SYSTEM_COLOR)
   AIL.McalDraw(AIL.M_DEFAULT, AIL.M_NULL, AilGraList, AIL.M_DRAW_RELATIVE_COORDINATE_SYSTEM, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Display the pass bead sections. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_BEAD_WIDTH_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilGraList, AIL.M_DRAW_WIDTH, AIL.M_PASS, AIL.M_TEMPLATE_LABEL(1), AIL.M_ALL, AIL.M_DEFAULT)

   # Display the missing bead sections. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_NOT_FOUND_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilGraList, AIL.M_DRAW_SEARCH_BOX, AIL.M_FAIL_NOT_FOUND, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   # Display bead sections which do not meet the minimum width criteria. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_SMALL_WIDTH_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilGraList, AIL.M_DRAW_SEARCH_BOX, AIL.M_FAIL_WIDTH_MIN, AIL.M_TEMPLATE_LABEL(1), AIL.M_ALL, AIL.M_DEFAULT)

   # Retrieve and display general bead results. 
   Score = AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_SCORE)
   GapCov = AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_GAP_COVERAGE)
   AvWidth = AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_WIDTH_AVERAGE)
   MaxGap = AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_GAP_MAX_LENGTH)

   print("The bead has been inspected:")
   print(" -Passing bead sections (green) cover {:.2f}% of the bead".format(Score))
   print(" -Missing bead sections (red) cover {:.2f}% of the bead".format(GapCov))
   print(" -Sections outside the specified tolerances are drawn in orange")
   print(" -The bead's average width is {:.2f} pixels".format(AvWidth))
   print(" -The bead's longest gap section is {:.2f} pixels".format(MaxGap))

   # Pause to show the result. 
   print("Press any key to continue.")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.MmodFree(AilModelFinderContext)
   AIL.MmodFree(AilModelFinderResult)
   AIL.MbeadFree(AilBeadContext)
   AIL.MbeadFree(AilBeadResult)
   AIL.McalFree(AilFixturingOffset)
   AIL.MbufFree(AilImageTraining)
   AIL.MbufFree(AilImageTarget)
   AIL.MgraFree(AilGraList)
   
def PredefinedBeadExample(AilSystem, AilDisplay):    

   # Restore target image into an automatically allocated image buffers. 
   AilImageTarget = AIL.MbufRestore(CAP_FILE_TARGET, AilSystem)

   # Display the training image buffer. 
   AIL.MdispSelect(AilDisplay, AilImageTarget)

   # Prepare the overlay for annotations. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlayImage = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_TRANSPARENT_COLOR)

   # Original template image. 
   print("\nPREDEFINED BEAD INSPECTION:")
   print("---------------------------\n")
   print("This program performs a bead inspection of a bottle")
   print("cap's contour using a predefined circular bead.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Allocate a bead context. 
   AilBeadContext = AIL.MbeadAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT)

   # Allocate a bead result. 
   AilBeadResult = AIL.MbeadAllocResult(AilSystem, AIL.M_DEFAULT)

   # Add the bead templates. 
   AIL.MbeadTemplate(AilBeadContext, AIL.M_ADD, AIL.M_BEAD_EDGE, AIL.M_TEMPLATE_LABEL(1), 0, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT)

   # Set the bead shape properties. 
   AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TRAINING_PATH, AIL.M_CIRCLE)

   AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TEMPLATE_CIRCLE_CENTER_X, CIRCLE_CENTER_X)
   AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TEMPLATE_CIRCLE_CENTER_Y, CIRCLE_CENTER_Y)
   AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_TEMPLATE_CIRCLE_RADIUS, CIRCLE_RADIUS)

   # Set the edge threshold value to extract the object shape. 
   AIL.MbeadControl(AilBeadContext, AIL.M_TEMPLATE_LABEL(1), AIL.M_THRESHOLD_VALUE, EDGE_THRESHOLD_VALUE)

   # Using the default fixed user defined nominal edge width. 
   AIL.MbeadControl(AilBeadContext, AIL.M_ALL, AIL.M_WIDTH_NOMINAL_MODE, AIL.M_USER_DEFINED)

   # Set the maximal expected contour deformation. 
   AIL.MbeadControl(AilBeadContext, AIL.M_ALL, AIL.M_FAIL_WARNING_OFFSET, MAX_CONTOUR_FOUND_OFFSET)

   # Set the maximum valid bead deformation. 
   AIL.MbeadControl(AilBeadContext, AIL.M_ALL, AIL.M_OFFSET_MAX, MAX_CONTOUR_DEVIATION_OFFSET)      

   # Display the bead in the overlay image. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, USER_TEMPLATE_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilOverlayImage, AIL.M_DRAW_POSITION, AIL.M_USER, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   # The bead template is entirely defined and is trained without sample image. 
   AIL.MbeadTrain(AilBeadContext, AIL.M_NULL, AIL.M_DEFAULT)

   # Display the trained bead. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, TRAINED_BEAD_WIDTH_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadContext, AilOverlayImage, AIL.M_DRAW_SEARCH_BOX, AIL.M_TRAINED, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   # Pause to show the template image and user points. 
   print("A circular template that was parametrically defined by the")
   print("user is displayed (in cyan). The template has been trained") 
   print("and the resulting search is displayed (in orange).")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Perform the inspection of the bead in the fixtured target image. 
   AIL.MbeadVerify(AilBeadContext, AilImageTarget, AilBeadResult, AIL.M_DEFAULT)

   # Clear the overlay annotation. 
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY_CLEAR, AIL.M_TRANSPARENT_COLOR)

   # Display the pass bead sections. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, PASS_BEAD_POSITION_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilOverlayImage, AIL.M_DRAW_POSITION, AIL.M_PASS, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   # Display the offset bead sections. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, FAIL_EDGE_OFFSET_COLOR)
   AIL.MbeadDraw(AIL.M_DEFAULT, AilBeadResult, AilOverlayImage, AIL.M_DRAW_POSITION, AIL.M_FAIL_OFFSET, AIL.M_ALL, AIL.M_ALL, AIL.M_DEFAULT)

   # Retrieve and display general bead results. 
   MaximumOffset = AIL.MbeadGetResult(AilBeadResult, AIL.M_TEMPLATE_LABEL(1), AIL.M_GENERAL, AIL.M_OFFSET_MAX)

   print("The bottle cap shape has been inspected:")
   print(" -Sections outside the specified offset tolerance are drawn in red")
   print(" -The maximum offset value is {:.2f} pixels.\n".format(MaximumOffset))

   # Pause to show the result. 
   print("Press any key to terminate.\n")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.MbeadFree(AilBeadContext)
   AIL.MbeadFree(AilBeadResult)
   AIL.MbufFree(AilImageTarget)

if __name__ == "__main__":
   MbeadExample()

```
