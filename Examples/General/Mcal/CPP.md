---
title: "Mcal"
description: "This example demonstrates the usage of the Calibration module."
ms.language: "C++"
---

# Mcal

> This example demonstrates the usage of the Calibration module.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `McalAlloc`, `McalAssociate`, `McalControl`, `McalDraw`, `McalFree`, `McalGetCoordinateSystem`, `McalGrid`, `McalInquire`, `McalSetCoordinateSystem`, `McalTransformImage`, `McalUniform`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MgraAllocList`, `MgraClear`, `MgraControl`, `MgraFree`, `MgraText`, `MmeasAllocMarker`, `MmeasDraw`, `MmeasFindMarker`, `MmeasFree`, `MmeasGetResult`, `MmeasSetMarker`, `MmeasSetScore`, `MmetAddFeature`, `MmetAlloc`, `MmetAllocResult`, `MmetCalculate`, `MmetDraw`, `MmetFree`, `MmetGetResult`, `MmetSetRegion`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Electronics, Machining, Applications, Measuring, Modules, Buffer, Display, Graphics, Measurement, Metrology, Calibration, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MCal.cpp
//
// Description: This program uses the Calibration module to:
//              - Remove distortion and then take measurements in world units using a 2D 
//                calibration.
//              - Perform a 3D calibration to take measurements at several known elevations.
//              - Calibrate a scene using a partial calibration grid that has a 2D code 
//                fiducial.
//
//              Printable calibration grids in PDF format can be found in your
//              "Aurora Imaging Library/##/Images/" directory.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// Example selection. 
#define RUN_LINEAR_CALIBRATION_EXAMPLE       M_YES
#define RUN_TSAI_CALIBRATION_EXAMPLE         M_YES
#define RUN_PARTIAL_GRID_CALIBRATION_EXAMPLE M_YES

// Grid offset specifications. 
#define GRID_OFFSET_X          0
#define GRID_OFFSET_Y          0
#define GRID_OFFSET_Z          0

// Example functions declarations.   
void LinearInterpolationCalibration(AIL_ID AilSystem, AIL_ID AilDisplay);
void TsaiCalibration(AIL_ID AilSystem, AIL_ID AilDisplay);
void PartialGridCalibration(AIL_ID AilSystem, AIL_ID AilDisplay);

//  Main function. 
int MosMain(void)
   {
   AIL_ID AilApplication, // Application identifier.
          AilSystem,      // System Identifier.     
          AilDisplay;     // Display identifier.    

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Print module name. 
   MosPrintf(AIL_TEXT("CALIBRATION MODULE:\n"));
   MosPrintf(AIL_TEXT("-------------------\n\n"));

#if (RUN_LINEAR_CALIBRATION_EXAMPLE)
   LinearInterpolationCalibration(AilSystem, AilDisplay);
#endif

#if (RUN_TSAI_CALIBRATION_EXAMPLE)
   TsaiCalibration(AilSystem, AilDisplay);
#endif

#if (RUN_PARTIAL_GRID_CALIBRATION_EXAMPLE)
   PartialGridCalibration(AilSystem, AilDisplay);
#endif

   // Free defaults.     
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }

//*****************************************************************************
// Linear interpolation example. 
//*****************************************************************************
// Source image files specification. 
#define GRID_IMAGE_FILE        M_IMAGE_PATH AIL_TEXT("CalGrid.mim")
#define BOARD_IMAGE_FILE       M_IMAGE_PATH AIL_TEXT("CalBoard.mim")

// World description of the calibration grid. 
#define GRID_ROW_SPACING       1
#define GRID_COLUMN_SPACING    1
#define GRID_ROW_NUMBER       18
#define GRID_COLUMN_NUMBER    25

// Measurement boxes specification. 
#define MEAS_BOX_POS_X1       55
#define MEAS_BOX_POS_Y1       24
#define MEAS_BOX_WIDTH1        7
#define MEAS_BOX_HEIGHT1     425

#define MEAS_BOX_POS_X2      225
#define MEAS_BOX_POS_Y2       11
#define MEAS_BOX_WIDTH2        7
#define MEAS_BOX_HEIGHT2     450

// Specification of the stripes' constraints. 
#define WIDTH_APPROXIMATION  410
#define WIDTH_VARIATION       25
#define MIN_EDGE_VALUE     5

void LinearInterpolationCalibration(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID  AilImage,          // Image buffer identifier.       
           AilOverlayImage,   // Overlay image.                 
           AilCalibration,    // Calibration identifier.        
           MeasMarker1,       // Measurement marker identifier. 
           MeasMarker2;       // Measurement marker identifier. 
   AIL_DOUBLE  WorldDistance1,  WorldDistance2;
   AIL_DOUBLE  PixelDistance1,  PixelDistance2;
   AIL_DOUBLE  PosX1, PosY1, PosX2, PosY2, PosX3, PosY3, PosX4, PosY4;
   AIL_INT     CalibrationStatus;

   // Clear the display 
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);

   // Restore source image into an automatically allocated image buffer. 
   MbufRestore(GRID_IMAGE_FILE, AilSystem, &AilImage);

   // Display the image buffer. 
   MdispSelect(AilDisplay, AilImage);

   // Prepare for overlay annotation. 
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);

   // Pause to show the original image. 
   MosPrintf(AIL_TEXT("\nLINEAR INTERPOLATION CALIBRATION:\n"));
   MosPrintf(AIL_TEXT("---------------------------------\n\n"));
   MosPrintf(AIL_TEXT("The displayed grid has been grabbed with a high distortion\n"));
   MosPrintf(AIL_TEXT("camera and will be used to calibrate the camera.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate a camera calibration context. 
   McalAlloc(AilSystem, M_DEFAULT, M_DEFAULT, &AilCalibration);

   // Calibrate the camera with the image of the grid and its world description. 
   McalGrid(AilCalibration, AilImage,
            GRID_OFFSET_X, GRID_OFFSET_Y, GRID_OFFSET_Z,
            GRID_ROW_NUMBER, GRID_COLUMN_NUMBER,
            GRID_ROW_SPACING, GRID_COLUMN_SPACING,
            M_DEFAULT, M_DEFAULT);

   McalInquire(AilCalibration, M_CALIBRATION_STATUS + M_TYPE_AIL_INT, &CalibrationStatus);
   if( CalibrationStatus == M_CALIBRATED )
      {
      // Perform a first image transformation with the calibration grid. 
      McalTransformImage(AilImage, AilImage, AilCalibration,
                         M_BILINEAR + M_OVERSCAN_CLEAR, M_DEFAULT, M_DEFAULT);

      // Pause to show the corrected image of the grid. 
      MosPrintf(AIL_TEXT("The camera has been calibrated and the image of the grid\n"));
      MosPrintf(AIL_TEXT("has been transformed to remove its distortions.\n"));
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Read the image of the board and associate the calibration to the image. 
      MbufLoad(BOARD_IMAGE_FILE, AilImage);
      McalAssociate(AilCalibration, AilImage, M_DEFAULT);

      // Allocate the measurement markers. 
      MmeasAllocMarker(AilSystem, M_STRIPE, M_DEFAULT, &MeasMarker1);
      MmeasAllocMarker(AilSystem, M_STRIPE, M_DEFAULT, &MeasMarker2);

      // Set the markers' measurement search region. 
      MmeasSetMarker(MeasMarker1, M_BOX_ORIGIN, MEAS_BOX_POS_X1, MEAS_BOX_POS_Y1);
      MmeasSetMarker(MeasMarker1, M_BOX_SIZE, MEAS_BOX_WIDTH1, MEAS_BOX_HEIGHT1);
      MmeasSetMarker(MeasMarker2, M_BOX_ORIGIN, MEAS_BOX_POS_X2, MEAS_BOX_POS_Y2);
      MmeasSetMarker(MeasMarker2, M_BOX_SIZE, MEAS_BOX_WIDTH2, MEAS_BOX_HEIGHT2);

      // Set markers' orientation. 
      MmeasSetMarker(MeasMarker1, M_ORIENTATION, M_HORIZONTAL, M_NULL);
      MmeasSetMarker(MeasMarker2, M_ORIENTATION, M_HORIZONTAL, M_NULL);

      // Set markers' settings to locate the largest stripe within the range
      // [WIDTH_APPROXIMATION - WIDTH_VARIATION,
      //  WIDTH_APPROXIMATION + WIDTH_VARIATION],
      // and with an edge strength over MIN_EDGE_VALUE. 
      MmeasSetMarker(MeasMarker1, M_EDGEVALUE_MIN, MIN_EDGE_VALUE, M_NULL);

      // Remove the default strength characteristic score mapping. 
      MmeasSetScore(MeasMarker1, M_STRENGTH_SCORE,
                                 0.0,
                                 0.0,
                                 M_MAX_POSSIBLE_VALUE,
                                 M_MAX_POSSIBLE_VALUE,
                                 M_DEFAULT,
                                 M_DEFAULT,
                                 M_DEFAULT);

      // Add a width characteristic score mapping (increasing ramp)
      // to find the largest stripe within a given range.
      //
      // Width score mapping to find the largest stripe within a given
      // width range ]Wmin, Wmax]:
      //
      //    Score
      //       ^
      //       |         /|
      //       |       /  |
      //       |     /    |
      //       +---------------> Width
      //           Wmin  Wmax

      MmeasSetScore(MeasMarker1, M_STRIPE_WIDTH_SCORE,
                                 WIDTH_APPROXIMATION - WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 M_DEFAULT,
                                 M_PIXEL,
                                 M_DEFAULT);

      // Set the same settings for the second marker. 
      MmeasSetMarker(MeasMarker2, M_EDGEVALUE_MIN, MIN_EDGE_VALUE, M_NULL);

      MmeasSetScore(MeasMarker2, M_STRENGTH_SCORE,
                                 0.0,
                                 0.0,
                                 M_MAX_POSSIBLE_VALUE,
                                 M_MAX_POSSIBLE_VALUE,
                                 M_DEFAULT,
                                 M_DEFAULT,
                                 M_DEFAULT);

      MmeasSetScore(MeasMarker2, M_STRIPE_WIDTH_SCORE,
                                 WIDTH_APPROXIMATION - WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 WIDTH_APPROXIMATION + WIDTH_VARIATION,
                                 M_DEFAULT,
                                 M_PIXEL,
                                 M_DEFAULT);

      // Find and measure the position and width of the board. 
      MmeasFindMarker(M_DEFAULT, AilImage, MeasMarker1, M_STRIPE_WIDTH+M_POSITION);
      MmeasFindMarker(M_DEFAULT, AilImage, MeasMarker2, M_STRIPE_WIDTH+M_POSITION);

      // Get the world width of the two markers. 
      MmeasGetResult(MeasMarker1, M_STRIPE_WIDTH, &WorldDistance1, M_NULL);
      MmeasGetResult(MeasMarker2, M_STRIPE_WIDTH, &WorldDistance2, M_NULL);

      // Get the pixel width of the two markers. 
      MmeasSetMarker(MeasMarker1, M_RESULT_OUTPUT_UNITS, M_PIXEL, M_NULL);
      MmeasSetMarker(MeasMarker2, M_RESULT_OUTPUT_UNITS, M_PIXEL, M_NULL);
      MmeasGetResult(MeasMarker1, M_STRIPE_WIDTH, &PixelDistance1, M_NULL);
      MmeasGetResult(MeasMarker2, M_STRIPE_WIDTH, &PixelDistance2, M_NULL);

      // Get the edges position in pixel to draw the annotations. 
      MmeasGetResult(MeasMarker1, M_POSITION+M_EDGE_FIRST,  &PosX1, &PosY1);
      MmeasGetResult(MeasMarker1, M_POSITION+M_EDGE_SECOND, &PosX2, &PosY2);
      MmeasGetResult(MeasMarker2, M_POSITION+M_EDGE_FIRST,  &PosX3, &PosY3);
      MmeasGetResult(MeasMarker2, M_POSITION+M_EDGE_SECOND, &PosX4, &PosY4);

      // Draw the measurement indicators on the image.  
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_YELLOW);
      MmeasDraw(M_DEFAULT, MeasMarker1, AilOverlayImage, M_DRAW_WIDTH, M_DEFAULT, M_RESULT);
      MmeasDraw(M_DEFAULT, MeasMarker2, AilOverlayImage, M_DRAW_WIDTH, M_DEFAULT, M_RESULT);

      MgraControl(M_DEFAULT, M_BACKCOLOR, M_COLOR_BLACK);
      MgraText(M_DEFAULT, AilOverlayImage, (AIL_INT)(PosX1+0.5-40),
         (AIL_INT)((PosY1+0.5)+((PosY2 - PosY1)/2.0)), AIL_TEXT(" Distance 1 "));
      MgraText(M_DEFAULT, AilOverlayImage, (AIL_INT)(PosX3+0.5-40),
         (AIL_INT)((PosY3+0.5)+((PosY4 - PosY3)/2.0)), AIL_TEXT(" Distance 2 "));

      // Pause to show the original image and the measurement results. 
      MosPrintf(AIL_TEXT("A distorted image grabbed with the same camera was loaded and\n"));
      MosPrintf(AIL_TEXT("calibrated measurements were done to evaluate ")
                                       AIL_TEXT("the board dimensions.\n"));
      MosPrintf(AIL_TEXT("\n========================================================\n"));
      MosPrintf(AIL_TEXT("                      Distance 1          Distance 2 \n"));
      MosPrintf(AIL_TEXT("--------------------------------------------------------\n"));
      MosPrintf(AIL_TEXT(" Calibrated unit:   %8.2lf cm           %6.2lf cm    \n"),
                                                         WorldDistance1, WorldDistance2);
      MosPrintf(AIL_TEXT(" Uncalibrated unit: %8.2lf pixels       %6.2lf pixels\n"),
                                                         PixelDistance1, PixelDistance2);
      MosPrintf(AIL_TEXT("========================================================\n\n"));
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Clear the display overlay. 
      MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);

      // Read the image of the PCB. 
      MbufLoad(BOARD_IMAGE_FILE, AilImage);

      // Transform the image of the board. 
      McalTransformImage(AilImage, AilImage, AilCalibration,
         M_BILINEAR+M_OVERSCAN_CLEAR, M_DEFAULT, M_DEFAULT);

      // show the transformed image of the board. 
      MosPrintf(AIL_TEXT("The image was corrected to remove its distortions.\n"));

      // Free measurement markers. 
      MmeasFree(MeasMarker1);
      MmeasFree(MeasMarker2);
      }
   else
      {
      MosPrintf(AIL_TEXT("Calibration generated an exception.\n"));
      MosPrintf(AIL_TEXT("See User Guide to resolve the situation.\n\n"));
      }

   // Wait for a key to be pressed.  
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Free all allocations. 
   McalFree(AilCalibration);
   MbufFree(AilImage);
   }


//*****************************************************************************
// Tsai example. 
//*****************************************************************************
// Source image files specification. 
#define GRID_ORIGINAL_IMAGE_FILE     M_IMAGE_PATH AIL_TEXT("CalGridOriginal.mim")
#define OBJECT_ORIGINAL_IMAGE_FILE   M_IMAGE_PATH AIL_TEXT("CalObjOriginal.mim")
#define OBJECT_MOVED_IMAGE_FILE      M_IMAGE_PATH AIL_TEXT("CalObjMoved.mim")

// World description of the calibration grid. 
#define GRID_ORG_ROW_SPACING       1.5
#define GRID_ORG_COLUMN_SPACING    1.5
#define GRID_ORG_ROW_NUMBER        12
#define GRID_ORG_COLUMN_NUMBER     13
#define GRID_ORG_OFFSET_X          0
#define GRID_ORG_OFFSET_Y          0 
#define GRID_ORG_OFFSET_Z          0

// Region parameters for metrology 
#define MEASURED_CIRCLE_LABEL       1
#define RING1_POS_X                2.3
#define RING1_POS_Y                3.9
#define RING2_POS_X                10.7
#define RING2_POS_Y                11.1

#define RING_START_RADIUS          1.25
#define RING_END_RADIUS            2.3

// measured plane position 
#define RING_THICKNESS             0.175
#define STEP_THICKNESS             4.0

// Color definitions 
#define ABSOLUTE_COLOR   M_RGB888(255,0,0)
#define RELATIVE_COLOR   M_RGB888(0,255,0)
#define REGION_COLOR     M_RGB888(0,100,255)
#define FEATURE_COLOR    M_RGB888(255,0,255)

// Functions declarations for this example. 
void MeasureRing(AIL_ID AilSystem, AIL_ID AilOverlayImage, AIL_ID AilImage, 
                 AIL_DOUBLE MeasureRing1X, AIL_DOUBLE MeasureRing1Y );
void ShowCameraInformation(AIL_ID AilCalibration);

void TsaiCalibration(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID  AilImage,          // Image buffer identifier.         
           AilOverlayImage,   // Overlay image buffer identifier. 
           AilCalibration;    // Calibration identifier.          

   AIL_INT CalibrationStatus;          

   // Restore source image into an automatically allocated image buffer. 
   MbufRestore(GRID_ORIGINAL_IMAGE_FILE, AilSystem, &AilImage);

   // Display the image buffer. 
   MdispSelect(AilDisplay, AilImage);
   // Prepare for overlay annotation. 
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);

   // Pause to show the original image. 
   MosPrintf(AIL_TEXT("\nTSAI BASED CALIBRATION:\n"));
   MosPrintf(AIL_TEXT("-----------------------\n\n"));
   MosPrintf(AIL_TEXT("The displayed grid has been grabbed with a high perspective\n"));
   MosPrintf(AIL_TEXT("camera and will be used to calibrate the camera.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate a camera calibration context. 
   McalAlloc(AilSystem, M_TSAI_BASED, M_DEFAULT, &AilCalibration);

   // Calibrate the camera with the image of the grid and its world description. 
   McalGrid(AilCalibration, AilImage,
            GRID_ORG_OFFSET_X, GRID_ORG_OFFSET_Y, GRID_ORG_OFFSET_Z,
            GRID_ORG_ROW_NUMBER, GRID_ORG_COLUMN_NUMBER,
            GRID_ORG_ROW_SPACING, GRID_ORG_COLUMN_SPACING,
            M_DEFAULT, M_DEFAULT);

   // Verify if the camera calibration is successful. 
   McalInquire(AilCalibration, M_CALIBRATION_STATUS + M_TYPE_AIL_INT, &CalibrationStatus);
   if( CalibrationStatus == M_CALIBRATED )
      {
      // Display the world absolute coordinate system 
      MgraControl(M_DEFAULT, M_COLOR, ABSOLUTE_COLOR);
      McalDraw(M_DEFAULT, AilCalibration, AilOverlayImage, M_DRAW_ABSOLUTE_COORDINATE_SYSTEM+M_DRAW_AXES, M_DEFAULT, M_DEFAULT);

      // Print camera information 
      MosPrintf(AIL_TEXT("The camera has been calibrated.\n"));
      MosPrintf(AIL_TEXT("The world absolute coordinate system is shown in red.\n\n"));
      ShowCameraInformation(AilCalibration);

      // Load source image into an image buffer. 
      MbufLoad(OBJECT_ORIGINAL_IMAGE_FILE, AilImage);

      // Associate the calibration to the image 
      McalAssociate(AilCalibration, AilImage, M_DEFAULT);
      
      // Set the offset of the camera calibration plane. 
      // This moves the relative origin at the top of the first metallic part 
      McalSetCoordinateSystem(AilImage,
         M_RELATIVE_COORDINATE_SYSTEM,
         M_ABSOLUTE_COORDINATE_SYSTEM,
         M_TRANSLATION + M_ASSIGN,
         M_NULL,
         0, 0, -RING_THICKNESS,
         M_DEFAULT);

      // Display the world relative coordinate system 
      MgraControl(M_DEFAULT, M_COLOR, RELATIVE_COLOR);
      McalDraw(M_DEFAULT, AilImage, AilOverlayImage, M_DRAW_RELATIVE_COORDINATE_SYSTEM + M_DRAW_FRAME, M_DEFAULT, M_DEFAULT);

      // Measure the first circle. 
      MosPrintf(AIL_TEXT("The relative coordinate system (shown in green) was translated by %.3f cm\n"), -RING_THICKNESS);
      MosPrintf(AIL_TEXT("in z to align it with the top of the first metallic part.\n"));
      MeasureRing(AilSystem, AilOverlayImage, AilImage, RING1_POS_X, RING1_POS_Y );
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Modify the offset of the camera calibration plane 
      // This moves the relative origin at the top of the second metallic part  
      McalSetCoordinateSystem(AilImage,
         M_RELATIVE_COORDINATE_SYSTEM,
         M_ABSOLUTE_COORDINATE_SYSTEM,
         M_TRANSLATION + M_COMPOSE_WITH_CURRENT,
         M_NULL,
         0, 0, -STEP_THICKNESS,
         M_DEFAULT);

      // Clear the overlay 
      MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);
      // Display the world absolute coordinate system 
      MgraControl(M_DEFAULT, M_COLOR, ABSOLUTE_COLOR);
      McalDraw(M_DEFAULT, AilCalibration, AilOverlayImage, M_DRAW_ABSOLUTE_COORDINATE_SYSTEM + M_DRAW_AXES, M_DEFAULT, M_DEFAULT);
      // Display the world relative coordinate system 
      MgraControl(M_DEFAULT, M_COLOR, RELATIVE_COLOR);
      McalDraw(M_DEFAULT, AilImage, AilOverlayImage, M_DRAW_RELATIVE_COORDINATE_SYSTEM + M_DRAW_FRAME, M_DEFAULT, M_DEFAULT);

      // Measure the second circle. 
      MosPrintf(AIL_TEXT("The relative coordinate system was translated by another %.3f cm\n"), -STEP_THICKNESS);
      MosPrintf(AIL_TEXT("in z to align it with the top of the second metallic part.\n"));
      MeasureRing(AilSystem, AilOverlayImage, AilImage, RING2_POS_X, RING2_POS_Y );
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();
      }
   else
      {
      MosPrintf(AIL_TEXT("Calibration generated an exception.\n"));
      MosPrintf(AIL_TEXT("See User Guide to resolve the situation.\n\n"));
      }

   // Free all allocations. 
   McalFree(AilCalibration);
   MbufFree(AilImage);
   }

// Measuring function with AilMetrology module 
void MeasureRing(AIL_ID AilSystem, AIL_ID AilOverlayImage, AIL_ID AilImage, 
                    AIL_DOUBLE MeasureRingX, AIL_DOUBLE MeasureRingY )
   {
   AIL_ID  AilMetrolContext,  // Metrology Context 
           AilMetrolResult;   // Metrology Result  
           
   AIL_DOUBLE Value;

   // Allocate metrology context and result. 
   MmetAlloc(AilSystem, M_DEFAULT, &AilMetrolContext);
   MmetAllocResult(AilSystem, M_DEFAULT, &AilMetrolResult);

   // Add a first measured segment feature to context and set its search region 
   MmetAddFeature(AilMetrolContext, M_MEASURED, M_CIRCLE, MEASURED_CIRCLE_LABEL,
                  M_DEFAULT, M_NULL, M_NULL, 0, M_DEFAULT);

   MmetSetRegion(AilMetrolContext, M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), 
                 M_DEFAULT, M_RING,
                 MeasureRingX, MeasureRingY,
                 RING_START_RADIUS, RING_END_RADIUS,
                 M_NULL, M_NULL );

   // Calculate 
   MmetCalculate(AilMetrolContext, AilImage, AilMetrolResult, M_DEFAULT);

   // Draw region 
   MgraControl(M_DEFAULT, M_COLOR, REGION_COLOR);
   MmetDraw(M_DEFAULT, AilMetrolResult, AilOverlayImage, M_DRAW_REGION, M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), M_DEFAULT);

   // Draw the circle 
   MgraControl(M_DEFAULT, M_COLOR, FEATURE_COLOR);
   MmetDraw(M_DEFAULT, AilMetrolResult, AilOverlayImage, M_DRAW_FEATURE, M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), M_DEFAULT);

   MmetGetResult(AilMetrolResult, M_FEATURE_LABEL(MEASURED_CIRCLE_LABEL), M_RADIUS, &Value);
   MosPrintf(AIL_TEXT("The large circle's radius was measured: %.3f cm.\n"), Value);

   // Free all allocations. 
   MmetFree(AilMetrolResult);
   MmetFree(AilMetrolContext);
   }

// Print the current camera position and orientation  
void ShowCameraInformation(AIL_ID AilCalibration)
   {
   AIL_DOUBLE CameraPosX,
              CameraPosY,
              CameraPosZ,
              CameraYaw,
              CameraPitch,
              CameraRoll;

   McalGetCoordinateSystem(AilCalibration,
                           M_CAMERA_COORDINATE_SYSTEM,
                           M_ABSOLUTE_COORDINATE_SYSTEM,
                           M_TRANSLATION,
                           M_NULL,
                           &CameraPosX, &CameraPosY, &CameraPosZ, 
                           M_NULL);

   McalGetCoordinateSystem(AilCalibration,
                           M_CAMERA_COORDINATE_SYSTEM,
                           M_ABSOLUTE_COORDINATE_SYSTEM,
                           M_ROTATION_YXZ,
                           M_NULL,
                           &CameraYaw, &CameraPitch, &CameraRoll, 
                           M_NULL);

   // Pause to show the camera position and orientation. 
   MosPrintf(AIL_TEXT("Camera position in cm:          (x, y, z)           ")
             AIL_TEXT("(%.2f, %.2f, %.2f)\n"), CameraPosX, CameraPosY, CameraPosZ);
   MosPrintf(AIL_TEXT("Camera orientation in degrees:  (yaw, pitch, roll)  ")
             AIL_TEXT("(%.2f, %.2f, %.2f)\n"), CameraYaw, CameraPitch, CameraRoll);
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();
   }

//*****************************************************************************
// Partial grid example. 
//*****************************************************************************
// Source image files specification. 
#define PARTIAL_GRID_IMAGE_FILE        M_IMAGE_PATH AIL_TEXT("PartialGrid.mim")

// Definition of the region to correct. 
#define CORRECTED_SIZE_X         60.0
#define CORRECTED_SIZE_Y         50.0
#define CORRECTED_OFFSET_X       -35.0
#define CORRECTED_OFFSET_Y       -5.0
#define CORRECTED_IMAGE_SIZE_X   512

// Functions declarations for this example. 
void DrawGridInfo(AIL_ID AilGraList, AIL_CONST_TEXT_PTR InfoTag, AIL_DOUBLE Value,
                  AIL_INT LineOffsetY, AIL_CONST_TEXT_PTR Units);

void PartialGridCalibration(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID  AilImage,                // Image buffer identifier.   
           AilCorrectedImage,       // Corrected image identifier.
           AilGraList,              // Graphic list identifier.   
           AilCalibration;          // Calibration identifier.    

   AIL_INT       CalibrationStatus, ImageType,CorrectedImageSizeY;
   AIL_DOUBLE    RowSpacing, ColumnSpacing, CorrectedPixelSize;
   AIL_TEXT_CHAR UnitName[M_GRID_UNIT_SHORT_NAME_MAX_SIZE];

   // Clear the display 
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);

   // Allocate a graphic list and associate it to the display. 
   MgraAllocList(AilSystem, M_DEFAULT, &AilGraList);
   MdispControl(AilDisplay, M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraList);

   // Restore source image into an automatically allocated image buffer. 
   MbufRestore(PARTIAL_GRID_IMAGE_FILE, AilSystem, &AilImage);
   MbufInquire(AilImage, M_TYPE, &ImageType);

   // Display the image buffer. 
   MdispSelect(AilDisplay, AilImage);

   // Pause to show the partial grid image. 
   MosPrintf(AIL_TEXT("\nPARTIAL GRID CALIBRATION:\n"));
   MosPrintf(AIL_TEXT("-------------------------\n\n"));
   MosPrintf(AIL_TEXT("A camera will be calibrated using a rectangular grid that\n"));
   MosPrintf(AIL_TEXT("is only partially visible in the camera's field of view.\n"));
   MosPrintf(AIL_TEXT("The 2D code in the center is used as a fiducial to retrieve\n"));
   MosPrintf(AIL_TEXT("the characteristics of the calibration grid.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Allocate the calibration object. 
   McalAlloc(AilSystem, M_TSAI_BASED, M_DEFAULT, &AilCalibration);

   // Set the calibration to calibrate a partial grid with fiducial. 
   McalControl(AilCalibration, M_GRID_PARTIAL, M_ENABLE);
   McalControl(AilCalibration, M_GRID_FIDUCIAL, M_DATAMATRIX);

   // Calibrate the camera with the partial grid with fiducial. 
   McalGrid(AilCalibration, AilImage,
            GRID_OFFSET_X, GRID_OFFSET_Y, GRID_OFFSET_Z,
            M_UNKNOWN, M_UNKNOWN, M_FROM_FIDUCIAL, M_FROM_FIDUCIAL,
            M_DEFAULT, M_CHESSBOARD_GRID);

   McalInquire(AilCalibration, M_CALIBRATION_STATUS + M_TYPE_AIL_INT, &CalibrationStatus);
   if(CalibrationStatus == M_CALIBRATED)
      {
      // Draw the absolute coordinate system. 
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);
      McalDraw(M_DEFAULT, AilCalibration, AilGraList, M_DRAW_ABSOLUTE_COORDINATE_SYSTEM,
         M_DEFAULT, M_DEFAULT);

      // Draw a box around the fiducial. 
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_CYAN);
      McalDraw(M_DEFAULT, AilCalibration, AilGraList, M_DRAW_FIDUCIAL_BOX,
         M_DEFAULT, M_DEFAULT);

      // Get the information of the grid read from the fiducial. 
      McalInquire(AilCalibration, M_ROW_SPACING, &RowSpacing);
      McalInquire(AilCalibration, M_COLUMN_SPACING, &ColumnSpacing);
      McalInquire(AilCalibration, M_GRID_UNIT_SHORT_NAME, UnitName);

      // Draw the information of the grid read from the fiducial. 
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);
      MgraControl(M_DEFAULT, M_INPUT_UNITS, M_DISPLAY);
      DrawGridInfo(AilGraList, AIL_TEXT("Row spacing"), RowSpacing, 0, UnitName);
      DrawGridInfo(AilGraList, AIL_TEXT("Col spacing"), ColumnSpacing, 1, UnitName);

      // Pause to show the calibration result. 
      MosPrintf(AIL_TEXT("The camera has been calibrated.\n\n"));
      MosPrintf(AIL_TEXT("The grid information read is displayed.\n"));
      MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
      MosGetch();

      // Calculate the pixel size and size Y of the corrected image. 
      CorrectedPixelSize = CORRECTED_SIZE_X / CORRECTED_IMAGE_SIZE_X;
      CorrectedImageSizeY = (AIL_INT)(CORRECTED_SIZE_Y / CorrectedPixelSize);

      // Allocate the corrected image. 
      MbufAlloc2d(AilSystem, CORRECTED_IMAGE_SIZE_X, CorrectedImageSizeY, ImageType,
         M_IMAGE + M_PROC + M_DISP, &AilCorrectedImage);

      // Calibrate the corrected image. 
      McalUniform(AilCorrectedImage, CORRECTED_OFFSET_X, CORRECTED_OFFSET_Y,
         CorrectedPixelSize, CorrectedPixelSize, 0.0, M_DEFAULT);

      // Correct the calibrated image. 
      McalTransformImage(AilImage, AilCorrectedImage, AilCalibration,
         M_BILINEAR + M_OVERSCAN_CLEAR, M_DEFAULT,
         M_WARP_IMAGE + M_USE_DESTINATION_CALIBRATION);

      // Select the corrected image on the display. 
      MgraClear(M_DEFAULT, AilGraList);
      MdispSelect(AilDisplay, AilCorrectedImage);

      // Pause to show the corrected image. 
      MosPrintf(AIL_TEXT("A sub-region of the grid was selected and transformed\n"));
      MosPrintf(AIL_TEXT("to remove the distortions.\n"));
      MosPrintf(AIL_TEXT("The sub-region dimensions and position are:\n"));
      MosPrintf(AIL_TEXT("   Size X  : % 3.3g %s\n"), CORRECTED_SIZE_X, UnitName);
      MosPrintf(AIL_TEXT("   Size Y  : % 3.3g %s\n"), CORRECTED_SIZE_Y, UnitName);
      MosPrintf(AIL_TEXT("   Offset X: % 3.3g %s\n"), CORRECTED_OFFSET_X, UnitName);
      MosPrintf(AIL_TEXT("   Offset Y: % 3.3g %s\n"), CORRECTED_OFFSET_Y, UnitName);

      // Wait for a key to be pressed. 
      MosPrintf(AIL_TEXT("Press any key to quit.\n\n"));
      MosGetch();

      MbufFree(AilCorrectedImage);
      }
   else
      {
      MosPrintf(AIL_TEXT("Calibration generated an exception.\n"));
      MosPrintf(AIL_TEXT("See User Guide to resolve the situation.\n\n"));
      MosPrintf(AIL_TEXT("Press any key to quit.\n\n"));
      MosGetch();
      }

   // Free all allocations. 
   McalFree(AilCalibration);
   MbufFree(AilImage);
   MgraFree(AilGraList);
   }

// Definition of the parameters for the drawing of the grid info 
#define LINE_HEIGHT      16
#define MAX_INFO_SIZE    64

// Draw an information of the grid. 
void DrawGridInfo(AIL_ID AilGraList, AIL_CONST_TEXT_PTR InfoTag,  AIL_DOUBLE Value,
                  AIL_INT LineOffsetY, AIL_CONST_TEXT_PTR Units)
   {
   AIL_TEXT_CHAR Info[MAX_INFO_SIZE];
   MosSprintf(Info, MAX_INFO_SIZE, AIL_TEXT("%s: %.3g %s"), InfoTag, Value, Units);
   MgraText(M_DEFAULT, AilGraList, 0, LineOffsetY * LINE_HEIGHT, Info);
   }

```
