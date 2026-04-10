---
doctype: Reference
module: im
function: MimRearrange
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimRearrange"
---

# MimRearrange

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

> Copies specified areas of a source buffer to the specified areas of a destination buffer.

## Syntax

```c
void MimRearrange(
    AIL_ID    RearrangeContextImId,  //in
    AIL_ID    SrcImageBufId,         //in
    AIL_ID    DstImageBufId,         //out
    AIL_INT64 ControlFlag            //in
)
```

## Description

This function copies specified areas of a source buffer to specified areas of a destination buffer. The rest of the source image buffer is ignored and not copied.

Before using this function, you must configure the specified image processing context with which operation to perform, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODE`](../../Reference/im/MimControl.md). This specifies whether to copy rectangular areas or rows of the source buffer. In addition, you must specify the offsets and sizes of the source and destination areas, using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_SOURCE`](../../Reference/im/MimPut.md), [`M_XY_DESTINATION`](../../Reference/im/MimPut.md), and [`M_XY_SIZE`](../../Reference/im/MimPut.md). Note that each array in the rearrangement image processing context must have the same number of elements. The source and destination image buffers need not be of the same size, but must be equal to or larger than the largest supplied source and destination coordinates.

Once the rearrangement image processing context is configured, preprocess the context by calling [`MimRearrange`](../../Reference/im/MimRearrange.md) with [`M_PREPROCESS`](../../Reference/im/MimRearrange.md). If the preprocess operation is not done explicitly (using [`M_PREPROCESS`](../../Reference/im/MimStatCalculate.md)), it will be done when [`MimRearrange`](../../Reference/im/MimRearrange.md) is first called. When preprocessing explicitly, the source and destination image buffers can be set to [`M_NULL`](../../Reference/im/MimRearrange.md). If, however, the source or destination image buffer is provided, it will be used in the preprocess operation, to better optimize future calls.

## Parameters

### `RearrangeContextImId` *(in, AIL_ID)*

Specifies the identifier of the rearrangement image processing context that indicates how to perform the operation and the locations to copy. This context should be previously allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_REARRANGE_CONTEXT`](../../Reference/im/MimAlloc.md).

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the operation to perform. This parameter must be set to one of the following values.

*For specifying the operation to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Performs the rearrangement operation. |
| `M_PREPROCESS` | Preprocesses the specified image processing context. Note that, if not called explicitly, this operation will be performed automatically upon the first call to [`MimRearrange`](../../Reference/im/MimRearrange.md).

If the identifier of the source and/or destination image buffer is not set to [`M_NULL`](../../Reference/im/MimRearrange.md), information about the source and/or destination image buffer will be used in the preprocessing operation. |
