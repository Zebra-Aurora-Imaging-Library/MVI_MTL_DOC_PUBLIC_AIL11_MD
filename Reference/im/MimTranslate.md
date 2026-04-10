---
doctype: Reference
module: im
function: MimTranslate
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimTranslate"
---

# MimTranslate

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

> Translate an image by a specified X- and/or Y-displacement, or generate the matrix to perform the transformation.

## Syntax

```c
void MimTranslate(
    AIL_ID     SrcImageBufId,         //in
    AIL_ID     DstImageOrArrayBufId,  //out
    AIL_DOUBLE DisplacementX,         //in
    AIL_DOUBLE DisplacementY,         //in
    AIL_INT64  InterpolationMode      //in
)
```

## Description

This function translates the source image by the specified X- and Y-displacement, writing results to the destination buffer. Alternatively, this function can generate the 3x3 or 3x2 matrix required to perform the transformation using [`MimWarp`](../../Reference/im/MimWarp.md).

This function can be used to align images to subpixel accuracy before, for example, subtracting them.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the source image to translate if performing the transformation. This parameter can be set to one of the following values:

*For specifying the source image buffer identifier*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to ignore this parameter; this parameter must be set to [`M_NULL`](../../Reference/im/MimTranslate.md)when generating the matrix that you can use with [`MimWarp`](../../Reference/im/MimWarp.md) to perform the translation. |
| `Image buffer identifier` | Specifies the identifier of the source image buffer to translate.

This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `DstImageOrArrayBufId` *(out, AIL_ID)*

Specifies the destination image buffer or array buffer, depending on whether performing the transformation. This parameter can be set to one of the following values:

*For specifying the destination buffer identifier*

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the destination [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer when generating the matrix.

When generating a warp matrix, this parameter must be given a single-band, 32-bit floating-point buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md)attribute and that has dimensions 3x2, for a first-order polynomial warping, or 3x3, for a perspective polynomial warping. If you pass this parameter a buffer with dimensions 3x2, [`MimWarp`](../../Reference/im/MimWarp.md)will assume the third row to be (0, 0, 1) when performing the warping. |
| `Image buffer identifier` | Specifies the identifier of the destination image buffer when translating the source image.

This buffer must not have a region of interest (ROI) associated with it. Using a buffer with an ROI will cause an error.

> **Note:** Note that after performing the operation, the destination image will not be calibrated, even if the source image was calibrated. |

### `DisplacementX` *(in, AIL_DOUBLE)*

Specifies the amount, in pixel units, by which to displace the source image in the X-direction. This parameter can be set to any positive or negative value.

### `DisplacementY` *(in, AIL_DOUBLE)*

Specifies the amount, in pixel units, by which to displace the source image in the Y-direction. This parameter can be set to any positive or negative value.

### `InterpolationMode` *(in, AIL_INT64)*

Specifies the interpolation mode when translating the source image. When only generating the matrix, this parameter is ignored and should be set to [`M_DEFAULT`](../../Reference/im/MimTranslate.md).

*For specifying the interpolation mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. When translating the source image, the default is the same as [`M_BILINEAR`](../../Reference/im/MimTranslate.md) + [`M_OVERSCAN_ENABLE`](../../Reference/im/MimTranslate.md).

When only generating the coefficient matrix, this is the only possible value. |
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

Note that when using [`M_OVERSCAN_FAST`](../../Reference/im/MimTranslate.md), the destination pixels in the overscan area are undefined. The pixels can therefore contain different values from one function call to the next, even if the function's parameter values are the same. |
