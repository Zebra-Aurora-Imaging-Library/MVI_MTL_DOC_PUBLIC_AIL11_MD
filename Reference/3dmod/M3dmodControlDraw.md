---
doctype: Reference
module: 3dmod
function: M3dmodControlDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodControlDraw"
---

# M3dmodControlDraw

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

> Control a setting of a draw 3D model finder context.

## Syntax

```c
void M3dmodControlDraw(
    AIL_ID     DrawContext3dmodId,  //out
    AIL_INT64  Operation,           //in
    AIL_INT64  ControlType,         //in
    AIL_DOUBLE ControlValue         //in
)
```

## Description

This function controls a specified setting of a draw 3D model finder context. These settings establish which features of a model or which results of found model occurrences to draw into the 3D graphics list (and how to draw them), when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md).

You can inquire about most of these settings using [`M3dmodInquireDraw`](../../Reference/3dmod/M3dmodInquireDraw.md).

## Parameters

### `DrawContext3dmodId` *(out, AIL_ID)*

Specifies the identifier of the draw 3D model finder context to control. The draw 3D model finder context must have been previously allocated on the required system using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with either [`M_DRAW_3D_GEOMETRIC_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) (to draw occurrences of geometric models) or [`M_DRAW_3D_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) (to draw surface models or occurrences of surface models).

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For a draw geometric or surface 3D model finder context

The following [`Operation`](../../Reference/3dmod/M3dmodControlDraw.md), [`ControlType`](../../Reference/3dmod/M3dmodControlDraw.md), and [`ControlValue`](../../Reference/3dmod/M3dmodControlDraw.md) parameter settings can be specified for a draw geometric or surface 3D model finder context.

---

### `M_ALL`

Applies the setting to all operations that support the control type.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `M_DEFAULT` | Specifies to use the default setting of the operation. |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmod/M3dmodControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies to use the default setting of the operation. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each occurrence with a different color, according to the mapping between each occurrence's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies to use the default setting of the operation. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_AXES`

Sets whether and how to draw the axes of either the model or each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md), depending on whether a find 3D model finder context or result buffer is specified. The resulting graphic will be of type [`M_GRAPHIC_TYPE_AXIS`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |

#### `M_LENGTH`

Sets the length of the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DEFAULT_LENGTH` *(default)* | Specifies that the length is the default size of the 3D model. |
| `Value > 0.0` | Specifies the length, in world units. |

#### `M_REFERENCE_X`

Sets the X-position of the graphic(s) origin, relative to the model or occurrence.  Note that [`M_REFERENCE_X`](../../Reference/3dmod/M3dmodControlDraw.md) is only available for a draw surface 3D model finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER` *(default)* | Specifies to draw the graphic(s) at the center of the model or occurrence. |
| `M_MAX` | Specifies to draw the graphic(s) at the model's or occurrence's maximum X-coordinate. |
| `M_MIN` | Specifies to draw the graphic(s) at the model's or occurrence's minimum X-coordinate. |

#### `M_REFERENCE_Y`

Sets the Y-position of the graphic(s) origin, relative to the model or occurrence.  Note that [`M_REFERENCE_Y`](../../Reference/3dmod/M3dmodControlDraw.md) is only available for a draw surface 3D model finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER` *(default)* | Specifies to draw the graphic(s) at the center of the model or occurrence. |
| `M_MAX` | Specifies to draw the graphic(s) at the model's or occurrence's maximum Y-coordinate. |
| `M_MIN` | Specifies to draw the graphic(s) at the model's or occurrence's minimum Y-coordinate. |

#### `M_REFERENCE_Z`

Sets the Z-position of the graphic(s) origin, relative to the model or occurrence.  Note that [`M_REFERENCE_Z`](../../Reference/3dmod/M3dmodControlDraw.md) is only available for a draw surface 3D model finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER` *(default)* | Specifies to draw the graphic(s) at the center of the model or occurrence. |
| `M_MAX` | Specifies to draw the graphic(s) at the model's or occurrence's maximum Z-coordinate. |
| `M_MIN` | Specifies to draw the graphic(s) at the model's or occurrence's minimum Z-coordinate. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_BOX`

Sets whether and how to draw the bounding box of each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md). The bounding box is the smallest-axis aligned box that contains the ideal geometric shape or surface of the model at the location of the occurrence, depending on the model type. If you enable [`M_DRAW_MODEL`](../../Reference/3dmod/M3dmodControlDraw.md), the bounding box is the smallest axis-aligned box that contains that graphic. Note that it does not necessarily contain all inlier points. The resulting graphic will be of type [`M_GRAPHIC_TYPE_BOX`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the graphic(s). |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmod/M3dmodControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_WIREFRAME` *(default)* | Specifies a wireframe appearance. The 3D graphic(s) appear as a set of lines connecting its vertices. |

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each occurrence with a different color, according to the mapping between each occurrence's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_INDEX`

Sets whether and how to draw the index of each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_TEXT`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |

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
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_YELLOW` *(default)* | Specifies the color yellow. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each occurrence with a different color, according to the mapping between each occurrence's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_FONT_SIZE`

Sets the size of the font with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |
| `Value > 0.0` | Specifies the font size. This is the height of one line of text, in world units. |

#### `M_OPACITY`

Sets the opacity of the 3D graphic(s) added to the 3D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_REFERENCE_X`

Sets the X-position of the graphic(s) origin, relative to the occurrence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER` *(default)* | Specifies to draw the graphic(s) at the center of the occurrence. |
| `M_MAX` | Specifies to draw the graphic(s) at the occurrence's maximum X-coordinate. |
| `M_MIN` | Specifies to draw the graphic(s) at the occurrence's minimum X-coordinate. |

#### `M_REFERENCE_Y`

Sets the Y-position of the graphic(s) origin, relative to the occurrence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER` *(default)* | Specifies to draw the graphic(s) at the center of the occurrence. |
| `M_MAX` | Specifies to draw the graphic(s) at the occurrence's maximum Y-coordinate. |
| `M_MIN` | Specifies to draw the graphic(s) at the occurrence's minimum Y-coordinate. |

#### `M_REFERENCE_Z`

Sets the Z-position of the graphic(s) origin, relative to the occurrence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER` *(default)* | Specifies to draw the graphic(s) at the center of the occurrence. |
| `M_MAX` | Specifies to draw the graphic(s) at the occurrence's maximum Z-coordinate. |
| `M_MIN` | Specifies to draw the graphic(s) at the occurrence's minimum Z-coordinate. |

#### `M_RENDER_LAYER`

Sets the layer on which to draw the graphic(s).  The graphics on higher layer are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 9` *(default)* | Specifies the layer on which the graphic will be rendered. |

#### `M_TEXT_DIRECTION`

Sets the direction to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FLIP` | Specifies to draw the graphic(s) such that it is mirrored across the occurrence's X-axis. |
| `M_SAME` *(default)* | Specifies to draw the graphic(s) in the direction of the occurrence's X-axis, and facing the direction of its Z-axis. |

---

### `M_DRAW_INLIER_POINTS`

Sets whether and how to draw the inlier points, based on [`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md), of each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_DOTS`](../../Reference/3dgra/M3dgraInquire.md).  Note that for a surface model, [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) must be enabled.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `M_DEFAULT` | Specifies the default value.  The default value for draw geometric 3D model finder contexts is [`M_ENABLE`](../../Reference/3dmod/M3dmodControlDraw.md).  The default value for draw surface 3D model finder contexts is [`M_DISABLE`](../../Reference/3dmod/M3dmodControlDraw.md). |

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
| `M_PSEUDO_COLOR` *(default)* | Specifies to draw the graphic for each occurrence with a different color, according to the mapping between each occurrence's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_MODEL`

Sets whether and how to draw the defined model or each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md), depending on whether a find 3D model finder context or result buffer is specified. In the case of a geometric model occurrence, the resulting graphic will be of the same geometric type as the model. In the case of a surface model or surface model occurrence, the resulting graphic will be of type [`M_GRAPHIC_TYPE_POINT_CLOUD`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmod/M3dmodControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

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
| `M_DEFAULT` | Specifies the default value.  The default value for draw geometric 3D model finder contexts is [`M_PSEUDO_COLOR`](../../Reference/3dmod/M3dmodControlDraw.md).  The default value for draw surface 3D model finder contexts is either [`M_AUTO_COLOR`](../../Reference/3dmod/M3dmodControlDraw.md) for a model in a find surface 3D model finder context or [`M_PSEUDO_COLOR`](../../Reference/3dmod/M3dmodControlDraw.md) for a result in a find surface 3D model finder result buffer. |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_AUTO_COLOR` | Specifies to color the graphic according to specific components in the source container. If the container is a point cloud, the graphic is colored based on the reflectance component. If the reflectance component does not exist, the intensity component is used. If there is no reflectance or intensity, use [`M_PSEUDO_COLOR`](../../Reference/3dmod/M3dmodControlDraw.md). |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each occurrence with a different color, according to the mapping between each occurrence's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_RESERVED_POINTS`

Sets whether and how to draw the reserved points of each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_DOTS`](../../Reference/3dgra/M3dgraInquire.md).  Occurrences of all types of models have reserved points and you can draw them; only occurrences of cylinder and sphere models allow you to control the distance of reserved points (using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_RESERVED_POINTS_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |

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
| `M_PSEUDO_COLOR` *(default)* | Specifies to draw the graphic for each occurrence with a different color, according to the mapping between each occurrence's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_GLOBAL_DRAW_SETTINGS`

Sets a global draw 3D model finder context setting.

#### `M_PSEUDO_COLOR_OFFSET`

Sets the offset to apply to either the model's or the occurrence's index when drawing with [`M_PSEUDO_COLOR`](../../Reference/3dmod/M3dmodControlDraw.md). You can use this control type to prevent the same colors from being assigned to different occurrences when drawing several results at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the offset to apply to the index. |

### For a draw surface 3D model finder context

The following [`Operation`](../../Reference/3dmod/M3dmodControlDraw.md), [`ControlType`](../../Reference/3dmod/M3dmodControlDraw.md), and [`ControlValue`](../../Reference/3dmod/M3dmodControlDraw.md) parameter settings can be specified for a draw surface 3D model finder context.

---

### `M_DRAW_AXES_POSITION`

Sets whether and how to draw the axes at either the model's or each specified occurrence's position when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md), depending on whether a find 3D model finder context or result buffer is specified. The axes are drawn at the model's reference axis origin/the model's reference axis origin transformed at each occurrence. The resulting graphic will be of type [`M_GRAPHIC_TYPE_AXIS`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |

#### `M_LENGTH`

Sets the length of the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DEFAULT_LENGTH` *(default)* | Specifies that the length is the default size of the 3D model. |
| `Value > 0.0` | Specifies the length, in world units. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_BACKGROUND_POINTS`

Sets whether and how to draw the points that were considered part of the background during the search. Background points include those of objects that are too big or too small to be part of an occurrence of the model. The resulting graphic will be of type [`M_GRAPHIC_TYPE_DOTS`](../../Reference/3dgra/M3dgraInquire.md).  This drawing operation is only supported if [`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) was enabled.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each occurrence with a different color, according to the mapping between each occurrence's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_FLOOR_POINTS`

Sets whether and how to draw the points that were considered part of the floor during the search. Floor points are determined using the floor plane, defined using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md), and the floor removal direction and offset ([`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REMOVE_FLOOR_DIRECTION`](../../Reference/3dmod/M3dmodControl.md) and [`M_REMOVE_FLOOR_OFFSET`](../../Reference/3dmod/M3dmodControl.md)). The resulting graphic will be of type [`M_GRAPHIC_TYPE_DOTS`](../../Reference/3dgra/M3dgraInquire.md).  This drawing operation is only supported if [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md) was enabled.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |

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
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_MAGENTA` *(default)* | Specifies the color magenta. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each occurrence with a different color, according to the mapping between each occurrence's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_MODEL_PREPROCESSED`

Sets whether and how to draw the preprocessed model that is in the specified find 3D model finder context. The resulting graphic will be of type [`M_GRAPHIC_TYPE_POINT_CLOUD`](../../Reference/3dgra/M3dgraInquire.md).  [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md) must be called before calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |

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
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_YELLOW` *(default)* | Specifies the color yellow. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each occurrence with a different color, according to the mapping between each occurrence's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |
