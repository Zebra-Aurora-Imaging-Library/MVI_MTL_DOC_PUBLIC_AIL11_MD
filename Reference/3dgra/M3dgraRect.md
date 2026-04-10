---
doctype: Reference
module: 3dgra
function: M3dgraRect
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraRect"
---

# M3dgraRect

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

> Add a rectangle graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraRect(
    AIL_ID     List3dgraId,        //out
    AIL_INT64  ParentLabel,        //in
    AIL_INT64  CreationMode,       //in
    AIL_DOUBLE X1OrMatrix3dGeoId,  //in
    AIL_DOUBLE Y1,                 //in
    AIL_DOUBLE Z1,                 //in
    AIL_DOUBLE X2,                 //in
    AIL_DOUBLE Y2,                 //in
    AIL_DOUBLE Z2,                 //in
    AIL_DOUBLE SizeX,              //in
    AIL_DOUBLE SizeY,              //in
    AIL_INT64  ControlFlag         //in
)
```

## Description

This function adds a rectangle graphic to the specified 3D graphics list, allowing you to, for example, view the rectangle graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the rectangle graphic. When the rectangle graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the rectangle graphic's parent must be the root node. All coordinates are expressed in the coordinate system of the rectangle graphic's parent.

The rectangle graphic has its own coordinate system that represents the rectangle graphic's position and orientation with respect to its parent's coordinate system. The origin of the rectangle graphic's coordinate system is the rectangle graphic's center, and its Z-axis is the rectangle graphic's normal vector. The rectangle graphic's coordinate system's XY (Z=0) plane lies on the rectangle graphic. You can change the position and orientation of the rectangle graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraRect.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the rectangle graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the rectangle graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraRect.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the rectangle graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CreationMode` *(in, AIL_INT64)*

Specifies how the rectangle graphic is defined.

### `X1OrMatrix3dGeoId` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the first point used to define the rectangle graphic or the identifier of a transformation matrix.

### `Y1` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the first point used to define the rectangle graphic.

### `Z1` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the first point used to define the rectangle graphic.

### `X2` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the second point used to define the rectangle graphic.

### `Y2` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the second point used to define the rectangle graphic.

### `Z2` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the second point used to define the rectangle graphic.

### `SizeX` *(in, AIL_DOUBLE)*

Specifies the X-size of the rectangle graphic.

*For setting the X-size of the rectangle graphics*

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the X-size of the rectangle graphic. |

### `SizeY` *(in, AIL_DOUBLE)*

Specifies the Y-size of the rectangle graphic.

*For setting the Y-size of the rectangle graphics*

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the Y-size of the rectangle graphic. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the rectangle graphic

Set unused parameters to [`M_DEFAULT`](../../Reference/3dgra/M3dgraRect.md).

---

### `M_CENTER_AND_NORMAL`

Defines the rectangle graphic using its center point and normal vector.  The center of the rectangle is defined using[`X1OrMatrix3dGeoId`](../../Reference/3dgra/M3dgraRect.md), [`Y1`](../../Reference/3dgra/M3dgraRect.md), and [`Z1`](../../Reference/3dgra/M3dgraRect.md) and the normal is defined using [`X2`](../../Reference/3dgra/M3dgraRect.md), [`Y2`](../../Reference/3dgra/M3dgraRect.md), and [`Z2`](../../Reference/3dgra/M3dgraRect.md).

---

### `M_TRANSFORMATION_MATRIX`

Defines the rectangle using a transformation matrix.  The transformation matrix should define a coordinate system whose origin defines the center of the rectangle and whose XY-plane (Z=0) defines plane on which the rectangle lies. To set up the matrix, use[`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) with [`M_COORDINATE_SYSTEM_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md); specify the coordinates of the new coordinate system's origin and the components of two of its axes. The third axis will automatically be calculated such that it is perpendicular to the first two and respects the right hand rule. [`M3dgeoMatrixSetWithAxes`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) with [`M_COORDINATE_SYSTEM_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) returns a transformation matrix that moves your new coordinate system to your current working coordinate system.

### Combination Constants — For setting the position and rotation relative to the 3D graphics list's root node

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the position and rotation of the 3D graphic relative to the 3D graphics list's root node.

| Value | Description |
| --- | --- |
| `M_RELATIVE_TO_ROOT` | Specifies to set the position and rotation of a 3D graphic relative to the 3D graphics list's root node. |

## Return Value

**Type:** `AIL_INT64`

Returns the label of the rectangle graphic added to the 3D graphics list.
