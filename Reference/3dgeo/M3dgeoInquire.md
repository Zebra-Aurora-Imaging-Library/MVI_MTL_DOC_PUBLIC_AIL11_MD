---
doctype: Reference
module: 3dgeo
function: M3dgeoInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoInquire"
---

# M3dgeoInquire

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

> Inquire about a 3D geometry object setting or transformation matrix object setting.

## Syntax

```c
AIL_DOUBLE M3dgeoInquire(
    AIL_ID    GeometryOrMatrix3dgeoId,  //in
    AIL_INT64 InquireType,              //in
    void *    UserVarPtr                //out
)
```

## Description

This function inquires about a specified setting of a 3D geometry object or transformation matrix object.

> **Note:** Note that if the specified 3D geometry object was transformed (for example, using the 3D image processing module), this function returns the modified coordinates and dimensions. In this case, the coordinates and dimensions returned by [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) might not be the same initial coordinates and dimensions used to define the 3D geometry object.

## Parameters

### `GeometryOrMatrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the 3D geometry object or transformation matrix object about which to inquire information. The object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) or [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `InquireType` *(in, AIL_INT64)*

Specifies the setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about any 3D geometry object

For any 3D geometry object, the [`GeometryOrMatrix3dgeoId`](../../Reference/3dgeo/M3dgeoInquire.md) and [`InquireType`](../../Reference/3dgeo/M3dgeoInquire.md) parameters can be set to one of the following:

---

### `3D geometry object ID`

Specifies a 3D geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

#### `M_DIMENSION`

Inquires the dimensionality of the 3D geometry. The dimensionality specifies, for a point on the geometry, the number of coordinates you must have to determine all three coordinates. For example, for a 3D line (dimensionality of 1), if you have the X-coordinate of a point on the line, you can find the corresponding Y- and Z-coordinates. Similarly, for a plane (dimensionality of 2), if you have the X- and Y-coordinates for a point on the plane, you can find the corresponding Z-coordinate. Use [`M3dgeoEvalCurve`](../../Reference/3dgeo/M3dgeoEvalCurve.md) to find missing coordinates for a 3D line; use [`M3dgeoEvalSurface`](../../Reference/3dgeo/M3dgeoEvalSurface.md) to find missing coordinates for a 3D plane or sphere.

| Value | Description |
| --- | --- |
| `0 <= Value <= 2` | Specifies the dimensionality of the 3D geometry. |

#### `M_GEOMETRY_TYPE`

Inquires the 3D geometry type.

| Value | Description |
| --- | --- |
| `M_BOX` | Specifies a box. |
| `M_CYLINDER` | Specifies a cylinder. |
| `M_LINE` | Specifies a line. |
| `M_NOT_INITIALIZED` | Specifies that a geometry has not been defined for the 3D geometry object. |
| `M_PLANE` | Specifies a plane. |
| `M_POINT` | Specifies a point. |
| `M_SPHERE` | Specifies a sphere. |

### For inquiring about a specific type of 3D geometry object

For a 3D geometry object of a specific type, the [`GeometryOrMatrix3dgeoId`](../../Reference/3dgeo/M3dgeoInquire.md) and [`InquireType`](../../Reference/3dgeo/M3dgeoInquire.md) parameters can be set to one of the following.  > **Note:** Note that for any 3D geometry object, you can use any inquire type available for its respective geometry type, regardless of the creation mode used to define the geometry.

---

### `3D box geometry object ID`

Specifies a 3D box geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and successfully defined as a box.  > **Note:** Note that a box's corners are assigned the following indices upon initially defining the box, based on the corner's position with respect to the axes of the working coordinate system. The index of a box corner does not change based on the orientation of the box.*[Image: 3dgeo_box.png]*

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the specified box corner, expressed in the working coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the specified box corner, expressed in the working coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the specified box corner, expressed in the working coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinates of the corners of the specified box face, expressed in the working coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinates of the corners of the specified box face, expressed in the working coordinate system. |

#### `?`

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinates of the corners of the specified box face, expressed in the working coordinate system. |

#### `M_AXIS_ALIGNED`

Inquires whether the box is axis-aligned with the working coordinate system.  Note that this inquire type returns the opposite of the [`M_ROTATED`](../../Reference/3dgeo/M3dgeoInquire.md) inquire type, except when an axis-aligned box is rotated by 180 degrees; in this case, [`M_AXIS_ALIGNED`](../../Reference/3dgeo/M3dgeoInquire.md) and [`M_ROTATED`](../../Reference/3dgeo/M3dgeoInquire.md) both return [`M_TRUE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the box is not axis-aligned; a rotation (other than a 180 degree rotation) has been applied. |
| `M_TRUE` | Specifies that the box is axis-aligned; no rotation (or a 180 degree rotation) has been applied. |

#### `M_CENTER_X`

Inquires the X-coordinate of the box's center point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the box's center point, expressed in the working coordinate system. |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the box's center point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the box's center point, expressed in the working coordinate system. |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the box's center point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the box's center point, expressed in the working coordinate system. |

#### `M_CORNER_X_ALL`

Inquires the X-coordinate of each of the box's 8 corners, in their index order.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-component the box's corner, expressed in the working coordinate system. |

#### `M_CORNER_Y_ALL`

Inquires the Y-coordinate of each of the box's 8 corners, in their index order.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-component the box's corner, expressed in the working coordinate system. |

#### `M_CORNER_Z_ALL`

Inquires the Z-coordinate of each of the box's 8 corners, in their index order.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-component the box's corner, expressed in the working coordinate system. |

#### `M_FACE_n_CORNERS_X`

Inquires the X-coordinates of each corner of box face _n_, in their index order, where _n_ is the box face index.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinates of the corners of the box face assigned with index _n_, expressed in the working coordinate system. |

#### `M_FACE_n_CORNERS_Y`

Inquires the Y-coordinates of each corner of box face _n_, in their index order, where _n_ is the box face index.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinates of the corners of the box face assigned with index _n_, expressed in the working coordinate system. |

#### `M_FACE_n_CORNERS_Z`

Inquires the Z-coordinates of each corner of box face _n_, in their index order, where _n_ is the box face index.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinates of the corners of the box face assigned with index _n_, expressed in the working coordinate system. |

#### `M_ROTATED`

Inquires whether the box has been rotated. This inquire type returns true when the box's rotation matrix is not the identity matrix. This can occur even if you did not use, for example, [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md) or [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) to explicitly rotate the box. If you copied the box from a statistics 3D image processing result buffer (after calculating a semi-oriented box) or a 3D model finder result buffer, it can have a rotation matrix other than the identity matrix.  Note that this inquire type returns the opposite of the [`M_AXIS_ALIGNED`](../../Reference/3dgeo/M3dgeoInquire.md) inquire type, except when an axis-aligned box is rotated by 180 degrees; in this case, [`M_AXIS_ALIGNED`](../../Reference/3dgeo/M3dgeoInquire.md) and [`M_ROTATED`](../../Reference/3dgeo/M3dgeoInquire.md) both return [`M_TRUE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the box is not rotated; its rotation matrix is the identity matrix. |
| `M_TRUE` | Specifies that the box is rotated; its rotation matrix is not the identity matrix. |

#### `M_SIZE_X`

Inquires the box's length along the X-axis, ignoring the box's rotation. The length is returned as if the box is axis-aligned.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the length of the unrotated box, in world units, along the X-axis of the working coordinate system. |

#### `M_SIZE_Y`

Inquires the box's length along the Y-axis, ignoring the box's rotation. The length is returned as if the box is axis-aligned.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the length of the unrotated box, in world units, along the Y-axis of the working coordinate system. |

#### `M_SIZE_Z`

Inquires the box's length along the Z-axis, ignoring the box's rotation. The length is returned as if the box is axis-aligned.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the length of the unrotated box, in world units, along the Z-axis of the working coordinate system. |

#### `M_UNROTATED_MAX_X`

Inquires the box's maximum X-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

| Value | Description |
| --- | --- |
| `Value` | Specifies the box's maximum X-coordinate, ignoring the box's rotation, expressed in the working coordinate system. |

#### `M_UNROTATED_MAX_Y`

Inquires the box's maximum Y-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

| Value | Description |
| --- | --- |
| `Value` | Specifies the box's maximum Y-coordinate, ignoring the box's rotation, expressed in the working coordinate system. |

#### `M_UNROTATED_MAX_Z`

Inquires the box's maximum Z-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

| Value | Description |
| --- | --- |
| `Value` | Specifies the box's maximum Z-coordinate, ignoring the box's rotation, expressed in the working coordinate system. |

#### `M_UNROTATED_MIN_X`

Inquires the box's minimum X-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

| Value | Description |
| --- | --- |
| `Value` | Specifies the box's minimum X-coordinate, ignoring the box's rotation, expressed in the working coordinate system. |

#### `M_UNROTATED_MIN_Y`

Inquires the box's minimum Y-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

| Value | Description |
| --- | --- |
| `Value` | Specifies the box's minimum Y-coordinate, ignoring the box's rotation, expressed in the working coordinate system. |

#### `M_UNROTATED_MIN_Z`

Inquires the box's minimum Z-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

| Value | Description |
| --- | --- |
| `Value` | Specifies the box's minimum Z-coordinate, ignoring the box's rotation, expressed in the working coordinate system. |

---

### `3D cylinder geometry object ID`

Specifies a 3D cylinder geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and successfully defined as a cylinder.*[Image: 3dgeo_cylinder.png]*

#### `M_AXIS_X`

Inquires the X-component of the cylinder's central axis unit vector. This vector does not reflect the cylinder's length, regardless of whether a vector was used to define the cylinder's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the X-component of the cylinder's central axis unit vector, expressed in the working coordinate system. |

#### `M_AXIS_Y`

Inquires the Y-component of the cylinder's central axis unit vector. This vector does not reflect the cylinder's length, regardless of whether a vector was used to define the cylinder's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Y-component of the cylinder's central axis unit vector, expressed in the working coordinate system. |

#### `M_AXIS_Z`

Inquires the Z-component of the cylinder's central axis unit vector. This vector does not reflect the cylinder's length, regardless of whether a vector was used to define the cylinder's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Z-component of the cylinder's central axis unit vector, expressed in the working coordinate system. |

#### `M_BASES`

Inquires whether the cylinder has bases.

| Value | Description |
| --- | --- |
| `M_WITH_BASES` | Specifies that the cylinder has bases. |
| `M_WITHOUT_BASES` | Specifies that the cylinder does not have bases. |

#### `M_CENTER_X`

Inquires the X-coordinate of the center point on the cylinder's central axis. This inquire type is only available if the cylinder's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the center point on the cylinder's central axis, expressed in the working coordinate system. |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the center point on the cylinder's central axis. This inquire type is only available if the cylinder's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the center point on the cylinder's central axis, expressed in the working coordinate system. |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the center point on the cylinder's central axis. This inquire type is only available if the cylinder's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the center point on the cylinder's central axis, expressed in the working coordinate system. |

#### `M_END_POINT_X`

Inquires the X-coordinate of the cylinder's end point, positioned at the center of the cylinder's second circular base. This inquire type is only available if the cylinder's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the cylinder's end point, expressed in the working coordinate system. |

#### `M_END_POINT_Y`

Inquires the Y-coordinate of the cylinder's end point, positioned at the center of the cylinder's second circular base. This inquire type is only available if the cylinder's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the cylinder's end point, expressed in the working coordinate system. |

#### `M_END_POINT_Z`

Inquires the Z-coordinate of the cylinder's end point, positioned at the center of the cylinder's second circular base. This inquire type is only available if the cylinder's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the cylinder's end point, expressed in the working coordinate system. |

#### `M_LENGTH`

Inquires the cylinder's length.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies that the cylinder is infinite. |
| `Value >= 0.0` | Specifies the cylinder's length in world units. |

#### `M_RADIUS`

Inquires the cylinder's radius.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the cylinder's radius in world units. |

#### `M_START_POINT_X`

Inquires the X-coordinate of the cylinder's start point on the cylinder's central axis.  When the cylinder's length is infinite, this returns the X-coordinate of the first point used to define the cylinder.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the first point on the central axis used to define the cylinder, expressed in the working coordinate system. |

#### `M_START_POINT_Y`

Inquires the Y-coordinate of the cylinder's start point on the cylinder's central axis.  When the cylinder's length is infinite, this returns the Y-coordinate of the first point used to define the cylinder.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the first point on the central axis used to define the cylinder, expressed in the working coordinate system. |

#### `M_START_POINT_Z`

Inquires the Z-coordinate of the cylinder's start point on the cylinder's central axis.  When the cylinder's length is infinite, this returns the Z-coordinate of the first point used to define the cylinder.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the first point on the central axis used to define the cylinder, expressed in the working coordinate system. |

---

### `3D line geometry object ID`

Specifies a 3D line geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and successfully defined as a line.

#### `M_AXIS_X`

Inquires the X-component of the line's direction unit vector. This vector does not reflect the line's length, regardless of whether a vector was used to define the line's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the X-component of the line's direction unit vector, expressed in the working coordinate system. |

#### `M_AXIS_Y`

Inquires the Y-component of the line's direction unit vector. This vector does not reflect the line's length, regardless of whether a vector was used to define the line's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Y-component of the line's direction unit vector, expressed in the working coordinate system. |

#### `M_AXIS_Z`

Inquires the Z-component of the line's direction unit vector. This vector does not reflect the line's length, regardless of whether a vector was used to define the line's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Z-component of the line's direction unit vector, expressed in the working coordinate system. |

#### `M_CENTER_X`

Inquires the X-coordinate of the line's center point. This inquire type is only available if the line's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the center point on the line, expressed in the working coordinate system. |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the line's center point. This inquire type is only available if the line's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the center point on the line, expressed in the working coordinate system. |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the line's center point. This inquire type is only available if the line's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the center point on the line, expressed in the working coordinate system. |

#### `M_END_POINT_X`

Inquires the X-coordinate of the line's end point. This inquire type is only available if the line's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the line's end point, expressed in the working coordinate system. |

#### `M_END_POINT_Y`

Inquires the Y-coordinate of the line's end point. This inquire type is only available if the line's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the line's end point, expressed in the working coordinate system. |

#### `M_END_POINT_Z`

Inquires the Z-coordinate of the line's end point. This inquire type is only available if the line's length is not [`M_INFINITE`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the line's end point, expressed in the working coordinate system. |

#### `M_LENGTH`

Inquires the line's length.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies that the line is infinite. |
| `Value >= 0.0` | Specifies the line's length in world units. |

#### `M_START_POINT_X`

Inquires the X-coordinate of the line's start point.  When the line's length is infinite, this returns the X-coordinate of the first point used to define the line.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the first point used to define the line, expressed in the working coordinate system. |

#### `M_START_POINT_Y`

Inquires the Y-coordinate of the line's start point.  When the line's length is infinite, this returns the Y-coordinate of the first point used to define the line.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the first point used to define the line, expressed in the working coordinate system. |

#### `M_START_POINT_Z`

Inquires the Z-coordinate of the line's start point.  When the line's length is infinite, this returns the Z-coordinate of the first point used to define the line.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the first point used to define the line, expressed in the working coordinate system. |

---

### `3D plane geometry object ID`

Specifies a 3D plane geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and successfully defined as a plane.

#### `M_CLOSEST_TO_ORIGIN_X`

Inquires the X-coordinate of the point on the plane closest to the origin of the working coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the point on the plane closest to the origin of the working coordinate system. |

#### `M_CLOSEST_TO_ORIGIN_Y`

Inquires the Y-coordinate of the point on the plane closest to the origin of the working coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the point on the plane closest to the origin of the working coordinate system. |

#### `M_CLOSEST_TO_ORIGIN_Z`

Inquires the Z-coordinate of the point on the plane closest to the origin of the working coordinate system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the point on the plane closest to the origin of the working coordinate system. |

#### `M_COEFFICIENT_A`

Inquires the coefficient _A_ of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the coefficient _A_ of the plane equation. |

#### `M_COEFFICIENT_B`

Inquires the coefficient _B_ of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the coefficient _B_ of the plane equation. |

#### `M_COEFFICIENT_C`

Inquires the coefficient _C_ of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the coefficient _C_ of the plane equation. |

#### `M_COEFFICIENT_D`

Inquires the coefficient _D_ of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `Value` | Specifies the coefficient _D_ of the plane equation. |

#### `M_NORMAL_X`

Inquires the X-component of the plane's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the X-component of the plane's normal unit vector, expressed in the working coordinate system. |

#### `M_NORMAL_Y`

Inquires the Y-component of the plane's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Y-component of the plane's normal unit vector, expressed in the working coordinate system. |

#### `M_NORMAL_Z`

Inquires the Z-component of the plane's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Z-component of the plane's normal unit vector, expressed in the working coordinate system. |

---

### `3D point geometry object ID`

Specifies a 3D point geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and successfully defined as a point.

#### `M_POSITION_X`

Inquires the X-coordinate of the point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the point, expressed in the working coordinate system. |

#### `M_POSITION_Y`

Inquires the Y-coordinate of the point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the point, expressed in the working coordinate system. |

#### `M_POSITION_Z`

Inquires the Z-coordinate of the point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the point, expressed in the working coordinate system. |

---

### `3D sphere geometry object ID`

Specifies a 3D sphere geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and successfully defined as a sphere.

#### `M_CENTER_X`

Inquires the X-coordinate of the sphere's center point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the sphere's center point, expressed in the working coordinate system. |

#### `M_CENTER_Y`

Inquires the Y-coordinate of the sphere's center point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the sphere's center point, expressed in the working coordinate system. |

#### `M_CENTER_Z`

Inquires the Z-coordinate of the sphere's center point.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Z-coordinate of the sphere's center point, expressed in the working coordinate system. |

#### `M_RADIUS`

Inquires the sphere's radius.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the sphere's radius in world units. |

### For inquiring about any transformation matrix object

For any transformation matrix object, the [`GeometryOrMatrix3dgeoId`](../../Reference/3dgeo/M3dgeoInquire.md) and [`InquireType`](../../Reference/3dgeo/M3dgeoInquire.md) parameters can be set to one of the following:

---

### `Transformation matrix object ID`

Specifies a transformation matrix object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

#### `M_AFFINE`

Inquires whether the transformation matrix object is an affine transformation matrix.  An affine transformation matrix has translation and/or rotation and/or scale transformations. The scale can be uniform or non-uniform.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the transformation matrix object is not an affine transformation matrix. |
| `M_TRUE` | Specifies that the transformation matrix object is an affine transformation matrix. |

#### `M_AXIS_ALIGNED`

Inquires whether the transformation matrix object is an axis-aligned transformation matrix.  An axis-aligned transformation matrix has translation and/or scale transformations.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the transformation matrix object is not an axis-aligned transformation matrix. |
| `M_TRUE` | Specifies that the transformation matrix object is an axis-aligned transformation matrix. |

#### `M_IDENTITY`

Inquires whether the transformation matrix object is an identity transformation matrix.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the transformation matrix object is not an identity matrix. |
| `M_TRUE` | Specifies that the transformation matrix object is an identity matrix. |

#### `M_MIRROR`

Inquires whether the transformation matrix object preserves the handedness of the coordinate system. If true, applying the matrix will flip the handedness of the coordinate system.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that applying the transformation matrix object will flip the handedness of the coordinate system. |
| `M_TRUE` | Specifies that applying the transformation matrix object preserves the handedness of the coordinate system. |

#### `M_RIGID`

Inquires whether the transformation matrix object is a rigid transformation matrix.  A rigid transformation matrix has translation and/or rotation transformations.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the transformation matrix object is not a rigid transformation matrix. |
| `M_TRUE` | Specifies that the transformation matrix object is a rigid transformation matrix. |

#### `M_ROTATION`

Inquires whether the transformation matrix object is a rotation transformation matrix.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the transformation matrix object is not a rotation transformation matrix. |
| `M_TRUE` | Specifies that the transformation matrix object is a rotation transformation matrix. |

#### `M_SCALE`

Inquires whether the transformation matrix object is a scale transformation matrix. The scale can be uniform or non-uniform.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the transformation matrix object is not a scale transformation matrix. |
| `M_TRUE` | Specifies that the transformation matrix object is a scale transformation matrix. |

#### `M_SCALE_UNIFORM`

Inquires whether the transformation matrix object is a uniform scale transformation matrix.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the transformation matrix object is not a uniform scale transformation matrix. |
| `M_TRUE` | Specifies that the transformation matrix object is a uniform scale transformation matrix. |

#### `M_SIMILARITY`

Inquires whether the transformation matrix object is a similarity transformation matrix.  A similarity transformation matrix has translation and/or rotation and/or uniform scale transformations.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the transformation matrix object is not a similarity transformation matrix. |
| `M_TRUE` | Specifies that the transformation matrix object is a similarity transformation matrix. |

#### `M_TRANSFORMATION_TYPE`

Inquires the transformation matrix object's transformation type.  > **Note:** Note that this inquire type will return the strictest type that matches the transformation matrix object. For example, if the transformation matrix object is a rotation transformation matrix, it will return true for [`M_ROTATION`](../../Reference/3dgeo/M3dgeoInquire.md), [`M_RIGID`](../../Reference/3dgeo/M3dgeoInquire.md), [`M_SIMILARITY`](../../Reference/3dgeo/M3dgeoInquire.md), and [`M_AFFINE`](../../Reference/3dgeo/M3dgeoInquire.md), but [`M_TRANSFORMATION_TYPE`](../../Reference/3dgeo/M3dgeoInquire.md) will return [`M_ROTATION`](../../Reference/3dgeo/M3dgeoInquire.md).*[Image: 3dgeo_matrix_types.png]*

| Value | Description |
| --- | --- |
| `M_AFFINE` | Specifies an affine transformation matrix. This is returned if the transformation matrix object has a non-uniform scale transformation, and at least one other type of transformation, including translation or rotation. |
| `M_AXIS_ALIGNED` | Specifies an axis-aligned transformation matrix. This is returned if the transformation matrix object has translation and scale transformations. |
| `M_IDENTITY` | Specifies an identity transformation matrix. |
| `M_PROJECTION` | Specifies a projection transformation matrix. |
| `M_RIGID` | Specifies a rigid transformation matrix. This is returned if the transformation matrix object has rotation and translation transformations. |
| `M_ROTATION` | Specifies a rotation transformation matrix. |
| `M_SCALE` | Specifies a scale transformation matrix. This is returned if the transformation matrix object has a non-uniform transformation. |
| `M_SCALE_UNIFORM` | Specifies a uniform scale transformation matrix. |
| `M_SIMILARITY` | Specifies a similarity transformation matrix. This is returned if the transformation matrix object has a uniform scale transformation, and at least one other type of transformation, including translation or rotation. |
| `M_TRANSLATION` | Specifies a translation transformation matrix. |

#### `M_TRANSLATION`

Inquires whether the transformation matrix object is a translation transformation matrix.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the transformation matrix object is not a translation transformation matrix. |
| `M_TRUE` | Specifies that the transformation matrix object is a translation transformation matrix. |

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the requested information, cast to an _AIL_DOUBLE_. If the requested information does not fit into an _AIL_DOUBLE_, this function will return `M_NULL`or truncate the information.
