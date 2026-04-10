---
doctype: Reference
module: im
function: MimFindExtreme
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimFindExtreme"
---

# MimFindExtreme

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

> Find an image buffer's extremes (minimum and/or maximum pixel values).

## Syntax

```c
void MimFindExtreme(
    AIL_ID    SrcImageBufId,      //in
    AIL_ID    ExtremeResultImId,  //out
    AIL_INT64 ExtremeType         //in
)
```

## Description

This function finds the maximum and/or minimum value of the specified source image and stores results in the specified extreme result buffer.

You can read the minimum and/or maximum from the result buffer, using [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) or [`MimGetResult`](../../Reference/im/MimGetResult.md), specifying [`M_VALUE`](../../Reference/im/MimGetResult.md) as the result type.

You can limit this function's results to a region of the source image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter must be given an image buffer identifier. The buffer must be 1-band.

### `ExtremeResultImId` *(out, AIL_ID)*

Specifies the identifier of the buffer in which to store the extreme values. This parameter must be given the identifier of an image processing result buffer that was allocated with [`MimAllocResult`](../../Reference/im/MimAllocResult.md) and that has an [`M_EXTREME_LIST`](../../Reference/im/MimAllocResult.md) type. If just the maximum or minimum is calculated, only one entry is needed. If both the minimum and maximum are calculated, the result buffer must have two entries. The minimum value is stored in the first entry and the maximum value is stored in the second.

### `ExtremeType` *(in, AIL_INT64)*

Specifies the type of extreme(s) to find.

*For specifying whether to find the minimum value, maximum value, or both*

| Value | Description |
| --- | --- |
| `M_MAX_VALUE` | Finds the maximum value. |
| `M_MIN_VALUE` | Finds the minimum value. |

*For specifying whether to find the minimum absolute value, maximum absolute value, or both*

| Value | Description |
| --- | --- |
| `M_MAX_ABS_VALUE` | Finds the maximum value, as an absolute value. |
| `M_MIN_ABS_VALUE` | Finds the minimum value, as an absolute value. |
