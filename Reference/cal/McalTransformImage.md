---
doctype: Reference
module: cal
function: McalTransformImage
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / cal / McalTransformImage"
---

# McalTransformImage

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Physically transform an image by removing distortions, or create LUTs that can be used to do the same.

## Syntax

```c
void McalTransformImage(
    AIL_ID    SrcImageBufId,      //in
    AIL_ID    DstImageOrLutId,    //out
    AIL_ID    CalibrationId,      //in
    AIL_INT64 InterpolationMode,  //in
    AIL_INT64 OperationType,      //in
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function removes distortions in an image by physically transforming the image according to a specified camera calibration context. This function can also just extract the warping lookup tables (LUTs) that would be used to transform the image; you can then use these LUTs to transform images with the same distortions, using [`MimWarp`](../../Reference/im/MimWarp.md).

Typically, the image is transformed such that:

- Its pixel coordinate system is aligned with its relative coordinate system.
- All the pixels in the destination image are square and represent the same size in world units.
- It is scaled and positioned in the destination image according to the specified fill mode.

After transformation, the image will be considered physically corrected if an [`M_FULL_CORRECTION`](../../Reference/cal/McalTransformImage.md) operation was performed, and the fill mode scaled the image such that all its pixels are square and represent the same size in world units.

If you want to preserve the average pixel size of the source image, you must allocate your destination image buffer with an appropriate width and height. If you are performing an [`M_FIT`](../../Reference/cal/McalTransformImage.md) type operation, you can use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_TRANSFORM_FIT_SIZE_X_PRESERVE_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) and [`M_TRANSFORM_FIT_SIZE_Y_PRESERVE_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) to determine the width and height of your destination image buffer, in pixels, that will best preserve the source image's average pixel size. Likewise, if you are performing an [`M_CLIP`](../../Reference/cal/McalTransformImage.md) type operation, you can use[`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_TRANSFORM_CLIP_SIZE_X_PRESERVE_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) and [`M_TRANSFORM_CLIP_SIZE_Y_PRESERVE_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) to determine the width and height of your destination image buffer, in pixels, that will best preserve the source image's average pixel size.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageOrLutId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or LUT buffer.

### `CalibrationId` *(in, AIL_ID)*

Specifies the camera calibration context with which to transform the image.

*For specifying the camera calibration context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the camera calibration context associated with the source image will be used.

If the source image is not associated with a camera calibration context, an Aurora Imaging Library error is returned. |
| `Calibration context identifier` | Specifies the identifier of a valid camera calibration context, which you have calibrated using [`McalGrid`](../../Reference/cal/McalGrid.md), [`McalList`](../../Reference/cal/McalList.md), [`McalUniform`](../../Reference/cal/McalUniform.md), or [`McalWarp`](../../Reference/cal/McalWarp.md).

If the source image is also associated with a camera calibration context, it will have precedence over the calibration context passed here, unless the calibration context identifiers differ and the source image does not have a constant pixel size. |

### `InterpolationMode` *(in, AIL_INT64)*

Specifies the interpolation mode to use when associating destination pixels with source points. This parameter must be set to one of the following values:

*For specifying the type of interpolation to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_NEAREST_NEIGHBOR`](../../Reference/cal/McalTransformImage.md) + [`M_OVERSCAN_ENABLE`](../../Reference/cal/McalTransformImage.md). |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated according to the depth and data type of the destination buffer. |
| `M_BILINEAR` | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

*For specifying overscan*

| Value | Description |
| --- | --- |
| `M_OVERSCAN_CLEAR` | Sets the destination pixel to 0, if the associated point falls outside the source buffer. |
| `M_OVERSCAN_DISABLE` | Leaves the destination pixel as is, if the associated point falls outside the source buffer. |
| `M_OVERSCAN_ENABLE` *(default)* | Uses pixels from the source buffer's ancestor buffer, if the associated point falls outside the source buffer. If the source buffer is not a child buffer or if the associated point falls outside the ancestor buffer, it leaves the destination pixel as is. |
| `M_OVERSCAN_FAST` | Specifies that Aurora Imaging Library will automatically select the overscan that optimizes speed, according to the specified operation and the target system. The overscan could be hardware-specific thereby having a different behavior than the other supported overscan modes.

Note that when using [`M_OVERSCAN_FAST`](../../Reference/cal/McalTransformImage.md), the destination pixels in the overscan area are undefined. The pixels can therefore contain different values from one function call to the next, even if the function's parameter values are the same. |

### `OperationType` *(in, AIL_INT64)*

Specifies the function's operation type. This parameter must be set to the following value:

*For specifying the function's operation type*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CORRECT_LENS_DISTORTION_ONLY` | Specifies a partial correction of the source image by only removing lens distortion, without modifying the perspective effect. In this case, the destination image is not calibrated. This operation is only supported for 3D-based camera calibration modes ([`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)). |
| `M_FULL_CORRECTION` *(default)* | Specifies a full correction of the source image. This corrects all distortions of the source image based on the provided camera calibration context. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter must be set to the following value:

*For specifying the type of result to output*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EXTRACT_LUT_X` | Extracts the X warping LUT for the image transformation. You can use the extracted LUT to transform an image using [`MimWarp`](../../Reference/im/MimWarp.md) with the [`OperationMode`](../../Reference/im/MimWarp.md) parameter to [`M_WARP_LUT`](../../Reference/im/MimWarp.md) + [`M_FIXED_POINT + 10`](../../Reference/im/MimWarp.md). Since the LUTs are generated with 10 fractional bits, you must specify this number of fractional bits when you call [`MimWarp`](../../Reference/im/MimWarp.md).

> **Note:** Note, [`InterpolationMode`](../../Reference/cal/McalTransformImage.md) must be set to [`M_DEFAULT`](../../Reference/cal/McalTransformImage.md). |
| `M_EXTRACT_LUT_Y` | Extracts the Y warping LUT for the image transformation. You can use the extracted LUT to transform an image using [`MimWarp`](../../Reference/im/MimWarp.md) with the [`OperationMode`](../../Reference/im/MimWarp.md) parameter to [`M_WARP_LUT`](../../Reference/im/MimWarp.md) + [`M_FIXED_POINT + 10`](../../Reference/im/MimWarp.md). Since the LUTs are generated with 10 fractional bits, you must specify this number of fractional bits when you call [`MimWarp`](../../Reference/im/MimWarp.md).

> **Note:** Note, [`InterpolationMode`](../../Reference/cal/McalTransformImage.md) must be set to [`M_DEFAULT`](../../Reference/cal/McalTransformImage.md). |
| `M_WARP_IMAGE` *(default)* | Transforms the source image and fits it into the destination image according to the specified fill mode. |

*For specifying how to position and scale the source image in the destination image*

| Value | Description |
| --- | --- |
| `M_CLIP` | Specifies that the source image is positioned and scaled such that every destination pixel maps to a valid source pixel. This fill mode does not produce any invalid pixels in the destination image, but some information might be lost. |
| `M_FIT` *(default)* | Specifies that the source image is positioned and scaled such that every source pixel maps to a valid destination pixel. This fill mode might produce invalid (undefined) pixels in the destination image, but no information is lost. |
| `M_USE_DESTINATION_CALIBRATION` | Specifies that the source image is positioned and scaled using the camera calibration information of the destination image. The source image is positioned such that its relative coordinate system is placed at the same location as the current relative coordinate system of the destination image. In addition, the source image is scaled to have the same pixel-to-world mapping as the destination image.

To use this option, the destination image must be a calibrated image with a uniform pixel size. To apply a camera calibration to the destination image that just scales and offsets the world coordinate system from the pixel coordinate system, use [`McalUniform`](../../Reference/cal/McalUniform.md) on the destination buffer prior to calling [`McalTransformImage`](../../Reference/cal/McalTransformImage.md).

Note that this option only affects how the image is positioned and scaled in the destination image; the source image is always corrected according to the camera calibration setting of the [`CalibrationId`](../../Reference/cal/McalTransformImage.md) parameter.

This option cannot be used with [`M_EXTRACT_LUT_X`](../../Reference/cal/McalTransformImage.md) or [`M_EXTRACT_LUT_Y`](../../Reference/cal/McalTransformImage.md) or when [`OperationType`](../../Reference/cal/McalTransformImage.md) is set to [`M_CORRECT_LENS_DISTORTION_ONLY`](../../Reference/cal/McalTransformImage.md). |
