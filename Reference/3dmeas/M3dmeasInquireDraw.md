---
doctype: Reference
module: 3dmeas
function: M3dmeasInquireDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasInquireDraw"
---

# M3dmeasInquireDraw

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

> Inquire about a setting of a draw 3D measurement context.

## Syntax

```c
AIL_INT64 M3dmeasInquireDraw(
    AIL_ID    DrawContext3dmeasId,  //in
    AIL_INT64 Operation,            //in
    AIL_INT64 InquireType,          //in
    void *    UserVarPtr            //out
)
```

## Description

This function inquires about a specified setting of a draw 3D measurement context. These settings establish which 3D measurement results to draw into the 3D graphics list when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md) and how to draw them.

You can control the draw 3D measurement context settings using [`M3dmeasControlDraw`](../../Reference/3dmeas/M3dmeasControlDraw.md).

## Parameters

### `DrawContext3dmeasId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D measurement context to inquire. The draw 3D measurement context must have been previously allocated on the required system using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_DRAW_3D_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), [`M_DRAW_3D_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), or [`M_DRAW_3D_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation about which to inquire.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dmeasInquireDraw`](../../Reference/3dmeas/M3dmeasInquireDraw.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring a draw profile, path, or template 3D measurement context

The following [`Operation`](../../Reference/3dmeas/M3dmeasInquireDraw.md) and [`InquireType`](../../Reference/3dmeas/M3dmeasInquireDraw.md) parameter settings are available to inquire about draw profile, path, or template 3D measurement context settings.

---

### `M_DRAW_MARKERS`

Inquires whether and how to draw the cross-like symbol at each specified marker's position, and for pair markers, a rectangle connecting the two transitions with a cross-like symbol at each transition position, when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_MARKERS_POSITION`

Inquires whether and how to draw the cross-like symbol at each specified marker's position when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_MARKERS_WIDTH`

Inquires whether and how to draw the width of each specified pair marker when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md). The width is drawn as a rectangle connecting the two transitions with a cross-like symbol at each transition position.  This operation is only supported for pair markers.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_PROFILE_EXTRACTED`

Inquires whether and how to draw the extracted points that form each specified profile when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_PROFILE_REGION`

Inquires whether and how to draw the region where each specified profile was extracted when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_PROFILE_RESPONSE`

Inquires whether and how to draw the response of each specified profile (after applying the filter) when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_TRANSITIONS`

Inquires whether and how to draw the step-like symbol at each specified marker's transition position when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_GLOBAL_DRAW_SETTINGS`

Inquires a global draw 3D measurement context setting.

#### `M_PSEUDO_COLOR_OFFSET`

Inquires the index of the volume element to draw.

| Value | Description |
| --- | --- |
| *(see [`M_PSEUDO_COLOR_OFFSET`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

### For inquiring a draw path 3D measurement context

The following [`Operation`](../../Reference/3dmeas/M3dmeasInquireDraw.md) and [`InquireType`](../../Reference/3dmeas/M3dmeasInquireDraw.md) parameter settings are available to inquire about draw path 3D measurement context settings.

---

### `M_DRAW_PATH`

Inquires whether and how to draw the path defined in the path 3D measurement context when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_PATH_PROJECTED`

Inquires whether and how to draw the projection of the path defined in the path 3D measurement context when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

### For inquiring a draw template 3D measurement context

The following [`Operation`](../../Reference/3dmeas/M3dmeasInquireDraw.md) and [`InquireType`](../../Reference/3dmeas/M3dmeasInquireDraw.md) parameter settings are available to inquire about draw template 3D measurement context settings.

---

### `M_DRAW_FIND_GEOMETRY`

Inquires whether and how to draw the line segments connecting the extracted markers when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_FIND_GEOMETRY_PROJECTED`

Inquires whether and how to draw the planes connecting the projections of the extracted markers when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_FIT_GEOMETRY`

Inquires whether and how to draw the fitted 3D geometry when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_FIT_GEOMETRY_PROJECTED`

Inquires whether and how to draw the projection of the fitted 3D geometry when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_FIT_MARKERS`

Inquires whether and how to draw the cross-like symbol at the position of each marker that was used for the fit operation when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_FIT_MARKERS_OUTLIERS`

Inquires whether and how to draw the cross-like symbol at the position of each marker that was considered an outlier for the fit operation when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_TEMPLATE`

Inquires whether and how to draw the template defined in the template 3D measurement context when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

---

### `M_DRAW_TEMPLATE_PROJECTED`

Inquires whether and how to draw the projection of the template defined in the template 3D measurement context when calling [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmeas/M3dmeasControlDraw.md))* |  |

### Combination Constants — For inquiring about the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get information about the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

#### `M_IS_SET_TO_DEFAULT`

Inquires whether the specified inquire type is set to its default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not set to its default value. |
| `M_TRUE` | Specifies that the inquire type is set to its default value. |

### Combination Constants — For inquiring whether an inquire type has a default value or whether it is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type has a default value or whether it is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
