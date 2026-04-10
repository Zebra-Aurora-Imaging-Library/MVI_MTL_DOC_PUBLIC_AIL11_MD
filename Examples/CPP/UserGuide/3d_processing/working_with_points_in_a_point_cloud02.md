---
title: "working_with_points_in_a_point_cloud02"
description: "Subsampling to keep feature points"
ms.snippet: "userguide.3d_processing.working_with_points_in_a_point_cloud02"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# working_with_points_in_a_point_cloud02

> Subsampling to keep feature points

**Language:** C++ | **Version:** 8.0+

```cpp
/* Allocate a subsample context and set it to normal vector mode. */
AilSubsampleContext = M3dimAlloc(AilSystem, M_SUBSAMPLE_CONTEXT, M_DEFAULT, M_NULL);
M3dimControl(AilSubsampleContext, M_SUBSAMPLE_MODE, M_SUBSAMPLE_NORMAL);

/* Control what makes a point distinct from its neighbors. */
M3dimControl(AilSubsampleContext, M_DISTINCT_ANGLE_DIFFERENCE, 12);
M3dimControl(AilSubsampleContext, M_NEIGHBORHOOD_DISTANCE, 10);

/* Apply sampling. */
M3dimSample(AilSubsampleContext, AilPointCloud, AilDstPointCloud, M_DEFAULT);
```
