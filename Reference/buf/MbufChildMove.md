---
doctype: Reference
module: buf
function: MbufChildMove
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufChildMove"
---

# MbufChildMove

> Move and resize a child buffer within the parent buffer.

## Syntax

```c
void MbufChildMove(
    AIL_ID    BufId,       //out
    AIL_INT   OffsetX,     //in
    AIL_INT   OffsetY,     //in
    AIL_INT   SizeX,       //in
    AIL_INT   SizeY,       //in
    AIL_INT64 ControlFlag  //in
)
```

## Description

This function moves and resizes a child buffer within the parent buffer. If the specified new offset or size causes the child buffer to be partially outside of the parent buffer, the child buffer can be clipped to the parent buffer (using [`M_CLIP`](../../Reference/buf/MbufChildMove.md)).

## Parameters

### `BufId` *(out, AIL_ID)*

Specifies the identifier of the child buffer to move and/or resize. This buffer should be allocated using [`MbufChild...`](../../Reference/buf/MbufChildColor.md).

### `OffsetX` *(in, AIL_INT)*

Specifies the new X-offset of the child buffer relative to the parent buffer's top-left pixel.

*For the X-offset*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default is the current X-offset of the child buffer. |
| `Value` | Specifies the X-offset, in pixels. |

### `OffsetY` *(in, AIL_INT)*

Specifies the new Y-offset of the child buffer relative to the parent's top-left pixel.

*For the Y-offset*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default is the current Y-offset of the child buffer. |
| `Value` | Specifies the Y-offset, in pixels. |

### `SizeX` *(in, AIL_INT)*

Specifies the new width of the child buffer.

*For the width*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default is the current width of the child buffer. |
| `Value > 0` | Specifies the width, in pixels. |

### `SizeY` *(in, AIL_INT)*

Specifies the new height of the child buffer.

*For the height*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default is the current height of the child buffer. |
| `Value > 0` | Specifies the height, in pixels. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter must be set to one of the following:

*For specifying the function's control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the values passed to the offset and size parameters as is, when moving the child buffer. Note that, if you try to move the child buffer outside the limits of the parent buffer, an error is reported. |
| `M_CLIP` | Specifies that the values passed to the offset and size parameters are automatically adjusted (if necessary) to ensure that the child buffer stays within the limits of the parent buffer. Note that, if you try to move the child buffer completely outside the limits of the parent buffer, an error is reported. |
