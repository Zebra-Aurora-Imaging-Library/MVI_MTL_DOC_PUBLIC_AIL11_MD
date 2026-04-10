---
title: "calibration_propagation01"
description: "Saving the calibration with the image file 01"
ms.snippet: "userguide.camera_calibration.calibration_propagation01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# calibration_propagation01

> Saving the calibration with the image file 01

**Language:** C++ | **Version:** 7.5+

```cpp
/* Save the image along with its associated calibration in a file. */
MbufExport(AIL_TEXT("Image.mim"), M_AIL_TIFF+M_WITH_CALIBRATION, ImageId);

/* Free the image. */
MbufFree(ImageId);

/* ... */

/* The following code can be in another application. */

/* Restore the image from the file. */
MbufRestore(AIL_TEXT("Image.mim"), SystemId, &ImageId);

/* Restore the calibration object from the file. */
McalRestore(AIL_TEXT("Image.mim"), SystemId, M_DEFAULT, &CalibrationContextId);

/* Associate the restored calibration context to the restored image. */
McalAssociate(CalibrationContextId, ImageId, M_DEFAULT);
```
