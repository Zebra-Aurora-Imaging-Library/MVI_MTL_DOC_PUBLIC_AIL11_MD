---
doctype: Reference
module: im
function: MimAugment
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimAugment"
---

# MimAugment

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

> Perform an augmentation operation on an image.

## Syntax

```c
void MimAugment(
    AIL_ID    AugmentationContextImId,              //in
    AIL_ID    SrcImageBufId,                        //in
    AIL_ID    DstImageBufOrAugmentationResultImId,  //out
    AIL_INT64 SeedValue,                            //in
    AIL_INT64 ControlFlag                           //in
)
```

## Description

This function performs an augmentation operation that allows you to create a plausible variation of an image. This is done by applying a specified ordered set of image processing operations with randomized settings within a specified range. To establish the image processing operations that [`MimAugment`](../../Reference/im/MimAugment.md) can perform, call [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_..._OP`](../../Reference/im/MimControl.md).

[`MimAugment`](../../Reference/im/MimAugment.md) places its results in the specified destination buffer, which can be an image or an augmentation result.

The image processing operations that you specify [`MimAugment`](../../Reference/im/MimAugment.md) to perform ([`M_AUG_..._OP`](../../Reference/im/MimControl.md)) are prioritized as follows:

|   |   |   |
| --- | --- | --- |
| 1. Affine operations, such as: - [`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_SCALE_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_TRANSLATION_X_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_TRANSLATION_Y_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_ASPECT_RATIO_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_SHEAR_X_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_SHEAR_Y_OP`](../../Reference/im/MimControl.md) | 2. Structure operations, such as: - [`M_AUG_DILATION_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_EROSION_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_DILATION_ASYM_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_EROSION_ASYM_OP`](../../Reference/im/MimControl.md) | 3. Geometric operations, such as: - [`M_AUG_FLIP_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_CROP_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_REDUCE_OP`](../../Reference/im/MimControl.md) |
| 4. Intensity operations, such as: - [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_INTENSITY_ADD_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_GAMMA_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_LIGHTING_DIRECTIONAL_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_SATURATION_GAIN_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_HUE_OFFSET_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_HSV_VALUE_GAIN_OP`](../../Reference/im/MimControl.md) | 5. Linear filter operations, such as: - [`M_AUG_SMOOTH_DERICHE_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_SMOOTH_GAUSSIAN_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_BLUR_MOTION_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_SHARPEN_DERICHE_OP`](../../Reference/im/MimControl.md) | 6. Noise operations, such as: - [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_NOISE_MULTIPLICATIVE_OP`](../../Reference/im/MimControl.md)<br/>- [`M_AUG_NOISE_SALT_PEPPER_OP`](../../Reference/im/MimControl.md) |

To modify the order in which [`MimAugment`](../../Reference/im/MimAugment.md) prioritizes the specified image processing operations, call [`MimControl`](../../Reference/im/MimControl.md) with [`M_PRIORITY`](../../Reference/im/MimControl.md).

## Parameters

### `AugmentationContextImId` *(in, AIL_ID)*

Specifies the identifier of the augmentation context. The context must have been allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_AUGMENTATION_CONTEXT`](../../Reference/im/MimAlloc.md).

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. If you are preprocessing the augmentation context ([`M_PREPROCESS`](../../Reference/im/MimAugment.md)), set this parameter to `M_NULL`.

### `DstImageBufOrAugmentationResultImId` *(out, AIL_ID)*

Specifies the identifier of the destination in which to write the result of the augmentation transformation. The identifier must be for an image buffer or an augmentation image processing result buffer. If you are preprocessing the augmentation context ([`M_PREPROCESS`](../../Reference/im/MimAugment.md)), set this parameter to `M_NULL`.

### `SeedValue` *(in, AIL_INT64)*

Specifies the initial value that will generate the randomized settings for the image processing operations used to perform the augmentation. This parameter must be set to one of the following values.

*For specifying the seed that will generate the randomized settings*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. This depends on the seed mode, which is set using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SEED_MODE`](../../Reference/im/MimControl.md). To use [`M_DEFAULT`](../../Reference/im/MimAugment.md), the seed mode must be set to [`M_RNG_AUTO`](../../Reference/im/MimControl.md) (default) or [`M_RNG_INIT_VALUE`](../../Reference/im/MimControl.md). |
| `M_NULL` | Specifies that you are preprocessing the augmentation context ([`M_PREPROCESS`](../../Reference/im/MimAugment.md)). |
| `0 <= Value < AIL_INT32_MAX` | Specifies a user-defined seed value. You can only specify a user-defined seed if [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SEED_MODE`](../../Reference/im/MimControl.md) is set to [`M_USER_DEFINED_SEED`](../../Reference/im/MimControl.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether [`MimAugment`](../../Reference/im/MimAugment.md) should perform the augmentation, or should explicitly preprocess the augmentation context. If you do not explicitly preprocess the context ([`M_PREPROCESS`](../../Reference/im/MimAugment.md)), [`MimAugment`](../../Reference/im/MimAugment.md) preprocesses it the first time you call the function.

*For specifying the operation to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Performs the augmentation. |
| `M_PREPROCESS` | Preprocesses the specified augmentation context explicitly. In this case, you must set the [`SrcImageBufId`](../../Reference/im/MimAugment.md), [`DstImageBufOrAugmentationResultImId`](../../Reference/im/MimAugment.md), and [`SeedValue`](../../Reference/im/MimAugment.md)parameters to [`M_NULL`](../../Reference/im/MimAugment.md). |
