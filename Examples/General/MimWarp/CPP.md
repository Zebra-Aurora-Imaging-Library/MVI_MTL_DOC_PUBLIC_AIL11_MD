---
title: "MimWarp"
description: "This program performs three types of warp transformations. First the image is stretched according to four specified reference points. Then it is warped on a sinusoid, and finally, the program loops while warping the image on a sphere."
ms.language: "C++"
---

# MimWarp

> This program performs three types of warp transformations. First the image is stretched according to four specified reference points. Then it is warped on a sinusoid, and finally, the program loops while warping the image on a sphere.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MappTimer`, `MbufAlloc2d`, `MbufChild2d`, `MbufChildMove`, `MbufClear`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufPut1d`, `MbufPut2d`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgenWarpParameter`, `MimWarp`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MimWarp.cpp 
//
// Description: This program performs three types of warp transformations. 
//              First the image is stretched according to four specified 
//              reference points. Then it is warped on a sinusoid, and 
//              finally, the program loops while warping the image on a 
//              sphere.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>
#include <math.h>
#include <malloc.h>

// Target image specifications.
#define IMAGE_FILE               M_IMAGE_PATH AIL_TEXT("BaboonMono.mim")
#define INTERPOLATION_MODE       M_NEAREST_NEIGHBOR
#if(INTERPOLATION_MODE == M_NEAREST_NEIGHBOR)
   #define FIXED_POINT_PRECISION    M_FIXED_POINT+0L
   #define FLOAT_TO_FIXED_POINT(x)  (1L * (x))
#else
   #define FIXED_POINT_PRECISION    M_FIXED_POINT+6L
   #define FLOAT_TO_FIXED_POINT(x)  (64L * (x))
#endif
#define ROTATION_STEP            1

// Utility functions.
void GenerateSphericLUT(AIL_INT ImageWidth, AIL_INT ImageHeight, 
                        short *AilLutXPtr, short *AilLutYPtr);


// Main function.
// --------------
int MosMain(void)
{
   AIL_ID   AilApplication,  // Application identifier.        
            AilSystem,       // System identifier.             
            AilDisplay,      // Display identifier.            
            AilDisplayImage, // Image buffer identifier.       
            AilSourceImage,  // Image buffer identifier.       
            Ail4CornerArray, // Coefficients buffer identifier.
            AilLutX,         // Lut buffer identifier.         
            AilLutY,         // Lut buffer identifier.         
            ChildWindow;     // Child Image identifier.        
   float    FourCornerMatrix[12] = {
            0.0,             // X coordinate of quadrilateral's 1st corner 
            0.0,             // Y coordinate of quadrilateral's 1st corner 
            456.0,           // X coordinate of quadrilateral's 2nd corner 
            62.0,            // Y coordinate of quadrilateral's 2nd corner 
            333.0,           // X coordinate of quadrilateral's 3rd corner 
            333.0,           // Y coordinate of quadrilateral's 3rd corner 
            100.0,           // X coordinate of quadrilateral's 4th corner 
            500.0,           // Y coordinate of quadrilateral's 4th corner 
            0.0,             // X coordinate of rectangle's top-left corner
            0.0,             // Y coordinate of rectangle's top-left corner
            511.0,           // X coordinate of rectangle's bottom-right corner
            511.0 };         // Y coordinate of rectangle's bottom-right corner
   AIL_INT  Precision     = FIXED_POINT_PRECISION;
   AIL_INT  Interpolation = INTERPOLATION_MODE;
   short    *AilLutXPtr,  *AilLutYPtr;
   AIL_INT  OffsetX       = 0;
   AIL_INT  ImageWidth=0, ImageHeight=0, ImageType=M_NULL, i=0, j=0;
   AIL_DOUBLE   FramesPerSecond = 0, Time = 0, NbLoop = 0;

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Restore the source image.
   MbufRestore(IMAGE_FILE, AilSystem, &AilSourceImage);

   // Allocate a display buffers and show the source image.
   MbufAlloc2d(AilSystem, MbufInquire(AilSourceImage, M_SIZE_X, &ImageWidth),
                          MbufInquire(AilSourceImage, M_SIZE_Y, &ImageHeight),
                          MbufInquire(AilSourceImage, M_TYPE,   &ImageType),
                          M_IMAGE+M_PROC+M_DISP, &AilDisplayImage);
   MbufCopy(AilSourceImage, AilDisplayImage);
   MdispSelect(AilDisplay, AilDisplayImage);

   // Print a message.
   MosPrintf(AIL_TEXT("\nWARPING:\n"));
   MosPrintf(AIL_TEXT("--------\n\n"));
   MosPrintf(AIL_TEXT("This image will be warped using different methods.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   
   // Four-corner LUT warping
   //-------------------------
   
   // Allocate 2 LUT buffers.
   MbufAlloc2d(AilSystem, ImageWidth, ImageHeight, 16L+M_SIGNED, M_LUT, &AilLutX);
   MbufAlloc2d(AilSystem, ImageWidth, ImageHeight, 16L+M_SIGNED, M_LUT, &AilLutY);

   // Allocate the coefficient buffer.
   MbufAlloc2d(AilSystem, 12L, 1L, 32L+M_FLOAT, M_ARRAY, &Ail4CornerArray);

   // Put warp values into the coefficient buffer.
   MbufPut1d(Ail4CornerArray, 0L, 12L, FourCornerMatrix);

   // Generate LUT buffers.
   MgenWarpParameter(Ail4CornerArray, AilLutX,   AilLutY,  
                     M_WARP_4_CORNER+Precision, M_DEFAULT, 
                     0.0, 0.0);

   // Clear the destination.
   MbufClear(AilDisplayImage,0);

   // Warp the image.
   MimWarp(AilSourceImage, AilDisplayImage, AilLutX, AilLutY, M_WARP_LUT+Precision,
           Interpolation);

   // Print a message.
   MosPrintf(AIL_TEXT("The image was warped from an arbitrary"));
   MosPrintf(AIL_TEXT(" quadrilateral to a square.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   
   // Sinusoidal LUT warping
   //------------------------

   // Allocate user-defined LUTs.
   AilLutXPtr = (short*)malloc(sizeof(short)*ImageHeight*ImageWidth);
   AilLutYPtr = (short*)malloc(sizeof(short)*ImageHeight*ImageWidth);
  
   // Fill the LUT with a sinusoidal waveforms with a 6-bit precision.
   for (j=0;j<ImageHeight;j++)
      {
      for (i=0;i<ImageWidth;i++)
         {
         AilLutYPtr[i+ (j*ImageWidth)] = 
            (short)FLOAT_TO_FIXED_POINT(((j) + (int)((20*sin(0.03*i)))));
         AilLutXPtr[i+ (j*ImageWidth)] = 
            (short)FLOAT_TO_FIXED_POINT(((i) + (int)((20*sin(0.03*j))))); 
         }
      }
   
   // Put the values into the LUT buffers.
   MbufPut2d(AilLutX, 0L, 0L, ImageWidth, ImageHeight, AilLutXPtr);
   MbufPut2d(AilLutY, 0L, 0L, ImageWidth, ImageHeight, AilLutYPtr);

   // Clear the destination.
   MbufClear(AilDisplayImage,0);

   // Warp the image.
   MimWarp(AilSourceImage,AilDisplayImage,AilLutX,AilLutY,M_WARP_LUT +Precision,
           Interpolation);
   
   // Wait for a key.
   MosPrintf(AIL_TEXT("The image was warped on two sinusoidal waveforms.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Continuous spherical LUT warping
   // --------------------------------

   // Allocate temporary buffer.
   MbufFree(AilSourceImage);
   MbufAlloc2d(AilSystem, ImageWidth*2, ImageHeight, ImageType, 
                                        M_IMAGE+M_PROC, &AilSourceImage);
                            
   // Reload the image.
   MbufLoad(IMAGE_FILE, AilSourceImage);
   
   // Fill the LUTs with a sphere pattern with a 6-bit precision.
   GenerateSphericLUT(ImageWidth, ImageHeight, AilLutXPtr,AilLutYPtr);
   MbufPut2d(AilLutX, 0L, 0L, ImageWidth, ImageHeight, AilLutXPtr);
   MbufPut2d(AilLutY, 0L, 0L, ImageWidth, ImageHeight, AilLutYPtr);

   // Duplicate the buffer to allow wrap around in the warping.
   MbufCopy(AilSourceImage, AilDisplayImage);
   MbufChild2d(AilSourceImage, ImageWidth, 0, ImageWidth, ImageHeight, &ChildWindow);
   MbufCopy(AilDisplayImage, ChildWindow);
   MbufFree(ChildWindow);
  
   // Clear the destination.
   MbufClear(AilDisplayImage,0);

   // Print a message and start the timer.
   MosPrintf(AIL_TEXT("The image is continuously warped on a sphere.\n"));
   MosPrintf(AIL_TEXT("Press any key to stop.\n\n"));
   MappTimer(M_DEFAULT, M_TIMER_RESET+M_SYNCHRONOUS, M_NULL);

   // Create a child in the buffer containing the two images.
   MbufChild2d(AilSourceImage, OffsetX, 0, ImageWidth, ImageHeight, &ChildWindow);

   // Warp the image continuously.
   while ((!MosKbhit()) || (OffsetX != (ImageWidth/4)))
      {
      // Move the child to the new position
      MbufChildMove(ChildWindow, OffsetX, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT);
      
      // Warp the child in the window.
      MimWarp(ChildWindow, AilDisplayImage, AilLutX, AilLutY, 
                           M_WARP_LUT+Precision, Interpolation);

      // Update the offset (shift the window to the right).
      OffsetX += ROTATION_STEP;
      
      // Reset the offset if the child is outside the buffer.
      if (OffsetX>ImageWidth-1)
         OffsetX = 0;

      NbLoop++;

      // Calculate and print the number of frames per second processed.
      MappTimer(M_DEFAULT, M_TIMER_READ+M_SYNCHRONOUS, &Time);
      FramesPerSecond = NbLoop/Time;
      MosPrintf(AIL_TEXT("Processing speed: %.0f Images/Sec.\r"), FramesPerSecond);
      }
   MosGetch();
   MosPrintf(AIL_TEXT("\nPress any key to end.\n"));
   MosGetch();

   // Free objects.
   free(AilLutXPtr);
   free(AilLutYPtr);

   MbufFree(ChildWindow);
   MbufFree(AilLutX);
   MbufFree(AilLutY);
   MbufFree(Ail4CornerArray);
   MbufFree(AilSourceImage);
   MbufFree(AilDisplayImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}


// Generate two custom LUTs used to map the image on a sphere.
// -----------------------------------------------------------
void GenerateSphericLUT(AIL_INT ImageWidth, AIL_INT ImageHeight, 
                        short *AilLutXPtr, short *AilLutYPtr)
{
   AIL_INT      i, j, k;
   AIL_DOUBLE       utmp, vtmp, tmp;
   short        v;
     
   // Set the radius of the sphere.
   AIL_DOUBLE       Radius = 200.0;         

   // Generate the X and Y buffers.
   for (j=0; j < ImageHeight; j++ )
      {
      k = j*ImageWidth;

      // Check that still in the sphere (in the Y axis).
      if (fabs( vtmp = ((AIL_DOUBLE)(j - (ImageHeight/2)) / Radius) ) < 1.0)
      {
      // We scan from top to bottom, so reverse the value obtained above
      // and obtain the angle.
      vtmp = acos( -vtmp );
      if(vtmp == 0.0)
         vtmp=0.0000001;

   
      // Compute the position to fetch in the source.
      v = (short)((vtmp/3.1415926) * (AIL_DOUBLE)(ImageHeight - 1) + 0.5);

      // Compute the Y coordinate of the sphere.
      tmp = Radius*sin(vtmp);

      for (i=0; i < ImageWidth; i++ )
         {
         // Check that still in the sphere.
         if ( fabs(utmp = ((AIL_DOUBLE)(i - (ImageWidth/2)) / tmp)) < 1.0 )
            {
            utmp = acos( -utmp);
         
            // Compute the position to fetch (fold the image in four).
            AilLutXPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(((utmp/3.1415926) * 
                     (AIL_DOUBLE)((ImageWidth/2) - 1) + 0.5));
            AilLutYPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(v);
            }  
         else
            {
            // Default position (fetch outside the buffer to 
            // activate the clear overscan).
            AilLutXPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(ImageWidth);
            AilLutYPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(ImageHeight);
            }  
         }   
      }
        else
      {
      for (i=0; i < ImageWidth ;i++ )
         {
         // Default position (fetch outside the buffer for clear overscan).
         AilLutXPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(ImageWidth);
         AilLutYPtr[i + k] = (short)FLOAT_TO_FIXED_POINT(ImageHeight);
         } 
      }
   }  
}  



```
