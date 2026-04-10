---
title: "calibration_propagation03"
description: "Saving the associated calibration context in a separate file 01"
ms.snippet: "userguide.camera_calibration.calibration_propagation03"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# calibration_propagation03

> Saving the associated calibration context in a separate file 01

**Language:** C++ | **Version:** 7.5+

```cpp
/* Save 2 images in JPEG files. Both images:
   - Are already associated to the same calibration contexts.
   - Are not child images.
   - Have not been corrected using McalTransformImage.
   - Have relative coordinate systems that have not been modified. */
McalInquire(Image1Id, M_ASSOCIATED_CALIBRATION+M_TYPE_AIL_ID, &CalibrationContextId);
McalSave(AIL_TEXT("Calibration.mca"), CalibrationContextId, M_DEFAULT);

MbufExport(AIL_TEXT("Image1.jpg"), M_JPEG_LOSSY, Image1Id);
MbufExport(AIL_TEXT("Image2.jpg"), M_JPEG_LOSSY, Image2Id);

/* Free the images and the calibration context. */
MbufFree(Image1Id);
MbufFree(Image2Id);
McalFree(CalibrationContextId);

/* ... */

/* The following code can be in another application. */

/* Restore the images from the files. */
MbufRestore(AIL_TEXT("Image1.jpg"), SystemId, &Image1Id);
MbufRestore(AIL_TEXT("Image2.jpg"), SystemId, &Image2Id);

/* Restore the calibration context from the file. */
McalRestore(AIL_TEXT("Calibration.mca"), SystemId, M_DEFAULT, &CalibrationContextId);

/* Associate the restored calibration context to the restored images. */
McalAssociate(CalibrationContextId, Image1Id, M_DEFAULT);
McalAssociate(CalibrationContextId, Image2Id, M_DEFAULT);
```
