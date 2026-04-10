---
doctype: Reference
module: im
function: MimClip
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimClip"
---

# MimClip

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

> Perform a point-to-point clipping operation.

## Syntax

```c
void MimClip(
    AIL_ID     SrcImageBufId,  //in
    AIL_ID     DstImageBufId,  //out
    AIL_INT64  Condition,      //in
    AIL_DOUBLE CondLow,        //in
    AIL_DOUBLE CondHigh,       //in
    AIL_DOUBLE WriteLow,       //in
    AIL_DOUBLE WriteHigh       //in
)
```

## Description

This function clips each image pixel that meets the specified condition. If the condition has one clipping point, each pixel that satisfies this condition is replaced with the specified [`WriteLow`](../../Reference/im/MimClip.md) value. If it has two clipping points, the pixels are either replaced with the [`WriteLow`](../../Reference/im/MimClip.md) or [`WriteHigh`](../../Reference/im/MimClip.md) value depending on the condition. Pixels that do not satisfy the condition are not replaced. Instead, they are copied as is to the destination buffer.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image data source. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `Condition` *(in, AIL_INT64)*

Specifies the clipping condition. This parameter must be set to one of the values below.

*For clipping*

| Value | Description |
| --- | --- |
| `M_IN_RANGE` | Replaces pixel values in the range of [`CondLow`](../../Reference/im/MimClip.md) and [`CondHigh`](../../Reference/im/MimClip.md), inclusive, with [`WriteLow`](../../Reference/im/MimClip.md).

||
||
|  |
| *[Image: MimClip_InRange.png]* | CondLow = 100 CondHigh = 150 WriteLow = 0 | |
| `M_OUT_RANGE` | Replaces pixel values less than [`CondLow`](../../Reference/im/MimClip.md) with [`WriteLow`](../../Reference/im/MimClip.md) and those greater than [`CondHigh`](../../Reference/im/MimClip.md) with [`WriteHigh`](../../Reference/im/MimClip.md).

|   |   |
| --- | --- |
| *[Image: MimClip_OutRange.png]* | CondLow = 100 CondHigh = 150 WriteLow = 0 WriteHigh = 0 | |
| `M_SATURATION` | Saturates source values according to the minimum and maximum values of the destination image buffer's data type. |

*For specifying conditions that use one clipping point*

| Value | Description |
| --- | --- |
| `M_EQUAL` | Replaces pixel values equal to [`CondLow`](../../Reference/im/MimClip.md) with [`WriteLow`](../../Reference/im/MimClip.md).

|   |   |
| --- | --- |
| *[Image: MimClip_Equal.png]* | CondLow = 25 WriteLow = 255 | |
| `M_GREATER` | Replaces pixel values greater than [`CondLow`](../../Reference/im/MimClip.md) with [`WriteLow`](../../Reference/im/MimClip.md).

|   |   |
| --- | --- |
| *[Image: MimClip_Greater.png]* | CondLow = 100 WriteLow = 255 | |
| `M_GREATER_OR_EQUAL` | Replaces pixel values greater than or equal to [`CondLow`](../../Reference/im/MimClip.md) with [`WriteLow`](../../Reference/im/MimClip.md). |
| `M_LESS` | Replaces pixel values less than [`CondLow`](../../Reference/im/MimClip.md) with [`WriteLow`](../../Reference/im/MimClip.md).

|   |   |
| --- | --- |
| *[Image: MimClip_Less.png]* | CondLow = 100 WriteLow = 255 | |
| `M_LESS_OR_EQUAL` | Replaces pixel values less than or equal to [`CondLow`](../../Reference/im/MimClip.md) with [`WriteLow`](../../Reference/im/MimClip.md). |
| `M_NON_FINITE` | Replace floating-point pixel values that are non-finite with [`WriteLow`](../../Reference/im/MimClip.md).

[`CondLow`](../../Reference/im/MimClip.md) must be set to [`M_DEFAULT`](../../Reference/im/MimClip.md). The source must be a 32-bit floating-point buffer. |
| `M_NOT_EQUAL` | Replaces pixel values not equal to [`CondLow`](../../Reference/im/MimClip.md) with [`WriteLow`](../../Reference/im/MimClip.md).

|   |   |
| --- | --- |
| *[Image: MimClip_NotEqual.png]* | CondLow = 25 WriteLow = 255 | |

### `CondLow` *(in, AIL_DOUBLE)*

Specifies the lower clipping point of the selected condition. If the condition uses only one limit, use this parameter to specify the required limit.

*For specifying the lower clipping point*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use all non-finite values. |
| `M_NULL` | Specifies that the lower limit is based on the minimum value of the destination image buffer's data type. You must set [`CondLow`](../../Reference/im/MimClip.md) to this value when the condition is set to [`M_SATURATION`](../../Reference/im/MimClip.md). |
| `Value` | Specifies the lower limit. This value is cast to the source buffer's data type and interpreted accordingly.

Note, if the source buffer is binary, [`CondLow`](../../Reference/im/MimClip.md) must be equal to 0 or 1. |

### `CondHigh` *(in, AIL_DOUBLE)*

Specifies the upper clipping point of the selected condition. If the condition uses only one limit, this parameter is ignored and should be set to [`M_NULL`](../../Reference/im/MimClip.md).

*For specifying the upper clipping point*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the upper limit is based on the maximum value of the destination image buffer's data type, or that the condition only uses one limit. You must set [`CondHigh`](../../Reference/im/MimClip.md) to this value when the condition is set to [`M_SATURATION`](../../Reference/im/MimClip.md). |
| `Value` | Specifies the upper limit. This value is cast to the source buffer's data type and interpreted accordingly.

Note, if the source buffer is binary, [`CondHigh`](../../Reference/im/MimClip.md) must be equal to 0 or 1. |

### `WriteLow` *(in, AIL_DOUBLE)*

Specifies the value to write to the destination buffer when a pixel satisfies the specified low clipping condition. If the condition uses only one replacement value, use this parameter to specify the required value.

*For specifying the value to write to the destination buffer when a pixel satisfies the specified low clipping point*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the value to write is the minimum value of the destination image buffer's data type. You must set [`WriteLow`](../../Reference/im/MimClip.md) to this value when the condition is set to [`M_SATURATION`](../../Reference/im/MimClip.md). |
| `Value` | Specifies the value to write when the pixel satisfies the low clipping condition. This value is cast to the destination buffer's data type.

Note, if the destination buffer is binary, [`WriteLow`](../../Reference/im/MimClip.md) must be equal to 0 or 1. |

### `WriteHigh` *(in, AIL_DOUBLE)*

Specifies the value to write to the destination buffer when a pixel satisfies the specified high clipping condition. If the condition uses only one replacement value, this parameter is not used and must be set to [`M_NULL`](../../Reference/im/MimClip.md).

*For specifying the value to write to the destination buffer when a pixel satisfies the specified high clipping point*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the value to write is the maximum value of the destination image buffer's data type, or that the condition only uses the low replacement value. You must set [`WriteHigh`](../../Reference/im/MimClip.md) to this value when the condition is set to [`M_SATURATION`](../../Reference/im/MimClip.md). |
| `Value` | Specifies the value to write when the pixel satisfies the high clipping condition. This value is cast to the destination buffer's data type.

Note, if the destination buffer is binary, [`WriteHigh`](../../Reference/im/MimClip.md) must be equal to 0 or 1. |

## Remarks

> Optimized in-place processing is supported (source equals destination), but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
