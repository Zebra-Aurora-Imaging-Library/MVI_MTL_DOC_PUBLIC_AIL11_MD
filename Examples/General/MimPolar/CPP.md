---
title: "MimPolar"
description: "This program uses the polar-to-rectangular transformation to unroll a string."
ms.language: "C++"
---

# MimPolar

> This program uses the polar-to-rectangular transformation to unroll a string.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufChild2d`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimPolarTransform`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Reading, Preprocessing, Modules, Buffer, Display, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImPolar.cpp 
//
// Description: This program uses the polar-to-rectangular transformation
//              to unroll a string.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>
#include <math.h>

// Target image specifications.
#define IMAGE_FILE              M_IMAGE_PATH AIL_TEXT("Polar.mim")

// Points used to compute the parameters of the circle.
#define POINT1_X   147
#define POINT1_Y   359
#define POINT2_X   246
#define POINT2_Y   404
#define POINT3_X   354
#define POINT3_Y   368

// Polar stripe features.
#define DELTA_RADIUS  25
#define START_ANGLE   210
#define END_ANGLE     330

// Utility function prototype.
void GenerateCircle ( AIL_DOUBLE X1, AIL_DOUBLE Y1, AIL_DOUBLE X2, 
                      AIL_DOUBLE Y2, AIL_DOUBLE X3, AIL_DOUBLE Y3, 
                      AIL_DOUBLE *CenterX, AIL_DOUBLE *CenterY, 
                      AIL_DOUBLE *Radius);

// Main function.
int MosMain(void)
{
   AIL_ID   AilApplication,     // Application identifier.       
            AilSystem,          // System identifier.            
            AilDisplay,         // Display identifier.           
            AilImage,           // Image buffer identifier.      
            AilPolarImage;      // Destination buffer identifier.     
   AIL_DOUBLE   SizeRadius,SizeAngle;
   AIL_DOUBLE   CenterX, CenterY, Radius;
   AIL_INT  OffsetX, OffsetY, SizeX, SizeY;

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, 
                             &AilDisplay, M_NULL, M_NULL);

   // Load the source image and display it.
   MbufRestore(IMAGE_FILE, AilSystem, &AilImage);
   MdispSelect(AilDisplay, AilImage);

   // Calculate the parameters of the circle.
   GenerateCircle(POINT1_X, POINT1_Y, POINT2_X, POINT2_Y,
                  POINT3_X, POINT3_Y, &CenterX, &CenterY, &Radius);
   
   // Get the size of the destination buffer.
   MimPolarTransform(AilImage, M_NULL, CenterX, CenterY, 
                     Radius+DELTA_RADIUS, Radius-DELTA_RADIUS, 
                     START_ANGLE, END_ANGLE, M_RECTANGULAR_TO_POLAR, 
                     M_NEAREST_NEIGHBOR+M_OVERSCAN_ENABLE, 
                     &SizeAngle, &SizeRadius);
   
   // Allocate the destination buffer.
   OffsetX  = (AIL_INT)((MbufInquire(AilImage, M_SIZE_X, M_NULL)/2) - (SizeAngle/2));
   OffsetY  = 20;
   SizeX    = (AIL_INT)ceil(SizeAngle);
   SizeY    = (AIL_INT)ceil(SizeRadius);
   MbufChild2d(AilImage, OffsetX, OffsetY, SizeX, SizeY, &AilPolarImage);
   
   // Print a message.
   MosPrintf(AIL_TEXT("\nPOLAR TRANSFORMATION:\n"));
   MosPrintf(AIL_TEXT("---------------------\n\n"));
   MosPrintf(AIL_TEXT("A string will be unrolled using a polar-to-rectangular ")
                                                   AIL_TEXT("transformation.\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();
   
   // Unroll the string.
   MimPolarTransform(AilImage, AilPolarImage, CenterX, CenterY, 
                     Radius+DELTA_RADIUS, Radius-DELTA_RADIUS, 
                     START_ANGLE, END_ANGLE, M_RECTANGULAR_TO_POLAR, 
                     M_NEAREST_NEIGHBOR+M_OVERSCAN_ENABLE, 
                     &SizeAngle, &SizeRadius);
   
   // Print a message on the Host screen.
   MosPrintf(AIL_TEXT("Press any key to end.\n"));
   MosGetch();
   
   // Free buffers.
   MbufFree(AilPolarImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, AilImage);

   return 0;
}


// GenerateCircle() function returns the center and the radius of a circle 
// defined by 3 non-collinear points.
void GenerateCircle ( AIL_DOUBLE X1, 
                      AIL_DOUBLE Y1, 
                      AIL_DOUBLE X2, 
                      AIL_DOUBLE Y2, 
                      AIL_DOUBLE X3, 
                      AIL_DOUBLE Y3, 
                      AIL_DOUBLE *CenterX, 
                      AIL_DOUBLE *CenterY, 
                      AIL_DOUBLE *Radius)
{
   AIL_DOUBLE Slope1 = 0.0;
   AIL_DOUBLE Slope2 = 0.0;
   AIL_DOUBLE MidPoint1X = 0.0;
   AIL_DOUBLE MidPoint1Y = 0.0;
   AIL_DOUBLE MidPoint2X = 0.0;
   AIL_DOUBLE MidPoint2Y = 0.0;
   AIL_DOUBLE Offset1 = 0.0;
   AIL_DOUBLE Offset2 = 0.0;

   // Compute the middle points of the two segments.
   MidPoint1X = (X1+X2)/2;
   MidPoint1Y = (Y1+Y2)/2;
   MidPoint2X = (X2+X3)/2;
   MidPoint2Y = (Y2+Y3)/2;

   // Compute the slope between points 1 and 2, and between 
   // points 2 and 3.
   if (((Y2-Y1) != 0.0) && ((Y3-Y2) != 0.0))
      {
      Slope1 = -(X2-X1)/(Y2-Y1);
      Slope2 = -(X3-X2)/(Y3-Y2);

      Offset1 = MidPoint1Y - Slope1*MidPoint1X;
      Offset2 = MidPoint2Y - Slope2*MidPoint2X;

      *CenterX = (Offset2-Offset1)/(Slope1-Slope2);
      *CenterY = Slope1*(*CenterX) + Offset1;
      }
   else if (((Y2-Y1) == 0.0) && ((Y3-Y2) != 0.0))
      {
      Slope2 = -(X3-X2)/(Y3-Y2);
      Offset2 = MidPoint2Y - Slope2*MidPoint2X;

      *CenterX = MidPoint1X;
      *CenterY = Slope2*(*CenterX) + Offset2;
      }
   else if (((Y2-Y1) != 0.0) && ((Y3-Y2) == 0.0))
      {
      Slope1 = -(X2-X1)/(Y2-Y1);
      Offset1 = MidPoint1Y - Slope1*MidPoint1X;

      *CenterX = MidPoint2X;
      *CenterY = Slope1*(*CenterX) + Offset1;
      }

   // Compute the radius.
   *Radius = sqrt(pow(*CenterX-X1,2) + pow(*CenterY-Y1,2));
}

```
