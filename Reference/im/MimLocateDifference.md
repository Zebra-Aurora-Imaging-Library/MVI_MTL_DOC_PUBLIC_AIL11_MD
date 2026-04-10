---
doctype: Reference
module: im
function: MimLocateDifference
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimLocateDifference"
---

# MimLocateDifference

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

> Locate the differences between 2 buffers, with some tolerance.

## Syntax

```c
AIL_INT MimLocateDifference(
    AIL_ID     LocateDifferenceContextImId,  //in
    AIL_ID     Src1ImageBufId,               //in
    AIL_ID     Src2ImageBufId,               //in
    AIL_ID     LocateDifferenceResultImId,   //out
    AIL_DOUBLE AbsTolerance,                 //in
    AIL_DOUBLE RelTolerance,                 //in
    AIL_INT64  ControlFlag                   //in
)
```

## Description

This function locates the differences between 2 buffers, with some tolerance, and put the positions and values of the 2 sources in the result object. Absolute and relative tolerances will be taken in account to determine if the pixels are different. You can limit this function's results to a region of the source image buffers using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

## Parameters

### `LocateDifferenceContextImId` *(in, AIL_ID)*

Specifies the predefined locate difference context to use.

*For specifying the locate difference context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LOCATE_DIFFERENCE_CONTEXT_TOLERANCE_MAX` *(default)* | Specifies a predefined locate difference context that takes the maximum of the absolute tolerance and the relative tolerance applied on the maximum absolute value. |
| `M_LOCATE_DIFFERENCE_CONTEXT_TOLERANCE_SUM` | Specifies a predefined locate difference context that takes the sum of the absolute tolerance and the relative tolerance applied on the maximum absolute value. |

### `Src1ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the first source image buffer.

### `Src2ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the second source image buffer.

### `LocateDifferenceResultImId` *(out, AIL_ID)*

Specifies the identifier of the image processing locate difference result buffer in which to write the results. The object must have previously allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md).

*For specifying the image processing locate difference result buffer identifier*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to not use an image processing locate difference result buffer. In this case, the function will return the number of differences. |
| `LocateDifferenceResultImId` | Specifies the identifier of the image processing locate difference result buffer in which to write the results. |

### `AbsTolerance` *(in, AIL_DOUBLE)*

Specifies an absolute tolerance.

*For specifying the absolute tolerance*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the absolute tolerance. |

### `RelTolerance` *(in, AIL_DOUBLE)*

Specifies a relative tolerance. Relative tolerance is applied on the highest absolute value.

*For specifying the relative tolerance*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the relative tolerance. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT`

If [`LocateDifferenceResultImId`](../../Reference/im/MimLocateDifference.md) is `M_NULL`, the returned value is the number of differences. If [`LocateDifferenceResultImId`](../../Reference/im/MimLocateDifference.md) is not `M_NULL`, the returned value is `M_INVALID`.
