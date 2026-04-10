---
doctype: Reference
module: im
function: MimBinarizeAdaptive
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimBinarizeAdaptive"
---

# MimBinarizeAdaptive

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

> Perform a point-to-point adaptive binarization on an image.

## Syntax

```c
void MimBinarizeAdaptive(
    AIL_ID    AdaptiveBinarizeContextImId,  //in
    AIL_ID    SrcImageBufId,                //in
    AIL_ID    SeedImage1BufId,              //in
    AIL_ID    SeedImage2BufId,              //in
    AIL_ID    BinarizedImageBufId,          //out
    AIL_ID    ThresholdImageBufId,          //out
    AIL_INT64 ControlFlag                   //in
)
```

## Description

This function performs an adaptive binary threshold operation on the specified image and places the resulting binarized image in the specified buffer. This function also provides the threshold values with which it binarized the image.

To perform the adaptive binarization, this function establishes a threshold value for each pixel in the source, according to the context's threshold mode. Each source pixel that is greater than its corresponding threshold value is set to the highest unsigned destination buffer value. For example, for 1-, 8-, and 16-bit destination buffers (signed or unsigned), the highest values are 1, 0xFF, and 0xFFFF, respectively. For 32-bit floating-point destination buffers, the highest value is 1. Source pixels that are less than or equal to their corresponding threshold value are set to 0, for destination buffers of any type.

To specify a particular mode with which to calculate threshold values, call [`MimControl`](../../Reference/im/MimControl.md) with [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md). Available threshold modes generally refer to adaptive algorithms, which perform multiple calculations on individual sections of an image. When compared to a conventional binarization ([`MimBinarize`](../../Reference/im/MimBinarize.md)), which calculates threshold values using an image as a whole, an adaptive binarization is more robust at processing images with objects that are difficult to identify. Such difficulties typically come from changes in illumination and poor or varying contrast. This type of robustness can cause an adaptive binarization to take longer to execute than a conventional one.

You can modify controls other than threshold mode to customize calculations, such as indicating a maximum ([`M_GLOBAL_MAX`](../../Reference/im/MimControl.md)) or minimum ([`M_GLOBAL_MIN`](../../Reference/im/MimControl.md)) threshold value. Available controls depend on the type of the adaptive binarization context.

1-bit, 8-bit, and 16-bit buffers are supported; their type can be signed or unsigned. 32-bit buffers are also supported; their type must be float. For best results, all image buffers should have the same number of bands and be the same type.

## Parameters

### `AdaptiveBinarizeContextImId` *(in, AIL_ID)*

Specifies the identifier of the adaptive binarization context. The context must have been allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with either [`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md) or [`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md).

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer with which to perform the adaptive binarization. This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

### `SeedImage1BufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to use as the first seed image. If the type of the specified context is [`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md), this function uses seeds as positional information in the source image to establish threshold values, given the threshold mode control ([`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md)). For contexts of type [`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md), seed information is not required; set this parameter to [`M_NULL`](../../Reference/im/MimBinarizeAdaptive.md).

*For specifying the first seed image buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Establishes seeds automatically. |
| `M_NULL` | Specifies to ignore this parameter. Use [`M_NULL`](../../Reference/im/MimBinarizeAdaptive.md) if the context is of type [`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md). |
| `Image buffer identifier` | Specifies the identifier of the first seed image. This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `SeedImage2BufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to use as the second seed image. If the type of the specified context is [`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md), this function uses seeds as positional information in the source image to establish threshold values, given the threshold mode control ([`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md)). For contexts of type [`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md), seed information is not required; set this parameter to [`M_NULL`](../../Reference/im/MimBinarizeAdaptive.md).

*For specifying the second seed image buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Establishes seeds automatically. This is only valid if the threshold mode control is set to [`M_TOGGLE`](../../Reference/im/MimControl.md), and the [`SeedImage1BufId`](../../Reference/im/MimBinarizeAdaptive.md) parameter is set to [`M_DEFAULT`](../../Reference/im/MimBinarizeAdaptive.md). |
| `M_NULL` | Specifies to ignore this parameter. Use [`M_NULL`](../../Reference/im/MimBinarizeAdaptive.md) if the threshold mode control is set to [`M_RECONSTRUCT`](../../Reference/im/MimControl.md) or [`M_LEVEL`](../../Reference/im/MimControl.md) (or the context is of type [`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md)). |
| `Image buffer identifier` | Specifies the identifier of the second seed image. This is only valid if the [`SeedImage1BufId`](../../Reference/im/MimBinarizeAdaptive.md) parameter specifies an image buffer identifier, and the threshold mode control is set to [`M_TOGGLE`](../../Reference/im/MimControl.md). This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `BinarizedImageBufId` *(out, AIL_ID)*

Specifies the identifier of the image buffer in which to put the result of the adaptive binarization operation. This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

### `ThresholdImageBufId` *(out, AIL_ID)*

Specifies the identifier of the image buffer in which to put the resulting threshold values with which to binarize the source image. This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
