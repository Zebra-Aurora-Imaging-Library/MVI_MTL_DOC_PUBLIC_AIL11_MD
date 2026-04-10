---
title: "Setting_the_rough_location_of_your_images"
description: "Comparing different methods of copying rough locations and transformations"
ms.snippet: "userguide.Registration.Setting_the_rough_location_of_your_images"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# Setting_the_rough_location_of_your_images

> Comparing different methods of copying rough locations and transformations

**Language:** C++ | **Version:** 8.0+

```cpp
ElementArray[0] = ReferenceImageId;
ElementArray[1] = GrabImageId;

// WITHOUT M_COPY_REG_RESULT.

// Grab the reference image.
MdigGrab(DigId, ReferenceImageId);

// Set the origin reference
MregSetLocation(ContextId, 0, M_REGISTRATION_GLOBAL, M_POSITION_XY,
                 0, 0, M_NULL, M_NULL, M_DEFAULT);

// Continuously grab new images and align them to the reference image.
while (1)
   {
   // Grab a new image.
   MdigGrab(DigId, GrabImageId);
   // Calculate the transformation that aligns the grabbed image
   // with the reference image.
   MregCalculate(ContextId, ElementArray, RegResultId, 2, M_DEFAULT);

   // The rough location of the next iteration will be the position
   // that has just been calculated in this iteration.
   MregGetResult(RegResultId, 1, M_POSITION_X+M_REFERENCE, &PositionX);
   MregGetResult(RegResultId, 1, M_POSITION_Y+M_REFERENCE, &PositionY);
   MregSetLocation(ContextId, 1, 0, M_POSITION_XY,
                    PositionX, PositionY, M_NULL, M_NULL, M_DEFAULT);
   }

// WITH M_COPY_REG_RESULT.

// Grab a reference image.
MdigGrab(DigId, ReferenceImageId);

// Set the origin reference
MregSetLocation(ContextId, 0, M_REGISTRATION_GLOBAL, M_POSITION_XY,
                0, 0, M_NULL, M_NULL, M_DEFAULT);

// Continuously grab new images and align them to the reference image.
while (1)
   {
   // Grab a new image.
   MdigGrab(DigId, GrabImageId);

   // Calculate the transformation that aligns the grabbed image
   // with the reference image.
   MregCalculate(ContextId, ElementArray, RegResultId, 2, M_DEFAULT);

   // The rough location of the next iteration will be the position
   // that has just been calculated in this iteration.
   MregSetLocation(ContextId, 1, 0, M_COPY_REG_RESULT,
                 (AIL_DOUBLE)RegResultId, M_DEFAULT, M_DEFAULT, M_NULL, M_DEFAULT);
   }
```
