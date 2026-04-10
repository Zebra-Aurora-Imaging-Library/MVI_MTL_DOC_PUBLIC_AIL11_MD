---
doctype: Reference
module: buf
function: MbufClearCond
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufClearCond"
---

# MbufClearCond

> Conditionally clears a buffer to a specified color.

## Syntax

```c
void MbufClearCond(
    AIL_ID     DstBufId,      //out
    AIL_DOUBLE RedOrMonoVal,  //in
    AIL_DOUBLE GreenVal,      //in
    AIL_DOUBLE BlueVal,       //in
    AIL_ID     CondBufId,     //in
    AIL_INT64  Condition,     //in
    AIL_DOUBLE CondValue      //in
)
```

## Description

This function clears a buffer to the specified color (the clear color), modifying only those entries of the buffer which have a corresponding entry in the condition buffer that satisfies the specified condition. Other entries are unchanged.

The clear color is specified by [`RedOrMonoVal`](../../Reference/buf/MbufClearCond.md), [`GreenVal`](../../Reference/buf/MbufClearCond.md), and [`BlueVal`](../../Reference/buf/MbufClearCond.md). If the buffer is one-band, only [`RedOrMonoVal`](../../Reference/buf/MbufClearCond.md) is used. The clear color is treated as if it had the same type and depth as the buffer.

> **Note:** Note that if a 1-band condition buffer is used with a 3-band buffer, the one band of the condition buffer will be used for each band in the buffer to clear.

For an image buffer, the clear can be limited to a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

## Parameters

### `DstBufId` *(out, AIL_ID)*

Specifies the identifier of the data buffer to conditionally clear. The data buffer must be a 1- or 3-band image, array, or LUT buffer, or 1-band kernel or structuring element buffer. If you specify an image buffer, it can have an ROI set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)).

### `RedOrMonoVal` *(in, AIL_DOUBLE)*

Specifies the red band of the clear color, or, if [`DstBufId`](../../Reference/buf/MbufClearCond.md) is a single band, this value is used as the clear color.

*For specifying the red or mono band*

| Value | Description |
| --- | --- |
| `Value` | Specifies the red band of the clear color, or the entire clear color. |

### `GreenVal` *(in, AIL_DOUBLE)*

Specifies the green band of the clear color.

*For specifying the green band*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that [`DstBufId`](../../Reference/buf/MbufClearCond.md) is a single band, and that this parameter is ignored. |
| `Value` | Specifies the green band of the clear color. |

### `BlueVal` *(in, AIL_DOUBLE)*

Specifies the blue band of the clear color.

*For specifying the blue band*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that [`DstBufId`](../../Reference/buf/MbufClearCond.md) is a single band, and that this parameter is ignored. |
| `Value` | Specifies the blue band of the clear color. |

### `CondBufId` *(in, AIL_ID)*

Specifies the identifier of the condition buffer. The condition buffer must be a 1- or 3-band image, array, or LUT buffer, or 1-band kernel or structuring element buffer. If you specify an image buffer, it can have a region of interest (ROI) associated with it, but it must be a raster region. Using an image buffer with an other type of ROI will cause an error.

### `Condition` *(in, AIL_INT64)*

Specifies the condition for which the condition buffer is tested. This parameter can be set to one of the following:

*For specifying the condition for which the buffer is tested*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Modify destination buffer entries corresponding to condition buffer entries that are non-zero. |
| `M_EQUAL` | Modify destination buffer entries corresponding to condition buffer entries that are equal to [`CondValue`](../../Reference/buf/MbufClearCond.md). |
| `M_NOT_EQUAL` | Modify destination buffer entries corresponding to condition buffer entries that are not equal to [`CondValue`](../../Reference/buf/MbufClearCond.md). |

### `CondValue` *(in, AIL_DOUBLE)*

Specifies the color value for the specified condition. Even though this value is of type _AIL_DOUBLE_, it is treated as if it had the same type and depth as the condition buffer. If the [`Condition`](../../Reference/buf/MbufClearCond.md) parameter is set to [`M_DEFAULT`](../../Reference/buf/MbufClearCond.md), [`CondValue`](../../Reference/buf/MbufClearCond.md) is ignored.

*For specifying a color value*

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value. This is only available when [`CondBufId`](../../Reference/buf/MbufClearCond.md) is a 3-band buffer, and both [`DstBufId`](../../Reference/buf/MbufClearCond.md) and [`CondBufId`](../../Reference/buf/MbufClearCond.md) are 8-bit buffers.

This allows you to compare each band of the condition buffer against a different value. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
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

*For specifying a grayscale value to be used with all bands*

| Value | Description |
| --- | --- |
| `Value` | Specifies an entry value.

If the condition buffer is binary, this value must be set to 0 or 1.

If the condition buffer is 3-band, this value will be used for each band individually. |
