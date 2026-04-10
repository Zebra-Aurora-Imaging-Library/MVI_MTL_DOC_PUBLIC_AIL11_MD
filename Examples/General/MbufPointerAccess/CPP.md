---
title: "MbufPointerAccess"
description: "This program shows how to use the pointer of a AIL buffer in order to directly access its data."
ms.language: "C++"
---

# MbufPointerAccess

> This program shows how to use the pointer of a AIL buffer in order to directly access its data.

**Language:** C++

**Functions used:** `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufChildColor`, `MbufControl`, `MbufFree`, `MbufInquire`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MbufPointerAccess.cpp
//
// Description: This program shows how to use the pointer of a
//              buffer in order to directly access its data.
//
// Note:        This program does not support Distributed Aurora Imaging Library (DAIL).
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>  

// Target image size. 
#define IMAGE_SIZE_X 512
#define IMAGE_SIZE_Y 512

// Pointer access example for a monochrome buffer. 
void MonochromeBufferPointerAccessExample(AIL_ID AilSystem, AIL_ID AilDisplay);

// Pointer access example for a color packed buffer. 
void ColorPackedBufferPointerAccessExample(AIL_ID AilSystem, AIL_ID AilDisplay);

// Pointer access example for a color planar buffer. 
void ColorPlanarBufferPointerAccessExample(AIL_ID AilSystem, AIL_ID AilDisplay);

// Utility functions 
AIL_UINT Mandelbrot(AIL_INT PosX, AIL_INT PosY,
                    AIL_DOUBLE RefX, AIL_DOUBLE RefY, AIL_DOUBLE Dim);
AIL_UINT8 GetColorFromIndex(AIL_INT Band, AIL_INT Index, AIL_INT MaxIndex);

int MosMain(void)
   {
   AIL_ID  AilApplication, // Application identifier.
           AilSystem,      // System identifier.     
           AilDisplay;     // Display identifier.    

   MosPrintf(AIL_TEXT("\nBuffer pointer access example.\n"));
   MosPrintf(AIL_TEXT("----------------------------------\n\n"));

   // Allocate default objects. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   if (MsysInquire(AilSystem, M_LOCATION, M_NULL) == M_LOCAL)
      {

      // Pointer access example for a monochrome buffer 
      MonochromeBufferPointerAccessExample(AilSystem, AilDisplay);

      // Pointer access example for a color packed buffer. 
      ColorPackedBufferPointerAccessExample(AilSystem, AilDisplay);

      // Pointer access example for a color planar buffer. 
      ColorPlanarBufferPointerAccessExample(AilSystem, AilDisplay);
      }
   else
      {
      // Print that the example don't run remotely. 
      MosPrintf(AIL_TEXT("This example doesn't run with Distributed Aurora Imaging Library.\n"));
      // Wait for a key to terminate. 
      MosPrintf(AIL_TEXT("Press a key to terminate.\n\n"));
      MosGetch();
      }
   // Free allocated objects. 
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
   }

// Pointer access example for a monochrome buffer. 
// ------------------------------------------------

// Pixel value calculation parameters. 
#define X_REF1 -0.500
#define Y_REF1 +0.002
#define DIM1   +3.200

void MonochromeBufferPointerAccessExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID     AilImage;             // Image buffer identifier. 
   AIL_UINT8* AilImagePtr = M_NULL; // Image pointer.           
   AIL_INT    AilImagePitch = 0;    // Image pitch.             
   AIL_INT    x, y;                 // Buffer access variables. 
   AIL_UINT   Value;                // Value to write.          

   MosPrintf(AIL_TEXT("- The data of a 8bits monochrome buffer is modified\n"));
   MosPrintf(AIL_TEXT("  using its pointer to directly access the memory.\n\n"));

   // Allocate a monochrome buffer. 
   MbufAlloc2d(AilSystem, IMAGE_SIZE_X, IMAGE_SIZE_Y, 8 + M_UNSIGNED,
      M_IMAGE + M_PROC + M_DISP, &AilImage);

   // Lock buffer for direct access. 
   MbufControl(AilImage, M_LOCK, M_DEFAULT);

   // Retrieving buffer data pointer and pitch information. 
   MbufInquire(AilImage, M_HOST_ADDRESS, &AilImagePtr);
   MbufInquire(AilImage, M_PITCH, &AilImagePitch);

   // Direct Access to the buffer's data. 
   if (AilImagePtr != M_NULL)
      {
      // For each row. 
      for (y = 0; y < IMAGE_SIZE_Y; y++)
         {
         // For each column. 
         for (x = 0; x < IMAGE_SIZE_X; x++)
            {
            // Calculate the pixel value. 
            Value = Mandelbrot(x, y, X_REF1, Y_REF1, DIM1);

            // Write the pixel using its pointer. 
            AilImagePtr[x] = AIL_UINT8(Value);
            }

         // Move pointer to the next line taking into account the image's pitch. 
         AilImagePtr += AilImagePitch;
         }

      //  Signals that the buffer data has been updated. 
      MbufControl(AilImage, M_MODIFIED, M_DEFAULT);

      // Unlock buffer. 
      MbufControl(AilImage, M_UNLOCK, M_DEFAULT);

      // Select to display. 
      MdispSelect(AilDisplay, AilImage);
      }
   else
      {
      MosPrintf(AIL_TEXT("The source buffer has no accessible memory\n"));
      MosPrintf(AIL_TEXT("address on this specific system. Try changing\n")); 
      MosPrintf(AIL_TEXT("the system in the Aurora Imaging Configurator utility.\n\n"));
      }

   // Print a message and wait for a key. 
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Free allocation. 
   MbufFree(AilImage);
   };

// Pointer access example for a color packed buffer. 
// --------------------------------------------------

// Pixel value calculation parameters. 
#define X_REF2 -1.1355
#define Y_REF2 -0.2510
#define DIM2   +0.1500

// Utility to pack B,G,R values into 32 bits integer. 
AIL_UINT32 PackToBGR32(AIL_UINT8 b, AIL_UINT8 g, AIL_UINT8 r) 
   {
   return ((AIL_UINT32)b | (AIL_UINT32)(g << 8) | (AIL_UINT32)(r << 16));
   };

void ColorPackedBufferPointerAccessExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID      AilImage;            // Image buffer identifier. 
   AIL_UINT32* AilImagePtr=M_NULL;  // Image pointer.           
   AIL_INT     AilImagePitch = 0;   // Image Pitch.             
   AIL_INT     x, y, NbBand = 3;    // Buffer access variables. 
   AIL_UINT    Value;               // Equation Value.          
   AIL_UINT32  Value_BGR32;         // Color Value to write.    

   MosPrintf(AIL_TEXT("- The data of a 32bits color packed buffer is modified\n"));
   MosPrintf(AIL_TEXT("  using its pointer to directly access the memory.\n\n"));

   // Allocate a color buffer. 
   MbufAllocColor(AilSystem, NbBand, IMAGE_SIZE_X, IMAGE_SIZE_Y, 8 + M_UNSIGNED,
      M_IMAGE + M_PROC + M_DISP + M_BGR32 + M_PACKED, &AilImage);

   // Lock buffer for direct access. 
   MbufControl(AilImage, M_LOCK, M_DEFAULT);

   // Retrieving buffer pointer and pitch information. 
   MbufInquire(AilImage, M_HOST_ADDRESS, &AilImagePtr);
   MbufInquire(AilImage, M_PITCH, &AilImagePitch);

   // Custom modification of the buffer's data. 
   if (AilImagePtr != M_NULL)
      {
      // For each row. 
      for (y = 0; y < IMAGE_SIZE_Y; y++)
         {
         // For each column. 
         for (x = 0; x < IMAGE_SIZE_X; x++)
            {
            // Calculate the pixel value. 
            Value = Mandelbrot(x, y, X_REF2, Y_REF2, DIM2);

            Value_BGR32 = PackToBGR32(
               GetColorFromIndex(M_BLUE, Value, 255),
               GetColorFromIndex(M_GREEN, Value, 255),
               GetColorFromIndex(M_RED, Value, 255)
               );

            // Write the color pixel using its pointer. 
            AilImagePtr[x] = Value_BGR32;
            }

         // Move pointer to the next line taking into account the image's pitch. 
         AilImagePtr += AilImagePitch;
         }

      //  Signals the buffer data has been updated. 
      MbufControl(AilImage, M_MODIFIED, M_DEFAULT);

      // Unlock buffer. 
      MbufControl(AilImage, M_UNLOCK, M_DEFAULT);

      // Select to display. 
      MdispSelect(AilDisplay, AilImage);

      }
   else
      {
      MosPrintf(AIL_TEXT("The source buffer has no accessible memory\n"));
      MosPrintf(AIL_TEXT("address on this specific system. Try changing\n")); 
      MosPrintf(AIL_TEXT("the system in the Aurora Imaging Configurator utility.\n\n"));
      }

   // Print a message and wait for a key. 
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Free allocation. 
   MbufFree(AilImage);
   };

// Pointer access example for a color planar buffer. 
// ------------------------------------------------

// Pixel value calculation parameters. 
#define X_REF3 -0.7453
#define Y_REF3 +0.1127
#define DIM3   +0.0060

void ColorPlanarBufferPointerAccessExample(AIL_ID AilSystem, AIL_ID AilDisplay)
   {
   AIL_ID      AilImage,                 // Image buffer identifier. 
               AilImageBand;             // Image band identifier.   
   AIL_UINT8*  AilImageBandPtr= M_NULL;  // Image band pointer.      
   AIL_INT     AilImagePitch = 0;        // Image Pitch.             
   AIL_INT     x, y, i, NbBand = 3;      // Buffer access variables. 
   AIL_UINT    Value;                    // Value write.             

   AIL_UINT    ColorBand[3] =    { M_RED, M_GREEN, M_BLUE    };

   MosPrintf(AIL_TEXT("- The data of a 24bits color planar buffer is modified using\n"));
   MosPrintf(AIL_TEXT("  each color band pointer's to directly access the memory.\n\n"));

   // Allocate a color buffer. 
   MbufAllocColor(AilSystem, NbBand, IMAGE_SIZE_X, IMAGE_SIZE_Y, 8 + M_UNSIGNED,
      M_IMAGE + M_PROC + M_DISP + M_PLANAR, &AilImage);

   // Retrieving buffer pitch information. 
   MbufInquire(AilImage, M_PITCH, &AilImagePitch);

   // Lock buffer for direct access. 
   MbufControl(AilImage, M_LOCK, M_DEFAULT);

   // Verifying the buffer has a host address. 
   MbufChildColor(AilImage, M_RED, &AilImageBand);
   MbufInquire(AilImageBand, M_HOST_ADDRESS, &AilImageBandPtr);
   MbufFree(AilImageBand);

   if (AilImageBandPtr != M_NULL)
      {
      // For each color band. 
      for (i = 0; i < NbBand; i++)
         {
         // Retrieving buffer color band pointer. 
         MbufChildColor(AilImage, ColorBand[i], &AilImageBand);
         MbufInquire(AilImageBand, M_HOST_ADDRESS, &AilImageBandPtr);

         // For each row. 
         for (y = 0; y < IMAGE_SIZE_Y; y++)
            {
            // For each column. 
            for (x = 0; x < IMAGE_SIZE_X; x++)
               {
               // Calculate the pixel value. 
               Value = Mandelbrot(x, y, X_REF3, Y_REF3, DIM3);

               // Write the color pixel using its pointer. 
               AilImageBandPtr[x] = GetColorFromIndex(ColorBand[i], Value, 255);
               }

            // Move pointer to the next line taking into account the image's pitch. 
            AilImageBandPtr += AilImagePitch;
            }

         // Release the child band identifier. 
         MbufFree(AilImageBand);
         }

      //  Signals the buffer data has been updated. 
      MbufControl(AilImage, M_MODIFIED, M_DEFAULT);

      // Unlock buffer. 
      MbufControl(AilImage, M_UNLOCK, M_DEFAULT);

      // Select to display. 
      MdispSelect(AilDisplay, AilImage);
      }
   else
      {
      MosPrintf(AIL_TEXT("The source buffer has no accessible memory\n"));
      MosPrintf(AIL_TEXT("address on this specific system. Try changing\n")); 
      MosPrintf(AIL_TEXT("the system in the Aurora Imaging Configurator utility.\n\n"));
      }

   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Free allocation. 
   MbufFree(AilImage);
   };

// Mandelbrot fractal utility functions. 
AIL_INT    MinVal(AIL_INT a, AIL_INT b)    { return (a > b) ? b : a;    }
AIL_DOUBLE Remap(AIL_DOUBLE pos, AIL_DOUBLE size, AIL_DOUBLE min, AIL_DOUBLE max)
   {
   return ( (((max-min) / size) * pos) + min );
   }

AIL_UINT Mandelbrot(AIL_INT PosX, AIL_INT PosY,
                    AIL_DOUBLE RefX, AIL_DOUBLE RefY, AIL_DOUBLE Dim)
   {
   const AIL_UINT maxIter = 256;
   AIL_DOUBLE xMin = RefX - (0.5 * Dim);
   AIL_DOUBLE xMax = RefX + (0.5 * Dim);
   AIL_DOUBLE yMin = RefY - (0.5 * Dim);
   AIL_DOUBLE yMax = RefY + (0.5 * Dim);
   AIL_DOUBLE x0 = Remap((AIL_DOUBLE)PosX, (AIL_DOUBLE)IMAGE_SIZE_X, xMin, xMax);
   AIL_DOUBLE y0 = Remap((AIL_DOUBLE)PosY, (AIL_DOUBLE)IMAGE_SIZE_Y, yMin, yMax);
   AIL_DOUBLE x = 0.0;
   AIL_DOUBLE y = 0.0;
   AIL_UINT Iter = 0;

   while (((x*x + y*y) < 4) && (Iter < maxIter))
      {
      AIL_DOUBLE Temp = x*x - y*y + x0;
      y = 2 * x*y + y0;
      x = Temp;
      Iter++;
      }

   return MinVal(255, Iter);
   }

// Calculate color from index. 
AIL_UINT8 GetColorFromIndex(AIL_INT Band, AIL_INT Index, AIL_INT MaxIndex)
   {
   AIL_UINT8* Segments = M_NULL;
   AIL_UINT8  SegmentsR[] =    {   0,   0,   0, 255, 255, 128 };
   AIL_UINT8  SegmentsG[] =    {   0,   0, 255, 255,   0,   0 };
   AIL_UINT8  SegmentsB[] =    { 128, 255, 255,   0,   0,   0 };

   switch (Band)
      {
      case M_RED:
         Segments = SegmentsR;
         break;
      case M_GREEN:
         Segments = SegmentsG;
         break;
      case M_BLUE:
         Segments = SegmentsB;
         break;
      }

   AIL_DOUBLE RemapedIndex = Index * MaxIndex / 256.0;
   AIL_UINT8  SegmentIndex = AIL_UINT8(RemapedIndex * 5.0 / 256.0);
   AIL_DOUBLE Slope = (Segments[SegmentIndex + 1] - Segments[SegmentIndex]) / (256.0 / 5.0);
   AIL_DOUBLE Offset = (Segments[SegmentIndex] - (Slope * SegmentIndex * 256.0 / 5.0));
   AIL_UINT8  Value = AIL_UINT8(Slope * RemapedIndex + Offset + 0.5);

   return Value;
   }

```
