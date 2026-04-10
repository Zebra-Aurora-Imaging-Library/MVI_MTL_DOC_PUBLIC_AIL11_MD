---
title: "moving_the_relative_coordinate_system_to_account_for_height"
description: "Calibrating in the relative coordinate system"
ms.snippet: "userguide.3d_analysis.moving_the_relative_coordinate_system_to_account_for_height"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# moving_the_relative_coordinate_system_to_account_for_height

> Calibrating in the relative coordinate system

**Language:** C++ | **Version:** 8.0+

```cpp
// Use the relative coordinate system Z=0 plane to calibrate the camera.
McalControl(ContextCalId, M_CALIBRATION_PLANE, M_RELATIVE_COORDINATE_SYSTEM);

// Specify the height of grid as the offset of the XY (Z=0) plane of the relative 
// coordinate system from the absolute coordinate system.
// Since the relative coordinate system is used as the camera calibration plane, 
// the XY plane of the relative coordinate system will be placed on top of the grid 
// upon calibration, and the absolute coordinate system will be placed so that 
// it will have the right offset from the relative coordinate system. Since the Z-axis 
// is pointing downward, the Z-coordinate on the top of the grid is negative.
McalSetCoordinateSystem(ContextCalId, M_RELATIVE_COORDINATE_SYSTEM,
   M_ABSOLUTE_COORDINATE_SYSTEM, M_TRANSLATION, M_NULL, 0.0, 0.0, -GRID_THICKNESS,
   M_DEFAULT);

// Calibrate the camera setup.
McalGrid(ContextCalId, ImageBufId, M_NULL, M_NULL, M_NULL, GRID_NB_ROWS, GRID_NB_COLUMNS,
   GRID_ROW_SPACING, GRID_COLUMN_SPACING, M_DEFAULT, M_DEFAULT);

// The Z=0 plane of the relative coordinate system is now on top of the grid, while the
// Z=0 of the absolute coordinate system is on the bottom of the grid, on the working
// surface. If you need to reset the relative coordinate system directly on the working
// surface, use the following:
McalSetCoordinateSystem(ContextCalId, M_RELATIVE_COORDINATE_SYSTEM,
   M_ABSOLUTE_COORDINATE_SYSTEM, M_IDENTITY, M_NULL, M_DEFAULT, M_DEFAULT, M_DEFAULT,
   M_DEFAULT);
```
