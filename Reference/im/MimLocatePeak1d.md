---
doctype: Reference
module: im
function: MimLocatePeak1d
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimLocatePeak1d"
---

# MimLocatePeak1d

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

> Performs peak intensity detection in every lane (row or column) of the source image.

## Syntax

```c
void MimLocatePeak1d(
    AIL_ID     ContextId,         //in
    AIL_ID     SrcImageBufId,     //in
    AIL_ID     ResultId,          //out
    AIL_INT    PeakWidthNominal,  //in
    AIL_INT    PeakWidthDelta,    //in
    AIL_DOUBLE MinContrast,       //in
    AIL_INT64  ControlFlag,       //in
    AIL_DOUBLE ControlValue       //in
)
```

## Description

This function detects the position and the intensity of a peak in every lane (row or column) of an image. It is well suited for applications that generate 3D data based on laser line or sheet of light profiling.

The function iterates through each lane. For each lane, the function determines the pixels that are in the peak neighborhood, as defined by [`PeakWidthNominal`](../../Reference/im/MimLocatePeak1d.md), [`PeakWidthDelta`](../../Reference/im/MimLocatePeak1d.md), and [`MinContrast`](../../Reference/im/MimLocatePeak1d.md). For each such region, it finds the position of the highest intensity with sub-pixel accuracy, and calculates the average intensity around that position. The position can be retrieved in a fixed-point representation, with 0 to 7 fractional bits.

For optimal performance, preprocess the result buffer by calling [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) with [`M_PREPROCESS`](../../Reference/im/MimLocatePeak1d.md). You can choose to preprocess with or without an image. When preprocessing with an image, specify a typical image in the series of images that you will process with [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md). The preprocessed image's size and bit depth are stored. When preprocessing without an image ([`SrcImageBufId`](../../Reference/im/MimLocatePeak1d.md) set to [`M_NULL`](../../Reference/im/MimLocatePeak1d.md)), you must specify the number of scan lanes in a typical image in the series with [`ControlFlag`](../../Reference/im/MimLocatePeak1d.md). When preprocessing without an image, it is assumed that all images are 8-bit unsigned.

If the preprocess operation is not done explicitly (using [`M_PREPROCESS`](../../Reference/im/MimLocatePeak1d.md)), [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) will preprocess the result buffer internally when it is first called. If a new image in the series has a different size or bit depth than the image used to preprocess, [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) will preprocess the result buffer again, given the new image's characteristics.

In a noisy image, or any image that might have multiple peaks in a single lane, such as when the object or surface is highly reflective, [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) can record the position and intensity of multiple peaks in a lane. You can create a custom application that sifts through the multiple peaks in each lane to determine the best peak in the lane for your needs. To record multiple peaks in a lane, use [`MimControl`](../../Reference/im/MimControl.md) with [`M_NUMBER_OF_PEAKS`](../../Reference/im/MimControl.md) set to the maximum number of peaks to record. For more information, see [Multiple peaks in a single lane](../../UserGuide/C05_Specialized_image_processing/S10_Peak_detection_and_depth_maps.md).

You can retrieve the calculated results from the result buffer using [`MimGetResultSingle`](../../Reference/im/MimGetResultSingle.md).

[`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) can accumulate results from multiple frames in one result buffer. To do so, you must set [`MimControl`](../../Reference/im/MimControl.md) with [`M_NUMBER_OF_FRAMES`](../../Reference/im/MimControl.md), which is the maximum number of frames for which results can be accumulated in the result buffer. When calling [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md), set [`M_FRAME_INDEX`](../../Reference/im/MimLocatePeak1d.md) to the appropriate index in the result buffer which corresponds to the frame for which to store results.

[`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) supports multi-frame image buffers. When passing a multi-frame image buffer, you must set [`MimControl`](../../Reference/im/MimControl.md) with [`M_FRAME_SIZE`](../../Reference/im/MimControl.md) to the Y-size of each individual frame. For information on multi-frame image buffers, see [Specifying the dimensions of a multi-frame image buffer](../../UserGuide/C23_Data_buffers/S03_Specifying_the_dimensions_of_a_data_buffer.md). Note that the total number of frames ([`M_NUMBER_OF_FRAMES`](../../Reference/im/MimControl.md)) can be different from the frame capacity of the source image buffer. For example, if the source image buffer has a 5-frame capacity and you have 10 total frames to process, 2 calls to [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) can be done before calling [`MimGetResultSingle`](../../Reference/im/MimGetResultSingle.md). Typically, you would set [`M_FRAME_INDEX`](../../Reference/im/MimLocatePeak1d.md) to frame 0 on the first call to [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md); then, set it to frame 5 on the second call. Finally, it is important to set [`M_FRAME_INDEX`](../../Reference/im/MimLocatePeak1d.md) such that the result buffer can hold all results produced from the call. That is, there must be enough indices available in the result buffer to accommodate results from all frames in the source image buffer. For more information on peak extraction from a multi-frame image buffer, see [Extraction of peaks from a multi-frame buffer](../../UserGuide/C05_Specialized_image_processing/S10_Peak_detection_and_depth_maps.md).

For results obtained from laser line images, you can draw an uncorrected depth map or intensity map using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_DEPTH_MAP_ROW`](../../Reference/im/MimDraw.md) or [`M_DRAW_INTENSITY_MAP_ROW`](../../Reference/im/MimDraw.md), respectively. For more information, see [Generating an uncorrected depth map](../../UserGuide/C05_Specialized_image_processing/S10_Peak_detection_and_depth_maps.md).

To verify that the peak was found at the right location, you can draw the calculated peak intensities at the calculated positions into an image buffer using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_PEAKS`](../../Reference/im/MimDraw.md).

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the image processing context.

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `ResultId` *(out, AIL_ID)*

Specifies the identifier of the image processing result buffer in which to store results. The result buffer must have been allocated using[`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_LOCATE_PEAK_1D_RESULT`](../../Reference/im/MimAllocResult.md).

### `PeakWidthNominal` *(in, AIL_INT)*

Specifies the nominal (expected average) width of the peak neighborhood. In laser line images, this is the average width of the laser line.

*For specifying the nominal width of the peak*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the value of the control type [`M_PEAK_WIDTH_NOMINAL`](../../Reference/im/MimControl.md), set using [`MimControl`](../../Reference/im/MimControl.md). The default value of [`M_PEAK_WIDTH_NOMINAL`](../../Reference/im/MimControl.md) is 20. |
| `M_NULL` |  |
| `Value > 0` | Specifies the average width, in pixels. |

### `PeakWidthDelta` *(in, AIL_INT)*

Specifies the number of pixels that can be added to or subtracted from the nominal width, when determining the range of allowable widths of the peak neighborhood.

*For specifying the variance of the width of the peak*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the value of the control type[`M_PEAK_WIDTH_DELTA`](../../Reference/im/MimControl.md), set using [`MimControl`](../../Reference/im/MimControl.md). The default value of [`M_PEAK_WIDTH_DELTA`](../../Reference/im/MimControl.md) is 20. |
| `M_NULL` |  |
| `Value > 0` | Specifies the maximum amount of variance from the nominal width, in pixels. |

### `MinContrast` *(in, AIL_DOUBLE)*

Specifies the minimum contrast (difference) between the intensity of the local background and the minimum acceptable intensity of pixels in the peak neighborhood.

*For specifying the contrast that defines the peak*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the value of the control type [`M_MINIMUM_CONTRAST`](../../Reference/im/MimControl.md), set using [`MimControl`](../../Reference/im/MimControl.md). The default value of [`M_MINIMUM_CONTRAST`](../../Reference/im/MimControl.md) is 100. |
| `M_NULL` |  |
| `0 <= Value <= 65534` | Specifies the minimum difference in intensity.

> **Note:** Note that the default value is 100, regardless of the bit depth of the image. To keep the default functionality in a 16-bit image, set this value to 25,600. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to explicitly preprocess the image processing result buffer; or specifies a frame index for which to extract peaks.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the [`ControlFlag`](../../Reference/im/MimLocatePeak1d.md) setting.

## Parameter Associations

### For specifying preprocessing or for specifying the frame index into which peaks will be extracted

The following [`ControlFlag`](../../Reference/im/MimLocatePeak1d.md) and corresponding [`ControlValue`](../../Reference/im/MimLocatePeak1d.md) parameter settings are used to control preprocessing and for specifying the frame index.

---

### `M_DEFAULT`

Specifies to perform the peak intensity detection on the specified image.  When the specified image processing result buffer has not been preprocessed or has been preprocessed with an image that has different characteristics than [`SrcImageBufId`](../../Reference/im/MimLocatePeak1d.md), (for instance, size or bit depth), [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) will automatically preprocess the image processing result buffer before performing the peak intensity detection.  If a multi-frame image buffer is used ([`MimControl`](../../Reference/im/MimControl.md) with [`M_FRAME_SIZE`](../../Reference/im/MimControl.md) not set to [`M_FULL_SIZE`](../../Reference/im/MimControl.md)), peak extraction begins at the first frame (frame 0).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_FRAME_INDEX`

Specifies the frame index in the [`M_LOCATE_PEAK_1D_RESULT`](../../Reference/im/MimAllocResult.md) result buffer into which extractions will be accumulated. If processing a multi-frame image buffer, this sets the first frame index in the result buffer into which extractions will be accumulated.  When accumulating results from multiple calls to [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md), set [`M_FRAME_INDEX`](../../Reference/im/MimLocatePeak1d.md) to the appropriate index in the result buffer. If using a multi-frame image buffer, specify a frame index that is subsequent to the frames processed in the preceding call, so as not to overwrite previous results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0. |
| `0 <= Value < M_NUMBER_OF_FRAMES` | Specifies the frame index into which peaks will be accumulated. |

---

### `M_PREPROCESS`

Specifies to preprocess the specified image processing result buffer. When [`M_PREPROCESS`](../../Reference/im/MimLocatePeak1d.md) is specified, set [`PeakWidthNominal`](../../Reference/im/MimLocatePeak1d.md), [`PeakWidthDelta`](../../Reference/im/MimLocatePeak1d.md), and [`MinContrast`](../../Reference/im/MimLocatePeak1d.md) to [`M_NULL`](../../Reference/im/MimLocatePeak1d.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior, when preprocessing with a source image. |
| `Value >= 1` | Specifies the number of scan lanes in subsequent source images, when preprocessing without an image ([`ControlFlag`](../../Reference/im/MimLocatePeak1d.md) set to [`M_PREPROCESS`](../../Reference/im/MimLocatePeak1d.md) and [`SrcImageBufId`](../../Reference/im/MimLocatePeak1d.md) set to [`M_NULL`](../../Reference/im/MimLocatePeak1d.md)). |

Specifies to ignore this parameter. This setting should be used exclusively when [`ControlFlag`](../../Reference/im/MimLocatePeak1d.md) is set to [`M_PREPROCESS`](../../Reference/im/MimLocatePeak1d.md); this is the only possible setting in this case.
