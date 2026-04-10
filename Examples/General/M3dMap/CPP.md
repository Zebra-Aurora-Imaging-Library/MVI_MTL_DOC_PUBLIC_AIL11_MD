---
title: "M3dMap"
description: "This program inspects a wood surface using laser profiling to find any depth defects and computes the volume of a cookie. Note: Printable calibration grids in PDF format can be found in your \"Zebra Imaging/Images/\" directory. Note: When considering a laser-based 3D reconstruction system, the file \"3D Setup Helper.xls\" can be used to accelerate prototyping by choosing an adequate hardware configuration (angle, distance, lens, camera, ...). The file is located in your \"Zebra Imaging/Tools/\" directory."
ms.language: "C++"
---

# M3dMap

> This program inspects a wood surface using laser profiling to find any depth defects and computes the volume of a cookie. Note: Printable calibration grids in PDF format can be found in your "Zebra Imaging/Images/" directory. Note: When considering a laser-based 3D reconstruction system, the file "3D Setup Helper.xls" can be used to accelerate prototyping by choosing an adequate hardware configuration (angle, distance, lens, camera, ...). The file is located in your "Zebra Imaging/Tools/" directory.

**Language:** C++

**Functions used:** `M3dmapAddScan`, `M3dmapAlloc`, `M3dmapAllocResult`, `M3dmapCalibrate`, `M3dmapControl`, `M3dmapCopyResult`, `M3dmapFree`, `M3dmapInquire`, `M3ddispControl`, `MappAlloc`, `MappFree`, `MappTimer`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobGetResult`, `MbufAlloc1d`, `MbufAlloc2d`, `MbufAllocColor`, `MbufAllocContainer`, `MbufClear`, `MbufCopy`, `MbufCopyColor2d`, `MbufDiskInquire`, `MbufFree`, `MbufControlContainer`, `MbufImportSequence`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `McalAlloc`, `McalDraw`, `McalFree`, `McalGrid`, `McalInquire`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispLut`, `MdispSelect`, `MgenLutRamp`, `MgraControl`, `MgraText`, `MimBinarize`, `MimControl`, `MimConvert`, `MimOpen`, `MsysAlloc`, `MsysFree`, `M3ddispAlloc`, `M3ddispFree`, `M3ddispSelect`, `M3ddispSetView`, `M3dgeoAlloc`, `M3dgeoFree`, `M3dgeoPlane`, `M3dimAlloc`, `M3dimCalibrateDepthMap`, `M3dimControl`, `M3dimCrop`, `M3dimFillGaps`, `M3dimFree`, `M3dimProject`, `M3dmetVolume`, `MappControl`

**Categories:** Overview, General, Industries, Machining, Food and Beverage, Applications, Inspecting/Proofing/Verifying, 3D profiling, Modules, Buffer, Display, Graphics, Image Processing, Calibration, Blob Analysis, 3D Map, 3D Display, 3D Graphics, 3D Image Processing, 3D Geometry, 3D Metrology, What's New, AIL 10.0 SP4, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    m3dmap.cpp
//
// Description: This program inspects a wood surface using 
//              sheet-of-light profiling (laser) to find any depth defects.
//
//              Printable calibration grids in PDF format can be found in your
//              "Aurora Imaging Library/##/Images/" directory.
//              
//              When considering a laser-based 3D reconstruction system, the file "3D Setup Helper.xls"
//              can be used to accelerate prototyping by choosing an adequate hardware configuration
//              (angle, distance, lens, camera, ...). The file is located in your
//              "Aurora Imaging Library/##/Tools/" directory.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>
#include <math.h>

// Example functions declarations.   
void DepthCorrectionExample(AIL_ID AilSystem, AIL_ID AilDisplay);
void CalibratedCameraExample(AIL_ID AilSystem, AIL_ID AilDisplay);

// Utility functions declarations.   
void PerformBlobAnalysis(AIL_ID AilSystem,
                         AIL_ID AilDisplay,
                         AIL_ID AilOverlayImage,
                         AIL_ID AilDepthMap);
void SetupColorDisplay(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_INT SizeBit);
AIL_ID Alloc3dDisplayId(AIL_ID AilSystem);
//****************************************************************************
// Main.
//****************************************************************************
int MosMain(void)
   {
   AIL_ID AilApplication,     // Application identifier. 
          AilSystem,          // System identifier.      
          AilDisplay;         // Display identifier.     

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Run the depth correction example. 
   DepthCorrectionExample(AilSystem, AilDisplay);

   // Run the calibrated camera example. 
   CalibratedCameraExample(AilSystem, AilDisplay);

   // Free defaults.     
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }

//****************************************************************************
// Depth correction example.
//****************************************************************************

// Input sequence specifications. 
#define REFERENCE_PLANES_SEQUENCE_FILE  M_IMAGE_PATH AIL_TEXT("ReferencePlanes.avi")
#define OBJECT_SEQUENCE_FILE            M_IMAGE_PATH AIL_TEXT("ScannedObject.avi")

// Peak detection parameters. 
#define PEAK_WIDTH_NOMINAL         10
#define PEAK_WIDTH_DELTA            8
#define MIN_CONTRAST              140

// Calibration heights in mm. 
static const double CORRECTED_DEPTHS[] = {1.25, 2.50, 3.75, 5.00};

#define SCALE_FACTOR   10000.0 // (depth in world units) * SCALE_FACTOR gives gray levels

// Annotation position. 
#define CALIB_TEXT_POS_X         400   
#define CALIB_TEXT_POS_Y          15

void DepthCorrectionExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID      AilOverlayImage,   // Overlay image buffer identifier.            
               AilImage,          // Image buffer identifier (for processing).   
               AilDepthMap,       // Image buffer identifier (for results).      
               AilLaser,          // 3dmap laser profiling context identifier.   
               AilCalibScan,      // 3dmap result buffer identifier for laser    
                                  // line calibration.                           
               AilScan;           // 3dmap result buffer identifier.             
   AIL_INT     SizeX,             // Width of grabbed images.                    
               SizeY,             // Height of grabbed images.                   
               NbReferencePlanes, // Number of reference planes of known heights.
               NbObjectImages;    // Number of frames for scanned objects.       
   int         n;                 // Counter.                                    
   AIL_DOUBLE  FrameRate,         // Number of grabbed frames per second (in AVI)
               StartTime,         // Time at the beginning of each iteration.    
               EndTime,           // Time after processing for each iteration.   
               WaitTime;          // Time to wait for next frame.                

   // Inquire characteristics of the input sequences. 
   MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, M_SIZE_X,           &SizeX);
   MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, M_SIZE_Y,           &SizeY);
   MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, M_NUMBER_OF_IMAGES, &NbReferencePlanes);
   MbufDiskInquire(REFERENCE_PLANES_SEQUENCE_FILE, M_FRAME_RATE,       &FrameRate);
   MbufDiskInquire(OBJECT_SEQUENCE_FILE,           M_NUMBER_OF_IMAGES, &NbObjectImages);

   // Allocate buffer to hold images. 
   MbufAlloc2d(AilSystem, SizeX, SizeY,  8 + M_UNSIGNED, M_IMAGE + M_DISP + M_PROC, &AilImage);
   MbufClear(AilImage, 0.0);

   MosPrintf(AIL_TEXT("\nDEPTH ANALYSIS:\n"));
   MosPrintf(AIL_TEXT("---------------\n\n"));
   MosPrintf(AIL_TEXT("This program performs a surface inspection to detect "));
   MosPrintf(AIL_TEXT("depth defects \n"));
   MosPrintf(AIL_TEXT("on a wood surface using a laser (sheet-of-light) "));
   MosPrintf(AIL_TEXT("profiling system.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Select display. 
   MdispSelect(AilDisplay, AilImage);

   // Prepare for overlay annotations. 
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);
   MgraControl(M_DEFAULT, M_BACKGROUND_MODE, M_TRANSPARENT);
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_WHITE);

   // Allocate 3dmap objects. 
   M3dmapAlloc(AilSystem, M_LASER, M_DEPTH_CORRECTION, &AilLaser);
   M3dmapAllocResult(AilSystem, M_LASER_CALIBRATION_DATA, M_DEFAULT, &AilCalibScan);

   // Set laser line extraction options. 
   AIL_ID AilPeakLocator;
   M3dmapInquire(AilLaser, M_DEFAULT, M_LOCATE_PEAK_1D_CONTEXT_ID+M_TYPE_AIL_ID, &AilPeakLocator);
   MimControl(AilPeakLocator, M_PEAK_WIDTH_NOMINAL, PEAK_WIDTH_NOMINAL);
   MimControl(AilPeakLocator, M_PEAK_WIDTH_DELTA  , PEAK_WIDTH_DELTA  );
   MimControl(AilPeakLocator, M_MINIMUM_CONTRAST  , MIN_CONTRAST      );

   // Open the calibration sequence file for reading. 
   MbufImportSequence(REFERENCE_PLANES_SEQUENCE_FILE, M_DEFAULT, M_NULL, M_NULL, NULL,
                                                                   M_NULL, M_NULL, M_OPEN);

   // Read and process all images in the input sequence. 
   MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &StartTime);

   for (n = 0; n < NbReferencePlanes; n++)
      {
      AIL_TEXT_CHAR CalibString[32];

      // Read image from sequence. 
      MbufImportSequence(REFERENCE_PLANES_SEQUENCE_FILE, M_DEFAULT, M_LOAD, M_NULL,
                                                          &AilImage, M_DEFAULT, 1, M_READ);

      // Annotate the image with the calibration height. 
      MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);
      MosSprintf(CalibString, 32, AIL_TEXT("Reference plane %d: %.2f mm"),
                     (int)(n+1), CORRECTED_DEPTHS[n]);
      MgraText(M_DEFAULT, AilOverlayImage, CALIB_TEXT_POS_X, CALIB_TEXT_POS_Y, CalibString);

      // Set desired corrected depth of next reference plane. 
      M3dmapControl(AilLaser, M_DEFAULT, M_CORRECTED_DEPTH,
                                                         CORRECTED_DEPTHS[n]*SCALE_FACTOR);

      // Analyze the image to extract laser line. 
      M3dmapAddScan(AilLaser, AilCalibScan, AilImage, M_NULL, M_NULL, M_DEFAULT, M_DEFAULT);

      // Wait to have a proper frame rate, if necessary. 
      MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &EndTime);
      WaitTime = (1.0 / FrameRate) - (EndTime - StartTime);
      if (WaitTime > 0)
         { MappTimer(M_DEFAULT, M_TIMER_WAIT, &WaitTime); }
      MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &StartTime);
      }

   // Close the calibration sequence file. 
   MbufImportSequence(REFERENCE_PLANES_SEQUENCE_FILE, M_DEFAULT, M_NULL, M_NULL, NULL,
                                                                  M_NULL, M_NULL, M_CLOSE);

   // Calibrate the laser profiling context using reference planes of known heights. 
   M3dmapCalibrate(AilLaser, AilCalibScan, M_NULL, M_DEFAULT);

   MosPrintf(AIL_TEXT("The laser profiling system has been calibrated using 4 reference\n"));
   MosPrintf(AIL_TEXT("planes of known heights.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   MosPrintf(AIL_TEXT("The wood surface is being scanned.\n\n"));

   // Free the result buffer used for calibration because it will not be used anymore. 
   M3dmapFree(AilCalibScan);
   AilCalibScan = M_NULL;

   // Allocate the result buffer for the scanned depth corrected data. 
   M3dmapAllocResult(AilSystem, M_DEPTH_CORRECTED_DATA, M_DEFAULT, &AilScan);

   // Open the object sequence file for reading. 
   MbufDiskInquire(OBJECT_SEQUENCE_FILE, M_FRAME_RATE, &FrameRate);
   MbufImportSequence(OBJECT_SEQUENCE_FILE, M_DEFAULT, M_NULL, M_NULL, NULL, M_NULL,
                                                                           M_NULL, M_OPEN);

   // Read and process all images in the input sequence. 
   MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &StartTime);
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);

   for (n = 0; n < NbObjectImages; n++)
      {
      // Read image from sequence. 
      MbufImportSequence(OBJECT_SEQUENCE_FILE, M_DEFAULT, M_LOAD, M_NULL, &AilImage,
                                                                     M_DEFAULT, 1, M_READ);

      // Analyze the image to extract laser line and correct its depth. 
      M3dmapAddScan(AilLaser, AilScan, AilImage, M_NULL, M_NULL, M_DEFAULT, M_DEFAULT);

      // Wait to have a proper frame rate, if necessary. 
      MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &EndTime);
      WaitTime = (1.0/FrameRate) - (EndTime - StartTime);
      if (WaitTime > 0)
         { MappTimer(M_DEFAULT, M_TIMER_WAIT, &WaitTime); }
      MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &StartTime);
      }

   // Close the object sequence file. 
   MbufImportSequence(OBJECT_SEQUENCE_FILE, M_DEFAULT, M_NULL, M_NULL, NULL, M_NULL,
                                                                          M_NULL, M_CLOSE);

   // Allocate the image for a partially corrected depth map. 
   MbufAlloc2d(AilSystem, SizeX, NbObjectImages, 16 + M_UNSIGNED,
                                                       M_IMAGE+M_PROC+M_DISP, &AilDepthMap);

   // Get partially corrected depth map from accumulated information in the result buffer. 
   M3dmapCopyResult(AilScan, M_DEFAULT, AilDepthMap,M_PARTIALLY_CORRECTED_DEPTH_MAP, M_DEFAULT);

   // Disable display updates. 
   MdispControl(AilDisplay, M_UPDATE, M_DISABLE);

   // Show partially corrected depth map and find defects. 
   SetupColorDisplay(AilSystem, AilDisplay, MbufInquire(AilDepthMap, M_SIZE_BIT, M_NULL));
   
   // Display partially corrected depth map. 
   MdispSelect(AilDisplay, AilDepthMap);
   MdispControl(AilDisplay, M_UPDATE, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);

   MosPrintf(AIL_TEXT("The pseudo-color depth map of the surface is displayed.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   PerformBlobAnalysis(AilSystem, AilDisplay, AilOverlayImage, AilDepthMap);

   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Disassociates display LUT and clear overlay. 
   MdispSelect(AilDisplay, M_NULL);
   MdispLut(AilDisplay, M_DEFAULT);
   MdispControl(AilDisplay, M_OVERLAY_CLEAR, M_DEFAULT);

   // Free all allocations. 
   M3dmapFree(AilScan);
   M3dmapFree(AilLaser);
   MbufFree(AilDepthMap);
   MbufFree(AilImage);
   }

// Values used for binarization. 
#define EXPECTED_HEIGHT     3.4   // Inspected surface should be at this height (in mm)
#define DEFECT_THRESHOLD    0.2   // Max acceptable deviation from expected height (mm)
#define SATURATED_DEFECT    1.0   // Deviation at which defect will appear red (in mm) 

// Radius of the smallest particles to keep. 
#define MIN_BLOB_RADIUS              3L

// Pixel offset for drawing text. 
#define TEXT_H_OFFSET_1            -50
#define TEXT_V_OFFSET_1             -6
#define TEXT_H_OFFSET_2            -30
#define TEXT_V_OFFSET_2              6

// Find defects in corrected depth map, compute max deviation and draw contours.  
void PerformBlobAnalysis(AIL_ID AilSystem,
                         AIL_ID AilDisplay,
                         AIL_ID AilOverlayImage,
                         AIL_ID AilDepthMap)
   {
   AIL_ID      AilBinImage,         // Binary image buffer identifier.      
               AilBlobContext,      // Blob context identifier.             
               AilBlobResult;       // Blob result buffer identifier.       
   AIL_INT     SizeX,               // Width of depth map.                  
               SizeY,               // Height of depth map.                 
               TotalBlobs = 0,      // Total number of blobs.               
               n,                   // Counter.                             
              *MinPixels;           // Maximum height of defects.           
   AIL_DOUBLE  DefectThreshold,     // A gray level below it is a defect.   
              *CogX,                // X coordinate of center of gravity.   
              *CogY;                // Y coordinate of center of gravity.   

   // Get size of depth map. 
   MbufInquire(AilDepthMap, M_SIZE_X, &SizeX);
   MbufInquire(AilDepthMap, M_SIZE_Y, &SizeY);

   // Allocate a binary image buffer for fast processing. 
   MbufAlloc2d(AilSystem, SizeX, SizeY,  1 + M_UNSIGNED, M_IMAGE+M_PROC, &AilBinImage);

   // Binarize image. 
   DefectThreshold = (EXPECTED_HEIGHT-DEFECT_THRESHOLD) * SCALE_FACTOR;
   MimBinarize(AilDepthMap, AilBinImage, M_FIXED+M_LESS_OR_EQUAL, DefectThreshold, M_NULL);

   // Remove small particles. 
   MimOpen(AilBinImage, AilBinImage, MIN_BLOB_RADIUS, M_BINARY);

   // Allocate a blob context.  
   MblobAlloc(AilSystem, M_DEFAULT, M_DEFAULT, &AilBlobContext);
  
   // Enable the Center Of Gravity and Min Pixel features calculation.  
   MblobControl(AilBlobContext, M_CENTER_OF_GRAVITY+M_GRAYSCALE, M_ENABLE);
   MblobControl(AilBlobContext, M_MIN_PIXEL, M_ENABLE);
 
   // Allocate a blob result buffer. 
   MblobAllocResult(AilSystem, M_DEFAULT, M_DEFAULT, &AilBlobResult);
 
   // Calculate selected features for each blob.  
   MblobCalculate(AilBlobContext, AilBinImage, AilDepthMap, AilBlobResult);
 
   // Get the total number of selected blobs.  
   MblobGetResult(AilBlobResult, M_DEFAULT, M_NUMBER + M_TYPE_AIL_INT, &TotalBlobs);
   MosPrintf(AIL_TEXT("Number of defects: %lld\n"), (long long)TotalBlobs);

   // Read and print the blob characteristics.  
   CogX = new AIL_DOUBLE[TotalBlobs];
   CogY = new AIL_DOUBLE[TotalBlobs];
   MinPixels = new AIL_INT[TotalBlobs];
   if(CogX && CogY && MinPixels)
      { 
      // Get the results. 
      MblobGetResult(AilBlobResult, M_DEFAULT, M_CENTER_OF_GRAVITY_X + M_GRAYSCALE, CogX);
      MblobGetResult(AilBlobResult, M_DEFAULT, M_CENTER_OF_GRAVITY_Y + M_GRAYSCALE, CogY);
      MblobGetResult(AilBlobResult, M_DEFAULT, M_MIN_PIXEL + M_TYPE_AIL_INT, MinPixels);

      // Draw the defects. 
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_RED);
      MblobDraw(M_DEFAULT, AilBlobResult, AilOverlayImage,
                M_DRAW_BLOBS, M_INCLUDED_BLOBS, M_DEFAULT);
      MgraControl(M_DEFAULT, M_COLOR, M_COLOR_WHITE);

      // Print the depth of each blob. 
      for(n=0; n < TotalBlobs; n++)
         {
         AIL_DOUBLE    DepthOfDefect;
         AIL_TEXT_CHAR DepthString[16];

         // Write the depth of the defect in the overlay. 
         DepthOfDefect = EXPECTED_HEIGHT - (MinPixels[n]/SCALE_FACTOR);
         MosSprintf(DepthString, 16, AIL_TEXT("%.2f mm"), DepthOfDefect);

         MosPrintf(AIL_TEXT("Defect #%lld: depth =%5.2f mm\n\n"),
                   (long long)n, DepthOfDefect);
         MgraText(M_DEFAULT, AilOverlayImage, CogX[n]+TEXT_H_OFFSET_1,
                                        CogY[n]+TEXT_V_OFFSET_1, AIL_TEXT("Defect depth"));
         MgraText(M_DEFAULT, AilOverlayImage, CogX[n]+TEXT_H_OFFSET_2,
                                                     CogY[n]+TEXT_V_OFFSET_2, DepthString);
         }

      delete[] CogX;       CogX = NULL;
      delete[] CogY;       CogY = NULL;
      delete[] MinPixels;  MinPixels = NULL;
      }
   else
      MosPrintf(AIL_TEXT("Error: Not enough memory.\n\n"));

   // Free all allocations. 
   MblobFree(AilBlobResult);
   MblobFree(AilBlobContext);
   MbufFree(AilBinImage);
   }

// Color constants for display LUT. 
#define BLUE_HUE  171.0          // Expected depths will be blue.   
#define RED_HUE   0.0            // Worst defects will be red.      
#define FULL_SATURATION 255      // All colors are fully saturated. 
#define HALF_LUMINANCE  128      // All colors have half luminance. 

// Creates a color display LUT to show defects in red. 
void SetupColorDisplay(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_INT SizeBit)
   {
   AIL_ID  AilRampLut1Band,      // LUT containing hue values.            
           AilRampLut3Band,      // RGB LUT used by display.              
           AilColorImage;        // Image used for HSL to RGB conversion. 
   AIL_INT DefectGrayLevel,      // Gray level under which all is red.    
           ExpectedGrayLevel,    // Gray level over which all is blue.    
           NbGrayLevels;

   // Number of possible gray levels in corrected depth map. 
   NbGrayLevels = (AIL_INT)((AIL_INT)1 << SizeBit);

   // Allocate 1-band LUT that will contain hue values. 
   MbufAlloc1d(AilSystem, NbGrayLevels, 8 + M_UNSIGNED, M_LUT, &AilRampLut1Band);

   // Compute limit gray values. 
   DefectGrayLevel   = (AIL_INT)((EXPECTED_HEIGHT-SATURATED_DEFECT)*SCALE_FACTOR);
   ExpectedGrayLevel = (AIL_INT)(EXPECTED_HEIGHT*SCALE_FACTOR);

   // Create hue values for each possible gray level. 
   MgenLutRamp(AilRampLut1Band, 0, RED_HUE, DefectGrayLevel, RED_HUE);
   MgenLutRamp(AilRampLut1Band, DefectGrayLevel, RED_HUE, ExpectedGrayLevel, BLUE_HUE);
   MgenLutRamp(AilRampLut1Band, ExpectedGrayLevel, BLUE_HUE, NbGrayLevels-1, BLUE_HUE);

   // Create a HSL image buffer. 
   MbufAllocColor(AilSystem, 3, NbGrayLevels, 1, 8 + M_UNSIGNED, M_IMAGE, &AilColorImage);
   MbufClear(AilColorImage, M_RGB888(0, FULL_SATURATION, HALF_LUMINANCE));

   // Set its H band (hue) to the LUT contents and convert the image to RGB. 
   MbufCopyColor2d(AilRampLut1Band, AilColorImage, 0, 0, 0, 0, 0, 0, NbGrayLevels, 1);
   MimConvert(AilColorImage, AilColorImage, M_HSL_TO_RGB);

   // Create RGB LUT to give to display and copy image contents. 
   MbufAllocColor(AilSystem, 3, NbGrayLevels, 1, 8 + M_UNSIGNED, M_LUT, &AilRampLut3Band);
   MbufCopy(AilColorImage, AilRampLut3Band);

   // Associates LUT to display. 
   MdispLut(AilDisplay, AilRampLut3Band);

   // Free all allocations. 
   MbufFree(AilRampLut1Band);
   MbufFree(AilRampLut3Band);
   MbufFree(AilColorImage);
   }

//****************************************************************************
// Calibrated camera example.
//****************************************************************************

// Input sequence specifications. 
#define GRID_FILENAME                M_IMAGE_PATH AIL_TEXT("GridForLaser.mim")
#define LASERLINE_FILENAME           M_IMAGE_PATH AIL_TEXT("LaserLine.mim")
#define OBJECT2_SEQUENCE_FILE        M_IMAGE_PATH AIL_TEXT("Cookie.avi")

// Camera calibration grid parameters. 
#define GRID_NB_ROWS             13
#define GRID_NB_COLS             12
#define GRID_ROW_SPACING         5.0     // in mm              
#define GRID_COL_SPACING         5.0     // in mm              

// Laser device setup parameters. 
#define CONVEYOR_SPEED           -0.2    // in mm/frame       

// Fully corrected depth map generation parameters. 
#define DEPTH_MAP_SIZE_X         480     // in pixels         
#define DEPTH_MAP_SIZE_Y         480     // in pixels         
#define GAP_DEPTH                1.5     // in mm             

// Peak detection parameters. 
#define PEAK_WIDTH_NOMINAL_2        9
#define PEAK_WIDTH_DELTA_2          7
#define MIN_CONTRAST_2             75

// Everything below this is considered as noise. 
#define MIN_HEIGHT_THRESHOLD 1.0 // in mm

void CalibratedCameraExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID      AilOverlayImage,   // Overlay image buffer identifier.         
               AilImage,          // Image buffer identifier (for processing).
               AilCalibration,    // Calibration context.                     
               AilDepthMap,       // Image buffer identifier (for results).   
               AilLaser,          // 3dmap laser profiling context identifier.
               AilCalibScan,      // 3dmap result buffer identifier for laser 
                                  // line calibration.                        
               AilScan,           // 3map result buffer identifier.           
               AilContainerId,    // Point cloud container identifier.        
               FillGapsContext;   // Fill gaps context identifier.            
   AIL_INT     CalibrationStatus, // Used to ensure if McalGrid() worked.    
               SizeX,             // Width of grabbed images.                 
               SizeY,             // Height of grabbed images.                
               NumberOfImages,    // Number of frames for scanned objects.    
               n;                 // Counter.                                 
   AIL_DOUBLE  FrameRate,         // Number of grabbed frames per second (in AVI). 
               StartTime,         // Time at the beginning of each iteration. 
               EndTime,           // Time after processing for each iteration.
               WaitTime,          // Time to wait for next frame.             
               Volume;            // Volume of scanned object.                
              

   MosPrintf(AIL_TEXT("\n3D PROFILING AND VOLUME ANALYSIS:\n"));
   MosPrintf(AIL_TEXT("---------------------------------\n\n"));
   MosPrintf(AIL_TEXT("This program generates fully corrected 3D data of a\n"));
   MosPrintf(AIL_TEXT("scanned cookie and computes its volume.\n"));
   MosPrintf(AIL_TEXT("The laser (sheet-of-light) profiling system uses a\n"));
   MosPrintf(AIL_TEXT("3d-calibrated camera.\n\n"));

   // Load grid image for camera calibration. 
   MbufRestore(GRID_FILENAME, AilSystem, &AilImage);

   // Select display. 
   MdispSelect(AilDisplay, AilImage);

   MosPrintf(AIL_TEXT("Calibrating the camera...\n\n"));

   MbufInquire(AilImage, M_SIZE_X, &SizeX);
   MbufInquire(AilImage, M_SIZE_Y, &SizeY);

   // Allocate calibration context in 3D mode. 
   McalAlloc(AilSystem, M_TSAI_BASED, M_DEFAULT, &AilCalibration);

   // Calibrate the camera. 
   McalGrid(AilCalibration, AilImage, 0.0, 0.0, 0.0, GRID_NB_ROWS, GRID_NB_COLS,
            GRID_ROW_SPACING, GRID_COL_SPACING, M_DEFAULT, M_CHESSBOARD_GRID);

   McalInquire(AilCalibration, M_CALIBRATION_STATUS+M_TYPE_AIL_INT, &CalibrationStatus);
   if (CalibrationStatus != M_CALIBRATED)
      {
      McalFree(AilCalibration);
      MbufFree(AilImage);
      MosPrintf(AIL_TEXT("Camera calibration failed.\n"));
      MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
      MosGetch();
      return;
      }

   // Prepare for overlay annotations. 
   MdispControl(AilDisplay, M_OVERLAY, M_ENABLE);
   MdispInquire(AilDisplay, M_OVERLAY_ID, &AilOverlayImage);
   MgraControl(M_DEFAULT, M_COLOR, M_COLOR_GREEN);

   // Draw camera calibration points. 
   McalDraw(M_DEFAULT, AilCalibration, AilOverlayImage, M_DRAW_IMAGE_POINTS,
                                                                     M_DEFAULT, M_DEFAULT);

   MosPrintf(AIL_TEXT("The camera was calibrated using a chessboard grid.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Disable overlay. 
   MdispControl(AilDisplay, M_OVERLAY, M_DISABLE);

   // Load laser line image. 
   MbufLoad(LASERLINE_FILENAME, AilImage);

   // Allocate 3dmap objects. 
   M3dmapAlloc(AilSystem, M_LASER, M_CALIBRATED_CAMERA_LINEAR_MOTION, &AilLaser);
   M3dmapAllocResult(AilSystem, M_LASER_CALIBRATION_DATA, M_DEFAULT, &AilCalibScan);

   // Set laser line extraction options. 
   AIL_ID AilPeakLocator;
   M3dmapInquire(AilLaser, M_DEFAULT, M_LOCATE_PEAK_1D_CONTEXT_ID+M_TYPE_AIL_ID, &AilPeakLocator);
   MimControl(AilPeakLocator, M_PEAK_WIDTH_NOMINAL, PEAK_WIDTH_NOMINAL_2);
   MimControl(AilPeakLocator, M_PEAK_WIDTH_DELTA  , PEAK_WIDTH_DELTA_2  );
   MimControl(AilPeakLocator, M_MINIMUM_CONTRAST  , MIN_CONTRAST_2      );

   // Calibrate laser profiling context. 
   M3dmapAddScan(AilLaser, AilCalibScan, AilImage, M_NULL, M_NULL, M_DEFAULT, M_DEFAULT);
   M3dmapCalibrate(AilLaser, AilCalibScan, AilCalibration, M_DEFAULT);

   MosPrintf(AIL_TEXT("The laser profiling system has been calibrated using the image\n"));
   MosPrintf(AIL_TEXT("of one laser line.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Free the result buffer use for calibration as it will not be used anymore. 
   M3dmapFree(AilCalibScan);
   AilCalibScan = M_NULL;

   // Allocate the result buffer to hold the scanned 3D points. 
   M3dmapAllocResult(AilSystem, M_POINT_CLOUD_RESULT, M_DEFAULT, &AilScan);

   // Set speed of scanned object (speed in mm/frame is constant). 
   M3dmapControl(AilLaser, M_DEFAULT, M_SCAN_SPEED, CONVEYOR_SPEED);

   // Inquire characteristics of the input sequence. 
   MbufDiskInquire(OBJECT2_SEQUENCE_FILE, M_NUMBER_OF_IMAGES, &NumberOfImages);
   MbufDiskInquire(OBJECT2_SEQUENCE_FILE, M_FRAME_RATE, &FrameRate);

   // Open the object sequence file for reading. 
   MbufImportSequence(OBJECT2_SEQUENCE_FILE, M_DEFAULT, M_NULL, M_NULL, NULL, M_NULL,
                                                                           M_NULL, M_OPEN);

   MosPrintf(AIL_TEXT("The cookie is being scanned to generate 3D data.\n\n"));

   // Read and process all images in the input sequence. 
   MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &StartTime);

   for (n = 0; n < NumberOfImages; n++)
      {
      // Read image from sequence. 
      MbufImportSequence(OBJECT2_SEQUENCE_FILE, M_DEFAULT, M_LOAD, M_NULL, &AilImage,
                                                                     M_DEFAULT, 1, M_READ);

      // Analyze the image to extract laser line and correct its depth. 
      M3dmapAddScan(AilLaser, AilScan, AilImage, M_NULL, M_NULL, M_POINT_CLOUD_LABEL(1), M_DEFAULT);

      // Wait to have a proper frame rate, if necessary. 
      MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &EndTime);
      WaitTime = (1.0/FrameRate) - (EndTime - StartTime);
      if (WaitTime > 0)
         MappTimer(M_DEFAULT, M_TIMER_WAIT, &WaitTime);
      MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &StartTime);
      }

   // Close the object sequence file. 
   MbufImportSequence(OBJECT2_SEQUENCE_FILE, M_DEFAULT, M_NULL, M_NULL, NULL, M_NULL,
                                                                          M_NULL, M_CLOSE);

   // Convert to M_CONTAINER for 3D processing. 
   MbufAllocContainer(AilSystem, M_PROC | M_DISP, M_DEFAULT, &AilContainerId);
   M3dmapCopyResult(AilScan, M_ALL, AilContainerId, M_POINT_CLOUD_UNORGANIZED, M_DEFAULT);

   // The container's reflectance is 16bits, but only uses the bottom 8. Set the maximum value to display it properly. 
   MbufControlContainer(AilContainerId, M_COMPONENT_REFLECTANCE, M_MAX, 255);

   // Allocate image for the fully corrected depth map. 
   MbufAlloc2d(AilSystem, DEPTH_MAP_SIZE_X, DEPTH_MAP_SIZE_Y, 16 + M_UNSIGNED,
               M_IMAGE + M_PROC + M_DISP, &AilDepthMap);

   // Include all points during depth map generation. 
   M3dimCalibrateDepthMap(AilContainerId, AilDepthMap, M_NULL, M_NULL, M_DEFAULT, M_NEGATIVE, M_DEFAULT);

   // Remove noise in the container close to the Z = 0. 
   AIL_ID AilPlane = M3dgeoAlloc(AilSystem, M_GEOMETRY, M_DEFAULT, M_NULL);
   M3dgeoPlane(AilPlane, M_COEFFICIENTS, 0.0, 0.0, 1.0,MIN_HEIGHT_THRESHOLD, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT);

   // M_INVERSE remove what is above the plane. 
   M3dimCrop(AilContainerId, AilContainerId, AilPlane, M_NULL, M_SAME, M_INVERSE);
   M3dgeoFree(AilPlane);

   MosPrintf(AIL_TEXT("Fully corrected 3D data of the cookie is displayed.\n\n"));

   AIL_ID M3dDisplay = Alloc3dDisplayId(AilSystem);
   if(M3dDisplay)
      {
      MosPrintf(AIL_TEXT("Press <R> on the display window to stop/start the rotation.\n\n"));
      M3ddispSelect(M3dDisplay, AilContainerId, M_SELECT, M_DEFAULT);
      M3ddispSetView(M3dDisplay, M_AUTO, M_BOTTOM_TILTED, M_DEFAULT, M_DEFAULT, M_DEFAULT);
      M3ddispControl(M3dDisplay, M_AUTO_ROTATE, M_ENABLE);
      }

   // Get fully corrected depth map from accumulated information in the result buffer. 
   M3dimProject(AilContainerId, AilDepthMap, M_NULL, M_DEFAULT, M_MIN_Z, M_DEFAULT, M_DEFAULT);

   // Set fill gaps parameters. 
   M3dimAlloc(AilSystem, M_FILL_GAPS_CONTEXT, M_DEFAULT, &FillGapsContext);
   M3dimControl(FillGapsContext, M_FILL_MODE,                  M_X_THEN_Y);
   M3dimControl(FillGapsContext, M_FILL_SHARP_ELEVATION,       M_MIN);
   M3dimControl(FillGapsContext, M_FILL_SHARP_ELEVATION_DEPTH, GAP_DEPTH);
   M3dimControl(FillGapsContext, M_FILL_BORDER,                M_DISABLE);

   M3dimFillGaps(FillGapsContext, AilDepthMap, M_NULL, M_DEFAULT);

   // Compute the volume of the depth map. 
   M3dmetVolume(AilDepthMap, M_XY_PLANE, M_TOTAL,  M_DEFAULT, &Volume, M_NULL);

   MosPrintf(AIL_TEXT("Volume of the cookie is %4.1f cm^3.\n\n"), Volume / 1000.0);
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   // Free all allocations. 
   if (M3dDisplay)
      { M3ddispFree(M3dDisplay); }
   M3dimFree(FillGapsContext);
   MbufFree(AilContainerId);
   M3dmapFree(AilScan);
   M3dmapFree(AilLaser);
   McalFree(AilCalibration);
   MbufFree(AilDepthMap);
   MbufFree(AilImage);
   }
//*****************************************************************************
// Allocates a 3D display and returns its identifier.  
//*****************************************************************************
 AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
   {
    MappControl(M_DEFAULT, M_ERROR, M_PRINT_DISABLE);
    AIL_ID AilDisplay3D = M3ddispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_NULL);
    MappControl(M_DEFAULT, M_ERROR, M_PRINT_ENABLE);

     if(!AilDisplay3D)
         {
         MosPrintf(AIL_TEXT("\n")
                   AIL_TEXT("The current system does not support the 3D display.\n\n"));
         }
      return AilDisplay3D;
     }

```
