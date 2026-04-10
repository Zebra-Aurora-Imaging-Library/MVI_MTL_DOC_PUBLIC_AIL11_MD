---
doctype: Reference
module: im
function: MimDeadPixelCorrection
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimDeadPixelCorrection"
---

# MimDeadPixelCorrection

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

> Correct dead pixels in an image.

## Syntax

```c
void MimDeadPixelCorrection(
    AIL_ID    DeadPixelContextImId,  //in
    AIL_ID    SrcImageBufId,         //in
    AIL_ID    DstImageBufId,         //out
    AIL_INT64 ControlFlag            //in
)
```

## Description

This function corrects dead pixels in the specified source image and places the resulting image in the destination image buffer. Dead pixels are replaced with an average of their non-dead pixel neighbors. Other pixels are copied from the source image buffer into the destination image buffer.

Before using this function, you must add the list of the coordinates of the dead pixels to the context, using [`MimPut`](../../Reference/im/MimPut.md) and two arrays (one for the X-coordinates and one for the Y-coordinates). Alternatively, you can add an image identifying the locations of the dead pixels to the context, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DEAD_PIXELS`](../../Reference/im/MimControl.md); this image acts as a dead pixel mask. In the dead pixel mask image, all non-zero pixels are considered dead pixels.

Specify the interpolation mode using [`MimControl`](../../Reference/im/MimControl.md) with [`M_INTERPOLATION_MODE`](../../Reference/im/MimControl.md). By default, the interpolation mode is [`M_AVERAGE`](../../Reference/im/MimControl.md).

The source and destination image buffers must be as large as (or larger than) the dead pixel mask, or equal to or larger than the largest specified coordinates of the dead pixels.

Once the dead pixel correction image processing context is configured, preprocess the context by calling [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) with [`M_PREPROCESS`](../../Reference/im/MimDeadPixelCorrection.md). If the preprocess operation was not done explicitly (using [`M_PREPROCESS`](../../Reference/im/MimStatCalculate.md)), it will be done when [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) is first called. When preprocessed explicitly, the source and destination image buffers can be set to [`M_NULL`](../../Reference/im/MimDeadPixelCorrection.md). If, however, the source or destination image buffer is provided, it will be used in the preprocess operation, to better optimize future calls.

## Parameters

### `DeadPixelContextImId` *(in, AIL_ID)*

Specifies the identifier of the dead pixels image processing context. The context must have been allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_DEAD_PIXEL_CONTEXT`](../../Reference/im/MimAlloc.md). In addition, the dead pixel image processing context must be already configured with the location of the dead pixels.

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The buffer must be a single-band image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer. The buffer must be a single-band image buffer.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the control flag. This parameter must be set to one of the following:

*For preprocessing the dead pixel image processing context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Corrects the dead pixels present in the source image. |
| `M_PREPROCESS` | Preprocesses the specified image processing context. Note that, if not called explicitly, this operation will be performed automatically upon the first call to [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md).

If the identifier of the source and/or destination buffer is not set to [`M_NULL`](../../Reference/im/MimDeadPixelCorrection.md), information about the source and/or destination image buffer will be used in the preprocessing operation. |

## Remarks

> In-place processing is supported, but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
