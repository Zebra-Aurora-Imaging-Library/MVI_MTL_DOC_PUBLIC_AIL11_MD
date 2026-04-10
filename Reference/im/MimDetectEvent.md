---
doctype: Reference
module: im
function: MimDetectEvent
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimDetectEvent"
---

# MimDetectEvent

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

> Detect if at least one of the pixels in the source image satisfies the specified condition.

## Syntax

```c
AIL_INT MimDetectEvent(
    AIL_ID     SrcImageBufId,  //in
    AIL_INT64  Condition,      //in
    AIL_DOUBLE CondLow,        //in
    AIL_DOUBLE CondHigh        //in
)
```

## Description

This function detects if at least one of the pixels in the source image satisfies the specified condition. To establish the location of the event, you can use [`MimLocateEvent`](../../Reference/im/MimLocateEvent.md).

You can limit this function's result to a region of the image buffer using a region of interest set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The buffer must be a 1-band image buffer.

### `Condition` *(in, AIL_INT64)*

Specifies the condition under which pixel values are considered an event.

*For conditions that use two limits*

| Value | Description |
| --- | --- |
| `M_IN_RANGE` | Specifies that pixels with values between [`CondLow`](../../Reference/im/MimDetectEvent.md) and [`CondHigh`](../../Reference/im/MimDetectEvent.md), inclusive, are considered events. |
| `M_OUT_RANGE` | Specifies that pixels with values less than [`CondLow`](../../Reference/im/MimDetectEvent.md), or greater than [`CondHigh`](../../Reference/im/MimDetectEvent.md), are considered events. |

*For conditions that use one limit*

| Value | Description |
| --- | --- |
| `M_EQUAL` | Specifies that only pixels with values equal to [`CondLow`](../../Reference/im/MimDetectEvent.md) will be considered an event. |
| `M_GREATER` | Specifies that only pixels with values greater than [`CondLow`](../../Reference/im/MimDetectEvent.md) will be considered an event. |
| `M_GREATER_OR_EQUAL` | Specifies that only pixels with values greater than or equal to the [`CondLow`](../../Reference/im/MimDetectEvent.md) will be considered an event. |
| `M_LESS` | Specifies that only pixels with values less than [`CondLow`](../../Reference/im/MimDetectEvent.md) will be considered an event. |
| `M_LESS_OR_EQUAL` | Specifies that only pixels with values less than or equal to [`CondLow`](../../Reference/im/MimDetectEvent.md) will be considered an event. |
| `M_NOT_EQUAL` | Specifies that only pixels with values not equal to [`CondLow`](../../Reference/im/MimDetectEvent.md) will be considered an event. |

### `CondLow` *(in, AIL_DOUBLE)*

Specifies the lower limit of the selected condition.

*For specifying the lower limit*

| Value | Description |
| --- | --- |
| `Value` | Specifies the lower limit. |

### `CondHigh` *(in, AIL_DOUBLE)*

Specifies the upper limit of the selected condition.

*For specifying the upper limit*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no upper limit is required. This setting should be selected for conditions that use one limit. |
| `Value` | Specifies the upper limit. |

## Return Value

**Type:** `AIL_INT`

The returned value is [`M_TRUE`](../../Reference/im/MimDetectEvent.md) if a pixel with the specified condition is found and [`M_FALSE`](../../Reference/im/MimDetectEvent.md) otherwise.
