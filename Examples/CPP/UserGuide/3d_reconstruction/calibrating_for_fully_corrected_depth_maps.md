---
title: "calibrating_for_fully_corrected_depth_maps"
description: "Calibrating for a fully corrected depth map"
ms.snippet: "userguide.3d_reconstruction.calibrating_for_fully_corrected_depth_maps"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# calibrating_for_fully_corrected_depth_maps

> Calibrating for a fully corrected depth map

**Language:** C++ | **Version:** 8.0+

```cpp
/* Allocate 3dmap objects. */
M3dmapAlloc(AilSystemId, M_LASER, M_CALIBRATED_CAMERA_LINEAR_MOTION, &LaserId);
M3dmapAllocResult(AilSystemId, M_LASER_CALIBRATION_DATA, M_DEFAULT, &ScanId);

/* Inquire internal locate peak Mim context Id, if peak detection needs configuration. */
M3dmapInquire(LaserId, M_DEFAULT, M_LOCATE_PEAK_1D_CONTEXT_ID, &AilPeakLocatorId);

/* Set settings with M3dmapControl() and MimControl(), if needed. */
/* ... */

/* Get image of laser line on Z=0 reference plane.  */
/* MbufLoad can also be used to load an image.      */
MdigGrab(DigId, AilImageId);

/* Calibrate 3D reconstruction context. */
/* AilCalibrationId is the identifier of the calibration context calibrated by
   McalGrid() or McalList() in a 3D mode. */
M3dmapAddScan(LaserId, ScanId, AilImageId, M_NULL, M_NULL, M_DEFAULT, M_DEFAULT);
M3dmapCalibrate(LaserId, ScanId, AilCalibrationId, M_DEFAULT);
```
