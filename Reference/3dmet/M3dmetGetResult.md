---
doctype: Reference
module: 3dmet
function: M3dmetGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetGetResult"
---

# M3dmetGetResult

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

> Get the specified type of result(s) from a 3D metrology result buffer.

## Syntax

```c
AIL_DOUBLE M3dmetGetResult(
    AIL_ID    Result3dmetId,  //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the result(s) of the specified type from a 3D metrology result buffer. For calculate 3D metrology result buffers, results are available after calling[`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md), [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md), or [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md). For fit 3D metrology result buffers, results are available after calling [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md). For statistics 3D metrology result buffers, results are available after calling [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md).

## Parameters

### `Result3dmetId` *(in, AIL_ID)*

Specifies the identifier of a calculate 3D metrology result buffer, fit 3D metrology result buffer, or statistics 3D metrology result buffer, from which to retrieve results.

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address in which to write the results. Since the [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) function also returns the results, you can set this parameter to `M_NULL`.

## Parameter Associations

### For specifying the type of result to retrieve from a calculate, fit, or statistics 3D metrology result buffer

The following [`Result3dmetId`](../../Reference/3dmet/M3dmetGetResult.md), [`ResultType`](../../Reference/3dmet/M3dmetGetResult.md), and [`ResultArrayPtr`](../../Reference/3dmet/M3dmetGetResult.md) parameter settings can be specified for different types of 3D metrology result buffers.

---

### `Calculate 3D metrology result buffer ID, for retrieving results of an M3dmetDistanceEx operation`

Specifies the identifier of a calculate 3D metrology result buffer that holds results of an [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) operation.

#### `M_RESULT_IMAGE_SIZE_X`

Retrieves the size in X of the source point cloud's range component or of the source depth map, in pixels. This is the same size in X as the source depth map or as the range of the source container used in the [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) operation.  Note, [`M_STATUS_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md) must not be [`M_NOT_INITIALIZED`](../../Reference/3dmet/M3dmetGetResult.md).

#### `M_RESULT_IMAGE_SIZE_Y`

Retrieves the size in Y of the source point cloud's range component or of the source depth map, in pixels. This is the same size in Y as the source depth map or as the range of the source container used in the [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) operation.  Note, [`M_STATUS_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md) must not be [`M_NOT_INITIALIZED`](../../Reference/3dmet/M3dmetGetResult.md).

#### `M_STATUS_DISTANCE`

Retrieves the global status of the last [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md).

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) was called and the operation has been completed successfully. |
| `M_NOT_INITIALIZED` | Specifies that the distance 3D metrology result buffer was not use in a call to [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) and contains no results. |

---

### `Calculate 3D metrology result buffer ID, for retrieving results of an M3dmetFeatureEx operation`

Specifies the identifier of a calculate 3D metrology result buffer that holds results of an [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) operation.

#### `?`

#### `M_ANGLE`

Retrieves the angle between source geometries.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_ANGLE`](../../Reference/3dmet/M3dmetFeatureEx.md).

#### `M_ANGLE_TO_EDGE_CASE`

Retrieves the angle of the deviation from a degenerate case.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_ORTHOGONALIZE`](../../Reference/3dmet/M3dmetFeatureEx.md), [`M_PROJECTION`](../../Reference/3dmet/M3dmetFeatureEx.md), [`M_RAY_CAST`](../../Reference/3dmet/M3dmetFeatureEx.md), or [`M_INTERSECTION`](../../Reference/3dmet/M3dmetFeatureEx.md) when not calculating the intersection points between a line and a geometry.

#### `M_CALCULATED_OBJECT_TYPE`

Retrieves the type of object contained in the specified calculate 3D metrology result buffer.  This result is always available.

| Value | Description |
| --- | --- |
| `M_GEOMETRY` | Specifies that a 3D geometry object is contained within the specified calculate 3D metrology result buffer. |
| `M_NONE` | Specifies that there is no calculated object contained within the specified calculate 3D metrology result buffer. |
| `M_TRANSFORMATION_MATRIX` | Specifies that a transformation matrix object is contained within the specified calculate 3D metrology result buffer. |

#### `M_DIFFERENCE`

Retrieves the difference between the union and the intersection of two source geometry objects.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_OVERLAP`](../../Reference/3dmet/M3dmetFeatureEx.md).

#### `M_DISTANCE`

Retrieves the distance calculated during the [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) operation.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_DISTANCE`](../../Reference/3dmet/M3dmetFeatureEx.md), [`M_SHORTEST_LINE`](../../Reference/3dmet/M3dmetFeatureEx.md), [`M_CLOSEST_POINT`](../../Reference/3dmet/M3dmetFeatureEx.md), or [`M_FARTHEST_POINT`](../../Reference/3dmet/M3dmetFeatureEx.md).  > **Note:** Note that [`M_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md) can return either the shortest or the longest distance, depending on the operation used.

#### `M_GEOMETRY_TYPE`

Retrieves the type of geometry contained in the specified calculate 3D metrology result buffer.  This result is available when [`M_CALCULATED_OBJECT_TYPE`](../../Reference/3dmet/M3dmetGetResult.md) returns [`M_GEOMETRY`](../../Reference/3dmet/M3dmetGetResult.md).

| Value | Description |
| --- | --- |
| *(see [`M_GEOMETRY_TYPE`](Reference/3dgeo/M3dgeoInquire.md))* |  |
| `M_NONE` | Specifies that there is no geometry contained within the specified calculate 3D metrology result buffer. |

#### `M_INTERSECTION`

Retrieves the volume of the intersection between two geometries.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_OVERLAP`](../../Reference/3dmet/M3dmetFeatureEx.md).

#### `M_IS_INSIDE`

Retrieves whether the first specified source geometry object is inside of the second source geometry object.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_CLIP`](../../Reference/3dmet/M3dmetFeatureEx.md) or [`M_IS_INSIDE`](../../Reference/3dmet/M3dmetFeatureEx.md).

| Value | Description |
| --- | --- |
| `M_INSIDE` | Specifies that the first source geometry object is completely contained within the second source geometry object. |
| `M_OUTSIDE` | Specifies that the first source geometry object is not contained within the second source geometry object. |
| `M_PARTIALLY_INSIDE` | Specifies that the first source geometry object is partially contained within the second source geometry object. |

#### `M_NUMBER`

Retrieves the number of geometries in the specified calculate 3D metrology result buffer.  This result is always available.

#### `M_OPERATION`

Retrieves the operation used during the last call to [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md).  This result is always available.

| Value | Description |
| --- | --- |
| *(see [`Operation`](Reference/3dmet/M3dmetFeatureEx.md))* |  |
| `M_CLIP` | Specifies that a 3D line geometry object was truncated such that it lies inside a second 3D geometry. |
| `M_CLOSEST_POINT` | Specifies that the point on the second specified 3D geometry that is closest to the first specified 3D geometry was calculated. |
| `M_EXTRUSION_BORDER` | Specifies that an extrusion of a 3D box's face was calculated such that the box's closest corner meets the specified plane. |
| `M_EXTRUSION_CENTER` | Specifies that an extrusion of a 3D box's face was calculated such that the center of the face touches the specified plane. |
| `M_FARTHEST_POINT` | Specifies that the point on the second specified 3D geometry that is farthest from the first specified 3D geometry was calculated. |
| `M_NORMAL_AT_POSITION` | Specifies that a unit normal line (one unit in length) was calculated. |
| `M_NOT_INITIALIZED` | Specifies that [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) has not been called with this calculate 3D metrology result buffer. |
| `M_ORTHOGONALIZE` | Specifies that the specified source 3D geometry was rotated such that it is arranged orthogonally with another 3D geometry. |
| `M_POINT_ON_LINE` | Specifies that a point on the source 3D line was created. |
| `M_POINT_ON_LINE_CLIPPED` | Specifies that a point on the source 3D line was created such that the point does not fall outside the limits of the line. |
| `M_PROJECTION` | Specifies that a point or line was projected onto a second 3D geometry, creating a new point or line on the surface of the second 3D geometry. |

#### `M_OVERLAP`

Retrieves the percentage of overlap between two source 3D geometries.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_OVERLAP`](../../Reference/3dmet/M3dmetFeatureEx.md).

#### `M_PARALLELISM`

Retrieves the deviation from a parallel orientation between two source 3D geometries.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_ANGLE`](../../Reference/3dmet/M3dmetFeatureEx.md) or [`M_PARALLELISM`](../../Reference/3dmet/M3dmetFeatureEx.md).

#### `M_PERPENDICULARITY`

Retrieves the deviation from a perpendicular orientation between two source 3D geometries.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_ANGLE`](../../Reference/3dmet/M3dmetFeatureEx.md) or [`M_PERPENDICULARITY`](../../Reference/3dmet/M3dmetFeatureEx.md).

#### `M_STATUS_EXTRUSION`

Retrieves whether the extrusion operation was successful.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_EXTRUSION_CENTER`](../../Reference/3dmet/M3dmetFeatureEx.md) or [`M_EXTRUSION_BORDER`](../../Reference/3dmet/M3dmetFeatureEx.md).

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies that the extrusion operation did not complete successfully. Failed extrusions can occur for an [`M_EXTRUSION_BORDER`](../../Reference/3dmet/M3dmetFeatureEx.md) operation when the plane intersects opposing parallel faces of the box. |
| `M_SUCCESS` | Specifies that the extrusion operation completed successfully. |

#### `M_UNION`

Retrieves the volume of the union of two source 3D geometries.  This result is available after calling [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) with [`M_OVERLAP`](../../Reference/3dmet/M3dmetFeatureEx.md).

---

### `Calculate 3D metrology result buffer ID, for retrieving results of an M3dmetVolumeEx operation`

Specifies the identifier of a calculate 3D metrology result buffer that holds results of an [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation.

#### `M_RESULT_ELEMENT_IMAGE_SIZE_X`

Retrieves the size in X of the source point cloud's mesh component or of the source depth map, in pixels. You can use this value to allocate an image buffer of the correct size when retrieving a result image created using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_VOLUME_ELEMENT_INDEX_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md), [`M_VOLUME_ELEMENT_MASK`](../../Reference/3dmet/M3dmetCopyResult.md), or [`M_VOLUME_ELEMENT_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md).

#### `M_RESULT_ELEMENT_IMAGE_SIZE_Y`

Retrieves the size in Y of the source point cloud's mesh component or of the source depth map, in pixels. You can use this value to allocate an image buffer of the correct size when retrieving a result image created using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_VOLUME_ELEMENT_INDEX_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md), [`M_VOLUME_ELEMENT_MASK`](../../Reference/3dmet/M3dmetCopyResult.md), or [`M_VOLUME_ELEMENT_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md).

#### `M_RESULT_IMAGE_SIZE_X`

Retrieves the size in X of the source point cloud's range component or of the source depth map, in pixels. You can use this value to allocate an image buffer of the correct size when retrieving a result image created using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_VOLUME_SOURCE_POINTS_MASK`](../../Reference/3dmet/M3dmetCopyResult.md) or [`M_VOLUME_SOURCE_POINTS_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md).

#### `M_RESULT_IMAGE_SIZE_Y`

Retrieves the size in Y of the source point cloud's range component or of the source depth map, in pixels. You can use this value to allocate an image buffer of the correct size when retrieving a result image created using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_VOLUME_SOURCE_POINTS_MASK`](../../Reference/3dmet/M3dmetCopyResult.md) or [`M_VOLUME_SOURCE_POINTS_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md).

#### `M_SAVE_VOLUME_INFO`

Retrieves whether volume information has been saved into the calculate 3D metrology result buffer. Volume information is necessary to copy results into an image buffer or container (for example, to create a status image using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) with [`M_VOLUME_ELEMENT_STATUS_IMAGE`](../../Reference/3dmet/M3dmetCopyResult.md)). Volume information is only saved if [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_SAVE_VOLUME_INFO`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_TRUE`](../../Reference/3dmet/M3dmetControl.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that volume information has not been saved. |
| `M_TRUE` | Specifies that volume information has been saved. |

#### `M_STATUS_VOLUME`

Retrieves the status of the volume operation.

| Value | Description |
| --- | --- |
| `M_FAIL_GAPS` | Specifies that the volume was not successfully computed, since the source Aurora Imaging Library object was a mesh with gaps, and there was no reference Aurora Imaging Library object. |
| `M_FAIL_INVALID_MESH` | Specifies that the volume was not successfully computed, since the mesh was not usable. |
| `M_SUCCESS` | Specifies that the volume was successfully computed. |
| `M_WARNING_GAPS` | Specifies that the volume was successfully computed, but there were gaps in the specified source depth map. |
| `M_NOT_INITIALIZED` | Specifies that the calculate 3D metrology result buffer was not used in a call to [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md), and contains no results. |

#### `M_VOLUME`

Retrieves the computed volume.

#### `M_VOLUME_MODE`

Retrieves the volume mode used to calculate the volume.

| Value | Description |
| --- | --- |
| `M_COMPLETE` *(default)* | Specifies to calculate all volume results, which can then be retrieved, copied, or drawn, according to the specified [`M_VOLUME_OUTPUT_MODE`](../../Reference/3dmet/M3dmetControl.md). |

#### `M_VOLUME_NB_ELEMENTS`

Retrieves the number of elements that were used to compute the volume.

#### `M_VOLUME_NB_NEGATIVE_ELEMENTS`

Retrieves the number of elements that were used to compute the negative part of the volume.  For more information, see [How the volume of a meshed point cloud with holes is calculated](../../UserGuide/C39_3D_Metrology/S06_Calculate_volume.md).

#### `M_VOLUME_NB_POSITIVE_ELEMENTS`

Retrieves the number of elements that were used to compute the positive part of the volume.

#### `M_VOLUME_NB_UNUSED_ELEMENTS`

Retrieves the number of valid elements that were not used to compute the volume. To illustrate, for a 10 x 10 depth map with 5 pixels corresponding to invalid data, 95 valid elements remain for a possible volume calculation. If the specified operation is [`M_ABOVE`](../../Reference/3dmet/M3dmetControl.md) and 40 elements contribute positively to the volume calculation, while 0 contribute negatively, then there are 55 valid elements that were not used to calculate the volume (95 - 40 - 0 = 55), and, for this example, [`M_VOLUME_NB_UNUSED_ELEMENTS`](../../Reference/3dmet/M3dmetGetResult.md) returns 55.

#### `M_VOLUME_REFERENCE_TYPE`

Retrieves the type of reference object that was used to compute the volume.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that a reference object was not used. |
| `M_DEPTH_MAP` | Specifies a depth map reference object. |
| `M_PLANE` | Specifies a plane reference object. |

---

### `Fit 3D metrology result ID`

Specifies the identifier of a fit 3D metrology result buffer.

#### `M_AXIS_X`

Retrieves the X-component of the fitted 3D geometry's unit vector.  If the fitted 3D geometry is a cylinder, the X-component of the cylinder's central axis unit vector is retrieved. This vector does not reflect the cylinder's length.  If the fitted 3D geometry is a line, the X-component of the line's direction unit vector is retrieved. This vector does not reflect the line's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the X-component of the unit vector, expressed in the working coordinate system. |

#### `M_AXIS_Y`

Retrieves the Y-component of the fitted 3D geometry's unit vector.  If the fitted 3D geometry is a cylinder, the Y-component of the cylinder's central axis unit vector is retrieved. This vector does not reflect the cylinder's length.  If the fitted 3D geometry is a line, the Y-component of the line's direction unit vector is retrieved. This vector does not reflect the line's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Y-component of the unit vector, expressed in the working coordinate system. |

#### `M_AXIS_Z`

Retrieves the Z-component of the fitted 3D geometry's unit vector.  If the fitted 3D geometry is a cylinder, the Z-component of the cylinder's central axis unit vector is retrieved. This vector does not reflect the cylinder's length.  If the fitted 3D geometry is a line, then the Z-component of the line's direction unit vector is retrieved. This vector does not reflect the line's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Z-component of the unit vector, expressed in the working coordinate system. |

#### `M_CENTER_X`

Retrieves the X-coordinate of the center point of the fitted 3D geometry, expressed in the working coordinate system.  If the fitted 3D geometry is a cylinder, the X-coordinate of the center point on the cylinder's central axis is retrieved.  If the fitted 3D geometry is a line, the X-coordinate of the center point on the line is retrieved.  If the fitted 3D geometry is a plane, the X-coordinate of the plane's center is retrieved. This center point is equivalent to the centroid of all inlier points.  If the fitted 3D geometry is a sphere, the X-coordinate of the sphere's center point is retrieved.

#### `M_CENTER_Y`

Retrieves the Y-coordinate of the center point of the fitted 3D geometry expressed in the working coordinate system.  If the fitted 3D geometry is a cylinder, the Y-coordinate of the center point on the cylinder's central axis is retrieved.  If the fitted 3D geometry is a line, the Y-coordinate of the center point on the line is retrieved.  If the fitted 3D geometry is a plane, the Y-coordinate of the plane's center is retrieved. This center point is equivalent to the centroid of all inlier points.  If the fitted 3D geometry is a sphere, the Y-coordinate of the sphere's center point is retrieved.

#### `M_CENTER_Z`

Retrieves the Z-coordinate of the center point of the fitted 3D geometry, expressed in the working coordinate system.  If the fitted 3D geometry is a cylinder, the Z-coordinate of the center point on the cylinder's central axis is retrieved.  If the fitted 3D geometry is a line, the Z-coordinate of the center point on the line is retrieved.  If the fitted 3D geometry is a plane, the Z-coordinate of the plane's center is retrieved. This center point is equivalent to the centroid of all inlier points.  If the fitted 3D geometry is a sphere, the Z-coordinate of the sphere's center point is retrieved.

#### `M_CLOSEST_TO_ORIGIN_X`

Retrieves the X-coordinate of the point on the fitted 3D plane geometry, closest to the origin of the working coordinate system.

#### `M_CLOSEST_TO_ORIGIN_Y`

Retrieves the Y-coordinate of the point on the fitted 3D plane geometry, closest to the origin of the working coordinate system.

#### `M_CLOSEST_TO_ORIGIN_Z`

Retrieves the Z-coordinate of the point on the fitted 3D plane geometry, closest to the origin of the working coordinate system.

#### `M_COEFFICIENT_A`

Retrieves the coefficient _A_ of the fitted 3D plane geometry's equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the coefficient _A_. |

#### `M_COEFFICIENT_B`

Retrieves the coefficient _B_ of the fitted 3D plane geometry's equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the coefficient _B_. |

#### `M_COEFFICIENT_C`

Retrieves the coefficient _C_ of the fitted 3D plane geometry's equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the coefficient _C_. |

#### `M_COEFFICIENT_D`

Retrieves the coefficient _D_ of the fitted 3D plane geometry's equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `Value` | Specifies the coefficient _D_. |

#### `M_END_POINT_X`

Retrieves the X-coordinate of the end point of the fitted 3D geometry, expressed in the working coordinate system.  If the fitted 3D geometry is a cylinder, the X-coordinate of the cylinder's end point (positioned at the center of the cylinder's second circular base) is retrieved.  If the fitted 3D geometry is a line, the X-coordinate of the line's end point is retrieved.

#### `M_END_POINT_Y`

Retrieves the Y-coordinate of the end point of the fitted 3D geometry, expressed in the working coordinate system.  If the fitted 3D geometry is a cylinder, the Y-coordinate of the cylinder's end point (positioned at the center of the cylinder's second circular base) is retrieved.  If the fitted 3D geometry is a line, the Y-coordinate of the line's end point is retrieved.

#### `M_END_POINT_Z`

Retrieves the Z-coordinate of the end point of the fitted 3D geometry, expressed in the working coordinate system.  If the fitted 3D geometry is a cylinder, the Z-coordinate of the cylinder's end point (positioned at the center of the cylinder's second circular base) is retrieved.  If the fitted 3D geometry is a line, the Z-coordinate of the line's end point is retrieved.

#### `M_FIT_RMS_ERROR`

Retrieves the root-mean-squared (RMS) error of the distance between the point cloud or depth map, and the fitted 3D geometry. Only inliers are considered when calculating the RMS error.  For planes, the error corresponds to the distance between the points and the fitted plane.  For spheres and cylinders, the error corresponds to the distance between the points and the surface of the sphere or cylinder. For cylinders, this specifically refers to the distance to the curved surface and not the circular bases.  This result is only available for retrieval after a successful fit.

#### `M_GEOMETRY_TYPE`

Retrieves the type of the fitted 3D geometry.  This result is only available for retrieval after a successful fit.

| Value | Description |
| --- | --- |
| `M_CYLINDER` | Specifies a cylinder. |
| `M_LINE` | Specifies a line. |
| `M_PLANE` | Specifies a plane. |
| `M_SPHERE` | Specifies a sphere. |

#### `M_LENGTH`

Retrieves the length of the fitted 3D cylinder geometry, or fitted 3D line geometry, in world units.

#### `M_NORMAL_X`

Retrieves the X-component of the fitted 3D plane geometry's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the X-component of the normal unit vector, expressed in the working coordinate system. |

#### `M_NORMAL_Y`

Retrieves the Y-component of the fitted 3D plane geometry's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Y-component of the normal unit vector, expressed in the working coordinate system. |

#### `M_NORMAL_Z`

Retrieves the Z-component of the fitted 3D plane geometry's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Z-component of the normal unit vector, expressed in the working coordinate system. |

#### `M_NUMBER_OF_POINTS_INLIERS`

Retrieves the number of points that were considered inliers during the fit operation.  This result is only available for retrieval after a fit operation.

#### `M_NUMBER_OF_POINTS_MISSING_DATA`

Retrieves the number of points with zero confidence (if you specified a container), or the number of points with an invalid value (if you specified a depth map). Note that in the case of depth maps, this only refers to the points inside the ROI.  This result is only available for retrieval after a fit operation.

#### `M_NUMBER_OF_POINTS_OUTLIERS`

Retrieves the number of points that were considered outliers during the fit operation.  This result is only available for retrieval after a fit operation.

#### `M_NUMBER_OF_POINTS_TOTAL`

Retrieves the total number of points. This number is equal to [`M_NUMBER_OF_POINTS_VALID`](../../Reference/3dmet/M3dmetGetResult.md) + [`M_NUMBER_OF_POINTS_MISSING_DATA`](../../Reference/3dmet/M3dmetGetResult.md).  This result is only available for retrieval after a fit operation.

#### `M_NUMBER_OF_POINTS_VALID`

Retrieves the number of valid points, which is equal to the number of points with non-zero confidence. This number is equal to [`M_NUMBER_OF_POINTS_INLIERS`](../../Reference/3dmet/M3dmetGetResult.md) + [`M_NUMBER_OF_POINTS_OUTLIERS`](../../Reference/3dmet/M3dmetGetResult.md).  This result is only available for retrieval after a fit operation.

#### `M_OUTLIER_DISTANCE`

Retrieves the outlier distance used for the fit. When [`M_AUTO_VALUE`](../../Reference/3dmet/M3dmetFit.md) was specified, the function returns the automatically calculated value; otherwise, it returns the value passed to [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md).  This result is only available for retrieval after a fit operation.

#### `M_RADIUS`

Retrieves the radius of the fitted 3D cylinder geometry or 3D sphere geometry in world units.

#### `M_RESULT_IMAGE_SIZE_X`

Retrieves the size in X of the range component of the source point cloud or depth map, in pixels. Note that you can use this value to allocate an image buffer of an appropriate size for [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md).  This result is only available for retrieval after a fit operation.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size in X. |

#### `M_RESULT_IMAGE_SIZE_Y`

Retrieves the size in Y of the range component of the source point cloud or depth map, in pixels. Note that you can use this value to allocate an image buffer of an appropriate size for [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md).  This result is only available for retrieval after a fit operation.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size in Y. |

#### `M_START_POINT_X`

Retrieves the X-coordinate of the fitted 3D geometry's start point, expressed in the working coordinate system.  If the fitted 3D geometry is a cylinder, the start point is positioned at the center of the cylinder's first circular base.  If the fitted 3D geometry is a line, the start point is positioned at the start of the line.

#### `M_START_POINT_Y`

Retrieves the Y-coordinate of the fitted 3D geometry's start point, expressed in the working coordinate system.  If the fitted 3D geometry is a cylinder, the start point is positioned at the center of the cylinder's first circular base.  If the fitted 3D geometry is a line, the start point is positioned at the start of the line.

#### `M_START_POINT_Z`

Retrieves the Z-coordinate of the fitted 3D geometry's start point, expressed in the working coordinate system.  If the fitted 3D geometry is a cylinder, the start point is positioned at the center of the cylinder's first circular base.  If the fitted 3D geometry is a line, the start point is positioned at the start of the line.

#### `M_STATUS_FIT`

Retrieves the status of the fit operation.  This result is always available for retrieval.

| Value | Description |
| --- | --- |
| `M_ALL_POINTS_COLLINEAR` | Specifies that the fit failed because the fit operation tried to fit a sphere, cylinder, or plane, to points that lie on the same straight line. |
| `M_ALL_POINTS_COPLANAR` | Specifies that the fit failed because the fit operation tried to fit a sphere or cylinder, to points that lie on the same plane. |
| `M_BAD_ESTIMATE` | Specifies that the fit failed because the initial fit estimate did not include any valid inliers. |
| `M_NOT_ENOUGH_VALID_DATA` | Specifies that the fit failed because there were not enough valid points to fit the specified 3D geometry. |
| `M_NOT_INITIALIZED` | Specifies that the fit 3D metrology result buffer was not used in a call to[`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md), and contains no results. |
| `M_SUCCESS` | Specifies that the fit operation completed successfully. |

---

### `Statistics 3D metrology result ID`

Specifies the identifier of a statistics 3D metrology result buffer.

#### `M_STAT_MAX`

Retrieves the maximum distance between the point cloud or depth map, and the reference 3D geometry.

#### `M_STAT_MAX_ABS`

Retrieves the maximum absolute distance between the point cloud or depth map, and the reference 3D geometry.

#### `M_STAT_MEAN`

Retrieves the average distance between the point cloud or depth map, and the reference 3D geometry.

#### `M_STAT_MEAN_ABS`

Retrieves the average absolute distance between the point cloud or depth map, and the reference 3D geometry.

#### `M_STAT_MIN`

Retrieves the minimum distance between the point cloud or depth map, and the reference 3D geometry.

#### `M_STAT_MIN_ABS`

Retrieves the minimum absolute distance between the point cloud or depth map, and the reference 3D geometry.

#### `M_STAT_NUMBER`

Retrieves the number of points that satisfied the condition specified when [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md) was called (using the [`Condition`](../../Reference/3dmet/M3dmetStat.md) parameter).

#### `M_STAT_RMS`

Retrieves the root-mean-square (RMS) error between the point cloud or depth map, and the reference 3D geometry object. Aurora Imaging Library calculates the RMS error using the following formula:  *[Image: 3dreg_RootMeanSquareError_eq.png]*

#### `M_STAT_STANDARD_DEVIATION`

Retrieves the standard deviation of all the distances calculated between the point cloud or depth map, and the reference 3D geometry. Aurora Imaging Library calculates the standard deviation using the following formula:  *[Image: 3dmet_StandardDeviation_eq.png]*

#### `M_STAT_SUM`

Retrieves the sum of all the distances calculated between the point cloud or depth map, and the reference 3D geometry.

#### `M_STAT_SUM_ABS`

Retrieves the sum of all the absolute distances calculated between the point cloud or depth map, and the reference 3D geometry.

#### `M_STAT_SUM_OF_SQUARES`

Retrieves the sum of squared distances between the point cloud or depth map, and the reference 3D geometry.

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the requested information, cast to an _AIL_DOUBLE_. If the requested information does not fit into an _AIL_DOUBLE_, this function will return `M_NULL`or truncate the information.
