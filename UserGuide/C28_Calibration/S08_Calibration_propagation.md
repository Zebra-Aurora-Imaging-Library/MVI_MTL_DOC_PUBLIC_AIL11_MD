---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Calibration_propagation
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Calibration propagation"
---

# Camera calibration propagation

Camera calibration contexts store a lot of information. When you associate a camera calibration context with an image or a digitizer, some of this information is copied and some of it is just referenced. In addition, when an operation is done on a calibrated image, its camera calibration information is typically propagated to the destination image or result buffer.

The three types of information stored in a camera calibration context are:

- The non-linear mapping between the world and pixel coordinate systems.
- Properties of the calibration setup, such as the type of grid used and its size.
- Camera calibration information, which represents the most functional part of the calibration, and consists primarily of a copy of all coordinate systems.

When a digitizer is associated with a camera calibration context ([`McalAssociate`](../../Reference/cal/McalAssociate.md)), the digitizer only retains a link to the camera calibration context. When you grab images with the digitizer, the grabbed images are automatically associated with the camera calibration context. A digitizer with an associated camera calibration context is known as a calibrated digitizer. To disassociate a camera calibration context from an image or digitizer, use [`McalAssociate`](../../Reference/cal/McalAssociate.md) with [`M_NULL`](../../Reference/cal/McalAssociate.md).

When an image is associated with a camera calibration context, it receives a copy of the calibration's coordinate systems and a reference to the camera calibration context for all other settings, including the intrinsic attributes. This data is referred to as the camera calibration information. An image with an associated camera calibration context is known as a calibrated image. If you change the relative coordinate system or camera position of the camera calibration context after association, the change will not affect the image, because the image has its own camera calibration information. If you change any other setting of the camera calibration context after association, the change will affect the calibrated image because those settings are retrieved through the reference to the camera calibration context.

A result buffer used by an analysis operation can also contain camera calibration information. If the Aurora Imaging Library module supports returning results in real-world units, the camera calibration information stored with the source images will be copied to the result buffer. For more information on getting results with respect to real-world units, see [Working with real-world units](S09_Working_with_realworld_units.md).

*[Image: cal_controls.png]*

When a child buffer is allocated in a calibrated image, the child buffer receives a copy of the camera calibration information stored in the parent, taking into account the different origin in the pixel coordinate system. Consequently, the absolute and relative coordinate systems of the child buffer are initially the same as the parent image, when the child is allocated. Since the child buffer has its own copy of the coordinate systems, you can displace its relative coordinate system to any new location. However, there exists a rigid link between the parent and child's relative coordinate systems, so if you move the parent's relative coordinate system, the child buffer's relative coordinate system will be displaced to maintain the same position and orientation that was previously established with respect to the parents relative coordinate system. For more information, see [Multiple relative coordinate systems](../C29_Fixturing/S04_Calibration_and_one_or_many_fixtures.md).

You can also use [`McalAssociate`](../../Reference/cal/McalAssociate.md) to copy the camera calibration information from an image or result buffer to another image or result buffer. [`MbufCopy`](../../Reference/buf/MbufCopy.md) will also copy all the camera calibration information associated with a buffer to the specified destination buffer.

## Processing calibrated images

When a camera calibration context is associated with an image, it has certain implications on processing operations applied to that image. Depending on the type of operation performed on the calibrated image, the destination image also receives a copy of the camera calibration information for the following circumstances:

- When performing point-to-point or neighborhood processing operations, the destination image receives a copy of the camera calibration information from the source image.
  If the operation uses more than one source image, the camera calibration information gets copied to the destination image only if all source images have the same camera calibration information; otherwise, the camera calibration information of the destination image is left unchanged.
- Geometric functions ([`MimFlip`](../../Reference/im/MimFlip.md), [`MimPolarTransform`](../../Reference/im/MimPolarTransform.md), [`MimResize`](../../Reference/im/MimResize.md), [`MimRotate`](../../Reference/im/MimRotate.md), [`MimTranslate`](../../Reference/im/MimTranslate.md), and [`MimWarp`](../../Reference/im/MimWarp.md)) always result in an uncalibrated image, even if the source image is calibrated.
  > **Note:** You can use [`McalWarp`](../../Reference/cal/McalWarp.md) to warp an existing calibration context and associate it with a warped uncalibrated image. For more information, see [Propagating camera calibration information after performing a geometric operation](S17_Propagating_calibration_information_after_performing_a_geometric_operation.md).
- The function [`MbufClear`](../../Reference/buf/MbufClear.md) always results in an uncalibrated image. You can use [`MgraClear`](../../Reference/gra/MgraClear.md) to clear a buffer while preserving the camera calibration context associated with the empty image.
- Functions for which the source data is always considered uncalibrated always produce uncalibrated images. These functions include, for example, [`MbufImport...`](../../Reference/buf/MbufImport.md) and [`MbufLoad`](../../Reference/buf/MbufLoad.md).
- A result buffer will receive a copy of the camera calibration information from the source images used to generate the result if and only if all source images have the same camera calibration information and the module supports returning results in world units; otherwise, the result buffer is left uncalibrated.

## Saving and reloading a calibrated image

When saving and reloading a calibrated image, you must account for its associated camera calibration context. You can either save it with the image file, or save it in a separate file. By default, the image's camera calibration information is not saved and, therefore, is not reloaded.

To inquire whether an image is calibrated, use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/cal/McalInquire.md).

### Saving the calibration with the image file

To save the image's associated calibrated object with the image file, use [`MbufExport`](../../Reference/buf/MbufExport.md) with [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md) + [`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md). To restore the associated camera calibration context, use [`McalRestore`](../../Reference/cal/McalRestore.md).

Typically, when restoring the camera calibration context, you will also restore the associated image, using [`MbufImport`](../../Reference/buf/MbufImport.md), [`MbufLoad`](../../Reference/buf/MbufLoad.md), or [`MbufRestore`](../../Reference/buf/MbufRestore.md), and then associate the restored image buffer to the restored camera calibration, using [`McalAssociate`](../../Reference/cal/McalAssociate.md). In this case, the calibrated image will always be in the same state as it was when it was previously saved. The following is a general example of how to save/restore the calibration (and the image) using [`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md).

> **Code example:** [userguide.camera_calibration.calibration_propagation01](userguide.camera_calibration.calibration_propagation01)

When using [`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md), the restored camera calibration context contains information relevant only to the restored image (of the same file); it is not recommended that you associate it with another image. For example, if you restore the camera calibration context ([`McalRestore`](../../Reference/cal/McalRestore.md)) from an image file that had been saved using [`MbufExport`](../../Reference/buf/MbufExport.md) with [`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md), the restored camera calibration context and the camera calibration context that was originally associated with the saved image, can have different settings. This can occur because the calibration settings that are specific to the saved image (such as the position of the relative coordinate system) are also present in the restored camera calibration context.

When restoring an image with a constant pixel size (including depth maps), use [`MbufImport`](../../Reference/buf/MbufImport.md) with [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md)+[`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md). This imports the image along with its uniform camera calibration information. In this case, the camera calibration is associated with a default uniform camera calibration.

### Saving the associated camera calibration context in a separate file

In certain cases, you might not want to save the associated camera calibration context in the same file as the image ([`MbufExport`](../../Reference/buf/MbufExport.md) with [`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md)). For example:

- You do not want to save the image in an Aurora Imaging Library file format.
  [`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md) must be used in combination with [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md). You cannot, for example, use an [`M_JPEG...`](../../Reference/buf/MbufExport.md) format.
- You want to associate the same camera calibration context to multiple images, but you only want to save it once (for example, to conserve disk space).

To save a calibrated image and its camera calibration context in a separate file, perform the following:

1. Obtain the identifier of the image's camera calibration context, using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/cal/McalInquire.md).
2. Save the camera calibration context, using [`McalSave`](../../Reference/cal/McalSave.md) or [`McalStream`](../../Reference/cal/McalStream.md).
3. Save the image buffer, using [`MbufSave`](../../Reference/buf/MbufSave.md) or [`MbufExport`](../../Reference/buf/MbufExport.md).

To reload the image and associate it with its camera calibration context, perform the following:

1. Restore the image buffer, using [`MbufRestore`](../../Reference/buf/MbufRestore.md).
2. Restore the camera calibration context, using [`McalRestore`](../../Reference/cal/McalRestore.md).
3. Associate the restored image with the restored camera calibration context, using [`McalAssociate`](../../Reference/cal/McalAssociate.md).

The following is an example of how to save two calibrated images and their common camera calibration context in separate files, and then reload the images and associate them with the restored camera calibration context:

> **Code example:** [userguide.camera_calibration.calibration_propagation03](userguide.camera_calibration.calibration_propagation03)

Note that if one or more of the following conditions are true, this save/reload procedure can result in a calibrated image that might not be in the same state as the previously saved image:

- The previously saved image was corrected, using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md).
- The previously saved image was calibrated, using [`McalUniform`](../../Reference/cal/McalUniform.md).
- The previously saved image was a child image.
- The relative coordinate system of the previously saved image was moved.

When you associate a camera calibration context with an image, its child images are also associated with the camera calibration context; their offset from the parent image is automatically taken into account. If you save a child image on its own into a file and restore it, it becomes an ordinary stand alone image; if you reassociate it with the original camera calibration context, the camera calibration context cannot automatically know that the image's pixel coordinate system should be offset from the original mapping.

If the camera calibration context is restored from a separate file, you can restore the calibration of the image (that used to be a child image) by associating it with the restored camera calibration context, and then specifying its original offset from its original parent that was calibrated, using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_CALIBRATION_CHILD_OFFSET_X`](../../Reference/cal/McalInquire.md) and [`M_CALIBRATION_CHILD_OFFSET_Y`](../../Reference/cal/McalInquire.md). The following is a general example of how to perform this procedure:

> **Code example:** [userguide.camera_calibration.calibration_propagation04](userguide.camera_calibration.calibration_propagation04)

If you create a camera calibration context for a child image, associate it to the child image, and then save the image separately from its parent, you will not need to specify its offset from its parent. Similarly, if you save the camera calibration context and the child image in the same file, you will not need to specify the child offset from its parent.
