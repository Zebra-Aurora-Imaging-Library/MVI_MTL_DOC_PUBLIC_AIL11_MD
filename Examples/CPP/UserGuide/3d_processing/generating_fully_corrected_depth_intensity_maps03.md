---
title: "generating_fully_corrected_depth_intensity_maps03"
ms.snippet: "userguide.3d_processing.generating_fully_corrected_depth_intensity_maps03"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# generating_fully_corrected_depth_intensity_maps03

**Language:** C++ | **Version:** 8.0+

```cpp
//<DESCRIPTION NAME = "userguide.3d_processing.generating_fully_corrected_depth_intensity_maps03" CONTENT = "Make only points that point upwards valid"/>

   /* Remove invalid points and points whose normal vector values are zero. */
   M3dimRemovePoints(AilPointCloud, AilPointCloud, M_INVALID_NORMALS, M_DEFAULT);

   /* Retrieve the confidence values and normals information for each point. */
   AIL_ID Confidence = MbufInquireContainer(AilPointCloud, M_COMPONENT_CONFIDENCE, M_COMPONENT_ID, M_NULL);
   AIL_ID Normals = MbufInquireContainer(AilPointCloud, M_COMPONENT_NORMALS_AIL, M_COMPONENT_ID, M_NULL);

   /* Make points whose normals point roughly upward valid, and all other points invalid. */
   auto NormalsZ = MbufChildColor(Normals, 2, M_UNIQUE_ID);
   MimBinarize(NormalsZ, Confidence, M_FIXED + M_GREATER, 0.9, M_NULL);
```
