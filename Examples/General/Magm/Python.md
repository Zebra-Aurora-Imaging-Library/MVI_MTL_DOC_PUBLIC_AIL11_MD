---
title: "Magm"
description: "This example demonstrates the usage of the Advanced Geometric Matcher module."
ms.language: "Python"
---

# Magm

> This example demonstrates the usage of the Advanced Geometric Matcher module.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MagmAlloc`, `MagmAllocResult`, `MagmControl`, `MagmDefine`, `MagmDraw`, `MagmFind`, `MagmFree`, `MagmGetResult`, `MagmPreprocess`, `MagmTrain`, `MagmCopyResult`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Finding/Locating, Modules, Advanced Geometric Matcher, Buffer, Display, Graphics, What's New, AIL 10.0 SP7, AIL 10.0 SP6

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: Magm.py 
#
# Synopsis:  This program consists of 2 examples that use the AGM module
#            to define a model and search for model occurrences in target images.
#            The first example extracts a single-definition model from a source image,
#            then quickly finds occurrences in a cluttered target image.
#            The second example constructs a composite-definition model through training,
#            then finds occurrences with slight variations in appearance in different target images.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
 
import ail11 as AIL
import os

# Path definitions.
MODEL_IMAGE_0_PATH     = os.path.join(AIL.M_IMAGE_PATH, "Magm", "CircuitPinsModel.mim")
MODEL_IMAGE_1_PATH     = os.path.join(AIL.M_IMAGE_PATH, "Magm", "SwitchModel.mim")
TARGET_IMAGE_0_PATH    = os.path.join(AIL.M_IMAGE_PATH, "Magm", "CircuitBoardTarget.mim")
TARGET_IMAGE_1_PATH    = os.path.join(AIL.M_IMAGE_PATH, "Magm", "SwitchTarget.mim")
TRAIN_IMAGES_PATH      = os.path.join(AIL.M_IMAGE_PATH, "Magm", "LabeledTrainImages.mbufc")
TEST_IMAGES_DIR_PATH   = os.path.join(AIL.M_IMAGE_PATH, "Magm", "Testset")

# Execute Single-definition model example.
def ExecuteSingleModelExample(
   AilSystem,
   AilModelDisplay,
   AilTargetDisplay,
   ModelImagePath,
   TargetImagePath,
   DetectionSearchAngleDeltaPos = 0.0):

   # Allocate a graphic list to hold the subpixel annotations to draw.
   AilGraphicList = AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT)

   # Associate the graphic list to the display for annotations.
   AIL.MdispControl(AilTargetDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList)

   # Restore the model image and display it.
   AilModelImage = AIL.MbufRestore(ModelImagePath, AilSystem)
   
   # Make the display a little bigger since the image is small.
   ModelWindowSizeX = AIL.MbufInquire(AilModelImage, AIL.M_SIZE_X, AIL.M_NULL) + 150;
   ModelWindowSizeY = AIL.MbufInquire(AilModelImage, AIL.M_SIZE_Y, AIL.M_NULL) + 150;

   AIL.MdispControl(AilModelDisplay, AIL.M_WINDOW_INITIAL_SIZE_X, ModelWindowSizeX);
   AIL.MdispControl(AilModelDisplay, AIL.M_WINDOW_INITIAL_SIZE_Y, ModelWindowSizeY);

   # Sets the initial, left-most, X-coordinate of the model disaplay and target display
   AIL.MdispControl(AilModelDisplay, AIL.M_WINDOW_INITIAL_POSITION_X, 10);
   AIL.MdispControl(AilModelDisplay, AIL.M_WINDOW_INITIAL_POSITION_Y, 10);

   AIL.MdispControl(AilTargetDisplay, AIL.M_WINDOW_INITIAL_POSITION_X, ModelWindowSizeX + 90);
   AIL.MdispControl(AilTargetDisplay, AIL.M_WINDOW_INITIAL_POSITION_Y, 10);

   # Display the model image.   
   AIL.MdispSelect(AilModelDisplay, AilModelImage)

   print("A single-definition model was defined from the displayed model image.\n");

   # Allocate a find AGM context.
   AilFindContext = AIL.MagmAlloc(AilSystem, AIL.M_GLOBAL_EDGE_BASED_FIND, AIL.M_DEFAULT)

   # Allocate a find AGM result buffer.
   AilSearchResult = AIL.MagmAllocResult(AilSystem, AIL.M_GLOBAL_EDGE_BASED_FIND_RESULT, AIL.M_DEFAULT)

   # Define the single-definition model.
   AIL.MagmDefine(AilFindContext, AIL.M_ADD, AIL.M_DEFAULT, AIL.M_SINGLE, AilModelImage, AIL.M_DEFAULT)

   # Set the minimum acceptable detection score.
   AIL.MagmControl(AilFindContext, AIL.M_AGM_MODEL_INDEX(0), AIL.M_ACCEPTANCE_DETECTION, 75)

   # Set the search angle range for the detection.
   AIL.MagmControl(AilFindContext, AIL.M_AGM_MODEL_INDEX(0), AIL.M_DETECTION_DELTA_POS_ANGLE, DetectionSearchAngleDeltaPos);

   # Preprocess the find AGM context.
   AIL.MagmPreprocess(AilFindContext, AIL.M_DEFAULT)

   # Restore the target image.
   AilTargetImage = AIL.MbufRestore(TargetImagePath, AilSystem)

   # Reset the time.
   AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL)

   # Find the model.
   AIL.MagmFind(AilFindContext, AilTargetImage, AilSearchResult, AIL.M_DEFAULT)

   # Read the find time.
   FindTime = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS)

   # Get the number of occurrences found.
   NumOccurrences = AIL.MagmGetResult(AilSearchResult, AIL.M_DEFAULT, AIL.M_NUMBER)
   if NumOccurrences > 0:
      XPositions      = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_POSITION_X)
      YPositions      = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_POSITION_Y)
      Angles          = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_ANGLE)
      DetectionScores = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_SCORE_DETECTION)
      FitScores       = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_SCORE_FIT)
      CoverageScores  = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_SCORE_COVERAGE)

      # Print the results for each occurrence found.
      print("The model was found in the target image:\n")
      print("Result   X Position   Y Position   Angle    DetectionScore   FitScore   CoverageScore\n")
      for n in range(NumOccurrences):
         print("{:<9}{:<13.2f}{:<13.2f}{:<9.2f}{:<17.2f}{:<11.2f}{:<11.2f}"
               .format(int(n), XPositions[n], YPositions[n], Angles[n], DetectionScores[n], FitScores[n], CoverageScores[n]))
         
      print("\nNumber of occurrences found in the target image: {}".format(int(NumOccurrences)))
      print("Search time: {:.1f} ms".format(FindTime * 1000.0))

      # Draw green edges and bounding boxes over the occurrences that were found.
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN)
      AIL.MagmDraw(AIL.M_DEFAULT, AilSearchResult, AilGraphicList, AIL.M_DRAW_EDGES + AIL.M_DRAW_BOX, AIL.M_ALL, AIL.M_DEFAULT)

      # Draw red positions over the occurrences that were found.
      AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED)
      AIL.MagmDraw(AIL.M_DEFAULT, AilSearchResult, AilGraphicList, AIL.M_DRAW_POSITION, AIL.M_ALL, AIL.M_DEFAULT)

   else :
      print("The model was not found in the target image")

   # Display the target image.
   AIL.MdispSelect(AilTargetDisplay, AilTargetImage)
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Put the display back to its default state.
   AIL.MdispControl(AilModelDisplay, AIL.M_WINDOW_INITIAL_SIZE_X, AIL.M_DEFAULT);
   AIL.MdispControl(AilModelDisplay, AIL.M_WINDOW_INITIAL_SIZE_Y, AIL.M_DEFAULT);

   # Remove the display.
   AIL.MdispSelect(AilModelDisplay , AIL.M_NULL);
   AIL.MdispSelect(AilTargetDisplay, AIL.M_NULL);

   # Free objects.
   AIL.MgraFree(AilGraphicList)
   AIL.MagmFree(AilFindContext)
   AIL.MagmFree(AilSearchResult)
   AIL.MbufFree(AilModelImage)
   AIL.MbufFree(AilTargetImage)

# Single-definition model example.
def SingleModelExample(AilSystem):
   print("This example shows that AGM is able to quickly find occurrences")
   print("in a large cluttered target image.")
   print("Press any key to continue.\n")
   AIL.MosGetch()

   # Allocate the model and target displays
   AilModelDisplay  = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT);
   AilTargetDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT);

   # Set the model and target display's titles
   AIL.MdispControl(AilModelDisplay , AIL.M_TITLE, "Model");
   AIL.MdispControl(AilTargetDisplay, AIL.M_TITLE, "Target");

   ExecuteSingleModelExample(AilSystem, AilModelDisplay, AilTargetDisplay, MODEL_IMAGE_0_PATH, TARGET_IMAGE_0_PATH);

   print("AGM can also find oriented occurrences.\n");
   ExecuteSingleModelExample(AilSystem, AilModelDisplay, AilTargetDisplay, MODEL_IMAGE_1_PATH, TARGET_IMAGE_1_PATH, 360.0);

   AIL.MdispFree(AilModelDisplay)
   AIL.MdispFree(AilTargetDisplay)

# Composite-definition model example.
def CompositeModelExample(AilSystem):
   print("This example shows that AGM is able to confidently find occurrences with appearance");
   print("variation in a complex background after training a composite-definition model.");

   # Allocate the display
   AilDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT);

   # Restore the training images.
   TrainImagesContainer = AIL.MbufRestore(TRAIN_IMAGES_PATH, AilSystem)

   # Print message about training image labels.
   print("\n*******************************************************")
   print("LOADING LABELED TRAINING IMAGES...")
   print("*******************************************************")

   print("Training requires labeled images with positive and negative samples.")
   print("Positive samples are occurrences delimited by blue boxes and")
   print("negative samples are background parts delimited by red boxes.")
   print("Typically, when false positives are detected in training images,")
   print("they should be used as negative samples to improve the training.")
   print("To ease the labeling of images, use the example AgmLabelingTool.")

   # Wait for a key to be pressed.
   print("\nPress any key to show the labeled images used in this training.")
   AIL.MosGetch()

   # Get the components from the container.
   NumTrainImage = AIL.MbufInquireContainer(TrainImagesContainer, AIL.M_CONTAINER, AIL.M_COMPONENT_LIST + AIL.M_NB_ELEMENTS)
   TrainImages = AIL.MbufInquireContainer(TrainImagesContainer, AIL.M_CONTAINER, AIL.M_COMPONENT_LIST)

   # Allocate a graphic list to hold the subpixel annotations to draw.
   Regions = AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT)

   # Associate the graphic list to the display for annotations.
   AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, Regions)

   # Display each labeled training image.
   print("\nOnly positive samples are given because AGM is able");
   print("to automatically generate negative samples.\n");

   for n in range(NumTrainImage):
      AIL.MgraClear(AIL.M_DEFAULT, Regions)
      AIL.MbufSetRegion(TrainImages[n], Regions, AIL.M_DEFAULT, AIL.M_EXTRACT, AIL.M_DEFAULT)
      AIL.MdispSelect(AilDisplay, TrainImages[n])
      print("Training image {}/{}".format(int(n+1), int(NumTrainImage)))
      print("Press any key to continue.")
      AIL.MosGetch()

   # Disassociate the graphic list from the display and stop displaying the training images.
   AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AIL.M_NULL)
   AIL.MdispSelect(AilDisplay, AIL.M_NULL)

   # Allocate a find AGM context.
   AilFindContext = AIL.MagmAlloc(AilSystem, AIL.M_GLOBAL_EDGE_BASED_FIND, AIL.M_DEFAULT)

   # Allocate a find AGM result buffer.
   AilSearchResult = AIL.MagmAllocResult(AilSystem, AIL.M_GLOBAL_EDGE_BASED_FIND_RESULT, AIL.M_DEFAULT)

   # Allocate a train AGM context.
   AilTrainContext = AIL.MagmAlloc(AilSystem, AIL.M_GLOBAL_EDGE_BASED_TRAIN, AIL.M_DEFAULT)

   # Allocate a train AGM result buffer.
   AilTrainResult = AIL.MagmAllocResult(AilSystem, AIL.M_GLOBAL_EDGE_BASED_TRAIN_RESULT, AIL.M_DEFAULT)

   # Define the composite-definition model.
   AIL.MagmDefine(AilTrainContext, AIL.M_ADD, AIL.M_DEFAULT, AIL.M_COMPOSITE, AIL.M_NULL, AIL.M_DEFAULT)

   # Enable the automatic generation of negative labels.
   AIL.MagmControl(AilTrainContext, AIL.M_AGM_MODEL_INDEX(0), AIL.M_NEGATIVE_LABELS_MODE, AIL.M_AUTO);

   # Preprocess the train AGM context.
   AIL.MagmPreprocess(AilTrainContext, AIL.M_DEFAULT)

   # Train the composite-definition model.
   print("\n*******************************************************")
   print("TRAINING... THIS WILL TAKE SOME TIME...")
   print("*******************************************************")

   TrainImagesContainerPtr = [TrainImagesContainer]

   AIL.MagmTrain(AilTrainContext, TrainImagesContainerPtr, 1, AilTrainResult, AIL.M_DEFAULT)

   # Check that the training process completed successfully.
   TrainStatus = AIL.MagmGetResult(AilTrainResult, AIL.M_DEFAULT, AIL.M_STATUS)
   if TrainStatus == AIL.M_COMPLETE:
      print("Training complete!")
      # Ensure that the trained model is valid before copying to the find AGM context.
      TrainedModelStatus = AIL.MagmGetResult(AilTrainResult, AIL.M_AGM_MODEL_INDEX(0), AIL.M_STATUS)
      if TrainedModelStatus == AIL.M_STATUS_TRAIN_OK :

         # Display all the positive and negative labels on the training images.
         print("Here are the training images with the positive labels");
         print("and the automatically generated negative labels.\n");

         AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, Regions);
         for i in range(NumTrainImage):
            AIL.MgraClear(AIL.M_DEFAULT, Regions);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_BLUE);
            AIL.MagmDraw(AIL.M_DEFAULT, AilTrainResult, Regions, AIL.M_DRAW_POS_RECTANGLES, AIL.M_AGM_IMAGE_INDEX(i), AIL.M_DEFAULT);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_RED);
            AIL.MagmDraw(AIL.M_DEFAULT, AilTrainResult, Regions, AIL.M_DRAW_NEG_RECTANGLES, AIL.M_AGM_IMAGE_INDEX(i), AIL.M_DEFAULT);
            AIL.MdispSelect(AilDisplay, TrainImages[i]);
            print("Training image {}/{}".format(int(i+1), int(NumTrainImage)))
            print("Press any key to continue.");
            AIL.MosGetch()
            
         # Disassociate the graphic list from the display and stop displaying the training images.
         AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AIL.M_NULL);
         AIL.MdispSelect(AilDisplay, AIL.M_NULL);

         # Copy the trained model to a find AGM context.
         AIL.MagmCopyResult(AilTrainResult, AIL.M_DEFAULT, AilFindContext, AIL.M_DEFAULT, AIL.M_TRAINED_MODEL, AIL.M_DEFAULT)

   # Preprocess find AGM context.
   AIL.MagmPreprocess(AilFindContext, AIL.M_DEFAULT)

   print("\n*******************************************************")
   print("FINDING WITH THE TRAINED MODEL...")
   print("*******************************************************")

   # Restore test images.
   FilesToSearch = os.path.join(TEST_IMAGES_DIR_PATH, "*.mim")
   NumberOfImages = AIL.MappFileOperation(AIL.M_DEFAULT, FilesToSearch, AIL.M_NULL, AIL.M_NULL, AIL.M_FILE_NAME_FIND+AIL.M_NB_ELEMENTS, AIL.M_DEFAULT)
   TestImages = []
   for n in range(NumberOfImages):
      Filename = AIL.MappFileOperation(AIL.M_DEFAULT, FilesToSearch, AIL.M_NULL, AIL.M_NULL, AIL.M_FILE_NAME_FIND, n)
      FilePath = os.path.join(TEST_IMAGES_DIR_PATH, Filename)
      TestImages.append(AIL.MbufRestore(FilePath, AilSystem))

   # Allocate a graphic list to hold the subpixel annotations to draw.
   AilGraphicList = AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT)

   # Associate the graphic list to the display for annotations.
   AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList)

   # Assign the color to draw.
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN)

   for n in range(NumberOfImages):
      # Find the model in the test image.
      AIL.MagmFind(AilFindContext, TestImages[n], AilSearchResult, AIL.M_DEFAULT)

      # Get the number of occurrences found.
      NumOccurrences = AIL.MagmGetResult(AilSearchResult, AIL.M_DEFAULT, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT)

      if NumOccurrences > 0:
         # Get the results of the search.
         XPositions = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_POSITION_X)
         YPositions = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_POSITION_Y)
         DetectionScores = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_SCORE_DETECTION)
         FitScores = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_SCORE_FIT)
         CoverageScores = AIL.MagmGetResult(AilSearchResult, AIL.M_ALL, AIL.M_SCORE_COVERAGE)

         # Print the results for each occurrence foud.
         print("The model was found in the target image:\n")
         print("Result   X Position   Y Position   DetectionScore   FitScore   CoverageScore\n")
         for k in range(NumOccurrences):
            print("{:<9}{:<13.2f}{:<13.2f}{:<17.2f}{:<11.2f}{:<11.2f}"
               .format(int(k), XPositions[k], YPositions[k], DetectionScores[k], FitScores[k], CoverageScores[k]))

         # Empty the graphic list.
         AIL.MgraClear(AIL.M_DEFAULT, AilGraphicList)

         # Draw bounding box
         AIL.MagmDraw(AIL.M_DEFAULT, AilSearchResult, AilGraphicList, AIL.M_DRAW_BOX, AIL.M_ALL, AIL.M_DEFAULT)
         
      else:
         print("The model was not found in the target image.\n")
         
      # Display the test image.
      AIL.MdispSelect(AilDisplay, TestImages[n])

      # Wait for a key to be pressed.
      if (n < NumberOfImages - 1):
         print("Press any key to continue.\n")
         AIL.MosGetch()      

   # Remove the display.
   AIL.MdispSelect(AilDisplay, AIL.M_NULL);

   # Free objects.
   for k in range(NumberOfImages):
      AIL.MbufFree(TestImages[k])
      
   AIL.MgraFree(AilGraphicList)
   AIL.MgraFree(Regions)
   AIL.MagmFree(AilTrainContext)
   AIL.MagmFree(AilTrainResult)
   AIL.MagmFree(AilFindContext)
   AIL.MagmFree(AilSearchResult)
   AIL.MbufFree(TrainImagesContainer)
   AIL.MdispFree(AilDisplay)

def MagmExample():
   # Allocate a default application, system, display and image.
   AilApplication, AilSystem = AIL.MappAllocDefault(AIL.M_DEFAULT, DispIdPtr=AIL.M_NULL, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Print the example synopsis.
   print("[EXAMPLE NAME]")
   print("Magm\n")
   print("[SYNOPSIS]")
   print("This program shows the use of the AGM module.\n")
   print("[MODULES USED]")
   print("Advanced Geometric Matcher, Buffer, Display, Graphics.\n")

   # Run single-definition model example.
   SingleModelExample(AilSystem)

   # Run composite-definition model example.
   CompositeModelExample(AilSystem)

   # Wait for a key to be pressed.
   print("Press any key to end.")
   AIL.MosGetch()
   
   # Free defaults.   
   AIL.MappFreeDefault(AilApplication, AilSystem, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL)
   
   return 0


if __name__ == "__main__":
   MagmExample()

```
