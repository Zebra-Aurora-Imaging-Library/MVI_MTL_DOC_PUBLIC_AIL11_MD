---
doctype: Reference
module: im
function: MimStatCalculate
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimStatCalculate"
---

# MimStatCalculate

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

> Calculates and stores a variety of statistics from the specified source image(s).

## Syntax

```c
void MimStatCalculate(
    AIL_ID    StatContextImId,  //in
    AIL_ID    SrcImageBufId,    //in
    AIL_ID    StatResultImId,   //out
    AIL_INT64 ControlFlag       //in
)
```

## Description

This function calculates a variety of statistics for the pixels in the source image(s). Results are stored in the specified statistics result buffer.

The specified image processing context establishes which statistics to calculate. This function can take statistics for the specified image ([`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md) context) or it can accumulate statistics for each pixel across multiple images ([`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md) context). For an [`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md) context, you can specify a condition that a pixel must satisfy to be included in the calculation using [`M_CONDITION`](../../Reference/im/MimControl.md). Co-occurrence statistics ([`M_STAT_GLCM...`](../../Reference/im/MimControl.md)) ignore any specified condition. If you need to calculate one type of single-image statistic and don't need to specify a condition, you can use a predefined context; otherwise, use [`MimAlloc`](../../Reference/im/MimAlloc.md) to set up a custom context that you can control using [`MimControl`](../../Reference/im/MimControl.md).

You can read the statistics from the result buffer using [`MimGetResult`](../../Reference/im/MimGetResult.md) or [`MimGetResult2d`](../../Reference/im/MimGetResult2d.md), depending on the statistical operation performed. You can also draw some statistical results, using[`MimDraw`](../../Reference/im/MimDraw.md).

Once the statistics image processing context is configured, preprocess the context and result buffer by calling [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) with [`M_PREPROCESS`](../../Reference/im/MimStatCalculate.md). If the preprocess operation is not done explicitly (using [`M_PREPROCESS`](../../Reference/im/MimStatCalculate.md)), it will be done when [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) is first called. When explicitly preprocessing a cumulative statistics image processing context and result buffer, the source image buffer can be set to [`M_NULL`](../../Reference/im/MimStatCalculate.md) if you set the size that the source buffer will be, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_SOURCE_SIZE_X`](../../Reference/im/MimControl.md) and [`M_SOURCE_SIZE_Y`](../../Reference/im/MimControl.md). Note that providing the source image buffer better optimizes future calls.

When working with an [`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md) context, you can remove the calculated statistics associated with a specified source image from the result buffer by calling [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) with [`M_REMOVE`](../../Reference/im/MimStatCalculate.md). To reset the results generated from finding the extremes of each pixel's value (such as, [`M_STAT_MIN`](../../Reference/im/MimControl.md)), use [`M_RESET_EXTREMES`](../../Reference/im/MimStatCalculate.md).

To reset all the results in the result buffer, free it using [`MimFree`](../../Reference/im/MimFree.md), and then allocate a new result buffer using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md). These actions apply to either context ([`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md) or [`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md)).

Note that when working with an [`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md) context, your source images should have the same dimensions, so that statistics are calculated for the same pixel across the multiple images.

For an [`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md), you can limit some of this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). Not all statistical operations support ROIs; for a list of those that do, see [`MimControl`](../../Reference/im/MimControl.md). Note that the [`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md) context does not support ROIs.

## Parameters

### `StatContextImId` *(in, AIL_ID)*

Specifies a statistics context, which determines the statistics to calculate. You can select the identifier of a previously allocated statistics context, or you can select a predefined statistics context to evaluate one type of single-image statistical calculation. The predefined statistics contexts do not need to be allocated.

*For specifying the statistics context, or specific statistic to calculate*

| Value | Description |
| --- | --- |
| `M_STAT_CONTEXT_MAX` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_MAX`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the maximum pixel value of the source image. |
| `M_STAT_CONTEXT_MAX_ABS` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_MAX_ABS`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the maximum absolute pixel value of the source image. |
| `M_STAT_CONTEXT_MEAN` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_MEAN`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the mean pixel value of the source image. |
| `M_STAT_CONTEXT_MIN` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_MIN`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the minimum pixel value of the source image. |
| `M_STAT_CONTEXT_MIN_ABS` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_MIN_ABS`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the minimum absolute pixel value of the source image. |
| `M_STAT_CONTEXT_NUMBER` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_NUMBER`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the number of valid pixels in the source image. This is mainly useful in the presence of a region. |
| `M_STAT_CONTEXT_STANDARD_DEVIATION` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_STANDARD_DEVIATION`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the standard deviation of the pixel values of the source image. |
| `M_STAT_CONTEXT_SUM` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_SUM`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the sum of the pixel values of the source image. |
| `M_STAT_CONTEXT_SUM_ABS` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_SUM_ABS`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the sum of the absolute pixel values of the source image. |
| `M_STAT_CONTEXT_SUM_OF_SQUARES` | Specifies a predefined single-image statistics image processing context with all control types set to their default except for [`M_STAT_SUM_OF_SQUARES`](../../Reference/im/MimControl.md), which is set to [`M_ENABLE`](../../Reference/im/MimControl.md).

Use this predefined context to calculate the sum of the squared pixel values of the source image. |
| `Statistics context identifier` | Specifies a statistics context identifier, previously allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md) or [`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md).

You can specify which statistics calculations are evaluated using [`MimControl`](../../Reference/im/MimControl.md). |

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The buffer used must be a 1-band image buffer.

### `StatResultImId` *(out, AIL_ID)*

Specifies the identifier of the result buffer. The buffer must have been allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies the operation to perform on the source image.

*To specify the operation to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Calculates the statistics specified using [`MimControl`](../../Reference/im/MimControl.md), or in a predefined statistics context. This is the only valid option for a predefined statistics context. |
| `M_PREPROCESS` | Preprocesses the specified image processing context and image processing result buffer. Note that, if not called explicitly, this operation will be performed automatically upon the first call to [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md).

If the identifier of the source image buffer is not set to [`M_NULL`](../../Reference/im/MimStatCalculate.md), information about the source image buffer will be used in the preprocessing operation. |
| `M_REMOVE` | Removes the results related to the specified source image. This removes the results of the calculations, performed on the specified source image, from the accumulated results in the result buffer.

To reset the minimum and maximum statistical results, use [`M_RESET_EXTREMES`](../../Reference/im/MimStatCalculate.md). The minimum and maximum statistical results cannot be removed.

> **Note:** Note that this value only works when dealing with a cumulative statistics context ([`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md)). |
| `M_RESET_EXTREMES` | Resets the results generated from [`M_STAT_MIN`](../../Reference/im/MimControl.md), [`M_STAT_MIN_ABS`](../../Reference/im/MimControl.md), [`M_STAT_MAX`](../../Reference/im/MimControl.md), and [`M_STAT_MAX_ABS`](../../Reference/im/MimControl.md) operations. Note that, when resetting the minimum and maximum results, the [`SrcImageBufId`](../../Reference/im/MimStatCalculate.md) parameter is ignored.

> **Note:** Note that this value only works when dealing with a cumulative statistics context ([`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md)). |
