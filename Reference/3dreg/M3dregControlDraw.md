---
doctype: Reference
module: 3dreg
function: M3dregControlDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregControlDraw"
---

# M3dregControlDraw

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

> Control a setting of a draw 3D registration context.

## Syntax

```c
void M3dregControlDraw(
    AIL_ID     DrawContext3dregId,  //out
    AIL_INT64  Operation,           //in
    AIL_INT64  ControlType,         //in
    AIL_DOUBLE ControlValue         //in
)
```

## Description

This function controls a specified setting of a draw 3D registration context. These settings establish which pairwise 3D registration results to draw into the 3D graphics list (and how to draw them), when calling [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md).

You can inquire about most of these settings using [`M3dregInquireDraw`](../../Reference/3dreg/M3dregInquireDraw.md).

Note that, prior to performing the 3D registration operation, you must specify to save pairs information at each iteration, using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_SAVE_PAIRS_INFO`](../../Reference/3dreg/M3dregControl.md) set to [`M_TRUE`](../../Reference/3dreg/M3dregControl.md). This ensures that results are available to draw.

## Parameters

### `DrawContext3dregId` *(out, AIL_ID)*

Specifies the identifier of the draw 3D registration context to control. The draw 3D registration context must have been previously allocated on the required system using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dreg/M3dregAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For a draw 3D registration context

The following [`Operation`](../../Reference/3dreg/M3dregControlDraw.md), [`ControlType`](../../Reference/3dreg/M3dregControlDraw.md), and [`ControlValue`](../../Reference/3dreg/M3dregControlDraw.md) parameter settings are available to control draw 3D registration context settings.

---

### `M_ALL`

Applies the setting to all operations that support the control type.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_COLOR`

Sets the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
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
| `M_DEFAULT` | Specifies to use the default setting of the operation. |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_OPACITY`

Sets the opacity of the 3D graphic(s) added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies to use the default setting of the operation. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies to use the default setting of the operation. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_EXCLUDED_POINTS`

Sets whether and how to draw the points that are not paired to any other points. The resulting graphic will be of type [`M_GRAPHIC_TYPE_POINT_CLOUD`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dreg/M3dregControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_COLOR`

Sets the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
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
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_GRAY` *(default)* | Specifies the color gray. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_OPACITY`

Sets the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 100. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 1 pixel. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_OVERLAP_POINTS`

Sets whether and how to draw overlapping (paired) points. Overlapping points are points in the reference point cloud that have been paired with at least one target point. The resulting graphic will be of type [`M_GRAPHIC_TYPE_POINT_CLOUD`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dreg/M3dregControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_COLOR`

Sets the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
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
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` *(default)* | Specifies to draw the graphic for each registration result element with a different color, according to the mapping between each registration result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_COLOR_COMPONENT`

Sets the color of the points in the point cloud(s) using one of the container's components.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISTANCE_IMAGE` *(default)* | Specifies to color the point cloud according to an internally generated distance image; the distance image is created using the distances between paired points. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_COLOR_LIMITS`

Sets the default limits of the values in the component used to color the point cloud(s) added to the 3D graphics list. Values between the minimum and the maximum are remapped linearly to values between the minimum and maximum possible display values. The values beyond the minimum or maximum are saturated. When [`M_COLOR_USE_LUT`](../../Reference/3dreg/M3dregControlDraw.md) is [`M_TRUE`](../../Reference/3dreg/M3dregControlDraw.md), the minimum and maximum get mapped to the LUT's extreme values.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DATA_EXTREMES_GLOBAL` *(default)* | Specifies to use the global minimum and maximum of the buffer's data. If the buffer has multiple bands, a single minimum and maximum is used for all of the bands. Points with 0 confidence are ignored. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_COLOR_USE_LUT`

Sets whether to color the point cloud(s) added to the 3D graphics list by mapping each value of the component specified by [`M_COLOR_COMPONENT`](../../Reference/3dreg/M3dregControlDraw.md) to a color in a LUT, and then using that color to display the corresponding point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` | Specifies not to use a LUT. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_TRUE` *(default)* | Specifies to use a LUT. |

#### `M_OPACITY`

Sets the opacity of the 3D graphic(s) added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_PAIRS`

Sets whether and how to draw lines between paired points, when calling [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md). The resulting graphic(s) will be of type [`M_GRAPHIC_TYPE_LINE`](../../Reference/3dgra/M3dgraInquire.md) or [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |
| `M_ENABLE` | Specifies to draw the graphic(s). |

#### `M_COLOR`

Sets the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
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
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_OPACITY`

Sets the opacity of the 3D graphic(s) added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_GLOBAL_DRAW_SETTINGS`

Sets a global draw 3D registration context setting.

#### `M_PSEUDO_COLOR_OFFSET`

Sets the offset to apply to the registration result element's index when drawing with [`M_PSEUDO_COLOR`](../../Reference/3dreg/M3dregControlDraw.md). You can use this control type to prevent the same colors from being assigned to different registration result elements when drawing several results at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the offset to apply to the index. |
