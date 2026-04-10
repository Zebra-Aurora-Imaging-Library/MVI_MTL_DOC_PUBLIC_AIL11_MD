---
doctype: Reference
module: im
function: MimRotate
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimRotate"
---

# MimRotate

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
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Yes |

> Rotate an image by a specified angle of rotation or generate the matrix to perform the transformation.

## Syntax

```c
void MimRotate(
    AIL_ID     SrcImageBufId,         //in
    AIL_ID     DstImageOrArrayBufId,  //out
    AIL_DOUBLE Angle,                 //in
    AIL_DOUBLE SrcCenX,               //in
    AIL_DOUBLE SrcCenY,               //in
    AIL_DOUBLE DstCenX,               //in
    AIL_DOUBLE DstCenY,               //in
    AIL_INT64  InterpolationMode      //in
)
```

## Description

This function rotates an image by the specified angle of rotation. Alternatively, this function can generate the 3x3 or 3x2 warp matrix required to perform the transformation using [`MimWarp`](../../Reference/im/MimWarp.md). If you specify a source image, it will be rotated using the specified interpolation mode. The center of rotation in the source image is determined by the specified X- and Y-source rotation-center coordinates. The rotated image will then be clipped to fit the destination buffer. It will be placed in the destination buffer with its center positioned at the specified X- and Y-destination center coordinates.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the source image to rotate if performing the transformation. This parameter can be set to one of the following values:

*For specifying the source image buffer identifier*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to ignore this parameter; this parameter must be set to [`M_NULL`](../../Reference/im/MimRotate.md)when generating the matrix that you can use with [`MimWarp`](../../Reference/im/MimWarp.md) to perform the rotation. |
| `Image buffer identifier` | Specifies the identifier of the source image buffer to rotate.

When rotating an image, this parameter must be given an image buffer identifier.

This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `DstImageOrArrayBufId` *(out, AIL_ID)*

Specifies the destination image buffer or array buffer, depending on whether performing the transformation. This parameter can be set to one of the following values:

*For specifying the destination buffer identifier*

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the destination [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer when generating the matrix.

When generating a warp matrix, this parameter must be given a single-band, 32-bit floating-point buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md)attribute and that has dimensions 3x2, for a first-order polynomial warping, or 3x3, for a perspective polynomial warping. If you pass this parameter a buffer with dimensions 3x2, [`MimWarp`](../../Reference/im/MimWarp.md)will assume the third row to be (0, 0, 1) when performing the warping. |
| `Image buffer identifier` | Specifies the identifier of the destination image buffer when rotating the source image.

When rotating an image, this parameter must be given an image buffer identifier.

This buffer must not have a region of interest (ROI) associated with it. Using a buffer with an ROI will cause an error.

> **Note:** Note that after performing the operation, the destination image will not be calibrated, even if the source image was calibrated. |

### `Angle` *(in, AIL_DOUBLE)*

Specifies the angle of rotation.

*For specifying the rotation angle*

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the angle of rotation, in degrees. When a positive angle is specified, the function rotates the image in a counter-clockwise direction, regardless of camera calibration settings. |

### `SrcCenX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate to use as the center of rotation in the source image. This parameter must be set to one of the values below.

*For specifying the X-coordinate (source)*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Causes the image to rotate about its true center. When specifying[`M_DEFAULT`](../../Reference/im/MimRotate.md)as a center of rotation, it is computed as `(_source buffer size_ - 1) / 2`.

You can only use this value when rotating an image. In this case, you must pass[`SrcImageBufId`](../../Reference/im/MimRotate.md)a source image buffer identifier, and not [`M_NULL`](../../Reference/im/MimRotate.md). |
| `Value` | Specifies the X-coordinate in the pixel coordinate system.

You must specify a specific value for the X coordinate of the source buffer center when only generating the warp matrix to perform the transformation. |

### `SrcCenY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate to use as the center of rotation in the source image. This parameter must be set to one of the values below.

*For specifying the Y-coordinate (source)*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Causes the image to rotate about its true center. When specifying[`M_DEFAULT`](../../Reference/im/MimRotate.md)as a center of rotation, it is computed as `(_source buffer size_ - 1) / 2`.

You can only use this value when rotating an image. In this case, you must pass[`SrcImageBufId`](../../Reference/im/MimRotate.md)a source image buffer identifier, and not [`M_NULL`](../../Reference/im/MimRotate.md). |
| `Value` | Specifies the Y-coordinate in the pixel coordinate system.

You must specify a specific value for the Y coordinate of the source buffer center when only generating the warp matrix to perform the transformation. |

### `DstCenX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate in the destination buffer to which the specified center of the rotated source image will be mapped. This parameter must be set to one of the values below.

*For specifying the X-coordinate (destination)*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Causes the true center of the destination buffer to be used. When specifying[`M_DEFAULT`](../../Reference/im/MimRotate.md)as a center of rotation, it is computed as `(_destination buffer size_ - 1) / 2`.

You can only use this value when rotating an image. In this case, you must pass[`SrcImageBufId`](../../Reference/im/MimRotate.md)a source image buffer identifier, and not [`M_NULL`](../../Reference/im/MimRotate.md). |
| `Value` | Specifies the X-coordinate in the pixel coordinate system.

You must specify a specific value for the X coordinate of the destination buffer center when only generating the warp matrix to perform the transformation. |

### `DstCenY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate in the destination buffer to which the specified center of the rotated source image will be mapped. This parameter must be set to one of the values below.

*For specifying the Y-coordinate (destination)*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Causes the true center of the destination buffer to be used. When specifying[`M_DEFAULT`](../../Reference/im/MimRotate.md)as a center of rotation, it is computed as `(_destination buffer size_ - 1) / 2`.

You can only use this value when rotating an image. In this case, you must pass[`SrcImageBufId`](../../Reference/im/MimRotate.md)a source image buffer, and not [`M_NULL`](../../Reference/im/MimRotate.md). |
| `Value` | Specifies the Y-coordinate in the pixel coordinate system.

You must specify a specific value for the Y coordinate of the destination buffer center when only generating the warp matrix to perform the transformation. |

### `InterpolationMode` *(in, AIL_INT64)*

Specifies the interpolation mode when rotating the source image. When only generating the matrix, this parameter is ignored and should be set to [`M_DEFAULT`](../../Reference/im/MimRotate.md).

*For specifying the interpolation mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. When rotating the source image, the default is the same as [`M_NEAREST_NEIGHBOR`](../../Reference/im/MimRotate.md) + [`M_OVERSCAN_ENABLE`](../../Reference/im/MimRotate.md).

When only generating the coefficients matrix, this is the only possible value. |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated according to the depth and data type of the destination buffer. |
| `M_BILINEAR` | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

*For overscan*

| Value | Description |
| --- | --- |
| `M_OVERSCAN_CLEAR` | Sets the destination pixel to 0. |
| `M_OVERSCAN_DISABLE` | Leaves the destination pixel as is. |
| `M_OVERSCAN_ENABLE` *(default)* | Uses pixels from the source buffer's ancestor buffer. If the source buffer is not a child buffer or if the point falls outside the ancestor buffer, it leaves the destination pixel as is. |
| `M_OVERSCAN_FAST` | Specifies that Aurora Imaging Library will automatically select the overscan that optimizes speed, according to the specified operation and the target system. The overscan could be hardware-specific thereby having a different behavior than the other supported overscan modes.

Note that when using [`M_OVERSCAN_FAST`](../../Reference/im/MimWarp.md), the destination pixels in the overscan area are undefined. The pixels can therefore contain different values from one function call to the next, even if the function's parameter values are the same. |

## Remarks

> In-place processing is supported, but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
