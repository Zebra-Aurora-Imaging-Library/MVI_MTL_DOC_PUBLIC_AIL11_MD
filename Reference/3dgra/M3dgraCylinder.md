---
doctype: Reference
module: 3dgra
function: M3dgraCylinder
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraCylinder"
---

# M3dgraCylinder

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

> Add a cylinder graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraCylinder(
    AIL_ID     List3dgraId,    //out
    AIL_INT64  ParentLabel,    //in
    AIL_INT64  CreationMode,   //in
    AIL_DOUBLE XPos1,          //in
    AIL_DOUBLE YPos1,          //in
    AIL_DOUBLE ZPos1,          //in
    AIL_DOUBLE XPos2OrVector,  //in
    AIL_DOUBLE YPos2OrVector,  //in
    AIL_DOUBLE ZPos2OrVector,  //in
    AIL_DOUBLE Radius,         //in
    AIL_DOUBLE Length,         //in
    AIL_INT64  ControlFlag     //in
)
```

## Description

This function adds a cylinder graphic to the specified 3D graphics list, allowing you to, for example, view the cylinder graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the cylinder graphic. When the cylinder graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the cylinder graphic's parent must be the root node. All coordinates are expressed in the coordinate system of the cylinder graphic's parent.

The cylinder graphic has its own coordinate system that represents the cylinder graphic's position and orientation with respect to its parent's coordinate system. The origin of the cylinder graphic's coordinate system is the cylinder graphic's start point, and its Z-axis is the cylinder graphic's central axis. You can inquire the cylinder graphic's start point using [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md) with [`M_START_POINT_...`](../../Reference/3dgra/M3dgraInquire.md). You can change the position and orientation of the cylinder graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraCylinder.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the cylinder graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the cylinder graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraCylinder.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the cylinder graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CreationMode` *(in, AIL_INT64)*

Specifies how the cylinder graphic is defined.

### `XPos1` *(in, AIL_DOUBLE)*

Specifies the first parameter used to define the cylinder graphic.

### `YPos1` *(in, AIL_DOUBLE)*

Specifies the second parameter used to define the cylinder graphic.

### `ZPos1` *(in, AIL_DOUBLE)*

Specifies the third parameter used to define the cylinder graphic.

### `XPos2OrVector` *(in, AIL_DOUBLE)*

Specifies the fourth parameter used to define the cylinder graphic.

### `YPos2OrVector` *(in, AIL_DOUBLE)*

Specifies the fifth parameter used to define the cylinder graphic.

### `ZPos2OrVector` *(in, AIL_DOUBLE)*

Specifies the sixth parameter used to define the cylinder graphic.

### `Radius` *(in, AIL_DOUBLE)*

Specifies the cylinder graphic's radius.

*For specifying the cylinder graphic's radius*

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the cylinder graphic's radius. |

### `Length` *(in, AIL_DOUBLE)*

Specifies to override the default cylinder graphic's length.

*For specifying to override the default cylinder graphic's length*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default length defined by the creation mode. |
| `M_INFINITE` | Specifies an infinite cylinder graphic. The cylinder graphic's size is determined upon creation, using its 3D graphics list's clipping box. |
| `Value > 0.0` | Specifies to override the cylinder graphic's length with a specific value. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the cylinder graphic

---

### `M_POINT_AND_VECTOR`

Defines the cylinder graphic using a point on the cylinder graphic's central axis and a nonzero vector defining the central axis direction.  By default, the length of the cylinder graphic is finite and is set to the vector's magnitude. The point is at the center of the cylinder graphic's first circular base.

---

### `M_TWO_POINTS`

Defines the cylinder graphic using any two non-identical points on the cylinder graphic's central axis.  By default, the length of the cylinder graphic is finite and is set to the distance between the two specified points. The first specified point is at the center of the cylinder graphic's first circular base and the second specified point is at the center of the cylinder graphic's second circular base.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the cylinder graphic added to the 3D graphics list.
