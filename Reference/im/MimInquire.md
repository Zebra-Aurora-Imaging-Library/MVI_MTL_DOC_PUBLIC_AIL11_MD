---
doctype: Reference
module: im
function: MimInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimInquire"
---

# MimInquire

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

> Inquire about an image processing context or result buffer setting.

## Syntax

```c
AIL_INT MimInquire(
    AIL_ID    ContextOrResultImId,  //in
    AIL_INT64 InquireType,          //in
    void *    UserVarPtr            //out
)
```

## Description

This function inquires information about an image processing context or result buffer setting. The image processing context or result buffer must have been allocated with [`MimAlloc`](../../Reference/im/MimAlloc.md) or [`MimAllocResult`](../../Reference/im/MimAllocResult.md), respectively.

## Parameters

### `ContextOrResultImId` *(in, AIL_ID)*

Specifies the identifier of the image processing context or result buffer about which to inquire information.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of information about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address of the variable in which to write the requested information. Since the [`MimInquire`](../../Reference/im/MimInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about any image processing result buffer

For any image processing result buffer, the [`ContextOrResultImId`](../../Reference/im/MimInquire.md) and [`InquireType`](../../Reference/im/MimInquire.md) parameters can be set to one of the following.

---

### `Image processing result buffer ID`

Specifies an image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md).

#### `M_ALLOCATED_TYPE`

Inquires the image processing result buffer's data type, as was set using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) upon allocation.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image processing result buffer's data-type cannot be determined. Note that any result buffer that does not support allocation with a combination value from the [`data`](../../Reference/im/MimAllocResult.md) table will return this value. |
| `M_TYPE_AIL_FLOAT` | Specifies that the image processing result buffer was allocated with an _AIL_FLOAT_ data-type. |
| `M_TYPE_AIL_INT` | Specifies that the image processing result buffer was allocated with an _AIL_INT_ data-type. |

#### `M_MODIFICATION_COUNT`

Inquires the number of modifications made to the result buffer since it was allocated.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of modifications. |

#### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the result buffer is allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

#### `M_RESULT_SIZE`

Inquires the number of entries in the result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library automatically establishes the size of the result buffer. |
| `Value > 0` | Specifies the number of buffer entries. |

#### `M_RESULT_TYPE`

Inquires the attribute or nature of the result buffer.

| Value | Description |
| --- | --- |
| `M_AUGMENTATION_RESULT` | Specifies that the buffer can hold [`MimAugment`](../../Reference/im/MimAugment.md) results. |
| `M_COUNT_LIST` | Specifies that the buffer can hold [`MimCountDifference`](../../Reference/im/MimCountDifference.md) results. |
| `M_EVENT_LIST` | Specifies that the buffer can hold [`MimLocateEvent`](../../Reference/im/MimLocateEvent.md) results. |
| `M_EXTREME_LIST` | Specifies that the buffer can hold [`MimFindExtreme`](../../Reference/im/MimFindExtreme.md) results. |
| `M_FIND_ORIENTATION_LIST` | Specifies that the buffer can hold [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) results. |
| `M_HIST_LIST` | Specifies that the buffer can hold [`MimHistogram`](../../Reference/im/MimHistogram.md) results. |
| `M_LOCATE_DIFFERENCE_RESULT` | Specifies that the buffer can hold [`MimLocateDifference`](../../Reference/im/MimLocateDifference.md) results. |
| `M_LOCATE_PEAK_1D_RESULT` | Specifies that the buffer can hold [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) results. |
| `M_PROJ_LIST` | Specifies that the buffer can hold [`MimProjection`](../../Reference/im/MimProjection.md) results. |
| `M_STATISTICS_RESULT` | Specifies that the buffer can hold [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) results. |
| `M_WAVELET_TRANSFORM_RESULT` | Specifies that the buffer can hold results from [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md). |
| `M_FLOAT` | Allocates a result buffer of type _float_. |

### For inquiring about any image processing context

For any image processing context, the [`ContextOrResultImId`](../../Reference/im/MimInquire.md) and [`InquireType`](../../Reference/im/MimInquire.md) parameters can be set to the following:

---

### `Image processing context ID`

Specifies an image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md).

#### `M_CONTEXT_TYPE`

Inquires the type of the image processing context.

| Value | Description |
| --- | --- |
| `M_AUGMENTATION_CONTEXT` | Specifies an image processing context that can be used with [`MimAugment`](../../Reference/im/MimAugment.md). |
| `M_BINARIZE_ADAPTIVE_CONTEXT` | Specifies an image processing context that can be used with [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) to perform an adaptive binarization. |
| `M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT` | Specifies an image processing context that can be used with [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) to perform an adaptive binarization using seeds. |
| `M_DEAD_PIXEL_CONTEXT` | Specifies an image processing context that can be used with [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md). |
| `M_DEINTERLACE_CONTEXT` | Specifies an image processing context that can be used with [`MimDeinterlace`](../../Reference/im/MimDeinterlace.md). |
| `M_FILTER_MAJORITY_CONTEXT` | Specifies an image processing context that can be used with [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md). |
| `M_FIND_ORIENTATION_CONTEXT` | Specifies an image processing context that can be used with [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md). |
| `M_FLAT_FIELD_CONTEXT` | Specifies an image processing context that can be used with [`MimFlatField`](../../Reference/im/MimFlatField.md). |
| `M_HISTOGRAM_EQUALIZE_ADAPTIVE_CONTEXT` | Specifies an image processing context that can be used with [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md). |
| `M_IM_REMAP_CONTEXT` | Specifies an image processing context that can be used with [`MimRemap`](../../Reference/im/MimRemap.md). |
| `M_LINEAR_FILTER_IIR_CONTEXT` | Specifies an image processing context that can be used with [`MimConvolve`](../../Reference/im/MimConvolve.md) or [`MimDifferential`](../../Reference/im/MimDifferential.md). |
| `M_LOCATE_PEAK_1D_CONTEXT` | Specifies an image processing context that can be used with [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md). |
| `M_MATCH_CONTEXT` | Specifies an image processing context that can be used with [`MimMatch`](../../Reference/im/MimMatch.md). |
| `M_REARRANGE_CONTEXT` | Specifies an image processing context that can be used with [`MimRearrange`](../../Reference/im/MimRearrange.md). |
| `M_STATISTICS_CONTEXT` | Specifies an image processing context that can be used with [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md). |
| `M_STATISTICS_CUMULATIVE_CONTEXT` | Specifies an image processing context that can be used with [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) when calculating statistics from multiple images. |
| `M_UNWARP_ALONG_PATH_CONTEXT` | Specifies an image processing context that can be used with [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md). |
| `M_WAVELET_TRANSFORM_CONTEXT` | Specifies an image processing context that can be used with [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md). |
| `M_WAVELET_TRANSFORM_CUSTOM_CONTEXT` | Specifies a custom image processing context that can be used with [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md). |

### For inquiring about a specific type of image processing context

For an image processing context of a specific type, the [`ContextOrResultImId`](../../Reference/im/MimInquire.md) and [`InquireType`](../../Reference/im/MimInquire.md) parameters can be set to one of the following:

---

### `Adaptive binarize context ID`

Specifies an adaptive binarize context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) operations.

#### `M_AVERAGE_MODE`

Inquires how Aurora Imaging Library establishes average pixel values.

| Value | Description |
| --- | --- |
| *(see [`M_AVERAGE_MODE`](Reference/im/MimControl.md))* |  |

#### `M_FOREGROUND_VALUE`

Inquires whether the objects to binarize are lighter or darker than the background.

| Value | Description |
| --- | --- |
| *(see [`M_FOREGROUND_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_GLOBAL_MAX`

Inquires the maximum threshold value.

| Value | Description |
| --- | --- |
| *(see [`M_GLOBAL_MAX`](Reference/im/MimControl.md))* |  |
| `M_UNKNOWN` | Specifies that there is no maximum threshold value restriction. |

#### `M_GLOBAL_MIN`

Inquires the minimum threshold value.

| Value | Description |
| --- | --- |
| *(see [`M_GLOBAL_MIN`](Reference/im/MimControl.md))* |  |
| `M_UNKNOWN` | Specifies that there is no minimum threshold value restriction. |

#### `M_GLOBAL_OFFSET`

Inquires the offset to add to each established threshold value.

| Value | Description |
| --- | --- |
| *(see [`M_GLOBAL_OFFSET`](Reference/im/MimControl.md))* |  |

#### `M_GLOBAL_OFFSET_SECOND_PASS`

Inquires the offset to apply to the threshold values for the second pass of a hysteresis adaptive binarization.

| Value | Description |
| --- | --- |
| *(see [`M_GLOBAL_OFFSET_SECOND_PASS`](Reference/im/MimControl.md))* |  |

#### `M_LOCAL_DIMENSION`

Inquires the size of the neighborhood that the threshold mode uses to establish threshold values.

| Value | Description |
| --- | --- |
| *(see [`M_LOCAL_DIMENSION`](Reference/im/MimControl.md))* |  |

#### `M_MINIMUM_CONTRAST`

Inquires the minimum contrast between background and foreground pixels.

| Value | Description |
| --- | --- |
| *(see [`M_MINIMUM_CONTRAST`](Reference/im/MimControl.md))* |  |

#### `M_NIBLACK_BIAS`

Inquires the bias for Niblack's binarization mode.

| Value | Description |
| --- | --- |
| *(see [`M_NIBLACK_BIAS`](Reference/im/MimControl.md))* |  |

#### `M_NIBLACK_BIAS_SECOND_PASS`

Inquires the bias for the second pass of a Niblack adaptive binarization that uses a hysteresis process.

| Value | Description |
| --- | --- |
| *(see [`M_NIBLACK_BIAS_SECOND_PASS`](Reference/im/MimControl.md))* |  |

#### `M_THRESHOLD_MODE`

Inquires how Aurora Imaging Library establishes the threshold values with which to binarize the source image.

| Value | Description |
| --- | --- |
| *(see [`M_THRESHOLD_MODE`](Reference/im/MimControl.md))* |  |

#### `M_THRESHOLD_TYPE`

Inquires the threshold type used for the adaptive binarization.

| Value | Description |
| --- | --- |
| *(see [`M_THRESHOLD_TYPE`](Reference/im/MimControl.md))* |  |

---

### `Adaptive binarize from seed context ID`

Specifies an adaptive binarize context that uses seeds, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) operations.

#### `M_FOREGROUND_VALUE`

Inquires whether the objects to binarize are lighter or darker than the background.

| Value | Description |
| --- | --- |
| *(see [`M_FOREGROUND_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_GLOBAL_OFFSET`

Inquires the offset to add to each established threshold value.

| Value | Description |
| --- | --- |
| *(see [`M_GLOBAL_OFFSET`](Reference/im/MimControl.md))* |  |

#### `M_NB_ITERATIONS`

Inquires the number of times to perform the adaptive threshold process specified with [`M_THRESHOLD_MODE`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NB_ITERATIONS`](Reference/im/MimControl.md))* |  |

#### `M_NB_SEED_ITERATIONS`

Inquires the number of iterations with which to internally establish seeds.

| Value | Description |
| --- | --- |
| *(see [`M_NB_SEED_ITERATIONS`](Reference/im/MimControl.md))* |  |

#### `M_THRESHOLD_MODE`

Inquires how Aurora Imaging Library uses seeds to establish the threshold values with which to binarize the source image.

| Value | Description |
| --- | --- |
| *(see [`M_THRESHOLD_MODE`](Reference/im/MimControl.md))* |  |

---

### `Augmentation context ID`

Specifies an augmentation context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_AUGMENTATION_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimAugment`](../../Reference/im/MimAugment.md) operations.

#### `M_AUG_ASPECT_RATIO_OP`

Inquires whether to enable the aspect ratio operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ASPECT_RATIO_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_ASPECT_RATIO_OP_MAX`

Inquires the maximum aspect ratio to apply for [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ASPECT_RATIO_OP_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_ASPECT_RATIO_OP_MIN`

Inquires the minimum aspect ratio to apply for [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ASPECT_RATIO_OP_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_ASPECT_RATIO_OP_MODE`

Inquires the aspect ratio mode to apply for [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ASPECT_RATIO_OP_MODE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_BLUR_MOTION_OP`

Inquires whether to enable the operation that applies a motion blur kernel with random direction and kernel size.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_BLUR_MOTION_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_BLUR_MOTION_OP_ANGLE_MAX`

Inquires the maximum value for the motion blur angle's random distribution [min, max], to apply for [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_BLUR_MOTION_OP_ANGLE_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_BLUR_MOTION_OP_ANGLE_MIN`

Inquires the minimum value for the motion blur angle's random distribution [min, max], to apply for [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_BLUR_MOTION_OP_ANGLE_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_BLUR_MOTION_OP_SIZE_MAX`

Inquires the maximum value for the kernel size's random distribution [min, max], to apply for [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_BLUR_MOTION_OP_SIZE_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_BLUR_MOTION_OP_SIZE_MIN`

Inquires the minimum value for the kernel size's random distribution [min, max], to apply for [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_BLUR_MOTION_OP_SIZE_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_CROP_OP`

Inquires whether to enable the crop operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_CROP_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_CROP_OP_FACTOR_X`

Inquires the crop width factor, to apply for [`M_AUG_CROP_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_CROP_OP_FACTOR_X`](Reference/im/MimControl.md))* |  |

#### `M_AUG_CROP_OP_FACTOR_Y`

Inquires the crop height factor, to apply for[`M_AUG_CROP_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_CROP_OP_FACTOR_Y`](Reference/im/MimControl.md))* |  |

#### `M_AUG_CROP_OP_RESIZE`

Inquires the crop rese option, to apply for [`M_AUG_CROP_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_CROP_OP_RESIZE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_DILATION_ASYM_OP`

Inquires whether to enable the asymmetrical dilation operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_DILATION_ASYM_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_DILATION_ASYM_OP_NB_ITERATIONS_MAX`

Inquires the maximum number of iterations for dilation, to apply for [`M_AUG_DILATION_ASYM_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_DILATION_ASYM_OP_NB_ITERATIONS_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_DILATION_OP`

Inquires whether the dilation operation was enabled.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_DILATION_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_DILATION_OP_NB_ITERATIONS_MAX`

Inquires the maximum number of iterations, to apply for [`M_AUG_DILATION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_DILATION_OP_NB_ITERATIONS_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_EROSION_ASYM_OP`

Inquires whether to enable the asymmetrical erosion operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_EROSION_ASYM_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_EROSION_ASYM_OP_NB_ITERATIONS_MAX`

Inquires the maximum number of iterations for dilation, to apply for [`M_AUG_EROSION_ASYM_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_EROSION_ASYM_OP_NB_ITERATIONS_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_EROSION_OP`

Inquires whether to enable the erosion operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_EROSION_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_EROSION_OP_NB_ITERATIONS_MAX`

Inquires the maximum number of iterations, to apply for [`M_AUG_EROSION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_EROSION_OP_NB_ITERATIONS_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_FLIP_OP`

Inquires whether to enable the flip operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_FLIP_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_FLIP_OP_DIRECTION`

Inquires the maximum number of iterations, to apply for [`M_AUG_EROSION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_EROSION_OP_NB_ITERATIONS_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_GAMMA_OP`

Inquires whether to enable the gamma correction operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_GAMMA_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_GAMMA_OP_DELTA`

Inquires the range for the random distribution of gamma values [gamma - delta, gamma + delta], to apply for [`M_AUG_GAMMA_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_GAMMA_OP_DELTA`](Reference/im/MimControl.md))* |  |

#### `M_AUG_GAMMA_OP_MODE`

Inquires the gamma correction mode, to apply for [`M_AUG_GAMMA_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_GAMMA_OP_MODE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_GAMMA_OP_VALUE`

Inquires the gamma value, to apply for [`M_AUG_GAMMA_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_GAMMA_OP_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_HSV_VALUE_GAIN_OP`

Inquires whether to enable the operation that multiplies (gain) the color image's value band (HSV) by a random factor.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_HSV_VALUE_GAIN_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_HSV_VALUE_GAIN_OP_MAX`

Inquires the maximum value for the random value factor's distribution [min, max], to use for [`M_AUG_HSV_VALUE_GAIN_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_HSV_VALUE_GAIN_OP_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_HSV_VALUE_GAIN_OP_MIN`

Inquires the minimum value for the random value factor's distribution [min, max], to use for [`M_AUG_HSV_VALUE_GAIN_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_HSV_VALUE_GAIN_OP_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_HUE_OFFSET_OP`

Inquires whether to enable the operation that adds a random hue angle to hue values.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_HUE_OFFSET_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_HUE_OFFSET_OP_MAX`

Inquires the maximum value for the random hue angles' distribution [min, max], to use for [`M_AUG_HUE_OFFSET_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_HUE_OFFSET_OP_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_HUE_OFFSET_OP_MIN`

Inquires the minimum value for the random hue angles' distribution [min, max], to use for [`M_AUG_HUE_OFFSET_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_HUE_OFFSET_OP_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_INTENSITY_ADD_OP`

Inquires whether to enable the intensity addition (offset) operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_INTENSITY_ADD_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_INTENSITY_ADD_OP_DELTA`

Inquires the range for the random distribution of intensity addition (offset) values [offset - delta, offset + delta], to apply for [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_INTENSITY_ADD_OP_DELTA`](Reference/im/MimControl.md))* |  |

#### `M_AUG_INTENSITY_ADD_OP_MODE`

Inquires the intensity addition value (offset) mode, to apply for [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_INTENSITY_ADD_OP_MODE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_INTENSITY_ADD_OP_VALUE`

Inquires the intensity addition value (offset), to apply for [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_INTENSITY_ADD_OP_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_INTENSITY_MULTIPLY_OP`

Inquires whether to enable the intensity multiplication (gain) operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_INTENSITY_MULTIPLY_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_INTENSITY_MULTIPLY_OP_DELTA`

Inquires the range for the random distribution of the intensity multiplication (gain) values [gain - delta, gain + delta], to apply for [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_INTENSITY_MULTIPLY_OP_DELTA`](Reference/im/MimControl.md))* |  |

#### `M_AUG_INTENSITY_MULTIPLY_OP_MODE`

Inquires the multiplication value (gain) mode, to apply for [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_INTENSITY_MULTIPLY_OP_MODE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_INTENSITY_MULTIPLY_OP_VALUE`

Inquires the intensity multiplication value (gain), to apply for [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_INTENSITY_MULTIPLY_OP_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP`

Inquires whether to enable the directional lighting (add ramp) operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_LIGHTING_DIRECTIONAL_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP_ANGLE_MAX`

Inquires an illumination to add to the image at a random angle (ramp), to apply for [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_LIGHTING_DIRECTIONAL_OP_ANGLE_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP_INTENSITY_MAX`

Inquires the maximum light intensity, to use for [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_LIGHTING_DIRECTIONAL_OP_INTENSITY_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP_INTENSITY_MIN`

Inquires the minimum light intensity, to use for [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_LIGHTING_DIRECTIONAL_OP_INTENSITY_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP_MODE`

Inquires the directional lighting operation mode, to apply for [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_LIGHTING_DIRECTIONAL_OP_MODE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`

Inquires whether to enable the operation that applies a Gaussian additive noise with the option of a random noise standard deviation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP_STDDEV`

Inquires standard deviation for noise, to apply for [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP_STDDEV`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP_STDDEV_DELTA`

Inquires the range for the random distribution of noise standard deviation values [stddev - delta, stddev + delta], to apply for [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP_STDDEV_DELTA`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP`

Inquires whether to enable the operation that applies a multiplicative noise with option of a random noise standard deviation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_MULTIPLICATIVE_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP_DISTRIBUTION`

Inquires the uniform distribution for noise generation, to apply for [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_MULTIPLICATIVE_OP_DISTRIBUTION`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP_INTENSITY_MIN`

Inquires an optional minimum noise intensity value (white noise threshold), to apply for [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_MULTIPLICATIVE_OP_INTENSITY_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP_STDDEV`

Inquires the standard deviation for noise, to apply for [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_MULTIPLICATIVE_OP_STDDEV`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP_STDDEV_DELTA`

Inquires the range for the random distribution of noise standard deviation values [stddev - delta, stddev + delta], to apply for [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_MULTIPLICATIVE_OP_STDDEV_DELTA`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_SALT_PEPPER_OP`

Inquires whether to enable salt and pepper noise with the option of random density.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_SALT_PEPPER_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_SALT_PEPPER_OP_DENSITY`

Inquires the density value for salt and pepper noise, to use for [`M_AUG_NOISE_SALT_PEPPER_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_SALT_PEPPER_OP_DENSITY`](Reference/im/MimControl.md))* |  |

#### `M_AUG_NOISE_SALT_PEPPER_OP_DENSITY_DELTA`

Inquires the range for the random distribution of noise density values [density - delta, density + delta], to use for [`M_AUG_NOISE_SALT_PEPPER_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_NOISE_SALT_PEPPER_OP_DENSITY_DELTA`](Reference/im/MimControl.md))* |  |

#### `M_AUG_REDUCE_OP`

Inquires whether to enable the reduce operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_REDUCE_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_REDUCE_OP_BACKGROUND_COLOR`

Inquires the background color to use for [`M_AUG_REDUCE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_REDUCE_OP_BACKGROUND_COLOR`](Reference/im/MimControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_AUG_REDUCE_OP_FACTOR_MAX`

Inquires the maximum scale factor by which to reduce the image for [`M_AUG_REDUCE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_REDUCE_OP_FACTOR_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_REDUCE_OP_FACTOR_MIN`

Inquires the minimum scale factor by which to reduce the image for [`M_AUG_REDUCE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_REDUCE_OP_FACTOR_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_RNG_INIT_VALUE`

Inquires the initialization value for the internal random number generator, when [`M_AUG_SEED_MODE`](../../Reference/im/MimControl.md) is set to [`M_RNG_AUTO`](../../Reference/im/MimControl.md) or [`M_RNG_INIT_VALUE`](../../Reference/im/MimControl.md). If you specified [`M_RNG_AUTO`](../../Reference/im/MimControl.md), you must preprocess the augmentation context before inquiring the initialization value.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_RNG_INIT_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_ROTATION_OP`

Inquires whether to enable the rotation operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ROTATION_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_ROTATION_OP_ANGLE_DELTA`

Inquires the delta angle, to apply for[`M_AUG_ROTATION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ROTATION_OP_ANGLE_DELTA`](Reference/im/MimControl.md))* |  |

#### `M_AUG_ROTATION_OP_ANGLE_MAX`

Inquires the maximum rotation angle, to apply for [`M_AUG_ROTATION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ROTATION_OP_ANGLE_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_ROTATION_OP_ANGLE_MIN`

Inquires the minimum rotation angle, to apply for [`M_AUG_ROTATION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ROTATION_OP_ANGLE_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_ROTATION_OP_ANGLE_REF`

Inquires the reference angle. All other angles for[`M_AUG_ROTATION_OP`](../../Reference/im/MimInquire.md)will be calculated relative to this angle.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ROTATION_OP_ANGLE_REF`](Reference/im/MimControl.md))* |  |

#### `M_AUG_ROTATION_OP_ANGLE_STEP`

Inquires the step size, to apply for[`M_AUG_ROTATION_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ROTATION_OP_ANGLE_STEP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SATURATION_GAIN_OP`

Inquires whether to enable the multiplication (gain) of saturation band (HSV) operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SATURATION_GAIN_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SATURATION_GAIN_OP_MAX`

Inquires the maximum value for the random saturation factor's distribution [min, max], to use for [`M_AUG_SATURATION_GAIN_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SATURATION_GAIN_OP_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SATURATION_GAIN_OP_MIN`

Inquires the minimum value for the random saturation factor's distribution [min, max], to use for [`M_AUG_SATURATION_GAIN_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SATURATION_GAIN_OP_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SCALE_OP`

Inquires whether to enable the scale operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SCALE_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SCALE_OP_FACTOR_MAX`

Inquires the maximum scale factor, to apply for [`M_AUG_SCALE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SCALE_OP_FACTOR_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SCALE_OP_FACTOR_MIN`

Inquires the minimum scale factor, to apply for [`M_AUG_SCALE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SCALE_OP_FACTOR_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SEED_MODE`

Inquires the mode with which to initialize seeds.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SEED_MODE`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SHARPEN_DERICHE_OP`

Inquires whether to enable the operation that applies a Deriche sharpening filter with a random filter smoothness value.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SHARPEN_DERICHE_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SHARPEN_DERICHE_OP_FACTOR_MAX`

Inquires the maximum value for the sharpen filter smoothness value's random distribution [min, max], to apply for [`M_AUG_SHARPEN_DERICHE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SHARPEN_DERICHE_OP_FACTOR_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SHARPEN_DERICHE_OP_FACTOR_MIN`

Inquires the minimum value for the sharpen filter smoothness value's random distribution [min, max], to apply for [`M_AUG_SHARPEN_DERICHE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SHARPEN_DERICHE_OP_FACTOR_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SHEAR_X_OP`

Inquires whether to enable the X-direction shear operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SHEAR_X_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SHEAR_X_OP_MAX`

Inquires the maximum value for the X-coefficient's random distribution [min, max], to use for [`M_AUG_SHEAR_X_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SHEAR_X_OP_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SHEAR_X_OP_MIN`

Inquires the minimum value for the X-coefficient's random distribution [min, max], to use for [`M_AUG_SHEAR_X_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SHEAR_X_OP_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SHEAR_Y_OP`

Inquires whether to enable the Y-direction shear operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SHEAR_Y_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SHEAR_Y_OP_MAX`

Inquires the maximum value for the Y-coefficient's random distribution [min, max], to use for [`M_AUG_SHEAR_Y_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SHEAR_Y_OP_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SHEAR_Y_OP_MIN`

Inquires the minimum value for the Y-coefficient's random distribution [min, max], to use for [`M_AUG_SHEAR_Y_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SHEAR_Y_OP_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SMOOTH_DERICHE_OP`

Inquires whether to enable the operation that performs a Deriche filter smoothing with random filter smoothness value.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SMOOTH_DERICHE_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SMOOTH_DERICHE_OP_FACTOR_MAX`

Inquires the maximum value for the Deriche smoothness value's random distribution [min, max], to use for [`M_AUG_SMOOTH_DERICHE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SMOOTH_DERICHE_OP_FACTOR_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SMOOTH_DERICHE_OP_FACTOR_MIN`

Inquires the minimum value for the Deriche smoothness value's random distribution [min, max], to use for [`M_AUG_SMOOTH_DERICHE_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SMOOTH_DERICHE_OP_FACTOR_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SMOOTH_GAUSSIAN_OP`

Inquires whether to enable the operation that performs a blurring of the image with a Gaussian blurring kernel calculated using a Gaussian function with a random standard deviation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SMOOTH_GAUSSIAN_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SMOOTH_GAUSSIAN_OP_STDDEV_MAX`

Inquires the maximum value for the Gaussian standard deviation's random distribution [min, max], to apply for [`M_AUG_SMOOTH_GAUSSIAN_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SMOOTH_GAUSSIAN_OP_STDDEV_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_SMOOTH_GAUSSIAN_OP_STDDEV_MIN`

Inquires the minimum value for the Gaussian standard deviation's random distribution [min, max], to apply for [`M_AUG_SMOOTH_GAUSSIAN_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_SMOOTH_GAUSSIAN_OP_STDDEV_MIN`](Reference/im/MimControl.md))* |  |

#### `M_AUG_TRANSLATION_X_OP`

Inquires whether to enable the X-direction translation operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_TRANSLATION_X_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_TRANSLATION_X_OP_MAX`

Inquires the maximum translation in the X-direction, to apply for [`M_AUG_TRANSLATION_X_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_TRANSLATION_X_OP_MAX`](Reference/im/MimControl.md))* |  |

#### `M_AUG_TRANSLATION_Y_OP`

Inquires whether to enable the Y-direction translation operation.

| Value | Description |
| --- | --- |
| *(see [`M_AUG_TRANSLATION_Y_OP`](Reference/im/MimControl.md))* |  |

#### `M_AUG_TRANSLATION_Y_OP_MAX`

Inquires the maximum translation in the Y-direction, to apply for [`M_AUG_TRANSLATION_Y_OP`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_TRANSLATION_Y_OP_MAX`](Reference/im/MimControl.md))* |  |

#### `M_PREPROCESSED`

Inquires whether the augmentation context was preprocessed (that is, whether [`MimAugment`](../../Reference/im/MimAugment.md)was called with [`M_PREPROCESS`](../../Reference/im/MimAugment.md)).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the augmentation context was not preprocessed. |
| `M_TRUE` | Specifies that the augmentation context was preprocessed. |

---

### `Cumulative statistics image processing context ID`

Specifies a cumulative statistics image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) operations.

#### `M_PREPROCESSED`

Inquires whether the cumulative statistics image processing context was preprocessed (that is, whether [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md)was called).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the cumulative statistics image processing context was not preprocessed. |
| `M_TRUE` | Specifies that the cumulative statistics image processing context was preprocessed. |

#### `M_SOURCE_SIZE_X`

Inquires the width of a typical source image.

| Value | Description |
| --- | --- |
| *(see [`M_SOURCE_SIZE_X`](Reference/im/MimControl.md))* |  |
| `M_INVALID` | Specifies that the width has not been set. |

#### `M_SOURCE_SIZE_Y`

Inquires the height of a typical source image.

| Value | Description |
| --- | --- |
| *(see [`M_SOURCE_SIZE_Y`](Reference/im/MimControl.md))* |  |
| `M_INVALID` | Specifies that the height has not been set. |

#### `M_STAT_MAX`

Inquires whether to calculate the maximum pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MAX`](Reference/im/MimControl.md))* |  |

#### `M_STAT_MAX_ABS`

Inquires whether to calculate the maximum absolute pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MAX_ABS`](Reference/im/MimControl.md))* |  |

#### `M_STAT_MEAN`

Inquires whether to calculate the mean value of the pixels as the statistical operation.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MEAN`](Reference/im/MimControl.md))* |  |

#### `M_STAT_MIN`

Inquires whether to calculate the minimum pixel value as the statistical operation.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MIN`](Reference/im/MimControl.md))* |  |

#### `M_STAT_MIN_ABS`

Inquires whether to calculate the minimum absolute pixel value as the statistical operation.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MIN_ABS`](Reference/im/MimControl.md))* |  |

#### `M_STAT_NUMBER`

Inquires whether to keep track of the number of source images.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_NUMBER`](Reference/im/MimControl.md))* |  |

#### `M_STAT_STANDARD_DEVIATION`

Inquires whether to calculate the standard deviation from the value of each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_STANDARD_DEVIATION`](Reference/im/MimControl.md))* |  |

#### `M_STAT_SUM`

Inquires whether to calculate the sum of the pixel values from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_SUM`](Reference/im/MimControl.md))* |  |

#### `M_STAT_SUM_ABS`

Inquires whether to calculate the sum of the absolute pixel values from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_SUM_ABS`](Reference/im/MimControl.md))* |  |

#### `M_STAT_SUM_OF_SQUARES`

Inquires whether to calculate the sum of the squared pixel values from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_SUM_OF_SQUARES`](Reference/im/MimControl.md))* |  |

---

### `Dead pixel correction image processing context ID`

Specifies a dead pixel correction image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_DEAD_PIXEL_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) operations.

#### `M_DEAD_PIXELS_IMAGE_ATTRIBUTE`

Inquires the attribute of the dead pixels image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that dead pixels have not been specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DEAD_PIXELS`](../../Reference/im/MimControl.md) or using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_DEAD_PIXELS`](../../Reference/im/MimPut.md). |
| `Value` | Specifies a bit-encoded value that indicates the image buffer's attributes, set at allocation. |

#### `M_DEAD_PIXELS_IMAGE_HEIGHT`

Inquires the height of the dead pixels image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that dead pixels have not been specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DEAD_PIXELS`](../../Reference/im/MimControl.md) or using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_DEAD_PIXELS`](../../Reference/im/MimPut.md). |
| `Value` | Specifies the height, in pixels. |

#### `M_DEAD_PIXELS_IMAGE_NB_BANDS`

Inquires the number of bands of the dead pixels image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that dead pixels have not been specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DEAD_PIXELS`](../../Reference/im/MimControl.md) or using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_DEAD_PIXELS`](../../Reference/im/MimPut.md). |
| `Value` | Specifies the number of bands. |

#### `M_DEAD_PIXELS_IMAGE_TYPE`

Inquires the bit depth and data type of the dead pixels image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that dead pixels have not been specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DEAD_PIXELS`](../../Reference/im/MimControl.md) or using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_DEAD_PIXELS`](../../Reference/im/MimPut.md). |
| `depth value + M_FLOAT` | Specifies the data depth and that the data type is floating-point. |
| `depth value + M_SIGNED` | Specifies the data depth and that the data type is signed. |
| `depth value + M_UNSIGNED` | Specifies the data depth and that the data type is unsigned. |

#### `M_DEAD_PIXELS_IMAGE_WIDTH`

Inquires the width of the dead pixels image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that dead pixels have not been specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DEAD_PIXELS`](../../Reference/im/MimControl.md) or using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_DEAD_PIXELS`](../../Reference/im/MimPut.md). |
| `Value` | Specifies the width, in pixels. |

#### `M_INTERPOLATION_MODE`

Inquires the interpolation mode used to establish the new value for dead pixels.

| Value | Description |
| --- | --- |
| *(see [`M_INTERPOLATION_MODE`](Reference/im/MimControl.md))* |  |

#### `M_XY_DEAD_PIXELS_ARRAY_SIZE`

Inquires the number of elements in the arrays containing the X- and Y-coordinates of the dead pixels.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of elements in the arrays. |

---

### `Deinterlacing image processing context ID`

Specifies a deinterlacing image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_DEINTERLACE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimDeinterlace`](../../Reference/im/MimDeinterlace.md) operations.

#### `M_DEINTERLACE_TYPE`

Inquires the deinterlacing algorithm.

| Value | Description |
| --- | --- |
| *(see [`M_DEINTERLACE_TYPE`](Reference/im/MimControl.md))* |  |

#### `M_DISCARD_FIELD`

Inquires the field that is discarded when [`M_DEINTERLACE_TYPE`](../../Reference/im/MimControl.md) is set to [`M_DISCARD`](../../Reference/im/MimControl.md) or [`M_ADAPTIVE_DISCARD`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_DISCARD_FIELD`](Reference/im/MimControl.md))* |  |

#### `M_FIRST_FIELD`

Inquires the field that is processed first when [`M_DEINTERLACE_TYPE`](../../Reference/im/MimControl.md) is set to [`M_BOB`](../../Reference/im/MimControl.md) or [`M_ADAPTIVE_BOB`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_FIRST_FIELD`](Reference/im/MimControl.md))* |  |

#### `M_MOTION_DETECT_NUM_FRAMES`

Inquires the number of frames to use to perform the motion detection.

| Value | Description |
| --- | --- |
| *(see [`M_MOTION_DETECT_NUM_FRAMES`](Reference/im/MimControl.md))* |  |

#### `M_MOTION_DETECT_OUTPUT`

Inquires whether the output images will be the deinterlaced images or images indicating which pixels are considered to be part of an object in motion (the internal motion detection mask).

| Value | Description |
| --- | --- |
| *(see [`M_MOTION_DETECT_OUTPUT`](Reference/im/MimControl.md))* |  |

#### `M_MOTION_DETECT_REFERENCE_FRAME`

Inquires the index of the frame to use for deinterlacing within the group of frames that are used for motion detection.

| Value | Description |
| --- | --- |
| *(see [`M_MOTION_DETECT_REFERENCE_FRAME`](Reference/im/MimControl.md))* |  |

#### `M_MOTION_DETECT_THRESHOLD`

Inquires the threshold value used for motion detection.

| Value | Description |
| --- | --- |
| *(see [`M_MOTION_DETECT_THRESHOLD`](Reference/im/MimControl.md))* |  |

#### `M_SOURCE_FIRST_IMAGE`

Inquires the index of the input image used to generate the first deinterlaced image.

| Value | Description |
| --- | --- |
| *(see [`M_SOURCE_FIRST_IMAGE`](Reference/im/MimControl.md))* |  |

---

### `Filter majority image processing context ID`

Specifies a filter majority image processing context identifier, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_FILTER_MAJORITY_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md)operations.

#### `M_IGNORED_VALUE`

Inquires the pixel value to ignore. If a pixel value in the neighborhood matches the ignored value, it will not be counted; the pixel is essentially excluded from the neighborhood. This value is only ignored if [`M_USE_IGNORED_VALUE`](../../Reference/im/MimInquire.md)is enabled.

| Value | Description |
| --- | --- |
| *(see [`M_IGNORED_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_TIE_BREAKER_MODE`

Inquires the method used to break a tie in the neighborhood.

| Value | Description |
| --- | --- |
| *(see [`M_TIE_BREAKER_MODE`](Reference/im/MimControl.md))* |  |

#### `M_USE_FREQUENCY_TIE_BREAKER`

Inquires whether the control type [`M_TIE_BREAKER_MODE`](../../Reference/im/MimControl.md) should break a tie in the neighborhood using the values present or, if weights are specified, using the non-weighted frequency at which the tied values appear.

| Value | Description |
| --- | --- |
| *(see [`M_USE_FREQUENCY_TIE_BREAKER`](Reference/im/MimControl.md))* |  |

#### `M_USE_IGNORED_VALUE`

Inquires if Aurora Imaging Library should ignore the value in the source image set by [`M_IGNORED_VALUE`](../../Reference/im/MimControl.md) when calling [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md).

| Value | Description |
| --- | --- |
| *(see [`M_USE_IGNORED_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_WEIGHT_IMAGE_ATTRIBUTE`

Inquires the attributes of the weight image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the weight image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_WEIGHT_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies a bit-encoded value that indicates the image buffer's attributes, set at allocation. |

#### `M_WEIGHT_IMAGE_HEIGHT`

Inquires the height of the weight image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the weight image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_WEIGHT_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the height of the weight image, in pixels. |

#### `M_WEIGHT_IMAGE_NB_BANDS`

Inquires the number of bands of the weight image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the weight image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_WEIGHT_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the number of bands. |

#### `M_WEIGHT_IMAGE_TYPE`

Inquires the bit depth and date type of the weight image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the weight image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_WEIGHT_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the bit depth and data type. |

#### `M_WEIGHT_IMAGE_WIDTH`

Inquires the width of the weight image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the weight image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_WEIGHT_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the width of the weight image, in pixels. |

---

### `Find orientation image processing context ID`

Specifies a find orientation image processing context identifier, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_FIND_ORIENTATION_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) operations.

#### `M_BORDER_ATTENUATION`

Inquires whether [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) must process the image's borders, or if the operation can ignore noise occurring close to the ends of the image buffer.

| Value | Description |
| --- | --- |
| *(see [`M_BORDER_ATTENUATION`](Reference/im/MimControl.md))* |  |

#### `M_FREQUENCY_CUTOFF_RATIO_HIGH`

Inquires the upper limit of frequencies in which to look for dominant orientations, as a percentage of the maximum frequency. Larger images will have higher maximum frequencies. Frequencies higher than this percentage will be ignored during computation. This can be useful for discarding noise in the image.

| Value | Description |
| --- | --- |
| *(see [`M_FREQUENCY_CUTOFF_RATIO_HIGH`](Reference/im/MimControl.md))* |  |

#### `M_FREQUENCY_CUTOFF_RATIO_LOW`

Inquires the lower limit of frequencies in which to look for dominant orientations, as a percentage of the maximum frequency. Larger images will have higher maximum frequencies. Frequencies lower than this percentage will be ignored during computation. This can be useful for discarding noise in the image.

| Value | Description |
| --- | --- |
| *(see [`M_FREQUENCY_CUTOFF_RATIO_LOW`](Reference/im/MimControl.md))* |  |

#### `M_INTERPOLATION_MODE`

Inquires the interpolation mode for the resizing used if the source image is of an inappropriate size. This will only be used if the source image has dimensions that are not a power of 2 (X-size and Y-size).

| Value | Description |
| --- | --- |
| *(see [`M_INTERPOLATION_MODE`](Reference/im/MimControl.md))* |  |

#### `M_MODE`

Inquires the resizing mode used if the source image is of an inappropriate size. Resizing will only occur if the source image has dimensions that are not a power of 2 (X-size and Y-size). The find orientation operation is then performed on the resized image, which is stored in a temporary image buffer. The original image is not altered.

| Value | Description |
| --- | --- |
| *(see [`M_MODE`](Reference/im/MimControl.md))* |  |

---

### `Flat-field image processing context ID`

Specifies a flat-field image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_FLAT_FIELD_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimFlatField`](../../Reference/im/MimFlatField.md) operations.

#### `M_DARK_CONST`

Inquires the dark constant value. This is used to remove thermal agitation recorded in the grabbed image (from the CCD) and to remove the darkest possible shade of black when removing uneven lighting from grabbed images.

| Value | Description |
| --- | --- |
| *(see [`M_DARK_CONST`](Reference/im/MimControl.md))* |  |
| `M_INVALID_CONST` | Specifies that the constant has not been set. |

#### `M_DARK_IMAGE_HEIGHT`

Inquires the height of the dark image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the dark image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DARK_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_DARK_CONST`](../../Reference/im/MimControl.md) is set. |
| `Value` | Specifies the height, in pixels. |

#### `M_DARK_IMAGE_NB_BANDS`

Inquires the number of bands of the dark image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DARK_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_DARK_CONST`](../../Reference/im/MimControl.md) is set. |
| `Value` | Specifies the number of bands. |

#### `M_DARK_IMAGE_TYPE`

Inquires a combination of two values: data type and data depth of the dark image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DARK_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_DARK_CONST`](../../Reference/im/MimControl.md) is set. |
| `M_UNSIGNED + 8` | Specifies 8-bit unsigned data. |
| `M_UNSIGNED + 16` | Specifies 16-bit unsigned data. |

#### `M_DARK_IMAGE_WIDTH`

Inquires the width of the dark image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DARK_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_DARK_CONST`](../../Reference/im/MimControl.md) is set. |
| `Value` | Specifies the width, in pixels. |

#### `M_EFFECTIVE_GAIN_CONST`

Inquires the gain factor used during the last call to [`MimFlatField`](../../Reference/im/MimFlatField.md) (when [`M_GAIN_CONST`](../../Reference/im/MimControl.md) set to [`M_AUTOMATIC`](../../Reference/im/MimControl.md)). If the gain factor was set manually, inquire the value using [`M_GAIN_CONST`](../../Reference/im/MimInquire.md).

| Value | Description |
| --- | --- |
| `M_INVALID_CONST` | Specifies that the gain factor has not been set. |
| `Value > 0` | Specifies the automatically calculated gain factor. |

#### `M_FLAT_CONST`

Inquires the flat constant value. This is used to remove the variations of CCD sensitivity recorded in the grabbed image (from the CCD) and to reduce the gray in the grabbed image when removing uneven lighting.

| Value | Description |
| --- | --- |
| *(see [`M_FLAT_CONST`](Reference/im/MimControl.md))* |  |
| `M_INVALID_CONST` | Specifies that the constant has not been set. |

#### `M_FLAT_IMAGE_HEIGHT`

Inquires the height of the flat image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_FLAT_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_FLAT_CONST`](../../Reference/im/MimControl.md) is set. |
| `Value` | Specifies the height, in pixels. |

#### `M_FLAT_IMAGE_NB_BANDS`

Inquires the number of bands in the flat image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_FLAT_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_FLAT_CONST`](../../Reference/im/MimControl.md) is set. |
| `Value` | Specifies the number of bands. |

#### `M_FLAT_IMAGE_TYPE`

Inquires the identifier of the flat image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_FLAT_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_FLAT_CONST`](../../Reference/im/MimControl.md) is set. |
| `M_UNSIGNED + 8` | Specifies 8-bit unsigned data. |
| `M_UNSIGNED + 16` | Specifies 16-bit unsigned data. |

#### `M_FLAT_IMAGE_WIDTH`

Inquires the width of the flat image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_FLAT_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_FLAT_CONST`](../../Reference/im/MimControl.md) is set. |
| `Value` | Specifies the width, in pixels. |

#### `M_GAIN_CONST`

Inquires the gain factor used to normalize (or scale) the result of the flat field calculation back to the full dynamic range of the destination image.

| Value | Description |
| --- | --- |
| *(see [`M_GAIN_CONST`](Reference/im/MimControl.md))* |  |
| `M_AUTOMATIC` | Specifies that the gain factor has been set to be automatically calculated. To inquire the automatically calculated value, use [`M_EFFECTIVE_GAIN_CONST`](../../Reference/im/MimInquire.md). |
| `M_INVALID_CONST` | Specifies that the gain factor has not been set. |

#### `M_OFFSET_CONST`

Inquires the offset constant value. This is used to remove the electrical bias recorded in the grabbed image (from the CCD) and to reduce the black in the flat image when removing uneven lighting.

| Value | Description |
| --- | --- |
| *(see [`M_OFFSET_CONST`](Reference/im/MimControl.md))* |  |
| `M_INVALID_CONST` | Specifies that the constant has not been set. |

#### `M_OFFSET_IMAGE_HEIGHT`

Inquires the height of the offset image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_OFFSET_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_OFFSET_CONST`](../../Reference/im/MimControl.md) is set. |
| `Value` | Specifies the height, in pixels. |

#### `M_OFFSET_IMAGE_NB_BANDS`

Inquires the number of bands in the offset image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_OFFSET_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_OFFSET_CONST`](../../Reference/im/MimControl.md) is set. |
| `Value` | Specifies the number of bands. |

#### `M_OFFSET_IMAGE_TYPE`

Inquires a combination of two values: data type and data depth of the offset image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_OFFSET_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_OFFSET_CONST`](../../Reference/im/MimControl.md) is set. |
| `M_UNSIGNED + 8` | Specifies 8-bit unsigned data. |
| `M_UNSIGNED + 16` | Specifies 16-bit unsigned data. |

#### `M_OFFSET_IMAGE_WIDTH`

Inquires the width of the offset image buffer.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_OFFSET_IMAGE`](../../Reference/im/MimControl.md). This value is also returned when [`M_OFFSET_CONST`](../../Reference/im/MimControl.md) is set. |
| `Value` | Specifies the width, in pixels. |

---

### `Histogram equalization adaptive context ID`

Specifies a histogram equalization adaptive context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_HISTOGRAM_EQUALIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) operations.

#### `M_ALPHA_VALUE`

Inquires the adjustment factor for [`M_EXPONENTIAL`](../../Reference/im/MimInquire.md) and [`M_RAYLEIGH`](../../Reference/im/MimInquire.md) operations.

| Value | Description |
| --- | --- |
| *(see [`M_ALPHA_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_CLIP_LIMIT`

Inquires the maximum percentage of values that a tile's histogram bin can represent.

| Value | Description |
| --- | --- |
| *(see [`M_CLIP_LIMIT`](Reference/im/MimControl.md))* |  |

#### `M_HIST_SIZE`

Inquires the number of bins for each tile's histogram.

| Value | Description |
| --- | --- |
| *(see [`M_HIST_SIZE`](Reference/im/MimControl.md))* |  |

#### `M_NUMBER_OF_TILES_X`

Inquires the number of tiles along the X-direction of the source image specified with [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md).

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_TILES_X`](Reference/im/MimControl.md))* |  |

#### `M_NUMBER_OF_TILES_Y`

Inquires the number of tiles along the Y-direction of the source image specified with [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md).

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_TILES_Y`](Reference/im/MimControl.md))* |  |

#### `M_OPERATION`

Inquires the equalization operation that [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) uses.

| Value | Description |
| --- | --- |
| *(see [`M_OPERATION`](Reference/im/MimControl.md))* |  |

---

### `Linear IIR filter image processing context ID`

Specifies a linear IIR filter image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimConvolve`](../../Reference/im/MimConvolve.md) and [`MimDifferential`](../../Reference/im/MimDifferential.md) operations.

#### `M_FILTER_DEFAULT_SHARPEN_PARAM`

Inquires the default sharpening operation value established by Aurora Imaging Library ([`MimDifferential`](../../Reference/im/MimDifferential.md)).

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the sharpening value. |

#### `M_FILTER_OPERATION`

Inquires the linear filter operation to perform ([`MimConvolve`](../../Reference/im/MimConvolve.md)).

| Value | Description |
| --- | --- |
| *(see [`M_FILTER_OPERATION`](Reference/im/MimControl.md))* |  |

#### `M_FILTER_RESPONSE_TYPE`

Inquires the filter response type ([`MimConvolve`](../../Reference/im/MimConvolve.md)).

| Value | Description |
| --- | --- |
| *(see [`M_FILTER_RESPONSE_TYPE`](Reference/im/MimControl.md))* |  |

#### `M_FILTER_SMOOTHNESS`

Inquires the degree of smoothness (strength of the denoising) to apply, according to the filter smoothness type ([`MimConvolve`](../../Reference/im/MimConvolve.md)).

| Value | Description |
| --- | --- |
| *(see [`M_FILTER_SMOOTHNESS`](Reference/im/MimControl.md))* |  |

#### `M_FILTER_SMOOTHNESS_TYPE`

Inquires the filter smoothness type ([`MimConvolve`](../../Reference/im/MimConvolve.md)).

| Value | Description |
| --- | --- |
| *(see [`M_FILTER_SMOOTHNESS_TYPE`](Reference/im/MimControl.md))* |  |

#### `M_FILTER_TYPE`

Inquires the type of predefined IIR filter to use for the convolution operation ([`MimConvolve`](../../Reference/im/MimConvolve.md)).

| Value | Description |
| --- | --- |
| *(see [`M_FILTER_TYPE`](Reference/im/MimControl.md))* |  |

---

### `Locate difference image processing result ID`

Specifies a locate difference image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md), and used in [`MimLocateDifference`](../../Reference/im/MimLocateDifference.md) operations.

#### `M_RESULT_OUTPUT_UNITS`

Inquires whether results are returned in pixel or world units.

| Value | Description |
| --- | --- |
| *(see [`M_RESULT_OUTPUT_UNITS`](Reference/im/MimControl.md))* |  |

---

### `Locate peak 1D image processing context ID`

Specifies a 1D locate peak image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_LOCATE_PEAK_1D_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) operations.

#### `M_FRAME_SIZE`

Inquires the Y-size of the frames.

| Value | Description |
| --- | --- |
| *(see [`M_FRAME_SIZE`](Reference/im/MimControl.md))* |  |

#### `M_MINIMUM_CONTRAST`

Inquires the minimum contrast.

| Value | Description |
| --- | --- |
| *(see [`M_MINIMUM_CONTRAST`](Reference/im/MimControl.md))* |  |

#### `M_NUMBER_OF_FRAMES`

Inquires the total number of frames for which peaks can be extracted.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_FRAMES`](Reference/im/MimControl.md))* |  |

#### `M_NUMBER_OF_PEAKS`

Inquires the maximum number of peaks to find along a given lane. If more than the specified number of peaks are found, the highest peaks are kept.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_PEAKS`](Reference/im/MimControl.md))* |  |

#### `M_PEAK_INTENSITY_RANGE`

Inquires the number of pixels used to calculate the average peak intensity.

| Value | Description |
| --- | --- |
| *(see [`M_PEAK_INTENSITY_RANGE`](Reference/im/MimControl.md))* |  |

#### `M_PEAK_WIDTH_DELTA`

Inquires the peak width delta.

| Value | Description |
| --- | --- |
| *(see [`M_PEAK_WIDTH_DELTA`](Reference/im/MimControl.md))* |  |

#### `M_PEAK_WIDTH_NOMINAL`

Inquires the peak width nominal.

| Value | Description |
| --- | --- |
| *(see [`M_PEAK_WIDTH_NOMINAL`](Reference/im/MimControl.md))* |  |

#### `M_SCAN_LANE_DIRECTION`

Inquires the direction in which to detect peaks.

| Value | Description |
| --- | --- |
| *(see [`M_SCAN_LANE_DIRECTION`](Reference/im/MimControl.md))* |  |

---

### `Match image processing context ID`

Specifies a match image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_MATCH_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimMatch`](../../Reference/im/MimMatch.md) operations.

#### `M_MASK_IMAGE_ATTRIBUTE`

Inquires the attribute of the mask image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the mask image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MASK_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies a bit-encoded value that indicates the image buffer's attributes, set at allocation. |

#### `M_MASK_IMAGE_HEIGHT`

Inquires the height of the mask image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the mask image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MASK_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the height, in pixels. |

#### `M_MASK_IMAGE_NB_BANDS`

Inquires the number of bands of the mask image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the mask image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MASK_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the number of bands. |

#### `M_MASK_IMAGE_TYPE`

Inquires the bit depth and data type of the mask image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the mask image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MASK_IMAGE`](../../Reference/im/MimControl.md). |
| `depth value + M_FLOAT` | Specifies the data depth and that the data type is floating-point. |
| `depth value + M_SIGNED` | Specifies the data depth and that the data type is signed. |
| `depth value + M_UNSIGNED` | Specifies the data depth and that the data type is unsigned. |

#### `M_MASK_IMAGE_WIDTH`

Inquires the width of the mask image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the mask image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MASK_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the width, in pixels. |

#### `M_MAX_SCORE`

Inquires the specified maximum score used to remap normalized correlation.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_SCORE`](Reference/im/MimControl.md))* |  |

#### `M_MODE`

Inquires the type of algorithm used when performing the match.

| Value | Description |
| --- | --- |
| *(see [`M_MODE`](Reference/im/MimControl.md))* |  |

#### `M_MODEL_IMAGE_ATTRIBUTE`

Inquires the attributes of the internal buffer of the model image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the model image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODEL_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies a bit-encoded value that indicates the image buffer's attributes, set at allocation. |

#### `M_MODEL_IMAGE_HEIGHT`

Inquires the height of the model image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the model image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODEL_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the height, in pixels. |

#### `M_MODEL_IMAGE_NB_BANDS`

Inquires the number of bands of the model image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the model image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODEL_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the number of bands. |

#### `M_MODEL_IMAGE_TYPE`

Inquires the bit depth and data type of the model image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the model image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODEL_IMAGE`](../../Reference/im/MimControl.md). |
| `M_UNSIGNED + 8` | Specifies 8-bit unsigned data. |
| `M_UNSIGNED + 16` | Specifies 16-bit unsigned data. |

#### `M_MODEL_IMAGE_WIDTH`

Inquires the width of the model image.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the model image buffer has not been set, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODEL_IMAGE`](../../Reference/im/MimControl.md). |
| `Value` | Specifies the width, in pixels. |

#### `M_MODEL_STEP`

Inquires whether every pixel or every other pixel in the model image is used when matching the source image to the model image.

| Value | Description |
| --- | --- |
| *(see [`M_MODEL_STEP`](Reference/im/MimControl.md))* |  |

#### `M_SCORE_TYPE`

Inquires how the final correlation score is computed.

| Value | Description |
| --- | --- |
| *(see [`M_SCORE_TYPE`](Reference/im/MimControl.md))* |  |

---

### `Rearrangement image processing context ID`

Specifies a rearrangement image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_REARRANGE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimRearrange`](../../Reference/im/MimRearrange.md) operations.

#### `M_MODE`

Inquires the processing mode.

| Value | Description |
| --- | --- |
| *(see [`M_MODE`](Reference/im/MimControl.md))* |  |
| `M_INVALID` | Specifies that the processing mode has not been set. |

#### `M_XY_DESTINATION_ARRAY_SIZE`

Inquires the size of the arrays containing the X- and Y-offsets of the areas in the destination image buffer in which to copy the source areas.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size of the arrays. |

#### `M_XY_SIZE_ARRAY_SIZE`

Inquires the size of the arrays containing the width and height of the areas to copy.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size of the arrays. |

#### `M_XY_SOURCE_ARRAY_SIZE`

Inquires the size of the arrays containing the X- and Y-offsets of the areas to copy from the source image buffer into the destination image buffer.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size of the arrays. |

---

### `Remap image processing context ID`

Specifies a remap image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_IM_REMAP_CONTEXT`](../../Reference/im/MimAlloc.md), and used in[`MimRemap`](../../Reference/im/MimRemap.md) operations.

#### `M_DST_END_VALUE`

Inquires the end pixel value used to compute the destination range.

| Value | Description |
| --- | --- |
| *(see [`M_DST_END_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_DST_RANGE`

Inquires whether to use the destination image buffer or the remap image processing context to determine the destination range.

| Value | Description |
| --- | --- |
| *(see [`M_DST_RANGE`](Reference/im/MimControl.md))* |  |

#### `M_DST_START_VALUE`

Inquires the start pixel value used to compute the destination range.

| Value | Description |
| --- | --- |
| *(see [`M_DST_START_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_REMAP_FUNCTION`

Inquires the function used to map the source image buffer to the destination image buffer.

| Value | Description |
| --- | --- |
| *(see [`M_REMAP_FUNCTION`](Reference/im/MimControl.md))* |  |

#### `M_SRC_END_VALUE`

Inquires the end pixel value used to compute the source range.

| Value | Description |
| --- | --- |
| *(see [`M_SRC_END_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_SRC_RANGE`

Inquires whether to use the source image buffer or the remap image processing context to determine the source range.

| Value | Description |
| --- | --- |
| *(see [`M_SRC_RANGE`](Reference/im/MimControl.md))* |  |

#### `M_SRC_START_VALUE`

Inquires the start pixel value used to compute the source range.

| Value | Description |
| --- | --- |
| *(see [`M_SRC_START_VALUE`](Reference/im/MimControl.md))* |  |

---

### `Statistics image processing context ID`

Specifies a statistics image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md), and used by[`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) as the operation to perform.

#### `M_COND_HIGH`

Inquires the upper limit of the selected condition (inquired using [`M_CONDITION`](../../Reference/im/MimInquire.md)) that establishes which pixels to include in the statistical operation(s).

| Value | Description |
| --- | --- |
| *(see [`M_COND_HIGH`](Reference/im/MimControl.md))* |  |

#### `M_COND_LOW`

Inquires the lower limit of the selected condition (inquired using [`M_CONDITION`](../../Reference/im/MimInquire.md)) that establishes which pixels to include in the statistical operation(s).

| Value | Description |
| --- | --- |
| *(see [`M_COND_LOW`](Reference/im/MimControl.md))* |  |

#### `M_CONDITION`

Inquires the condition for the statistical computation that establishes which pixels to include in the statistical operation(s).

| Value | Description |
| --- | --- |
| *(see [`M_CONDITION`](Reference/im/MimControl.md))* |  |

#### `M_GLCM_PAIR_OFFSET_X`

Inquires the displacement in X between pixel pairs used to generate the co-occurrence matrix.

| Value | Description |
| --- | --- |
| *(see [`M_GLCM_PAIR_OFFSET_X`](Reference/im/MimControl.md))* |  |

#### `M_GLCM_PAIR_OFFSET_Y`

Inquires the displacement in Y between pixel pairs used to generate the co-occurrence matrix.

| Value | Description |
| --- | --- |
| *(see [`M_GLCM_PAIR_OFFSET_Y`](Reference/im/MimControl.md))* |  |

#### `M_GLCM_QUANTIFICATION`

Inquires the depth of the gray level of the co-occurrence matrix.

| Value | Description |
| --- | --- |
| *(see [`M_GLCM_QUANTIFICATION`](Reference/im/MimControl.md))* |  |

#### `M_PREPROCESSED`

Inquires whether the statistics image processing context was preprocessed (that is, whether [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md)was called).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the statistics image processing context was not preprocessed. |
| `M_TRUE` | Specifies that the statistics image processing context was preprocessed. |

#### `M_SOURCE_SIZE_X`

Inquires the width of a typical source image.

| Value | Description |
| --- | --- |
| *(see [`M_SOURCE_SIZE_X`](Reference/im/MimControl.md))* |  |
| `M_INVALID` | Specifies that the width has not been set. |

#### `M_SOURCE_SIZE_Y`

Inquires the height of a typical source image.

| Value | Description |
| --- | --- |
| *(see [`M_SOURCE_SIZE_Y`](Reference/im/MimControl.md))* |  |
| `M_INVALID` | Specifies that the height has not been set. |

#### `M_STAT_ANGULAR_DATA_COHERENCE`

Inquires whether to calculate the coherence of the angular data.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_ANGULAR_DATA_COHERENCE`](Reference/im/MimControl.md))* |  |

#### `M_STAT_ANGULAR_DATA_MEAN`

Inquires whether to calculate the dominant direction of the unit vectors.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_ANGULAR_DATA_MEAN`](Reference/im/MimControl.md))* |  |

#### `M_STAT_GLCM_CONTRAST`

Inquires whether to calculate the contrast of all the grayscale pixel values within a specified neighborhood in relation to the normalized co-occurrence probability of each pixel.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_GLCM_CONTRAST`](Reference/im/MimControl.md))* |  |

#### `M_STAT_GLCM_CORRELATION`

Inquires whether to calculate the correlation within the normalized co-occurrence probability in a specified neighborhood.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_GLCM_CORRELATION`](Reference/im/MimControl.md))* |  |

#### `M_STAT_GLCM_DISSIMILARITY`

Inquires whether to calculate the dissimilarity between the normalized co-occurrence probability and the gray level co-occurrence matrix of a specified neighborhood.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_GLCM_DISSIMILARITY`](Reference/im/MimControl.md))* |  |

#### `M_STAT_GLCM_ENERGY`

Inquires whether to calculate the energy between two points in the gray level co-occurrence matrix of a given neighborhood.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_GLCM_ENERGY`](Reference/im/MimControl.md))* |  |

#### `M_STAT_GLCM_ENTROPY`

Inquires whether to calculate the inverse of the energy between two points in the normalized co-occurrence probability of a specified neighborhood.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_GLCM_ENTROPY`](Reference/im/MimControl.md))* |  |

#### `M_STAT_GLCM_HOMOGENEITY`

Inquires whether to calculate the similarity between the normalized co-occurrence probability and the gray level co-occurrence matrix of a given neighborhood.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_GLCM_HOMOGENEITY`](Reference/im/MimControl.md))* |  |

#### `M_STAT_MAX`

Inquires whether to calculate the maximum pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MAX`](Reference/im/MimControl.md))* |  |

#### `M_STAT_MAX_ABS`

Inquires whether to calculate the maximum absolute pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MAX_ABS`](Reference/im/MimControl.md))* |  |

#### `M_STAT_MEAN`

Inquires whether to calculate the mean value of the pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MEAN`](Reference/im/MimControl.md))* |  |

#### `M_STAT_MIN`

Inquires whether to calculate the minimum pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MIN`](Reference/im/MimControl.md))* |  |

#### `M_STAT_MIN_ABS`

Inquires whether to calculate the minimum absolute pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_MIN_ABS`](Reference/im/MimControl.md))* |  |

#### `M_STAT_NUMBER`

Inquires whether to keep track of the number of pixels, from the source image, that satisfied the condition specified when calling [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md).

| Value | Description |
| --- | --- |
| *(see [`M_STAT_NUMBER`](Reference/im/MimControl.md))* |  |

#### `M_STAT_ORIENTATION_DATA_COHERENCE`

Inquires whether to calculate the coherence of the orientation data.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_ORIENTATION_DATA_COHERENCE`](Reference/im/MimControl.md))* |  |

#### `M_STAT_ORIENTATION_DATA_MEAN`

Inquires whether to calculate the dominant orientation of the unit vectors.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_ORIENTATION_DATA_MEAN`](Reference/im/MimControl.md))* |  |

#### `M_STAT_STANDARD_DEVIATION`

Inquires whether to calculate the standard deviation from the value of each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_STANDARD_DEVIATION`](Reference/im/MimControl.md))* |  |

#### `M_STAT_SUM`

Inquires whether to calculate the sum of the pixel values from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_SUM`](Reference/im/MimControl.md))* |  |

#### `M_STAT_SUM_ABS`

Inquires whether to calculate the sum of the absolute pixel values from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_SUM_ABS`](Reference/im/MimControl.md))* |  |

#### `M_STAT_SUM_OF_SQUARES`

Inquires whether to calculate the sum of the squared pixel values from each pixel location across the source image.

| Value | Description |
| --- | --- |
| *(see [`M_STAT_SUM_OF_SQUARES`](Reference/im/MimControl.md))* |  |

#### `M_STEP_SIZE_X`

Inquires the distance between the neighborhood along the X-axis in which to evaluate the specified statistic.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_SIZE_X`](Reference/im/MimControl.md))* |  |

#### `M_STEP_SIZE_Y`

Inquires the distance between the neighborhood along the Y-axis in which to evaluate the specified statistic.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_SIZE_Y`](Reference/im/MimControl.md))* |  |

#### `M_TILE_SIZE_X`

Inquires the X-size (width) of the neighborhood in which to evaluate the specified statistic.

| Value | Description |
| --- | --- |
| *(see [`M_TILE_SIZE_X`](Reference/im/MimControl.md))* |  |
| `M_INVALID_CONST` | Specifies that the constant ([`M_TILE_SIZE_X`](../../Reference/im/MimControl.md)) has not been set. |

#### `M_TILE_SIZE_Y`

Inquires the Y-size (height) of the neighborhood in which to evaluate the specified statistic.

| Value | Description |
| --- | --- |
| *(see [`M_TILE_SIZE_Y`](Reference/im/MimControl.md))* |  |
| `M_INVALID_CONST` | Specifies that the constant ([`M_TILE_SIZE_Y`](../../Reference/im/MimControl.md)) has not been set. |

---

### `Unwarp along path image processing context ID`

Specifies an unwarp along image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_UNWARP_ALONG_PATH_CONTEXT`](../../Reference/im/MimAlloc.md), and used by[`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) as the operation to perform.

#### `M_DESTINATION_SIZE_X`

Inquires the width of the destination buffer if one was specified when calling [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) with [`M_PREPROCESS`](../../Reference/im/MimUnwarpAlongPath.md). If a destination buffer was not specified, this is the length (X-size) of the path, rounded up to the nearest integer.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the width of the destination buffer. |

#### `M_DESTINATION_SIZE_Y`

Inquires the height of the destination buffer if one was specified when calling [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) with [`M_PREPROCESS`](../../Reference/im/MimUnwarpAlongPath.md). If a destination buffer was not specified, this is the width (Y-size) of the path, rounded up to the nearest integer, set using [`MimControl`](../../Reference/im/MimControl.md) with [`M_SINGLE_WIDTH`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the height of the destination buffer. |

#### `M_INTERPOLATION_MODE`

Inquires the interpolation mode used when unwarping along the path.

| Value | Description |
| --- | --- |
| *(see [`M_INTERPOLATION_MODE`](Reference/im/MimControl.md))* |  |

#### `M_MODE`

Inquires the type of path. This path must be set using [`MimPut`](../../Reference/im/MimPut.md).

| Value | Description |
| --- | --- |
| *(see [`M_MODE`](Reference/im/MimControl.md))* |  |

#### `M_OVERSCAN`

Inquires how to handle pixels that map outside of the source image buffer.

| Value | Description |
| --- | --- |
| *(see [`M_OVERSCAN`](Reference/im/MimControl.md))* |  |

#### `M_PATH_ARRAY_SIZE`

Inquires the required array size to store the values retrieved using [`MimGet`](../../Reference/im/MimGet.md) with [`M_XY_PATH`](../../Reference/im/MimGet.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size of the array. |

#### `M_SINGLE_WIDTH`

Inquires the single width for all points along the path.

| Value | Description |
| --- | --- |
| *(see [`M_SINGLE_WIDTH`](Reference/im/MimControl.md))* |  |

#### `M_UNWARP_SAMPLING_MODE`

Inquires which algorithm to use for the unwarp along path operation.

| Value | Description |
| --- | --- |
| *(see [`M_UNWARP_SAMPLING_MODE`](Reference/im/MimControl.md))* |  |

#### `M_WORK_PATH_ARRAY_SIZE`

Inquires the required array size to store the values retrieved using [`MimGet`](../../Reference/im/MimGet.md) with [`M_XY_WORK_PATH`](../../Reference/im/MimGet.md), [`M_XY_WORK_TOP_PATH`](../../Reference/im/MimGet.md), and [`M_XY_WORK_BOTTOM_PATH`](../../Reference/im/MimGet.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size of the array. |

---

### `Wavelet image processing context ID`

Specifies a wavelet image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md) or [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) operations. Unless otherwise specified, inquire types apply to both [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md) and [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md) context types.

#### `M_FILTER_FORWARD_HIGH_PASS_ID`

Inquires the identifier of the internal buffer containing the values used for the high-pass filter of a forward transformation.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that there is no information to inquire about. |
| `Kernel Buffer ID` | Specifies the identifier of the kernel buffer containing the filter values. |

#### `M_FILTER_FORWARD_LOW_PASS_ID`

Inquires the identifier of the internal buffer containing the values used for the low-pass filter of a forward transformation.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that there is no information to inquire about. |
| `Kernel Buffer ID` | Specifies the identifier of the kernel buffer containing the filter values. |

#### `M_FILTER_REVERSE_HIGH_PASS_ID`

Inquires the identifier of the internal buffer containing the values used for the high-pass filter of a reverse transformation.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that there is no information to inquire about. |
| `Kernel Buffer ID` | Specifies the identifier of the kernel buffer containing the filter values. |

#### `M_FILTER_REVERSE_LOW_PASS_ID`

Inquires the identifier of the internal buffer containing the values used for the low-pass filter of a reverse transformation.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that there is no information to inquire about. |
| `Kernel Buffer ID` | Specifies the identifier of the kernel buffer containing the filter values. |

#### `M_TRANSFORMATION_DOMAIN`

Inquires whether the mathematical domain of the filter specified to perform the wavelet transformation consists of complex numbers or only real numbers. Complex numbers have an imaginary part (imaginary numbers), in addition to a real part (real numbers).

| Value | Description |
| --- | --- |
| `M_COMPLEX` | Specifies that the mathematical domain of the filter consists of real and imaginary numbers. Such transformations use a complex type of wavelet filter ([`MimControl`](../../Reference/im/MimControl.md) with [`M_WAVELET_TYPE`](../../Reference/im/MimControl.md)), or a custom wavelet filter containing real and imaginary numbers ([`MimWaveletSetFilter`](../../Reference/im/MimWaveletSetFilter.md)). |
| `M_REAL` | Specifies that the mathematical domain of the filter consists of only real numbers. Such transformations use a non-complex type of wavelet filter ([`MimControl`](../../Reference/im/MimControl.md) with [`M_WAVELET_TYPE`](../../Reference/im/MimControl.md)), or a custom wavelet filter containing real numbers only ([`MimWaveletSetFilter`](../../Reference/im/MimWaveletSetFilter.md)). |

#### `M_TRANSFORMATION_MODE`

Inquires the wavelet transformation mode.

| Value | Description |
| --- | --- |
| *(see [`M_TRANSFORMATION_MODE`](Reference/im/MimControl.md))* |  |

#### `M_WAVELET_CONTEXT_TYPE`

Inquires the type of wavelet transformation context allocated.

| Value | Description |
| --- | --- |
| `M_CUSTOM` | Specifies that the wavelet context was allocated using[`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md). |
| `M_PREDEFINED` | Specifies that the wavelet context was allocated using [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md). |

#### `M_WAVELET_SIZE`

Inquires the size of the wavelet defined in the context. If the context type is [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md), the size depends on the specified [`M_WAVELET_TYPE`](../../Reference/im/MimControl.md) control. If the context type is [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md), the size depends on the filters specified with [`MimWaveletSetFilter`](../../Reference/im/MimWaveletSetFilter.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size of the wavelet. |

#### `M_WAVELET_TYPE`

Inquires the type of wavelet filter used by the wavelet transformation context. This setting is only available for an [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md) context type.

| Value | Description |
| --- | --- |
| *(see [`M_WAVELET_TYPE`](Reference/im/MimControl.md))* |  |
| `M_CUSTOM` | Specifies a custom wavelet filter. This indicates that you are using an [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md) context type. Custom wavelet filters are set with [`MimWaveletSetFilter`](../../Reference/im/MimWaveletSetFilter.md). |

### Combination Constants — For inquiring the priority (order) with which

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the priority (order) with which [`MimAugment`](../../Reference/im/MimAugment.md) performs the corresponding operation.

#### `M_PRIORITY`

Inquires the priority with which [`MimAugment`](../../Reference/im/MimAugment.md) performs the corresponding operation.

| Value | Description |
| --- | --- |
| `Value >= 0` *(default)* | Specifies the priority (order). |

### Combination Constants — For inquiring the probability that

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the probability that [`MimAugment`](../../Reference/im/MimAugment.md) performs the corresponding operation.

#### `M_PROBABILITY`

Inquires the probability that [`MimAugment`](../../Reference/im/MimAugment.md) performs the corresponding operation.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the probability, as a percentage. |

### Combination Constants — For specifying whether undecimated wavelet transformations are centered

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify undecimated wavelet transformations that are centered.

| Value | Description |
| --- | --- |
| `M_CENTER` | Specifies undecimated wavelet transformations that are centered. |

### Combination Constants — For specifying which internal buffer to inquire about (real or imaginary numbers)

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify which internal buffer to inquire about (real or imaginary numbers).

| Value | Description |
| --- | --- |
| `M_IMAGINARY_PART` | Inquires about the internal buffer containing the imaginary part of the values used by the wavelet filter. |
| `M_REAL_PART` *(default)* | Inquires about the internal buffer containing the real part of the values used by the wavelet filter. |

### For inquiring about a specific type of image processing result buffer

For a specific type of image processing result buffer, the [`ContextOrResultImId`](../../Reference/im/MimInquire.md) and [`InquireType`](../../Reference/im/MimInquire.md) parameters can be set to one of the following:

---

### `Event list image processing result ID`

Specifies an event list image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md), and used in [`MimLocateEvent`](../../Reference/im/MimLocateEvent.md) operations.

#### `M_RESULT_OUTPUT_UNITS`

Inquires whether results are returned in pixel or world units.

| Value | Description |
| --- | --- |
| *(see [`M_RESULT_OUTPUT_UNITS`](Reference/im/MimControl.md))* |  |

---

### `Intensity histogram image processing result ID`

Specifies an intensity histogram image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md), and used in [`MimHistogram`](../../Reference/im/MimHistogram.md) operations.

#### `M_HIST_BIN_SIZE_MODE`

Inquires the number of values each histogram bin can hold.

| Value | Description |
| --- | --- |
| *(see [`M_HIST_BIN_SIZE_MODE`](Reference/im/MimControl.md))* |  |

#### `M_HIST_SMOOTHING_ITERATIONS`

Inquires the number of smoothing iterations to perform on the histogram after it has been generated.

| Value | Description |
| --- | --- |
| *(see [`M_HIST_SMOOTHING_ITERATIONS`](Reference/im/MimControl.md))* |  |

---

### `Locate peak 1D image processing result ID`

Specifies a 1D locate peak image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_LOCATE_PEAK_1D_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) operations.

#### `M_PEAK_INTENSITY_INVALID_VALUE`

Inquires the intensity value to use for missing peaks.

| Value | Description |
| --- | --- |
| *(see [`M_PEAK_INTENSITY_INVALID_VALUE`](Reference/im/MimControl.md))* |  |

#### `M_SORT_CRITERION`

Inquires the characteristic with which to sort peaks.

| Value | Description |
| --- | --- |
| *(see [`M_SORT_CRITERION`](Reference/im/MimControl.md))* |  |

### Combination Constants — For specifying the order to sort peaks

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the order to sort peaks.

| Value | Description |
| --- | --- |
| `M_SORT_DOWN` | Sorts peaks in descending order. |
| `M_SORT_UP` | Sorts peaks in ascending order. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_. Note that [`M_TYPE_AIL_ID`](../../Reference/im/MimInquire.md) should only be used with [`M_OWNER_SYSTEM`](../../Reference/im/MimInquire.md).

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, if it is an_AIL_INT_. If the requested information is an _AIL_DOUBLE_ or if it is an_AIL_INT64_ on a 32-bit system, this function will return `M_NULL`.

## Remarks

> Note that some of the values listed above are not available in Aurora Imaging Library Lite. See the value's corresponding operation function for Aurora Imaging Library Lite availability.
