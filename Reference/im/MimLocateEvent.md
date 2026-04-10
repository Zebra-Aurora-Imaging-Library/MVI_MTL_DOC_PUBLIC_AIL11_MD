---
doctype: Reference
module: im
function: MimLocateEvent
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimLocateEvent"
---

# MimLocateEvent

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

> Find pixel coordinates or values that satisfy a specified condition.

## Syntax

```c
AIL_INT MimLocateEvent(
    AIL_ID     SrcImageBufId,    //in
    AIL_ID     EventResultImId,  //out
    AIL_INT64  Condition,        //in
    AIL_DOUBLE CondLow,          //in
    AIL_DOUBLE CondHigh          //in
)
```

## Description

This function finds the coordinates and value of the pixels that satisfy the specified condition in the specified source image. Results are stored in the specified event result buffer.

You can retrieve the values, X- or Y-coordinates, and the number of pixels that satisfy the condition, using [`MimGetResult`](../../Reference/im/MimGetResult.md) or [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md). If the result buffer does not have the required number of entries to hold the total number of events that satisfy the condition, the number returned will be limited to the number of entries in the result buffer.

You can limit this function's results to a region of the source image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image for the operation. This parameter must be given the identifier of a 1-band buffer.

### `EventResultImId` *(out, AIL_ID)*

Specifies the identifier of the buffer in which to store the event results. This parameter must be given the identifier of an image processing result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with an [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md) type. The buffer must have enough entries to hold the expected number of events.

### `Condition` *(in, AIL_INT64)*

Specifies the condition under which pixel values are considered an event. This parameter must be set to one of the values below.

*For conditions that use two limits*

| Value | Description |
| --- | --- |
| `M_IN_RANGE` | Specifies that pixels with values between [`CondLow`](../../Reference/im/MimLocateEvent.md) and [`CondHigh`](../../Reference/im/MimLocateEvent.md), inclusive, are considered events. |
| `M_OUT_RANGE` | Specifies that pixels with values less than [`CondLow`](../../Reference/im/MimLocateEvent.md), or greater than [`CondHigh`](../../Reference/im/MimLocateEvent.md), are considered events. |

*For conditions that use one limit*

| Value | Description |
| --- | --- |
| `M_EQUAL` | Specifies that only pixels with values equal to [`CondLow`](../../Reference/im/MimLocateEvent.md) will be considered an event. |
| `M_GREATER` | Specifies that only pixels with values greater than [`CondLow`](../../Reference/im/MimLocateEvent.md) will be considered an event. |
| `M_GREATER_OR_EQUAL` | Specifies that only pixels with values greater than or equal to the [`CondLow`](../../Reference/im/MimLocateEvent.md) will be considered an event. |
| `M_LESS` | Specifies that only pixels with values less than [`CondLow`](../../Reference/im/MimLocateEvent.md) will be considered an event. |
| `M_LESS_OR_EQUAL` | Specifies that only pixels with values less than or equal to [`CondLow`](../../Reference/im/MimLocateEvent.md) will be considered an event. |
| `M_NOT_EQUAL` | Specifies that only pixels with values not equal to [`CondLow`](../../Reference/im/MimLocateEvent.md) will be considered an event. |

*For conditions that use no limit*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that all pixels satisfying the local maxima or local minima condition will be considered an event. If you do not specify a local maxima or local minima condition, [`M_ALL`](../../Reference/im/MimLocateEvent.md) results in the coordinates and value of all the source pixels. |

*For specifying a local maxima or local minima as part of the condition*

| Value | Description |
| --- | --- |
| `M_LOCAL_MAX_NOT_STRICT` | Specifies that within a 3x3 neighborhood of a pixel, the pixel must be equal to or greater than all of its neighbors for it to be considered an event.

|   |   |   |
| --- | --- | --- |
| =&lt; | =&lt; | =&lt; |
| =&lt; | X | =&lt; |
| =&lt; | =&lt; | =&lt; |

The following is an example that shows a pixel, at the center of its 3x3 neighborhood, that satisfies the [`M_LOCAL_MAX_NOT_STRICT`](../../Reference/im/MimLocateEvent.md) condition.

|   |   |   |
| --- | --- | --- |
| 5 | 5 | 5 |
| 5 | 5 | 5 |
| 5 | 5 | 5 | |
| `M_LOCAL_MAX_STRICT_MEDIUM` | Specifies that within a 3x3 neighborhood of a pixel, the pixel must be equal to or greater than all of its neighbors, and it must also be greater than the pixels on its left and the pixel below, for it to be considered an event.

|   |   |   |
| --- | --- | --- |
| &lt; | =&lt; | =&lt; |
| &lt; | X | =&lt; |
| &lt; | &lt; | =&lt; |

The following is an example that shows a pixel, at the center of its 3x3 neighborhood, that satisfies the [`M_LOCAL_MAX_STRICT_MEDIUM`](../../Reference/im/MimLocateEvent.md) condition.

|   |   |   |
| --- | --- | --- |
| 4 | 5 | 5 |
| 4 | 5 | 5 |
| 4 | 4 | 5 | |
| `M_LOCAL_MIN_NOT_STRICT` | Specifies that within a 3x3 neighborhood of a pixel, the pixel must be equal to or less than all of its neighbors for it to be considered an event.

|   |   |   |
| --- | --- | --- |
| => | => | => |
| => | X | => |
| => | => | => |

The following is an example that shows a pixel, at the center of its 3x3 neighborhood, that satisfies the [`M_LOCAL_MIN_NOT_STRICT`](../../Reference/im/MimLocateEvent.md) condition.

|   |   |   |
| --- | --- | --- |
| 4 | 4 | 4 |
| 4 | 4 | 4 |
| 4 | 4 | 4 | |
| `M_LOCAL_MIN_STRICT_MEDIUM` | Specifies that within a 3x3 neighborhood of a pixel, the pixel must be equal to or less than all of its neighbors, and it must also be less than the pixels on its left and the pixel below, for it to be considered an event.

|   |   |   |
| --- | --- | --- |
| > | => | => |
| > | X | => |
| > | > | => |

The following is an example that shows a pixel, at the center of its 3x3 neighborhood, that satisfies the [`M_LOCAL_MIN_STRICT_MEDIUM`](../../Reference/im/MimLocateEvent.md) condition.

|   |   |   |
| --- | --- | --- |
| 5 | 4 | 4 |
| 5 | 4 | 4 |
| 5 | 5 | 4 | |

### `CondLow` *(in, AIL_DOUBLE)*

Specifies the lower limit of the selected condition.

*For specifying the lower limit*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no lower limit is required. This setting should be selected for conditions that use [`M_ALL`](../../Reference/im/MimLocateEvent.md). |
| `Value` | Specifies the lower limit. |

### `CondHigh` *(in, AIL_DOUBLE)*

Specifies the upper limit of the selected condition.

*For specifying the upper limit*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no upper limit is required. This setting should be selected for conditions that use one limit or [`M_ALL`](../../Reference/im/MimLocateEvent.md). |
| `Value` | Specifies the upper limit. |

## Return Value

**Type:** `AIL_INT`

If [`EventResultImId`](../../Reference/im/MimLocateEvent.md) is `M_NULL`, the returned value is the number of pixels that satisfy the specified condition in the specified source image. If [`EventResultImId`](../../Reference/im/MimLocateEvent.md) is not `M_NULL`, the returned value is `M_INVALID`. In that case, use either [`MimGetResult`](../../Reference/im/MimGetResult.md) or [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md), with [`M_NB_EVENT`](../../Reference/im/MimGetResult.md) as the result type, to retrieve the number of pixels that satisfy the specified condition.
