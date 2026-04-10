---
title: "rotating_a_camera_attached_to_robotic_hand"
description: "Rotating a camera attached to robotic arm"
ms.snippet: "userguide.calibration.rotating_a_camera_attached_to_robotic_hand"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# rotating_a_camera_attached_to_robotic_hand

> Rotating a camera attached to robotic arm

**Language:** C++ | **Version:** 8.0+

```cpp
/*
*   A robotic arm is holding the camera. 
*   The arm rolls 30 degrees around its own axis (the Z axis)
*/

McalSetCoordinateSystem(CalibrationId, M_TOOL_COORDINATE_SYSTEM, M_TOOL_COORDINATE_SYSTEM, 
                  M_ROTATION_Z + M_COMPOSE_WITH_CURRENT, M_NULL, 30.0,
                  M_DEFAULT, M_DEFAULT, M_DEFAULT);
```
