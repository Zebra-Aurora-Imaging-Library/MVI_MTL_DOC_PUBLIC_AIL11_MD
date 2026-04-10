---
doctype: Reference
module: 3dmod
function: M3dmodInquireDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodInquireDraw"
---

# M3dmodInquireDraw

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

> Inquire about a setting of a draw 3D model finder context.

## Syntax

```c
AIL_INT64 M3dmodInquireDraw(
    AIL_ID    DrawContext3dmodId,  //in
    AIL_INT64 Operation,           //in
    AIL_INT64 InquireType,         //in
    void *    UserVarPtr           //out
)
```

## Description

This function inquires about a specified setting of a draw 3D model finder context. These settings establish which features of a model or which results of found model occurrences to draw into the 3D graphics list when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md) and how to draw them.

You can control the draw 3D model finder context settings using [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md).

## Parameters

### `DrawContext3dmodId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D model finder context to inquire. The draw 3D model finder context must have been previously allocated on the required system using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with either [`M_DRAW_3D_GEOMETRIC_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) (to draw occurrences of geometric models) or [`M_DRAW_3D_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) (to draw surface models or occurrences of surface models).

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation about which to inquire.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dmodInquireDraw`](../../Reference/3dmod/M3dmodInquireDraw.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring a draw geometric or surface 3D model finder context

The following [`Operation`](../../Reference/3dmod/M3dmodInquireDraw.md) and [`InquireType`](../../Reference/3dmod/M3dmodInquireDraw.md) parameter settings can be specified for a draw 3D model finder context.

---

### `M_DRAW_AXES`

Inquires whether and how to draw the axes of either the model or each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md), depending on whether a find 3D model finder context or result buffer is specified.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_LENGTH`

Inquires the length of the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_LENGTH`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_REFERENCE_X`

Inquires the X-position of the graphic(s) origin, relative to the model or occurrence.  Note that [`M_REFERENCE_X`](../../Reference/3dmod/M3dmodInquireDraw.md) is only available for a draw surface 3D model finder context.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_X`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_REFERENCE_Y`

Inquires the Y-position of the graphic(s) origin, relative to the model or occurrence.  Note that [`M_REFERENCE_Y`](../../Reference/3dmod/M3dmodInquireDraw.md) is only available for a draw surface 3D model finder context.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_Y`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_REFERENCE_Z`

Inquires the Z-position of the graphic(s) origin, relative to the model or occurrence.  Note that [`M_REFERENCE_Z`](../../Reference/3dmod/M3dmodInquireDraw.md) is only available for a draw surface 3D model finder context.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_Z`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmod/M3dmodControlDraw.md))* |  |

---

### `M_DRAW_BOX`

Inquires whether and how to draw the bounding box of each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md). The bounding box is the smallest-axis aligned box that contains the ideal geometric shape or surface of the model at the location of the occurrence, depending on the model type. If you enable [`M_DRAW_MODEL`](../../Reference/3dmod/M3dmodInquireDraw.md), the bounding box is the smallest axis-aligned box that contains that graphic. Note that it does not necessarily contain all inlier points.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmod/M3dmodControlDraw.md))* |  |

---

### `M_DRAW_INDEX`

Inquires whether and how to draw the index of each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_FONT_SIZE`

Inquires the size of the font with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_FONT_SIZE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_REFERENCE_X`

Inquires the X-position of the graphic(s) origin, relative to the occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_X`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_REFERENCE_Y`

Inquires the Y-position of the graphic(s) origin, relative to the occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_Y`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_REFERENCE_Z`

Inquires the Z-position of the graphic(s) origin, relative to the occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_Z`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_RENDER_LAYER`

Inquires the layer on which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_TEXT_DIRECTION`

Inquires the direction to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_TEXT_DIRECTION`](Reference/3dmod/M3dmodControlDraw.md))* |  |

---

### `M_DRAW_INLIER_POINTS`

Inquires whether and how to draw the inlier points, based on [`M_FIT_DISTANCE`](../../Reference/3dmod/M3dmodControl.md), of each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md).  Note that for a surface model, [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) must be enabled.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmod/M3dmodControlDraw.md))* |  |

---

### `M_DRAW_MODEL`

Inquires whether and how to draw either the defined model or each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md), depending on whether a find 3D model finder context or result buffer is specified.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmod/M3dmodControlDraw.md))* |  |

---

### `M_DRAW_RESERVED_POINTS`

Inquires whether and how to draw the reserved points of each specified occurrence when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmod/M3dmodControlDraw.md))* |  |

---

### `M_GLOBAL_DRAW_SETTINGS`

Inquires a global draw 3D model finder context setting.

#### `M_PSEUDO_COLOR_OFFSET`

Inquires the offset to apply to either the model's or the occurrence's index when drawing with [`M_PSEUDO_COLOR`](../../Reference/3dmod/M3dmodInquireDraw.md).

| Value | Description |
| --- | --- |
| *(see [`M_PSEUDO_COLOR_OFFSET`](Reference/3dmod/M3dmodControlDraw.md))* |  |

### For inquiring a draw surface 3D model finder context

The following [`Operation`](../../Reference/3dmod/M3dmodInquireDraw.md) and [`InquireType`](../../Reference/3dmod/M3dmodInquireDraw.md) parameter settings can be specified for a draw surface 3D model finder context.

---

### `M_DRAW_AXES_POSITION`

Inquires whether and how to draw the axes at either the model's or each specified occurrence's position when calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md), depending on whether a find 3D model finder context or result buffer is specified. The axes are drawn at the model's reference axis origin/the model's reference axis origin transformed at each occurrence.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_LENGTH`

Inquires the length of the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_LENGTH`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmod/M3dmodControlDraw.md))* |  |

---

### `M_DRAW_BACKGROUND_POINTS`

Inquires whether and how to draw the points that were considered part of the background during the search. Background points include those of objects that are too big or too small to be part of an occurrence of the model. The corresponding drawing operation is only supported if [`M_REMOVE_BACKGROUND`](../../Reference/3dmod/M3dmodControl.md) was enabled.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmod/M3dmodControlDraw.md))* |  |

---

### `M_DRAW_FLOOR_POINTS`

Inquires whether and how to draw the points that were considered part of the floor during the search. Floor points are determined using the floor plane, defined using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_FLOOR`](../../Reference/3dmod/M3dmodCopy.md), and the floor removal direction and offset ([`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REMOVE_FLOOR_DIRECTION`](../../Reference/3dmod/M3dmodControl.md) and [`M_REMOVE_FLOOR_OFFSET`](../../Reference/3dmod/M3dmodControl.md)). The corresponding drawing operation is only supported if [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md) was enabled.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmod/M3dmodControlDraw.md))* |  |

---

### `M_DRAW_MODEL_PREPROCESSED`

Inquires whether and how to draw the preprocessed model that is in the specified find 3D model finder context.  [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md) must be called before calling [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmod/M3dmodControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmod/M3dmodControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmod/M3dmodControlDraw.md))* |  |

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
