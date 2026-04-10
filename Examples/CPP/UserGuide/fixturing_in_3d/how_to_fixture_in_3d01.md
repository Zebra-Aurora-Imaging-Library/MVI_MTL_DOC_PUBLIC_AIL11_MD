---
title: "how_to_fixture_in_3d01"
description: "3D Fixturing using registration"
ms.snippet: "userguide.fixturing_in_3d.how_to_fixture_in_3d01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# how_to_fixture_in_3d01

> 3D Fixturing using registration

**Language:** C++ | **Version:** 7.5+

```cpp
   AIL_ID AilPointCloudArray[2] = { ModelPointCloud, ObjectPointCloud };
   AIL_ID AilRegResult = M3dregAllocResult(AilSystem, M_PAIRWISE_REGISTRATION_RESULT, M_DEFAULT, M_NULL);

   /* Set the preregistration mode to align the centroids of the two point clouds. */
   M3dregControl(AilContext, M_DEFAULT, M_PREREGISTRATION_MODE, M_CENTROID);

   /* Perform registration and write the resulting transformation matrix into a result buffer. */
   M3dregCalculate(AilContext,
	   AilPointCloudArray,
	   2,
	   AilRegResult,
	   M_DEFAULT);

   /* Allocate a transformation matrix object and copy the registration matrix into it. */
   AIL_ID MatrixId = M3dgeoAlloc(M_DEFAULT_HOST, M_TRANSFORMATION_MATRIX, M_DEFAULT, M_NULL);
   M3dregCopyResult(AilRegResult, 1, 0, MatrixId, M_REGISTRATION_MATRIX, M_DEFAULT);

   /* Apply the transformation matrix to the point cloud and store it in another point cloud. */
   M3dimMatrixTransform(ObjectPointCloud, AilFixturedPointCloud, MatrixId, M_DEFAULT);
```
