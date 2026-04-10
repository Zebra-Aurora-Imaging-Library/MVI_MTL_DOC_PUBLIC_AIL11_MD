---
doctype: Reference
module: buf
function: MbufCopyColor2d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufCopyColor2d"
---

# MbufCopyColor2d

> Copy a two-dimensional region of one or all bands of an image buffer to another buffer.

## Syntax

```c
void MbufCopyColor2d(
    AIL_ID  SrcBufId,  //in
    AIL_ID  DstBufId,  //out
    AIL_INT SrcBand,   //in
    AIL_INT SrcOffX,   //in
    AIL_INT SrcOffY,   //in
    AIL_INT DstBand,   //in
    AIL_INT DstOffX,   //in
    AIL_INT DstOffY,   //in
    AIL_INT SizeX,     //in
    AIL_INT SizeY      //in
)
```

## Description

This function copies a two-dimensional region of one or all bands of the specified source buffer to the specified band(s) of the destination buffer. It can also be used to retrieve or replace a color band from a color buffer.

If the source is a monochrome buffer and the destination is a multi-band buffer, the unique source buffer band is inserted into the specified band of the destination buffer. If the source is a multi-band buffer and the destination is a monochrome buffer, the specified source buffer band is extracted from the source buffer and written to the destination buffer. If both are multi-band buffers, the specified band(s) is copied from the source to the destination.

If the source buffer depth is greater than that of the destination, the most significant bits are truncated when the data is copied to the destination. If the destination depth is greater than that of the source, the source data is zero or sign-extended (depending on the type of the source) when copied to the destination. Also, the buffers must have the same number of bands if all bands are to be copied.

Note that when copying from a non-binary buffer to a binary destination buffer, all non-zero pixels in the source buffer are represented as ones (1) in the binary destination buffer. When copying a binary buffer to a buffer of a different depth, each bit is copied into the least-significant bit of a different destination pixel. The remaining bits of the destination pixel are set to 0; to propagate the bit value to all bits, use [`MimBinarize`](../../Reference/im/MimBinarize.md).

Note, if the source is a 1-band image buffer associated with a LUT buffer and the destination is a 3-band image buffer, the source data copied to destination is first mapped through the LUT.

Note that when copying into the U or V band of a YUV destination buffer, the dimensions of the U or V band of the destination buffer must not be smaller than the dimensions of the source buffer.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source data buffer.

### `DstBufId` *(out, AIL_ID)*

Specifies the identifier of the destination data buffer.

### `SrcBand` *(in, AIL_INT)*

Specifies the source band from which to copy. [`SrcBand`](../../Reference/buf/MbufCopyColor2d.md) can be set to one of the following values:

*For the source band*

| Value | Description |
| --- | --- |
| `M_ALL_BANDS` | Specifies all bands (for multi-band buffers). |
| `M_BLUE` | Specifies the blue color band (for RGB buffers). |
| `M_GREEN` | Specifies the green color band (for RGB buffers). |
| `M_HUE` | Specifies the hue band (for HSL and HSV buffers). |
| `M_LUMINANCE` | Specifies the luminance band (for HSL buffers). |
| `M_RED` | Specifies the red color band (for RGB buffers). |
| `M_SATURATION` | Specifies the saturation band (for HSL and HSV buffers). |
| `M_U` | Specifies the U band (for YUV buffers). |
| `M_V` | Specifies the V band (for YUV and HSV buffers). |
| `M_Y` | Specifies the Y band (for YUV buffers). |
| `0 <= Value < #bands` | Specifies the index of the band to copy.

The relationship between index value and band for RGB, HSL, HSV, and YUV buffers is the following:

|   |   |
| --- | --- |
| 0 | Corresponds to the red band for RGB buffers, the hue band for HSL and HSV buffers, or the Y band for YUV buffers. |
| 1 | Corresponds to the green band for RGB buffers, or the saturation band for HSL and HSV buffers, or the U band for YUV buffers. |
| 2 | Corresponds to the blue band for RGB buffers, or the luminance band for HSL buffers, or the V band for YUV and HSV buffers. | |

### `SrcOffX` *(in, AIL_INT)*

Specifies the horizontal pixel offset of the region to read relative to the source buffer starting coordinate. The offset must be within the width of the source buffer.

### `SrcOffY` *(in, AIL_INT)*

Specifies the vertical pixel offset of the region to read relative to the source buffer starting coordinate. The offset must be within the height of the source buffer.

### `DstBand` *(in, AIL_INT)*

Specifies the destination band in which to copy.

### `DstOffX` *(in, AIL_INT)*

Specifies the horizontal pixel offset of the region to write relative to the destination buffer starting coordinate. The offset must be within the width of the destination buffer.

### `DstOffY` *(in, AIL_INT)*

Specifies the vertical pixel offset of the region to write relative to the destination buffer starting coordinate. The offset must be within the height of the destination buffer.

### `SizeX` *(in, AIL_INT)*

Specifies the width of the region to be copied, starting from the specified offset ([`SrcOffX`](../../Reference/buf/MbufCopyColor2d.md), [`DstOffX`](../../Reference/buf/MbufCopyColor2d.md)).

### `SizeY` *(in, AIL_INT)*

Specifies the height of the region to be copied, starting from the specified offset ([`SrcOffY`](../../Reference/buf/MbufCopyColor2d.md), [`DstOffY`](../../Reference/buf/MbufCopyColor2d.md)).
