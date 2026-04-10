---
doctype: Reference
module: 3dmet
function: M3dmetInquireDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetInquireDraw"
---

# M3dmetInquireDraw

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

> Inquire about a setting of a draw 3D metrology context.

## Syntax

```c
AIL_INT64 M3dmetInquireDraw(
    AIL_ID    DrawContext3dmetId,  //in
    AIL_INT64 Operation,           //in
    AIL_INT64 InquireType,         //in
    void *    UserVarPtr           //out
)
```

## Description

This function inquires about a specified setting of a draw 3D metrology context. These settings establish which metrology results to draw into the 3D graphics list when calling [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md) and how to draw them.

You can control the draw 3D metrology context settings using [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md).

## Parameters

### `DrawContext3dmetId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D metrology context to inquire. The draw 3D metrology context must have been previously allocated on the required system using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation about which to inquire.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dmetInquireDraw`](../../Reference/3dmet/M3dmetInquireDraw.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring a draw 3D metrology context

The following [`Operation`](../../Reference/3dmet/M3dmetInquireDraw.md) and [`InquireType`](../../Reference/3dmet/M3dmetInquireDraw.md) parameter settings are available to inquire about draw 3D metrology contexts settings.

---

### `M_DRAW_VOLUME_ELEMENTS`

Inquires whether and how to draw the volume elements used to compute the volume, independent of whether the elements contributed positively or negatively to the volume.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmet/M3dmetControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_VOLUME_ELEMENT_APPEARANCE`

Inquires whether to draw the volume element(s) as a surface or volume.

| Value | Description |
| --- | --- |
| *(see [`M_VOLUME_ELEMENT_APPEARANCE`](Reference/3dmet/M3dmetControlDraw.md))* |  |

---

### `M_DRAW_VOLUME_NEGATIVE_ELEMENTS`

Inquires whether and how to draw the volume elements that contributed negatively to the volume computation; these elements decreased the volume.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmet/M3dmetControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_VOLUME_ELEMENT_APPEARANCE`

Inquires whether to draw the volume element(s) as a surface or volume.

| Value | Description |
| --- | --- |
| *(see [`M_VOLUME_ELEMENT_APPEARANCE`](Reference/3dmet/M3dmetControlDraw.md))* |  |

---

### `M_DRAW_VOLUME_POSITIVE_ELEMENTS`

Inquires wwhether and how to draw the volume elements that contributed positively to the volume computation; these elements increased the volume.

#### `M_ACTIVE`

inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dmet/M3dmetControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_THICKNESS`

inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dmet/M3dmetControlDraw.md))* |  |

#### `M_VOLUME_ELEMENT_APPEARANCE`

Inquires whether to draw the volume element(s) as a surface or volume.

| Value | Description |
| --- | --- |
| *(see [`M_VOLUME_ELEMENT_APPEARANCE`](Reference/3dmet/M3dmetControlDraw.md))* |  |

---

### `M_GLOBAL_DRAW_SETTINGS`

Inquires a global draw 3D metrology context setting.

#### `M_VOLUME_ELEMENT_INDEX`

Inquires the index of the volume element to draw.

| Value | Description |
| --- | --- |
| *(see [`M_VOLUME_ELEMENT_INDEX`](Reference/3dmet/M3dmetControlDraw.md))* |  |

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
