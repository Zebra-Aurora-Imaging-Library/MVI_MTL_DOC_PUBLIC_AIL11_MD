---
doctype: Reference
module: 3dgra
function: M3dgraArcFill
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraArcFill"
---

# M3dgraArcFill

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

> Add a filled circular arc graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraArcFill(
    AIL_ID     List3dgraId,   //out
    AIL_INT64  ParentLabel,   //in
    AIL_INT64  CreationMode,  //in
    AIL_DOUBLE X1,            //in
    AIL_DOUBLE Y1,            //in
    AIL_DOUBLE Z1,            //in
    AIL_DOUBLE X2,            //in
    AIL_DOUBLE Y2,            //in
    AIL_DOUBLE Z2,            //in
    AIL_DOUBLE X3,            //in
    AIL_DOUBLE Y3,            //in
    AIL_DOUBLE Z3,            //in
    AIL_DOUBLE Angle,         //in
    AIL_INT64  ControlFlag    //in
)
```

## Description

This function adds a filled circular arc graphic or, with an angle of 360 degrees, a filled circle graphic to the specified 3D graphics list, allowing you to, for example, view the filled circular arc graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the filled arc graphic. When the filled arc graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the filled arc graphic's parent must be the root node. The coordinates of the filled arc's vertices are expressed in the coordinate system of the filled arc graphic's parent.

The filled arc graphic has its own coordinate system that represents the filled arc's position and orientation with respect to its parent's coordinate system. The filled arc's coordinate system's origin is the filled arc's center, its X-axis is the vector directed from the filled arc's center to the filled arc's start point, and its Z-axis is the filled arc's normal vector. You can inquire the filled arc's start point and normal vector using [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md) with [`M_START_POINT_...`](../../Reference/3dgra/M3dgraInquire.md) and [`M_NORMAL...`](../../Reference/3dgra/M3dgraInquire.md), respectively. You can change the position and orientation of the filled arc graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraArcFill.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the filled arc graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the filled arc graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraArcFill.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the filled arc graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CreationMode` *(in, AIL_INT64)*

Specifies how the filled arc graphic is defined.

### `X1` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the first point used to define the filled arc.

### `Y1` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the first point used to define the filled arc.

### `Z1` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the first point used to define the filled arc.

### `X2` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the second point used to define the filled arc.

### `Y2` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the second point used to define the filled arc.

### `Z2` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the second point used to define the filled arc.

### `X3` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the third point used to define the filled arc.

### `Y3` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the third point used to define the filled arc.

### `Z3` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the third point used to define the filled arc.

### `Angle` *(in, AIL_DOUBLE)*

Specifies the angle used to define the filled arc.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the filled arc graphic

Set unused parameters to [`M_DEFAULT`](../../Reference/3dgra/M3dgraArcFill.md).

---

### `M_CENTER_AND_TWO_POINTS`

Defines a filled circular arc graphic using a center and two end points. The result will be a filled circular arc, with a radius defined by the center and the first end point. The filled arc extends from the first end point to the point on the line that passes through the center and the second end point. Therefore, the second end point does not have to be a point on the filled arc.*[Image: 3dgra_arc_fill_center_and_two_points.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BIGGEST_ANGLE` | Specifies to use the biggest angle formed by the specified center and end points. |
| `M_SMALLEST_ANGLE` *(default)* | Specifies to use the smallest angle formed by the specified center and end points. |

---

### `M_CENTER_AND_TWO_VECTORS`

Defines a filled circular arc graphic using a center and two vectors. The first vector defines the distance between the center and the first end point, and its magnitude determines the filled arc radius. The second vector defines the distance between the center and the second end point. The result will be a filled circular arc that extends from the first end point, to the point on the line that passes through the center and the second end point. Therefore, the second end point does not have to be a point on the filled arc.*[Image: 3dgra_arc_fill_center_and_two_vectors.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BIGGEST_ANGLE` | Specifies to use the biggest angle formed by the specified center and end points. |
| `M_SMALLEST_ANGLE` *(default)* | Specifies to use the smallest angle formed by the specified center and end points. |

---

### `M_NORMAL_AND_ANGLE`

Defines a filled circular arc graphic using a center, a starting vector, a normal vector, and an filled arc angle. The starting vector defines the direction between the center and the start point, as well as the filled arc radius. The direction in which the filled arc extends follows the right-hand rule. If the starting direction vector and the normal vector are not perpendicular, the normal vector is orthogonalized.*[Image: 3dgra_arc_fill_normal_and_angle.png]*

---

### `M_THREE_POINTS`

Defines a filled circular arc graphic using three points on the filled arc. The filled arc starts at the first point, passes through the second point, and ends at the third point.*[Image: 3dgra_arc_fill_three_points.png]*

## Return Value

**Type:** `AIL_INT64`

Returns the label of the filled arc graphic added to the 3D graphics list.
