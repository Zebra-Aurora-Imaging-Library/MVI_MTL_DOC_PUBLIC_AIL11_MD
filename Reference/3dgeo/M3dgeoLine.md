---
doctype: Reference
module: 3dgeo
function: M3dgeoLine
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoLine"
---

# M3dgeoLine

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

> Define an arbitrary 3D line geometry object.

## Syntax

```c
void M3dgeoLine(
    AIL_ID     Geometry3dgeoId,  //out
    AIL_INT64  CreationMode,     //in
    AIL_DOUBLE XPos1,            //in
    AIL_DOUBLE YPos1,            //in
    AIL_DOUBLE ZPos1,            //in
    AIL_DOUBLE XPos2OrVector,    //in
    AIL_DOUBLE YPos2OrVector,    //in
    AIL_DOUBLE ZPos2OrVector,    //in
    AIL_DOUBLE Length,           //in
    AIL_INT64  ControlFlag       //in
)
```

## Description

This function defines an arbitrary 3D line geometry object. You can translate, rotate, scale, or transform the resulting line, using the 3D image processing module.

You can use the resulting line to, for example, perform an arithmetic operation on a depth map using the 3D image processing module, or calculate its distance from each point in a point cloud using the 3D metrology module.

If you want to define a line geometry object from results obtained in a different module, you can use the copy function of that module.

All coordinates are expressed in world units in the working coordinate system.

> **Note:** Note that if [`Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoLine.md) specifies a previously-defined line, you can leave some of its attributes unchanged, even if that attribute was set using a different creation mode or was modified using the 3D image processing module. To do so, set the corresponding parameter to [`M_UNCHANGED`](../../Reference/3dgeo/M3dgeoLine.md).

## Parameters

### `Geometry3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the 3D geometry object, previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `CreationMode` *(in, AIL_INT64)*

Specifies how the line is defined.

### `XPos1` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the first point used to define the line.

### `YPos1` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the first point used to define the line.

### `ZPos1` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the first point used to define the line.

### `XPos2OrVector` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the second point used to define the line, or the X-component of the vector defining the line's direction and length (if [`Length`](../../Reference/3dgeo/M3dgeoLine.md) is [`M_DEFAULT`](../../Reference/3dgeo/M3dgeoLine.md)).

### `YPos2OrVector` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the second point used to define the line, or the Y-component of the vector defining the line's direction and length (if [`Length`](../../Reference/3dgeo/M3dgeoLine.md) is [`M_DEFAULT`](../../Reference/3dgeo/M3dgeoLine.md)).

### `ZPos2OrVector` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the second point used to define the line, or the Z-component of the vector defining the line's direction and length (if [`Length`](../../Reference/3dgeo/M3dgeoLine.md) is [`M_DEFAULT`](../../Reference/3dgeo/M3dgeoLine.md)).

### `Length` *(in, AIL_DOUBLE)*

Specifies to override the default line length.

*For specifying to override the default line length*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default length defined by the creation mode. |
| `M_INFINITE` | Specifies an infinite line. |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value >= 0.0` | Specifies to override the line's length with a specific value. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the line

---

### `M_POINT_AND_VECTOR`

Defines the line using a point on the line and a nonzero vector defining the line's direction.  By default, the length of the line is finite and is set to the vector's magnitude. The point is at the start of the line. You can determine the coordinates of the line's end point by adding the given vector's X-, Y-, and Z-components to the first point's respective coordinates. You can also use [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_END_POINT_...`](../../Reference/3dgeo/M3dgeoInquire.md).  > **Note:** Note that if you set one of the vector's components to [`M_UNCHANGED`](../../Reference/3dgeo/M3dgeoLine.md), you must also set the other two components to [`M_UNCHANGED`](../../Reference/3dgeo/M3dgeoLine.md).

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the X-coordinate of the first point on the line. |
| *(see [`M_POINT_AND_VECTOR`](Reference/3dgeo/M3dgeoLine.md))* |  |
| *(see [`M_POINT_AND_VECTOR`](Reference/3dgeo/M3dgeoLine.md))* |  |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the X-component of the vector defining the line's direction. |
| *(see [`M_POINT_AND_VECTOR`](Reference/3dgeo/M3dgeoLine.md))* |  |
| *(see [`M_POINT_AND_VECTOR`](Reference/3dgeo/M3dgeoLine.md))* |  |

---

### `M_TWO_POINTS`

Defines the line using any two non-identical points.  By default, the length of the line is finite and is set to the distance between the two specified points. The line starts at the first point, and ends at the second point.

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the X-coordinate of the first point on the line. |
| *(see [`M_TWO_POINTS`](Reference/3dgeo/M3dgeoLine.md))* |  |
| *(see [`M_TWO_POINTS`](Reference/3dgeo/M3dgeoLine.md))* |  |
