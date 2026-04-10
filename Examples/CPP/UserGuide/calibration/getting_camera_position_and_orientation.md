---
title: "getting_camera_position_and_orientation"
description: "Inquiring about the camera's position and orientation"
ms.snippet: "userguide.calibration.getting_camera_position_and_orientation"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# getting_camera_position_and_orientation

> Inquiring about the camera's position and orientation

**Language:** C++ | **Version:** 8.0+

```cpp
/*
   Get the position and orientation of the camera 
   with respect to the absolute coordinate system.
*/
AIL_DOUBLE CameraPosX, CameraPosY, CameraPosZ;
AIL_DOUBLE AngleY, AngleX, AngleZ;

McalGetCoordinateSystem(CalibrationID, 
                        M_CAMERA_COORDINATE_SYSTEM, 
                        M_ABSOLUTE_COORDINATE_SYSTEM,
                        M_TRANSLATION, 
                        M_NULL, &CameraPosX, &CameraPosY, &CameraPosZ, M_NULL);

McalGetCoordinateSystem(CalibrationID, 
                        M_CAMERA_COORDINATE_SYSTEM, 
                        M_ABSOLUTE_COORDINATE_SYSTEM,
                        M_ROTATION_YXZ, 
                        M_NULL, &AngleY, &AngleX, &AngleZ, M_NULL);
```
