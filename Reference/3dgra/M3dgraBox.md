---
doctype: Reference
module: 3dgra
function: M3dgraBox
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraBox"
---

# M3dgraBox

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

> Add a box graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraBox(
    AIL_ID     List3dgraId,    //out
    AIL_INT64  ParentLabel,    //in
    AIL_INT64  CreationMode,   //in
    AIL_DOUBLE XPos1,          //in
    AIL_DOUBLE YPos1,          //in
    AIL_DOUBLE ZPos1,          //in
    AIL_DOUBLE XPos2OrLength,  //in
    AIL_DOUBLE YPos2OrLength,  //in
    AIL_DOUBLE ZPos2OrLength,  //in
    AIL_ID     Matrix3dgeoId,  //in
    AIL_INT64  ControlFlag     //in
)
```

## Description

This function adds a box graphic to the specified 3D graphics list, allowing you to, for example, view the box graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the box graphic. When the box graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the box graphic's parent must be the root node. All coordinates are expressed in the coordinate system of the box graphic's parent.

The box graphic has its own coordinate system that represents the box graphic's position and orientation with respect to its parent's coordinate system. The origin of the box graphic's coordinate system is the box graphic's center, and the box graphic's orientation (and its coordinate system) is set relative to its parent using the [`Matrix3dgeoId`](../../Reference/3dgra/M3dgraBox.md) parameter. Note that you can change the box graphic's position and orientation using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraBox.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the box graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the box graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraBox.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the box graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CreationMode` *(in, AIL_INT64)*

Specifies how the box graphic is defined.

### `XPos1` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the first point used to define the box graphic.

### `YPos1` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the first point used to define the box graphic.

### `ZPos1` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the first point used to define the box graphic.

### `XPos2OrLength` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the second point used to define the box graphic, or the length of the box graphic along its coordinate system's X-axis.

### `YPos2OrLength` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the second point used to define the box graphic, or the length of the box graphic along its coordinate system's Y-axis.

### `ZPos2OrLength` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the second point used to define the box graphic, or the length of the box graphic along its coordinate system's Z-axis.

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the transformation matrix that defines the box graphic's orientation with respect to its parent's coordinate system.

*For specifying the transformation matrix object identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_IDENTITY_MATRIX`](../../Reference/3dgra/M3dgraBox.md). |
| `M_IDENTITY_MATRIX` | Specifies the identity matrix. This means that the box graphic's position and orientation is the same as the position and orientation of its parent's coordinate system. The resulting box graphic is axis-aligned with respect to its parent's coordinate system. |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix that defines the box graphic's orientation with respect to its parent's coordinate system. The transformation matrix must be of type [`M_ROTATION`](../../Reference/3dgeo/M3dgeoInquire.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the box graphic

Set unused parameters to [`M_DEFAULT`](../../Reference/3dgra/M3dgraBox.md).

---

### `M_BOTH_CORNERS`

Defines the box graphic using any two opposite corners.

---

### `M_CENTER_AND_DIMENSION`

Defines the box graphic by its center point and its length along each of its coordinate system's axes.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the box graphic along its coordinate system's X-axis, in world units. |
| `Value > 0.0` | Specifies the length of the box graphic along its coordinate system's Y-axis, in world units. |
| `Value > 0.0` | Specifies the length of the box graphic along its coordinate system's Z-axis, in world units. |

---

### `M_CORNER_AND_DIMENSION`

Defines the box graphic by one of its corners and its length along each of its coordinate system's axes.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the box graphic along its coordinate system's X-axis, in world units. |
| `Value > 0.0` | Specifies the length of the box graphic along its coordinate system's Y-axis, in world units. |
| `Value > 0.0` | Specifies the length of the box graphic along its coordinate system's Z-axis, in world units. |

## Return Value

**Type:** `AIL_INT64`

Returns the label of the box graphic added to the 3D graphics list.
