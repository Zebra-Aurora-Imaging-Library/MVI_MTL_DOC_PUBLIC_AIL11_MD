---
doctype: Reference
module: 3dgeo
function: M3dgeoCylinder
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoCylinder"
---

# M3dgeoCylinder

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

> Define a 3D cylinder geometry object.

## Syntax

```c
void M3dgeoCylinder(
    AIL_ID     Geometry3dgeoId,  //out
    AIL_INT64  CreationMode,     //in
    AIL_DOUBLE XPos1,            //in
    AIL_DOUBLE YPos1,            //in
    AIL_DOUBLE ZPos1,            //in
    AIL_DOUBLE XPos2OrVector,    //in
    AIL_DOUBLE YPos2OrVector,    //in
    AIL_DOUBLE ZPos2OrVector,    //in
    AIL_DOUBLE Radius,           //in
    AIL_DOUBLE Length,           //in
    AIL_INT64  ControlFlag       //in
)
```

## Description

This function defines an arbitrary right circular 3D cylinder geometry object. You can translate, rotate, scale, or transform the resulting cylinder, using the 3D image processing module.

You can use the resulting cylinder to, for example, crop a point cloud or perform an arithmetic operation on a depth map using the 3D image processing module, or calculate its distance from each point in a point cloud using the 3D metrology module.

If you want to define a cylinder geometry object from results obtained in a different module, you can use the copy function of that module.

All coordinates are expressed in world units in the working coordinate system.

> **Note:** Note that if [`Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoCylinder.md) specifies a previously-defined cylinder, you can leave some of its attributes unchanged, even if that attribute was set using a different creation mode or was modified using the 3D image processing module. To do so, set the corresponding parameter to [`M_UNCHANGED`](../../Reference/3dgeo/M3dgeoCylinder.md).

## Parameters

### `Geometry3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the 3D geometry object, previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `CreationMode` *(in, AIL_INT64)*

Specifies how the cylinder is defined.

### `XPos1` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of a point on the cylinder's central axis.

### `YPos1` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of a point on the cylinder's central axis.

### `ZPos1` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of a point on the cylinder's central axis.

### `XPos2OrVector` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the cylinder's second defining point, or the X-component of the cylinder's central axis vector.

### `YPos2OrVector` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the cylinder's second defining point, or the Y-component of the cylinder's central axis vector.

### `ZPos2OrVector` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the cylinder's second defining point, or the Z-component of the cylinder's central axis vector.

### `Radius` *(in, AIL_DOUBLE)*

Specifies the cylinder's radius.

*For specifying the cylinder's radius*

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value > 0.0` | Specifies the cylinder's radius. |

### `Length` *(in, AIL_DOUBLE)*

Specifies to override the default cylinder length.

*For specifying to override the default cylinder length*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default length defined by the creation mode. |
| `M_INFINITE` | Specifies an infinite cylinder. |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value >= 0.0` | Specifies to override the cylinder's length with a specific value. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether the resulting cylinder has bases. Cylinders with [`Length`](../../Reference/3dgeo/M3dgeoCylinder.md) set to [`M_INFINITE`](../../Reference/3dgeo/M3dgeoCylinder.md) cannot have bases.

*For specifying if the cylinder has bases*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

For a finite cylinder, this value is the same as [`M_WITH_BASES`](../../Reference/3dgeo/M3dgeoCylinder.md). For an infinite cylinder, this value is the same as [`M_WITHOUT_BASES`](../../Reference/3dgeo/M3dgeoCylinder.md). |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `M_WITH_BASES` | Specifies that the resulting cylinder has a circular base at each end. This value is only supported for finite cylinders. |
| `M_WITHOUT_BASES` | Specifies that the resulting cylinder does not have bases. |

## Parameter Associations

### For specifying the cylinder

---

### `M_POINT_AND_VECTOR`

Defines the cylinder using a point on the cylinder's central axis and a nonzero vector defining the central axis direction.  By default, the length of the cylinder is finite and is set to the vector's magnitude. The point is at the center of the cylinder's first circular base.  > **Note:** Note that if you set one of the vector's components to [`M_UNCHANGED`](../../Reference/3dgeo/M3dgeoCylinder.md), you must also set the other two components to [`M_UNCHANGED`](../../Reference/3dgeo/M3dgeoCylinder.md).

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the X-coordinate of the first point on the cylinder's central axis. |
| *(see [`M_POINT_AND_VECTOR`](Reference/3dgeo/M3dgeoCylinder.md))* |  |
| *(see [`M_POINT_AND_VECTOR`](Reference/3dgeo/M3dgeoCylinder.md))* |  |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the X-component of the vector defining the central axis direction. |
| *(see [`M_POINT_AND_VECTOR`](Reference/3dgeo/M3dgeoCylinder.md))* |  |
| *(see [`M_POINT_AND_VECTOR`](Reference/3dgeo/M3dgeoCylinder.md))* |  |

---

### `M_TWO_POINTS`

Defines the cylinder using any two non-identical points on the cylinder's central axis.  By default, the length of the cylinder is finite and is set to the distance between the two specified points. The first specified point is at the center of the cylinder's first circular base and the second specified point is at the center of the cylinder's second circular base.

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the X-coordinate of the first point on the cylinder's central axis. |
| *(see [`M_TWO_POINTS`](Reference/3dgeo/M3dgeoCylinder.md))* |  |
| *(see [`M_TWO_POINTS`](Reference/3dgeo/M3dgeoCylinder.md))* |  |
