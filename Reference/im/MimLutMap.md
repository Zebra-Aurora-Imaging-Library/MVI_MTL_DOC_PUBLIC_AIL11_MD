---
doctype: Reference
module: im
function: MimLutMap
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimLutMap"
---

# MimLutMap

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

> Perform a point-to-point LUT mapping operation.

## Syntax

```c
void MimLutMap(
    AIL_ID SrcImageBufId,  //in
    AIL_ID DstImageBufId,  //out
    AIL_ID LutBufId        //in
)
```

## Description

This function maps each pixel in the specified source image to values determined by the specified lookup table (LUT).

If the source image is signed, negative numbers are treated as unsigned values and address the LUT accordingly. For example, -1 is represented as 0xff for 8-bit image data and 0xffff for 16-bit image data and will therefore address LUT entry 0xff or 0xffff, respectively.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter must be given an image buffer identifier. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the results. This parameter must be given an image buffer identifier. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `LutBufId` *(in, AIL_ID)*

Specifies the LUT through which to map source input values. This parameter must be given a valid LUT buffer identifier.

## Remarks

> In-place processing is supported, but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
