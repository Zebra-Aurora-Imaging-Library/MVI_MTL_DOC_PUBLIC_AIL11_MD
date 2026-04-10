---
doctype: Reference
module: im
function: MimControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimControl"
---

# MimControl

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

> Control an image processing context or result buffer setting.

## Syntax

```c
void MimControl(
    AIL_ID     ContextOrResultImId,  //out
    AIL_INT64  ControlType,          //in
    AIL_DOUBLE ControlValue          //in
)
```

## Description

This function allows you to control an image processing context or result buffer setting. All the control type settings can be inquired using [`MimInquire`](../../Reference/im/MimInquire.md).

## Parameters

### `ContextOrResultImId` *(out, AIL_ID)*

Specifies the identifier of the image processing context or result buffer. The image processing context or result buffer must have been previously allocated on the system using [`MimAlloc`](../../Reference/im/MimAlloc.md) or [`MimAllocResult`](../../Reference/im/MimAllocResult.md), respectively.

### `ControlType` *(in, AIL_INT64)*

Specifies the processing feature to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value needed for the control. When the [`ControlValue`](../../Reference/im/MimControl.md) is an image buffer, the internal representation of the image stored in the image processing context might not be the same as the original image.

## Parameter Associations

### For specifying the control type and control value for an image processing context or result buffer

The following [`ContextOrResultImId`](../../Reference/im/MimControl.md), [`ControlType`](../../Reference/im/MimControl.md), and [`ControlValue`](../../Reference/im/MimControl.md) parameter settings can be specified for different types of image processing contexts or result buffers.

---

### `Adaptive binarize context ID`

Specifies an adaptive binarize context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) operations.  The main setting with which to control an adaptive binarize context is [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md). In general, all other control settings are used by the specified thresholding process to establish the threshold values with which to binarize.

#### `M_AVERAGE_MODE`

Sets how Aurora Imaging Library establishes average pixel values that can be required to determine threshold values. This is typically used when [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md) is set to [`M_NIBLACK`](../../Reference/im/MimControl.md)or [`M_LOCAL_MEAN`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GAUSSIAN` | Specifies a Gaussian type average. |
| `M_UNIFORM` *(default)* | Specifies a uniform type average. |

#### `M_FOREGROUND_VALUE`

Sets whether the objects to binarize are lighter or darker than the background.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FOREGROUND_BLACK` | Specifies that the objects to binarize are darker than the background. |
| `M_FOREGROUND_WHITE` *(default)* | Specifies that the objects to binarize are lighter than the background. |

#### `M_GLOBAL_MAX`

Sets the maximum threshold value. The threshold destination image ([`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md)) cannot hold values higher than [`M_GLOBAL_MAX`](../../Reference/im/MimInquire.md). Higher threshold values are clipped.  If the source image ([`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md)) also has a maximum value restriction ([`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MAX`](../../Reference/buf/MbufControl.md)), Aurora Imaging Library uses the lower maximum value as the actual maximum.  Before calling [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md), if both [`M_GLOBAL_MAX`](../../Reference/im/MimInquire.md) and [`M_GLOBAL_MIN`](../../Reference/im/MimInquire.md)are set to values, then [`M_GLOBAL_MAX`](../../Reference/im/MimInquire.md) must be greater than or equal to [`M_GLOBAL_MIN`](../../Reference/im/MimInquire.md).  By default, Aurora Imaging Library binarizes pixels with an intensity higher than the maximum as part of the foreground (object). To change this behavior, use the [`M_FOREGROUND_VALUE`](../../Reference/im/MimControl.md) control.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that there is no maximum threshold value restriction imposed by [`M_GLOBAL_MAX`](../../Reference/im/MimInquire.md). |
| `Value` | Specifies the maximum threshold value. |

#### `M_GLOBAL_MIN`

Sets the minimum threshold value. The threshold destination image ([`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md)) cannot hold values lower than [`M_GLOBAL_MIN`](../../Reference/im/MimInquire.md). Lower threshold values are clipped.  If the source image ([`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md)) also has a minimum value restriction ([`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MIN`](../../Reference/buf/MbufControl.md)), Aurora Imaging Library uses the greater minimum value as the actual maximum.  Before calling [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md), if both [`M_GLOBAL_MIN`](../../Reference/im/MimInquire.md) and [`M_GLOBAL_MAX`](../../Reference/im/MimInquire.md) are set to values, then [`M_GLOBAL_MIN`](../../Reference/im/MimInquire.md)must be less than or equal to[`M_GLOBAL_MAX`](../../Reference/im/MimInquire.md).  By default, Aurora Imaging Library binarizes pixels with an intensity lower than the minimum as part of the background. To change this behavior, use the [`M_FOREGROUND_VALUE`](../../Reference/im/MimControl.md) control.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that there is no minimum threshold value restriction imposed by [`M_GLOBAL_MIN`](../../Reference/im/MimInquire.md). |
| `Value` | Specifies the minimum threshold value. |

#### `M_GLOBAL_OFFSET`

Sets the offset to add to each threshold value. [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) uses the adjusted threshold values. The specified offset is reflected in the threshold destination image ([`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the offset. |

#### `M_GLOBAL_OFFSET_SECOND_PASS`

Sets the offset to apply to the threshold values for the second pass of a hysteresis adaptive binarization. Aurora Imaging Library applies the offset for the second pass in the same way that it applies the offset for the first pass ([`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md)). For [`M_GLOBAL_OFFSET_SECOND_PASS`](../../Reference/im/MimControl.md) to have an effect, you must specify a threshold type other than [`M_SINGLE`](../../Reference/im/MimControl.md).  For an [`M_NIBLACK`](../../Reference/im/MimControl.md) threshold mode, either [`M_GLOBAL_OFFSET_SECOND_PASS`](../../Reference/im/MimControl.md) must have a value different from [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md), or [`M_NIBLACK_BIAS_SECOND_PASS`](../../Reference/im/MimControl.md) must have a value different from [`M_NIBLACK_BIAS`](../../Reference/im/MimControl.md); otherwise, Aurora Imaging Library generates an error. For other threshold modes, Aurora Imaging Library generates an error if [`M_GLOBAL_OFFSET_SECOND_PASS`](../../Reference/im/MimControl.md) and [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md) have the same value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the offset. |

#### `M_LOCAL_DIMENSION`

Specifies the size of the neighborhood that the threshold mode uses to establish threshold values.  For an [`M_NIBLACK`](../../Reference/im/MimControl.md) or [`M_LOCAL_MEAN`](../../Reference/im/MimControl.md) threshold mode, the size should be the largest square that represents a uniform background. The size should also be greater than the object's expected thickness. For an [`M_BERNSEN`](../../Reference/im/MimControl.md) threshold mode, the size should be slightly larger than half the object's expected thickness. For an [`M_PSEUDOMEDIAN`](../../Reference/im/MimControl.md) threshold mode, the size should be half the object's expected thickness.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the size of the neighborhood, in pixels. Only _integer_ values accepted. |

#### `M_MINIMUM_CONTRAST`

Sets the minimum contrast between background and foreground (object) pixels. Aurora Imaging Library binarizes (classifies) pixels in a neighborhood as background if they do not meet the minimum contrast. An [`M_LOCAL_MEAN`](../../Reference/im/MimControl.md) threshold mode ignores the minimum contrast.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum contrast. |

#### `M_NIBLACK_BIAS`

Sets the bias for Niblack's binarization mode. This value only has an effect if [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md) is set to [`M_NIBLACK`](../../Reference/im/MimControl.md).  The bias gives you some general control over thresholding. A higher bias binarizes fainter values as part of the object. A lower bias binarizes fainter values as part of the background.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the bias. Typical values range from 0.1 to 0.3. |

#### `M_NIBLACK_BIAS_SECOND_PASS`

Sets the bias for the second pass of a Niblack adaptive binarization that uses a hysteresis process. For this control type to have an effect, you must specify an [`M_NIBLACK`](../../Reference/im/MimControl.md) threshold mode and you must specify a threshold type other than [`M_SINGLE`](../../Reference/im/MimControl.md).  Either [`M_NIBLACK_BIAS_SECOND_PASS`](../../Reference/im/MimControl.md) must have a value different from [`M_NIBLACK_BIAS`](../../Reference/im/MimControl.md), or [`M_GLOBAL_OFFSET_SECOND_PASS`](../../Reference/im/MimControl.md) must have a value different from [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md); otherwise, Aurora Imaging Library generates an error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the bias. |

#### `M_THRESHOLD_MODE`

Sets how Aurora Imaging Library establishes the threshold values with which to binarize the source image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BERNSEN` | Specifies that threshold values are established using Bernsen's adaptive threshold algorithm. This represents a type of morphological erosion and dilation. This threshold results in the fastest process. |
| `M_LOCAL_MEAN` | Specifies that threshold values are established using adaptive local mean calculations. This is a simplified version of [`M_NIBLACK`](../../Reference/im/MimControl.md). [`M_LOCAL_MEAN`](../../Reference/im/MimControl.md) usually results in a faster, though less precise, binarization than [`M_NIBLACK`](../../Reference/im/MimControl.md). |
| `M_NIBLACK` *(default)* | Specifies that threshold values are established using Niblack's adaptive threshold algorithm. This setting offers the highest precision. The processing time is usually quite quick. |
| `M_PSEUDOMEDIAN` | Specifies that threshold values are established using adaptive pseudomedian calculations. This is similar to an [`M_BERNSEN`](../../Reference/im/MimControl.md) threshold, except it represents a type of morphological open or close process instead of erosion or dilation. |

#### `M_THRESHOLD_TYPE`

Sets the type of threshold to use for the adaptive binarization.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HYSTERESIS` | Specifies that the adaptive binarization uses a hysteresis process. In this case, Aurora Imaging Library performs a geodesic reconstruction (a type of morphological operation) after a second pass of the specified threshold.  Regardless of the threshold mode, Aurora Imaging Library uses [`M_GLOBAL_OFFSET_SECOND_PASS`](../../Reference/im/MimControl.md) instead of [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md) during the second pass of [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md). For an [`M_NIBLACK`](../../Reference/im/MimControl.md) threshold mode, the second pass uses [`M_NIBLACK_BIAS_SECOND_PASS`](../../Reference/im/MimControl.md) instead of [`M_NIBLACK_BIAS`](../../Reference/im/MimControl.md). Aurora Imaging Library generates an error if every [`M_..._SECOND_PASS`](../../Reference/im/MimControl.md) value it uses is the same as its first pass counterpart. |
| `M_IN_RANGE` | Specifies to use the values inside the range defined by the two passes as the foreground. |
| `M_OUT_RANGE` | Specifies to use the values outside the range defined by the two passes as the foreground. |
| `M_SINGLE` *(default)* | Specifies to use a single pass of the specified threshold. Hysteresis is not used. |

---

### `Adaptive binarize from seed context ID`

Specifies an adaptive binarize context that uses seeds, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) operations.  The main setting with which to control an adaptive binarize context that uses seeds is [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md). In general, all other control settings are used by the specified thresholding process to establish the threshold values with which to binarize.

#### `M_FOREGROUND_VALUE`

Sets whether the objects to binarize are lighter or darker than the background.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FOREGROUND_BLACK` | Specifies that the objects to binarize are darker than the background. |
| `M_FOREGROUND_WHITE` *(default)* | Specifies that the objects to binarize are lighter than the background. |

#### `M_GLOBAL_OFFSET`

Sets the offset to add to each established threshold value. Binarization uses the adjusted threshold values, however offsets do not change the threshold values themselves. The content of the threshold destination image ([`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) with [`ThresholdImageBufId`](../../Reference/im/MimBinarizeAdaptive.md)) remains unaltered.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the offset. |

#### `M_NB_ITERATIONS`

Sets the number of times to perform the adaptive threshold process specified with [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_TO_IDEMPOTENCE` *(default)* | Specifies that the threshold process iterates until idempotence is reached. This is the number of iterations at which subsequent iterations do not alter results. |
| `Value > 0` | Specifies the number of iterations. Only _integer_ values accepted. The threshold process for an [`M_TOGGLE`](../../Reference/im/MimControl.md) threshold mode is always performed once. |

#### `M_NB_SEED_ITERATIONS`

Sets the number of iterations with which to internally establish the seeds that the threshold mode requires. This value only has an effect if you do not specify your own seed images with [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of iterations. Only _integer_ values accepted. |

#### `M_THRESHOLD_MODE`

Sets how Aurora Imaging Library uses seeds to establish the threshold values with which to binarize the source image. You can provide the required seed images when you call [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md). If you do not, Aurora Imaging Library internally establishes the seed data.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LEVEL` | Specifies that threshold values are established using an adaptive leveling. This essentially performs two geodesic reconstructions. One that processes the foreground as white, and the other that processes the foreground as black. This results in [`M_LEVEL`](../../Reference/im/MimControl.md) generally taking twice as long as [`M_RECONSTRUCT`](../../Reference/im/MimControl.md). [`M_LEVEL`](../../Reference/im/MimControl.md) uses one seed image. |
| `M_RECONSTRUCT` *(default)* | Specifies that threshold values are established using an adaptive geodesic reconstruction. This represents a type of morphological erosion or dilation. [`M_RECONSTRUCT`](../../Reference/im/MimControl.md) uses one seed image. [`M_RECONSTRUCT`](../../Reference/im/MimControl.md)is typically faster than [`M_LEVEL`](../../Reference/im/MimControl.md) and slower than [`M_TOGGLE`](../../Reference/im/MimControl.md). |
| `M_TOGGLE` | Specifies that threshold values are established as one of two possibilities, defined by the seeds. Aurora Imaging Library compares the source pixel (including the offset) to each seed. The value of the closest seed is the threshold value for that pixel.[`M_TOGGLE`](../../Reference/im/MimControl.md) uses two seed images (typically min and max values). [`M_TOGGLE`](../../Reference/im/MimControl.md) is the fastest threshold mode. |

---

### `Augmentation context ID`

Specifies an augmentation context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_AUGMENTATION_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimAugment`](../../Reference/im/MimAugment.md) operations.

#### `M_AUG_ASPECT_RATIO_OP`

Sets whether to enable the aspect ratio operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_ASPECT_RATIO_OP_MAX`

Sets the maximum aspect ratio, to apply for [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 16.0` *(default)* | Specifies the maximum aspect ratio. |

#### `M_AUG_ASPECT_RATIO_OP_MIN`

Sets the minimum aspect ratio, to apply for [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 16.0` *(default)* | Specifies the minimum aspect ratio. |

#### `M_AUG_ASPECT_RATIO_OP_MODE`

Sets the aspect ratio mode, to apply for [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BOTH` *(default)* | Specifies that Aurora Imaging Library sets the aspect ratio mode. That is, either [`M_INVERT`](../../Reference/im/MimControl.md) or [`M_NORMAL`](../../Reference/im/MimControl.md) is randomly set. |
| `M_INVERT` | Specifies to apply the aspect ratio as height/width. In this case, the width stays the same while the height changes. |
| `M_NORMAL` | Specifies to apply the aspect ratio as width/height. In this case, the height stays the same while the width changes. |

#### `M_AUG_BLUR_MOTION_OP`

Sets whether to enable the operation that applies a motion blur kernel with random direction and kernel size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_BLUR_MOTION_OP_ANGLE_MAX`

Sets the maximum value for the motion blur angle's random distribution [min, max], to apply for [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the maximum value for the motion blur angle's random distribution [min, max]. |

#### `M_AUG_BLUR_MOTION_OP_ANGLE_MIN`

Sets the minimum value for the motion blur angle's random distribution [min, max], to apply for [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the minimum value for the motion blur angle's random distribution [min, max]. |

#### `M_AUG_BLUR_MOTION_OP_SIZE_MAX`

Sets the maximum value for the kernel size's random distribution [min, max], to apply for [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `5 <= Value <= 15` *(default)* | Specifies the maximum value for the kernel size's random distribution. |

#### `M_AUG_BLUR_MOTION_OP_SIZE_MIN`

Sets the minimum value for the kernel size's random distribution [min, max], to apply for [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `5 <= Value <= 15` *(default)* | Specifies the min value for the kernel size's random distribution. |

#### `M_AUG_CROP_OP`

Sets whether to enable the crop operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_CROP_OP_FACTOR_X`

Sets the crop width factor, to apply for [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md). The actual width with which to crop depends on the specified factor and the source image ([`MimAugment`](../../Reference/im/MimAugment.md)); that is: `_CropWidth_ = [`M_AUG_CROP_OP_FACTOR_X`](../../Reference/im/MimControl.md) x _SourceImageWidth_`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the width factor. |

#### `M_AUG_CROP_OP_FACTOR_Y`

Sets the crop height factor, to apply for [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md). The actual height with which to crop depends on the specified factor and the source image ([`MimAugment`](../../Reference/im/MimAugment.md)); that is: `_CropHeight_ = [`M_AUG_CROP_OP_FACTOR_Y`](../../Reference/im/MimControl.md) x _SourceImageHeight_`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the height factor. |

#### `M_AUG_CROP_OP_RESIZE`

Sets the crop resize option, to apply for [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` *(default)* | Specifies to not resize. |
| `M_TRUE` | Specifies to resize the crop dimensions according to the destination buffer size ([`MimAugment`](../../Reference/im/MimAugment.md)). |

#### `M_AUG_DILATION_ASYM_OP`

Sets whether to enable the asymmetrical dilation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_DILATION_ASYM_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_DILATION_ASYM_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_DILATION_ASYM_OP_NB_ITERATIONS_MAX`

Sets the maximum number of iterations for dilation, to apply for [`M_AUG_DILATION_ASYM_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number of iterations. |

#### `M_AUG_DILATION_OP`

Sets whether to enable the dilation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_DILATION_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_DILATION_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_DILATION_OP_NB_ITERATIONS_MAX`

Sets the maximum number of iterations, to apply for [`M_AUG_DILATION_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number of iterations. |

#### `M_AUG_EROSION_ASYM_OP`

Sets whether to enable the asymmetrical erosion operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_EROSION_ASYM_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_EROSION_ASYM_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_EROSION_ASYM_OP_NB_ITERATIONS_MAX`

Sets the maximum number of iterations for dilation, to apply for [`M_AUG_EROSION_ASYM_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number of iterations. |

#### `M_AUG_EROSION_OP`

Sets whether to enable the erosion operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_EROSION_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_EROSION_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_EROSION_OP_NB_ITERATIONS_MAX`

Sets the maximum number of iterations, to apply for [`M_AUG_EROSION_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number of iterations. |

#### `M_AUG_FLIP_OP`

Sets whether to enable the flip operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_FLIP_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_FLIP_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_FLIP_OP_DIRECTION`

Sets the direction with which to flip the image, to apply for [`M_AUG_FLIP_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BOTH` *(default)* | Specifies to flip in either the horizontal or vertical direction. |
| `M_FLIP_HORIZONTAL` | Specifies to flip exclusively in the horizontal direction. |
| `M_FLIP_VERTICAL` | Specifies to flip exclusively in the vertical direction. |

#### `M_AUG_GAMMA_OP`

Sets whether to enable the gamma correction operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_GAMMA_OP_DELTA`

Sets the range for the random distribution of gamma values, to apply for [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md).  The range for the random distribution of gamma values can be expressed as: [gamma - delta, gamma + delta].

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 10.0` *(default)* | Specifies the range for the random distribution of gamma values. |

#### `M_AUG_GAMMA_OP_MODE`

Sets the gamma correction mode, to apply for [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md).  If you call [`MimAugment`](../../Reference/im/MimAugment.md) with a 1-band buffer, Aurora Imaging Library applies the gamma correction to band 0, regardless of the specified mode (for example, to retrieve the gamma correction value applied by [`MimAugment`](../../Reference/im/MimAugment.md), you must call [`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_AUG_GAMMA_VALUE_BAND_0`](../../Reference/im/MimGetResult.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL_BANDS` *(default)* | Specifies to apply the same gamma correction to each band. |
| `M_PER_BAND` | Specifies to apply a different random gamma correction to each band. |

#### `M_AUG_GAMMA_OP_VALUE`

Sets the gamma value, to apply for [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 10.0` *(default)* | Specifies the gamma value. |

#### `M_AUG_HSV_VALUE_GAIN_OP`

Sets whether to enable the operation that multiplies the image's value band (in the HSV color space) by a random factor. Multiplying an image's value band can be seen as a type of gain operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_HSV_VALUE_GAIN_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_HSV_VALUE_GAIN_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_HSV_VALUE_GAIN_OP_MAX`

Sets the maximum value for the random value factor's distribution [min, max], to use for [`M_AUG_HSV_VALUE_GAIN_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the maximum value for the random value factor's distribution. |

#### `M_AUG_HSV_VALUE_GAIN_OP_MIN`

Sets the minimum value for the random value factor's distribution [min, max], to use for [`M_AUG_HSV_VALUE_GAIN_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum value for the random value factor's distribution. |

#### `M_AUG_HUE_OFFSET_OP`

Sets whether to enable the operation that adds a random hue angle to hue values.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_HUE_OFFSET_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_HUE_OFFSET_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_HUE_OFFSET_OP_MAX`

Sets the maximum value for the random hue angles' distribution [min, max], to use for [`M_AUG_HUE_OFFSET_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the maximum value for the random hue angles' distribution [min, max]. |

#### `M_AUG_HUE_OFFSET_OP_MIN`

Sets the minimum value for the random hue angles' distribution [min, max], to use for [`M_AUG_HUE_OFFSET_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the minimum value for the random hue angles' distribution [min, max]. |

#### `M_AUG_INTENSITY_ADD_OP`

Sets whether to enable the intensity addition operation. Adding to an image's intensity can be seen as a type of offset operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_INTENSITY_ADD_OP_DELTA`

Sets the range for the random distribution of intensity addition (offset) values, to apply for [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimControl.md).  The random distribution of intensity addition (offset) values can be expressed as: [offset - delta, offset + delta].

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_VALUE` *(default)* | Specifies a value that is automatically established according to the data type and data depth of the source image buffer with which to perform [`MimAugment`](../../Reference/im/MimAugment.md).  | Source image buffer | [`M_AUTO_VALUE`](../../Reference/im/MimControl.md) | | --- | --- | | 8-bit unsigned | 127.0 | | 16-bit unsigned | 32767.0 | | 32-bit floating-point | 0.5 | |
| `Value >= 0.0` | Specifies the range for the random distribution of intensity addition (offset) values. |

#### `M_AUG_INTENSITY_ADD_OP_MODE`

Sets the intensity addition value (offset) mode, to apply for [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL_BANDS` | Specifies to apply the intensity addition value (offset) to each band. |
| `M_AUTO_VALUE` *(default)* | Specifies a value that is automatically established according to the number of bands of the source image buffer with which to perform [`MimAugment`](../../Reference/im/MimAugment.md).  This is the same as [`M_LUMINANCE`](../../Reference/im/MimControl.md) for color (3-band) image buffers, and the same as [`M_ALL_BANDS`](../../Reference/im/MimControl.md) for all other image buffers. |
| `M_LUMINANCE` | Specifies to apply the intensity addition value (offset) to the luminance band only. This value is only supported for color (3-band) image buffers. |

#### `M_AUG_INTENSITY_ADD_OP_VALUE`

Sets the intensity addition value (offset), to apply for [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the intensity addition value (offset). |

#### `M_AUG_INTENSITY_MULTIPLY_OP`

Sets whether to enable the intensity multiplication operation. Multiplying an image's intensity can be seen as a type of gain operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_INTENSITY_MULTIPLY_OP_DELTA`

Sets the range for the random distribution of the intensity multiplication (gain) values, to apply for [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimControl.md).  The random distribution of the intensity multiplication (gain) values can be expressed as: [gain - delta, gain + delta].

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the range for the random distribution of the intensity multiplication (gain) values. |

#### `M_AUG_INTENSITY_MULTIPLY_OP_MODE`

Sets the multiplication value (gain) mode, to apply for [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL_BANDS` | Specifies to apply the multiplication value (gain) to each band. |
| `M_AUTO_VALUE` *(default)* | Specifies a value that is automatically established according to the number of bands of the source image buffer with which to perform [`MimAugment`](../../Reference/im/MimAugment.md).  This is the same as [`M_LUMINANCE`](../../Reference/im/MimControl.md) for color (3-band) image buffers, and the same as [`M_ALL_BANDS`](../../Reference/im/MimControl.md) for all other image buffers. |
| `M_LUMINANCE` | Specifies to apply the multiplication value (gain) to the luminance band only. This value is only supported for color (3-band) image buffers. |

#### `M_AUG_INTENSITY_MULTIPLY_OP_VALUE`

Sets the intensity multiplication value (gain), to apply for [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the intensity multiplication value (gain). |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP`

Sets whether to enable the directional lighting operation. Modifying an image's directional lighting can be seen as a type of add-ramp operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP_ANGLE_MAX`

Sets an illumination to add to the image at a random angle (ramp), to apply for [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the maximum angle (ramp) with which to add the illumination, in degrees. |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP_INTENSITY_MAX`

Sets the maximum light intensity, to use for [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the maximum light intensity. |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP_INTENSITY_MIN`

Sets the minimum light intensity, to use for [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum light intensity. |

#### `M_AUG_LIGHTING_DIRECTIONAL_OP_MODE`

Sets the directional lighting operation mode, to apply for [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL_BANDS` | Specifies to apply the directional lighting to each band. |
| `M_AUTO_VALUE` *(default)* | Specifies a value that is automatically established according to the number of bands of the source image buffer with which to perform [`MimAugment`](../../Reference/im/MimAugment.md).  This is the same as [`M_LUMINANCE`](../../Reference/im/MimControl.md) for color (3-band) image buffers, and the same as [`M_ALL_BANDS`](../../Reference/im/MimControl.md) for all other image buffers. |
| `M_LUMINANCE` | Specifies to apply the directional lighting to the luminance band only. This value is only supported for color (3-band) image buffers. |

#### `M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`

Sets whether to enable the operation that applies a Gaussian additive noise with the option of a random noise standard deviation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP_STDDEV`

Sets standard deviation for noise, to apply for [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_VALUE` *(default)* | Specifies a value that is automatically established according to the data type and data depth of the source image buffer with which to perform [`MimAugment`](../../Reference/im/MimAugment.md).  | Source image buffer | [`M_AUTO_VALUE`](../../Reference/im/MimControl.md) | | --- | --- | | 8-bit unsigned | 25.0 | | 16-bit unsigned | 3553.5 | | 32-bit floating-point | 0.5 | |
| `Value >= 0.0` | Specifies the standard deviation for noise. |

#### `M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP_STDDEV_DELTA`

Sets the range for the random distribution of noise standard deviation values, to apply for [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](../../Reference/im/MimControl.md).  The range for the random distribution of noise standard deviation values can be expressed as: [stddev - delta, stddev + delta].

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_VALUE` *(default)* | Specifies a value that is automatically established according to the data type and data depth of the source image buffer with which to perform [`MimAugment`](../../Reference/im/MimAugment.md).  | Source image buffer | [`M_AUTO_VALUE`](../../Reference/im/MimControl.md) | | --- | --- | | 8-bit unsigned | 25.0 | | 16-bit unsigned | 3553.5 | | 32-bit floating-point | 0.5 | |
| `Value >= 0.0` | Specifies the range for the random distribution of noise standard deviation values. |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP`

Sets whether to enable the operation that applies a multiplicative noise with option of a random noise standard deviation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP_DISTRIBUTION`

Sets the uniform distribution for noise generation, to apply for [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GAUSSIAN` | Specifies a Gaussian distribution for noise generation. |
| `M_UNIFORM` *(default)* | Specifies a uniform distribution for noise generation. |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP_INTENSITY_MIN`

Sets an optional minimum noise intensity value (white noise threshold), to apply for [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies an optional minimum noise intensity value (white noise threshold). |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP_STDDEV`

Sets the standard deviation for noise, to apply for [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the standard deviation for noise. |

#### `M_AUG_NOISE_MULTIPLICATIVE_OP_STDDEV_DELTA`

Sets the range for the random distribution of noise standard deviation values, to apply for [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimControl.md).  The range for the random distribution of noise standard deviation values can be expressed as: [stddev - delta, stddev + delta].

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the range for the random distribution of noise standard deviation values. |

#### `M_AUG_NOISE_SALT_PEPPER_OP`

Sets whether to enable salt and pepper noise with the option of random density.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_NOISE_SALT_PEPPER_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_NOISE_SALT_PEPPER_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_NOISE_SALT_PEPPER_OP_DENSITY`

Sets the density value for salt and pepper noise, to use for [`M_AUG_NOISE_SALT_PEPPER_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the density value for salt and pepper noise. |

#### `M_AUG_NOISE_SALT_PEPPER_OP_DENSITY_DELTA`

Sets the range for the random distribution of noise density values, to use for [`M_AUG_NOISE_SALT_PEPPER_OP`](../../Reference/im/MimControl.md).  The range for the random distribution of noise density values can be expressed as: [density - delta, density + delta].

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the range for the random distribution of noise density values. |

#### `M_AUG_REDUCE_OP`

Sets whether to enable the reduce operation.  [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md) scales down an image by a random factor between [`M_AUG_REDUCE_OP_FACTOR_MIN`](../../Reference/im/MimControl.md) and [`M_AUG_REDUCE_OP_FACTOR_MAX`](../../Reference/im/MimControl.md). The scaled down image is positioned with a random X and Y offset, and the operation will add a background of color [`M_AUG_REDUCE_OP_BACKGROUND_COLOR`](../../Reference/im/MimControl.md), maintaining the original image size. The offset can be retrieved by using [`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_AUG_REDUCE_OFFSET_X`](../../Reference/im/MimGetResult.md) and [`M_AUG_REDUCE_OFFSET_Y`](../../Reference/im/MimGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_REDUCE_OP_BACKGROUND_COLOR`

Sets the background color to use for [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the background color. |
| `M_COLOR_BLACK` *(default)* | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_DONT_CARE_CLASS` | Specifies the color (255, 255, 255). |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `Value` | Specifies a grayscale value. The buffer can be a 1-band or multi-band buffer. The specified value is cast to the buffer type and depth.  Note that a grayscale value can be any integer or floating-point number. If the given value exceeds the range of the possible values that can be stored in each band of the destination buffer, the least significant bits of the value are used. |

#### `M_AUG_REDUCE_OP_FACTOR_MAX`

Sets the maximum scale factor by which to reduce the image for [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0.9. |
| `0.0 < Value <= 1.0` | Specifies the maximum scale factor. |

#### `M_AUG_REDUCE_OP_FACTOR_MIN`

Sets the minimum scale factor by which to reduce the image for [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0.5. |
| `0.0 < Value <= 1.0` | Specifies the minimum scale factor. |

#### `M_AUG_RNG_INIT_VALUE`

Sets the explicit initialization value for the internal random number generator, when [`M_AUG_SEED_MODE`](../../Reference/im/MimControl.md) is set to [`M_RNG_INIT_VALUE`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value < AIL_INT32_MAX` *(default)* | Specifies the explicit initialization value for the internal random number generator. |

#### `M_AUG_ROTATION_OP`

Sets whether to enable the rotation operation.  To rotate the image, a random rotation angle is selected from the following range: `AngleMin &lt;= (AngleRef + i * AngleStep) ± (AngleDelta / 2) &lt; AngleMax`, where _AngleMin_ represents the minimum rotation angle ([`M_AUG_ROTATION_OP_ANGLE_MIN`](../../Reference/im/MimControl.md)), _AngleRef_ represents the reference angle ([`M_AUG_ROTATION_OP_ANGLE_REF`](../../Reference/im/MimControl.md)), _i_ is a randomly generated integer, _AngleStep_ represents the step size ([`M_AUG_ROTATION_OP_ANGLE_STEP`](../../Reference/im/MimControl.md)), _AngleDelta_ represents the delta angle ([`M_AUG_ROTATION_OP_ANGLE_DELTA`](../../Reference/im/MimControl.md)), and _AngleMax_ represents the maximum rotation angle ([`M_AUG_ROTATION_OP_ANGLE_MAX`](../../Reference/im/MimControl.md)). If the randomly selected rotation angle is outside the range of _AngleMin_ and _AngleMax_, it will be discarded and another will be generated (with another randomly selected _i_).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_ROTATION_OP_ANGLE_DELTA`

Sets the delta angle, to apply for[`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md). The delta angle is the angular range, relative to (AngleRef + i * AngleStep).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the delta angle, in degrees. |

#### `M_AUG_ROTATION_OP_ANGLE_MAX`

Sets the maximum rotation angle, to apply for [`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the maximum rotation angle, in degrees. |

#### `M_AUG_ROTATION_OP_ANGLE_MIN`

Sets the minimum rotation angle, to apply for [`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the minimum rotation angle, in degrees. |

#### `M_AUG_ROTATION_OP_ANGLE_REF`

Sets the reference angle. All other angles for[`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md)will be calculated relative to this angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the reference angle, in degrees. |

#### `M_AUG_ROTATION_OP_ANGLE_STEP`

Sets the step angle, to apply for[`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md). The step angle is the distance, or gap, relative to the reference angle. The random integer i determines the number of rotation steps from the reference angle to reach the angle range from which to randomly select a rotation angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the reference angle, in degrees. |

#### `M_AUG_SATURATION_GAIN_OP`

Sets whether to enable the operation that multiplies the image's saturation band (in the HSV color space) by a random factor. Multiplying an image's saturation band can be seen as a type of gain operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_SATURATION_GAIN_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_SATURATION_GAIN_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_SATURATION_GAIN_OP_MAX`

Sets the maximum value for the random saturation factor's distribution [min, max], to use for [`M_AUG_SATURATION_GAIN_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the maximum value for the random saturation factor's distribution. |

#### `M_AUG_SATURATION_GAIN_OP_MIN`

Sets the minimum value for the random saturation factor's distribution [min, max], to use for [`M_AUG_SATURATION_GAIN_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum value for the random saturation factor's distribution. |

#### `M_AUG_SCALE_OP`

Sets whether to enable the scale operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_SCALE_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_SCALE_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_SCALE_OP_FACTOR_MAX`

Sets the maximum scale factor, to apply for [`M_AUG_SCALE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 16.0` *(default)* | Specifies the maximum scale factor. |

#### `M_AUG_SCALE_OP_FACTOR_MIN`

Sets the minimum scale factor, to apply for [`M_AUG_SCALE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 16.0` *(default)* | Specifies the minimum scale factor. |

#### `M_AUG_SEED_MODE`

Sets the mode with which to initialize seeds.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RNG_AUTO` *(default)* | Specifies that the initialization value of the internal random number generator is chosen at random. |
| `M_RNG_INIT_VALUE` | Specifies an explicit initialization value for the internal random number generator, using [`M_AUG_RNG_INIT_VALUE`](../../Reference/im/MimControl.md). Specifying the same initialization value lets you rerun the augmentation with the same randomized settings. This initialization value is saved with the augmentation context. |
| `M_USER_DEFINED_SEED` | Specifies a user-defined value for the seeds, using [`MimAugment`](../../Reference/im/MimAugment.md) with the [`SeedValue`](../../Reference/im/MimAugment.md) parameter. |

#### `M_AUG_SHARPEN_DERICHE_OP`

Sets whether to enable the operation that applies a Deriche sharpening filter with a random filter smoothness value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_SHARPEN_DERICHE_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_SHARPEN_DERICHE_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_SHARPEN_DERICHE_OP_FACTOR_MAX`

Sets the maximum value for the sharpen filter smoothness value's random distribution [min, max], to apply for [`M_AUG_SHARPEN_DERICHE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the maximum value for the sharpen filter smoothness value's random distribution [min, max]. |

#### `M_AUG_SHARPEN_DERICHE_OP_FACTOR_MIN`

Sets the minimum value for the sharpen filter smoothness value's random distribution [min, max], to apply for [`M_AUG_SHARPEN_DERICHE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum value for the sharpen filter smoothness value's random distribution [min, max]. |

#### `M_AUG_SHEAR_X_OP`

Sets whether to enable the X-direction shear operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_SHEAR_X_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_SHEAR_X_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_SHEAR_X_OP_MAX`

Sets the maximum value for the X-coefficient's random distribution [min, max], to use for [`M_AUG_SHEAR_X_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the maximum value for the X-coefficient's random distribution [min, max]. |

#### `M_AUG_SHEAR_X_OP_MIN`

Sets the minimum value for the X-coefficient's random distribution [min, max], to use for [`M_AUG_SHEAR_X_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the minimum value for the X-coefficient's random distribution [min, max]. |

#### `M_AUG_SHEAR_Y_OP`

Sets whether to enable the Y-direction shear operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_SHEAR_Y_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_SHEAR_Y_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_SHEAR_Y_OP_MAX`

Sets the maximum value for the Y-coefficient's random distribution [min, max], to use for [`M_AUG_SHEAR_Y_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the maximum value for the Y-coefficient's random distribution [min, max]. |

#### `M_AUG_SHEAR_Y_OP_MIN`

Sets the minimum value for the Y-coefficient's random distribution [min, max], to use for [`M_AUG_SHEAR_Y_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the minimum value for the Y-coefficient's random distribution [min, max]. |

#### `M_AUG_SMOOTH_DERICHE_OP`

Sets whether to enable the operation that performs a Deriche filter smoothing with random filter smoothness value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_SMOOTH_DERICHE_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_SMOOTH_DERICHE_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_SMOOTH_DERICHE_OP_FACTOR_MAX`

Sets the maximum value for the Deriche smoothness value's random distribution [min, max], to use for [`M_AUG_SMOOTH_DERICHE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the maximum value for the Deriche smoothness value's random distribution [min, max]. |

#### `M_AUG_SMOOTH_DERICHE_OP_FACTOR_MIN`

Sets the minimum value for the Deriche smoothness value's random distribution [min, max], to use for [`M_AUG_SMOOTH_DERICHE_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum value for the Deriche smoothness value's random distribution [min, max]. |

#### `M_AUG_SMOOTH_GAUSSIAN_OP`

Sets whether to enable the operation that performs a blurring of the image with a Gaussian blurring kernel calculated using a Gaussian function with a random standard deviation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_SMOOTH_GAUSSIAN_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_SMOOTH_GAUSSIAN_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_SMOOTH_GAUSSIAN_OP_STDDEV_MAX`

Sets the maximum value for the Gaussian standard deviation's random distribution [min, max], to apply for [`M_AUG_SMOOTH_GAUSSIAN_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the maximum value for the Gaussian standard deviation's random distribution [min, max]. |

#### `M_AUG_SMOOTH_GAUSSIAN_OP_STDDEV_MIN`

Sets the minimum value for the Gaussian standard deviation's random distribution [min, max], to apply for [`M_AUG_SMOOTH_GAUSSIAN_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum value for the Gaussian standard deviation's random distribution [min, max]. |

#### `M_AUG_TRANSLATION_X_OP`

Sets whether to enable the X-direction translation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_TRANSLATION_X_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_TRANSLATION_X_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_TRANSLATION_X_OP_MAX`

Sets the maximum translation in the X-direction, to apply for [`M_AUG_TRANSLATION_X_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the maximum translation in the X-direction. |

#### `M_AUG_TRANSLATION_Y_OP`

Sets whether to enable the Y-direction translation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable [`M_AUG_TRANSLATION_Y_OP`](../../Reference/im/MimControl.md). |
| `M_ENABLE` | Specifies to enable [`M_AUG_TRANSLATION_Y_OP`](../../Reference/im/MimControl.md). |

#### `M_AUG_TRANSLATION_Y_OP_MAX`

Sets the maximum translation in the Y-direction, to apply for [`M_AUG_TRANSLATION_Y_OP`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the maximum translation in the Y-direction. |

---

### `Cumulative statistics image processing context ID`

Specifies a cumulative statistics image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) operations.  To process multiple images, call [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) multiple times, each time with a different source image. Note that the source image must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.  Note, use multiple calls to this function to enable multiple statistical operations. Enabling fewer statistics will help increase the speed of the operation.

#### `M_SOURCE_SIZE_X`

Sets the width of a typical source image. This value is only used when not providing a source image to [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) with [`M_PREPROCESS`](../../Reference/im/MimStatCalculate.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the width of a typical source image, in pixels. |

#### `M_SOURCE_SIZE_Y`

Sets the height of a typical source image. This value is only used when not providing a source image to [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) with [`M_PREPROCESS`](../../Reference/im/MimStatCalculate.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the height of a typical source image, in pixels. |

#### `M_STAT_MAX`

Sets whether to calculate the maximum pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_MAX_ABS`

Sets whether to calculate the maximum absolute pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_MEAN`

Sets whether to calculate the mean pixel value for each pixel location across the different source images.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_MIN`

Sets whether to calculate the minimum pixel value for each pixel location across the different source images.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_MIN_ABS`

Sets whether to calculate the minimum absolute pixel value for each pixel location across the different source images.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_NUMBER`

Sets whether to keep track of the number of source images.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to keep track. |
| `M_ENABLE` | Specifies to keep track. |

#### `M_STAT_STANDARD_DEVIATION`

Sets whether to calculate the standard deviation. Note that Aurora Imaging Library calculates the standard deviation using the following formula:  *[Image: im_standard_deviation_eq.png]*

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_SUM`

Sets whether to calculate the sum of the pixel value for each pixel location across the different source images.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_SUM_ABS`

Sets whether to calculate the sum of the absolute pixel values for each pixel location across the different source images.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_SUM_OF_SQUARES`

Sets whether to calculate the sum of the squared pixel values for each pixel location across the different source images.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

---

### `Dead pixel correction image processing context ID`

Specifies a dead pixel correction image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_DEAD_PIXEL_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) operations.

#### `M_DEAD_PIXELS`

Sets the dead pixels image buffer used to identify dead pixels in the source image, where all non-zero pixels are considered dead pixels.  Note that you should only use this control type if you have not specified a series of values that identify the dead pixels, using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_DEAD_PIXELS`](../../Reference/im/MimPut.md).

| Value | Description |
| --- | --- |
| `Dead pixel mask image buffer ID` | Specifies identifier of the image buffer containing the dead pixel mask.  The buffer must be a single-band image buffer, allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md).  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

#### `M_INTERPOLATION_MODE`

Sets the interpolation mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` *(default)* | Specifies to overwrite a dead pixel with an interpolation performed using a weighted average of all its neighboring pixels in the source image. |

---

### `Deinterlacing image processing context ID`

Specifies a deinterlacing image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_DEINTERLACE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimDeinterlace`](../../Reference/im/MimDeinterlace.md) operations.

#### `M_DEINTERLACE_TYPE`

Sets the deinterlacing algorithm to use. The chosen algorithm can either be applied to all the pixels in the source image or to the pixels that are part of an object in motion (adaptive version of the algorithm).  To determine if a pixel is part of a moving object, the adaptive algorithm compares it with the pixel at the same location in neighboring frames. If the difference between the maximum and minimum pixel intensity exceeds a set threshold ([`M_MOTION_DETECT_THRESHOLD`](../../Reference/im/MimControl.md)), then the pixel is considered to be part of a moving object. Otherwise, the pixel is considered to be part of the background. The deinterlacing algorithm is not applied to the background pixels. Instead, the background pixels in the output image will be formed by the corresponding pixels in the even or odd field.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ADAPTIVE_AVERAGE` | Specifies that the average algorithm is applied to the pixels that are considered to be part of a moving object and leaves the background pixels unchanged. |
| `M_ADAPTIVE_BOB` | Specifies that the bob algorithm is applied to the pixels that are considered to be part of a moving object and leaves the background pixels unchanged. |
| `M_ADAPTIVE_DISCARD` | Specifies that the discard algorithm is applied to the pixels that are considered to be part of a moving object and leaves the background pixels unchanged. |
| `M_AVERAGE` | Performs the averaging algorithm. This algorithm is equivalent to performing the discard algorithm twice, once using the first field in the frame and once using the second. The resulting two frames will then be averaged to form one deinterlaced output frame. |
| `M_BOB` | Performs the bob algorithm. This algorithm performs the discard algorithm twice, once using the first field in the frame and once using the second. The result is two output frames. Therefore, the output frame rate is twice as high as the input frame rate. |
| `M_DISCARD` *(default)* | Performs the discard algorithm. This algorithm takes one field from the source image and discards the other. The second field is then calculated from this field. Each row of the second field is obtained by averaging the two corresponding neighboring rows in the first field. For example, the first row of the second field is calculated from the average of the first and second rows of the first field. |

#### `M_DISCARD_FIELD`

Sets the field to discard when using the [`M_DISCARD`](../../Reference/im/MimControl.md) or [`M_ADAPTIVE_DISCARD`](../../Reference/im/MimControl.md) algorithm. Note, in the averaging and bob algorithms, the discard algorithm is called twice; the first field is discarded on the first call and the second field is discarded on the second call.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EVEN_FIELD` *(default)* | Specifies that the even field is discarded. |
| `M_ODD_FIELD` | Specifies that the odd field is discarded. |

#### `M_FIRST_FIELD`

Sets the first field to be processed for each input frame and consequently sets the order of the output frames when using the [`M_BOB`](../../Reference/im/MimControl.md) or [`M_ADAPTIVE_BOB`](../../Reference/im/MimControl.md) algorithm.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EVEN_FIELD` *(default)* | Specifies that the even field will be processed first. |
| `M_ODD_FIELD` | Specifies that the odd field will be processed first. |

#### `M_MOTION_DETECT_NUM_FRAMES`

Sets the number of frames to use for comparison purposes to determine if a pixel is part of an object in motion.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 1` *(default)* | Specifies the number of frames. |

#### `M_MOTION_DETECT_OUTPUT`

Sets whether the output images of [`MimDeinterlace`](../../Reference/im/MimDeinterlace.md) are deinterlaced images or images indicating which pixels are considered to be part of an object in motion (the internal motion detection mask). In the latter case, the pixel values are either 0, if they are part of a background object, or the maximum unsigned value (0xFF for an 8 bit image), if they are part of an object in motion.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the output images are the deinterlaced images. |
| `M_ENABLE` | Specifies that the output images indicate the background pixels and the pixels that are considered to be part of the object in motion. |

#### `M_MOTION_DETECT_REFERENCE_FRAME`

Sets the index of the frame to process within the group of frames that are used for motion detection. This frame is used as the reference frame.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER_FRAME` *(default)* | Specifies that the center frame in the group is used as the reference frame. |
| `M_FIRST_FRAME` | Specifies that the first frame in the group is used as the reference frame. |
| `M_LAST_FRAME` | Specifies that the last frame in the group is used as the reference frame. |
| `0 <= Value < M_MOTION_DETECT_NUM_FRAMES` | Specifies the index of the frame relative to the first frame of the group. The first frame of the group has index 0. |

#### `M_MOTION_DETECT_THRESHOLD`

Sets the threshold value used to differentiate between pixels that are part of objects in motion and background pixels. Each pixel is compared with the pixel at the same location in neighboring frames. If the difference between the maximum and minimum pixel intensity exceeds the specified threshold, the pixel is considered part of a moving object. Otherwise, it is considered part of the background.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the threshold. |

#### `M_SOURCE_FIRST_IMAGE`

Sets the index of the input image used to generate the first deinterlaced image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the index of the image in the source image array. |

---

### `Event list image processing result ID`

Specifies an event list image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md), and used in [`MimLocateEvent`](../../Reference/im/MimLocateEvent.md) operations.

#### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MimGetResult`](../../Reference/im/MimGetResult.md) or [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) generates an error if the result was not calculated on a calibrated image. |

---

### `Filter majority image processing context ID`

Specifies a filter majority image processing context identifier, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_FILTER_MAJORITY_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md)operations.

#### `M_IGNORED_VALUE`

Sets a single pixel value to ignore. If a pixel value in the neighborhood matches the ignored value, it will not be counted; the pixel is essentially excluded from the neighborhood. For example, if you want to ignore a background color present in your source image, you can set this control type to the value of the pixels corresponding to the background color.  If you want to use this control type, you must also set [`M_USE_IGNORED_VALUE`](../../Reference/im/MimControl.md) to [`M_ENABLE`](../../Reference/im/MimControl.md).  The default value of this control type is 0. If you set [`M_USE_IGNORED_VALUE`](../../Reference/im/MimControl.md) to [`M_ENABLE`](../../Reference/im/MimControl.md) but do not specify a value to ignore, pixels whose values are 0 will be ignored.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the pixel value to ignore. |

#### `M_TIE_BREAKER_MODE`

Sets the method used to break a tie in the neighborhood. In the case of a tie (two or more pixel values occur with the greatest frequency in the neighborhood), this control type specifies which value should be selected to replace the value of the central pixel.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX` | Specifies to replace the central pixel value with the greatest value present among the tied values. If you have specified weights for your neighborhood and have set [`M_USE_FREQUENCY_TIE_BREAKER`](../../Reference/im/MimControl.md) to [`M_ENABLE`](../../Reference/im/MimControl.md), this control value specifies to replace the central pixel value with the most common (greatest non-weighted frequency) value present among the tied values. |
| `M_MEDIAN` | Specifies to replace the central pixel value with the median value present among the tied values. If you have specified weights for your neighborhood and have set [`M_USE_FREQUENCY_TIE_BREAKER`](../../Reference/im/MimControl.md) to [`M_ENABLE`](../../Reference/im/MimControl.md), this control value specifies to replace the central pixel value with the value that has the median non-weighted frequency present among the tied values. |
| `M_MIN` *(default)* | Specifies to replace the central pixel value with the smallest value present that appears among the tied values. If you have specified weights for your neighborhood and have set [`M_USE_FREQUENCY_TIE_BREAKER`](../../Reference/im/MimControl.md) to [`M_ENABLE`](../../Reference/im/MimControl.md), this control value specifies to replace the central pixel value with the least common (lowest non-weighted frequency) value present among the tied values. |

#### `M_USE_FREQUENCY_TIE_BREAKER`

Sets whether the control type [`M_TIE_BREAKER_MODE`](../../Reference/im/MimControl.md) should break a tie in the neighborhood using the values present or, if weights are specified, using the non-weighted frequency at which the tied values appear.  If you have specified weights for the pixels in your neighborhood, it is possible that two pixel values are tied despite not occurring with equal frequency in the neighborhood. For example, if your neighborhood contains four pixels with value 5 and weight 1, and two pixels with value 6 and weight 2, they are considered to be tied. You can use this control type to control whether [`M_TIE_BREAKER_MODE`](../../Reference/im/MimControl.md) breaks the tie by comparing the actual pixel values (by comparing 5 to 6) or by comparing their non-weighted frequencies (by comparing four pixels present to two pixels present).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to choose the value with which to replace the central pixel by comparing the values present in the neighborhood. |
| `M_ENABLE` | Specifies to choose the value with which to replace the central pixel by comparing the non-weighted frequency at which the values are found in the neighborhood. |

#### `M_USE_IGNORED_VALUE`

Sets whether Aurora Imaging Library should ignore the value in the source image set by [`M_IGNORED_VALUE`](../../Reference/im/MimControl.md) when calling [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to ignore any value when calling [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md). |
| `M_ENABLE` | Specifies to ignore the pixels in the source image whose values match the ignored value when calling [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md). |

#### `M_WEIGHT_IMAGE`

Sets the identifier of an image buffer, allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md), that defines the neighborhood. When you pass a custom majority filter image processing context to [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md), this is how you define the neighborhood and, optionally, specify different weights, instead of passing a structuring element to the function. This allows you to save the weights with the context when calling [`MimSave`](../../Reference/im/MimSave.md) or [`MimStream`](../../Reference/im/MimStream.md). The shape and size of the neighborhood will match that of the weight image. For example, if you want a rectangular 3x3 neighborhood, you should set the width and height of the buffer to 3 pixels when allocating it.  If you load non-uniform pixel values into this buffer, certain neighborhood pixels will have more influence during the majority filter operation. For example, if you set the central pixel's value to 2, and all others to 1, then the central pixel's value will be counted twice during the majority filter operation. You can use [`MbufPut2d`](../../Reference/buf/MbufPut2d.md) to populate your weight image buffer with pixel weights.

| Value | Description |
| --- | --- |
| `Weight image buffer identifier` | Specifies the identifier of the weight image buffer that you have previously allocated, using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md).  Note that floating-point image buffers are not permitted. Additionally, if your weight image buffer is associated with a region of interest, it will cause an error. |

---

### `Find orientation image processing context ID`

Specifies a find orientation image processing context identifier, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_FIND_ORIENTATION_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) operations.

#### `M_BORDER_ATTENUATION`

Sets whether [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) must process the image's borders, or if the operation can ignore spatial patterns occurring close to the ends of the image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) cannot ignore the image's borders. The find orientation operation will use the borders. |
| `M_ENABLE` *(default)* | Specifies that [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) can ignore the image's borders. The find orientation operation will not necessarily use the borders. |

#### `M_FREQUENCY_CUTOFF_RATIO_HIGH`

Specifies the upper limit of frequencies in which to look for dominant orientations, as a percentage of the maximum frequency; the maximum frequency is dictated by the size of the image. Larger images will have higher maximum frequencies. Frequencies higher than this percentage will be ignored during computation. This can be useful for discarding noise in the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the upper limit of frequencies in which to look for dominant orientations, as a percentage of the maximum frequency. For example, by specifying 95% (the default), frequencies above `_MaxFrequency_ x 95%` will be ignored. |

#### `M_FREQUENCY_CUTOFF_RATIO_LOW`

Specifies the lower limit of frequencies in which to look for dominant orientations, as a percentage of the maximum frequency; the maximum frequency is dictated by the size of the image. Larger images will have higher maximum frequencies. Frequencies lower than this percentage will be ignored during computation. This can be useful for discarding noise in the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the lower limit of frequencies in which to look for dominant orientations, as a percentage of the maximum frequency. For example, by specifying 5% (the default), frequencies below `_MaxFrequency_ x 5%` will be ignored. |

#### `M_INTERPOLATION_MODE`

Sets the interpolation mode used to internally resize the source image if it is of an inappropriate size. This will only be used if the source image has dimensions that are not a power of 2 (X-size and Y-size).  Note that you cannot set an interpolation mode if you are using [`M_CLIP_CENTER`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` *(default)* | Specifies averaging interpolation. The [`M_MODE`](../../Reference/im/MimControl.md) control type must be set to [`M_RESIZE_DOWN`](../../Reference/im/MimControl.md). |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated according to the depth and data type of the destination buffer. |
| `M_BILINEAR` | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_INTERPOLATE` | Specifies interpolated resizing. For resizing up, this is equivalent to bilinear; for resizing down, this is equivalent to averaging. This gives the best speed/result compromise for interpolated resizing. |
| `M_MAX` | Specifies an interpolation based on the maximum pixel value in the source image area. The [`M_MODE`](../../Reference/im/MimControl.md) control type must be set to [`M_RESIZE_DOWN`](../../Reference/im/MimControl.md).  Note that this can alter the shapes of objects and reduce the robustness of the operation. |
| `M_MIN` | Specifies an interpolation based on the minimum pixel value in the source image area. The [`M_MODE`](../../Reference/im/MimControl.md) control type must be set to [`M_RESIZE_DOWN`](../../Reference/im/MimControl.md).  Note that this can alter the shapes of objects and reduce the robustness of the operation. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

#### `M_MODE`

Sets the resizing mode used if the source image is of an inappropriate size. Resizing will only occur if the source image has dimensions that are not a power of 2 (X-size and Y-size). The find orientation operation is then performed on the resized image, which is stored in a temporary image buffer. The original image is not altered.  For more information on resizing, see [Basic geometric transforms](../../UserGuide/C03_Image_processing/S15_Basic_geometrical_transform.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLIP_CENTER` *(default)* | Specifies to perform the find orientation operation using the largest centered portion of the image with dimensions that are a power of 2 (X-size and Y-size). |
| `M_RESIZE_DOWN` | Specifies to perform the find orientation operation on a subsampled version of the image with the closest possible dimensions that are a power of 2 (X-size and Y-size). |
| `M_RESIZE_UP` | Specifies to perform the find orientation operation on a zoomed version of the image with the closest possible dimensions that are a power of 2 (X-size and Y-size). |

---

### `Flat-field image processing context ID`

Specifies a flat-field image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_FLAT_FIELD_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimFlatField`](../../Reference/im/MimFlatField.md) operations.

#### `M_DARK_CONST`

Sets the dark constant value. This is used to remove thermal agitation recorded in the grabbed image (from the CCD) or to remove the darkest possible shade of black when removing uneven lighting from grabbed images.  > **Note:** Note that [`M_DARK_CONST`](../../Reference/im/MimControl.md) and [`M_DARK_IMAGE`](../../Reference/im/MimControl.md) both cannot be set in the same flat-field image processing context.

| Value | Description |
| --- | --- |
| `0 <= Value <= 65535` | Specifies the constant. |

#### `M_DARK_IMAGE`

Sets the identifier of the dark image. This is used to remove thermal agitation recorded in the grabbed image (from the CCD) or to remove the dark shadows from the source image when removing uneven lighting from grabbed images. Note that, this image should be a uniformly dark area (such as, grabbing with the lens cap on your camera).  > **Note:** Note that [`M_DARK_CONST`](../../Reference/im/MimControl.md) and [`M_DARK_IMAGE`](../../Reference/im/MimControl.md) both cannot be set in the same flat-field image processing context.

| Value | Description |
| --- | --- |
| `DarkImageId` | Specifies the identifier of the image buffer. This image buffer must be an 8- or 16-bit unsigned processing image buffer.  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

#### `M_FLAT_CONST`

Sets the flat constant value. This is used to remove the variations of CCD sensitivity recorded in the grabbed image (from the CCD) or to reduce the gray in the grabbed image when removing uneven lighting.  > **Note:** Note that [`M_FLAT_CONST`](../../Reference/im/MimControl.md) and [`M_FLAT_IMAGE`](../../Reference/im/MimControl.md) both cannot be set in the same flat-field image processing context.

| Value | Description |
| --- | --- |
| `0 <= Value <= 65535` | Specifies the constant. |

#### `M_FLAT_IMAGE`

Sets the identifier of the flat image. This is used to remove the variations of CCD sensitivity recorded in the grabbed image (from the CCD) or to reduce the gray in the grabbed image when removing uneven lighting. Note that, this image should be a uniform light gray area. When dealing with CCD sensitivity, the exposure time should be relatively short. Alternatively, when dealing with uneven lighting, the exposure time should be set so that no pixel is saturated.  > **Note:** Note that [`M_FLAT_CONST`](../../Reference/im/MimControl.md) and [`M_FLAT_IMAGE`](../../Reference/im/MimControl.md) both cannot be set in the same flat-field image processing context.

| Value | Description |
| --- | --- |
| `FlatImageId` | Specifies the identifier of the image buffer. This image buffer must be an 8- or 16-bit unsigned processing buffer.  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

#### `M_GAIN_CONST`

Sets the gain factor used to normalize (or scale) the result of the flat field calculation back to the full dynamic range of the destination image.

| Value | Description |
| --- | --- |
| `M_AUTOMATIC` | Specifies an automatic gain factor.  The automatically generated gain factor is determined by subtracting the flat image ([`M_FLAT_IMAGE`](../../Reference/im/MimControl.md)) from the offset image ([`M_OFFSET_IMAGE`](../../Reference/im/MimControl.md)) and then taking the average of the resulting image's pixels.  If a constant value is specified instead of images (by using [`M_FLAT_CONST`](../../Reference/im/MimControl.md) and [`M_OFFSET_CONST`](../../Reference/im/MimControl.md)), Aurora Imaging Library returns the result of the subtraction instead. |
| `Value > 0.0` | Specifies the gain factor. |

#### `M_OFFSET_CONST`

Sets the offset constant value. This is used to remove the electrical bias recorded in the grabbed image (from the CCD) or to reduce the black in the flat image when removing uneven lighting.  > **Note:** Note that [`M_OFFSET_CONST`](../../Reference/im/MimControl.md) and [`M_OFFSET_IMAGE`](../../Reference/im/MimControl.md) both cannot be set in the same flat-field image processing context.

| Value | Description |
| --- | --- |
| `0 <= Value <= 65535` | Specifies the constant. |

#### `M_OFFSET_IMAGE`

Sets the identifier of the offset image. This is used to remove the electrical bias recorded in the image (from the CCD) or to reduce the black in the flat image when removing uneven lighting. Note that, this image should be a uniform dark area. When dealing with electrical bias, the exposure time should be relatively short. Alternatively, when dealing with uneven lighting, the exposure time should be the same as for the flat image (that is, so that no pixel is saturated).  > **Note:** Note that [`M_OFFSET_CONST`](../../Reference/im/MimControl.md) and [`M_OFFSET_IMAGE`](../../Reference/im/MimControl.md) both cannot be set in the same flat-field image processing context.

| Value | Description |
| --- | --- |
| `OffsetImageId` | Specifies the identifier of the image buffer. This image buffer must be an 8- or 16-bit unsigned processing buffer.  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

---

### `Histogram equalization adaptive context ID`

Specifies a histogram equalization adaptive context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_HISTOGRAM_EQUALIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) operations.

#### `M_ALPHA_VALUE`

Sets the adjustment factor for [`M_EXPONENTIAL`](../../Reference/im/MimControl.md) and [`M_RAYLEIGH`](../../Reference/im/MimControl.md) operations. For other operations, [`M_ALPHA_VALUE`](../../Reference/im/MimControl.md) is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the adjustment factor.  For an [`M_EXPONENTIAL`](../../Reference/im/MimControl.md) operation, greater adjustment values result in less occurrences of the most frequent pixels of the histogram in the resulting image buffer.  For an [`M_RAYLEIGH`](../../Reference/im/MimControl.md) operation, greater adjustment values result in greater occurrences of the most frequent pixels of the histogram in the resulting image buffer. |

#### `M_CLIP_LIMIT`

Sets the maximum percentage of values that a tile's histogram bin can represent.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum percentage. For example, if a tile has 100 pixels and you specify a maximum limit of 10%, there can be no bin in that tile's histogram with more than 10 values. This essentially limits the contrast. Exceeding values are distributed evenly among the tile's other histogram bins. |

#### `M_HIST_SIZE`

Sets the number of bins for each tile's histogram.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_SOURCE` *(default)* | Specifies that [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) automatically determines the number of bins for each tile's histogram, according to the number of intensities that are possible in the specified image buffer. For example, if you specify an 8-bit unsigned buffer, each tile's histogram will have 256 bins. |
| `Value >= 2` | Specifies the number of bins for each tile's histogram. Only _integer_ values accepted. |

#### `M_NUMBER_OF_TILES_X`

Sets the number of tiles along the X-direction of the source image specified with [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md).  Given the number of tiles in the X- and Y-direction, the size of the source image, and the requirement that tiles be congruent rectangles, [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) is able to establish the size of the tiles with which to process the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 2` *(default)* | Specifies the number of tiles. Only _integer_ values accepted. |

#### `M_NUMBER_OF_TILES_Y`

Sets the number of tiles along the Y-direction of the source image specified with [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md).  Given the number of tiles in the Y- and X-direction, the size of the source image, and the requirement that tiles be congruent rectangles, [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) is able to establish the size of the tiles with which to process the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 2` *(default)* | Specifies the number of tiles. Only _integer_ values accepted. |

#### `M_OPERATION`

Sets the equalization operation that [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) uses.  The cumulative probability distribution, _ P<sub>f</sub>(f) _, of the input image is approximated by its cumulative histogram: *[Image: mimhistogramequalize_prdis.png]*   For more information, refer to *Digital Image Processing* (Pratt, William K., 1978, John Wiley & Sons).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EXPONENTIAL` | Specifies an equalization density function which generates an Exponential distribution.  Output probability density model: *[Image: mimhistogramequalize_exou.png]*  Transfer function: *[Image: mimhistogramequalize_extr.png]* |
| `M_HYPER_CUBE_ROOT` | Specifies an equalization density function which generates a Hyperbolic Cube Root distribution.  Output probability density model: *[Image: mimhistogramequalize_hcrou.png]*  Transfer function: *[Image: mimhistogramequalize_hcrtr.png]* |
| `M_HYPER_LOG` | Specifies an equalization density function which generates a Hyperbolic Logarithmic distribution.  Output probability density model: *[Image: mimhistogramequalize_hlou.png]*  Transfer function: *[Image: mimhistogramequalize_hltr.png]* |
| `M_RAYLEIGH` | Specifies an equalization density function which generates a Rayleigh distribution.  Output probability density model: *[Image: mimhistogramequalize_raou.png]*  Transfer function: *[Image: mimhistogramequalize_ratr.png]* |
| `M_UNIFORM` *(default)* | Specifies an equalization density function which generates a Uniform distribution.  Output probability density model: *[Image: mimhistogramequalize_unou.png]*  Transfer function: *[Image: mimhistogramequalize_untr.png]* |

---

### `Intensity histogram image processing result ID`

Specifies an intensity histogram image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md), and used in [`MimHistogram`](../../Reference/im/MimHistogram.md) operations.

#### `M_HIST_BIN_SIZE_MODE`

Sets the number of values each histogram bin can represent. To specify the number of histogram bins in the result, use [`MimAllocResult`](../../Reference/im/MimAllocResult.md) and the [`NbEntries`](../../Reference/im/MimAllocResult.md) parameter. For example, if you are using an 8-bit unsigned source image, and you want to have one bin for every possible intensity value, you should set the [`NbEntries`](../../Reference/im/MimAllocResult.md) parameter to 256.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIT_SRC_DATA` | Specifies that [`MimHistogram`](../../Reference/im/MimHistogram.md) determines the resulting bin size according to the source image's minimum and maximum intensity values, and the total number of bins. For example, if you call [`MimHistogram`](../../Reference/im/MimHistogram.md) with a 16-bit signed buffer that holds a source image with a minimum pixel intensity value of 10 and a maximum of 1009, the histogram's bins must account for values between 10 and 1009, which is a total of 1000 possible values. In this case, if the histogram has 100 bins, each bin can represent 10 values (1000/100). |
| `M_FIT_SRC_RANGE` | Specifies that [`MimHistogram`](../../Reference/im/MimHistogram.md) determines the resulting bin size according to the full range of possible values in the source buffer, and the total number of bins. For example, if you call [`MimHistogram`](../../Reference/im/MimHistogram.md) with a 16-bit signed buffer, the histogram's bins must account for intensity values ranging from -32768 to 32767, which is a total of 65536 possible values. In this case, if the histogram has 512 bins, each bin can represent 128 values (65536/512). Note that the minimum and maximum values possible for a buffer can be modified using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md). |
| `M_FREEDMAN` | Specifies that [`MimHistogram`](../../Reference/im/MimHistogram.md) determines the resulting bin size according to the Freedman–Diaconis rule. This is a statistical estimate based on an equation of the general form: `_BinSize_=2_I_ _Q_ _R_(_x_)_n_ <sup>-1/3</sup>`.  For more information, see *On the histogram as a density estimator* (Freedman, David; Diaconis, Persi., 1981, Springer). |
| `M_REGULAR` *(default)* | Specifies that each histogram bin can hold 1 value. |

#### `M_HIST_SMOOTHING_ITERATIONS`

Sets the number of smoothing iterations to perform on the histogram after it has been generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of smoothing iterations. Only _integer_ values accepted.  The smoothing applied is an integer-based averaging of the histogram; the resulting number of values might therefore be different than the number of values in the source image. |

---

### `Linear IIR filter image processing context ID`

Specifies a linear IIR filter image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimConvolve`](../../Reference/im/MimConvolve.md) and [`MimDifferential`](../../Reference/im/MimDifferential.md) operations.

#### `M_FILTER_OPERATION`

Sets the linear filter operation to perform. For all derivatives, the result is signed. The destination buffer should be a signed or floating-point buffer (even if an error is not returned). The result of a derivative in an unsigned buffer will wrap around negative numbers. In all cases, if a result does not fit in the buffer, the least-significant n-bits of the result will be written to the destination buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIRST_DERIVATIVE_X` | Specifies to compute the first derivative of the image, with respect to X, using the specified filter type. |
| `M_FIRST_DERIVATIVE_Y` | Specifies to compute the first derivative of the image, with respect to Y, using the specified filter type. |
| `M_SECOND_DERIVATIVE_X` | Specifies to compute the second derivative of the image, with respect to X, using the specified filter type. |
| `M_SECOND_DERIVATIVE_XY` | Specifies to compute the second derivative of the image, with respect to X and Y, using the specified filter type. |
| `M_SECOND_DERIVATIVE_Y` | Specifies to compute the second derivative of the image, with respect to Y, using the specified filter type. |
| `M_SMOOTH` *(default)* | Performs a smoothing operation on the image using the specified filter type. |

#### `M_FILTER_RESPONSE_TYPE`

Sets the filter response type. In all cases, if a result does not fit in the buffer, the least-significant n-bits of the result will be written to the destination buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SLOPE` *(default)* | Specifies a response that is normalized according to the input slope. With this setting, the filter response is consistent with the local slope of the input signal; that is, the rate of change of local intensity values in the input image is in line with the filter response. Any derivative operations performed maintain this consistency. For example, if using a first derivative operation, the response to a ramp input will be its slope. For derivative operations, a floating-point buffer should be used since the result is not remapped and theoretically the result could be an infinite slope. Note that saturation can occur with this setting. |
| `M_STEP` | Specifies a response that is normalized according to a step input. When [`M_FILTER_STEP_RESPONSE_MAPPING`](../../Reference/im/MimControl.md) is set to [`M_ACCORDING_TO_DEST`](../../Reference/im/MimControl.md), the response is adjusted so that saturation will not occur; that is, the maximum filter response is set according to the depth of the destination image buffer so that the destination image buffer can hold value extremes without saturation. For derivative operations, the result will be mapped to fit in a signed buffer with the same size as the source. The result is mapped between min +1 and max. For example, for an 8-bit buffer, the result will be mapped between -127 and 127. |

#### `M_FILTER_SMOOTHNESS`

Specifies the degree of smoothness (strength of the denoising) to apply, according to the filter smoothness type. Larger values result in greater smoothing.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies a default smoothness value that is mid-range. For an [`M_NORMALIZED`](../../Reference/im/MimControl.md) filter smoothness type, the value is 50. For an [`M_SIZE`](../../Reference/im/MimControl.md) or [`M_NATIVE`](../../Reference/im/MimControl.md) filter smoothness type, the default value results in smoothing that is approximately equivalent to setting [`M_NORMALIZED`](../../Reference/im/MimControl.md) to 50. |
| `Value >= 0.0` | Specifies the smoothness value.  The range available depends on the smoothness type. For an [`M_NORMALIZED`](../../Reference/im/MimControl.md) type, the range is 0 &lt;= Value &lt;= 100. For an [`M_NATIVE`](../../Reference/im/MimControl.md) type, the range is 0 &lt; Value &lt; 1. For an [`M_SIZE`](../../Reference/im/MimControl.md) type, the value entered reflects the extent to which neighboring pixels affect the central pixel of the region receiving the filter. A size of 10, for example, is roughly equivalent to a 21 x 21 convolution kernel, where the 10 pixels on each side of the central pixel will have an impact on the filter operation. |

#### `M_FILTER_SMOOTHNESS_TYPE`

Specifies the filter smoothness type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NATIVE` | Specifies to control the alpha (*[Image: Deriche_Alpha.png]*  ), beta (*[Image: Shen_Beta.png]*  ), or sigma (*[Image: Vliet_Sigma.png]*  ) value in the Deriche, Shen-Castan, or Vliet weighting function, depending on the filter type specified with [`M_FILTER_TYPE`](../../Reference/im/MimControl.md). Whereas the [`M_NORMALIZED`](../../Reference/im/MimControl.md) and [`M_SIZE`](../../Reference/im/MimControl.md) filter smoothness type settings reorganize the data to provide a more intuitive approach (such as the 0-100 range of [`M_NORMALIZED`](../../Reference/im/MimControl.md)), this setting makes no such transformation and allows you to control the raw (native) smoothness directly. This is an advanced setting, and requires knowledge of the Deriche, Shen-Castan, or Vliet filter formulas.  Set the required alpha (*[Image: Deriche_Alpha.png]*  ), beta (*[Image: Shen_Beta.png]*  ), or sigma (*[Image: Vliet_Sigma.png]*  ) value with [`M_FILTER_SMOOTHNESS`](../../Reference/im/MimControl.md). |
| `M_NORMALIZED` *(default)* | Specifies to use a normalized smoothness value that is between 0 and 100. |
| `M_SIZE` | Specifies to use a smoothness value driven by a spatial size. This size specifies the extent (similar to a kernel width) to which neighboring pixels affect the central pixel of the region receiving the filter. |

#### `M_FILTER_STEP_RESPONSE_MAPPING`

Sets how to remap a derivative result. Note that smoothing operations ([`M_FILTER_OPERATION`](../../Reference/im/MimControl.md) with [`M_SMOOTH`](../../Reference/im/MimControl.md)) are not remapped.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_DEST` | Specifies to remap to the range of the destination buffer. When using this setting, the source and destination buffers cannot be floating-point buffers. Additionally, the destination must be a signed buffer. |
| `M_ACCORDING_TO_SOURCE` *(default)* | Specifies to remap to a signed (floating-point) result that has the same bit-depth as the source buffer. For example, for an 8-bit unsigned buffer, the result will be mapped between -127 and 127. |

#### `M_FILTER_TYPE`

Specifies the type of IIR filter to use for the convolution operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DERICHE` *(default)* | Specifies a Deriche infinite support filter. [`M_DERICHE`](../../Reference/im/MimControl.md) implements an approximation of the Gaussian filter using an IIR weighted exponential function, of which the general form is:  *[Image: DericheFunction.png]*  For the Deriche filter, the neighborhoods' influence decreases much slower as the distance from the central pixel increases, compared to the Shen-Castan filter. |
| `M_SHEN` | Specifies a Shen-Castan infinite support exponential filter. This is an exponential weighting function, of the general form:  *[Image: ShenFunction.png]*  For the Shen-Castan filter, the neighborhoods' influence decreases much faster as the distance from the central pixel increases, compared to the Deriche filter. |
| `M_VLIET` | Specifies a Vliet infinite support filter. This is an approximation of the Gaussian function, which has the general form:  *[Image: Gaussian_general_formula.png]*  The Vliet filter is an accurate approximation of the Gaussian function, especially when sigma (*[Image: Vliet_Sigma.png]*  ) is large. |

---

### `Locate difference image processing result ID`

Specifies a locate difference image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md), and used in [`MimLocateDifference`](../../Reference/im/MimLocateDifference.md) operations.

#### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MimGetResult`](../../Reference/im/MimGetResult.md) or [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) generates an error if the result was not calculated on a calibrated image. |

---

### `Locate peak 1D image processing context ID`

Specifies a 1D locate peak image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_LOCATE_PEAK_1D_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) operations.

#### `M_FRAME_SIZE`

Sets the Y-size of the frames. The Y-size must be set before preprocessing the context.  [`M_FRAME_SIZE`](../../Reference/im/MimControl.md) will always be the Y-size, regardless of the direction set with [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md).  If a multi-frame image buffer is used, [`M_FRAME_SIZE`](../../Reference/im/MimControl.md) sets the Y-size of each individual frame.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FULL_SIZE` *(default)* | Specifies to use the buffer as a single frame. |
| `Value >= 1` | Specifies the Y-size of the frames. |

#### `M_MINIMUM_CONTRAST`

Sets the minimum contrast (difference) between the intensity of the local 1D background and the minimum acceptable intensity of a pixel in the peak neighborhood.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 255` *(default)* | Specifies the minimum contrast. |

#### `M_NUMBER_OF_FRAMES`

Sets the total number of frames for which peaks can be extracted. This is the maximum number of frames for which results can be accumulated. The number of frames must be set before preprocessing the context.  Note that when passing a multi-frame image buffer to [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md), the number of frames processed by each call to [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) can be different from [`M_NUMBER_OF_FRAMES`](../../Reference/im/MimControl.md). If [`M_NUMBER_OF_FRAMES`](../../Reference/im/MimControl.md) is set to 10, and a 5-frame source image buffer is passed, you can call [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) 2 times to process the total number of frames. Note that, in this example case, the 2 calls need not be successive. You can retrieve results using [`MimGetResultSingle`](../../Reference/im/MimGetResultSingle.md) at any time after a [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the number of frames. |

#### `M_NUMBER_OF_PEAKS`

Sets the maximum number of peaks to find along a given lane. If more than the specified number of peaks are found, the highest peaks are kept.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of peaks. Only _integer_ values accepted. |

#### `M_PEAK_INTENSITY_RANGE`

Sets the number of pixels used to calculate the average peak intensity. The pixels are chosen around the peak intensity pixel.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of pixels. |

#### `M_PEAK_WIDTH_DELTA`

Sets the number of pixels that can be added to or subtracted from the nominal width, when determining the range of allowable widths of the peak neighborhood.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of pixels. |

#### `M_PEAK_WIDTH_NOMINAL`

Sets the nominal (expected average) width of the peak neighborhood. In laser line images, this is the average width of the laser line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of pixels. |

#### `M_SCAN_LANE_DIRECTION`

Sets the direction in which to detect peaks.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HORIZONTAL` | Detects peaks along the image's X-axis.  Typically for laser line images with a vertical laser line. |
| `M_VERTICAL` *(default)* | Detects peaks along the image's Y-axis.  Typically for laser line images with a horizontal laser line. |

---

### `Locate peak 1D image processing result ID`

Specifies a locate peak 1D image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_LOCATE_PEAK_1D_RESULT`](../../Reference/im/MimAllocResult.md), and used in [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) operations.

#### `M_PEAK_INTENSITY_INVALID_VALUE`

Sets the intensity value to use for missing peaks.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INVALID` *(default)* | Specifies the value -1. This is adequate for most applications. |
| `Value` | Specifies any integer value, for applications with specific needs. 0 is a common choice in this case. |

#### `M_SORT_CRITERION`

Sets the characteristic with which to sort peaks.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default; the default is [`M_PEAK_INTENSITY`](../../Reference/im/MimControl.md) + [`M_SORT_DOWN`](../../Reference/im/MimControl.md). Note that [`M_SORT_DOWN`](../../Reference/im/MimControl.md) is not the default sort order when explicitly specifying the sort criterion. |
| `M_PEAK_INTENSITY` | Orders peaks according to their intensity. |
| `M_PEAK_POSITION` | Orders peaks according to their position. |

---

### `Match image processing context ID`

Specifies a match image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_MATCH_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimMatch`](../../Reference/im/MimMatch.md) operations.

#### `M_MASK_IMAGE`

Sets the image buffer containing the mask image. All non-zero values are considered masked pixels that will be ignored during the match.

| Value | Description |
| --- | --- |
| `MaskImageId` | Specifies the identifier of the image buffer containing the mask image.  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

#### `M_MAX_SCORE`

Sets the maximum score when performing a match using the normalized grayscale correlation mode. When using other matching modes, this control type is ignored.  This control type causes Aurora Imaging Library to linearly remap the range of the internal match results to the range established, from the lowest possible value to the maximum score specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX_DEPTH` *(default)* | Specifies to establish the maximum score based on the pixel depth of the destination buffer (for example, when dealing with a 16-bit signed buffer, the maximum value would be 32, 767). Note that, when using floating-point destination buffers, the range is [-1, 1]; unless you use a clipping score type (using[`M_SCORE_TYPE`](../../Reference/im/MimControl.md) set to either [`M_NORM_CLIP`](../../Reference/im/MimControl.md) or [`M_NORM_CLIP_SQR`](../../Reference/im/MimControl.md)), in which case the range is [0, 1]. |
| `Value` | Specifies the maximum score. This score can be any positive value less than the maximum predetermined by the destination buffer's pixel depth. |

#### `M_MODE`

Sets the type of computation to perform when matching the source image to the model image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CORRELATE` | Computes a grayscale correlation. |
| `M_CORRELATE_NORMALIZED` *(default)* | Computes a normalized grayscale correlation. |
| `M_MEAN_OF_ABS_DIFFERENCES` | Computes the sum of absolute differences divided by the number of pixels used to compute it, resulting in a 8-bit range result. |
| `M_SUM_OF_ABS_DIFFERENCES` | Computes sum of the absolute differences. Note that the result might overflow in 16-bit destination image buffer. |

#### `M_MODEL_IMAGE`

Sets the image buffer containing the model image.

| Value | Description |
| --- | --- |
| `ModelImageId` | Specifies the identifier of the image buffer containing the model image. Note that this buffer must be a 1-band, 8-bit unsigned image buffer.  This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

#### `M_MODEL_STEP`

Sets whether to use every pixel or every other pixel in the model image when matching the source image to the model image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1` *(default)* | Specifies to take every pixel (on both axes) to compute the match. |
| `2` | Specifies to take every other pixel (on both axes) to compute the match. |

#### `M_SCORE_TYPE`

Sets how to compute the final correlation score. Note that this control type is only used when performing a match using normalized grayscale correlation mode (using [`M_MODE`](../../Reference/im/MimControl.md) set to [`M_CORRELATE_NORMALIZED`](../../Reference/im/MimControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NORM` | Specifies to use normalized grayscale correlation to compute the final match score. |
| `M_NORM_CLIP` | Specifies to clip the results from a normalized grayscale correlation when computing the final correlation score. The calculation used is the same as [`M_NORM`](../../Reference/im/MimControl.md), but any value less than 0 is clipped (that is, recorded as 0). |
| `M_NORM_CLIP_SQR` *(default)* | Specifies to clip the results from the square of the normalized grayscale correlation when computing the final correlation score. The calculation used is the same as [`M_SQR_NORM`](../../Reference/im/MimControl.md), but any value less than 0 is clipped (that is, recorded as 0). |
| `M_SQR_NORM` | Specifies to use the square of the normalized grayscale correlation to compute the final correlation score. |

---

### `Rearrangement image processing context ID`

Specifies a rearrangement image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_REARRANGE_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimRearrange`](../../Reference/im/MimRearrange.md) operations.

#### `M_MODE`

Sets the processing mode.

| Value | Description |
| --- | --- |
| `M_LINES` | Specifies that each area to be rearranged is a single horizontal line. |
| `M_RECTS` | Specifies that each area to be rearranged is a single rectangle. |

---

### `Remap image processing context ID`

Specifies a remap image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_IM_REMAP_CONTEXT`](../../Reference/im/MimAlloc.md), and used in[`MimRemap`](../../Reference/im/MimRemap.md) operations.

#### `M_DST_END_VALUE`

Sets the end pixel value used to compute the destination range.  > **Note:** Note, this is only used when [`M_DST_RANGE`](../../Reference/im/MimControl.md) is set to [`M_USE_CONTEXT`](../../Reference/im/MimControl.md). When calling [`MimRemap`](../../Reference/im/MimRemap.md), if the value falls outside the range of the destination buffer, it will be saturated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the end pixel value. |

#### `M_DST_RANGE`

Sets whether to use the destination image buffer or the remap image processing context to determine the destination range.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USE_BUFFER` | Specifies to use the minimum and maximum pixel values of the destination image buffer. By default, this is the full value range according to the buffer type, unless [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) have been set to other values. |
| `M_USE_CONTEXT` *(default)* | Specifies to use the context's destination start and end values specified with [`M_DST_START_VALUE`](../../Reference/im/MimControl.md) and [`M_DST_END_VALUE`](../../Reference/im/MimControl.md). |

#### `M_DST_START_VALUE`

Sets the start pixel value used to compute the destination range.  > **Note:** Note, this is only used when [`M_DST_RANGE`](../../Reference/im/MimControl.md) is set to [`M_USE_CONTEXT`](../../Reference/im/MimControl.md). When calling [`MimRemap`](../../Reference/im/MimRemap.md), if the value falls outside the range of the destination buffer, it will be saturated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the start pixel value. |

#### `M_REMAP_FUNCTION`

Sets the function used to map the source image buffer to the destination image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNIFORM` *(default)* | Specifies to perform a uniform linear mapping. |

#### `M_SRC_END_VALUE`

Sets the end pixel value used to compute the source range.  > **Note:** Note, this is only used when [`M_SRC_RANGE`](../../Reference/im/MimControl.md) is set to [`M_USE_CONTEXT`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the end pixel value. |

#### `M_SRC_RANGE`

Sets whether to use the source image buffer or the remap image processing context to determine the source range.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USE_BUFFER` | Specifies to use the minimum and maximum pixel values of the source image buffer. By default, this is the full value range according to the buffer type, unless [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) have been set to other values. |
| `M_USE_CONTEXT` *(default)* | Specifies to use the context's source start and end values specified with [`M_SRC_START_VALUE`](../../Reference/im/MimControl.md) and [`M_SRC_END_VALUE`](../../Reference/im/MimControl.md). |

#### `M_SRC_START_VALUE`

Sets the start pixel value used to compute the source range.  > **Note:** Note, this is only used when [`M_SRC_RANGE`](../../Reference/im/MimControl.md) is set to [`M_USE_CONTEXT`](../../Reference/im/MimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the start pixel value. |

---

### `Statistics image processing context ID`

Specifies a statistics image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md), and used by[`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) as the operation to perform.  Note, use multiple calls to this function to enable multiple statistical operations. Enabling fewer statistics will help increase the speed of the operation.

#### `M_COND_HIGH`

Sets the upper limit of the selected condition (set using [`M_CONDITION`](../../Reference/im/MimControl.md)) that establishes which pixels to include in the statistical operation(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no upper limit. |
| `Value` | Specifies the upper limit. |

#### `M_COND_LOW`

Sets the lower limit of the selected condition (set using [`M_CONDITION`](../../Reference/im/MimControl.md)) that establishes which pixels to include in the statistical operation(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no lower limit. |
| `Value` | Specifies the lower limit. |

#### `M_CONDITION`

Sets the condition for the statistical computation that establishes which pixels to include in the statistical operation(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that no condition is set. Note that, all pixels will be used for statistical calculations. |
| `M_EQUAL` | Specifies that only pixels with values equal to [`M_COND_LOW`](../../Reference/im/MimControl.md) will be used for statistical calculations. |
| `M_GREATER` | Specifies that only pixels with values greater than [`M_COND_LOW`](../../Reference/im/MimControl.md) will be used for statistical calculations. |
| `M_GREATER_OR_EQUAL` | Specifies that only pixels with values greater than or equal to [`M_COND_LOW`](../../Reference/im/MimControl.md) will be used for statistical calculations. |
| `M_IN_RANGE` | Specifies that pixels with values between [`M_COND_LOW`](../../Reference/im/MimControl.md) and [`M_COND_HIGH`](../../Reference/im/MimControl.md), inclusive, will be used for statistical calculations. |
| `M_LESS` | Specifies that only pixels with values less than [`M_COND_LOW`](../../Reference/im/MimControl.md) will be used for statistical calculations. |
| `M_LESS_OR_EQUAL` | Specifies that only pixels with values less than or equal to [`M_COND_LOW`](../../Reference/im/MimControl.md) will be used for statistical calculations. |
| `M_NOT_EQUAL` | Specifies that pixels with values equal to [`M_COND_LOW`](../../Reference/im/MimControl.md) will be not used for statistical calculations. |
| `M_OUT_RANGE` | Specifies that pixels with values less than [`M_COND_LOW`](../../Reference/im/MimControl.md), or greater than [`M_COND_HIGH`](../../Reference/im/MimControl.md), will be used for statistical calculations. |

#### `M_GLCM_PAIR_OFFSET_X`

Sets the displacement in X between pixel pairs used to generate the co-occurrence matrix.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the displacement in X. |

#### `M_GLCM_PAIR_OFFSET_Y`

Sets the displacement in Y between pixel pairs used to generate the co-occurrence matrix.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the displacement in Y. |

#### `M_GLCM_QUANTIFICATION`

Sets the depth of the gray level of the co-occurrence matrix.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 10` *(default)* | Specifies the depth. If the depth is set to a value greater than 10, the co-occurrence matrix is rescaled to 10 bits. |

#### `M_SOURCE_SIZE_X`

Sets the width of a typical source image. This value is only used when not providing a source image to [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) with [`M_PREPROCESS`](../../Reference/im/MimStatCalculate.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the width of a typical source image, in pixels. |

#### `M_SOURCE_SIZE_Y`

Sets the height of a typical source image. This value is only used when not providing a source image to [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) with [`M_PREPROCESS`](../../Reference/im/MimStatCalculate.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the height of a typical source image, in pixels. |

#### `M_STAT_ANGULAR_DATA_COHERENCE`

Calculates the coherence of the angular data. The coherence represents the directional trend of all the angular data in the image and is a measure of how coherent or parallel the unit vectors are with respect to each other. The coherence is calculated as the ratio between the length (norm) of the vector sum and its maximum possible length (which corresponds to the number of pixels being considered, since each pixel is presumed to have a length of one). Numerically, this quantity lies between 0 and 1, where 0 represents absolutely no coherence (unit vectors pointing in random directions) and 1 is total coherence (unit vectors pointing in same direction).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_ANGULAR_DATA_MEAN`

Sets whether to calculate the dominant direction of the unit vectors. This angle is a representation of the mean (or average) angle of all the angular data.  Note that to obtain more representative results, the mean angle should be analyzed in context with the coherence. For example, if your image has a coherence of 0.1 and a mean angle of 65°, the mean angle is not very meaningful because the unit vectors are pointing in random, incoherent directions. However, if your image has a coherence of 0.9 and a mean angle of 65°, this indicates that the unit vectors are more or less pointing in the same direction and their average value is around 65°.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_GLCM_CONTRAST`

Sets whether to calculate the contrast of all the grayscale pixel values within a specified neighborhood in relation to the normalized co-occurrence probability of each pixel.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_GLCM_CORRELATION`

Sets whether to measure the local variations in the grayscale co-occurrence matrix.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_GLCM_DISSIMILARITY`

Sets whether to measures the joint probability co-occurrence. The result determines how close your data comes to forming a straight line.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_GLCM_ENERGY`

Sets whether to calculate the energy between two points in the gray level co-occurrence matrix of a given neighborhood. The energy is the pixel value of the cell divided by the sum of the value of the cells in the neighborhood.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_GLCM_ENTROPY`

Sets whether to calculate the inverse of the energy between two points in the normalized co-occurrence probability of a specified neighborhood.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_GLCM_HOMOGENEITY`

Sets whether to calculate the similarity between the normalized co-occurrence probability and the gray level co-occurrence matrix of a given neighborhood. Homogeneity weighs values by the inverse of the contrast weight, with weights decreasing exponentially away from the diagonal.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_MAX`

Sets whether to calculate the maximum pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_MAX_ABS`

Sets whether to calculate the maximum absolute pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_MEAN`

Sets whether to calculate the mean value of the pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_MIN`

Sets whether to calculate the minimum pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_MIN_ABS`

Sets whether to calculate the minimum absolute pixel value from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_NUMBER`

Sets whether to keep track of the number of pixels, from the source image, that satisfied the condition specified when calling [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to keep track. |
| `M_ENABLE` | Specifies to keep track. |

#### `M_STAT_ORIENTATION_DATA_COHERENCE`

Sets whether to calculate the coherence of the orientation data. The coherence represents the directional trend of all the orientation data in the image and is a measure of how coherent or parallel the orientation of the unit vectors are with respect to each other. The coherence is calculated as the ratio between the length (norm) of the vector sum and its maximum possible length (which corresponds to the number of pixels being considered, since each pixel is presumed to have a length of one). Numerically, this quantity lies between 0 and 1, where 0 represents absolutely no coherence (unit vectors pointing in random directions) and 1 is total coherence (unit vectors pointing in same direction).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_ORIENTATION_DATA_MEAN`

Sets whether to calculate the dominant orientation of the unit vectors. The orientation of a unit vector is its inclination, without its direction; if two unit vectors have opposite directions (for example, an angle of 0° and 180°), they have the same orientation (0°). Calculating the dominant orientation using unit vectors with opposite directions (0° and 180°) would result in the magnitude of their vector sum being double the original value (2) and a dominant orientation of 0°. Whereas, calculating the dominant orientation using two unit vectors that are separated by 90° (for example, an angle of 0° and 90°) would result in the magnitude of their vector sum being 0, effectively canceling each other out (no dominant orientation), and an invalid orientation angle.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_STANDARD_DEVIATION`

Sets whether to calculate the standard deviation from the value of each pixel location across the source image. Note that Aurora Imaging Library calculates the standard deviation using the following formula:  *[Image: im_standard_deviation_eq.png]*

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_SUM`

Sets whether to calculate the sum of the pixel values from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_SUM_ABS`

Sets whether to calculate the sum of the absolute pixel values from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STAT_SUM_OF_SQUARES`

Sets whether to calculate the sum of the squared pixel values from each pixel location across the source image.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to perform this statistical operation. |
| `M_ENABLE` | Specifies to perform this statistical operation. |

#### `M_STEP_SIZE_X`

Sets the distance between the neighborhood along the X-axis in which to evaluate the specified statistic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Width` *(default)* | Specifies the width of the neighborhood, in pixels. |

#### `M_STEP_SIZE_Y`

Sets the distance between the neighborhood along the Y-axis in which to evaluate the specified statistic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Height` *(default)* | Specifies the height of the neighborhood, in pixels. |

#### `M_TILE_SIZE_X`

Sets the X-size (width) of the neighborhood in which to evaluate the specified statistic.

| Value | Description |
| --- | --- |
| `Width` | Specifies the width of the neighborhood. |

#### `M_TILE_SIZE_Y`

Sets the Y-size (height) of the neighborhood in which to evaluate the specified statistic.

| Value | Description |
| --- | --- |
| `Height` | Specifies the height of the neighborhood. |

---

### `Unwarp along path image processing context ID`

Specifies an unwarp along path image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_UNWARP_ALONG_PATH_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) operations.

#### `M_INTERPOLATION_MODE`

Sets the interpolation mode used when unwarping along the path.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated according to the depth and data type of the destination buffer. |
| `M_BILINEAR` | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` *(default)* | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

#### `M_MODE`

Sets the type of path along which to unwarp the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_POLYLINE` *(default)* | Specifies that the path is defined by a polyline. This path must be set using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_PATH`](../../Reference/im/MimPut.md) and the width of the path set using [`MimControl`](../../Reference/im/MimControl.md) [`M_SINGLE_WIDTH`](../../Reference/im/MimControl.md). |

#### `M_OVERSCAN`

Sets how to handle pixels that map outside of the source image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OVERSCAN_CLEAR` | Specifies to set the destination pixel to 0 if the corresponding point falls outside of the source buffer. |
| `M_OVERSCAN_DISABLE` | Specifies to leave the destination pixels as is. |
| `M_OVERSCAN_ENABLE` *(default)* | Specifies to use pixels from the source buffer's ancestor buffer. If the source buffer is not a child buffer, or if the point falls outside of the ancestor buffer, the destination pixel is left as is. |

#### `M_SINGLE_WIDTH`

Sets a single width for all points along the path.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the width of the path, in pixels. |

#### `M_UNWARP_SAMPLING_MODE`

Sets which algorithm to use for the unwarp along path operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ADAPTIVE_ALONG_PATH` | Specifies to spread the deformation over the entire path. Quadrilaterals are defined from the intersection of the top and bottom lines of the path (one half-width from the center line). The deformation will occur over the entire quadrilateral. |
| `M_ADAPTIVE_AT_JUNCTION` *(default)* | Specifies to deform only at the junction of each path segment. Rectangles are defined from the shortest side (top or bottom) of the quadrilateral. Triangles are generated to fill the rest of the quadrilateral. The deformation will only occur within the triangular region at the junction. This mode preserves the width of the path. |

---

### `Wavelet image processing context ID`

Specifies a wavelet image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md) or [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md), and used in [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) or [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) operations. Unless otherwise specified, settings apply to both [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md) and [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md) context types.

#### `M_TRANSFORMATION_MODE`

Sets the wavelet transformation mode. Modifying the transformation mode can affect how Aurora Imaging Library samples the data and produces the wavelet results (for example, the diagonal, horizontal, and vertical coefficients).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DYADIC` *(default)* | Specifies a dyadic wavelet transformation. For each transformation level, Aurora Imaging Library samples the wavelet coefficients by a factor of 2. For example, when drawing dyadic results, they are at different sizes, at different levels. Dyadic transformations generally apply to signal coding and data compression. Performing [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) with [`M_DYADIC`](../../Reference/im/MimControl.md) is typically faster than [`M_UNDECIMATED`](../../Reference/im/MimControl.md), though the quality of denoising is usually lower. In general, [`M_DYADIC`](../../Reference/im/MimControl.md) uses less resources (processing time and memory) than [`M_UNDECIMATED`](../../Reference/im/MimControl.md). |
| `M_UNDECIMATED` | Specifies an undecimated wavelet transformation. Such transformations are designed to overcome the lack of invariance between transformation levels in dyadic wavelet transformations. For example, when drawing undecimated results, they are at the same size regardless of the level. Undecimated transformations generally apply to signal denoising and pattern recognition. Performing [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) with [`M_UNDECIMATED`](../../Reference/im/MimControl.md) is typically slower than [`M_DYADIC`](../../Reference/im/MimControl.md), though the quality of denoising is usually higher. In general, [`M_UNDECIMATED`](../../Reference/im/MimControl.md) uses more resources (processing time and memory) than [`M_DYADIC`](../../Reference/im/MimControl.md). |

#### `M_WAVELET_TYPE`

Sets a predefined type of wavelet filter. Only available if the context type is [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md). The mathematical domain of complex filter types ([`M_..._COMPLEX`](../../Reference/im/MimControl.md)) consists of numbers that have a real part (real numbers) and an imaginary part (imaginary numbers). The mathematical domain of filter types that are not complex consist of real numbers only.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DAUBECHIES_1` | Specifies a Daubechies wavelet filter that uses 1 vanishing moment and real coefficients. |
| `M_DAUBECHIES_2` | Specifies a Daubechies wavelet filter that uses 2 vanishing moments and real coefficients. |
| `M_DAUBECHIES_3` | Specifies a Daubechies wavelet filter that uses 3 vanishing moments and real coefficients. |
| `M_DAUBECHIES_3_COMPLEX` | Specifies a Daubechies wavelet filter that uses 3 vanishing moments and complex coefficients. |
| `M_DAUBECHIES_4` | Specifies a Daubechies wavelet filter that uses 4 vanishing moments and real coefficients. |
| `M_DAUBECHIES_5` | Specifies a Daubechies wavelet filter that uses 5 vanishing moments and real coefficients. |
| `M_DAUBECHIES_5_COMPLEX` | Specifies a Daubechies wavelet filter that uses 5 vanishing moments and complex coefficients. |
| `M_DAUBECHIES_6` | Specifies a Daubechies wavelet filter that uses 6 vanishing moments and real coefficients. |
| `M_DAUBECHIES_7` | Specifies a Daubechies wavelet filter that uses 7 vanishing moments and real coefficients. |
| `M_DAUBECHIES_7_COMPLEX` | Specifies a Daubechies wavelet filter that uses 7 vanishing moments and complex coefficients. |
| `M_DAUBECHIES_8` | Specifies a Daubechies wavelet filter that uses 8 vanishing moments and real coefficients. |
| `M_HAAR` *(default)* | Specifies a Haar wavelet filter. Haar uses real coefficients. |
| `M_SYMLET_1` | Specifies a Symlet wavelet filter that uses 1 vanishing moment and real coefficients. |
| `M_SYMLET_2` | Specifies a Symlet wavelet filter that uses 2 vanishing moments and real coefficients. |
| `M_SYMLET_3` | Specifies a Symlet wavelet filter that uses 3 vanishing moments and real coefficients. |
| `M_SYMLET_4` | Specifies a Symlet wavelet filter that uses 4 vanishing moments and real coefficients. |
| `M_SYMLET_5` | Specifies a Symlet wavelet filter that uses 5 vanishing moments and real coefficients. |
| `M_SYMLET_6` | Specifies a Symlet wavelet filter that uses 6 vanishing moments and real coefficients. |
| `M_SYMLET_7` | Specifies a Symlet wavelet filter that uses 7 vanishing moments and real coefficients. |
| `M_SYMLET_8` | Specifies a Symlet wavelet filter that uses 8 vanishing moments and real coefficients. |

### Combination Constants — For specifying the priority (order) with which

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the priority (order) with which [`MimAugment`](../../Reference/im/MimAugment.md) performs the corresponding operation.

#### `M_PRIORITY`

Sets the priority (order) with which [`MimAugment`](../../Reference/im/MimAugment.md) performs the corresponding operation. This overrides the default priority that [`MimAugment`](../../Reference/im/MimAugment.md) uses.

| Value | Description |
| --- | --- |
| `0 <= Value <= 100` | Specifies the priority (order) value. Operations with lower priority (order) values are performed before operations with higher priority (order) values. If multiple operations have the same priority (order) value, [`MimAugment`](../../Reference/im/MimAugment.md) internally chooses the priority (order). |

### Combination Constants — For specifying the probability that

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the probability that [`MimAugment`](../../Reference/im/MimAugment.md) performs the corresponding operation.

#### `M_PROBABILITY`

Sets the probability that [`MimAugment`](../../Reference/im/MimAugment.md) performs the corresponding operation.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the probability, as a percentage. |

### Combination Constants — For specifying whether undecimated wavelet transformations are centered

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify undecimated wavelet transformations that are centered.

| Value | Description |
| --- | --- |
| `M_CENTER` | Specifies undecimated wavelet transformations that are centered. Undecimated wavelet transformations that are not centered can appear misaligned (for example, when drawing results). |

### Combination Constants — For specifying the order to sort peaks

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the order to sort peaks.

| Value | Description |
| --- | --- |
| `M_SORT_DOWN` | Sorts peaks in descending order. |
| `M_SORT_UP` *(default)* | Sorts peaks in ascending order. |

## Remarks

> Note that some of the values listed above are not available in Aurora Imaging Library Lite. See the value's corresponding operation function for Aurora Imaging Library Lite availability.

This control type is intended to adjust how [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) internally processes laser line images. When using [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md), you typically set the value using the function's parameters; this control type only sets the default value used by the function.

When specifying a color (3-band) image, [`MimAugment`](../../Reference/im/MimAugment.md) assumes an RGB color space by default. Operations that require a different color space (for example, HSV) internally convert the source image color. If your source image is not RGB, the conversion produces erroneous results.

Note that this control type is only valid when calculating co-occurrence statistics ([`M_STAT_GLCM_...`](../../Reference/im/MimControl.md)). When used with any other statistic, this control type is ignored.

> **Note:** Note that the image on which the calculation is performed must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

Define the neighborhood using[`M_GLCM_PAIR_OFFSET_X`](../../Reference/im/MimControl.md), [`M_GLCM_PAIR_OFFSET_Y`](../../Reference/im/MimControl.md), [`M_GLCM_QUANTIFICATION`](../../Reference/im/MimControl.md), [`M_TILE_SIZE_X`](../../Reference/im/MimControl.md), and [`M_TILE_SIZE_Y`](../../Reference/im/MimControl.md).
