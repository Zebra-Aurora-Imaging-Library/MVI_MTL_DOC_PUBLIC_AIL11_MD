---
doctype: Reference
module: 3dmod
function: M3dmodGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodGetResult"
---

# M3dmodGetResult

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

> Get the specified type of result(s) from a 3D model finder result buffer.

## Syntax

```c
AIL_DOUBLE M3dmodGetResult(
    AIL_ID    Result3dmodId,  //in
    AIL_INT64 ResultIndex,    //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the result(s) of the specified type from a 3D model finder result buffer.

## Parameters

### `Result3dmodId` *(in, AIL_ID)*

Specifies the identifier of the 3D model finder result buffer from which to retrieve results. The result buffer must have been previously allocated on the system using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_..._RESULT`](../../Reference/3dmod/M3dmodAllocResult.md).

### `ResultIndex` *(in, AIL_INT64)*

Specifies where to get results. This parameter must be set to one of the following values:

*For specifying the index of the 3D model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to retrieve results for all model occurrences in the 3D model finder result buffer. |
| `M_GENERAL` *(default)* | Specifies to retrieve general results from the 3D model finder result buffer. |
| `0 <= Value < M_NUMBER` | Specifies the index of the model occurrence for which to retrieve results. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the array in which to write results. Since [`M3dmodGetResult`](../../Reference/3dmod/M3dmodGetResult.md) also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For retrieving general results from the 3D model finder result buffer

To retrieve a general result, the [`ResultType`](../../Reference/3dmod/M3dmodGetResult.md) parameter should be set to one of the following values. In this case, you must set [`ResultIndex`](../../Reference/3dmod/M3dmodGetResult.md) to [`M_GENERAL`](../../Reference/3dmod/M3dmodGetResult.md) or [`M_DEFAULT`](../../Reference/3dmod/M3dmodGetResult.md). The following results are only available for retrieval after a 3D model finder operation, unless otherwise specified.

---

### `3D model finder result buffer ID`

Specifies a box, cylinder, rectangular plane, sphere, surface, or planar surface 3D model finder result buffer, allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_..._RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), and used to store [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) results.

#### `M_FIT_DISTANCE_USED`

Retrieves the fit distance used during the 3D model finder operation. For a surface model, this result is only available if [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) was enabled.

#### `M_NUMBER`

Retrieves the number of model occurrences found.

#### `M_REMOVE_FLOOR_OFFSET_USED`

Retrieves the offset, relative to the defined floor plane, within which points are removed. This result is only available if [`M_REMOVE_FLOOR`](../../Reference/3dmod/M3dmodControl.md) was enabled.  This result is only available for surface and planar surface 3D model finder result buffers.

#### `M_RESULT_IMAGE_SIZE_X`

Retrieves the size in X of the range component of the target point cloud, in pixels.  Note that you can use this value to allocate an image buffer of an appropriate size for [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) to create an occurrence index image or mask image.

#### `M_RESULT_IMAGE_SIZE_Y`

Retrieves the size in Y of the range component of the target point cloud, in pixels.  Note that you can use this value to allocate an image buffer of an appropriate size for [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) to create an occurrence index image or mask image.

#### `M_STATUS`

Retrieves the global status of the last [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) operation.

| Value | Description |
| --- | --- |
| `M_ALL_POINTS_REPLICATED` | Specifies that all points of the scene were replicated.  Note that this result is only available for surface and planar surface 3D model finder result buffers. |
| `M_COMPLETE` | Specifies that the 3D model finder operation completed successfully. 3D model finder results are available. |
| `M_CURRENTLY_CALCULATING` | Specifies that the 3D model finder operation is ongoing. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected internal error occurred during the 3D model finder operation. |
| `M_MISSING_COMPONENT_NORMALS_AIL` | Specifies that the source point cloud container does not have the [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufInquireContainer.md) component, so the 3D model finder operation was unable to be completed. You can use [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md) to generate a normals component. |
| `M_NOT_ENOUGH_FEATURES` | Specifies that the surface 3D model finder operation was not completed because of an insufficient number of unique features. |
| `M_NOT_ENOUGH_MEMORY` | Specifies that the 3D model finder operation was not completed because of a lack of available memory. |
| `M_NOT_ENOUGH_VALID_DATA` | Specifies that the 3D model finder operation failed because there were not enough valid points. |
| `M_NOT_INITIALIZED` | Specifies that the 3D model finder result buffer was not used in a call to [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md), and contains no results. |
| `M_STOPPED_BY_REQUEST` | Specifies that the 3D model finder operation was stopped from another thread using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md)with [`M_STOP_FIND`](../../Reference/3dmod/M3dmodControl.md). |
| `M_TIMEOUT_REACHED` | Specifies that the 3D model finder operation took longer than the allowed value, specified using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_TIMEOUT`](../../Reference/3dmod/M3dmodControl.md), and stopped before completion. |
| `M_TRANSFORMED` | Specifies that the surface or planar surface 3D model finder result buffer contains results that were transformed using [`M3dmodModifyResult`](../../Reference/3dmod/M3dmodModifyResult.md) with [`M_TRANSFORM`](../../Reference/3dmod/M3dmodModifyResult.md). |

#### `M_STATUS_REFINE_REGISTRATION`

Retrieves the global status of the refine registration applied to the different occurrences. This result is only available if [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) was enabled.  This result is only available for surface and planar surface 3D model finder result buffers.

| Value | Description |
| --- | --- |
| `M_APPLIED` | Specifies that refine registration was applied successfully to all occurrences. |
| `M_APPLIED_AND_REJECTED` | Specifies that even though[`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) was enabled, at least one registration was not applied successfully to an occurrence. |
| `M_NOT_APPLIED` | Specifies that refine registration was not enabled, so it was not applied to the occurrences. |

#### `M_STATUS_SCENE_RESOLUTION`

Retrieves whether the resolution of the surface model was compatible with the resolution of the scene. If the resolution of the model was much lower or higher than that of the scene, this result type will return [`M_INCOMPATIBLE`](../../Reference/3dmod/M3dmodGetResult.md). You can use [`M3dmodInquire`](../../Reference/3dmod/M3dmodInquire.md) with [`M_MODEL_RESOLUTION`](../../Reference/3dmod/M3dmodInquire.md) to inquire the resolution of the model. You can use [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_SEARCH_POINT_RESOLUTION`](../../Reference/3dmod/M3dmodControl.md) to specify the resolution of the scene; Aurora Imaging Library will attempt to increase or decrease the model's resolution accordingly.  This result is only available for surface and planar surface 3D model finder result buffers.

| Value | Description |
| --- | --- |
| `M_COMPATIBLE` | Specifies that the resolution was compatible. |
| `M_INCOMPATIBLE` | Specifies that the resolution was not compatible. |

### For retrieving a result of a single occurrence or all occurrences from the 3D model finder result buffer

To retrieve a result for a single occurrence or for all occurrences, the [`ResultType`](../../Reference/3dmod/M3dmodGetResult.md) parameter should be set to one of the following values. In this case, you must set [`ResultIndex`](../../Reference/3dmod/M3dmodGetResult.md) to the index of the occurrence or [`M_ALL`](../../Reference/3dmod/M3dmodGetResult.md). The following results are only available for retrieval after a 3D model finder operation.

---

### `Box 3D model finder result buffer ID`

Specifies a box 3D model finder result buffer, allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_BOX_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), and used to store [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) results.

#### `M_AREA`

Retrieves the surface area of the model occurrence. The surface area is calculated using the following formula: `_SA_ = 2(_l_ * _w_ + _l_ * _h_ + _w_ * _h_)`.

#### `M_CENTER_X`

Retrieves the X-coordinate of the center point of the box occurrence or its specified face (depending on whether you add **M_FACE_INDEX()** to [`M_CENTER_X`](../../Reference/3dmod/M3dmodGetResult.md)), expressed in the working coordinate system.  For a box occurrence, the X-coordinate of the box's center point is retrieved.  For a box occurrence's face, the X-coordinate of the face's center point is retrieved.

#### `M_CENTER_Y`

Retrieves the Y-coordinate of the center point of the box occurrence or its specified face (depending on whether you add **M_FACE_INDEX()** to [`M_CENTER_Y`](../../Reference/3dmod/M3dmodGetResult.md)), expressed in the working coordinate system.  For a box occurrence, the Y-coordinate of the box's center point is retrieved.  For a box occurrence's face, the Y-coordinate of the face's center point is retrieved.

#### `M_CENTER_Z`

Retrieves the Z-coordinate of the center point of the box occurrence or its specified face (depending on whether you add **M_FACE_INDEX()** to [`M_CENTER_Z`](../../Reference/3dmod/M3dmodGetResult.md)), expressed in the working coordinate system.  For a box occurrence, the Z-coordinate of the box's center point is retrieved.  For a box occurrence's face, the Z-coordinate of the face's center point is retrieved.

#### `M_COMPLETION_STATUS`

Retrieves which method was used to complete the box.

| Value | Description |
| --- | --- |
| `M_COMPLETION_TO_BACKGROUND` | Specifies that the face was extruded to a background plane. |
| `M_COMPLETION_TO_STAIRCASE` | Specifies that the face was extruded to an edge of a staircase plane. |
| `M_COMPLETION_TO_USER_SIZE` | Specifies that the face was extruded to the completion size. |
| `M_NONE` | Specifies that no completion method was used because an occurrence with more than 1 face was found. |

#### `M_COVERAGE`

Retrieves the plane coverage of the box occurrence's specified face. The plane coverage is the percentage of a plane's surface found in the face and covered by inlier points.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the plane coverage, as a percentage. |

#### `M_NORMAL_X`

Retrieves the X-component of the normal unit vector of the box occurrence's specified face.

| Value | Description |
| --- | --- |
| `-1.0 <= Value <= 1.0` | Specifies the X-component of the box face's normal unit vector, expressed in the working coordinate system. |

#### `M_NORMAL_Y`

Retrieves the Y-component of the normal unit vector of the box occurrence's specified face.

| Value | Description |
| --- | --- |
| `-1.0 <= Value <= 1.0` | Specifies the Y-component of the box face's normal unit vector, expressed in the working coordinate system. |

#### `M_NORMAL_Z`

Retrieves the Z-component of the normal unit vector of the box occurrence's specified face.

| Value | Description |
| --- | --- |
| `-1.0 <= Value <= 1.0` | Specifies the Z-component of the box face's normal unit vector, expressed in the working coordinate system. |

#### `M_NUMBER_OF_POINTS`

Retrieves the number of points in the box occurrence or its specified face, depending on whether you add **M_FACE_INDEX()** to [`M_NUMBER_OF_POINTS`](../../Reference/3dmod/M3dmodGetResult.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_NUMBER_OF_POINTS_RESERVED`

Retrieves the number of reserved points in the box occurrence or its specified face, depending on whether you add **M_FACE_INDEX()** to [`M_NUMBER_OF_POINTS_RESERVED`](../../Reference/3dmod/M3dmodGetResult.md). These are the points in the reserved area around the occurrence. They are not considered in the fit, are not included in the calculation of the score, and are not considered for other occurrences.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_NUMBER_OF_VISIBLE_FACES`

Retrieves the number of visible faces (planes) used to fit the box occurrence.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of visible faces (planes). |

#### `M_RMS_ERROR`

Retrieves the root-mean-square (RMS) error between the points found in the box occurrence or its specified face (depending on whether you add **M_FACE_INDEX()** to [`M_RMS_ERROR`](../../Reference/3dmod/M3dmodGetResult.md)) and the source model.

#### `M_SCORE`

Retrieves the score of the occurrence, where the score is based on the occurrence's model coverage. The model coverage is the percentage of the model's surface found in the occurrence and covered by inlier points. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. You can set the maximum expected model coverage, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).  You can add **M_FACE_INDEX()** to [`M_SCORE`](../../Reference/3dmod/M3dmodGetResult.md) to retrieve this result for one or all of the box occurrence's faces. The score of a box face is defined relative to the maximum expected plane coverage. In this case, a face with a score of 100% has a plane coverage equal to the maximum expected plane coverage. You can set the maximum expected plane coverage, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_PLANE_MAX_COVERAGE`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the score of the occurrence, as a percentage. |

#### `M_SIZE_X`

Retrieves the length of the box occurrence along its X-axis.

#### `M_SIZE_Y`

Retrieves the length of the box occurrence along its Y-axis.

#### `M_SIZE_Z`

Retrieves the length of the box occurrence along its Z-axis.

#### `M_UNROTATED_MAX_X`

Retrieves the box occurrence's maximum X-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

#### `M_UNROTATED_MAX_Y`

Retrieves the box occurrence's maximum Y-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

#### `M_UNROTATED_MAX_Z`

Retrieves the box occurrence's maximum Z-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

#### `M_UNROTATED_MIN_X`

Retrieves the box occurrence's minimum X-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

#### `M_UNROTATED_MIN_Y`

Retrieves the box occurrence's minimum Y-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

#### `M_UNROTATED_MIN_Z`

Retrieves the box occurrence's minimum Z-coordinate, ignoring the box's rotation. The coordinate is returned as if the box is axis-aligned.

#### `M_VISIBLE`

Retrieves whether the box occurrence's specified face was visible in the target. A face is only considered a visible face of the box occurrence if its score is greater than or equal to [`M_PLANE_ACCEPTANCE`](../../Reference/3dmod/M3dmodInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the face was not visible. |
| `M_TRUE` | Specifies that the face was visible. |

#### `M_VOLUME`

Retrieves the volume of the model occurrence. The volume is calculated using the following formula: `_V_ = _l_ * _w_ * _h_`.

---

### `Cylinder 3D model finder result buffer ID`

Specifies a cylinder 3D model finder result buffer, allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_CYLINDER_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), and used to store [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) results.

#### `M_AREA`

Retrieves the surface area of the model occurrence. The surface area of a cylinder occurrence with no bases is calculated using the following formula: `_SA_ = 2π_r_ _h_`. The surface area of a cylinder occurrence with bases is calculated using the following formula: `_SA_ = 2π_r_ _h_ + 2π_r_ <sup>2</sup>`.

#### `M_AXIS_X`

Retrieves the X-component of the cylinder occurrence's central axis unit vector. This vector does not reflect the cylinder's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the X-component of the unit vector, expressed in the working coordinate system. |

#### `M_AXIS_Y`

Retrieves the Y-component of the cylinder occurrence's central axis unit vector. This vector does not reflect the cylinder's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Y-component of the unit vector, expressed in the working coordinate system. |

#### `M_AXIS_Z`

Retrieves the Z-component of the cylinder occurrence's central axis unit vector. This vector does not reflect the cylinder's length.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Z-component of the unit vector, expressed in the working coordinate system. |

#### `M_CENTER_X`

Retrieves the X-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a cylinder model occurrence, the X-coordinate of the center point on the cylinder's central axis is retrieved.

#### `M_CENTER_Y`

Retrieves the Y-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a cylinder model occurrence, the Y-coordinate of the center point on the cylinder's central axis is retrieved.

#### `M_CENTER_Z`

Retrieves the Z-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a cylinder model occurrence, the Z-coordinate of the center point on the cylinder's central axis is retrieved.

#### `M_END_POINT_X`

Retrieves the X-coordinate of the cylinder occurrence's end point, expressed in the working coordinate system. The end position is located at the center of the cylinder's second circular base.

#### `M_END_POINT_Y`

Retrieves the Y-coordinate of the cylinder occurrence's end point, expressed in the working coordinate system. The end position is located at the center of the cylinder's second circular base.

#### `M_END_POINT_Z`

Retrieves the Z-coordinate of the cylinder occurrence's end point, expressed in the working coordinate system. The end position is located at the center of the cylinder's second circular base.

#### `M_LENGTH`

Retrieves the length of the cylinder occurrence in world units.

#### `M_NUMBER_OF_POINTS`

Retrieves the number of points in the model occurrence.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_NUMBER_OF_POINTS_RESERVED`

Retrieves the number of reserved points in the model occurrence. These are the points in the reserved area around the occurrence (established using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_RESERVED_POINTS_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)). They are not considered in the fit, are not included in the calculation of the score, and are not considered for other occurrences.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_RADIUS`

Retrieves the radius of the cylinder occurrence in world units.

#### `M_RMS_ERROR`

Retrieves the root-mean-square (RMS) error between the points found in the occurrence and the source model.

#### `M_SCORE`

Retrieves the score of the occurrence, where the score is based on the occurrence's model coverage. The model coverage is the percentage of the model's surface found in the occurrence and covered by inlier points. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. You can set the maximum expected model coverage, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value < 100.0` | Specifies the score of the occurrence, as a percentage. |

#### `M_START_POINT_X`

Retrieves the X-coordinate of the cylinder occurrence's start point, expressed in the working coordinate system. The start position is located at the center of the cylinder's first circular base.

#### `M_START_POINT_Y`

Retrieves the Y-coordinate of the cylinder occurrence's start point, expressed in the working coordinate system. The start position is located at the center of the cylinder's first circular base.

#### `M_START_POINT_Z`

Retrieves the Z-coordinate of the cylinder's occurrence's start point, expressed in the working coordinate system. The start position is located at the center of the cylinder's first circular base.

#### `M_VOLUME`

Retrieves the volume of the model occurrence. The volume is calculated using the following formula: `_V_ = π_r_ <sup>2</sup> _h_`.

---

### `Rectangular plane 3D model finder result buffer ID`

Specifies a rectangular plane 3D model finder result buffer, allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_RECTANGULAR_PLANE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), and used to store [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) results.

#### `M_AREA`

Retrieves the surface area of the model occurrence. The surface area is calculated using the following formula: `_SA_ = _l_ * _w_`.

#### `M_CENTER_X`

Retrieves the X-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a rectangular plane model occurrence, the X-coordinate of the center point of the rectangular plane's bounding box is retrieved.

#### `M_CENTER_Y`

Retrieves the Y-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a rectangular plane model occurrence, the Y-coordinate of the center point of the rectangular plane's bounding box is retrieved.

#### `M_CENTER_Z`

Retrieves the Z-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a rectangular plane model occurrence, the Z-coordinate of the center point of the rectangular plane's bounding box is retrieved.

#### `M_CLOSEST_TO_ORIGIN_X`

Retrieves the X-coordinate of the point on the rectangular plane occurrence closest to the origin of the working coordinate system.

#### `M_CLOSEST_TO_ORIGIN_Y`

Retrieves the Y-coordinate of the point on the rectangular plane occurrence closest to the origin of the working coordinate system.

#### `M_CLOSEST_TO_ORIGIN_Z`

Retrieves the Z-coordinate of the point on the rectangular plane occurrence closest to the origin of the working coordinate system.

#### `M_COEFFICIENT_A`

Retrieves the coefficient A of the plane equation,`Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the coefficient A of the plane equation. |

#### `M_COEFFICIENT_B`

Retrieves the coefficient B of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the coefficient B of the plane equation. |

#### `M_COEFFICIENT_C`

Retrieves the coefficient C of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the coefficient C of the plane equation. |

#### `M_COEFFICIENT_D`

Retrieves the coefficient D of the plane equation, `Ax + By + Cz + D = 0`.

| Value | Description |
| --- | --- |
| `Value` | Specifies the coefficient D of the plane equation. |

#### `M_NORMAL_X`

Retrieves the X-component of the rectangular plane occurrence's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the X-component of the rectangular plane occurrence's normal unit vector, expressed in the working coordinate system. |

#### `M_NORMAL_Y`

Retrieves the Y-component of the rectangular plane occurrence's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Y-component of the rectangular plane occurrence's normal unit vector, expressed in the working coordinate system. |

#### `M_NORMAL_Z`

Retrieves the Z-component of the rectangular plane occurrence's normal unit vector.

| Value | Description |
| --- | --- |
| `-1.0 >= Value >= 1.0` | Specifies the Z-component of the rectangular plane occurrence's normal unit vector, expressed in the working coordinate system. |

#### `M_NUMBER_OF_POINTS`

Retrieves the number of points in the model occurrence.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_NUMBER_OF_POINTS_RESERVED`

Retrieves the number of reserved points in the model occurrence. These are the points in the reserved area around the occurrence. They are not considered in the fit, are not included in the calculation of the score, and are not considered for other occurrences.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_RMS_ERROR`

Retrieves the root-mean-square (RMS) error between the points found in the occurrence and the source model.

#### `M_SCORE`

Retrieves the score of the occurrence, where the score is based on the occurrence's model coverage. The model coverage is the percentage of the model's surface found in the occurrence and covered by inlier points. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. You can set the maximum expected model coverage, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value < 100.0` | Specifies the score of the occurrence, as a percentage. |

#### `M_SIZE_X`

Retrieves the length of the rectangular plane occurrence along its X-axis.

#### `M_SIZE_Y`

Retrieves the length of the rectangular plane occurrence along its Y-axis.

---

### `Sphere 3D model finder result buffer ID`

Specifies a sphere 3D model finder result buffer, allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_SPHERE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), and used to store [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) results.

#### `M_AREA`

Retrieves the surface area of the model occurrence. The surface area is calculated using the following formula: `_SA_ = 4π_r_ <sup>2</sup>`.

#### `M_CENTER_X`

Retrieves the X-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a sphere model occurrence, the X-coordinate of the sphere's center point is retrieved.

#### `M_CENTER_Y`

Retrieves the Y-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a sphere model occurrence, the Y-coordinate of the sphere's center point is retrieved.

#### `M_CENTER_Z`

Retrieves the Z-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a sphere model occurrence, the Z-coordinate of the sphere's center point is retrieved.

#### `M_NUMBER_OF_POINTS`

Retrieves the number of points in the model occurrence.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_NUMBER_OF_POINTS_RESERVED`

Retrieves the number of reserved points in the model occurrence. These are the points in the reserved area around the occurrence (established using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_RESERVED_POINTS_DISTANCE`](../../Reference/3dmod/M3dmodControl.md)). They are not considered in the fit, are not included in the calculation of the score, and are not considered for other occurrences.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_RADIUS`

Retrieves the radius of the sphere occurrence in world units.

#### `M_RMS_ERROR`

Retrieves the root-mean-square (RMS) error between the points found in the occurrence and the source model.

#### `M_SCORE`

Retrieves the score of the occurrence, where the score is based on the occurrence's model coverage. The model coverage is the percentage of the model's surface found in the occurrence and covered by inlier points. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. You can set the maximum expected model coverage, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value < 100.0` | Specifies the score of the occurrence, as a percentage. |

#### `M_VOLUME`

Retrieves the volume of the model occurrence. The volume is calculated using the following formula: `_V_ = (4/3)π_r_ <sup>3</sup>`.

---

### `Surface or planar surface 3D model finder result buffer ID`

Specifies a surface or planar surface 3D model finder result buffer, allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md) or [`M_FIND_PLANAR_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), and used to store [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) results.  Note that **M_SCORE_GRIP()**, [`M_SCORE_GRIP_BEST`](../../Reference/3dmod/M3dmodGetResult.md), and [`M_SCORE_GRIP_BEST_LABEL`](../../Reference/3dmod/M3dmodGetResult.md) are not available for planar surface 3D model finder result buffers.

#### `?`

| Value | Description |
| --- | --- |
| `0.0 <= Value < 100.0` | Specifies the grip score, as a percentage. |

#### `M_CENTER_X`

Retrieves the X-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a surface model occurrence, the X-coordinate of the center of the surface's bounding box is retrieved.

#### `M_CENTER_Y`

Retrieves the Y-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a surface model occurrence, the Y-coordinate of the center of the surface's bounding box is retrieved.

#### `M_CENTER_Z`

Retrieves the Z-coordinate of the center point of the model occurrence, expressed in the working coordinate system.  For a surface model occurrence, the Z-coordinate of the center of the surface's bounding box is retrieved.

#### `M_NUMBER_OF_POINTS`

Retrieves the number of points in the model occurrence.  This result is only available if [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) was enabled.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_NUMBER_OF_POINTS_RESERVED`

Retrieves the number of reserved points in the model occurrence. These are the points in the reserved area around the occurrence. They are not considered in the fit, are not included in the calculation of the score, and are not considered for other occurrences.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of points. |

#### `M_POSITION_X`

Retrieves the X-coordinate of the surface occurrence. This is the X-position of the model's reference axis transformed at the occurrence.  Note, the origin of the model's reference axis is at the origin of the working coordinate system of the original point cloud from which the surface model was defined.

#### `M_POSITION_Y`

Retrieves the Y-coordinate of the surface occurrence. This is the Y-position of the model's reference axis transformed at the occurrence.  Note, the origin of the model's reference axis is at the origin of the working coordinate system of the original point cloud from which the surface model was defined.

#### `M_POSITION_Z`

Retrieves the Z-coordinate of the surface occurrence. This is the Z-position of the model's reference axis transformed at the occurrence.  Note, the origin of the model's reference axis is at the origin of the working coordinate system of the original point cloud from which the surface model was defined.

#### `M_RMS_ERROR`

Retrieves the root-mean-square (RMS) error between the points found in the occurrence and the source model. For a surface model, this result is only available if [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) was enabled. To improve fit results, you should also enable refine registration ([`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md)) prior to performing the search. By default, Aurora Imaging Library uses the features of the model and target to find occurrences, but it doesn't do any fitting. To get more accurate score and pose information, you should enable refine registration.

#### `M_SCORE`

Retrieves the score of the occurrence, where the score is based on the occurrence's model coverage. The model coverage is the percentage of the model's surface found in the occurrence and covered by inlier points. The score is defined relative to the maximum expected model coverage, such that an occurrence with a score of 100% has a model coverage equal to the maximum expected model coverage. You can set the maximum expected model coverage, using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_COVERAGE_MAX`](../../Reference/3dmod/M3dmodControl.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value < 100.0` | Specifies the score of the occurrence, as a percentage. |

#### `M_SCORE_COLOR`

Retrieves the color score of the occurrence, where the color score indicates the similarity between the color of the model and the color of the occurrence.  This result is only available if [`M_USE_COLOR`](../../Reference/3dmod/M3dmodControl.md) was enabled.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 100.0` | Specifies the color score of the occurrence, as a percentage. |

#### `M_SCORE_FIT`

Retrieves the fit score of the occurrence, where the fit score is based on the Euclidean distance between the occurrence's points and the model. The score is calculated as follows:  `Fit Score = 1 - Normalized RMS error`  This result is only available if [`M_SAVE_FIT_INFO`](../../Reference/3dmod/M3dmodControl.md) was enabled. To improve fit results, you should also enable refine registration ([`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md) with [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md)) prior to performing the search. By default, Aurora Imaging Library uses the features of the model and target to find occurrences, but it doesn't do any fitting. To get more accurate score and pose information, you should enable refine registration.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 100.0` | Specifies the fit score of the occurrence, as a percentage. |

#### `M_SCORE_GRIP_BEST`

Retrieves the highest grip score among all labels. The grip score is the percentage of the model's labeled surface found in the occurrence and covered by inlier points.  This result is only available for surface 3D model finder result buffers and only if a grip label image that contains non-zero values was copied to the find surface 3D model finder context, using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_GRIP_LABEL_IMAGE`](../../Reference/3dmod/M3dmodCopy.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value < 100.0` | Specifies the best grip score. |

#### `M_SCORE_GRIP_BEST_LABEL`

Retrieves the label with the highest grip score. The grip score is the percentage of the model's labeled surface found in the occurrence and covered by inlier points.  This result is only available for surface 3D model finder result buffers and only if a grip label image that contains non-zero values was copied to the find surface 3D model finder context, using [`M3dmodCopy`](../../Reference/3dmod/M3dmodCopy.md) with [`M_GRIP_LABEL_IMAGE`](../../Reference/3dmod/M3dmodCopy.md).

| Value | Description |
| --- | --- |
| `0 < Value <= 255` | Specifies the label. |

#### `M_SCORE_TARGET`

Retrieves the target score of the occurrence, where the target score is based on the target coverage. Target coverage is a measure of the points found in the occurrence that don't have matching points in the original model (that is, extra points), weighted by the deviation in position of the common points. Points found in the occurrence that are not present in the model will reduce the target score. For example, a target score of 100% means that there are no points in the occurrence that do not fit the model.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 100.0` | Specifies the target score of the occurrence, as a percentage. |

#### `M_STATUS_REFINE_REGISTRATION`

Retrieves the status of the refine registration applied to the occurrence. This result is only available for surface model occurrences and only if [`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) was enabled.

| Value | Description |
| --- | --- |
| `M_APPLIED` | Specifies that refine registration was applied successfully to the occurrence. |
| `M_APPLIED_AND_REJECTED` | Specifies that even though[`M_REFINE_REGISTRATION`](../../Reference/3dmod/M3dmodControl.md) was enabled, at least one registration was not applied successfully to the occurrence. |
| `M_NOT_APPLIED` | Specifies that refine registration was not enabled, so it was not applied to the occurrence. |

### Combination Constants — For specifying the box face's index

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify for which face of the box to retrieve results.

| Value | Description |
| --- | --- |
| `M_FACE_INDEX` | Specifies to retrieve results for a specific face of a box occurrence. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the requested information, cast to an _AIL_DOUBLE_. If the requested information does not fit into an _AIL_DOUBLE_, this function will return `M_NULL` or truncate the information.

This result is always available.
