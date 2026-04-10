---
doctype: Reference
module: 3dmet
function: M3dmetFeature
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetFeature"
---

# M3dmetFeature

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

> Calculate scalar measurements from 3D geometries or transformation matrices.

## Syntax

```c
AIL_DOUBLE M3dmetFeature(
    AIL_ID       Src1GeometryOrMatrix3dgeoId,  //in
    AIL_ID       Src2GeometryOrMatrix3dgeoId,  //in
    AIL_INT64    Operation,                    //in
    AIL_DOUBLE   Param,                        //in
    AIL_DOUBLE * ResultPtr                     //out
)
```

## Description

This function calculates scalar measurements from source 3D geometry objects or transformation matrix objects.

## Parameters

### `Src1GeometryOrMatrix3dgeoId` *(in, AIL_ID)*

Specifies the first source 3D geometry object or transformation matrix object.

*For specifying the first source 3D geometry or transformation matrix*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |
| `Transformation matrix object identifier` | Specifies the identifier of a transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

### `Src2GeometryOrMatrix3dgeoId` *(in, AIL_ID)*

Specifies the second source 3D geometry object or transformation matrix object.

*For specifying the second source 3D geometry*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |
| `Transformation matrix object identifier` | Specifies the identifier of a transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

### `Param` *(in, AIL_DOUBLE)*

Specifies a scalar value for certain operations, where required. Set this parameter to `M_DEFAULT` if not used.

### `ResultPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the results. Since the [`M3dmetFeature`](../../Reference/3dmet/M3dmetFeature.md) function also returns the calculated value, you can set this parameter to `M_NULL`.

## Parameter Associations

### For specifying the type of calculation to perform

---

### `M_ANGLE`

Specifies to calculate the angle between source 3D geometries.

#### `3D cylinder geometry object identifier`

Specifies the identifier of a 3D cylinder geometry object, for finding its angle with another 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder.*[Image: 3dmet_angle_cylinder.png]*

#### `3D line geometry object identifier`

Specifies the identifier of the 3D line geometry object, for finding its angle with another 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.*[Image: 3dmet_angle_line.png]*

#### `3D plane geometry object identifier`

Specifies the identifier of the 3D plane geometry object, for finding its angle with another 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane.*[Image: 3dmet_angle_plane.png]*

#### `Transformation matrix object identifier`

Specifies the identifier of the transformation matrix, for finding its angle with another transformation matrix. The transformation matrix must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). You will obtain the angle between the two rotation matrices.  > **Note:** Note that the two matrices must be similarity transformations, and must have the same handedness. That is, if you call [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_SIMILARITY`](../../Reference/3dgeo/M3dgeoInquire.md), it must return [`M_TRUE`](../../Reference/3dgeo/M3dgeoInquire.md), and if you call [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_MIRROR`](../../Reference/3dgeo/M3dgeoInquire.md), it must return the same value for both matrices.

---

### `M_CLIP`

Specifies to calculate if a 3D line geometry object is inside, outside, or partially inside a second 3D geometry. Note that, if the second geometry is a plane, inside is the same side as the plane's normal vector. To inquire this vector, use [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_NORMAL_...`](../../Reference/3dgeo/M3dgeoInquire.md).

#### `3D geometry object identifier`

Specifies the identifier of a 3D line geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the length of the 3D line. |
| *(see [`M_IS_INSIDE`](Reference/3dmet/M3dmetGetResult.md))* |  |

---

### `M_CLOSEST_POINT`

Specifies to calculate the distance between the first specified 3D geometry (which must be a point) and the closest point on the second specified geometry.*[Image: 3dmet_closest_point_distance.png]*

#### `3D point geometry object identifier`

Specifies the identifier of a 3D point geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point.

---

### `M_DISTANCE`

Specifies to calculate the distance between two 3D points, the origins of two transformation matrices, or the origin of a transformation matrix and a 3D point.

#### `3D point geometry object identifier - distance between points`

Specifies the identifier of a 3D point geometry object, for finding the distance between it and another point. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point. *[Image: 3dmet_distance_points.png]*

#### `Transformation matrix object identifier - distance between coordinate system origin and point`

Specifies the identifier of a transformation matrix object that defines a coordinate system, for finding the distance between its origin and a point. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).*[Image: 3dmet_distance_origin_point.png]*

#### `Transformation matrix object identifier - distance between coordinate system origins`

Specifies the identifier of a transformation matrix object that defines a coordinate system, for finding the distance between its origin and the origin of another coordinate system. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).*[Image: 3dmet_distance_origins.png]*

---

### `M_EXTRUSION_BORDER`

Specifies to calculate whether extruding a box face to a plane would succeed, if extruding until a corner of the closest face meets the plane. This operation evaluates the specified 3D box and plane geometries to determine if the box face that is most parallel and closest to the plane can be extruded until the face's closest corner meets the plane.  > **Note:** If the most parallel face and its opposite both intersect the plane, there is no meaningful way to extrude the box. In this case, the operation returns [`M_FAIL`](../../Reference/3dmet/M3dmetFeature.md).

#### `3D box geometry object identifier`

Specifies the identifier of a 3D box geometry object, for determining whether it can be extruded to a plane. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies that the extrusion operation would not complete successfully. |
| `M_SUCCESS` | Specifies that the extrusion operation would complete successfully. |

---

### `M_EXTRUSION_CENTER`

Specifies to calculate whether extruding a box face to a plane would succeed, if extruding until the center of the closest face meets the plane. This operation evaluates the specified 3D box and plane geometries to determine if the box face that is most parallel and closest to the plane can be extruded until the center of the face touches the plane.

#### `3D box geometry object identifier`

Specifies the identifier of a 3D box geometry object, for determining whether it can be extruded to a plane. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies that the extrusion operation would not complete successfully. |
| `M_SUCCESS` | Specifies that the extrusion operation would complete successfully. |

---

### `M_FARTHEST_POINT`

Specifies to calculate the distance between the first specified 3D geometry (which must be a point) and the farthest point on the second specified geometry. *[Image: 3dmet_farthest_point_distance.png]*

#### `3D point geometry object identifier`

Specifies the identifier of a 3D point geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point.

---

### `M_INTERSECTION`

Specifies to calculate the number of intersections between source 3D geometries; the value returned is the number of points or lines that constitute the intersection(s).

#### `3D line geometry object identifier - intersection points`

Specifies the identifier of the 3D line geometry object, for finding its intersection with a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line. You will obtain the number of intersection points, either 0, 1, or 2.*[Image: 3dmet_intersection_points_scalar.png]*  > **Note:** For an intersection operation that treats the source 3D line as a ray (extending infinitely in one direction), use [`M_RAY_CAST`](../../Reference/3dmet/M3dmetFeature.md).

#### `3D plane geometry object identifier - intersection line`

Specifies the identifier of a 3D plane geometry object, for finding its intersection with a second plane. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane. You will obtain the number of intersection lines, either 0 or 1. *[Image: 3dmet_intersection_line_scalar.png]*

#### `3D plane geometry object identifier - intersection polygon`

Specifies the identifier of the 3D plane geometry object, for finding its intersection with a box. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane. You will obtain the number of intersection lines, either 0, 3, 4, 5, or 6.  *[Image: 3dmet_intersection_polygon_scalar.png]*

---

### `M_IS_INSIDE`

Specifies to calculate if a first source 3D geometry is inside, outside, or partially inside a second 3D geometry. For the two source 3D geometries in the following image, the function returns [`M_PARTIALLY_INSIDE`](../../Reference/3dmet/M3dmetGetResult.md). *[Image: 3dmet_is_inside.png]*

#### `3D geometry object identifier`

Specifies the identifier of the first 3D geometry object, for calculating if it is inside, outside, or partially inside the second 3D geometry object. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box, cylinder, line, point, or sphere.

| Value | Description |
| --- | --- |
| *(see [`M_IS_INSIDE`](Reference/3dmet/M3dmetGetResult.md))* |  |

---

### `M_OVERLAP`

Specifies to calculate the percentage of volume overlap between source 3D geometries. The percentage is calculated according to the following formula, where _V<sub>intersection</sub> _ is the overlapping volume and _V<sub>union</sub> _ is the combined 3D geometry volume: `(V<sub>intersection</sub> / V<sub>union</sub>) * 100`  Set the resolution of the calculation with [`M_OVERLAP`](../../Reference/3dmet/M3dmetFeature.md), which sets the smallest cubic unit with which to calculate the volume.  > **Note:** Note that infinite cylinders are not supported.

#### `3D box geometry object identifier`

Specifies the identifier of a 3D box geometry object, for finding the percentage of volume overlap between the box and another 3D geometry. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.  *[Image: 3dmet_overlap.png]*

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the resolution, in world units. |

#### `3D cylinder geometry object identifier`

Specifies the identifier of a 3D cylinder geometry object, for finding the percentage of volume overlap between the cylinder and another 3D geometry. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder.  > **Note:** Note that infinite cylinders are not supported for this operation.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the resolution, in world units. |

#### `3D sphere geometry object identifier`

Specifies the identifier of a 3D sphere geometry object, for finding the percentage of volume overlap between the sphere and another 3D geometry. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a sphere.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the resolution, in world units. |

---

### `M_PARALLELISM`

Specifies to calculate the angular deviation from a parallel orientation for source 3D geometries.  For a 3D cylinder, the angle is measured from the cylinder's central axis. For a 3D plane, the angle is measured from the plane's normal vector.

#### `3D cylinder geometry object identifier`

Specifies the identifier of a 3D cylinder geometry object, for finding the degree to which it is parallel to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the cylinder is parallel to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeature.md)).  *[Image: 3dmet_paralellism.png]*

#### `3D line geometry object identifier`

Specifies the identifier of a 3D line geometry object, for finding the degree to which it is parallel to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the line is parallel to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeature.md)).

#### `3D plane geometry object identifier`

Specifies the identifier of a 3D plane geometry object, for finding the degree to which it is parallel to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the plane is parallel to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeature.md)).

---

### `M_PERPENDICULARITY`

Specifies to calculate the angular deviation from a perpendicular orientation for source 3D geometries.  For a 3D cylinder, the angle is measured from the cylinder's central axis. For a 3D plane, the angle is measured from the plane's normal vector.

#### `3D cylinder geometry object identifier`

Specifies the identifier of a 3D cylinder geometry object, for finding the degree to which it is perpendicular to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the cylinder is perpendicular to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeature.md)).  *[Image: 3dmet_perpendicularity.png]*

#### `3D line geometry object identifier`

Specifies the identifier of a 3D line geometry object, for finding the degree to which it is perpendicular to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the line is perpendicular to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeature.md)).

#### `3D plane geometry object identifier`

Specifies the identifier of a 3D plane geometry object, for finding the degree to which it is perpendicular to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the plane is perpendicular to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeature.md)).

---

### `M_RAY_CAST`

Specifies to calculate the number of intersection points between a ray and a second source 3D geometry, where a ray is a line that extends infinitely in one direction from the line's start point.*[Image: 3dmet_ray_cast_scalar.png]*

#### `3D line geometry object identifier`

Specifies the identifier of the 3D line geometry object that defines the ray, for finding its intersection with a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line. You will obtain the number of intersection points, either 0, 1, or 2.

---

### `M_SHORTEST_LINE`

Specifies to calculate the length of the shortest line between two source 3D line geometries.*[Image: 3dmet_shortest_line.png]*

#### `3D line geometry object identifier`

Specifies the identifier of the first 3D line geometry object. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the requested information, cast to an _AIL_DOUBLE_. If the requested information does not fit into an _AIL_DOUBLE_, this function will return `M_NULL` or truncate the information.
