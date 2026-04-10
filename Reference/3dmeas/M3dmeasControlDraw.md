---
doctype: Reference
module: 3dmeas
function: M3dmeasControlDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasControlDraw"
---

# M3dmeasControlDraw

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

> Control a setting of a draw 3D measurement context.

## Syntax

```c
void M3dmeasControlDraw(
    AIL_ID     DrawContext3dmeasId,  //out
    AIL_INT64  Operation,            //in
    AIL_INT64  ControlType,          //in
    AIL_DOUBLE ControlValue          //in
)
```

## Description

This function controls a specified setting of a draw 3D measurement context. These settings establish which 3D measurement results to draw into the 3D graphics list (and how to draw them), when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

You can inquire about most of these settings using [`M3dmeasInquireDraw`](../../Reference/3dmeas/M3dmeasInquireDraw.md).

## Parameters

### `DrawContext3dmeasId` *(out, AIL_ID)*

Specifies the identifier of the draw 3D measurement context to control. The 3D measurement context must have been previously allocated on the system using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_DRAW_3D_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), [`M_DRAW_3D_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), or [`M_DRAW_3D_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For controlling a draw profile, path, or template 3D measurement context setting

The following [`Operation`](../../Reference/3dmeas/M3dmeasControlDraw.md), [`ControlType`](../../Reference/3dmeas/M3dmeasControlDraw.md), and [`ControlValue`](../../Reference/3dmeas/M3dmeasControlDraw.md) parameter settings are available to control draw profile, path, or template 3D measurement context settings.

---

### `M_ALL`

Applies the setting to all operations that support the control type.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_DEFAULT` | Specifies to use the default setting of the operation. |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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

### `M_DRAW_MARKERS`

Sets whether and how to draw the cross-like symbol at each specified marker's position, and for pair markers, a rectangle connecting the two transitions with a cross-like symbol at each transition position, when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md). Note that this is equivalent to [`M_DRAW_MARKERS_POSITION`](../../Reference/3dmeas/M3dmeasControlDraw.md) for single markers and [`M_DRAW_MARKERS_POSITION`](../../Reference/3dmeas/M3dmeasControlDraw.md) + [`M_DRAW_MARKERS_WIDTH`](../../Reference/3dmeas/M3dmeasControlDraw.md) for pair markers.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points. Note that a cross-like symbol drawn with a solid appearance is the same as a cross-like symbol drawn with a wireframe appearance.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` *(default)* | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_MARKERS_POSITION`

Sets whether and how to draw the cross-like symbol at each specified marker's position when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md). Note that this is equivalent to [`M_DRAW_MARKERS`](../../Reference/3dmeas/M3dmeasControlDraw.md) for single markers.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points. Note that a cross-like symbol drawn with a solid appearance is the same as a cross-like symbol drawn with a wireframe appearance.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` *(default)* | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_MARKERS_WIDTH`

Sets whether and how to draw the width of each specified pair marker when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The width is drawn as a rectangle connecting the two transitions with a cross-like symbol at each transition position. The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md).  This operation is only supported for pair markers.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points. Note that a cross-like symbol drawn with a solid appearance is the same as a cross-like symbol drawn with a wireframe appearance.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` *(default)* | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_PROFILE_EXTRACTED`

Sets whether and how to draw the extracted points that form each specified profile when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). Note that this includes points introduced after filling gaps. The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_YELLOW` *(default)* | Specifies the color yellow. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_PROFILE_REGION`

Sets whether and how to draw the region where each specified profile was extracted when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_POLYGON`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_MAGENTA` *(default)* | Specifies the color magenta. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

#### `M_OPACITY`

Sets the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 20. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_PROFILE_RESPONSE`

Sets whether and how to draw the response of each specified profile (after applying the filter) when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_CYAN` *(default)* | Specifies the color cyan. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_TRANSITIONS`

Sets whether and how to draw the step-like symbol at each specified marker's transition position when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` *(default)* | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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

---

### `M_GLOBAL_DRAW_SETTINGS`

Sets a global draw 3D measurement context setting.

#### `M_PSEUDO_COLOR_OFFSET`

Sets the offset to apply to either the 3D measurement result element's index when drawing with [`M_PSEUDO_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md) or [`M_PSEUDO_DARK_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md). You can use this control type to prevent the same colors from being assigned to different result elements when drawing several results at once.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the offset to apply to the index. |

### For controlling a draw path 3D measurement context setting

The following [`Operation`](../../Reference/3dmeas/M3dmeasControlDraw.md), [`ControlType`](../../Reference/3dmeas/M3dmeasControlDraw.md), and [`ControlValue`](../../Reference/3dmeas/M3dmeasControlDraw.md) parameter settings are available to control draw path 3D measurement context settings.

---

### `M_DRAW_PATH`

Sets whether and how to draw the path defined in the path 3D measurement context when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINE`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_MAGENTA` *(default)* | Specifies the color magenta. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_PATH_PROJECTED`

Sets whether and how to draw the projection of the path defined in the path 3D measurement context when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_POLYGON`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_MAGENTA` *(default)* | Specifies the color magenta. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

#### `M_OPACITY`

Sets the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 50. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

### For controlling a draw template 3D measurement context setting

The following [`Operation`](../../Reference/3dmeas/M3dmeasControlDraw.md), [`ControlType`](../../Reference/3dmeas/M3dmeasControlDraw.md), and [`ControlValue`](../../Reference/3dmeas/M3dmeasControlDraw.md) parameter settings are available to control draw template 3D measurement context settings.

---

### `M_DRAW_FIND_GEOMETRY`

Sets whether and how to draw the line segments connecting the extracted markers when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |
| `M_ENABLE` | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_CYAN` *(default)* | Specifies the color cyan. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_FIND_GEOMETRY_PROJECTED`

Sets whether and how to draw the planes connecting the projections of the extracted markers when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_POINT_CLOUD`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_DARK_CYAN` *(default)* | Specifies the color dark cyan. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

#### `M_OPACITY`

Sets the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 50. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_FIT_GEOMETRY`

Sets whether and how to draw the fitted 3D geometry when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINE`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_YELLOW` *(default)* | Specifies the color yellow. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_FIT_GEOMETRY_PROJECTED`

Sets whether and how to draw the projection of the fitted 3D geometry when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_POLYGON`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_DARK_YELLOW` *(default)* | Specifies the color dark yellow. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

#### `M_OPACITY`

Sets the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 50. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_FIT_MARKERS`

Sets whether and how to draw the cross-like symbol at the position of each marker that was used for the fit operation when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points. Note that a cross-like symbol drawn with a solid appearance is the same as a cross-like symbol drawn with a wireframe appearance.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_FIT_MARKERS_OUTLIERS`

Sets whether and how to draw the cross-like symbol at the position of each marker that was considered an outlier for the fit operation when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points. Note that a cross-like symbol drawn with a solid appearance is the same as a cross-like symbol drawn with a wireframe appearance.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_RED` *(default)* | Specifies the color red. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_TEMPLATE`

Sets whether and how to draw the template defined in the template 3D measurement context when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_LINE`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |
| `M_ENABLE` *(default)* | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_MAGENTA` *(default)* | Specifies the color magenta. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

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
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_TEMPLATE_PROJECTED`

Sets whether and how to draw the projection of the template defined in the template 3D measurement context when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The resulting graphic will be of type [`M_GRAPHIC_TYPE_POLYGON`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets whether to draw the graphic(s) as a solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dmeas/M3dmeasControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to use as the color. |
| `M_COLOR_MAGENTA` *(default)* | Specifies the color magenta. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different color, according to the mapping between each result element's index and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |
| `M_PSEUDO_DARK_COLOR` | Specifies to draw the graphic for each 3D measurement result element with a different dark color, according to the mapping between each element's index and a distinct dark color corresponding to those in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap, where the dark color has the same hue but a lower luminance. |

#### `M_OPACITY`

Sets the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 50. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |
