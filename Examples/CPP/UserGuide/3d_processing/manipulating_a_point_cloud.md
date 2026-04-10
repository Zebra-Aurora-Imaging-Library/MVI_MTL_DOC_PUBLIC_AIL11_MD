---
title: "manipulating_a_point_cloud"
description: "Transforming points"
ms.snippet: "userguide.3d_processing.manipulating_a_point_cloud"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# manipulating_a_point_cloud

> Transforming points

**Language:** C++ | **Version:** 8.0+

```cpp
/* Amount of translation and rotation of an object in a scene is established, for example, using 3D model finder. */

/* Set transformation coefficients. */
M3dgeoMatrixSetTransform(AilMatrix, M_TRANSLATION, Translation_X, Translation_Y, Translation_Z, M_DEFAULT, M_DEFAULT);
M3dgeoMatrixSetTransform(AilMatrix, M_ROTATION_XYZ, Rotation_X, Rotation_Y, Rotation_Z, M_DEFAULT, M_COMPOSE_WITH_CURRENT);

/* Find inverse matrix. */
M3dgeoMatrixSetTransform(AilMatrix, M_INVERSE, AilMatrix, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT);

/* Apply transformation to point cloud. */
M3dimMatrixTransform(AilPointCloud, AilDstPointCloud, AilMatrix, M_DEFAULT);
```
