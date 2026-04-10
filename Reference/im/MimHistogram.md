---
doctype: Reference
module: im
function: MimHistogram
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimHistogram"
---

# MimHistogram

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

> Generate the intensity histogram of an image.

## Syntax

```c
void MimHistogram(
    AIL_ID SrcImageBufId,  //in
    AIL_ID HistResultImId  //out
)
```

## Description

This function calculates the histogram (or pixel intensity distribution) of the specified source image and stores the results in the specified result buffer.

You can control the bin size of the calculated histogram, and the number of smoothing iterations to perform on it, by calling [`MimControl`](../../Reference/im/MimControl.md) with [`M_HIST_BIN_SIZE_MODE`](../../Reference/im/MimControl.md) and [`M_HIST_SMOOTHING_ITERATIONS`](../../Reference/im/MimControl.md). By default, a histogram has 1 value per bin, and has not been smoothed.

To read the resulting histogram values from the result buffer, use [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) or [`MimGetResult`](../../Reference/im/MimGetResult.md), and specify [`M_CUMULATIVE_VALUE`](../../Reference/im/MimGetResult.md), [`M_HIST_...`](../../Reference/im/MimGetResult.md), [`M_PERCENTILE_VALUE`](../../Reference/im/MimGetResult.md), or [`M_VALUE`](../../Reference/im/MimGetResult.md) as the result type.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The image buffer must be 1-band.

### `HistResultImId` *(out, AIL_ID)*

Specifies the identifier of the destination for the histogram results. This parameter must be given the identifier of an image processing result buffer that was allocated with [`MimAllocResult`](../../Reference/im/MimAllocResult.md) and has an [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md) type.
