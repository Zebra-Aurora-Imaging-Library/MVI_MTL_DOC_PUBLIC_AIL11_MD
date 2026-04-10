---
doctype: Reference
module: im
function: MimProjection
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimProjection"
---

# MimProjection

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

> Project a 2D image into 1D.

## Syntax

```c
void MimProjection(
    AIL_ID     SrcImageBufId,            //in
    AIL_ID     DstImageBufOrResultImId,  //out
    AIL_DOUBLE ProjectionAxisAngle,      //in
    AIL_INT64  Operation,                //in
    AIL_DOUBLE OperationValue            //in
)
```

## Description

This function projects a two-dimensional image into one dimension, based on the specified projection operation and projection axis angle, writing results to the specified projection image processing result buffer or a destination image buffer. The results are generated based on the specified operation and calculated along each lane of pixels in the image, perpendicular to the specified projection axis angle. The 90° projection of the image (lanes = rows) using a summation operation is known as the row profile; the 0° projection (lanes = columns) as the column profile.

Writing results to a projection image processing result buffer allows you to manipulate results using an array once you have retrieved the results using [`MimGetResult`](../../Reference/im/MimGetResult.md)or [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md).

You can limit this function's results to a region of the source image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The image buffer must be a 1-band buffer.

### `DstImageBufOrResultImId` *(out, AIL_ID)*

Specifies the destination buffer in which to write the projection results.

*For specifying the projection result buffer identifier or a destination image buffer*

| Value | Description |
| --- | --- |
| `Destination image buffer ID` | Specifies the identifier of an image buffer allocated with [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The destination image buffer must have the same data type (unsigned, signed, or float) as the source buffer. For an [`M_SUM`](../../Reference/im/MimProjection.md) operation, this image buffer must be of 32-bit depth. For an [`M_MAX`](../../Reference/im/MimProjection.md), [`M_MEAN`](../../Reference/im/MimProjection.md), [`M_MEDIAN`](../../Reference/im/MimProjection.md), [`M_MIN`](../../Reference/im/MimProjection.md), [`M_RANK`](../../Reference/im/MimProjection.md), or [`M_RANK_PERCENTILE`](../../Reference/im/MimProjection.md)operation, this image buffer can have a depth greater than or equal to that of the source image buffer. |
| `Destination result buffer ID` | Specifies the identifier of an image processing result buffer allocated using[`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md). The buffer should have as many locations as there are lanes to project in the image perpendicular to the specified projection axis angle.

You can read the projection values from the result buffer using [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) or [`MimGetResult`](../../Reference/im/MimGetResult.md)with [`M_VALUE`](../../Reference/im/MimGetResult.md). |

### `ProjectionAxisAngle` *(in, AIL_DOUBLE)*

Specifies the angle of the projection axis. Lanes are perpendicular to this angle. This parameter can be set to one of the following:

*For specifying the angle of projection in degrees*

| Value | Description |
| --- | --- |
| `M_0_DEGREE` | Specifies a projection onto the axis at 0° (lanes = columns). |
| `M_90_DEGREE` | Specifies a projection onto the axis at 90° (lanes = rows). |

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

*For specifying the type of operation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX` | Specifies to find the maximum pixel value along each lane. |
| `M_MEAN` | Specifies to find the mean pixel value along each lane. |
| `M_MEDIAN` | Specifies to find the median pixel value along each lane. |
| `M_MIN` | Specifies to find the minimum pixel value along each lane. |
| `M_RANK` | Specifies to find the pixel value with the specified rank ([`OperationValue`](../../Reference/im/MimProjection.md)) along each lane. The pixels in a lane are ranked in ascending order based on pixel value.

The rank cannot be larger than the lane size of the image. This can be inquired using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_SIZE_X`](../../Reference/buf/MbufInquire.md) or [`M_SIZE_Y`](../../Reference/buf/MbufInquire.md), depending on the projection axis. |
| `M_RANK_PERCENTILE` | Specifies to find the pixel value with the specified rank ([`OperationValue`](../../Reference/im/MimProjection.md)) along each lane; the rank is specified as a percentage of the lane length. The pixels in a lane are ranked in ascending order based on pixel value. |
| `M_SUM` *(default)* | Specifies to sum all pixel values along each lane.

Note that, if the sum of pixels in any lane is larger than the depth of the result buffer or destination image buffer, the result will be undefined. |

### `OperationValue` *(in, AIL_DOUBLE)*

Specifies the rank for [`M_RANK`](../../Reference/im/MimProjection.md), and the percentile for [`M_RANK_PERCENTILE`](../../Reference/im/MimProjection.md). For other operations, this parameter must be set to `M_NULL`.
