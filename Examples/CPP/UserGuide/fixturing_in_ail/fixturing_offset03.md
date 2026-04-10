---
title: "fixturing_offset03"
description: "Establishing a position and angle for the fixturing offset object"
ms.snippet: "userguide.fixturing_in_ail.fixturing_offset03"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# fixturing_offset03

> Establishing a position and angle for the fixturing offset object

**Language:** C++ | **Version:** 7.5+

```cpp
   /* First way to learn a fixturing offset. */

   /* Define models. */
   /* ... */

   /* Learn the offset from a Pattern Matching model or from a Model Finder model. */
   McalFixture(M_NULL, FixturingOffset1Id, M_LEARN_OFFSET, M_MODEL_PAT, 
               PatContextId, ImageModelDefinitionId,
			   0, /* Pattern Matching model index. */ M_DEFAULT, M_DEFAULT);
   McalFixture(M_NULL, FixturingOffset2Id, M_LEARN_OFFSET, M_MODEL_MOD, 
               ModContextId, 0, /* Model finder context and model index. */ 
               M_DEFAULT, M_DEFAULT, M_DEFAULT);

   /* Second way to learn a fixturing offset. */

   /* Find the models in a training image. */
   /* ... */

   /* Learn the offset from a Pattern Matching result or from a Model Finder result. */
   McalFixture(M_NULL, FixturingOffset3Id, M_LEARN_OFFSET, M_RESULT_PAT, 
               PatResultId, 0, /* Pattern matching result and occurrence index. */
               M_DEFAULT, M_DEFAULT, M_DEFAULT);
   McalFixture(M_NULL, FixturingOffset4Id, M_LEARN_OFFSET, M_RESULT_MOD, 
               ModResultId, 0, /* Model finder result and occurrence index. */
               M_DEFAULT, M_DEFAULT, M_DEFAULT);

   /* Third way to learn a fixturing offset. */

   /* Learn the offset from a preestabilished position and angle
      expressed in the relative coordinate system of ImageId. */
   McalFixture(M_NULL, FixturingOffset5Id, M_LEARN_OFFSET, M_POINT_AND_ANGLE, 
               ImageId, X, Y, Angle, M_DEFAULT);
```
