---
doctype: Reference
module: 3dmap
function: M3dmapGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapGetResult"
---

# M3dmapGetResult

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

> Get the specified type of result(s) from a 3D alignment or 3D reconstruction result buffer.

## Syntax

```c
void M3dmapGetResult(
    AIL_ID    Result3dmapId,  //in
    AIL_INT   LabelOrIndex,   //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the result(s) of the specified type from a result buffer. For [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), and [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction result buffers, results are available after using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) to fill the result buffer with the extracted laser line data. For 3D alignment result buffers, results are available after using [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md).

An [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction result buffer contains an array of distinct point clouds. You can retrieve results from an individual point cloud in the result buffer, from an aggregate of all point clouds in the result buffer, or from the result buffer itself using the [`LabelOrIndex`](../../Reference/3dmap/M3dmapGetResult.md) parameter.

## Parameters

### `Result3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D alignment or 3D reconstruction result buffer from which to retrieve results.

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the point cloud(s) in the specified 3D reconstruction result buffer, or the entire result buffer itself, from which to get results. Only 3D reconstruction result buffers allocated using [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) have point clouds that can be specified using this parameter. For all other types of 3D reconstruction result buffers, set this parameter to `M_DEFAULT`.

*For specifying the point cloud(s) or point cloud container*

| Value | Description |
| --- | --- |
| `M_POINT_CLOUD_INDEX` | Specifies the array index of a point cloud in the specified 3D reconstruction result buffer. |
| `M_POINT_CLOUD_LABEL` | Specifies the label for a point cloud in the specified 3D reconstruction result buffer. |
| `M_ALL` | Specifies to get results about all point clouds in the specified 3D reconstruction result buffer. |
| `M_GENERAL` | Specifies to get results about the point cloud container (3D reconstruction result buffer allocated using [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md)). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to get.

### `ResultArrayPtr` *(out, *void)*

Specifies the address in which to write the results.

## Parameter Associations

### For retrieving results from a 3D reconstruction result buffer of type

To retrieve a result from an [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction result buffer, the [`ResultType`](../../Reference/3dmap/M3dmapGetResult.md) parameter can be set to one of the following values:

---

### `M_HAS_FEATURE`

Retrieves whether the specified point cloud(s) have the specified feature (such as position or intensity data). The feature is specified using the combination value.  When the [`LabelOrIndex`](../../Reference/3dmap/M3dmapGetResult.md) parameter is set to **M_POINT_CLOUD_INDEX()** or **M_POINT_CLOUD_LABEL()**, [`M_HAS_FEATURE`](../../Reference/3dmap/M3dmapGetResult.md) returns whether the specified point cloud has the feature.  When the [`LabelOrIndex`](../../Reference/3dmap/M3dmapGetResult.md) parameter is set to [`M_GENERAL`](../../Reference/3dmap/M3dmapGetResult.md), [`M_HAS_FEATURE`](../../Reference/3dmap/M3dmapGetResult.md) returns whether all the point clouds in the specified result buffer have the feature.  When the [`LabelOrIndex`](../../Reference/3dmap/M3dmapGetResult.md) parameter is set to [`M_ALL`](../../Reference/3dmap/M3dmapGetResult.md), [`M_HAS_FEATURE`](../../Reference/3dmap/M3dmapGetResult.md) returns an array where the value of each element in the array is whether the point cloud at the corresponding index has the feature.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the specified point cloud(s) do not have the feature. |
| `M_TRUE` | Specifies that the specified point cloud(s) do have the feature. |

---

### `M_NUMBER_OF_3D_POINTS`

Retrieves the number of 3D points extracted from laser line data and stored in the specified point cloud(s).  When the [`LabelOrIndex`](../../Reference/3dmap/M3dmapGetResult.md) parameter is set to **M_POINT_CLOUD_INDEX()** or **M_POINT_CLOUD_LABEL()**, [`M_NUMBER_OF_3D_POINTS`](../../Reference/3dmap/M3dmapGetResult.md) returns the number of points in the specified point cloud.  When the [`LabelOrIndex`](../../Reference/3dmap/M3dmapGetResult.md) parameter is set to [`M_GENERAL`](../../Reference/3dmap/M3dmapGetResult.md), [`M_NUMBER_OF_3D_POINTS`](../../Reference/3dmap/M3dmapGetResult.md) returns the total number of points in all the point clouds in the specified result buffer.  When the [`LabelOrIndex`](../../Reference/3dmap/M3dmapGetResult.md) parameter is set to [`M_ALL`](../../Reference/3dmap/M3dmapGetResult.md), [`M_NUMBER_OF_3D_POINTS`](../../Reference/3dmap/M3dmapGetResult.md) returns an array where the value of each element in the array is the number of points in the point cloud at the corresponding index.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of 3D points. |

---

### `M_TOTAL_DISPLACEMENT_Y`

Retrieves the total displacement along the Y-axis (typically the conveyor) from the first call to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).  If the value of [`M_SCAN_SPEED`](../../Reference/3dmap/M3dmapControl.md) ([`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md)) has remained constant since the last call to [`M3dmapClear`](../../Reference/3dmap/M3dmapClear.md), [`M_TOTAL_DISPLACEMENT_Y`](../../Reference/3dmap/M3dmapGetResult.md) is equivalent to [`M_SCAN_SPEED`](../../Reference/3dmap/M3dmapControl.md) x (number of calls to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md)-1).  > **Note:** [`M_TOTAL_DISPLACEMENT_Y`](../../Reference/3dmap/M3dmapGetResult.md) is only available when the [`LabelOrIndex`](../../Reference/3dmap/M3dmapGetResult.md) parameter is set to [`M_GENERAL`](../../Reference/3dmap/M3dmapGetResult.md).

### Combination Constants — For specifying the feature to check

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to determine whether the feature exists in the specified location.

| Value | Description |
| --- | --- |
| `M_INTENSITY` | Specifies to determine if intensity information exists in the specified point cloud(s). |
| `M_POSITION` | Specifies to determine if position information exists in the specified point cloud(s). |

### Combination Constants — For modifying the number of points returned

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify which 3D points should be included when returning the number of points.

| Value | Description |
| --- | --- |
| `M_EXCLUDE_INVALID_POINTS` | Specifies to exclude all points in the specified point cloud(s) that are set to **M_INVALID_POINT**. Only valid points are included in the calculation. |

### Combination Constants — For including the last scan only

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify to return the points of the last scan only.

| Value | Description |
| --- | --- |
| `M_LAST_SCAN` | Specifies to return only those points generated during the last call to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md). |

### For retrieving results from a 3D reconstruction result buffer of type

To retrieve a result about partially corrected depth maps from an [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction result buffer, the [`ResultType`](../../Reference/3dmap/M3dmapGetResult.md) parameter can be set to one of the following values:

---

### `M_PARTIALLY_CORRECTED_DEPTH_MAP_BUFFER_TYPE`

Retrieves the data type and depth that an image buffer should have to store the partially corrected depth map.

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 8` | Specifies that the image buffer should be an 8-bit unsigned buffer. |
| `M_UNSIGNED + 16` | Specifies that the image buffer should be a 16-bit unsigned buffer. |

---

### `M_PARTIALLY_CORRECTED_DEPTH_MAP_SIZE_X`

Retrieves the X-size, in pixels, that an image buffer should have to store the partially corrected depth map.  If the [`MimControl`](../../Reference/im/MimControl.md) [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) control type setting is set to [`M_VERTICAL`](../../Reference/im/MimControl.md), this is equal to the X-size of the image buffer passed to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).  If the [`MimControl`](../../Reference/im/MimControl.md) [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) control type setting is set to [`M_HORIZONTAL`](../../Reference/im/MimControl.md), this is equal to the Y-size of the image buffer passed to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).

---

### `M_PARTIALLY_CORRECTED_DEPTH_MAP_SIZE_Y`

Retrieves the Y-size, in pixels, that an image buffer should have to store the partially corrected depth map.  This is equal to the number of times [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) was called or the value of the [`M_MAX_FRAMES`](../../Reference/3dmap/M3dmapControl.md) control, whichever is smaller.

### For retrieving results from a 3D reconstruction result buffer of type

To retrieve a result from a 3D reconstruction result buffer of type [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) or [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), or from an individual point cloud, the [`ResultType`](../../Reference/3dmap/M3dmapGetResult.md) parameter can be set to one of the following values:

---

### `M_CAMERA_IMAGE_SIZE_X`

Retrieves the X-size of the image buffer from which the last laser line was extracted using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).

---

### `M_CAMERA_IMAGE_SIZE_Y`

Retrieves the Y-size of the image buffer from which the last laser line was extracted using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).

---

### `M_INTENSITY_MAP_BUFFER_TYPE`

Retrieves the data type and depth that an image buffer should have to store the intensity map.  The returned depth and data type are the same as those of the image buffer, passed to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md), containing the grabbed image of the laser line.

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 8` | Specifies that the image buffer should be an 8-bit unsigned buffer. |
| `M_UNSIGNED + 16` | Specifies that the image buffer should be a 16-bit unsigned buffer. |

---

### `M_NUMBER_OF_MISSING_DATA_LAST_SCAN`

Retrieves the number of points (pixels) with unknown values in the last laser line extracted using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).

---

### `M_SCAN_LANE_DIRECTION`

Retrieves whether laser line detection was performed vertically or horizontally.

---

### `M_UNCORRECTED_DEPTH_MAP_BUFFER_TYPE`

Retrieves the data type and depth that an image buffer should have to store the uncorrected depth map.

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 8` | Specifies that the image buffer should be an 8-bit unsigned buffer. |
| `M_UNSIGNED + 16` | Specifies that the image buffer should be a 16-bit unsigned buffer. |

---

### `M_UNCORRECTED_DEPTH_MAP_FIXED_POINT`

Retrieves the number of fractional bits used for the uncorrected depth map.

---

### `M_UNCORRECTED_DEPTH_MAP_SIZE_X`

Retrieves the X-size, in pixels, that an image buffer should have to store the uncorrected depth map.  > **Note:** Note that [`M_UNCORRECTED_DEPTH_MAP_SIZE_X`](../../Reference/3dmap/M3dmapGetResult.md) is also the X-size for storing the intensity map.

---

### `M_UNCORRECTED_DEPTH_MAP_SIZE_Y`

Retrieves the Y-size, in pixels, that an image buffer should have to store the uncorrected depth map.  > **Note:** Note that [`M_UNCORRECTED_DEPTH_MAP_SIZE_Y`](../../Reference/3dmap/M3dmapGetResult.md) is also the Y-size for storing the intensity map.

### For retrieving results from a 3D alignment result buffer.

To retrieve a result from a 3D alignment result buffer, the [`ResultType`](../../Reference/3dmap/M3dmapGetResult.md) parameter can be set to one of the following values:  > **Note:** Note that aside from [`M_STATUS`](../../Reference/3dmap/M3dmapGetResult.md), these result types are only available after a successfully completed alignment operation (when[`M_STATUS`](../../Reference/3dmap/M3dmapGetResult.md) is [`M_COMPLETE`](../../Reference/3dmap/M3dmapGetResult.md)) or if a status warning is returned (when [`M_STATUS`](../../Reference/3dmap/M3dmapGetResult.md) is [`M_RESULTS_Z_NEED_BACKGROUND`](../../Reference/3dmap/M3dmapGetResult.md), [`M_MAYBE_IMPRECISE`](../../Reference/3dmap/M3dmapGetResult.md), or [`M_CHAMFER_NOT_FOUND`](../../Reference/3dmap/M3dmapGetResult.md)).

---

### `M_3D_SCALE_Y`

Retrieves the Y-scaling factor necessary to correct the Y-coordinates of the point clouds.  > **Note:** Scale distortions in the Y-direction cause shrinking or stretching of the object scanned by the 3D profile sensor.

---

### `M_3D_SHEAR_X`

Retrieves by how much to offset the X-coordinates of the point cloud from the X-coordinates of its previous row to correct the shear in X. Using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md), you can set [`M_3D_SHEAR_X`](../../Reference/buf/MbufControlContainer.md) of the range component to this value, prior to calling [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), to correct the shear in X.  > **Note:** Shear distortions in the X-direction cause skewing of the object scanned by the 3D profile sensor.

---

### `M_3D_SHEAR_Z`

Retrieves by how much to offset the Z-coordinates of the point cloud from the Z-coordinates of its previous row to correct the shear in Z. Using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md), you can set [`M_3D_SHEAR_Z`](../../Reference/buf/MbufControlContainer.md) of the range component to this value, prior to calling [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), to correct the shear in Z.  > **Note:** Shear distortions in the Z-direction cause inaccurate object thickness scanned by the 3D profile sensor.

---

### `M_HOLES_FOUND`

Retrieves the detection status of the two holes on the alignment disk. This result verifies that the alignment disk's holes have been taken into consideration during the alignment operation.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_DISK`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the two holes are not detected on the alignment disk. The two holes have not been taken into consideration during the alignment operation.  > **Note:** Note that if the two holes are not detected on the alignment disk, [`M_Y_MIRRORED`](../../Reference/3dmap/M3dmapGetResult.md) will not be correct. |
| `M_TRUE` | Specifies that the two holes are detected on the alignment disk. The two holes have been taken into consideration during the alignment operation. |

---

### `M_PYRAMID_CORNERS_X`

Retrieves the uncorrected X-coordinate for each of the corners of the truncated pyramid on the alignment pyramid. The order in which the uncorrected X-coordinates are returned is not guaranteed; however, the results are returned uniformly across the [`M_PYRAMID_CORNERS_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_PYRAMID_CORNERS_Y`](../../Reference/3dmap/M3dmapGetResult.md), and [`M_PYRAMID_CORNERS_Z`](../../Reference/3dmap/M3dmapGetResult.md) result types.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md).

---

### `M_PYRAMID_CORNERS_Y`

Retrieves the uncorrected Y-coordinate for each of the corners of the truncated pyramid on the alignment pyramid. The order in which the uncorrected Y-coordinates are returned is not guaranteed, however the results are returned uniformly across the [`M_PYRAMID_CORNERS_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_PYRAMID_CORNERS_Y`](../../Reference/3dmap/M3dmapGetResult.md), and [`M_PYRAMID_CORNERS_Z`](../../Reference/3dmap/M3dmapGetResult.md) result types.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md).

---

### `M_PYRAMID_CORNERS_Z`

Retrieves the uncorrected Z-coordinate for each of the corners of the truncated pyramid on the alignment pyramid. The order in which the uncorrected Z-coordinates are returned is not guaranteed, however the results are returned uniformly across the [`M_PYRAMID_CORNERS_X`](../../Reference/3dmap/M3dmapGetResult.md), [`M_PYRAMID_CORNERS_Y`](../../Reference/3dmap/M3dmapGetResult.md), and [`M_PYRAMID_CORNERS_Z`](../../Reference/3dmap/M3dmapGetResult.md) result types.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md).

---

### `M_RMS_ERROR`

Retrieves the root-mean-square (RMS) error. The RMS error is calculated using the distance measured between the alignment pyramid's aligned corners and their target corners.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md).

---

### `M_SENSOR_PITCH`

Retrieves the angle of the pitch (the angle of rotation around the X-axis) of the 3D profile sensor, in degrees; this is the pitch angle if the required rotation around the Z-axis is applied ([`M_SENSOR_YAW_BEFORE_PITCH`](../../Reference/3dmap/M3dmapGetResult.md)) before applying a rotation around the X-axis.  > **Note:** This is not the angle necessary to remove the shear. To retrieve the angle of the pitch of the 3D profile sensor, necessary to remove shear in the Z-direction, use [`M_3D_SHEAR_Z`](../../Reference/3dmap/M3dmapGetResult.md).

---

### `M_SENSOR_PITCH_BEFORE_YAW`

Retrieves the angle of the pitch (the angle of rotation around the X-axis) of the 3D profile sensor, in degrees; this is the pitch angle if the rotation around the X-axis is applied before applying the required rotation around the Z-axis ([`M_SENSOR_YAW`](../../Reference/3dmap/M3dmapGetResult.md)).  > **Note:** This is not the angle necessary to remove the shear. To retrieve the angle of the pitch of the 3D profile sensor, necessary to remove shear in the Z-direction, use [`M_3D_SHEAR_Z`](../../Reference/3dmap/M3dmapGetResult.md).

---

### `M_SENSOR_YAW`

Retrieves the angle of the yaw (the angle of rotation around the Z-axis) of the 3D profile sensor, in degrees; this is the yaw angle if the required rotation around the X-axis is applied ([`M_SENSOR_PITCH_BEFORE_YAW`](../../Reference/3dmap/M3dmapGetResult.md)) before applying a rotation around the Z-axis.  > **Note:** This is not the angle necessary to remove the shear. To retrieve the angle of the yaw of the 3D profile sensor, necessary to remove shear in the X-direction, use [`M_3D_SHEAR_X`](../../Reference/3dmap/M3dmapGetResult.md).

---

### `M_SENSOR_YAW_BEFORE_PITCH`

Retrieves the angle of the yaw (the angle of rotation around the Z-axis) of the 3D profile sensor, in degrees; this is the yaw angle if the rotation around the Z-axis is applied before applying the required rotation around the X-axis ([`M_SENSOR_PITCH`](../../Reference/3dmap/M3dmapGetResult.md)).  > **Note:** This is not the angle necessary to remove the shear. To retrieve the angle of the yaw of the 3D profile sensor, necessary to remove shear in the X-direction, use [`M_3D_SHEAR_X`](../../Reference/3dmap/M3dmapGetResult.md).

---

### `M_STATUS`

Retrieves the status of the alignment operation. This result is always available for retrieval.

| Value | Description |
| --- | --- |
| `M_ALL_POINTS_COLLINEAR` | Specifies that the alignment operation failed. This occurs when all the points found create a line. |
| `M_CHAMFER_NOT_FOUND` | Specifies that the alignment pyramid's chamfer was not found. When this occurs, the step direction (the sign of [`M_3D_SCALE_Y`](../../Reference/3dmap/M3dmapGetResult.md)) could be wrong and/or the rotation around the Z-axis might be off by a factor of 90 degrees.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md). |
| `M_COMPLETE` | Specifies that the alignment operation completed successfully. |
| `M_DISK_EDGES_NOT_FOUND` | Specifies that the alignment disk's edges could not be found. This occurs when the disk is clipped during the scan, or the edges of the disk were not clear enough to determine the start and end of the disk.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_DISK`](../../Reference/3dmap/M3dmapControl.md). |
| `M_DISK_NOT_FOUND` | Specifies that the alignment disk could not be found. This occurs when the scan is too noisy for the disk to be found.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_DISK`](../../Reference/3dmap/M3dmapControl.md). |
| `M_INVALID_STEP_LENGTH` | Specifies that the step length is invalid. This occurs when the user-defined step length is set to an invalid value. |
| `M_MATHEMATICAL_EXCEPTION` | Specifies that the alignment pyramid could not converge to a feasible alignment solution. This occurs when the optimizer throws an exception. This can be resolved by properly following the design constraints of the alignment pyramid, or by setting [`M_STEP_LENGTH_MODE`](../../Reference/3dmap/M3dmapControl.md) to a value that is not [`M_UNKNOWN`](../../Reference/3dmap/M3dmapControl.md).  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md). |
| `M_MAYBE_IMPRECISE` | Specifies that the alignment pyramid is not precise enough (scan resolution). This typically occurs when the chamfer is not detected. This can be resolved by properly following the design constraints of the alignment pyramid, and ensuring that the chamfer is visible in the scan.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md). |
| `M_NOT_ENOUGH_VALID_DATA` | Specifies that the alignment operation failed. This occurs when there are not enough valid points. |
| `M_NOT_INITIALIZED` | Specifies that the alignment result buffer was not used in a call to[`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md), and contains no results. |
| `M_PYRAMID_NOT_FOUND` | Specifies that the alignment pyramid could not be found. This occurs when its truncated pyramid's faces or corners are not found.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_PYRAMID`](../../Reference/3dmap/M3dmapControl.md). |
| `M_RESULTS_Z_NEED_BACKGROUND` | Specifies that the required background was not found. This occurs when the background is outside the FOV of the 3D profile sensor. In this case, the background will be estimated with respect to [`M_CAMERA_TILT_X`](../../Reference/3dmap/M3dmapControl.md) set to [`M_ZERO`](../../Reference/3dmap/M3dmapControl.md).  > **Note:** Note that in this case,[`M_3D_SHEAR_Z`](../../Reference/3dmap/M3dmapGetResult.md) and[`M_SENSOR_PITCH`](../../Reference/3dmap/M3dmapGetResult.md) will not be accurate. The presence of a background is also required for [`M_ALIGN_Z_DIRECTION`](../../Reference/3dmap/M3dmapControl.md), [`M_ALIGN_Z_POSITION`](../../Reference/3dmap/M3dmapControl.md), and [`M_CAMERA_TILT_X`](../../Reference/3dmap/M3dmapControl.md) in [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md). |

---

### `M_STEP_LENGTH`

Retrieves the norm of the calculated step vector. This vector will include shear in the X and Z-axes, and scale in the Y-axis if they are present.  When the 3D profile sensor is parallel to the plane of motion (conveyor) and has no shear in the X or Z-axes, the step length will be the distance between scan lines in the Y-direction.

---

### `M_Y_MIRRORED`

Retrieves whether the alignment will cause a mirroring of the Y-axis. If [`M_HOLES_FOUND`](../../Reference/3dmap/M3dmapGetResult.md) returns [`M_FALSE`](../../Reference/3dmap/M3dmapGetResult.md) and the two holes are not detected on the alignment disk, the result of [`M_Y_MIRRORED`](../../Reference/3dmap/M3dmapGetResult.md) will be incorrect.  > **Note:** This result is only available when [`M_OBJECT_SHAPE`](../../Reference/3dmap/M3dmapControl.md) is [`M_DISK`](../../Reference/3dmap/M3dmapControl.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the Y-axis is not mirrored. |
| `M_TRUE` | Specifies that the Y-axis is mirrored.  > **Note:** After alignment, the scan will be reflected around the Y-axis. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

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

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.
