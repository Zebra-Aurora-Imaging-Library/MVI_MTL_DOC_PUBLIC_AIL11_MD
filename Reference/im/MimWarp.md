---
doctype: Reference
module: im
function: MimWarp
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimWarp"
---

# MimWarp

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

> Perform a warping.

## Syntax

```c
void MimWarp(
    AIL_ID    SrcImageBufId,     //in
    AIL_ID    DstImageBufId,     //out
    AIL_ID    WarpParam1BufId,   //in
    AIL_ID    WarpParam2BufId,   //in
    AIL_INT64 OperationMode,     //in
    AIL_INT64 InterpolationMode  //in
)
```

## Description

This function warps an image through either a 3x3 matrix-defined warping or LUTs. You can perform first-order polynomial warpings or perspective polynomial warpings using either a 3x3 coefficients matrix or lookup tables (LUTs). You can perform custom warpings using LUTs.

A warping associates each pixel position of the destination buffer, (_x<sub>d</sub>, y<sub>d</sub> _), with a specific point in the source buffer, (_x<sub>s</sub>, y<sub>s</sub> _), and then determines the pixel value of (_x<sub>d</sub>, y<sub>d</sub> _) from its associated point and from a specified interpolation mode.

When performing a first-order polynomial warping, (_x<sub>d</sub>, y<sub>d</sub> _) gets associated with (_x<sub>s</sub>, y<sub>s</sub> _) through the following equations:

`_x_ <sub>s</sub> = a<sub>0</sub> _x_ <sub>d</sub> + a<sub>1</sub> _y_ <sub>d</sub> + a<sub>2</sub>`

`_y_ <sub>s</sub> = b<sub>0</sub> _x_ <sub>d</sub> + b<sub>1</sub> _y_ <sub>d</sub> + b<sub>2</sub>`

When performing a perspective polynomial warping, (_x<sub>d</sub>, y<sub>d</sub> _) gets associated with (_x<sub>s</sub>, y<sub>s</sub> _) through the following equations:

`_x_ <sub>s</sub> = (a<sub>0</sub> _x_ <sub>d</sub> + a<sub>1</sub> _y_ <sub>d</sub> + a<sub>2</sub>)/(c<sub>0</sub> _x_ <sub>d</sub> + c<sub>1</sub> _y_ <sub>d</sub> + c<sub>2</sub>)`

`_y_ <sub>s</sub> = (b<sub>0</sub> _x_ <sub>d</sub> + b<sub>1</sub> _y_ <sub>d</sub> + b<sub>2</sub>)/(c<sub>0</sub> _x_ <sub>d</sub> + c<sub>1</sub> _y_ <sub>d</sub> + c<sub>2</sub>)`

When performing a 3x3 matrix-defined warping, [`WarpParam1BufId`](../../Reference/im/MimWarp.md) specifies the required coefficients (_a<sub>0</sub>...a<sub>2</sub>, b<sub>0</sub>...b<sub>2</sub> _) or (_a<sub>0</sub>...a<sub>2</sub>, b<sub>0</sub>...b<sub>2</sub>, c<sub>0</sub>...c<sub>2</sub> _) and [`WarpParam2BufId`](../../Reference/im/MimWarp.md) must be set to [`M_NULL`](../../Reference/im/MimWarp.md). The coefficients can be automatically generated using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md) or can be user-supplied.

When performing a warping using LUTs, _x<sub>s</sub> _is determined from (_x<sub>d</sub>, y<sub>d</sub> _) through one LUT and _y<sub>s</sub> _ is determined from (_x<sub>d</sub>, y<sub>d</sub> _) through another LUT. In this case, [`WarpParam1BufId`](../../Reference/im/MimWarp.md) specifies the LUT for _x<sub>s</sub> _ and [`WarpParam2BufId`](../../Reference/im/MimWarp.md) specifies the LUT for _y<sub>s</sub> _. The LUTs can be user-supplied or can be automatically generated from a 3x3 coefficient matrix (for a first-order or polynomial warping) using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the buffer on which to perform the warping. This buffer can be of any type.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the buffer in which to place the results of the warping. This buffer can be of any type.

### `WarpParam1BufId` *(in, AIL_ID)*

Specifies the buffer containing the matrix coefficients or the LUT buffer from which `_x_ <sub>s</sub>` is determined.

### `WarpParam2BufId` *(in, AIL_ID)*

Specifies the LUT buffer from which `_y_ <sub>s</sub>` is determined. This buffer must be a signed 16- or 32-bit integer buffer or a 32-bit floating-point buffer, have the same X and Y size as the destination image buffer, and have an [`M_LUT`](../../Reference/buf/MbufAlloc2d.md) attribute.

### `OperationMode` *(in, AIL_INT64)*

Specifies the mode of operation. This parameter must be set to one of the values below.

*For specifying the mode of operation*

| Value | Description |
| --- | --- |
| `M_WARP_LUT` | Performs the warping through LUTs. |
| `M_WARP_POLYNOMIAL` | Performs the warping through a 3x3 coefficient matrix. This includes a first-order polynomial warping or a perspective polynomial warping.

Note that with a perspective polynomial warping matrix, [`MimWarp`](../../Reference/im/MimWarp.md) might return an error if the calculation of _(c<sub>0</sub>x<sub>d</sub> + c<sub>1</sub>y<sub>d</sub> + c<sub>2</sub>)_is 0 in the destination buffer, regardless of whether or not the matrix was generated using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md). |

*For*

| Value | Description |
| --- | --- |
| `M_FIXED_POINT + n` | Specifies the number of fractional bits for the source point (`_x_ <sub>s</sub>`, `_y_ <sub>s</sub>`). To do so, add the define [`M_FIXED_POINT + n`](../../Reference/im/MimWarp.md) to [`M_WARP_LUT`](../../Reference/im/MimWarp.md) where `_n_ >= 0`.

If nothing is added to [`M_WARP_LUT`](../../Reference/im/MimWarp.md), it is assumed that there are no fractional bits in the coordinates of the source point (and therefore interpolation is not required). |

### `InterpolationMode` *(in, AIL_INT64)*

Specifies the interpolation mode. This parameter must be set to one of the values below.

*For specifying the interpolation mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_NEAREST_NEIGHBOR`](../../Reference/im/MimWarp.md) + [`M_OVERSCAN_ENABLE`](../../Reference/im/MimWarp.md). |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated according to the depth and data type of the destination buffer. |
| `M_BILINEAR` | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

*For overscan*

| Value | Description |
| --- | --- |
| `M_OVERSCAN_CLEAR` | Sets the destination pixel to 0, if the associated point falls outside the source buffer. |
| `M_OVERSCAN_DISABLE` | Leaves the destination pixel as is. |
| `M_OVERSCAN_ENABLE` *(default)* | Uses pixels from the source buffer's ancestor buffer. If the source buffer is not a child buffer or if the point falls outside the ancestor buffer, leave the destination pixel as is. |
| `M_OVERSCAN_FAST` | Specifies that Aurora Imaging Library will automatically select the overscan that optimizes speed, according to the specified operation and the target system. The overscan could be hardware-specific thereby having a different behavior than the other supported overscan modes.

Note that when using [`M_OVERSCAN_FAST`](../../Reference/im/MimWarp.md), the destination pixels in the overscan area are undefined. The pixels can therefore contain different values from one function call to the next, even if the function's parameter values are the same. |
