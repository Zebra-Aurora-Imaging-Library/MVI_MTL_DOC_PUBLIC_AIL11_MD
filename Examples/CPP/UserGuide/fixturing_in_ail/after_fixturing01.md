---
title: "after_fixturing01"
description: "Transforming your image after fixturing"
ms.snippet: "userguide.fixturing_in_ail.after_fixturing01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# after_fixturing01

> Transforming your image after fixturing

**Language:** C++ | **Version:** 7.5+

```cpp
/* Set up phase. */

/* If the image is not yet corrected, do it now. */
McalTransformImage(GoldenTemplateImageId, GoldenTemplateImageId, M_NULL, 
                   M_DEFAULT, M_DEFAULT, M_DEFAULT);

/* Define a model in the golden template image. */
MmodDefine(ModContextId, M_IMAGE, GoldenTemplateImageId,
           ModelOffsetX, ModelOffsetY, ModelWidth, ModelHeight);

/* Learn the fixturing offset from the model. */
McalFixture(M_NULL, FixturingOffsetId, M_LEARN_OFFSET, M_MODEL_MOD, 
            ModContextId, 0, 
            M_DEFAULT, M_DEFAULT, M_DEFAULT);

/* ... */

/* Critical loop. */
while (!Stop)
   {
   /* Acquire image. */
   /* ... */

   /* Find the model in GrabbedImageId */
   MmodFind(ModContextId, GrabbedImageId, ModResultId);

   /* Make sure the model is found. */
   /* ... */

   /* Move the relative coordinate system (with learned offset)
      according to the found location of the model. */
   McalFixture(GrabbedImageId, FixturingOffsetId, M_MOVE_RELATIVE, M_RESULT_MOD,
               ModResultId, 0,
               M_DEFAULT, M_DEFAULT, M_DEFAULT);

   /* Transform the grabbed image such that its relative coordinate system
      is at the same position and angle as the golden template image.  */
   McalAssociate(GoldenTemplateImageId, TransformedGrabbedImageId, M_DEFAULT);
   McalTransformImage(GrabbedImageId, TransformedGrabbedImageId, M_NULL,
                      M_DEFAULT, M_DEFAULT, 
                      M_USE_DESTINATION_CALIBRATION);

   /* Process transformed image
      (probably comparing it with GoldenTemplateImageId).
      ... */
   }
```
