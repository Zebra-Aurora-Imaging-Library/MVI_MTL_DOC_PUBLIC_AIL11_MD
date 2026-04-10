---
title: "moving_the_relative_coordinate_system03"
description: "Moving using a position and angle that are with respect to the absolute coordinate system"
ms.snippet: "userguide.fixturing_in_ail.moving_the_relative_coordinate_system03"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# moving_the_relative_coordinate_system03

> Moving using a position and angle that are with respect to the absolute coordinate system

**Language:** C++ | **Version:** 7.5+

```cpp
/* Move the relative coordinate system of ImageId, with no offset,
   at the position (X3, Y3) and angle Theta3.
   This point and this angle are expressed in the absolute coordinate system. */
McalRelativeOrigin(ImageId, X3, Y3, 0.0, Theta3, M_DEFAULT);
```
