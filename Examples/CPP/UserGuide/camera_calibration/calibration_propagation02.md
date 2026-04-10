---
title: "calibration_propagation02"
description: "Saving the calibration with the image file 02"
ms.snippet: "userguide.camera_calibration.calibration_propagation02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# calibration_propagation02

> Saving the calibration with the image file 02

**Language:** C++ | **Version:** 7.5+

```cpp
/* Edit calibration state of many images. */

McalAssociate(OriginalCalibrationContextId, ImageId, M_NULL);

/* Correct the image. */
McalTransformImage(ImageId, CorrectedImageId, M_NULL, 
                   M_DEFAULT, M_DEFAULT, M_DEFAULT);

/* Allocate a child of the image. */
MbufChild2d(ImageId, 10, 10, 100, 100, &ChildId);

/* Change the relative origin of the image.
   Note: This can also have been done using McalFixture or McalSetCoordinateSystem. */
McalRelativeOrigin(ImageId, 10.0, 20.0, 0.0, 45.0, M_DEFAULT);

/* Save the images along with their calibration contexts. */
MbufExport(AIL_TEXT("Corrected.mim"), M_AIL_TIFF+M_WITH_CALIBRATION, CorrectedImageId);
MbufExport(AIL_TEXT("Child.mim")    , M_AIL_TIFF+M_WITH_CALIBRATION, ChildId);
MbufExport(AIL_TEXT("Image.mim")    , M_AIL_TIFF+M_WITH_CALIBRATION, ImageId);

/* Free all images and restore them. */
MbufFree(CorrectedImageId);
MbufFree(ChildId);
MbufFree(ImageId);

MbufRestore(AIL_TEXT("Corrected.mim"), SystemId, &CorrectedImageId);
MbufRestore(AIL_TEXT("Child.mim")    , SystemId, &ChildId);
MbufRestore(AIL_TEXT("Image.mim")    , SystemId, &ImageId);

/* Restore the calibration context from the same files. */
McalRestore(AIL_TEXT("Corrected.mim"), SystemId, M_DEFAULT, &CorrectedCalObjectId);
McalRestore(AIL_TEXT("Child.mim")    , SystemId, M_DEFAULT, &ChildCalObjectId);
McalRestore(AIL_TEXT("Image.mim")    , SystemId, M_DEFAULT, &ImageCalObjectId);

McalAssociate(CorrectedCalObjectId, CorrectedImageId, M_DEFAULT);
McalAssociate(ChildCalObjectId    , ChildId         , M_DEFAULT);
McalAssociate(ImageCalObjectId    , ImageId         , M_DEFAULT);

/* At this point, the mapping between pixel coordinates and relative coordinates
   for each image is exactly the same as it was before saving.
   However, none of the restored calibration contexts are the same as the original
   calibration context (CalibrationContextId). */
```
