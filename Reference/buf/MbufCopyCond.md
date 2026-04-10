---
doctype: Reference
module: buf
function: MbufCopyCond
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufCopyCond"
---

# MbufCopyCond

> Copy conditionally the source buffer to the destination buffer.

## Syntax

```c
void MbufCopyCond(
    AIL_ID     SrcBufId,   //in
    AIL_ID     DestBufId,  //out
    AIL_ID     CondBufId,  //in
    AIL_INT64  Condition,  //in
    AIL_DOUBLE CondValue   //in
)
```

## Description

This function copies the source buffer data to the destination buffer, modifying only those entries of the destination buffer which have a corresponding entry in the condition buffer that satisfies the specified condition. Other entries are unchanged. If the source and destination buffers are of different data types, Aurora Imaging Library converts the data automatically.

If the source buffer depth is greater than that of the destination, the most-significant bits are truncated when the data is copied to the destination. If the destination depth is greater than that of the source, the source data is zero or sign-extended (depending on the type of the source) when copied to the destination. For example, the data is zero-extended when copying an 8-bit unsigned buffer to a 16-bit unsigned buffer.

> **Note:** Note that if a 1-band condition buffer is used with a multi-band destination buffer, the one band of the condition buffer will be used for each destination band.

Note, all multi-band buffers passed to this function must have the same number of bands.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source data buffer.

### `DestBufId` *(out, AIL_ID)*

Specifies the identifier of the destination data buffer.

### `CondBufId` *(in, AIL_ID)*

Specifies the identifier of the condition buffer.

### `Condition` *(in, AIL_INT64)*

Specifies the condition for which the condition buffer is tested. This parameter can be set to one of the following:

*For specifying the condition for which the buffer is tested*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Modify destination buffer entries corresponding to condition buffer entries that are non-zero. |
| `M_EQUAL` | Modify destination buffer entries corresponding to condition buffer entries that are equal to [`CondValue`](../../Reference/buf/MbufCopyCond.md). |
| `M_NOT_EQUAL` | Modify destination buffer entries corresponding to condition buffer entries that are not equal to [`CondValue`](../../Reference/buf/MbufCopyCond.md). |

### `CondValue` *(in, AIL_DOUBLE)*

Specifies the color value for the specified condition. Even though this value is of type _AIL_DOUBLE_, it is treated as if it had the same type and depth as the condition buffer. If the [`Condition`](../../Reference/buf/MbufCopyCond.md) parameter is set to [`M_DEFAULT`](../../Reference/buf/MbufCopyCond.md), [`CondValue`](../../Reference/buf/MbufCopyCond.md) is ignored.

*For specifying a color value*

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when the source buffer, destination buffer, and condition buffer are 8-bit, and the condition buffer is a 3-band buffer.

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
| `Value` | Specifies a grayscale value.

If the condition buffer is binary, this value must be set to 0 or 1.

If the condition buffer is multi-band, this value will be used for each band individually. |
