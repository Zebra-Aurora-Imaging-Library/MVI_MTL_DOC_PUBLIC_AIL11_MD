---
title: "extracting_depth_map"
description: "Extracting a partially corrected depth map"
ms.snippet: "userguide.3d_reconstruction.extracting_depth_map"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# extracting_depth_map

> Extracting a partially corrected depth map

**Language:** C++ | **Version:** 8.0+

```cpp
/* Retrieve the expected SizeX and SizeY of the depth map and type of the intensity map.*/
M3dmapGetResult(ScanId, M_DEFAULT,
   M_PARTIALLY_CORRECTED_DEPTH_MAP_SIZE_X+M_TYPE_AIL_INT, &SizeX);
M3dmapGetResult(ScanId, M_DEFAULT,
   M_PARTIALLY_CORRECTED_DEPTH_MAP_SIZE_Y+M_TYPE_AIL_INT, &SizeY);
M3dmapGetResult(ScanId, M_DEFAULT,
   M_INTENSITY_MAP_BUFFER_TYPE+M_TYPE_AIL_INT, &IntensityMapType);

/* Allocate image buffers for the depth map and intensity map. */
MbufAlloc2d(AilSystemId, SizeX, SizeY, 16+M_UNSIGNED,
                        M_IMAGE+M_PROC, &DepthMapImageId);
MbufAlloc2d(AilSystemId, SizeX, SizeY, IntensityMapType, 
                        M_IMAGE+M_PROC, &IntensityMapImageId);

/* Generate the depth map and intensity map.  */
M3dmapCopyResult(ScanId, M_DEFAULT, DepthMapImageId, M_PARTIALLY_CORRECTED_DEPTH_MAP, M_DEFAULT);
M3dmapCopyResult(ScanId, M_DEFAULT, IntensityMapImageId, M_INTENSITY_MAP, M_DEFAULT);
```
