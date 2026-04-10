---
title: "moving_the_relative_coordinate_system05"
description: "Moving using a metrology reference frame"
ms.snippet: "userguide.fixturing_in_ail.moving_the_relative_coordinate_system05"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# moving_the_relative_coordinate_system05

> Moving using a metrology reference frame

**Language:** C++ | **Version:** 7.5+

```cpp
/* Move the relative coordinate system of ImageId, with no offset,
   at the location of the local frame 1 of the metrology context,
   (location is contained in the metrology result MetResultId). */
McalFixture(ImageId, M_NULL, M_MOVE_RELATIVE, M_RESULT_MET, MetResultId,
            1, /* Local frame index = 1. */
            M_DEFAULT, M_DEFAULT, M_DEFAULT);
```
