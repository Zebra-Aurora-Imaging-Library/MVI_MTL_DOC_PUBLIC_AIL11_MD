---
title: "configuring_the_laser_line_setup"
description: "Configuring the locate peak 1D parameters for laser line extraction"
ms.snippet: "userguide.3d_reconstruction.configuring_the_laser_line_setup"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# configuring_the_laser_line_setup

> Configuring the locate peak 1D parameters for laser line extraction

**Language:** C++ | **Version:** 8.0+

```cpp
/* Allocate the 3D reconstruction context used for laser line profiling*/
M3dmapAlloc(AilSystemId, M_LASER, M_CALIBRATED_CAMERA_LINEAR_MOTION, &AilLaserContextId);

/* Set the locate peak 1D parameters for laser line extraction. */
M3dmapInquire(AilLaserContextId, M_DEFAULT, M_LOCATE_PEAK_1D_CONTEXT_ID + M_TYPE_AIL_ID, &AilPeakLocatorId);
MimControl(AilPeakLocatorId, M_PEAK_WIDTH_NOMINAL, PEAK_WIDTH_NOMINAL);
MimControl(AilPeakLocatorId, M_PEAK_WIDTH_DELTA, PEAK_WIDTH_DELTA);
MimControl(AilPeakLocatorId, M_MINIMUM_CONTRAST, MIN_CONTRAST);
```
