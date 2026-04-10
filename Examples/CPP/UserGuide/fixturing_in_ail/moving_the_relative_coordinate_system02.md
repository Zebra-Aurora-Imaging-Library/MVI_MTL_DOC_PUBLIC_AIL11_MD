---
title: "moving_the_relative_coordinate_system02"
description: "Moving using a position and angle expressed in a specified relative coordinate system"
ms.snippet: "userguide.fixturing_in_ail.moving_the_relative_coordinate_system02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# moving_the_relative_coordinate_system02

> Moving using a position and angle expressed in a specified relative coordinate system

**Language:** C++ | **Version:** 7.5+

```cpp
/* Move the relative coordinate system of ImageId, with no offset,
   at the position (X2, Y2) and angle Theta2.
   This point and this angle are expressed in the relative coordinate
   system of ImageId (before the move). 
   After the move, the relative coordinate system will be placed at position (X3, Y3)
   and angle Theta3. */
McalFixture(ImageId, M_NULL, M_MOVE_RELATIVE, M_POINT_AND_ANGLE, 
            M_DEFAULT, /* CS for (X2, Y2) and Theta2: M_DEFAULT means use ImageId. */
            X2, Y2, Theta2, M_DEFAULT);
```
