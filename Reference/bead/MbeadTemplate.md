---
doctype: Reference
module: bead
function: MbeadTemplate
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadTemplate"
---

# MbeadTemplate

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

> Add, delete, or modify a bead template.

## Syntax

```c
void MbeadTemplate(
    AIL_ID             ContextBeadId,           //out
    AIL_INT64          Operation,               //in
    AIL_INT64          BeadType,                //in
    AIL_INT            LabelOrIndex,            //in
    AIL_INT            GraListIdOrSizeOfArray,  //in
    const AIL_DOUBLE * FirstArrayPtr,           //in
    const AIL_DOUBLE * SecondArrayPtr,          //in
    const AIL_INT *    ThirdArrayPtr,           //in
    AIL_INT64          ControlFlag              //in
)
```

## Description

This function adds a new bead template to a bead context. This function also performs operations on templates already in a context, such as deleting them, modifying them, or adding, deleting, or modifying their vertices.

By default, Aurora Imaging Library interprets certain input settings for a template in pixel units (such as position and dimension). To specify that Aurora Imaging Library should interpret these settings in world units, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md) set to [`M_WORLD`](../../Reference/bead/MbeadControl.md).

Whenever you add, delete, or modify a bead template, you must train it using [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). Once you train the template, you can use it to verify beads with [`MbeadVerify`](../../Reference/bead/MbeadVerify.md).

## Parameters

### `ContextBeadId` *(out, AIL_ID)*

Specifies the identifier of a bead context. The bead context must have been previously allocated on the required system using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies to add a new bead template to a bead context, or specifies another operation to perform with a template that is already in a context.

### `BeadType` *(in, AIL_INT64)*

Specifies the type of the new bead template to add to a bead context, or specifies that the type information is not required.

*For specifying the bead type*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default type. When adding a new bead template to a bead context, [`M_DEFAULT`](../../Reference/bead/MbeadTemplate.md) is the same as [`M_BEAD_STRIPE`](../../Reference/bead/MbeadTemplate.md).

When performing an operation with a template that is already in a context, [`M_DEFAULT`](../../Reference/bead/MbeadTemplate.md) indicates that the type information is not required. This is the only possible setting for these operations. |
| `M_BEAD_EDGE` | Specifies to add a new bead template consisting of an edge. In this case, the template represents an outline of a bead delineated by a boundary established from intensity transitions in an image, such as the skeletal border of an object.

*[Image: BeadEdgeType.png]*

By default, Aurora Imaging Library assumes the foreground of the edge-bead is white and is on the counter-clockwise side of the path direction, while the background is assumed to be black and on the clockwise side of the edge-bead. To modify how Aurora Imaging Library establishes the foreground, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_FOREGROUND_VALUE`](../../Reference/bead/MbeadControl.md).

You can only specify [`M_BEAD_EDGE`](../../Reference/bead/MbeadTemplate.md) when adding a new bead template to a bead context. |
| `M_BEAD_STRIPE` | Specifies to add a new bead template consisting of a stripe. In this case, the template represents a bead that has two edges and a width, such as a strip of some malleable material (for example, glue or lead).

*[Image: BeadStripeType.png]*

By default, Aurora Imaging Library assumes the stripe-bead is white and is on a black background. By default, Aurora Imaging Library also assumes that the bead's nominal width is consistent and is automatically learned from the training phase. To modify how Aurora Imaging Library establishes the foreground and width, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_FOREGROUND_VALUE`](../../Reference/bead/MbeadControl.md) and [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md).

You can only specify [`M_BEAD_STRIPE`](../../Reference/bead/MbeadTemplate.md) when adding a new bead template to a bead context. |

### `LabelOrIndex` *(in, AIL_INT)*

Specifies a unique label for a new bead template to add to a bead context, or specifies the index or label of a template in a context (or all templates in a context) with which to perform the operation. This parameter should be set to one of the following values:

*For specifying a bead template*

| Value | Description |
| --- | --- |
| `M_TEMPLATE_INDEX` | Specifies the index of the bead template. |
| `M_TEMPLATE_LABEL` | Specifies the label of the bead template. |
| `M_ALL` | Specifies all bead templates. |

### `GraListIdOrSizeOfArray` *(in, AIL_INT)*

Specifies information required for the operation. If this information is not needed, set this parameter to `M_NULL`.

### `FirstArrayPtr` *(in, *AIL_DOUBLE)*

Specifies an attribute of the operation. Its definition is dependent on the [`Operation`](../../Reference/bead/MbeadTemplate.md) parameter setting. If the operation does not require this information, set this parameter to `M_NULL`.

### `SecondArrayPtr` *(in, *AIL_DOUBLE)*

Specifies an attribute of the operation. Its definition is dependent on the [`Operation`](../../Reference/bead/MbeadTemplate.md) parameter setting. If the operation does not require this information, set this parameter to `M_NULL`.

### `ThirdArrayPtr` *(in, *AIL_INT)*

Specifies an attribute of the operation. Its definition is dependent on the [`Operation`](../../Reference/bead/MbeadTemplate.md) parameter setting. If the operation does not require this information, set this parameter to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For adding a new bead template to a bead context

The value in the table below is for adding a new bead template to a bead context. In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadTemplate.md) parameter to **M_TEMPLATE_LABEL()** and specify a unique label value for the new template.

---

### `M_ADD`

Adds a new bead template, based on the specified vertices or a fixed circle or segment, to a bead context.  A template must have a path and a width. To establish the path of the template, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md). The template's default path ([`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md)) follows a polyline that Aurora Imaging Library establishes using the specified vertices ([`FirstArrayPtr`](../../Reference/bead/MbeadTemplate.md) and [`SecondArrayPtr`](../../Reference/bead/MbeadTemplate.md)) and then refining them according to a training image ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)). To specify a polyline path with a trained position that exactly follows the specified vertices, use [`M_POLYLINE`](../../Reference/bead/MbeadControl.md). In either case, the order in which you specify the vertices establishes the path's shape and direction.  You can also specify that the template's path follows a fixed segment or circle, which you must define using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TEMPLATE_CIRCLE_...`](../../Reference/bead/MbeadControl.md) or [`M_TEMPLATE_SEGMENT_...`](../../Reference/bead/MbeadControl.md). In this case you should not set any vertices.  To establish the width of the template, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md). By default ([`M_AUTO_UNIFORM`](../../Reference/bead/MbeadControl.md)), Aurora Imaging Library uses a training image to establish an average bead width. To use a training image to establish a unique width at the position of each trained point, use [`M_AUTO_CONTINUOUS`](../../Reference/bead/MbeadControl.md). You can also explicitly specify the template's width using [`M_USER_DEFINED`](../../Reference/bead/MbeadControl.md).  You can modify a template's vertices or width after you have set them (for example, using [`M_INSERT`](../../Reference/bead/MbeadTemplate.md) or [`M_SET_WIDTH_NOMINAL`](../../Reference/bead/MbeadTemplate.md)); keep this in mind since Aurora Imaging Library bases the template's path and width using the most current settings when you call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md).

---

### `M_ADD_FROM_GRAPHIC_LIST`

Adds a new bead template, based on the vertices of a polyline (or polygon) from a 2D graphics list, to a bead context. The 2D graphics list must contain one polyline (or polygon) graphic, added with [`MgraLines`](../../Reference/gra/MgraLines.md) or [`MgraInteractive`](../../Reference/gra/MgraInteractive.md). Aurora Imaging Library considers the specified vertices ([`MgraLines`](../../Reference/gra/MgraLines.md) or [`MgraInteractive`](../../Reference/gra/MgraInteractive.md)) as the template's vertices. Note that a polygon is simply a polyline that Aurora Imaging Library automatically closes by connecting the final vertex to the first with a straight line. Any operation or setting that applies to polylines also applies to polygons.  A template must have a path and a width. To establish the path of the template, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md). The template's default path ([`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md)) follows a polyline that Aurora Imaging Library establishes using the specified vertices ([`MgraLines`](../../Reference/gra/MgraLines.md) or [`MgraInteractive`](../../Reference/gra/MgraInteractive.md)) and then refining them according to a training image ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)). To specify a polyline path with a trained position that exactly follows the specified vertices, use [`M_POLYLINE`](../../Reference/bead/MbeadControl.md). In either case, the order in which you specify the vertices ([`MgraLines`](../../Reference/gra/MgraLines.md)) establishes the path's shape and direction.  To establish the width of the template, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md). By default ([`M_AUTO_UNIFORM`](../../Reference/bead/MbeadControl.md)), Aurora Imaging Library uses a training image to establish an average bead width. To use a training image to establish a unique width at the position of each trained point, use [`M_AUTO_CONTINUOUS`](../../Reference/bead/MbeadControl.md). You can also explicitly specify the template's width using [`M_USER_DEFINED`](../../Reference/bead/MbeadControl.md).  You can modify a template's vertices or width after you have set them (for example, using [`M_INSERT`](../../Reference/bead/MbeadTemplate.md) or [`M_SET_WIDTH_NOMINAL`](../../Reference/bead/MbeadTemplate.md)); keep this in mind since Aurora Imaging Library bases the template's path and width using the most current settings when you call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md).

### For performing an operation with a bead template in a bead context

The values in the table below are for performing an operation with a bead template in a bead context. Unless otherwise specified, you can only perform these operations if the template's path follows a polyline, and you must set the [`LabelOrIndex`](../../Reference/bead/MbeadTemplate.md) parameter to a specific template, using either **M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**.

---

### `M_DELETE`

Deletes the specified vertices from the specified template, or deletes the specified template.  You can delete templates whose path follows a polyline, circle, or segment; however you can only delete vertices from a template if its path follows a polyline.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to delete the templates indicated by the [`LabelOrIndex`](../../Reference/bead/MbeadTemplate.md) parameter. To delete all templates, set the [`LabelOrIndex`](../../Reference/bead/MbeadTemplate.md) parameter to [`M_ALL`](../../Reference/bead/MbeadTemplate.md). |
| `M_ALL` | Specifies to delete all vertices from the specified template. In this case, you cannot set the [`LabelOrIndex`](../../Reference/bead/MbeadTemplate.md) parameter to [`M_ALL`](../../Reference/bead/MbeadTemplate.md). |
| `Value` | Specifies the size of the array ([`ThirdArrayPtr`](../../Reference/bead/MbeadTemplate.md)) containing the vertices to delete from the specified bead template. In this case, you cannot set the [`LabelOrIndex`](../../Reference/bead/MbeadTemplate.md) parameter to [`M_ALL`](../../Reference/bead/MbeadTemplate.md). |
| `M_NULL` | Specifies to ignore this parameter. You must set this parameter to [`M_NULL`](../../Reference/bead/MbeadTemplate.md) when you are deleting templates ([`GraListIdOrSizeOfArray`](../../Reference/bead/MbeadTemplate.md) is set to [`M_NULL`](../../Reference/bead/MbeadTemplate.md)) or when deleting all vertices from a template ([`GraListIdOrSizeOfArray`](../../Reference/bead/MbeadTemplate.md) is set to [`M_ALL`](../../Reference/bead/MbeadTemplate.md)). |
| `ArrayAddress` | Specifies the address of the array that indicates the vertices to delete. The array must have the same size as specified using [`GraListIdOrSizeOfArray`](../../Reference/bead/MbeadTemplate.md). |

---

### `M_INSERT`

Inserts the specified vertices into the specified bead template.

| Value | Description |
| --- | --- |
| `M_BEGIN` | Specifies that the vertices will be inserted before the bead template's first vertex. |
| `M_END` | Specifies that the vertices will be inserted after the bead template's last vertex. |
| `-1 <= Value < TotalNumberOfVertices` | Specifies an index value representing the position in the template after which to insert the specified vertices. Since the index of the template's first vertex is 0, you must set a value between negative one (inserts vertices before the bead template's first vertex) and the total number of vertices in the template minus one (inserts vertices after the bead template's last vertex). |

---

### `M_REPLACE`

Replaces the specified vertices of the specified bead template.

---

### `M_ROTATE_POINTS`

Rotates the specified bead template around its origin (0,0).  This operation supports templates with any type of path.

---

### `M_SCALE_POINTS`

Scales the specified bead template.  This operation supports templates with any type of path.

---

### `M_SET_WIDTH_NOMINAL`

Specifies the expected width of the specified bead template. When using [`M_SET_WIDTH_NOMINAL`](../../Reference/bead/MbeadTemplate.md), Aurora Imaging Library adjusts the template's nominal width ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md)) according to the specified width value ([`FirstArrayPtr`](../../Reference/bead/MbeadTemplate.md)) at each specified vertex of the template ([`ThirdArrayPtr`](../../Reference/bead/MbeadTemplate.md)). You should only set the width when it differs from the nominal width of the bead. Aurora Imaging Library linearly interpolates the width between consecutive vertices.  *[Image: BeadWidthNominal.png]*  For [`M_SET_WIDTH_NOMINAL`](../../Reference/bead/MbeadTemplate.md) to have an effect, you must specify that the bead's nominal width is user-defined, using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) set to [`M_USER_DEFINED`](../../Reference/bead/MbeadControl.md).

---

### `M_TRANSLATE_POINTS`

Moves the specified bead template.  This operation supports templates with any type of path.
