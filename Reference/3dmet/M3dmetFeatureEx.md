---
doctype: Reference
module: 3dmet
function: M3dmetFeatureEx
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetFeatureEx"
---

# M3dmetFeatureEx

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

> Calculates geometric and scalar measurements from 3D geometries or transformation matrices.

## Syntax

```c
void M3dmetFeatureEx(
    AIL_ID     Feature3dmetContextId,        //in
    AIL_ID     Src1GeometryOrMatrix3dgeoId,  //in
    AIL_ID     Src2GeometryOrMatrix3dgeoId,  //in
    AIL_ID     Src3Geometry3dgeoId,          //in
    AIL_ID     Result3dmetOr3dgeoId,         //out
    AIL_INT64  Operation,                    //in
    AIL_DOUBLE Param,                        //in
    AIL_INT64  ControlFlag                   //in
)
```

## Description

This function calculates geometric and scalar measurements from source 3D geometry or transformation matrix objects. To more easily compute certain scalar measurements, use [`M3dmetFeature`](../../Reference/3dmet/M3dmetFeature.md).

## Parameters

### `Feature3dmetContextId` *(in, AIL_ID)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Src1GeometryOrMatrix3dgeoId` *(in, AIL_ID)*

Specifies the first source 3D geometry or transformation matrix object.

*For specifying the first source 3D geometry or transformation matrix object*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |
| `Transformation matrix object identifier` | Specifies the identifier of a transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

### `Src2GeometryOrMatrix3dgeoId` *(in, AIL_ID)*

Specifies the second source 3D geometry or transformation matrix object.

*For specifying the second source 3D geometry or transformation matrix object*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |
| `Transformation matrix object identifier` | Specifies the identifier of a transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

### `Src3Geometry3dgeoId` *(in, AIL_ID)*

Specifies the third source 3D geometry object.

*For specifying the third source 3D geometry*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |

### `Result3dmetOr3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the calculate 3D metrology result buffer in which to store the results of the calculate operation, or specifies the 3D geometry object in which to define a resulting geometry.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

### `Param` *(in, AIL_DOUBLE)*

Specifies a scalar value for certain operations, where required. Set this parameter to `M_DEFAULT` if not used.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the type of calculation to perform

---

### `M_ANGLE`

Specifies to calculate the angle between source 3D geometries.  You can retrieve results for this operation using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_ANGLE`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D cylinder geometry object identifier`

Specifies the identifier of a 3D cylinder geometry object, for finding its angle with another 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder.*[Image: 3dmet_angle_cylinder.png]*

#### `3D line geometry object identifier`

Specifies the identifier of the 3D line geometry object, for finding its angle with another 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.  *[Image: 3dmet_angle_line.png]*

#### `3D plane geometry object identifier`

Specifies the identifier of the 3D plane geometry object, for finding its angle with another 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane.*[Image: 3dmet_angle_plane.png]*

#### `Transformation matrix object identifier`

Specifies the identifier of the transformation matrix, for finding its angle with another transformation matrix. The transformation matrix must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). You will obtain the angle of the axis-angle rotation between the matrices. This is the angle through which an object with the Src1 rotation must be rotated to align with an object with the Src2 rotation.  > **Note:** Note that the two matrices must be similarity transformations, and must have the same handedness. That is, if you call [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_SIMILARITY`](../../Reference/3dgeo/M3dgeoInquire.md), it must return [`M_TRUE`](../../Reference/3dgeo/M3dgeoInquire.md), and if you call [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_MIRROR`](../../Reference/3dgeo/M3dgeoInquire.md), it must return the same value for both matrices.

---

### `M_BOUNDING_BOX`

Specifies to calculate the axis-aligned bounding box of source 3D geometries. *[Image: 3dmet_bounding_box.png]*  You can specify geometries of type [`M_NOT_INITIALIZED`](../../Reference/3dgeo/M3dgeoInquire.md) for [`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md). If not initialized, this geometry has no effect on the result. This allows you to specify the same geometry for [`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md) and [`Result3dmetOr3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md) without previously initializing the result 3D geometry object, which is useful when using a loop to compute the bounding box of many geometries.  > **Note:** Note that infinite geometries are not supported.

#### `3D geometry object identifier`

Specifies the identifier of a 3D geometry object, for finding its axis-aligned bounding box, or the combined axis-aligned bounding box with a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box, finite cylinder, finite line, point, or sphere.

---

### `M_CLIP`

Specifies to truncate a 3D line geometry object such that it lies inside a second 3D geometry. Note that, if the second geometry is a plane, inside is the same side as the plane's normal vector. To inquire this vector, use [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_NORMAL_...`](../../Reference/3dgeo/M3dgeoInquire.md). *[Image: 3dmet_clip_line.png]*  You can retrieve results for this operation using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_IS_INSIDE`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D geometry object identifier`

Specifies the identifier of a 3D line geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line. If the line is inside the second specified 3D geometry object ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)), you will obtain a clipped line; you will obtain a geometry of type [`M_NOT_INITIALIZED`](../../Reference/3dgeo/M3dgeoInquire.md) if the line does not fall inside the second 3D geometry.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the length of the clipped line. |

---

### `M_CLOSEST_POINT`

Specifies to calculate the point on the second specified 3D geometry that is closest to the first specified 3D geometry, which must be a point. This operation also calculates the distance between the points.*[Image: 3dmet_closest_point.png]*  To retrieve the calculated distance, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D point geometry object identifier`

Specifies the identifier of a 3D point geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point. You will obtain the point on the second 3D geometry that is closest to this point.

---

### `M_DISTANCE`

Specifies to calculate the distance between source 3D geometries.  You can retrieve results for this operation using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D point geometry object identifier - distance between points`

Specifies the identifier of the 3D point geometry object, for finding the distance between it and another point. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point. *[Image: 3dmet_distance_points.png]*

#### `Transformation matrix object identifier - distance between coordinate system origin and point`

Specifies the identifier of a transformation matrix object that defines a coordinate system, for finding the distance between its origin and a point. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).*[Image: 3dmet_distance_origin_point.png]*

#### `Transformation matrix object identifier - distance between coordinate system origins`

Specifies the identifier of a transformation matrix object that defines a coordinate system, for finding the distance between its origin and the origin of another coordinate system. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).*[Image: 3dmet_distance_origins.png]*

---

### `M_EXTRUSION_BORDER`

Specifies to lengthen or shorten a 3D box geometry relative to a specified plane; Aurora Imaging Library extrudes the box face that is most parallel and closest to the plane until the face's closest corner meets the plane.  *[Image: 3dmet_extrusion_border.png]*  You can retrieve whether this operation was successful using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_STATUS_EXTRUSION`](../../Reference/3dmet/M3dmetGetResult.md).  > **Note:** If the most parallel face and its opposite both intersect the plane, there is no meaningful way to extrude the box. In this case, the specified 3D box geometry is copied directly into the result object, and [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_STATUS_EXTRUSION`](../../Reference/3dmet/M3dmetGetResult.md) returns [`M_FAIL`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D box geometry object identifier`

Specifies the identifier of a 3D box geometry object, for extruding it to a plane 3D geometry. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

---

### `M_EXTRUSION_CENTER`

Specifies to lengthen or shorten a 3D box geometry relative to a specified plane; Aurora Imaging Library extrudes the box face that is most parallel and closest to the plane until the center of the face touches the plane.  *[Image: 3dmet_extrusion_center.png]*  You can retrieve whether this operation was successful using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_STATUS_EXTRUSION`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D box geometry object identifier`

Specifies the identifier of a 3D box geometry object, for extruding it to a plane 3D geometry. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

---

### `M_FARTHEST_POINT`

Specifies to calculate the point on the second specified 3D geometry that is farthest from the first specified 3D geometry, which must be a point. This operation also calculates the distance between the points.*[Image: 3dmet_farthest_point.png]*  To retrieve the calculated distance, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D point geometry object identifier`

Specifies the identifier of a 3D point geometry object, for finding the point on the second specified 3D geometry that is farthest from the first source point. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point. You will obtain the point on the second source geometry object that is the farthest distance from this point.

---

### `M_INTERPOLATION`

Specifies to calculate the interpolation between source 3D geometries or matrices. Both sources must be the same type of 3D geometry, or they must both be transformation matrix objects.  *[Image: 3dmet_interpolate.png]*  You must specify an interpolation factor ([`M_INTERPOLATION`](../../Reference/3dmet/M3dmetFeatureEx.md)), where 0.0 maps to the first source 3D geometry, and 1.0 maps to the second source 3D geometry. Values between 0.0 and 1.0 are interpolated, and values outside of this range are extrapolated. If extrapolation would result in a negative scale, the scale is set to 0.  This calculation is a linear interpolation between source objects. In general, given Src1, Src2, and an interpolation factor, the resulting object's size will be: Factor*Src1 + (1-Factor)*Src2.

#### `3D geometry object identifier - interpolate between geometries`

Specifies the identifier of the first of two 3D geometry objects of the same type, for finding the interpolation. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined successfully.  > **Note:** Note that, for 3D lines and cylinders, both source geometries must be finite or infinite.

#### `Transformation object identifier - interpolate between transformation matrices`

Specifies the identifier of the first of two transformation matrix objects, for finding the interpolation. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a similarity transformation.

---

### `M_INTERSECTION`

Specifies to calculate the intersection line, points, or polygon between source 3D geometries.  You can retrieve the number of resulting 3D geometries for this operation using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_NUMBER`](../../Reference/3dmet/M3dmetGetResult.md).  If all source 3D geometries are planes, you can retrieve the parallelism between the planes, using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_ANGLE_TO_EDGE_CASE`](../../Reference/3dmet/M3dmetGetResult.md); you can also retrieve the parallelism between a source line and plane. Note that, for three source planes,[`M_ANGLE_TO_EDGE_CASE`](../../Reference/3dmet/M3dmetGetResult.md) returns the parallelism of the two planes that are most parallel, where 0 means completely parallel.

#### `3D line geometry object identifier - intersection points`

Specifies the identifier of the 3D line geometry object, for finding its intersection with a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line. If the line intersects the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)), you can obtain 1 or 2 intersection points; you will obtain only 1 point if the second geometry is a plane or if the line is finite and does not cross the second geometry entirely. If the line does not intersect the second specified 3D geometry, you will obtain a geometry of type [`M_NOT_INITIALIZED`](../../Reference/3dgeo/M3dgeoInquire.md).*[Image: 3dmet_intersection_points.png]*  > **Note:** For an intersection operation that treats the source 3D line as a ray (extending infinitely in one direction), use [`M_RAY_CAST`](../../Reference/3dmet/M3dmetFeatureEx.md).

#### `3D plane geometry object identifier - intersection line`

Specifies the identifier of a 3D plane geometry object, for finding its intersection with a second plane. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane. If the plane intersects the second specified plane ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)), you will obtain 1 infinite intersection line.*[Image: 3dmet_intersection_line.png]*

#### `3D plane geometry object identifier - intersection point`

Specifies the identifier of a 3D plane geometry object, for finding its intersection with a second and third plane. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane. If the plane intersects the second and third specified planes ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md) and [`Src3Geometry3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md), respectively), you will obtain 1 intersection point. If the plane does not intersect the second and third specified planes, you will obtain a geometry of type [`M_NOT_INITIALIZED`](../../Reference/3dgeo/M3dgeoInquire.md).*[Image: 3dmet_intersection_point.png]*

#### `3D plane geometry object identifier - intersection polygon`

Specifies the identifier of the 3D plane geometry object, for finding its intersection with a 3D box geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane. If the plane intersects the specified box ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)), you will obtain a polygon consisting of 3 to 6 finite intersection lines.  *[Image: 3dmet_intersection_polygon.png]*

---

### `M_IS_INSIDE`

Specifies to calculate if a first source 3D geometry is inside, outside, or partially inside a second 3D geometry. For the two source 3D geometries in the following image, the calculated result is [`M_PARTIALLY_INSIDE`](../../Reference/3dmet/M3dmetGetResult.md).*[Image: 3dmet_is_inside.png]*  You can retrieve results for this operation using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_IS_INSIDE`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D geometry object identifier`

Specifies the identifier of the first 3D geometry object, for calculating if it is inside, outside, or partially inside the second 3D geometry object. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box, cylinder, line, point, or sphere.

---

### `M_NORMAL_AT_POSITION`

Specifies to calculate a unit normal line (one unit in length) on a source 3D geometry closest to a given point.  *[Image: 3dmet_normal_at_position.png]*

#### `3D point geometry object identifier`

Specifies the identifier of a 3D point geometry object, for calculating a unit line along an outward facing normal direction that is in line with the point, with a start point at the surface of a second specified 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point. Note that the point can be inside or outside the second 3D geometry; the calculated unit line is always directed outwards from the second geometry's surface.

---

### `M_ORTHOGONALIZE`

Specifies to rotate the specified source 3D geometry such that it is arranged orthogonally with another 3D geometry.  You can specify a point ([`M_ORTHOGONALIZE`](../../Reference/3dmet/M3dmetFeatureEx.md)) about which to perform the rotation. If you do not specify a point, the rotation is performed around the first specified 3D geometry's start point, or, if the first 3D geometry is a plane, around the origin.

#### `3D cylinder geometry object identifier`

Specifies the identifier of a 3D cylinder geometry object, for rotation to an orthogonal position with respect to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder.  *[Image: 3dmet_orthogonalize_no_point_plane.png]*

#### `3D line geometry object identifier`

Specifies the identifier of a 3D line geometry object, for rotation to an orthogonal position with respect to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder.

#### `3D plane geometry object identifier`

Specifies the identifier of a 3D plane geometry object, for rotation to an orthogonal position with respect to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane.

---

### `M_OVERLAP`

Specifies to calculate the percentage of volume overlap between source 3D geometries. The percentage is calculated according to the following formula, where _V<sub>intersection</sub> _ is the overlapping volume and _V<sub>union</sub> _ is the combined 3D geometry volume: `(V<sub>intersection</sub> / V<sub>union</sub>) * 100`  Set the resolution of the calculation with [`M_OVERLAP`](../../Reference/3dmet/M3dmetFeatureEx.md), which sets the smallest cubic unit with which to calculate the volume.  > **Note:** Note that infinite cylinders are not supported.  You can retrieve results for this operation using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_OVERLAP`](../../Reference/3dmet/M3dmetGetResult.md).  You can also use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) to retrieve the volume of the union of the two source 3D geometries ([`M_UNION`](../../Reference/3dmet/M3dmetGetResult.md)), the volume of their intersection ([`M_INTERSECTION`](../../Reference/3dmet/M3dmetGetResult.md)), and the difference between the union and intersection ([`M_UNION`](../../Reference/3dmet/M3dmetGetResult.md) - [`M_INTERSECTION`](../../Reference/3dmet/M3dmetGetResult.md)) of the two source 3D geometries ([`M_DIFFERENCE`](../../Reference/3dmet/M3dmetGetResult.md)).

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

Specifies to calculate the angular deviation from a parallel orientation for source 3D geometries.  For a 3D cylinder, the angle is measured from the cylinder's central axis. For a 3D plane, the angle is measured from the plane's normal vector.  You can retrieve results for this operation using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_PARALLELISM`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D cylinder geometry object identifier`

Specifies the identifier of a 3D cylinder geometry object, for finding the degree to which it is parallel to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the cylinder is parallel to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)).  *[Image: 3dmet_paralellism.png]*

#### `3D line geometry object identifier`

Specifies the identifier of a 3D line geometry object, for finding the degree to which it is parallel to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the line is parallel to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)).

#### `3D plane geometry object identifier`

Specifies the identifier of a 3D plane geometry object, for finding the degree to which it is parallel to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the plane is parallel to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)).

---

### `M_PERPENDICULARITY`

Specifies to calculate the angular deviation from a perpendicular orientation for source 3D geometries.  For a 3D cylinder, the angle is measured from the cylinder's central axis. For a 3D plane, the angle is measured from the plane's normal vector.  You can retrieve results for this operation using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_PERPENDICULARITY`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D cylinder geometry object identifier`

Specifies the identifier of a 3D cylinder geometry object, for finding the degree to which it is perpendicular to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the cylinder is perpendicular to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)).  *[Image: 3dmet_perpendicularity.png]*

#### `3D line geometry object identifier`

Specifies the identifier of a 3D line geometry object, for finding the degree to which it is perpendicular to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the line is perpendicular to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)).

#### `3D plane geometry object identifier`

Specifies the identifier of a 3D plane geometry object, for finding the degree to which it is perpendicular to a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane. You will obtain an angle from 0 to 90 degrees, where 0 indicates that the plane is perpendicular to the second specified 3D geometry ([`Src2GeometryOrMatrix3dgeoId`](../../Reference/3dmet/M3dmetFeatureEx.md)).

---

### `M_POINT_ON_LINE`

Specifies to calculate a 3D point geometry on the source 3D line geometry at a specified distance from the line's start point.*[Image: 3dmet_point_on_line.png]*

#### `3D line geometry object identifier`

Specifies the identifier of a 3D line geometry object, on which to place the calculated 3D point. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.

---

### `M_POINT_ON_LINE_CLIPPED`

Specifies to calculate a 3D point geometry on the source 3D line geometry at a specified distance from the line's start point. If the specified distance would place the point outside the limits of a finite line, the point is clipped onto the line's end point or start point, whichever is closer.*[Image: 3dmet_point_on_line_clipped.png]*

#### `3D line geometry object identifier`

Specifies the identifier of a 3D line geometry object, on which to calculate the 3D point. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.

---

### `M_PROJECTION`

Specifies to calculate a point or line 3D geometry from a projection of a source point or line onto the surface of a second source 3D geometry. You can project a line onto a 3D plane only.

#### `3D line geometry object identifier - project line`

Specifies the identifier of a 3D line geometry object, for projecting onto a 3D plane geometry. The 3D line geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.  *[Image: 3dmet_projection_line.png]*

#### `3D point geometry object identifier - project point`

Specifies the identifier of a 3D point geometry object, for projecting onto a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point. *[Image: 3dmet_projection_point.png]*

---

### `M_RAY_CAST`

Specifies to calculate the intersection point(s) between a ray and a second source 3D geometry, where a ray is a line that extends infinitely in one direction from the line's start point.*[Image: 3dmet_ray_cast.png]*  You can retrieve results for this operation using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_NUMBER`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D line geometry object identifier`

Specifies the identifier of the 3D line geometry object that defines the ray, for finding its intersection point(s) with a second 3D geometry. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.

---

### `M_SHORTEST_LINE`

Specifies to calculate the shortest line between two source 3D line geometries. This operation also calculates the length of the resulting line.*[Image: 3dmet_shortest_line.png]*  To retrieve the resulting line's length, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md).

#### `3D line geometry object identifier`

Specifies the identifier of the first 3D line geometry object. The 3D geometry must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.
