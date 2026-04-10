---
title: "calibration_propagation04"
description: "Saving the associated calibration context in a separate file 01"
ms.snippet: "userguide.camera_calibration.calibration_propagation04"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# calibration_propagation04

> Saving the associated calibration context in a separate file 01

**Language:** C++ | **Version:** 7.5+

```cpp
/* Associate a calibration to an image and allocate a child of this image. */
McalAssociate(CalibrationContextId, ImageId, M_DEFAULT);
MbufChild2d(ImageId, ChildOffsetX, ChildOffsetY, ChildSizeX, ChildSizeY, &ChildId);

/* Save the child images and the calibration context in separate files. */
MbufExport(AIL_TEXT("Child.mim"), M_AIL_TIFF, ChildId);
McalSave(AIL_TEXT("Calibration.mca"), CalibrationContextId, M_DEFAULT);

/* Free the image and the calibration context. */
MbufFree(ChildId);
McalFree(CalibrationContextId);

/* ... */

/* The following code can be in another application. */

/* Restore the image from the file. */
MbufRestore(AIL_TEXT("Child.mim"), SystemId, &ChildId);

/* Restore the calibration context from the file. */
McalRestore(AIL_TEXT("Calibration.mca"), SystemId, M_DEFAULT, &CalibrationContextId);

/* Associate the restored calibration context to the restored image. */
McalAssociate(CalibrationContextId, ChildId, M_DEFAULT);

/* Specify the original offset of the restored image from its parent
   (at the time it was originally calibrated). */
McalControl(ChildId, M_CALIBRATION_CHILD_OFFSET_X, ChildOffsetX);
McalControl(ChildId, M_CALIBRATION_CHILD_OFFSET_Y, ChildOffsetY);
```
