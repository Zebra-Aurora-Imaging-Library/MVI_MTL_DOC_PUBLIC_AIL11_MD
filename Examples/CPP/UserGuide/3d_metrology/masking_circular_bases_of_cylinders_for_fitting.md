---
title: "masking_circular_bases_of_cylinders_for_fitting"
description: "Uses a fitted plane as a mask to remove points"
ms.snippet: "userguide.3d_metrology.masking_circular_bases_of_cylinders_for_fitting"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# masking_circular_bases_of_cylinders_for_fitting

> Uses a fitted plane as a mask to remove points

**Language:** C++ | **Version:** 7.5+

```cpp
// Specify a tolerance for the fitting operation
static const AIL_DOUBLE PLANE_TOLERANCE = 1.0;

// Fit a plane using default settings
AIL_ID AilFitResult = M3dmetAllocResult(AilSystem, M_FIT_RESULT, M_DEFAULT, M_NULL);
M3dmetFit(M_DEFAULT, AilPointCloud, M_PLANE, AilFitResult, PLANE_TOLERANCE, M_DEFAULT);

// Inquire the ID of the point cloud's confidence component
AIL_ID ConfidenceBuffer;
MbufInquireContainer(AilPointCloud, M_COMPONENT_CONFIDENCE, M_COMPONENT_ID, &ConfidenceBuffer);

// Copy only the points that were considered outliers during the fit operation 
// into ConfidenceBuffer
M3dmetCopyResult(AilFitResult, ConfidenceBuffer, M_OUTLIER_MASK, M_DEFAULT);

M3dmetFree(AilFitResult);
```
