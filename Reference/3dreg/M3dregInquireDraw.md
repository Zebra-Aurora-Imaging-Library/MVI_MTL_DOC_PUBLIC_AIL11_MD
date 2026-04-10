---
doctype: Reference
module: 3dreg
function: M3dregInquireDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregInquireDraw"
---

# M3dregInquireDraw

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

> Inquire about a setting of a draw 3D registration context.

## Syntax

```c
AIL_INT64 M3dregInquireDraw(
    AIL_ID    DrawContext3dregId,  //in
    AIL_INT64 Operation,           //in
    AIL_INT64 InquireType,         //in
    void *    UserVarPtr           //out
)
```

## Description

This function inquires about a specified setting of a draw 3D registration context. These settings establish which pairwise 3D registration results to draw into the 3D graphics list (and how to draw them), when calling [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md).

You can control the draw 3D registration context settings using [`M3dregControlDraw`](../../Reference/3dreg/M3dregControlDraw.md).

## Parameters

### `DrawContext3dregId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D registration context to inquire. The draw 3D registration context must have been previously allocated on the required system using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dreg/M3dregAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation about which to inquire.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dregInquireDraw`](../../Reference/3dreg/M3dregInquireDraw.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring a draw 3D registration context

The following [`Operation`](../../Reference/3dreg/M3dregInquireDraw.md) and [`InquireType`](../../Reference/3dreg/M3dregInquireDraw.md) parameter settings are available to inquire about draw 3D registration contexts settings.

---

### `M_DRAW_EXCLUDED_POINTS`

Inquires whether and how to draw the points that are not paired to any other points. The resulting graphic will be of type [`M_GRAPHIC_TYPE_POINT_CLOUD`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dreg/M3dregControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dreg/M3dregControlDraw.md))* |  |

---

### `M_DRAW_OVERLAP_POINTS`

Inquires whether and how to draw overlapping (paired) points. Overlapping points are points in the reference point cloud that have been paired with at least one target point. The resulting graphic will be of type [`M_GRAPHIC_TYPE_POINT_CLOUD`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dreg/M3dregControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_COLOR_COMPONENT`

Inquires the component used to color the point cloud(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_COMPONENT`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_COLOR_LIMITS`

Inquires the limits of the color component. Values beyond min/max are saturated. When using a LUT, the min/max get mapped to the LUT's extreme values.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_LIMITS`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_COLOR_USE_LUT`

Inquires whether to color the point cloud(s) added to the 3D graphics list by mapping each value of the component specified by [`M_COLOR_COMPONENT`](../../Reference/3dreg/M3dregInquireDraw.md) to a color in a LUT, and then using that color to display the corresponding point.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_USE_LUT`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dreg/M3dregControlDraw.md))* |  |

---

### `M_DRAW_PAIRS`

Inquires whether and how to draw lines between paired points, when calling [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md). The resulting graphic(s) will be of type [`M_GRAPHIC_TYPE_LINE`](../../Reference/3dgra/M3dgraInquire.md) or [`M_GRAPHIC_TYPE_LINES`](../../Reference/3dgra/M3dgraInquire.md).

#### `M_ACTIVE`

Sets whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dreg/M3dregControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dreg/M3dregControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dreg/M3dregControlDraw.md))* |  |

---

### `M_GLOBAL_DRAW_SETTINGS`

Inquires a global draw 3D registration context setting.

#### `M_PSEUDO_COLOR_OFFSET`

Inquires the offset to apply to the registration element result's index when drawing with [`M_PSEUDO_COLOR`](../../Reference/3dreg/M3dregInquireDraw.md).

| Value | Description |
| --- | --- |
| *(see [`M_PSEUDO_COLOR_OFFSET`](Reference/3dreg/M3dregControlDraw.md))* |  |

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
