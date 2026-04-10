---
title: "fixturing_offset01"
description: "Moving the reference location prior to searching"
ms.snippet: "userguide.fixturing_in_ail.fixturing_offset01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# fixturing_offset01

> Moving the reference location prior to searching

**Language:** C++ | **Version:** 7.5+

```cpp
/* Set up phase. */

/* Set the model reference location
   (model 0 in Model Finder context ModContextId). */
MmodControl(ModContextId, 0, M_REFERENCE_X, ReferenceX);
MmodControl(ModContextId, 0, M_REFERENCE_Y, ReferenceY);
MmodControl(ModContextId, 0, M_REFERENCE_ANGLE, ReferenceAngle);

/* Critical loop. */
while (!Stop)
   {
   /* Acquire image. */
   /* ... */

   /* Find the model in GrabbedImageId. */
   MmodFind(ModContextId, GrabbedImageId, ModResultId);

   /* Loop on all model occurrences. */
   MmodGetResult(ModResultId, M_GENERAL, M_NUMBER+M_TYPE_AIL_INT, &Number);

   for (ResultIndex = 0; ResultIndex < Number; ResultIndex++)
      {
      /* Move the relative coordinate system to the reference location
         of the found model. */
      McalFixture(GrabbedImageId, M_NULL, M_MOVE_RELATIVE, M_RESULT_MOD, 
                  ModResultId, ResultIndex,
                  M_DEFAULT, M_DEFAULT, M_DEFAULT);

      /* Do some analysis. */
      /* ... */
      }
   }
```
