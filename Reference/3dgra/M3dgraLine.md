---
doctype: Reference
module: 3dgra
function: M3dgraLine
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraLine"
---

# M3dgraLine

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

> Add a line graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraLine(
    AIL_ID     List3dgraId,     //out
    AIL_INT64  ParentLabel,     //in
    AIL_INT64  CreationMode,    //in
    AIL_INT64  Symbol,          //in
    AIL_DOUBLE PointX,          //in
    AIL_DOUBLE PointY,          //in
    AIL_DOUBLE PointZ,          //in
    AIL_DOUBLE PointOrVectorX,  //in
    AIL_DOUBLE PointOrVectorY,  //in
    AIL_DOUBLE PointOrVectorZ,  //in
    AIL_DOUBLE Length,          //in
    AIL_INT64  ControlFlag      //in
)
```

## Description

This function adds a line graphic to the specified 3D graphics list, allowing you to, for example, view the line graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the line graphic. When the line graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the line graphic's parent must be the root node. All coordinates are expressed in the coordinate system of the line graphic's parent.

The line graphic has its own coordinate system that represents the line graphic's position and orientation with respect to its parent's coordinate system. The origin of the line graphic's coordinate system is the line graphic's start point, and its Z-axis shares the same direction as the line graphic's direction. You can inquire the line graphic's start point using [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md) with [`M_START_POINT_...`](../../Reference/3dgra/M3dgraInquire.md). You can change the position and orientation of the line graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

To create one or more lines, use [`M3dgraLines`](../../Reference/3dgra/M3dgraLines.md).

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraLine.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the line graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the line graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraLine.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the line graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CreationMode` *(in, AIL_INT64)*

Specifies how the line graphic is defined.

### `Symbol` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `PointX` *(in, AIL_DOUBLE)*

Specifies the first parameter used to define the line graphic.

### `PointY` *(in, AIL_DOUBLE)*

Specifies the second parameter used to define the line graphic.

### `PointZ` *(in, AIL_DOUBLE)*

Specifies the third parameter used to define the line graphic.

### `PointOrVectorX` *(in, AIL_DOUBLE)*

Specifies the fourth parameter used to define the line graphic.

### `PointOrVectorY` *(in, AIL_DOUBLE)*

Specifies the fifth parameter used to define the line graphic.

### `PointOrVectorZ` *(in, AIL_DOUBLE)*

Specifies the sixth parameter used to define the line graphic.

### `Length` *(in, AIL_DOUBLE)*

Specifies to override the default line graphic's length.

*For specifying to override the default line graphic's length*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default length defined by the creation mode. |
| `M_INFINITE` | Specifies an infinite line graphic. The line graphic's size is determined upon creation, using its 3D graphics list's clipping box. |
| `Value > 0.0` | Specifies to override the line graphic's length with a specific value. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the line graphic

---

### `M_POINT_AND_VECTOR`

Defines the line graphic using a point on the line and a nonzero vector defining the line's direction.  By default, the length of the line graphic is finite and is set to the vector's magnitude. The point is at the start of the line graphic.

---

### `M_TWO_POINTS`

Defines the line graphic using any two non-identical points.  By default, the length of the line graphic is finite and is set to the distance between the two specified points. The line graphic starts at the first point, and ends at the second point.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the line graphic added to the 3D graphics list.
