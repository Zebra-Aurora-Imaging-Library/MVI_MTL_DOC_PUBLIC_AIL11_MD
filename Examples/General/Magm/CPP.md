---
title: "Magm"
description: "This example demonstrates the usage of the Advanced Geometric Matcher module."
ms.language: "C++"
---

# Magm

> This example demonstrates the usage of the Advanced Geometric Matcher module.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MagmAlloc`, `MagmAllocResult`, `MagmControl`, `MagmDefine`, `MagmDraw`, `MagmFind`, `MagmFree`, `MagmGetResult`, `MagmPreprocess`, `MagmTrain`, `MagmCopyResult`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Finding/Locating, Modules, Advanced Geometric Matcher, Buffer, Display, Graphics, What's New, AIL 10.0 SP7, AIL 10.0 SP6

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Magm.cpp
//
// Description: Synopsis: This program consists of 2 examples that use the AGM module
//              to define a model and search for model occurrences in target images.
//              The first example extracts a single-definition model from a source image,
//              then quickly finds occurrences in a cluttered target image.
//              The second example constructs a composite-definition model through training,
//              then finds occurrences with slight variations in appearance in different target images.
//
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Path definitions.
#define EXAMPLE_IMAGE_DIR_PATH     M_IMAGE_PATH AIL_TEXT("/Magm/")

#define MODEL_IMAGE_0_PATH       EXAMPLE_IMAGE_DIR_PATH AIL_TEXT("CircuitPinsModel.mim")
#define MODEL_IMAGE_1_PATH       EXAMPLE_IMAGE_DIR_PATH AIL_TEXT("SwitchModel.mim")
#define TARGET_IMAGE_0_PATH      EXAMPLE_IMAGE_DIR_PATH AIL_TEXT("CircuitBoardTarget.mim")
#define TARGET_IMAGE_1_PATH      EXAMPLE_IMAGE_DIR_PATH AIL_TEXT("SwitchTarget.mim")
#define TRAIN_IMAGES_PATH        EXAMPLE_IMAGE_DIR_PATH AIL_TEXT("LabeledTrainImages.mbufc")
#define TEST_IMAGES_DIR_PATH     EXAMPLE_IMAGE_DIR_PATH AIL_TEXT("Testset/")

// Function declarations.
void SingleModelExample(AIL_ID AilSystem);
void CompositeModelExample(AIL_ID AilSystem);

//****************************************************************************
// Main.
//****************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication,    // Application identifier.
          AilSystem;         // System identifier.

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, M_NULL, M_NULL, M_NULL);

   // Print the example synopsis.
   MosPrintf(AIL_TEXT("[EXAMPLE NAME]\n"));
   MosPrintf(AIL_TEXT("Magm\n\n"));
   MosPrintf(AIL_TEXT("[SYNOPSIS]\n"));
   MosPrintf(AIL_TEXT("This program shows the use of the AGM module.\n\n"));
   MosPrintf(AIL_TEXT("[MODULES USED]\n"));
   MosPrintf(AIL_TEXT("Advanced Geometric Matcher, Buffer, Display, Graphics.\n\n"));

   // Run single-definition model example.
   SingleModelExample(AilSystem);

   // Run composite-definition model example.
   CompositeModelExample(AilSystem);

   // Wait for a key to be pressed.
   MosPrintf(AIL_TEXT("Press any key to end.\n"));
   MosGetch();
   
   // Free defaults.
   MappFreeDefault(AilApplication, AilSystem, M_NULL, M_NULL, M_NULL);

   return 0;
   }

//****************************************************************************
// Execute Single-definition model example.
//****************************************************************************
void ExecuteSingleModelExample(
   AIL_ID AilSystem,
   AIL_ID AilModelDisplay,
   AIL_ID AilTargetDisplay,
   AIL_CONST_TEXT_PTR ModelImagePath,
   AIL_CONST_TEXT_PTR TargetImagePath,
   AIL_DOUBLE DetectionSearchAngleDeltaPos = 0.0)
   {
   AIL_ID AilGraphicList;    // Graphic list identifier.
   AIL_ID AilFindContext;    // Find AGM context identifier.
   AIL_ID AilSearchResult;   // Find AGM result buffer identifier.
   AIL_ID AilModelImage;     // Image buffer identifier.
   AIL_ID AilTargetImage;    // Image buffer identifier.

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilTargetDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

   // Restore the model image.
   MbufRestore(ModelImagePath, AilSystem, &AilModelImage);

   // Make the model display a little bigger since the image is small.
   AIL_INT ModelWindowSizeX = MbufInquire(AilModelImage, M_SIZE_X, M_NULL) + 150;
   AIL_INT ModelWindowSizeY = MbufInquire(AilModelImage, M_SIZE_Y, M_NULL) + 150;

   MdispControl(AilModelDisplay, M_WINDOW_INITIAL_SIZE_X, ModelWindowSizeX);
   MdispControl(AilModelDisplay, M_WINDOW_INITIAL_SIZE_Y, ModelWindowSizeY);

   // Sets the initial, left-most, X-coordinate of the model disaplay and target display
   MdispControl(AilModelDisplay, M_WINDOW_INITIAL_POSITION_X, 10);
   MdispControl(AilModelDisplay, M_WINDOW_INITIAL_POSITION_Y, 10);

   MdispControl(AilTargetDisplay, M_WINDOW_INITIAL_POSITION_X, ModelWindowSizeX + 90);
   MdispControl(AilTargetDisplay, M_WINDOW_INITIAL_POSITION_Y, 10);

   // Display the model image.
   MdispSelect(AilModelDisplay, AilModelImage);

   MosPrintf(AIL_TEXT("A single-definition model was defined "));
   MosPrintf(AIL_TEXT("from the displayed model image.\n\n"));

   // Allocate a find AGM context.
   MagmAlloc(AilSystem, M_GLOBAL_EDGE_BASED_FIND, M_DEFAULT, &AilFindContext);

   // Allocate a find AGM result buffer.
   MagmAllocResult(AilSystem, M_GLOBAL_EDGE_BASED_FIND_RESULT, M_DEFAULT, &AilSearchResult);

   // Define the single-definition model.
   MagmDefine(AilFindContext, M_ADD, M_DEFAULT, M_SINGLE, AilModelImage, M_DEFAULT);

   // Set the minimum acceptable detection score.
   MagmControl(AilFindContext, M_AGM_MODEL_INDEX(0), M_ACCEPTANCE_DETECTION, 75.0);

   // Set the search angle range for the detection.
   MagmControl(AilFindContext, M_AGM_MODEL_INDEX(0), M_DETECTION_DELTA_POS_ANGLE, DetectionSearchAngleDeltaPos);

   // Preprocess the find AGM context.
   MagmPreprocess(AilFindContext, M_DEFAULT);

   // Restore the target image.
   MbufRestore(TargetImagePath, AilSystem, &AilTargetImage);

   // Reset the time.
   AIL_DOUBLE FindTime = 0.0;
   MappTimer(M_DEFAULT, M_TIMER_RESET + M_SYNCHRONOUS, M_NULL);

   // Find the model.
   MagmFind(AilFindContext, AilTargetImage, AilSearchResult, M_DEFAULT);

   // Read the find time.
   MappTimer(M_DEFAULT, M_TIMER_READ + M_SYNCHRONOUS, &FindTime);

   // Get the number of occurrences found.
   AIL_INT NumOccurrences = 0;
   MagmGetResult(AilSearchResult, M_DEFAULT, M_NUMBER + M_TYPE_AIL_INT, &NumOccurrences);
   if(NumOccurrences > 0)
      {
      std::vector<AIL_DOUBLE> XPositions;
      std::vector<AIL_DOUBLE> YPositions;
      std::vector<AIL_DOUBLE> Angles;
      std::vector<AIL_DOUBLE> DetectionScores;
      std::vector<AIL_DOUBLE> FitScores;
      std::vector<AIL_DOUBLE> CoverageScores;
      MagmGetResult(AilSearchResult, M_ALL, M_POSITION_X, XPositions);
      MagmGetResult(AilSearchResult, M_ALL, M_POSITION_Y, YPositions);
      MagmGetResult(AilSearchResult, M_ALL, M_ANGLE, Angles);
      MagmGetResult(AilSearchResult, M_ALL, M_SCORE_DETECTION, DetectionScores);
      MagmGetResult(AilSearchResult, M_ALL, M_SCORE_FIT, FitScores);
      MagmGetResult(AilSearchResult, M_ALL, M_SCORE_COVERAGE, CoverageScores);

      // Print the results for each occurrence found.
      MosPrintf(AIL_TEXT("The model was found in the target image:\n\n"));
      MosPrintf(AIL_TEXT("Result   X Position   Y Position   Angle    ")
                AIL_TEXT("DetectionScore   FitScore   CoverageScore\n\n"));
      for(AIL_INT i = 0; i < NumOccurrences; ++i)
         {
         MosPrintf(AIL_TEXT("%-9i%-13.2f%-13.2f%-9.2f%-17.2f%-11.2f%-11.2f\n"),
                   i, XPositions[i], YPositions[i], Angles[i],
                   DetectionScores[i], FitScores[i], CoverageScores[i]);
         }
      MosPrintf(AIL_TEXT("\nNumber of occurrences found in the target image: %i\n"), NumOccurrences);
      MosPrintf(AIL_TEXT("Search time: %.1f ms\n"), FindTime * 1000.0);

      // Draw green edges and bounding boxes over the occurrences that were found.
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);
      MagmDraw(M_DEFAULT, AilSearchResult, AilGraphicList,
               M_DRAW_EDGES + M_DRAW_BOX, M_ALL, M_DEFAULT);

      // Draw red positions over the occurrences that were found.
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);
      MagmDraw(M_DEFAULT, AilSearchResult, AilGraphicList,
               M_DRAW_POSITION, M_ALL, M_DEFAULT);
      }
   else
      {
      MosPrintf(AIL_TEXT("The model was not found in the target image.\n"));
      }

   // Display the target image.
   MdispSelect(AilTargetDisplay, AilTargetImage);

   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Put the display back to its default state.
   MdispControl(AilModelDisplay, M_WINDOW_INITIAL_SIZE_X, M_DEFAULT);
   MdispControl(AilModelDisplay, M_WINDOW_INITIAL_SIZE_Y, M_DEFAULT);

   // Remove the display.
   MdispSelect(AilModelDisplay , M_NULL);
   MdispSelect(AilTargetDisplay, M_NULL);

   // Free objects.
   MgraFree(AilGraphicList);
   MagmFree(AilFindContext);
   MagmFree(AilSearchResult);
   MbufFree(AilModelImage);
   MbufFree(AilTargetImage);
   }

//****************************************************************************
// Single-definition model example.
//****************************************************************************
void SingleModelExample(AIL_ID AilSystem)
   {
   MosPrintf(AIL_TEXT("This example shows that AGM is able "));
   MosPrintf(AIL_TEXT("to quickly find occurrences\n"));
   MosPrintf(AIL_TEXT("in a large cluttered target image.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   AIL_ID AilModelDisplay;   // Display identifier.
   AIL_ID AilTargetDisplay;  // Display identifier.

   // Allocate the model and target displays
   MdispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, &AilModelDisplay );
   MdispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, &AilTargetDisplay);

   // Set the model and target display's titles
   MdispControl(AilModelDisplay , M_TITLE, AIL_TEXT("Model"));
   MdispControl(AilTargetDisplay, M_TITLE, AIL_TEXT("Target"));

   ExecuteSingleModelExample(AilSystem, AilModelDisplay, AilTargetDisplay, MODEL_IMAGE_0_PATH, TARGET_IMAGE_0_PATH);

   MosPrintf(AIL_TEXT("AGM can also find oriented occurrences.\n\n"));
   ExecuteSingleModelExample(AilSystem, AilModelDisplay, AilTargetDisplay, MODEL_IMAGE_1_PATH, TARGET_IMAGE_1_PATH, 360.0);

   // Free the displays
   MdispFree(AilModelDisplay );
   MdispFree(AilTargetDisplay);
   }

//****************************************************************************
// Composite-definition model example.
//****************************************************************************
void CompositeModelExample(AIL_ID AilSystem)
   {
   AIL_ID AilDisplay;           // Display identifier.
   AIL_ID AilGraphicList;       // Graphic list identifier.
   AIL_ID AilTrainContext;      // Train AGM context identifier.
   AIL_ID AilTrainResult;       // Train AGM result buffer identifier.
   AIL_ID AilFindContext;       // Find context identifier.
   AIL_ID AilSearchResult;      // Find AGM result buffer identifier.
   AIL_ID Regions;              // Graphic list identifier.
   AIL_ID TrainImagesContainer; // Container buffer identifier.

   MosPrintf(AIL_TEXT("This example shows that AGM is able "));
   MosPrintf(AIL_TEXT("to confidently find occurrences with appearance\n"));
   MosPrintf(AIL_TEXT("variation in a complex background "));
   MosPrintf(AIL_TEXT("after training a composite-definition model.\n"));

   // Allocate the display
   MdispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, &AilDisplay);

   // Restore the training images.
   MbufRestore(TRAIN_IMAGES_PATH, AilSystem, &TrainImagesContainer);

   // Print message about training image labels.
   MosPrintf(AIL_TEXT("\n*******************************************************\n"));
   MosPrintf(AIL_TEXT("LOADING LABELED TRAINING IMAGES...\n"));
   MosPrintf(AIL_TEXT("*******************************************************\n"));

   MosPrintf(AIL_TEXT("Training requires labeled images with positive and negative samples.\n"));
   MosPrintf(AIL_TEXT("Positive samples are occurrences delimited by blue boxes and\n"));
   MosPrintf(AIL_TEXT("negative samples are background parts delimited by red boxes.\n"));
   MosPrintf(AIL_TEXT("Typically, when false positives are detected in training images,\n"));
   MosPrintf(AIL_TEXT("they should be used as negative samples to improve the training.\n"));
   MosPrintf(AIL_TEXT("To ease the labeling of images, use the example AgmLabelingTool.\n"));

   // Wait for a key to be pressed.
   MosPrintf(AIL_TEXT("\nPress any key to show the labeled images used in this training.\n"));
   MosGetch();

   // Get the components from the container.
   std::vector<AIL_ID> TrainImages;
   MbufInquireContainer(TrainImagesContainer, M_CONTAINER, M_COMPONENT_LIST, TrainImages);

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &Regions);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, Regions);

   // Display each labeled training image.
   MosPrintf(AIL_TEXT("\nOnly positive samples are given because AGM is able\n"));
   MosPrintf(AIL_TEXT("to automatically generate negative samples.\n\n"));

   AIL_INT NumTrainImage = (AIL_INT)TrainImages.size();
   for(AIL_INT i = 0; i < NumTrainImage; ++i)
      {
      MgraClear(M_DEFAULT, Regions);
      MbufSetRegion(TrainImages[i], Regions, M_DEFAULT, M_EXTRACT, M_DEFAULT);
      MdispSelect(AilDisplay, TrainImages[i]);
      MosPrintf(AIL_TEXT("Training image %i/%i\n"), i + 1, NumTrainImage);
      MosPrintf(AIL_TEXT("Press any key to continue.\n"));
      MosGetch();
      }

   // Disassociate the graphic list from the display and stop displaying the training images.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, M_NULL);
   MdispSelect(AilDisplay, M_NULL);

   // Allocate a find AGM context.
   MagmAlloc(AilSystem, M_GLOBAL_EDGE_BASED_FIND, M_DEFAULT, &AilFindContext);

   // Allocate a find AGM result buffer.
   MagmAllocResult(AilSystem, M_GLOBAL_EDGE_BASED_FIND_RESULT, M_DEFAULT, &AilSearchResult);

   // Allocate a train AGM context.
   MagmAlloc(AilSystem, M_GLOBAL_EDGE_BASED_TRAIN, M_DEFAULT, &AilTrainContext);

   // Allocate a train AGM result buffer.
   MagmAllocResult(AilSystem, M_GLOBAL_EDGE_BASED_TRAIN_RESULT, M_DEFAULT, &AilTrainResult);

   // Define the composite-definition model.
   MagmDefine(AilTrainContext, M_ADD, M_DEFAULT, M_COMPOSITE, M_NULL, M_DEFAULT);

   // Enable the automatic generation of negative labels.
   MagmControl(AilTrainContext, M_AGM_MODEL_INDEX(0), M_NEGATIVE_LABELS_MODE, M_AUTO);

   // Preprocess the train AGM context.
   MagmPreprocess(AilTrainContext, M_DEFAULT);

   // Train the composite-definition model.
   MosPrintf(AIL_TEXT("\n*******************************************************\n"));
   MosPrintf(AIL_TEXT("TRAINING... THIS WILL TAKE SOME TIME...\n"));
   MosPrintf(AIL_TEXT("*******************************************************\n"));
   MagmTrain(AilTrainContext, &TrainImagesContainer, 1, AilTrainResult, M_DEFAULT);

   // Check that the training process completed successfully.
   AIL_INT TrainStatus = -1;
   MagmGetResult(AilTrainResult, M_DEFAULT, M_STATUS, &TrainStatus);
   if(TrainStatus == M_COMPLETE)
      {
      MosPrintf(AIL_TEXT("Training complete!\n"));

      // Ensure that the trained model is valid before copying to the find AGM context.
      AIL_INT TrainedModelStatus = -1;
      MagmGetResult(AilTrainResult, M_AGM_MODEL_INDEX(0), M_STATUS, &TrainedModelStatus);
      if(TrainedModelStatus == M_STATUS_TRAIN_OK)
         {
         // Display all the positive and negative labels on the training images.
         MosPrintf(AIL_TEXT("Here are the training images with the positive labels\n"));
         MosPrintf(AIL_TEXT("and the automatically generated negative labels.\n\n"));

         MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, Regions);
         for(AIL_INT i = 0; i < NumTrainImage; ++i)
            {
            MgraClear(M_DEFAULT, Regions);
            MgraControl(M_DEFAULT, M_COLOR, M_COLOR_BLUE);
            MagmDraw(M_DEFAULT, AilTrainResult, Regions, M_DRAW_POS_RECTANGLES, M_AGM_IMAGE_INDEX(i), M_DEFAULT);
            MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);
            MagmDraw(M_DEFAULT, AilTrainResult, Regions, M_DRAW_NEG_RECTANGLES, M_AGM_IMAGE_INDEX(i), M_DEFAULT);
            MdispSelect(AilDisplay, TrainImages[i]);
            MosPrintf(AIL_TEXT("Training image %i/%i\n"), i + 1, NumTrainImage);
            MosPrintf(AIL_TEXT("Press any key to continue.\n"));
            MosGetch();
            }

         // Disassociate the graphic list from the display and stop displaying the training images.
         MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, M_NULL);
         MdispSelect(AilDisplay, M_NULL);

         // Copy the trained model to a find AGM context.
         MagmCopyResult(AilTrainResult, M_DEFAULT, AilFindContext,
            M_DEFAULT, M_TRAINED_MODEL, M_DEFAULT);
         }
      }

   // Preprocess find AGM context.
   MagmPreprocess(AilFindContext, M_DEFAULT);

   MosPrintf(AIL_TEXT("\n*******************************************************\n"));
   MosPrintf(AIL_TEXT("FINDING WITH THE TRAINED MODEL...\n"));
   MosPrintf(AIL_TEXT("*******************************************************\n"));

   // Restore test images.
   AIL_INT NumberOfImages = 0;
   AIL_STRING FilesToSearch = TEST_IMAGES_DIR_PATH;
   FilesToSearch += AIL_TEXT("*.mim");
   MappFileOperation(M_DEFAULT, FilesToSearch, M_NULL, M_NULL,
                     M_FILE_NAME_FIND + M_NB_ELEMENTS, M_DEFAULT, &NumberOfImages);
   std::vector<AIL_ID> TestImages(NumberOfImages);
   for(AIL_INT i = 0; i < NumberOfImages; i++)
      {
      AIL_STRING Filename;
      MappFileOperation(M_DEFAULT, FilesToSearch, M_NULL, M_NULL,
                        M_FILE_NAME_FIND, i, Filename);
      AIL_STRING FilePath = TEST_IMAGES_DIR_PATH + Filename;
      MbufRestore(FilePath, AilSystem, &TestImages[i]);
      }

   // Allocate a graphic list to hold the subpixel annotations to draw.
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraphicList);

   // Associate the graphic list to the display for annotations.
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicList);

   // Assign the color to draw.
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);
   for(AIL_INT i = 0; i < NumberOfImages; ++i)
      {
      // Find the model in the test image.
      MagmFind(AilFindContext, TestImages[i], AilSearchResult, M_DEFAULT);

      // Get the number of occurrences found.
      AIL_INT NumOccurrences = 0;
      MagmGetResult(AilSearchResult, M_DEFAULT, M_NUMBER + M_TYPE_AIL_INT, &NumOccurrences);

      if(NumOccurrences > 0)
         {
         // Get the results of the search.
         std::vector<AIL_DOUBLE> XPositions;
         std::vector<AIL_DOUBLE> YPositions;
         std::vector<AIL_DOUBLE> DetectionScores;
         std::vector<AIL_DOUBLE> FitScores;
         std::vector<AIL_DOUBLE> CoverageScores;
         MagmGetResult(AilSearchResult, M_ALL, M_POSITION_X, XPositions);
         MagmGetResult(AilSearchResult, M_ALL, M_POSITION_Y, YPositions);
         MagmGetResult(AilSearchResult, M_ALL, M_SCORE_DETECTION, DetectionScores);
         MagmGetResult(AilSearchResult, M_ALL, M_SCORE_FIT, FitScores);
         MagmGetResult(AilSearchResult, M_ALL, M_SCORE_COVERAGE, CoverageScores);

         // Print the results for each occurrence foud.
         MosPrintf(AIL_TEXT("The model was found in the target image:\n\n"));
         MosPrintf(AIL_TEXT("Result   X Position   Y Position   ")
                   AIL_TEXT("DetectionScore   FitScore   CoverageScore\n\n"));
         for(AIL_INT j = 0; j < NumOccurrences; ++j)
            {
            MosPrintf(AIL_TEXT("%-9i%-13.2f%-13.2f%-17.2f%-11.2f%-11.2f\n"),
                      j, XPositions[j], YPositions[j],
                      DetectionScores[j], FitScores[j], CoverageScores[j]);
            }

         // Empty the graphic list.
         MgraClear(M_DEFAULT, AilGraphicList);

         // Draw bounding box
         MagmDraw(M_DEFAULT, AilSearchResult, AilGraphicList, M_DRAW_BOX, M_ALL, M_DEFAULT);
         }
      else
         {
         MosPrintf(AIL_TEXT("The model was not found in the target image.\n"));
         }

      // Display the test image.
      MdispSelect(AilDisplay, TestImages[i]);

      // Wait for a key to be pressed.
      if(i < NumberOfImages - 1)
         {
         MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
         MosGetch();
         }
      }

   // Remove the display.
   MdispSelect(AilDisplay, M_NULL);

   // Free objects.
   for(AIL_INT i = 0; i < NumberOfImages; ++i)
      {
      MbufFree(TestImages[i]);
      }
   MgraFree(AilGraphicList);
   MgraFree(Regions);
   MagmFree(AilTrainContext);
   MagmFree(AilTrainResult);
   MagmFree(AilFindContext);
   MagmFree(AilSearchResult);
   MbufFree(TrainImagesContainer);
   MdispFree(AilDisplay);
   }

```
