---
doctype: Reference
module: 3dgeo
function: M3dgeoMatrixSetWithAxes
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoMatrixSetWithAxes"
---

# M3dgeoMatrixSetWithAxes

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

> Defines a transformation matrix that can transform points from the working coordinate system into the specified coordinate system, or vice versa.

## Syntax

```c
void M3dgeoMatrixSetWithAxes(
    AIL_ID     Matrix3dgeoId,  //out
    AIL_INT64  Mode,           //in
    AIL_DOUBLE OriginX,        //in
    AIL_DOUBLE OriginY,        //in
    AIL_DOUBLE OriginZ,        //in
    AIL_DOUBLE Axis1VectorX,   //in
    AIL_DOUBLE Axis1VectorY,   //in
    AIL_DOUBLE Axis1VectorZ,   //in
    AIL_DOUBLE Axis2VectorX,   //in
    AIL_DOUBLE Axis2VectorY,   //in
    AIL_DOUBLE Axis2VectorZ,   //in
    AIL_INT64  ControlFlag     //in
)
```

## Description

This function defines a transformation matrix that can transform points from the working coordinate system into a new specified coordinate system, or vice versa.

The new coordinate system is described by its origin and its orientation with respect to the working coordinate system. The [`Mode`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md) parameter specifies which two axes are used to define the new coordinate system. The first axis is defined using [`Axis1VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis1VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis1VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). The second axis is defined using [`Axis2VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis2VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis2VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). The components of the vector aligned with the second axis can be approximate, since this axis will be internally orthogonalized. However, the second axis must not be parallel to the first axis. The third axis is internally deduced by computing the vector that is orthogonal to the first two axes and respects the right-hand rule.

By default, this function returns the transformation matrix that transforms points from the working coordinate system into the new coordinate system. To get the transformation matrix that transforms the points from the new coordinate system into the working coordinate system, use [`M_COORDINATE_SYSTEM_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md).

> **Note:** Note that the two specified axes must have a magnitude that is greater than 0.

## Parameters

### `Matrix3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the transformation matrix object, previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `Mode` *(in, AIL_INT64)*

Specifies which two axes to use to define the new coordinate system. The first letter determines the first axis, and the second letter determines the second axis.

*For specifying the two axes to use*

| Value | Description |
| --- | --- |
| `M_XY_AXES` | Specifies the X-axis, and then the Y-axis. The X-axis is specified by [`Axis1VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis1VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis1VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). The Y-axis is approximately specified by [`Axis2VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis2VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis2VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). |
| `M_XZ_AXES` | Specifies the X-axis, and then the Z-axis. The X-axis is specified by [`Axis1VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis1VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis1VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). The Z-axis is approximately specified by [`Axis2VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis2VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis2VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). |
| `M_YX_AXES` | Specifies the Y-axis, and then the X-axis. The Y-axis is specified by [`Axis1VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis1VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis1VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). The X-axis is approximately specified by [`Axis2VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis2VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis2VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). |
| `M_YZ_AXES` | Specifies the Y-axis, and then the Z-axis. The Y-axis is specified by [`Axis1VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis1VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis1VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). The Z-axis is approximately specified by [`Axis2VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis2VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis2VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). |
| `M_ZX_AXES` | Specifies the Z-axis, and then the X-axis. The Z-axis is specified by [`Axis1VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis1VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis1VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). The X-axis is approximately specified by [`Axis2VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis2VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis2VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). |
| `M_ZY_AXES` | Specifies the Z-axis, and then the Y-axis. The Z-axis is specified by [`Axis1VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis1VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis1VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). The Y-axis is approximately specified by [`Axis2VectorX`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), [`Axis2VectorY`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md), and [`Axis2VectorZ`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). |

*For specifying the direction of the transformation between coordinate systems*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_POSITION_TRANSFORMATION`](../../Reference/3dgeo/M3dgeoMatrixSetWithAxes.md). |
| `M_COORDINATE_SYSTEM_TRANSFORMATION` | Specifies to return the transformation matrix that transforms points such that their coordinates are expressed relative to the specified position and orientation. When you apply the resulting matrix, point coordinates are passively transformed, and will be expressed relative to the newly specified origin and axes. This is a backward mapping that you can use to fixture points to a new coordinate system. In the example below, the point P (3, 0, 2) is expressed as P′ (2, 0, 1) in the new coordinate system.

*[Image: 3dgeo_coord_sys_transform_shifted_origin.png]* |
| `M_POSITION_TRANSFORMATION` | Specifies to return the transformation matrix that transforms points such that they are offset by the specified position and orientation. When you apply the resulting matrix, point coordinates are actively transformed, and will be expressed relative to the original origin and axes. This is a forward mapping that you can use to move points. In the example below, the point P (3, 0, 2) is moved to P′ (4, 0, 3). Note that P's offset from the original coordinate system is equivalent to P′'s offset from the new coordinate system.

*[Image: 3dgeo_pos_transform_shifted_origin.png]* |

### `OriginX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the new coordinate system's origin, expressed in the working coordinate system.

### `OriginY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the new coordinate system's origin, expressed in the working coordinate system.

### `OriginZ` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the new coordinate system's origin, expressed in the working coordinate system.

### `Axis1VectorX` *(in, AIL_DOUBLE)*

Specifies the X-component of the vector aligned with the new coordinate system's first axis, expressed in the working coordinate system.

### `Axis1VectorY` *(in, AIL_DOUBLE)*

Specifies the Y-component of the vector aligned with the new coordinate system's first axis, expressed in the working coordinate system.

### `Axis1VectorZ` *(in, AIL_DOUBLE)*

Specifies the Z-component of the vector aligned with the new coordinate system's first axis, expressed in the working coordinate system.

### `Axis2VectorX` *(in, AIL_DOUBLE)*

Specifies the approximate X-component of the vector aligned with the new coordinate system's second axis, expressed in the working coordinate system.

### `Axis2VectorY` *(in, AIL_DOUBLE)*

Specifies the approximate Y-component of the vector aligned with the new coordinate system's second axis, expressed in the working coordinate system.

### `Axis2VectorZ` *(in, AIL_DOUBLE)*

Specifies the approximate Z-component of the vector aligned with the new coordinate system's second axis, expressed in the working coordinate system.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
