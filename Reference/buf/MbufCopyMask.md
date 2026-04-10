---
doctype: Reference
module: buf
function: MbufCopyMask
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufCopyMask"
---

# MbufCopyMask

> Copy buffer with mask.

## Syntax

```c
void MbufCopyMask(
    AIL_ID    SrcBufId,   //in
    AIL_ID    DestBufId,  //out
    AIL_INT64 MaskValue   //in
)
```

## Description

This function copies the specified source buffer data to the specified destination buffer, modifying only the bits of the destination that have a non-zero corresponding bit in the mask.

Note, if the source and destination are both multi-band buffers, they must have the same number of bands.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source data buffer. Note that the source data buffer cannot be of type floating-point.

### `DestBufId` *(out, AIL_ID)*

Specifies the identifier of the destination data buffer. Note that the destination data buffer cannot be of type floating-point.

### `MaskValue` *(in, AIL_INT64)*

Specifies the mask value. Even though this value is of type _AIL_INT64_, it is treated as if it had the same depth as the destination buffer; the most-significant bits that are not required are ignored.
