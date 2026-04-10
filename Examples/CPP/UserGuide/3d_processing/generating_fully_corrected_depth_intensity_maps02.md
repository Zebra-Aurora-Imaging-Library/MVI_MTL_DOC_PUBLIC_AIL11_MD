---
title: "generating_fully_corrected_depth_intensity_maps02"
description: "Get valid points only"
ms.snippet: "userguide.3d_processing.generating_fully_corrected_depth_intensity_maps02"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# generating_fully_corrected_depth_intensity_maps02

> Get valid points only

**Language:** C++ | **Version:** 8.0+

```cpp
std::vector<AIL_FLOAT> X;
std::vector<AIL_FLOAT> Y;
std::vector<AIL_FLOAT> Z;

/* Retrieve valid points. */
M3dimGet(AilPointCloud, M_COMPONENT_RANGE, M_DEFAULT, M_DEFAULT, X, Y, Z);
```
