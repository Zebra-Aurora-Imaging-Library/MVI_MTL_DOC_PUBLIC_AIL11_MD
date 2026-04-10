---
title: "get_depth_map_span"
description: "Calculating the depth map world limits"
ms.snippet: "userguide.3d_reconstruction.get_depth_map_span"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# get_depth_map_span

> Calculating the depth map world limits

**Language:** C++ | **Version:** 8.0+

```cpp
/* Pixel coordinates of the depth map's top-left and bottom right corners. */
AIL_DOUBLE DepthMapPixelX[2] = {-0.5, (AIL_DOUBLE)DepthMapSizeX-0.5};
AIL_DOUBLE DepthMapPixelY[2] = {-0.5, (AIL_DOUBLE)DepthMapSizeY-0.5};

/* Minimum and maximum depth map intensity: [0,254] for 8-bit buffers, and [0,65534] for
   16-bit buffers. Note that 255 and 65535 are reserved values for missing data. */
AIL_DOUBLE DepthMapIntensity[2] = { 0.0, (AIL_DOUBLE)((1 << DepthMapSizeBit)-2)};

/* Compute the minimum and maximum X, Y, Z world coordinates of the depth map. */
AIL_DOUBLE WorldLimitsX[2], WorldLimitsY[2], WorldLimitsZ[2];
McalTransformCoordinate3dList(AilDepthMap, M_PIXEL_COORDINATE_SYSTEM,
   M_RELATIVE_COORDINATE_SYSTEM, 2, DepthMapPixelX, DepthMapPixelY, DepthMapIntensity,
   WorldLimitsX, WorldLimitsY, WorldLimitsZ, M_DEPTH_MAP);
```
