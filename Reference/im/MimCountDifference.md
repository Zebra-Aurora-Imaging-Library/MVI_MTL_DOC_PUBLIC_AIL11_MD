---
doctype: Reference
module: im
function: MimCountDifference
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimCountDifference"
---

# MimCountDifference

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

> Count the number of pixels that differ in each image.

## Syntax

```c
AIL_INT MimCountDifference(
    AIL_ID Src1ImageBufId,  //in
    AIL_ID Src2ImageBufId,  //in
    AIL_ID CountResultImId  //out
)
```

## Description

This function finds the number of differences between the two specified source buffers and stores the resulting number in the specified result buffer.

You can read the number of differences from the result buffer, using [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) or [`MimGetResult`](../../Reference/im/MimGetResult.md), specifying [`M_VALUE`](../../Reference/im/MimGetResult.md) as the result type.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `Src1ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the first image data source. This parameter can be given an image buffer identifier. The image buffer must be 1-band. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `Src2ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the second image data source. This parameter can only be given an image buffer identifier. The image buffer must be 1-band. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `CountResultImId` *(out, AIL_ID)*

Specifies the identifier of the buffer in which to store the differences. This parameter must be given the identifier of an image processing result buffer that was allocated with [`MimAllocResult`](../../Reference/im/MimAllocResult.md) and has an [`M_COUNT_LIST`](../../Reference/im/MimAllocResult.md) type. The buffer needs only one entry.

## Return Value

**Type:** `AIL_INT`

If [`CountResultImId`](../../Reference/im/MimCountDifference.md) is `M_NULL`, the returned value is the number of differences between the two specified source buffers. If [`CountResultImId`](../../Reference/im/MimCountDifference.md) is not `M_NULL`, the returned value is `M_INVALID`. In that case, use either [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) or [`MimGetResult`](../../Reference/im/MimGetResult.md), with [`M_VALUE`](../../Reference/im/MimGetResult.md) as the result type, to retrieve the number of differences.
