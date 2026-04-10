---
doctype: Reference
module: 3dblob
function: M3dblobControlDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobControlDraw"
---

# M3dblobControlDraw

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

> Control a setting of a draw 3D blob analysis context.

## Syntax

```c
void M3dblobControlDraw(
    AIL_ID     DrawContext3dblobId,  //out
    AIL_INT64  Operation,            //in
    AIL_INT64  ControlType,          //in
    AIL_DOUBLE ControlValue          //in
)
```

## Description

This function controls a specified setting of a draw 3D blob analysis context. These settings establish which blob features to draw into the 3D graphics list (and how to draw them), when calling [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md).

You can inquire about most of these settings using [`M3dblobInquireDraw`](../../Reference/3dblob/M3dblobInquireDraw.md).

## Parameters

### `DrawContext3dblobId` *(out, AIL_ID)*

Specifies the identifier of the draw 3D blob analysis context to control. The 3D blob analysis context must have been previously allocated on the system using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For controlling a draw blob setting

The following [`Operation`](../../Reference/3dblob/M3dblobControlDraw.md), [`ControlType`](../../Reference/3dblob/M3dblobControlDraw.md), and [`ControlValue`](../../Reference/3dblob/M3dblobControlDraw.md) parameter settings can be specified for different types of features.

---

### `M_ALL`

Applies the setting to all operations that support the control type.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `M_DEFAULT` | Specifies to use the default setting of the operation. |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dblob/M3dblobControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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

### `M_DRAW_BLOBS`

Sets whether and how to draw the points inside each specified blob.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |
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
| `M_PSEUDO_COLOR` *(default)* | Specifies to draw the graphic for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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

### `M_DRAW_BOX`

Sets whether and how to draw the bounding box of each specified blob. [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to draw the graphic(s). |
| `M_ENABLE` | Specifies to draw the graphic(s). |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dblob/M3dblobControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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

### `M_DRAW_EXCLUDED_POINTS`

Sets whether and how to draw the points that are not part of each specified blob (or not part of any blob).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

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
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_FERET_GENERAL`

Sets whether and how to draw the custom Feret of each specified blob. [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobControl.md) and [`M_FERET_CONTACT_POINTS`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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

### `M_DRAW_FERET_MAX`

Sets whether and how to draw the maximum diameter Feret of each specified blob. [`M_FERET_MAX_DIAMETER`](../../Reference/3dblob/M3dblobControl.md) and [`M_FERET_CONTACT_POINTS`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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

### `M_DRAW_FERET_MIN`

Sets whether and how to draw the minimum diameter Feret of each specified blob. [`M_FERET_MIN_DIAMETER`](../../Reference/3dblob/M3dblobControl.md) and [`M_FERET_CONTACT_POINTS`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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

### `M_DRAW_INDEX`

Sets whether and how to draw the index at the centroid of each specified blob.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

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
| `M_PSEUDO_COLOR` | Specifies to draw the index for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_FONT_SIZE`

Sets the size of the font with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |
| `Value > 0.0` | Specifies the font size. This is the height of one line of text, in world units. |

#### `M_OPACITY`

Sets the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 100. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_RENDER_LAYER`

Sets the render layer on which to draw the graphic(s). The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 2. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_LABEL`

Sets whether and how to draw the label at the centroid of each specified blob.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

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
| `M_PSEUDO_COLOR` | Specifies to draw the label for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

#### `M_FONT_SIZE`

Sets the size of the font with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GRAPHIC_LIST_DEFAULT` *(default)* | Specifies to use the 3D graphics list's default setting. |
| `Value > 0.0` | Specifies the font size. This is the height of one line of text, in world units. |

#### `M_OPACITY`

Sets the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 100. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

#### `M_RENDER_LAYER`

Sets the render layer on which to draw the graphic(s). The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_DEFAULT` | Specifies the default value; the default value is 2. |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_NEAREST_BLOB`

Sets whether and how to draw the shortest line from each specified blob to its nearest blob. [`M_NEAREST_BLOB`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand. If [`M_NEAREST_BLOB`](../../Reference/3dblob/M3dblobControl.md) was calculated but there was no nearest blob, nothing is drawn.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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

### `M_DRAW_PCA`

Sets whether and how to draw the principal component of each specified blob; this is drawn as perpendicular axes centered on each blob's centroid and oriented along the axis directions determined from a principal component analysis (PCA). [`M_PCA`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Sets the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |

---

### `M_DRAW_PCA_BOX`

Sets whether and how to draw the PCA-aligned bounding box of each specified blob. [`M_PCA_BOX`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dblob/M3dblobControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dgra/M3dgraControl.md))* |  |
| `M_GRAPHIC_LIST_DEFAULT` | Specifies to use the 3D graphics list's default setting. |
| `M_WIREFRAME` *(default)* | Specifies a wireframe appearance. The graphic(s) appear as a set of lines connecting its vertices. |

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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

### `M_DRAW_SEMI_ORIENTED_BOX`

Specifies whether and how to draw the semi-oriented bounding box of each specified blob. [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_APPEARANCE`

Sets the appearance of the graphic(s) to solid surface, wireframe, or points.  The color of the points, wireframe, or outline is determined by [`M_COLOR`](../../Reference/3dblob/M3dblobControlDraw.md), while the color of the solid surface is determined by the default fill color of the 3D graphics list ([`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md)).

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
| `M_PSEUDO_COLOR` | Specifies to draw the graphic for each blob with a different color, according to the mapping between each blob's label value and a distinct color in the [`M_COLORMAP_DISTINCT_256`](../../Reference/gen/MgenLutFunction.md) colormap. |

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

Sets a global draw 3D blob analysis context setting.

#### `M_PSEUDO_COLOR_OFFSET`

Sets the offset to apply to the blob's label when drawing with [`M_PSEUDO_COLOR`](../../Reference/3dblob/M3dblobControlDraw.md). When drawing blobs from different segmentations into the same display, Aurora Imaging Library assigns identical color sequences to the different segmentations, which causes the graphics for different blobs to receive the same color. If you increment the value assigned to [`M_PSEUDO_COLOR_OFFSET`](../../Reference/3dblob/M3dblobControlDraw.md) by the maximum label value of the blobs in the previously drawn segmentation ([`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_MAX_LABEL_VALUE`](../../Reference/3dblob/M3dblobGetResult.md)), the colors assigned to the current segmentation will be unique.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the offset to apply to the label. |
