---
doctype: Reference
module: buf
function: MbufCopyColor
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufCopyColor"
---

# MbufCopyColor

> Copy one or all bands of an image buffer.

## Syntax

```c
void MbufCopyColor(
    AIL_ID  SrcBufId,   //in
    AIL_ID  DestBufId,  //out
    AIL_INT Band        //in
)
```

## Description

This function copies one or all bands of the specified source buffer to the specified destination buffer. It can also be used to retrieve or replace a color band from a color image.

If the source is a monochrome buffer and the destination is a multi-band buffer, the source buffer band is copied into the specified band of the destination buffer. If the source is a multi-band buffer and the destination is a monochrome buffer, the specified source buffer band is copied from the source buffer and written to the destination buffer. If both are multi-band buffers, the specified band(s) is copied from the source to the destination.

If the source buffer depth is greater than that of the destination, the most significant bits are truncated when the data is copied to the destination. If the destination depth is greater than that of the source, the source data is zero or sign-extended (depending on the type of the source) when copied to the destination. Also, the buffers must have the same number of bands if all bands are to be copied.

Note, when copying from a non-binary buffer to a binary buffer, all non-zero pixels in the source buffer are represented as ones (1) in the binary buffer. When copying a binary buffer to a buffer of a different depth, each bit is copied into the least-significant bit of a different destination pixel. The remaining bits of the destination pixel are set to 0; to propagate the bit value to all bits, use [`MimBinarize`](../../Reference/im/MimBinarize.md).

Note, if the source is a 1-band image buffer associated with a LUT buffer and the destination is a 3-band image buffer, the source data copied to destination is first mapped through the LUT.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source data buffer.

### `DestBufId` *(out, AIL_ID)*

Specifies the identifier of the destination data buffer.

### `Band` *(in, AIL_INT)*

Specifies the band to copy and/or in which to store the copied data. Set this parameter to one of the following values:

*For specifying the band*

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
