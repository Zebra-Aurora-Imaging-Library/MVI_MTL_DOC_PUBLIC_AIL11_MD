---
doctype: Reference
module: buf
function: MbufControlArea
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufControlArea"
---

# MbufControlArea

> Control a specified area of a buffer.

## Syntax

```c
void MbufControlArea(
    AIL_ID     BufId,        //out
    AIL_INT    OffsetX,      //in
    AIL_INT    OffsetY,      //in
    AIL_INT    SizeX,        //in
    AIL_INT    SizeY,        //in
    AIL_INT    Band,         //in
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function allows you to control a specified area of a buffer.

## Parameters

### `BufId` *(out, AIL_ID)*

Specifies the identifier of the buffer to control.

### `OffsetX` *(in, AIL_INT)*

Specifies the horizontal pixel offset of the area.

### `OffsetY` *(in, AIL_INT)*

Specifies the vertical pixel offset of the area.

### `SizeX` *(in, AIL_INT)*

Specifies the width of the area, in pixels.

### `SizeY` *(in, AIL_INT)*

Specifies the height of the area, in pixels.

### `Band` *(in, AIL_INT)*

Specifies the band of the area. This parameter can be set to one of the following values.

*For the band of the area*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL_BANDS` *(default)* | Specifies all bands (for multi-band buffers). |
| `M_BLUE` | Specifies the blue color band (for RGB buffers). |
| `M_GREEN` | Specifies the green color band (for RGB buffers). |
| `M_RED` | Specifies the red color band (for RGB buffers). |
| `0 <= Value < #bands` | Specifies the index of the band.

The relationship between index value and band for RGB, HSL, HSV, and YUV buffers is the following:

|   |   |
| --- | --- |
| 0 | Corresponds to the red band for RGB buffers, the hue band for HSL and HSV buffers, or the Y band for YUV buffers. |
| 1 | Corresponds to the green band for RGB buffers, or the saturation band for HSL and HSV buffers, or the U band for YUV buffers. |
| 2 | Corresponds to the blue band for RGB buffers, or the luminance band for HSL buffers, or the V band for YUV and HSV buffers. | |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of buffer setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value required for the setting.

## Parameter Associations

### For the buffer settings

---

### `M_MODIFIED`

Signals Aurora Imaging Library that the content in the specified area of the buffer was modified without using Aurora Imaging Library. This control should be used to ensure that Aurora Imaging Library updates its internal information on this area of the buffer. For example, if an area of a buffer that is selected on a display is modified using a pointer to its memory, the display will not be updated automatically. You should use this control type to force a display update of this area.  Note that this setting does the same thing as using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MODIFIED`](../../Reference/buf/MbufControl.md), but it is faster because it updates a specific area of the buffer, instead of updating the whole buffer.  This setting increments the buffer's modification counter. You can inquire about the counter's current value using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_MODIFICATION_COUNT`](../../Reference/buf/MbufInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Implements the default behavior. |
