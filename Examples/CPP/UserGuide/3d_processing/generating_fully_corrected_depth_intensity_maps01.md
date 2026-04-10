---
title: "generating_fully_corrected_depth_intensity_maps01"
description: "Generate fully corrected depth map in box region"
ms.snippet: "userguide.3d_processing.generating_fully_corrected_depth_intensity_maps01"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# generating_fully_corrected_depth_intensity_maps01

> Generate fully corrected depth map in box region

**Language:** C++ | **Version:** 8.0+

```cpp
/* Crop the point cloud. */
M3dimCrop(AilPointCloud, AilCroppedPointCloud, AilBox, M_NULL, M_DEFAULT, M_DEFAULT);
M3dimCalibrateDepthMap(AilBox, AilDepthmap, M_NULL, M_NULL, M_DEFAULT, M_POSITIVE, M_DEFAULT);

/* Project the point cloud on the depth map. */
M3dimProject(AilCroppedPointCloud, AilDepthmap, M_NULL, M_POINT_BASED, M_MAX_Z, M_DEFAULT, M_DEFAULT);
```
