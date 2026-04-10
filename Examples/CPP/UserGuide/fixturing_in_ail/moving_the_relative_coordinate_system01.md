---
title: "moving_the_relative_coordinate_system01"
description: "Moving using a model"
ms.snippet: "userguide.fixturing_in_ail.moving_the_relative_coordinate_system01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# moving_the_relative_coordinate_system01

> Moving using a model

**Language:** C++ | **Version:** 7.5+

```cpp
/* Move the relative coordinate system of ImageId, with no offset,
   at the location of occurrence 0 of the pattern matching model,
   (location is contained in the pattern matching result PatResultId). */
McalFixture(ImageId, M_NULL, M_MOVE_RELATIVE, M_RESULT_PAT, PatResultId,
            0, /* Occurrence index = 0. */
            M_DEFAULT, M_DEFAULT, M_DEFAULT);
```
