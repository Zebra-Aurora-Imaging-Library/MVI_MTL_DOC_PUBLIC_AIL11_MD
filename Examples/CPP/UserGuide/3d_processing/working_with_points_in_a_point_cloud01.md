---
title: "working_with_points_in_a_point_cloud01"
description: "Subsampling using a grid"
ms.snippet: "userguide.3d_processing.working_with_points_in_a_point_cloud01"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# working_with_points_in_a_point_cloud01

> Subsampling using a grid

**Language:** C++ | **Version:** 8.0+

```cpp
static const AIL_DOUBLE GRID_SIZE = 1.0;

/* Allocate a subsample context and set it to grid mode. */
AilSubsampleContext = M3dimAlloc(AilSystem, M_SUBSAMPLE_CONTEXT, M_DEFAULT, M_NULL);
M3dimControl(AilSubsampleContext, M_SUBSAMPLE_MODE, M_SUBSAMPLE_GRID);

/* Organize the unorganized source point cloud with resolution in X and Y equal to the grid size. */
M3dimControl(AilSubsampleContext, M_ORGANIZATION_TYPE, M_ORGANIZED);
M3dimControl(AilSubsampleContext, M_GRID_SIZE_Z, M_INFINITE);
M3dimControl(AilSubsampleContext, M_GRID_SIZE_X, GRID_SIZE);
M3dimControl(AilSubsampleContext, M_GRID_SIZE_Y, GRID_SIZE);

/* Select the point with the lowest coordinate along the M_INFINITE axis for each cell. */
M3dimControl(AilSubsampleContext, M_POINT_SELECTED, M_MIN);

/* Apply sampling. */
M3dimSample(AilSubsampleContext, AilPointCloud, AilDstPointCloud, M_DEFAULT);
```
