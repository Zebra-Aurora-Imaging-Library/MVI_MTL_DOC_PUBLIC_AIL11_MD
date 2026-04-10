---
title: "working_with_points_in_a_point_cloud03"
description: "Subsampling using decimation"
ms.snippet: "userguide.3d_processing.working_with_points_in_a_point_cloud03"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# working_with_points_in_a_point_cloud03

> Subsampling using decimation

**Language:** C++ | **Version:** 8.0+

```cpp
/* Allocate a subsample context and set it to decimate mode. */
AilSubsampleContext = M3dimAlloc(AilSystem, M_SUBSAMPLE_CONTEXT, M_DEFAULT, M_NULL);
M3dimControl(AilSubsampleContext, M_SUBSAMPLE_MODE, M_SUBSAMPLE_DECIMATE);

/* Set the interval size and specify to keep the organization type. */
M3dimControl(AilSubsampleContext, M_STEP_SIZE_X, DECIMATION_STEP);
M3dimControl(AilSubsampleContext, M_STEP_SIZE_Y, DECIMATION_STEP);
M3dimControl(AilSubsampleContext, M_ORGANIZATION_TYPE, M_ORGANIZED);

/* Apply sampling. */
M3dimSample(AilSubsampleContext, AilPointCloud, AilDstPointCloud, M_DEFAULT);
```
