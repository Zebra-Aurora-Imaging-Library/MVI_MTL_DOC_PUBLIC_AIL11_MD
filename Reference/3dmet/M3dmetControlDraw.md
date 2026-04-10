---
doctype: Reference
module: 3dmet
function: M3dmetControlDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetControlDraw"
---

# M3dmetControlDraw

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

> Control a setting of a draw 3D metrology context.

## Syntax

```c
void M3dmetControlDraw(
    AIL_ID     DrawContext3dmetId,  //out
    AIL_INT64  Operation,           //in
    AIL_INT64  ControlType,         //in
    AIL_DOUBLE ControlValue         //in
)
```

## Description

This function controls a specified setting of a draw 3D metrology context. These settings establish which metrology results to draw into the 3D graphics list (and how to draw them), when calling [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md).

You can inquire about most of these settings using [`M3dmetInquireDraw`](../../Reference/3dmet/M3dmetInquireDraw.md).

## Parameters

### `DrawContext3dmetId` *(out, AIL_ID)*

Specifies the identifier of the draw 3D metrology context to control. The 3D metrology context must have been previously allocated on the system using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For controlling a draw 3D metrology context setting

The following [`Operation`](../../Reference/3dmet/M3dmetControlDraw.md), [`ControlType`](../../Reference/3dmet/M3dmetControlDraw.md), and [`ControlValue`](../../Reference/3dmet/M3dmetControlDraw.md) parameter settings are available to control draw 3D metrology context settings.

---

### `M_ALL`

Applies the setting to all operations that support the control type.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmet/M3dmetControlDraw.md))* |  |
| `M_DEFAULT` | Specifies to use the default setting of the operation. |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmet/M3dmetControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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

#### `M_OPACITY`

Sets the opacity with which to draw the graphic(s).

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

### `M_DRAW_VOLUME_ELEMENTS`

Sets whether and how to draw the volume elements used to compute the volume, independent of whether the elements contributed positively or negatively to the volume.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to draw the graphic(s). |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmet/M3dmetControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |
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
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |
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
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_VOLUME_ELEMENT_APPEARANCE`

Sets whether to draw the volume element(s) as a surface or volume.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SURFACE` | Specifies to draw the surfaces of the source and reference objects that were specified for the [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation. |
| `M_SURFACE_REFERENCE` | Specifies to draw only the surfaces of the reference object that was specified for the [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation. |
| `M_SURFACE_SOURCE` | Specifies to draw only the surfaces of the source object that was specified for the [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation. |
| `M_VOLUME` *(default)* | Specifies to draw each volume element as a distinguishable volume. The volume elements are the individual volumes that were used to calculate the volume during the [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation.  Depending on the source and reference objects specified for the volume operation, Aurora Imaging Library draws each volume element as a triangular prism (point cloud source with a plane reference), a triangular pyramid (point cloud source without a reference), or a rectangular prism (depth map source with or without a reference).  > **Note:** Note that to render transparency for a volume (that is, when [`M_OPACITY`](../../Reference/3dmet/M3dmetControlDraw.md) is less than 100.0), a powerful GPU is required, especially when the number of volume elements is large. |

---

### `M_DRAW_VOLUME_NEGATIVE_ELEMENTS`

Sets whether and how to draw the volume elements that contributed negatively to the volume computation; these elements decreased the volume.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmet/M3dmetControlDraw.md))* |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmet/M3dmetControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |
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
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_RED` *(default)* | Specifies the color red. |
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
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_VOLUME_ELEMENT_APPEARANCE`

Sets whether to draw the volume element(s) as a surface or volume.

| Value | Description |
| --- | --- |
| *(see [`M_VOLUME_ELEMENT_APPEARANCE`](Reference/3dmet/M3dmetControlDraw.md))* |  |
| `M_VOLUME` *(default)* | Specifies to draw each volume element as a distinguishable volume. The volume elements are the individual volumes that were used to calculate the volume during the [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation.  Depending on the source and reference objects specified for the volume operation, Aurora Imaging Library draws each volume element as a triangular prism (point cloud source with a plane reference), a triangular pyramid (point cloud source without a reference), or a rectangular prism (depth map source with or without a reference).  > **Note:** Note that to render transparency for a volume (that is, when [`M_OPACITY`](../../Reference/3dmet/M3dmetControlDraw.md) is less than 100.0), a powerful GPU is required, especially when the number of volume elements is large. |

---

### `M_DRAW_VOLUME_POSITIVE_ELEMENTS`

Sets whether and how to draw the volume elements that contributed positively to the volume computation; these elements increased the volume.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmet/M3dmetControlDraw.md))* |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmet/M3dmetControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |
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
| `M_COLOR_GREEN` *(default)* | Specifies the color green. |
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
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_VOLUME_ELEMENT_APPEARANCE`

Sets whether to draw the volume element(s) as a surface or volume.

| Value | Description |
| --- | --- |
| *(see [`M_VOLUME_ELEMENT_APPEARANCE`](Reference/3dmet/M3dmetControlDraw.md))* |  |
| `M_VOLUME` *(default)* | Specifies to draw each volume element as a distinguishable volume. The volume elements are the individual volumes that were used to calculate the volume during the [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation.  Depending on the source and reference objects specified for the volume operation, Aurora Imaging Library draws each volume element as a triangular prism (point cloud source with a plane reference), a triangular pyramid (point cloud source without a reference), or a rectangular prism (depth map source with or without a reference).  > **Note:** Note that to render transparency for a volume (that is, when [`M_OPACITY`](../../Reference/3dmet/M3dmetControlDraw.md) is less than 100.0), a powerful GPU is required, especially when the number of volume elements is large. |

---

### `M_GLOBAL_DRAW_SETTINGS`

Specifies a global draw 3D metrology context setting.

#### `M_VOLUME_ELEMENT_INDEX`

Sets the index of the volume element to draw.  You can use [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_VOLUME_ELEMENT_INDEX_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md) and [`M_VOLUME_ELEMENT_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md) to determine which volume elements were used in the volume computation and whether the contribution was positive, negative, or both.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to draw all the volume elements. |
| `Value >= 0` | Specifies the index. |
