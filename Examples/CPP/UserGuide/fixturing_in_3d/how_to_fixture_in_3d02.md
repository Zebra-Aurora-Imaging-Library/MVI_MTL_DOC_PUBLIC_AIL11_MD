---
title: "how_to_fixture_in_3d02"
description: "3D Fixturing using metrology"
ms.snippet: "userguide.fixturing_in_3d.how_to_fixture_in_3d02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# how_to_fixture_in_3d02

> 3D Fixturing using metrology

**Language:** C++ | **Version:** 7.5+

```cpp
/* Mask out the object. */
M3dimCrop(AilPtCldContainer, AilPtCldContainer, AilBox, M_NULL, M_DEFAULT, M_INVERSE);

/* Fit a plane to the point cloud. */
M3dmetFit(M_DEFAULT, AilPtCldContainer, M_PLANE, AilFitResult, M_INFINITE, M_DEFAULT);

/* Check if the fit was successful. */ 
M3dmetGetResult(AilFitResult, M_STATUS, &Status);
if (Status == M_SUCCESS)
   {
   /*Allocate a transformation matrix object and copy the fit results into it. */
   AilFixturingMatrix = M3dgeoAlloc(AilSystem, M_TRANSFORMATION_MATRIX, M_DEFAULT, M_UNIQUE_ID);
   M3dmetCopyResult(AilFitResult, AilFixturingMatrix, M_FIXTURING_MATRIX, M_DEFAULT);

   /* Apply the transformation matrix to the point cloud of a scanned object. */
   M3dimMatrixTransform(PtCldObject, PtCldObject, AilFixturingMatrix, M_DEFAULT);
   }
```
