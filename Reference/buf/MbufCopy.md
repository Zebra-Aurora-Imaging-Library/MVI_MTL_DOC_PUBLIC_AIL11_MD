---
doctype: Reference
module: buf
function: MbufCopy
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufCopy"
---

# MbufCopy

> Copy data from one buffer to another.

## Syntax

```c
void MbufCopy(
    AIL_ID SrcBufId,  //in
    AIL_ID DestBufId  //out
)
```

## Description

This function copies the specified source buffer data to the specified destination buffer. If the source and destination buffers are of different data types, Aurora Imaging Library converts the data automatically.

If the source buffer depth is greater than that of the destination, the most significant bits are truncated when the data is copied into the destination. If the destination depth is greater than that of the source, the source data is zero or sign-extended (depending on the type of the source) when copied into the destination. If the destination is larger in size than the source, exceeding areas of the buffer are unaffected.

Note, when copying from a non-binary buffer to a binary buffer, all non-zero pixels in the source buffer are represented as ones (1) in the binary buffer. When copying a binary buffer to a buffer of a different depth, each bit is copied into the least-significant bit of a different destination pixel. The remaining bits of the destination pixel are set to 0; to propagate the bit value to all bits, use [`MimBinarize`](../../Reference/im/MimBinarize.md).

When copying from a floating-point buffer to an integer buffer, the values are truncated.

If the source buffer is a 3-band YUV buffer and the destination buffer is a 1-band buffer, only the Y band (luminance) is copied. If the source buffer is a 3-band RGB buffer and the destination buffer is a 1-band buffer, only the red band is copied.

Note, if the source is a 1-band image buffer associated with a LUT buffer and the destination is a 3-band image buffer, the source data copied to destination is first mapped through the LUT.

Note, if the source and destination are both multi-band buffers, they must have the same number of bands.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source data buffer.

### `DestBufId` *(out, AIL_ID)*

Specifies the identifier of the destination data buffer.
