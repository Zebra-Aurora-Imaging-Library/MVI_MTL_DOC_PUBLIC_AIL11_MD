---
title: "changing_tool_coordinate_system"
description: "Changing pose of tool coordinate system according to robot base."
ms.snippet: "userguide.calibration.changing_tool_coordinate_system"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# changing_tool_coordinate_system

> Changing pose of tool coordinate system according to robot base.

**Language:** C++ | **Version:** 8.0+

```cpp
/*
*   Set the position and orientation of the tool coordinate system according 
*   to the robot encoders.
*/

/* First, obtain the tool's position and orientation using the robot's software.*/

/* Second, call McalSetCoordinateSystem() twice.*/

McalSetCoordinateSystem(CalibrationId, M_TOOL_COORDINATE_SYSTEM, 
   M_ROBOT_BASE_COORDINATE_SYSTEM, M_TRANSLATION + M_ASSIGN, M_NULL,
   ToolPosX, ToolPosY, ToolPosZ, M_DEFAULT);

McalSetCoordinateSystem(CalibrationId, M_TOOL_COORDINATE_SYSTEM, M_TOOL_COORDINATE_SYSTEM,
   M_ROTATION_XYZ + M_ASSIGN, M_NULL, AngleX, AngleY, AngleZ, M_DEFAULT);
```
