---
title: "calibrating_for_partially_corrected_depth_maps"
description: "Calibrating for a partially corrected depth map"
ms.snippet: "userguide.3d_reconstruction.calibrating_for_partially_corrected_depth_maps"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# calibrating_for_partially_corrected_depth_maps

> Calibrating for a partially corrected depth map

**Language:** C++ | **Version:** 8.0+

```cpp
/* Allocate 3dmap objects. */
M3dmapAlloc(AilSystemId, M_LASER, M_DEPTH_CORRECTION, &LaserId);
M3dmapAllocResult(AilSystemId, M_LASER_CALIBRATION_DATA, M_DEFAULT, &ScanId);

/* Inquire internal locate peak Mim context Id, if peak detection needs configuration. */
M3dmapInquire(LaserId, M_DEFAULT, M_LOCATE_PEAK_1D_CONTEXT_ID, &AilPeakLocatorId);

/* Set settings with M3dmapControl() and MimControl(), if needed. */
/* ... */

for (n = 0; n < NB_CALIBRATION_IMAGES; n++)
   {
   /* Grab next image. */
   /* MbufLoad() can also be used to load an image. */
   MdigGrab(DigId, AilImageId);

   /* Set desired corrected depth of next reference plane. */
   /* CorrectedDepths is an array which has been filled with known depths. */
   M3dmapControl(LaserId, M_DEFAULT, M_CORRECTED_DEPTH, CorrectedDepths[n]);

   /* Analyze the image to extract laser line. */
   M3dmapAddScan(LaserId, ScanId, AilImageId, M_NULL, M_NULL, M_DEFAULT, M_DEFAULT);
   }

/* Calibrate the laser profiling context using reference planes of known heights. */
M3dmapCalibrate(LaserId, ScanId, M_NULL, M_DEFAULT);
```
