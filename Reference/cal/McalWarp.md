---
doctype: Reference
module: cal
function: McalWarp
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalWarp"
---

# McalWarp

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

> Define the mapping characteristics of a camera calibration context according to an existing camera calibration context.

## Syntax

```c
void McalWarp(
    AIL_ID     SrcImageOrContextCalId,  //in
    AIL_ID     DstImageOrContextCalId,  //out
    AIL_ID     WarpParam1Id,            //in
    AIL_ID     WarpParam2Id,            //in
    AIL_DOUBLE OffsetX,                 //in
    AIL_DOUBLE OffsetY,                 //in
    AIL_DOUBLE SizeX,                   //in
    AIL_DOUBLE SizeY,                   //in
    AIL_INT    RowNumber,               //in
    AIL_INT    ColumnNumber,            //in
    AIL_INT64  TransformationType,      //in
    AIL_INT64  ControlFlag              //in
)
```

## Description

This function defines the mapping of a camera calibration context according to a previously configured camera calibration context. Note that the camera calibration mode of the destination camera calibration context need not necessarily be the same as the camera calibration mode of the source context. The resulting mapping will be an accurate approximation of the source mapping for the specified image or context. This can be used, for example, to approximate a complex camera calibration using a constant pixel size camera calibration, simplifying further calculations.

This function can also be used to generate a new context by warping a previously calibrated context. The resulting context can then be associated with the uncalibrated warped image. This can be used, for example, to calibrate an image that has been warped using [`MimWarp`](../../Reference/im/MimWarp.md). [`MimWarp`](../../Reference/im/MimWarp.md)returns an uncalibrated image which can then be associated (using [`McalAssociate`](../../Reference/cal/McalAssociate.md)) with the newly warped camera calibration context.

To define new mapping that will only apply to a specific region of an image, use the [`OffsetX`](../../Reference/cal/McalWarp.md), [`OffsetY`](../../Reference/cal/McalWarp.md), [`SizeX`](../../Reference/cal/McalWarp.md) and [`SizeY`](../../Reference/cal/McalWarp.md) parameters. The mapping of the new camera calibration context will apply only to this defined area. See the description of these parameters in the Parameters section located below for more information.

## Parameters

### `SrcImageOrContextCalId` *(in, AIL_ID)*

Specifies the identifier of the camera calibration context, calibrated image, or corrected image From which the source mapping is taken.

### `DstImageOrContextCalId` *(out, AIL_ID)*

Specifies the identifier of the destination camera calibration context or image buffer.

### `WarpParam1Id` *(in, AIL_ID)*

Specifies the buffer containing the matrix of coefficients or the LUT buffer from which source X-coordinates are determined.

### `WarpParam2Id` *(in, AIL_ID)*

Specifies the LUT buffer from which source Y-coordinates are determined.

### `OffsetX` *(in, AIL_DOUBLE)*

Specifies the X pixel coordinate of the top-left calibration point.

### `OffsetY` *(in, AIL_DOUBLE)*

Specifies the Y pixel coordinate of the top-left calibration point.

### `SizeX` *(in, AIL_DOUBLE)*

Specifies the width of the grid of calibration points, in pixels.

### `SizeY` *(in, AIL_DOUBLE)*

Specifies the height of the grid of calibration points, in pixels.

### `RowNumber` *(in, AIL_INT)*

Specifies the number of rows of calibration points to be used for camera calibration.

*For specifying the number of rows:*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 2` *(default)* | Specifies the number of rows of calibration points to be used for camera calibration. |

### `ColumnNumber` *(in, AIL_INT)*

Specifies the number of columns of calibration points to be used for camera calibration.

*For specifying the number of columns:*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 2` *(default)* | Specifies the number of columns of calibration points to be used for camera calibration. |

### `TransformationType` *(in, AIL_INT64)*

Specifies the type of transformation. This parameter must be set to one of the following values.

*For specifying the type of transformation:*

| Value | Description |
| --- | --- |
| `M_IDENTITY` | No transformation is performed. This is used when [`McalWarp`](../../Reference/cal/McalWarp.md) approximates a new camera calibration context based on a source camera calibration context. In such cases, [`WarpParam1Id`](../../Reference/cal/McalWarp.md) and [`WarpParam2Id`](../../Reference/cal/McalWarp.md) must be set to [`M_NULL`](../../Reference/cal/McalWarp.md). |
| `M_WARP_LUT` | Performs the warping through LUTs. In such cases, [`WarpParam1Id`](../../Reference/cal/McalWarp.md) and [`WarpParam2Id`](../../Reference/cal/McalWarp.md) must be buffers allocated with the [`M_LUT`](../../Reference/buf/MbufAlloc2d.md) attribute. |
| `M_WARP_POLYNOMIAL` | Performs the warping using a 3x3 coefficient matrix. This includes first-order polynomial warping or perspective polynomial warping. In such cases, [`WarpParam1Id`](../../Reference/cal/McalWarp.md) must indicate a buffer having the [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) attribute and [`WarpParam2Id`](../../Reference/cal/McalWarp.md) must be set to[`M_NULL`](../../Reference/cal/McalWarp.md). |

*For specifying fractional bits*

| Value | Description |
| --- | --- |
| `M_FIXED_POINT + n` | Specifies the number of fractional bits for source coordinates. To do so, add the define [`M_FIXED_POINT + n`](../../Reference/cal/McalWarp.md) to [`M_WARP_LUT`](../../Reference/cal/McalWarp.md) where _n_ >= 0. If nothing is added to [`M_WARP_LUT`](../../Reference/cal/McalWarp.md), it is assumed that there are no fractional bits in the coordinates of the source point. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to`M_DEFAULT`.
