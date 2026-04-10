---
doctype: Reference
module: im
function: MimHistogramEqualizeAdaptive
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimHistogramEqualizeAdaptive"
---

# MimHistogramEqualizeAdaptive

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

> Perform an adaptive histogram equalization on an image.

## Syntax

```c
void MimHistogramEqualizeAdaptive(
    AIL_ID    AdaptiveEqualizeContextImId,  //in
    AIL_ID    SrcImageBufId,                //in
    AIL_ID    DstImageBufId,                //out
    AIL_INT64 ControlFlag                   //in
)
```

## Description

This function enhances the contrast of the specified source image by applying a Contrast Limited Adaptive Histogram Equalization (CLAHE). The resulting image is placed in the destination image buffer.

This function first partitions the specified source image into a set of rectangular sections of equal size, referred to as tiles, and calculates histograms for each one. The histograms are then used to transform the image and produce an enhanced (equalized) version of it. By default, [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) uses 8 tiles along the image's X- and Y-direction. To modify the number of tiles, call [`MimControl`](../../Reference/im/MimControl.md) with [`M_NUMBER_OF_TILES_X`](../../Reference/im/MimControl.md) and [`M_NUMBER_OF_TILES_Y`](../../Reference/im/MimControl.md). Though [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) can take longer to execute than a conventional histogram equalization ([`MimHistogramEqualize`](../../Reference/im/MimHistogramEqualize.md)), it can help minimize the over-amplification of noise and the potential for saturation, particularly in images with inconsistent lighting where some sections are significantly brighter than others.

By default, the maximum percentage of pixels that can be represented by a histogram bin is limited to 1% of all the pixels in its respective tile. Exceeding values are distributed evenly among that histogram's other bins. To modify this behavior, use the [`M_CLIP_LIMIT`](../../Reference/im/MimControl.md) control.

You can also modify the number of histogram bins for each tile, as well as the equalization operation performed, using the [`M_HIST_SIZE`](../../Reference/im/MimControl.md) and [`M_OPERATION`](../../Reference/im/MimControl.md) controls. By default, [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) determines the number of histogram bins automatically according to the specified source image and performs a uniform-type of equalization.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `AdaptiveEqualizeContextImId` *(in, AIL_ID)*

Specifies the identifier of the adaptive histogram equalization context. The context must have been allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_HISTOGRAM_EQUALIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md).

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The buffer must be a 1-band, 8- or 16-bit image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_PROC`](../../Reference/buf/MbufAlloc1d.md). The type of the source image buffer must be the same as the destination image buffer. To inquire this information, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_TYPE`](../../Reference/buf/MbufInquire.md).

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer. The buffer must be a 1-band, 8- or 16-bit image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_PROC`](../../Reference/buf/MbufAlloc1d.md). The type of the destination image buffer must be the same as the source image buffer. To inquire this information, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_TYPE`](../../Reference/buf/MbufInquire.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
