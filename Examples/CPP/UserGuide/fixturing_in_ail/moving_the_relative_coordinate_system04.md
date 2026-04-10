---
title: "moving_the_relative_coordinate_system04"
description: "Copying the new relative coordinate system to other images"
ms.snippet: "userguide.fixturing_in_ail.moving_the_relative_coordinate_system04"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# moving_the_relative_coordinate_system04

> Copying the new relative coordinate system to other images

**Language:** C++ | **Version:** 7.5+

```cpp
/* Copy the relative coordinate system location from the SrcImageId to
   the DestImageId. */
McalFixture(DestImageId, M_NULL, M_MOVE_RELATIVE, M_POINT_AND_ANGLE,
            SrcImageId, 
            0.0, 0.0, 0.0, M_DEFAULT);
```
