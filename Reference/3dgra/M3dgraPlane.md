---
doctype: Reference
module: 3dgra
function: M3dgraPlane
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraPlane"
---

# M3dgraPlane

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

> Add a plane graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraPlane(
    AIL_ID     List3dgraId,   //out
    AIL_INT64  ParentLabel,   //in
    AIL_INT64  CreationMode,  //in
    AIL_DOUBLE X1,            //in
    AIL_DOUBLE Y1,            //in
    AIL_DOUBLE Z1,            //in
    AIL_DOUBLE X2OrD,         //in
    AIL_DOUBLE Y2,            //in
    AIL_DOUBLE Z2,            //in
    AIL_DOUBLE X3,            //in
    AIL_DOUBLE Y3,            //in
    AIL_DOUBLE Z3,            //in
    AIL_DOUBLE Size,          //in
    AIL_INT64  ControlFlag    //in
)
```

## Description

This function adds a plane graphic to the specified 3D graphics list, allowing you to, for example, view the plane graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the plane graphic. When the plane graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the plane graphic's parent must be the root node. All coordinates are expressed in the coordinate system of the plane graphic's parent.

The plane graphic has its own coordinate system that represents the plane graphic's position and orientation with respect to its parent's coordinate system. The plane graphic's coordinate system's origin is the point on the plane graphic closest to the origin of its parent's coordinate system, and its Z-axis is the plane graphic's normal vector. The plane graphic's coordinate system's XY (Z=0) plane lies on the plane graphic. You can change the position and orientation of the plane graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraPlane.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the plane graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the plane graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraPlane.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the plane graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CreationMode` *(in, AIL_INT64)*

Specifies how the plane graphic is defined.

### `X1` *(in, AIL_DOUBLE)*

Specifies the first parameter used to define the plane graphic.

### `Y1` *(in, AIL_DOUBLE)*

Specifies the second parameter used to define the plane graphic.

### `Z1` *(in, AIL_DOUBLE)*

Specifies the third parameter used to define the plane graphic.

### `X2OrD` *(in, AIL_DOUBLE)*

Specifies the fourth parameter used to define the plane graphic.

### `Y2` *(in, AIL_DOUBLE)*

Specifies the fifth parameter used to define the plane graphic.

### `Z2` *(in, AIL_DOUBLE)*

Specifies the sixth parameter used to define the plane graphic.

### `X3` *(in, AIL_DOUBLE)*

Specifies the seventh parameter used to define the plane graphic.

### `Y3` *(in, AIL_DOUBLE)*

Specifies the eighth parameter used to define the plane graphic.

### `Z3` *(in, AIL_DOUBLE)*

Specifies the ninth parameter used to define the plane graphic.

### `Size` *(in, AIL_DOUBLE)*

Specifies the size of the plane graphic. Finite plane graphics are square-shaped. If the plane graphic is infinite, it is clipped using the 3D graphics list's clipping box, which results in a polygon with 3 to 6 edges.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the plane graphic

Set unused parameters to [`M_DEFAULT`](../../Reference/3dgra/M3dgraPlane.md).

---

### `M_COEFFICIENTS`

Defines the plane graphic using coefficients from the plane equation (`Ax + By + Cz + D = 0`).  Coefficients _A_, _B_, and _C_ cannot all be zero.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_INFINITE`](../../Reference/3dgra/M3dgraPlane.md). |
| `M_INFINITE` | Specifies an infinite plane graphic. The plane graphic's size is determined upon creation, using its 3D graphics list's clipping box. |

---

### `M_POINT_AND_NORMAL`

Defines the plane graphic using a point on the plane graphic and the plane graphic's normal.  The plane graphic's normal must have a non-zero magnitude.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_INFINITE`](../../Reference/3dgra/M3dgraPlane.md). |
| `M_INFINITE` | Specifies an infinite plane graphic. The plane graphic's size is determined upon creation, using its 3D graphics list's clipping box. |
| `Value > 0.0` | Specifies the length of the plane graphic's sides. The plane graphic is a square centered on ([`X1`](../../Reference/3dgra/M3dgraPlane.md), [`Y1`](../../Reference/3dgra/M3dgraPlane.md), [`Z1`](../../Reference/3dgra/M3dgraPlane.md)). |

---

### `M_POINT_AND_TWO_VECTORS`

Defines the plane graphic using a point on the plane graphic and two vectors parallel to the plane graphic.  The two vectors must have non-zero magnitudes and cannot be co-linear.

| Value | Description |
| --- | --- |
| *(see [`M_POINT_AND_NORMAL`](Reference/3dgra/M3dgraPlane.md))* |  |

---

### `M_THREE_POINTS`

Defines the plane graphic using three points on the plane graphic.

| Value | Description |
| --- | --- |
| *(see [`M_POINT_AND_NORMAL`](Reference/3dgra/M3dgraPlane.md))* |  |

## Return Value

**Type:** `AIL_INT64`

Returns the label of the plane graphic added to the 3D graphics list.
