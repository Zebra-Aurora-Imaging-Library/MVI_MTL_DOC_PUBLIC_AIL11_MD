---
doctype: Reference
module: 3dblob
function: M3dblobInquireDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobInquireDraw"
---

# M3dblobInquireDraw

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

> Inquire about a setting of a draw 3D analysis context.

## Syntax

```c
AIL_INT64 M3dblobInquireDraw(
    AIL_ID    DrawContext3dblobId,  //in
    AIL_INT64 Operation,            //in
    AIL_INT64 InquireType,          //in
    void *    UserVarPtr            //out
)
```

## Description

This function inquires about a specified setting of a draw 3D blob analysis context. Draw blob settings establish which blob features to draw into the 3D graphics list when calling [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md) and how to draw them.

You can control the draw blob settings using [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md).

## Parameters

### `DrawContext3dblobId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D blob analysis context to inquire.

### `Operation` *(in, AIL_INT64)*

Specifies the draw operation about which to inquire.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dblobInquireDraw`](../../Reference/3dblob/M3dblobInquireDraw.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a

The following [`Operation`](../../Reference/3dblob/M3dblobInquireDraw.md) and [`InquireType`](../../Reference/3dblob/M3dblobInquireDraw.md) parameter settings are available to inquire about  settings.

---

### `M_DRAW_BLOBS`

Inquires whether and how to draw the points inside each specified blob.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_BOX`

Inquires whether and how to draw the bounding box of each specified blob. [`M_BOUNDING_BOX`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_EXCLUDED_POINTS`

Inquires whether and how to draw the points that are not part of each specified blob (or not part of any blob).

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_FERET_GENERAL`

Inquires whether and how to draw the custom Feret of each specified blob. [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobControl.md) and [`M_FERET_CONTACT_POINTS`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_FERET_MAX`

Inquires whether and how to draw the maximum diameter Feret of each specified blob. [`M_FERET_MAX_DIAMETER`](../../Reference/3dblob/M3dblobControl.md) and [`M_FERET_CONTACT_POINTS`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_FERET_MIN`

Inquires whether and how to draw the minimum diameter Feret of each specified blob. [`M_FERET_MIN_DIAMETER`](../../Reference/3dblob/M3dblobControl.md) and [`M_FERET_CONTACT_POINTS`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_INDEX`

Inquires whether and how to draw the index at the centroid of each specified blob.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_FONT_SIZE`

Inquires the size of the font with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_FONT_SIZE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_RENDER_LAYER`

Inquires the render layer on which to draw the graphic(s). The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_LABEL`

Inquires whether and how to draw the label(s) at the centroid of each specified blob.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_FONT_SIZE`

Inquires the size of the font with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_FONT_SIZE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_RENDER_LAYER`

Inquires the render layer on which to draw the graphic(s). The graphics on higher layers are always drawn completely in front of graphics on lower layers even if those on higher layers are farther away.

| Value | Description |
| --- | --- |
| *(see [`M_RENDER_LAYER`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_NEAREST_BLOB`

Inquires whether and how to draw the shortest line from each specified blob to its nearest blob. [`M_NEAREST_BLOB`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand. If [`M_NEAREST_BLOB`](../../Reference/3dblob/M3dblobControl.md) was calculated but there was no nearest blob, nothing is drawn.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_PCA`

Inquires whether and how to draw the principal component of each specified blob; this is drawn as perpendicular axes centered on each blob's centroid and oriented along the axis directions determined from a principal component analysis (PCA). [`M_PCA`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_PCA_BOX`

Inquires whether and how to draw the PCA-aligned bounding box of each specified blob. [`M_PCA_BOX`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_DRAW_SEMI_ORIENTED_BOX`

Specifies whether and how to draw the semi-oriented bounding box of each specified blob. [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobControl.md) must have been calculated beforehand.

#### `M_ACTIVE`

Inquires whether to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_ACTIVE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_APPEARANCE`

Inquires whether to draw the graphic(s) as a solid surface, wireframe, or points.

| Value | Description |
| --- | --- |
| *(see [`M_APPEARANCE`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_COLOR`

Inquires the color with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_COLOR`](Reference/3dblob/M3dblobControlDraw.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

#### `M_OPACITY`

Inquires the opacity with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_OPACITY`](Reference/3dblob/M3dblobControlDraw.md))* |  |

#### `M_THICKNESS`

Inquires the thickness with which to draw the graphic(s).

| Value | Description |
| --- | --- |
| *(see [`M_THICKNESS`](Reference/3dblob/M3dblobControlDraw.md))* |  |

---

### `M_GLOBAL_DRAW_SETTINGS`

Inquires a global draw 3D blob analysis context setting.

#### `M_PSEUDO_COLOR_OFFSET`

Inquires the offset to apply to the blob's label when drawing with [`M_PSEUDO_COLOR`](../../Reference/3dblob/M3dblobInquireDraw.md).

| Value | Description |
| --- | --- |
| *(see [`M_PSEUDO_COLOR_OFFSET`](Reference/3dblob/M3dblobControlDraw.md))* |  |

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
