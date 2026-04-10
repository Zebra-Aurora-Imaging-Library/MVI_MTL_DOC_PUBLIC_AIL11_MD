---
doctype: Reference
module: im
function: MimGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimGetResult"
---

# MimGetResult

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

> Get the specified type of results from an image processing result buffer.

## Syntax

```c
void MimGetResult(
    AIL_ID    ResultImId,   //in
    AIL_INT64 ResultType,   //in
    void *    UserArrayPtr  //out
)
```

## Description

This function retrieves all the results of the specified type from the specified result buffer and stores them in the specified one-dimensional destination user array. Results are only available after calling [`MimCountDifference`](../../Reference/im/MimCountDifference.md), [`MimFindExtreme`](../../Reference/im/MimFindExtreme.md), [`MimHistogram`](../../Reference/im/MimHistogram.md), [`MimLocateEvent`](../../Reference/im/MimLocateEvent.md), [`MimLocateDifference`](../../Reference/im/MimLocateDifference.md), [`MimProjection`](../../Reference/im/MimProjection.md), [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md), or [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md).

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

> **Note:** If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MimControl`](../../Reference/im/MimControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/im/MimControl.md) control type set to [`M_PIXEL`](../../Reference/im/MimControl.md). However, note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/im/MimControl.md) to [`M_WORLD`](../../Reference/im/MimControl.md) without specifying a calibrated image in which to calculate the results, [`MimGetResult`](../../Reference/im/MimGetResult.md) will generate an error.

If your results were calculated using [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md), the [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) buffer contains results from an operation performed on a single image ([`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md) context), or from cumulative operations performed on numerous images ([`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md) context). Single image results are calculated from all pixels, a region of interest (ROI), or tiled regions of the single image; whereas, cumulative results are calculated for the same pixel location across multiple images. Use [`MimGetResult2d`](../../Reference/im/MimGetResult2d.md) to obtain most cumulative results. Note that [`M_STAT_NUMBER`](../../Reference/im/MimGetResult.md) and [`M_TOTAL_NUMBER`](../../Reference/im/MimGetResult.md) are available for cumulative results using [`MimGetResult`](../../Reference/im/MimGetResult.md). Also use [`MimGetResult`](../../Reference/im/MimGetResult.md) to retrieve statistics results from a single image divided into tiles (specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_TILE_SIZE...`](../../Reference/im/MimControl.md) and [`M_STEP_SIZE...`](../../Reference/im/MimControl.md)). For a tiled image, one result per tile is returned for most of the supported statistics.

## Parameters

### `ResultImId` *(in, AIL_ID)*

Specifies the identifier of the image processing result buffer from which to retrieve results.

### `ResultType` *(in, AIL_INT64)*

Specifies the type of results to retrieve.

### `UserArrayPtr` *(out, *void)*

Specifies the address of the one-dimensional array in which to write the results read from the Aurora Imaging Library result buffer.

## Parameter Associations

---

### `M_AUG_ASPECT_RATIO`

Retrieves the aspect ratio applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_ASPECT_RATIO_MODE`

Retrieves the mode of the aspect ratio applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_ASPECT_RATIO_OP_MODE`](Reference/im/MimControl.md))* |  |

---

### `M_AUG_BLUR_MOTION_ANGLE`

Retrieves the motion blur angle applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_BLUR_MOTION_SIZE`

Retrieves the size of the motion blur kernel applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_CROP_BOTTOM_RIGHT_X`

Retrieves the X-coordinate of the bottom-right corner of the cropping applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_CROP_BOTTOM_RIGHT_Y`

Retrieves the Y-coordinate of the bottom-right corner of the cropping applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_CROP_TOP_LEFT_X`

Retrieves the X-coordinate of the top-left corner of the cropping applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_CROP_TOP_LEFT_Y`

Retrieves the Y-coordinate of the top-left corner of the cropping applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_DILATION_ASYM_NB_ITERATIONS`

Retrieves the number of iterations of the asymmetrical dilation applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_DILATION_ASYM_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_DILATION_ASYM_SUBAREA`

Retrieves the area selected for the asymmetrical dilation applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_DILATION_ASYM_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_LEFT_HALF` | Specifies the left half of the image. |
| `M_LOWER_HALF` | Specifies the lower half of the image. |
| `M_RIGHT_HALF` | Specifies the right half of the image. |
| `M_UPPER_HALF` | Specifies the upper half of the image. |

---

### `M_AUG_DILATION_NB_ITERATIONS`

Retrieves the number of iterations of the dilation applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_DILATION_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_EROSION_ASYM_NB_ITERATIONS`

Retrieves the number of iterations of the asymmetrical erosion applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_EROSION_ASYM_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_EROSION_ASYM_SUBAREA`

Retrieves the area selected for the asymmetrical erosion applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_EROSION_ASYM_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_LEFT_HALF` | Specifies the left half of the image. |
| `M_LOWER_HALF` | Specifies the lower half of the image. |
| `M_RIGHT_HALF` | Specifies the right half of the image. |
| `M_UPPER_HALF` | Specifies the upper half of the image. |

---

### `M_AUG_EROSION_NB_ITERATIONS`

Retrieves the number of iterations of the erosion applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_EROSION_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_FLIP_DIRECTION`

Retrieves the direction of the flip applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_FLIP_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_AUG_FLIP_OP_DIRECTION`](Reference/im/MimControl.md))* |  |

---

### `M_AUG_GAMMA_VALUE_BAND_0`

Retrieves the value of the gamma correction for band 0 applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_GAMMA_VALUE_BAND_1`

Retrieves the value of the gamma correction for band 1 applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_GAMMA_VALUE_BAND_2`

Retrieves the value of the gamma correction for band 2 applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_GAMMA_VALUE_BAND + n`

Retrieves the value of the gamma correction for band _n_ applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md). Note that _n_ is an integer between 0 and the value returned by [`M_AUG_GAMMA_VALUE_NB_BANDS`](../../Reference/im/MimGetResult.md) - 1.

---

### `M_AUG_GAMMA_VALUE_NB_BANDS`

Retrieves the number of bands for which gamma correction was applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_HSV_VALUE_GAIN`

Retrieves the value with which the value band (in the HSV color space) was multiplied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_HSV_VALUE_GAIN_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_HUE_OFFSET`

Retrieves the angular value added to the hue band (in the HSV color space) by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_HUE_OFFSET_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_INTENSITY_ADD_VALUE`

Retrieves the value of the intensity addition (offset) applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_INTENSITY_MULTIPLY_VALUE`

Retrieves the value of the intensity multiplication (gain) applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_LIGHTING_DIRECTIONAL_ANGLE`

Retrieves the angle of the directional lighting (add-ramp) applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_NOISE_GAUSSIAN_ADDITIVE_STDDEV`

Retrieves the standard deviation of the Gaussian additive noise applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_NOISE_MULTIPLICATIVE_STDDEV`

Retrieves the standard deviation of the multiplicative noise applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_NOISE_SALT_PEPPER_DENSITY`

Retrieves the density of the salt and pepper noise applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_NOISE_SALT_PEPPER_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_OPERATION_ASSOCIATED_WITH_RESULT_TYPES`

Retrieves the list of the operations, and their associated result types, that [`MimAugment`](../../Reference/im/MimAugment.md) applied.

---

### `M_AUG_OPERATION_RESULT_TYPES`

Retrieves the list of the result types that are related to the operations that [`MimAugment`](../../Reference/im/MimAugment.md) applied.

---

### `M_AUG_OPERATION_RESULT_VALUES`

Retrieves the list of the values that were used for the operations that [`MimAugment`](../../Reference/im/MimAugment.md) applied.

---

### `M_AUG_OPERATIONS_APPLIED`

Retrieves a list of **M_TRUE** and **M_FALSE** values, indicating whether the corresponding enabled operations, returned by [`M_AUG_OPERATIONS_ENABLED`](../../Reference/im/MimGetResult.md), were actually applied by [`MimAugment`](../../Reference/im/MimAugment.md). For example, if [`M_AUG_OPERATIONS_APPLIED`](../../Reference/im/MimGetResult.md) returns [**M_TRUE**,**M_TRUE**,**M_FALSE**] and [`M_AUG_OPERATIONS_ENABLED`](../../Reference/im/MimGetResult.md) returns [[`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md),[`M_AUG_SCALE_OP`](../../Reference/im/MimControl.md),[`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimControl.md)], it means that [`MimAugment`](../../Reference/im/MimAugment.md)applied the rotation and scale operations, but did not apply the aspect ratio operation.  The order of the **M_TRUE** and **M_FALSE** values that [`M_AUG_OPERATIONS_APPLIED`](../../Reference/im/MimGetResult.md) returns is the same as the order of the operations returned by [`M_AUG_OPERATIONS_ENABLED`](../../Reference/im/MimGetResult.md).

---

### `M_AUG_OPERATIONS_ENABLED`

Retrieves the list of the operations that were enabled for [`MimAugment`](../../Reference/im/MimAugment.md). Operations are enabled using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_..._OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_OPTIMAL_SIZE_X`

Retrieves the suggested optimal width of an image buffer in which to produce all possible augmentation results. To establish this width, Aurora Imaging Library considers every image processing operation that is currently enabled for the augmentation ([`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_..._OP`](../../Reference/im/MimControl.md)). The optimal image buffer size can be useful, for example, when drawing the resulting image of the augmentation ([`M_DRAW_AUG_IMAGE`](../../Reference/im/MimDraw.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the width, in pixels. |

---

### `M_AUG_OPTIMAL_SIZE_Y`

Retrieves the suggested optimal height of an image buffer in which to produce all possible augmentation results. To establish this height, Aurora Imaging Library considers every image processing operation that is currently enabled for the augmentation ([`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_..._OP`](../../Reference/im/MimControl.md)). The optimal image buffer size can be useful, for example, when drawing the resulting image of the augmentation ([`M_DRAW_AUG_IMAGE`](../../Reference/im/MimDraw.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the height, in pixels. |

---

### `M_AUG_REDUCE_FACTOR`

Retrieves the reducing scale factor applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_REDUCE_OFFSET_X`

Retrieves the X offset applied to by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_REDUCE_OFFSET_Y`

Retrieves the Y offset applied to by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_ROTATION_ANGLE`

Retrieves the rotation angle applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_SATURATION_GAIN`

Retrieves the value with which the saturation band (in the HSV color space) was multiplied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SATURATION_GAIN_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_SCALE_FACTOR`

Retrieves the scale applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SCALE_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_SEED_USED`

Retrieves the value (seed) used to generate the randomized settings to perform the augmentation.

| Value | Description |
| --- | --- |
| `Value` | Specifies the seed. |

---

### `M_AUG_SHARPEN_DERICHE_VALUE`

Retrieves the value of the Deriche sharpening filter applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SHARPEN_DERICHE_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_SHEAR_X`

Retrieves the horizontal direction of the shear applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SHEAR_X_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_SHEAR_Y`

Retrieves the vertical direction of the shear applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SHEAR_Y_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_SMOOTH_DERICHE_VALUE`

Retrieves the value of the Deriche filter smoothing applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SMOOTH_DERICHE_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_SMOOTH_GAUSSIAN_STDDEV`

Retrieves the standard deviation of the Gaussian smoothing applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SMOOTH_GAUSSIAN_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_TRANSLATION_X`

Retrieves the horizontal translation applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_TRANSLATION_X_OP`](../../Reference/im/MimControl.md).

---

### `M_AUG_TRANSLATION_Y`

Retrieves the vertical translation applied by [`MimAugment`](../../Reference/im/MimAugment.md), from an [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This result's related operation is specified using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_TRANSLATION_Y_OP`](../../Reference/im/MimControl.md).

---

### `M_CUMULATIVE_VALUE`

Retrieves the cumulative histogram values from an [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_HIST_REAL_SIZE`

Retrieves the number of values (entries) used by the histogram, from an [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_HIST_VALUE_OFFSET`

Retrieves the intensity value in the source that corresponds to the first bin (entry 0) in the histogram, from an [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md) result buffer. This result depends on the specified histogram mode, set using [`MimControl`](../../Reference/im/MimControl.md) with [`M_HIST_BIN_SIZE_MODE`](../../Reference/im/MimControl.md). This result returns an error if you set the mode to [`M_REGULAR`](../../Reference/im/MimControl.md).

---

### `M_HIST_VALUE_RANGE`

Retrieves the number of intensity values in the source that were used to generate the histogram, from an [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md) result buffer. This result depends on the specified histogram mode, set using [`MimControl`](../../Reference/im/MimControl.md) with [`M_HIST_BIN_SIZE_MODE`](../../Reference/im/MimControl.md). This result returns an error if you set the mode to [`M_REGULAR`](../../Reference/im/MimControl.md).

---

### `M_NB_EVENT`

Retrieves the number of events from an [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md) result buffer. Note that if the result buffer did not have the required number of entries to hold the total number of events that satisfied the condition, the number returned is limited to the number of entries in the result buffer. In this case, results from a multi-core processing application might differ from run to run as results are added globally from the multiple threads.

---

### `M_NUMBER`

Retrieves the number of differences from an [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. Note that if the result buffer did not have the required number of entries to hold the total number of differences that satisfied the condition, the number returned is limited to the number of entries in the result buffer. In this case, results from a multi-core processing application might differ from run to run as results are added globally from the multiple threads.

---

### `M_NUMBER_OF_LEVELS`

Retrieves the total number of transformation levels calculated by the wavelet transformation, from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_ORIGINAL_IMAGE_SIZE_X`

Retrieves the unaltered width of the image used by the wavelet transformation, from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_ORIGINAL_IMAGE_SIZE_Y`

Retrieves the unaltered height of the image used by the wavelet transformation, from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_PERCENTILE_VALUE`

Retrieves the inverse cumulative histogram from an [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md) result buffer. Each value retrieved is the pixel value whereby _n_% of the pixels have a value less than or equal to that pixel value, where _n_ is the index of the user array.

---

### `M_POSITION_X`

Retrieves the image X-coordinate of each event from an [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md) or [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_POSITION_Y`

Retrieves the image Y-coordinate of each event from an [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md) or [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_SIZE_X`

Retrieves the number of tiles along the X-dimension, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_SIZE_Y`

Retrieves the number of tiles along the Y-dimension, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_SRC1_VALUE`

Retrieves source 1 values for each differences from [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_SRC2_VALUE`

Retrieves source 2 values for each differences from [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_ANGULAR_DATA_COHERENCE`

Retrieves the coherence, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer, of pixels representing angles and as such, treated as unit vectors. Coherence represents the directional trend of a group of vectors and is a measure of how parallel or coherent these vectors are with respect to each other.

---

### `M_STAT_ANGULAR_DATA_MEAN`

Retrieves the angle of the vector sum, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer, of pixels representing angles, and as such, treated as unit vectors. The angular component of a vector sum, calculated from the vector addition of unit vectors, is a representation of the average or mean angle of the unit vectors.

---

### `M_STAT_GLCM_CONTRAST`

Retrieves the contrast of all the grayscale pixel values within a specified neighborhood in relation to the normalized co-occurrence probability of each pixel, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_GLCM_CORRELATION`

Retrieves the correlation within the normalized co-occurrence probability in a specified neighborhood, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_GLCM_DISSIMILARITY`

Retrieves the dissimilarity between the normalized co-occurrence probability and the gray level co-occurrence matrix of a specified neighborhood.

---

### `M_STAT_GLCM_ENERGY`

Retrieves the energy between two points in the gray level co-occurrence matrix of a given neighborhood, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_GLCM_ENTROPY`

Retrieves the inverse of the energy between two points in the normalized co-occurrence probability of a specified neighborhood, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_GLCM_HOMOGENEITY`

Retrieves the similarity between the normalized co-occurrence probability and the gray level co-occurrence matrix of a given neighborhood, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_MAX`

Retrieves the maximum pixel value from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_MAX_ABS`

Retrieves the maximum absolute pixel value from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_MEAN`

Retrieves the mean pixel value from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_MIN`

Retrieves the minimum pixel value from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_MIN_ABS`

Retrieves the minimum absolute pixel value from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_NUMBER`

Retrieves the number of pixels or the number of images, depending on the context type.  For an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer used with an [`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md), this value represents the number of pixels that satisfied the condition specified when calling [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md).  For an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer used with an [`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md), this value represents the number of images used to calculate the current result. This does not include images that have previously been removed from the calculation.

---

### `M_STAT_ORIENTATION_DATA_COHERENCE`

Retrieves the coherence of the orientation data from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_ORIENTATION_DATA_MEAN`

Retrieves the dominant orientation of the unit vectors from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_STANDARD_DEVIATION`

Retrieves the pixel standard deviation value from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_SUM`

Retrieves the sum of the pixel values from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_SUM_ABS`

Retrieves the sum of the absolute pixel values from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_STAT_SUM_OF_SQUARES`

Retrieves the sum of the squared pixel values from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_TOTAL_NUMBER`

Retrieves the total number of images that were used to calculate the results in an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. This number is the sum of [`M_STAT_NUMBER`](../../Reference/im/MimGetResult.md) and all the images that have previously been removed from the calculation using [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) with [`M_REMOVE`](../../Reference/im/MimStatCalculate.md).  This is only available for an result buffer whose results were obtained using an [`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md) context.

---

### `M_TRANSFORMATION_DOMAIN`

Retrieves whether the mathematical domain of the resulting wavelet transformation values consist of complex numbers or only real numbers, from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. Complex numbers have an imaginary part (imaginary numbers), in addition to a real part (real numbers).

| Value | Description |
| --- | --- |
| `M_COMPLEX` | Specifies that the mathematical domain of the values consist of real and imaginary numbers. Such transformations come from using a complex type of wavelet filter ([`MimControl`](../../Reference/im/MimControl.md) with [`M_WAVELET_TYPE`](../../Reference/im/MimControl.md)), or a custom wavelet filter containing real and imaginary numbers ([`MimWaveletSetFilter`](../../Reference/im/MimWaveletSetFilter.md)). |
| `M_REAL` | Specifies that the mathematical domain of the values consist of only real numbers. Such transformations come from using a non-complex type of wavelet filter ([`MimControl`](../../Reference/im/MimControl.md) with [`M_WAVELET_TYPE`](../../Reference/im/MimControl.md)), or a custom wavelet filter containing real numbers only ([`MimWaveletSetFilter`](../../Reference/im/MimWaveletSetFilter.md)). |

---

### `M_TRANSFORMATION_MODE`

Retrieves the wavelet transformation mode, from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

| Value | Description |
| --- | --- |
| *(see [`M_TRANSFORMATION_MODE`](Reference/im/MimControl.md))* |  |

---

### `M_VALID`

Retrieves the validity of projection data for each lane, from an [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md)result buffer. This is only useful when a region of interest is associated with the source image buffer of the projection operation.  A lane of pixels will contain no valid pixels if it is excluded from the region of interest associated with the source image; this means that projection data cannot be calculated for the lane. However, the [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md)result buffer contains an entry for each lane of pixels found in the source image, regardless of the use of a region of interest. So when your source image buffer has a region of interest, you should use [`M_VALID`](../../Reference/im/MimGetResult.md)to check the validity of the data for the lane in the result buffer.  Note that for an [`M_SUM`](../../Reference/im/MimProjection.md)projection operation, if a lane of pixels does not contain valid projection data, a neutral pixel value of 0 is used for that lane. In this case, [`M_VALID`](../../Reference/im/MimGetResult.md)will indicate valid projection data for all lanes of pixels in the image.

| Value | Description |
| --- | --- |
| `0` | Specifies invalid projection data for the lane due to no valid pixels residing in the lane of pixels in the source image corresponding to the [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md) entry. |
| `Value != 0` | Specifies valid projection data for the lane. |

---

### `M_VALUE`

Retrieves values from an [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md), [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md), [`M_EXTREME_LIST`](../../Reference/im/MimAllocResult.md), [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md), or [`M_COUNT_LIST`](../../Reference/im/MimAllocResult.md) result buffer.  If an [`M_EXTREME_LIST`](../../Reference/im/MimAllocResult.md) type result buffer contains both the minimum and maximum value of an image, the minimum is written first, followed by the maximum.

---

### `M_WAVELET_DRAW_SIZE_X`

Retrieves the width required to draw, for every level calculated, all wavelet coefficient results (diagonal, horizontal, vertical, and the approximation), from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. You can perform the related drawing operation using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WAVELET`](../../Reference/im/MimDraw.md).

---

### `M_WAVELET_DRAW_SIZE_X_WITH_PADDING`

Retrieves the width required to draw, for every level calculated, all wavelet coefficient results (diagonal, horizontal, vertical, and the approximation), from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. You can perform the related drawing operation using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WAVELET_WITH_PADDING`](../../Reference/im/MimDraw.md).

---

### `M_WAVELET_DRAW_SIZE_Y`

Retrieves the height required to draw, for every level calculated, all wavelet coefficient results (diagonal, horizontal, vertical, and the approximation), from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. You can perform the related drawing operation using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WAVELET`](../../Reference/im/MimDraw.md).

---

### `M_WAVELET_DRAW_SIZE_Y_WITH_PADDING`

Retrieves the height required to draw, for every level calculated, all wavelet coefficient results (diagonal, horizontal, vertical, and the approximation), from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. You can perform the related drawing operation using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WAVELET_WITH_PADDING`](../../Reference/im/MimDraw.md).

---

### `M_WAVELET_SIZE`

Retrieves the size of the wavelet transformation, from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_WAVELET_TYPE`

Retrieves the type of the wavelet transformation, from an [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

| Value | Description |
| --- | --- |
| *(see [`M_WAVELET_TYPE`](Reference/im/MimControl.md))* |  |
| `M_CUSTOM` | Specifies a custom wavelet processing. |

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For retrieving results as a percentage

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the requested results as a percentage.

#### `M_PERCENTAGE`

Retrieves the histogram values or cumulative histogram values, as a percentage. The values are multiplied by 100 and then divided by the total number of elements in the histogram.

### Combination Constants — For specifying whether undecimated wavelet transformations are centered

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine if undecimated wavelet transformations are centered.

| Value | Description |
| --- | --- |
| `M_CENTER` | Specifies undecimated wavelet transformations that are centered. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

## Remarks

> Note that some of the values listed above are not available in Aurora Imaging Library Lite. See the result's corresponding operation function for Aurora Imaging Library Lite availability.

The returned list is sorted according to the priority that [`MimAugment`](../../Reference/im/MimAugment.md) uses to perform the specified image processing operations.
