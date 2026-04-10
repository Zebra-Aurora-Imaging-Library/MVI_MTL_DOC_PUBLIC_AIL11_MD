---
title: "MdispWindowLeveling"
description: "This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling."
ms.language: "C++"
---

# MdispWindowLeveling

> This AIL program shows how to display a 10-bit monochrome Medical diagnostics image and apply a LUT to perform interactive Window Leveling.

**Language:** C++

**Functions used:** `MbufControl`, `MappAlloc`, `MappFree`, `MbufAlloc1d`, `MbufCopy`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispLut`, `MdispSelect`, `MdispSelectWindow`, `MgenLutRamp`, `MgraLine`, `MgraText`, `MimAllocResult`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Preprocessing, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdispWindowLeveling.cpp
//
// Description: This program shows how to display a 10-bit monochrome Medical image
//              and applies a LUT to do interactive Window Leveling.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>
#include <stdlib.h>

// Image file to load. 
#define IMAGE_NAME      AIL_TEXT("ArmsMono10bit.mim")
#define IMAGE_FILE      M_IMAGE_PATH IMAGE_NAME

// Draw the LUT shape (if disabled reduces CPU usage). 
#define DRAW_LUT_SHAPE  M_YES

// Utility function and macros. 
void DrawLutShape(AIL_ID AilDisplay, 
                  AIL_ID AilOriginalImage, 
                  AIL_ID AilImage, 
                  AIL_INT Start,
                  AIL_INT End, 
                  AIL_INT InflexionIntensity, 
                  AIL_INT ImageMaxValue, 
                  AIL_INT DisplayMaxValue);
#define MosMin(a, b) (((a) < (b)) ? (a) : (b))
#define MosMax(a, b) (((a) > (b)) ? (a) : (b))

int MosMain(void)
{
   AIL_ID AilApplication,       // Application identifier. 
          AilSystem,            // System identifier.      
          AilDisplay,           // Display identifier.     
          AilImage,             // Image buffer identifier.
          AilOriginalImage = 0, // Image buffer identifier.
          AilLut;               // Lut buffer identifier.  
   AIL_INT ImageSizeX, ImageSizeY, ImageMaxValue;
   AIL_INT DisplaySizeBit, DisplayMaxValue;
   AIL_INT Start, End, Step, InflectionLevel;
   AIL_INT Ch;
   

   // Allocate the application, System and Display. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Restore target image from disk. 
   MbufRestore(IMAGE_FILE, AilSystem, &AilImage);

   // Dynamically calculates the maximum value of the image using processing. 
   AIL_ID AilExtremeResult = M_NULL;
   MimAllocResult((AIL_ID)MbufInquire(AilImage, M_OWNER_SYSTEM, M_NULL),
                  1L, M_EXTREME_LIST, &AilExtremeResult);
   MimFindExtreme(AilImage, AilExtremeResult, M_MAX_VALUE);
   MimGetResult(AilExtremeResult, M_VALUE, &ImageMaxValue);
   MimFree(AilExtremeResult);

   // Set the maximum value of the image to indicate to Aurora Imaging Library how to initialize 
   // the default display LUT.
   MbufControl(AilImage, M_MAX, (AIL_DOUBLE)ImageMaxValue);

   // Display the image (to specify a user-defined window, use MdispSelectWindow()). 
   MdispSelect(AilDisplay, AilImage);

   // Determine the maximum displayable value of the current display. 
   MdispInquire(AilDisplay, M_SIZE_BIT, &DisplaySizeBit);
   DisplayMaxValue = (1<<DisplaySizeBit)-1;

   // Print key information. 
   MosPrintf(AIL_TEXT("\nINTERACTIVE WINDOW LEVELING:\n"));
   MosPrintf(AIL_TEXT("----------------------------\n\n"));

   MosPrintf(AIL_TEXT("Image name : %s\n"),IMAGE_NAME);

   MosPrintf(AIL_TEXT("Image size : %d x %d\n"), 
             (int)MbufInquire(AilImage, M_SIZE_X, &ImageSizeX), 
             (int)MbufInquire(AilImage, M_SIZE_Y, &ImageSizeY));
   MosPrintf(AIL_TEXT("Image max  : %4d\n"),   (int)ImageMaxValue);
   MosPrintf(AIL_TEXT("Display max: %4d\n\n"), (int)DisplayMaxValue);
   
   // Allocate a LUT buffer according to the image maximum value and 
   // display pixel depth.
   MbufAlloc1d(AilSystem, ImageMaxValue+1, 
                               ((DisplaySizeBit>8) ? 16 : 8)+M_UNSIGNED, M_LUT, &AilLut);

   // Generate a LUT with a full range ramp and set its maximum value. 
   MgenLutRamp(AilLut, 0, 0, ImageMaxValue, (AIL_DOUBLE)DisplayMaxValue);
   MbufControl(AilLut, M_MAX, (AIL_DOUBLE)DisplayMaxValue);

   // Set the display LUT. 
   MdispLut(AilDisplay, AilLut);

   // Interactive Window Leveling using keyboard. 
   MosPrintf(AIL_TEXT("Keys assignment:\n\n"));
   MosPrintf(AIL_TEXT("Arrow keys :    Left=move Left, Right=move Right, ")
             AIL_TEXT("Down=Narrower, Up=Wider.\n"));
   MosPrintf(AIL_TEXT("Intensity keys: L=Lower,  U=Upper,  R=Reset.\n"));
   MosPrintf(AIL_TEXT("Press enter to end.\n\n"));

   // Modify LUT shape according to the arrow keys and update it. 
   Ch = 0;
   Start = 0;
   End = ImageMaxValue;
   InflectionLevel = DisplayMaxValue;
   Step = (ImageMaxValue+1)/128;
   Step = MosMax(Step,4);
   while (Ch != '\r')
      {
      switch (Ch)
         {
         // Left arrow: Move region left. 
         case 0x4B:
            { Start-=Step; End-=Step; break; }

         // Right arrow: Move region right. 
         case 0x4D:
            { Start+=Step; End+=Step; break; }

         // Down arrow: Narrow region. 
         case 0x50:
            { Start+=Step; End-=Step; break; }

         // Up arrow: Widen region. 
         case 0x48:
            { Start-=Step, End+=Step; break; }

         // L key: Lower inflexion point. 
         case 'L':
         case 'l':
            { InflectionLevel--; break; }

         // U key: Upper inflexion point. 
         case 'U':
         case 'u':
            { InflectionLevel++; break; }

         // R key: Reset the LUT to full image range. 
         case 'R':
         case 'r':
            { Start=0; End=ImageMaxValue; InflectionLevel=DisplayMaxValue; break; }

         }

      // Saturate. 
      End   = MosMin(End,ImageMaxValue);
      Start = MosMin(Start,End);
      End   = MosMax(End,Start);
      Start = MosMax(Start,0);
      End   = MosMax(End,0);
      InflectionLevel = MosMax(InflectionLevel, 0);
      InflectionLevel = MosMin(InflectionLevel, DisplayMaxValue);
      MosPrintf(AIL_TEXT("Inflection points: Low=(%d,0), High=(%d,%d).   \r"),
                                                    (int)Start, (int)End, InflectionLevel);

      // Generate a LUT with 3 slopes and saturated at both ends. 
      MgenLutRamp(AilLut, 0, 0, Start, 0);
      MgenLutRamp(AilLut, Start, 0, End, (AIL_DOUBLE)InflectionLevel);
      MgenLutRamp(AilLut, End, (AIL_DOUBLE)InflectionLevel, ImageMaxValue, 
                                               (AIL_DOUBLE)DisplayMaxValue);

      // Update the display LUT. 
      MdispLut(AilDisplay, AilLut);

      // Draw the current LUT's shape in the image.
      // Note: This simple annotation method requires
      //       significant update and CPU time.
      if (DRAW_LUT_SHAPE)
         {
         if (!AilOriginalImage)
            MbufRestore(IMAGE_FILE, AilSystem, &AilOriginalImage);
         DrawLutShape(AilDisplay, AilOriginalImage, AilImage, Start, End,
                                  InflectionLevel, ImageMaxValue, DisplayMaxValue);
         }

      // If its an arrow key, get the second code. 
      if ((Ch = MosGetch()) == 0xE0)
           Ch = MosGetch();
      }
   MosPrintf(AIL_TEXT("\n\n"));

   // Free all allocations. 
   MbufFree(AilLut);
   MbufFree(AilImage);
   if (AilOriginalImage)
      MbufFree(AilOriginalImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}

// Function to draw the current LUT's shape in the image.
//
// Note: This simple annotation method requires significant update
//        and CPU time since it repaints the entire image every time.
void DrawLutShape(AIL_ID AilDisplay, 
                  AIL_ID AilOriginalImage, 
                  AIL_ID AilImage, 
                  AIL_INT Start,
                  AIL_INT End, 
                  AIL_INT InflexionIntensity, 
                  AIL_INT ImageMaxValue, 
                  AIL_INT DisplayMaxValue)
   {
    AIL_INT        ImageSizeX, ImageSizeY;
    AIL_DOUBLE     Xstart, Xend, Xstep, Ymin, Yinf, Ymax, Ystep;
    AIL_TEXT_CHAR  String[8];

    // Inquire image dimensions. 
    MbufInquire(AilImage, M_SIZE_X, &ImageSizeX);
    MbufInquire(AilImage, M_SIZE_Y, &ImageSizeY);

    // Calculate the drawing parameters. 
    Xstep  = (AIL_DOUBLE)ImageSizeX/(AIL_DOUBLE)ImageMaxValue;
    Xstart = Start*Xstep;
    Xend   = End*Xstep;
    Ystep  = ((AIL_DOUBLE)ImageSizeY/4.0)/(AIL_DOUBLE)DisplayMaxValue;
    Ymin   = ((AIL_DOUBLE)ImageSizeY-2);
    Yinf   = Ymin-(InflexionIntensity*Ystep);
    Ymax   = Ymin-(DisplayMaxValue*Ystep);

    // To increase speed, disable display update until all annotations are done. 
    MdispControl(AilDisplay, M_UPDATE, M_DISABLE);

    // Restore the original image. 
    MbufCopy(AilOriginalImage, AilImage);

    // Draw axis max and min values. 
    MgraControl(M_DEFAULT, M_COLOR, (AIL_DOUBLE)ImageMaxValue);
    MgraText(M_DEFAULT, AilImage, 4, (AIL_INT)Ymin-22, AIL_TEXT("0"));
    MosSprintf(String, 8, AIL_TEXT("%d"), (int)DisplayMaxValue);
    MgraText(M_DEFAULT, AilImage, 4, (AIL_INT)Ymax-16, String);
    MosSprintf(String, 8, AIL_TEXT("%d"), (int)ImageMaxValue);
    MgraText(M_DEFAULT, AilImage, ImageSizeX-38 , (AIL_INT)Ymin-22, String);

    // Draw LUT Shape (X axis is display values and Y is image values). 
    MgraLine(M_DEFAULT, AilImage, 0, (AIL_INT)Ymin, (AIL_INT)Xstart, (AIL_INT)Ymin);
    MgraLine(M_DEFAULT, AilImage, (AIL_INT)Xstart, (AIL_INT)Ymin, 
                                                      (AIL_INT)Xend, (AIL_INT)Yinf);
    MgraLine(M_DEFAULT, AilImage, (AIL_INT)Xend, (AIL_INT)Yinf, 
                                                       ImageSizeX-1, (AIL_INT)Ymax);

    // Enable display update to show the result. 
    MdispControl(AilDisplay, M_UPDATE, M_ENABLE);
   }


```
