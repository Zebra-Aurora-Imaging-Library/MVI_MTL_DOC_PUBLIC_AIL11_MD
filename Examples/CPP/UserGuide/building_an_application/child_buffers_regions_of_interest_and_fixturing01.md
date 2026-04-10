---
title: "child_buffers_regions_of_interest_and_fixturing01"
description: "Learning the model offset"
ms.snippet: "userguide.building_an_application.child_buffers_regions_of_interest_and_fixturing01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# child_buffers_regions_of_interest_and_fixturing01

> Learning the model offset

**Language:** C++ | **Version:** 7.5+

```cpp
// [...]

// Associates the default uniform calibration context with your image; 
// this sets the absolute coordinate system to have the same origin 
// and orientation as the pixel coordinate system
// and sets the world units equal to 1 pixel.
McalUniform(TargetImage, 0.0, 0.0, 1.0, 1.0, 0.0, M_DEFAULT);

// Find all occurrences of the model in the target image.
MmodFind(ModCtx, TargetImage, ModRes);

// Allocate a stripe marker.
AIL_ID MeasStripe = MmeasAllocMarker(System, M_STRIPE, M_DEFAULT, M_NULL);
MmeasSetMarker(MeasStripe, M_ORIENTATION, M_HORIZONTAL, M_NULL);

// Use a graphics list to define the region to be fixtured and used by measurement.
const AIL_DOUBLE SearchRegionOriginX  = 30;
const AIL_DOUBLE SearchRegionOriginY  = 10;
const AIL_DOUBLE SearchRegionSizeX    = 200;
const AIL_DOUBLE SearchRegionSizeY    = 70;

MgraControl(M_DEFAULT, M_INPUT_UNITS, M_WORLD);
MgraRect(M_DEFAULT, GraphicLst, SearchRegionOriginX, SearchRegionOriginY,
                                SearchRegionSizeX, SearchRegionSizeY);
MbufSetRegion(TargetImage, GraphicLst, M_DEFAULT, M_NO_RASTERIZE + M_FILL_REGION, M_DEFAULT);

// [...]

AIL_INT NbFound = 0;
MmodGetResult(ModRes, M_GENERAL, M_NUMBER + M_TYPE_AIL_INT, &NbFound);

// For each found occurrence of the model.
for(AIL_INT i = 0; i < NbFound; i++)
   {
   // [...]

   // Moves the image's relative coordinate system to the reference position and 
   // orientation of the current model occurrence. 
   McalFixture(TargetImage, M_NULL, M_MOVE_RELATIVE, M_RESULT_MOD, ModRes, i,
                                    M_DEFAULT, M_DEFAULT, M_DEFAULT);

   // Measure the stripe in the fixtured search region.
   MmeasFindMarker(M_DEFAULT, TargetImage, MeasStripe, M_DEFAULT);

   // Analyze the results of the measured stripe.
   MmeasGetResult(MeasStripe, M_ANGLE + M_EDGE_FIRST  , &Edge1AngleDeg, M_NULL);
   MmeasGetResult(MeasStripe, M_ANGLE + M_EDGE_SECOND , &Edge2AngleDeg, M_NULL);

   // [...]
   }
```
