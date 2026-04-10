---
doctype: Reference
module: im
function: MimMatch
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimMatch"
---

# MimMatch

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

> Generates a series of correlation values between the source image and a model image.

## Syntax

```c
void MimMatch(
    AIL_ID    MatchContextImId,  //in
    AIL_ID    SrcImageBufId,     //in
    AIL_ID    DstImageBufId,     //out
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function generates a series of correlation values (match scores) resulting from the model image being compared to every pixel in your image. The [`MimMatch`](../../Reference/im/MimMatch.md) operation results in an image of correlation data. The closer the resulting pixel value is to the specified maximum score, the closer the match between the source image buffer and the model image.

This function uses a match image processing context to match a pixel in the source image with a given model image. Specify the model image using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODEL_IMAGE`](../../Reference/im/MimControl.md). You can also specify the algorithm to use to perform the correlation (the mode), using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODE`](../../Reference/im/MimControl.md). In addition, you can limit the region in the model image with which to perform the correlation, using a mask image; specify the mask image using [`MimControl`](../../Reference/im/MimControl.md) with[`M_MASK_IMAGE`](../../Reference/im/MimControl.md). In the match mask image, all non-zero pixels are considered mask pixels that are ignored during the match.

When dealing with large images, you can reduce the number of comparisons performed from 1 every pixel to 1 every other pixel in your model image, using [`MimControl`](../../Reference/im/MimControl.md) with[`M_MODEL_STEP`](../../Reference/im/MimControl.md).

Before you call this function to perform a match operation, preprocess the match image processing context by calling [`MimMatch`](../../Reference/im/MimMatch.md) with [`M_PREPROCESS`](../../Reference/im/MimMatch.md). If the preprocess operation is not done explicitly (using [`M_PREPROCESS`](../../Reference/im/MimMatch.md)), it will be done when [`MimMatch`](../../Reference/im/MimMatch.md) is first called. When preprocessed explictly, the source and destination image buffers can be set to [`M_NULL`](../../Reference/im/MimMatch.md). If, however, the source or destination image buffer is provided, it will be used in the preprocess operation, to better optimize future calls.

## Parameters

### `MatchContextImId` *(in, AIL_ID)*

Specifies the identifier of the match image processing context. The context must have been allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_MATCH_CONTEXT`](../../Reference/im/MimAlloc.md). In addition, the match image processing context must already be configured with the identifier of the model image.

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The buffer must be an 8-bit unsigned image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer. The buffer must be either a 16-bit signed or a 32-bit floating-point image buffer.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the operation to perform. This parameter must be set to one of the following values.

*For specifying the operation to perform.*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Performs the match operation according to the settings of the match image processing context. |
| `M_PREPROCESS` | Preprocess the specified image processing context. Note that, if not called explicitly, this operation will be performed automatically with the first call to [`MimMatch`](../../Reference/im/MimMatch.md).

If the identifier of the source and/or destination buffer is not set to [`M_NULL`](../../Reference/im/MimMatch.md), information about the source and/or destination image buffer will be used in the preprocessing operation. |
