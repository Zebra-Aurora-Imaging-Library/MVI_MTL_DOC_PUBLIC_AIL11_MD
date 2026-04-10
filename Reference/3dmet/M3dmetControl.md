---
doctype: Reference
module: 3dmet
function: M3dmetControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetControl"
---

# M3dmetControl

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

> Control a setting for a 3D metrology context or result buffer.

## Syntax

```c
void M3dmetControl(
    AIL_ID     ContextOrResult3dmetId,  //out
    AIL_INT64  ControlType,             //in
    AIL_DOUBLE ControlValue             //in
)
```

## Description

This function allows you to control a setting of a 3D metrology context or result buffer. You can typically inquire these settings using [`M3dmetInquire`](../../Reference/3dmet/M3dmetInquire.md).

To control draw 3D metrology settings for [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md), use [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) instead.

## Parameters

### `ContextOrResult3dmetId` *(out, AIL_ID)*

Specifies the identifier of the 3D metrology context or result buffer to control. The 3D metrology context or result buffer must have been previously allocated on the required system using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) or [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md), respectively.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value needed for the control.

## Parameter Associations

### To control a statistics 3D metrology context

The following [`ControlType`](../../Reference/3dmet/M3dmetControl.md) and [`ControlValue`](../../Reference/3dmet/M3dmetControl.md) parameter settings can be specified for statistics 3D metrology contexts, and control which statistics [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md) calculates.

---

### `M_STAT_MAX`

Sets whether to calculate the maximum distance between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the maximum distance. |
| `M_ENABLE` | Specifies to calculate the maximum distance. |

---

### `M_STAT_MAX_ABS`

Sets whether to calculate the maximum absolute distance between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the maximum absolute distance. |
| `M_ENABLE` | Specifies to calculate the maximum absolute distance. |

---

### `M_STAT_MEAN`

Sets whether to calculate the average distance between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the average distance. |
| `M_ENABLE` | Specifies to calculate the average distance. |

---

### `M_STAT_MEAN_ABS`

Sets whether to calculate the average absolute distance between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the average absolute distance. |
| `M_ENABLE` | Specifies to calculate the average absolute distance. |

---

### `M_STAT_MIN`

Sets whether to calculate the minimum distance between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the minimum distance. |
| `M_ENABLE` | Specifies to calculate the minimum distance. |

---

### `M_STAT_MIN_ABS`

Sets whether to calculate the minimum absolute distance between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the minimum absolute distance. |
| `M_ENABLE` | Specifies to calculate the minimum absolute distance. |

---

### `M_STAT_NUMBER`

Sets whether to record the number of points that meet the condition specified when calling [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md) (using the [`Condition`](../../Reference/3dmet/M3dmetStat.md), [`CondLow`](../../Reference/3dmet/M3dmetStat.md) and [`CondHigh`](../../Reference/3dmet/M3dmetStat.md) parameters).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to record the number of points that meet the specified condition. |
| `M_ENABLE` | Specifies to record the number of points that meet the specified condition. |

---

### `M_STAT_RMS`

Sets whether to calculate the root-mean-square (RMS) error between the point cloud or depth map, and the specified reference object. Aurora Imaging Library calculates the RMS error using the following formula:  *[Image: 3dreg_RootMeanSquareError_eq.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the RMS error. |
| `M_ENABLE` | Specifies to calculate the RMS error. |

---

### `M_STAT_STANDARD_DEVIATION`

Sets whether to calculate the standard deviation of all the distances calculated between the point cloud or depth map, and the specified reference object. Aurora Imaging Library calculates the standard deviation using the following formula:  *[Image: 3dmet_StandardDeviation_eq.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the standard deviation. |
| `M_ENABLE` | Specifies to calculate the standard deviation. |

---

### `M_STAT_SUM`

Sets whether to calculate the sum of all the distances calculated between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the sum of all the distances. |
| `M_ENABLE` | Specifies to calculate the sum of all the distances. |

---

### `M_STAT_SUM_ABS`

Sets whether to calculate the sum of all the absolute distances calculated between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the sum of all absolute distances. |
| `M_ENABLE` | Specifies to calculate the sum of all absolute distances. |

---

### `M_STAT_SUM_OF_SQUARES`

Sets whether to calculate the sum of squared distances between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the sum of squared distances. |
| `M_ENABLE` | Specifies to calculate the sum of squared distances. |

### To control a fit 3D metrology context

The following [`ControlType`](../../Reference/3dmet/M3dmetControl.md) and [`ControlValue`](../../Reference/3dmet/M3dmetControl.md) parameter settings can be specified for a fit 3D metrology context, and control how [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) performs the fit operation.

---

### `M_ESTIMATION_MODE`

Sets how to compute an initial fit estimate between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FROM_GEOMETRY` | Specifies that an initial fit estimate is determined using a geometry object that is copied into the fit 3D metrology context using [`M3dmetCopy`](../../Reference/3dmet/M3dmetCopy.md) with [`M_ESTIMATE_GEOMETRY`](../../Reference/3dmet/M3dmetCopy.md).  The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. Supported 3D geometries include cylinder, line, plane, and sphere. Note that fitting a 3D box geometry is not supported. |
| `M_NO_SAMPLING` | Specifies that an initial fit estimate is calculated using all available points in a point cloud or depth map.  It is recommended that you only use this mode when there is a low percentage of outliers among the points of the point cloud or depth map. |
| `M_RANDOM_SAMPLING` *(default)* | Specifies that an initial fit estimate is determined using a random sampling consensus (RANSAC) algorithm.  This mode tries to find the best fit of a 3D geometry object to a point cloud by trying to fit the reference object to several random samplings of the point cloud or depth map, and uses the best fit (with the lowest root-mean-squared (RMS) error) as the best initial approximation.  Random sampling consensus assumes that the best fit of the Aurora Imaging Library object will use the random sampling of points that contains the fewest outliers. Using an initial fit estimate that ignores outliers will prevent [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) from fitting the Aurora Imaging Library object incorrectly.  To optimize the likelihood of a good initial fit estimate, you can specify a value for [`M_EXPECTED_OUTLIER_PERCENTAGE`](../../Reference/3dmet/M3dmetControl.md). |

---

### `M_EXPECTED_OUTLIER_PERCENTAGE`

Sets the expected percentage of outliers among the points of the point cloud or depth map to be fitted.  Note, this is only used for [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) operations with [`OutlierDistance`](../../Reference/3dmet/M3dmetFit.md) set to [`M_AUTO_VALUE`](../../Reference/3dmet/M3dmetFit.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 100.0` *(default)* | Specifies the expected percentage of outliers among the points of the point cloud or depth map to be fitted. |

---

### `M_FIT_ITERATIONS_MAX`

Sets the maximum number of iterations to use during the fit operation.  If this value is set to 0, [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) returns the result of the initial fit estimate, as specified using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with[`M_ESTIMATION_MODE`](../../Reference/3dmet/M3dmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number of iterations to use during the fit operation. |

---

### `M_INLIER_AMOUNT_THRESHOLD`

Sets a minimum number of inliers required for the fit operation to end.  After each iteration of the fit operation, the total number of inliers and the RMS error are calculated. If the number of inliers exceeds [`M_INLIER_AMOUNT_THRESHOLD`](../../Reference/3dmet/M3dmetControl.md), and the average RMS error is less than [`M_RMS_ERROR_THRESHOLD`](../../Reference/3dmet/M3dmetControl.md), the fit operation will stop.  A point is considered an inlier when its distance from the reference geometry is less than the value passed to the[`OutlierDistance`](../../Reference/3dmet/M3dmetFit.md) parameter when [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md)is called. The status of the fit operation can be retrieved using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md) with [`M_STATUS_FIT`](../../Reference/3dmet/M3dmetGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that there is no minimum number of inliers. |
| `Value >= 0` | Specifies the minimum number of inliers. |

---

### `M_RMS_ERROR_THRESHOLD`

Sets the maximum RMS error required for the fit operation to end.  After each iteration of the fit operation, the average RMS error is calculated. If the average RMS error is less than [`M_RMS_ERROR_THRESHOLD`](../../Reference/3dmet/M3dmetControl.md), and the number of inliers exceeds [`M_INLIER_AMOUNT_THRESHOLD`](../../Reference/3dmet/M3dmetControl.md), the fit operation will stop.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the maximum RMS error. |

### To control a volume 3D metrology context

The following [`ControlType`](../../Reference/3dmet/M3dmetControl.md) and [`ControlValue`](../../Reference/3dmet/M3dmetControl.md) parameter settings can be specified for volume 3D metrology contexts, and control how [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) performs the volume calculation.

---

### `M_SAVE_VOLUME_INFO`

Sets whether to save information from the volume computation for use with [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) or [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md) operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` *(default)* | Specifies that the volume information is not saved. |
| `M_TRUE` | Specifies that the volume information is saved. |

---

### `M_VOLUME_MODE`

Sets how to calculate the total volume, given the position of the source Aurora Imaging Library object relative to the reference object.  When you specify [`M_COMPLETE`](../../Reference/3dmet/M3dmetControl.md), all volume results will be available to retrieve, copy, or draw. Otherwise, only the specified volume (for example, [`M_ABOVE`](../../Reference/3dmet/M3dmetControl.md)) will be available. Use [`M_VOLUME_OUTPUT_MODE`](../../Reference/3dmet/M3dmetControl.md) to set which result(s) to retrieve, copy, or draw.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABOVE` | Specifies that the total volume is equal only to the volume above the reference Aurora Imaging Library object. Any volume below the reference Aurora Imaging Library object is ignored. |
| `M_DIFFERENCE` | Specifies that the total volume is calculated as the volume above the reference Aurora Imaging Library object, minus the volume below the reference Aurora Imaging Library object. |
| `M_TOTAL` *(default)* | Specifies that the total volume is calculated as the sum of all volumes between the source and reference Aurora Imaging Library objects, whether above or below the reference Aurora Imaging Library object. |
| `M_UNDER` | Specifies that the total volume is equal only to the volume under the reference Aurora Imaging Library object. Any volume above the reference Aurora Imaging Library object is ignored. |
| `M_COMPLETE` *(default)* | Specifies to calculate all volume results, which can then be retrieved, copied, or drawn, according to the specified [`M_VOLUME_OUTPUT_MODE`](../../Reference/3dmet/M3dmetControl.md). |

### To control a calculate 3D metrology result buffer that holds results of an M3dmetVolumeEx operation

The following [`ControlType`](../../Reference/3dmet/M3dmetControl.md) and [`ControlValue`](../../Reference/3dmet/M3dmetControl.md) parameter settings can be specified for a calculate 3D metrology result buffer that holds results of an [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation.

---

### `M_VOLUME_OUTPUT_MODE`

Sets the volume mode for which to retrieve, copy, or draw results, using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md), [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md), or [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md), respectively, when [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_COMPLETE`](../../Reference/3dmet/M3dmetControl.md).  If you have set [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) to a single volume statistic (that is, not [`M_COMPLETE`](../../Reference/3dmet/M3dmetControl.md)), then you can leave this control type at its default setting ([`M_AUTO`](../../Reference/3dmet/M3dmetControl.md)), which means that the output mode will automatically match that of [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md).  If [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_COMPLETE`](../../Reference/3dmet/M3dmetControl.md), any of the volume output modes are available. [`M_AUTO`](../../Reference/3dmet/M3dmetControl.md) uses [`M_TOTAL`](../../Reference/3dmet/M3dmetControl.md).  > **Note:** This control type is ignored when no reference object was specified and the volume was calculated for a mesh without holes, since the result is always the total volume.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABOVE` | Specifies that the total volume is equal only to the volume above the reference Aurora Imaging Library object. Any volume below the reference Aurora Imaging Library object is ignored. |
| `M_DIFFERENCE` | Specifies that the total volume is calculated as the volume above the reference Aurora Imaging Library object, minus the volume below the reference Aurora Imaging Library object. |
| `M_TOTAL` *(default)* | Specifies that the total volume is calculated as the sum of all volumes between the source and reference Aurora Imaging Library objects, whether above or below the reference Aurora Imaging Library object. |
| `M_UNDER` | Specifies that the total volume is equal only to the volume under the reference Aurora Imaging Library object. Any volume above the reference Aurora Imaging Library object is ignored. |
| `M_AUTO` *(default)* | Specifies to match the [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) setting, unless [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_COMPLETE`](../../Reference/3dmet/M3dmetControl.md), in which case [`M_TOTAL`](../../Reference/3dmet/M3dmetControl.md) is used. |

### To control a distance 3d metrology context

The following [`ControlType`](../../Reference/3dmet/M3dmetControl.md) and [`ControlValue`](../../Reference/3dmet/M3dmetControl.md) parameter settings can be specified for distance 3D metrology contexts, and control how [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) performs the distance calculation.

---

### `M_DISTANCE_METRIC_OPERATOR`

Sets the operator applied to the distance from [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md). See [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) for the supported settings for [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) given the setting of [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md), [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md), and the reference object. Note that if this control type can only be set to one setting, and you specify an unavailable setting, it will be ignored and the appropriate setting will be used instead. When more than one setting is available, and an unavailable setting is specified, an error is generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` *(default)* | Specifies to use an absolute metric when calculating the distance.  The returned distance is always positive. |
| `M_SIGNED` | Specifies to use a signed metric when calculating the distance.  The returned distance can be either positive or negative. |
| `M_SQUARED` | Specifies to use a squared metric when calculating the distance.  The returned value is the squared distance. |

---

### `M_DISTANCE_NORMAL`

Sets whether to compute the normal distance as the dot product between the paired source point's normal and the reference point's normal.  To compute the normal distance, the source must be a point cloud container with an[`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufControlContainer.md) component. In addition, only the following [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) settings are supported and must be used with the following reference objects:  |   |   | | --- | --- | | [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) | Reference object | | [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetControl.md) | Sphere, box, cylinder, plane, or a point cloud with an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufControlContainer.md) component. | | [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md) | Point cloud with an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufControlContainer.md) component. |  If these conditions are not met, an error is thrown.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not compute the normal distance. |
| `M_ENABLE` | Specifies to compute the normal distance. |

---

### `M_PAIRING_DISTANCE_MAX`

Sets the maximum distance between a source point and a reference point to be paired. Distances larger than this are invalid.  Note, this is only used when the reference object is a depth map or a point cloud.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no maximum distance. |
| `Value > 0.0` | Specifies the maximum distance. |

---

### `M_PAIRING_DISTANCE_METRIC`

Sets the metric used when choosing a source point's paired reference element. The paired reference element is that which is closest to the source point using the specified metric.  Note, if the reference object is a depth map, only [`M_DIRECTION_Z`](../../Reference/3dmet/M3dmetControl.md) is available. If another option is chosen, it will be ignored and [`M_DIRECTION_Z`](../../Reference/3dmet/M3dmetControl.md) is still used. See [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) for the supported settings for [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) given the setting of [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md), [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md), and the reference object. Note that if this control type can only be set to one setting, and you specify an unavailable setting, it will be ignored and the appropriate setting will be used instead. When more than one setting is available, and an unavailable setting is specified, an error is generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically choose the metric based on the reference object.  If the reference object is a depth map, [`M_AUTO`](../../Reference/3dmet/M3dmetControl.md) is the same as[`M_DIRECTION_Z`](../../Reference/3dmet/M3dmetControl.md). For all other reference objects, [`M_AUTO`](../../Reference/3dmet/M3dmetControl.md) is the same as[`M_EUCLIDEAN`](../../Reference/3dmet/M3dmetControl.md). |
| `M_DIRECTION_NORMAL` | Specifies to use the distance in the normal direction of the source point. Note, the source container must be a point cloud container with an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufControlContainer.md) component or an error is thrown. |
| `M_DIRECTION_Z` | Specifies to use the distance in the Z-direction. |
| `M_EUCLIDEAN` | Specifies to use the Euclidean distance. |
| `M_MANHATTAN` | Specifies to use the Manhattan distance. |

---

### `M_PAIRING_PERPENDICULAR_DISTANCE_MAX`

Sets the maximum perpendicular distance between a source point and a reference point to be paired. Distances greater than this are invalid.  Note, this setting is only used when [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md)is set to [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md), [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_DIRECTION_NORMAL`](../../Reference/3dmet/M3dmetControl.md), and [`M_PAIRING_PERPENDICULAR_DISTANCE_MAX_MODE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmet/M3dmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the maximum perpendicular distance. |

---

### `M_PAIRING_PERPENDICULAR_DISTANCE_MAX_MODE`

Sets whether an automatic value or a user-defined value is used for the maximum perpendicular distance between a source point and a reference point to be paired.  > **Note:** Note, this setting is only used when [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md)is set to [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md), [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_DIRECTION_NORMAL`](../../Reference/3dmet/M3dmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies an automatic value. The automatic value is chosen based on the density of the point cloud. |
| `M_USER_DEFINED` | Specifies to use the user-defined value specified using [`M_PAIRING_PERPENDICULAR_DISTANCE_MAX`](../../Reference/3dmet/M3dmetControl.md). |

---

### `M_PAIRING_TYPE`

Sets against which part of the reference object to measure when performing an [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) operation.  All possible combinations of reference object, [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md), and [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) are listed below each [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md). When only one setting is available for [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) or [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md), if an unavailable setting is specified, it will be ignored. For example, If you specify a box geometry with [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) set to [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetControl.md), [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) set to [`M_MANHATTAN`](../../Reference/3dmet/M3dmetControl.md) and the [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) set to [`M_SIGNED`](../../Reference/3dmet/M3dmetControl.md), [`M_SIGNED`](../../Reference/3dmet/M3dmetControl.md) is ignored and [`M_ABSOLUTE`](../../Reference/3dmet/M3dmetControl.md)is used instead. When more than one setting is available, and an unavailable setting is specified, an error is thrown.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | [`M_AUTO`](../../Reference/3dmet/M3dmetControl.md) is the same as the first available setting: [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetControl.md), [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md), [`M_DISTANCE_TO_REFERENCE`](../../Reference/3dmet/M3dmetControl.md) in decreasing preference. |
| `M_DISTANCE_TO_NEIGHBOR` | Specifies to calculate the distance between the points of the source point cloud or depth map, and their nearest point in the specified reference point cloud or depth map.  Note, for this pairing type, only the following reference objects are supported and must be used with the following settings:  |   |   |   | | --- | --- | --- | | Reference object. | [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) | [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) | | Depth map. | [`M_DIRECTION_Z`](../../Reference/3dmet/M3dmetControl.md) | [`M_ABSOLUTE`](../../Reference/3dmet/M3dmetControl.md) or [`M_SIGNED`](../../Reference/3dmet/M3dmetControl.md). | | Point cloud container. | [`M_EUCLIDEAN`](../../Reference/3dmet/M3dmetControl.md) or [`M_DIRECTION_NORMAL`](../../Reference/3dmet/M3dmetControl.md). | [`M_ABSOLUTE`](../../Reference/3dmet/M3dmetControl.md) | |
| `M_DISTANCE_TO_REFERENCE` | Specifies to calculate the distance between the points of the source point cloud or depth map and the specified reference 3D geometry.  The paired reference point depends on the type of 3D geometry object specified:  - Sphere:   - Calculates the pairing distance to the center of the sphere. - Point:   - Calculates the pairing distance to the point. - Cylinder:   - Calculates the pairing distance to the nearest position on the central axis of the cylinder. The central axis is extended infinitely, even if the cylinder is finite. - Line:   - Calculates the pairing distance to the nearest position on the line. Unlike the central axis of the cylinder, a finite line is not extended infinitely for this calculation.  [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) must be set to [`M_EUCLIDEAN`](../../Reference/3dmet/M3dmetControl.md). [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) can be set to [`M_ABSOLUTE`](../../Reference/3dmet/M3dmetControl.md) or [`M_SQUARED`](../../Reference/3dmet/M3dmetControl.md). |
| `M_DISTANCE_TO_SURFACE` | Specifies to calculate the distance between the points of the source point cloud or depth map, and the surface of the specified reference object.  Note, for this pairing type, only the following reference objects are supported and must be used with the following settings:  |   |   |   | | --- | --- | --- | | Reference object. | [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetControl.md) | [`M_DISTANCE_METRIC_OPERATOR`](../../Reference/3dmet/M3dmetControl.md) | | Sphere. | [`M_EUCLIDEAN`](../../Reference/3dmet/M3dmetControl.md) | [`M_ABSOLUTE`](../../Reference/3dmet/M3dmetControl.md) or [`M_SIGNED`](../../Reference/3dmet/M3dmetControl.md). | | Plane, box, or cylinder. | [`M_EUCLIDEAN`](../../Reference/3dmet/M3dmetControl.md) | [`M_ABSOLUTE`](../../Reference/3dmet/M3dmetControl.md) or [`M_SQUARED`](../../Reference/3dmet/M3dmetControl.md). | | Plane. | [`M_DIRECTION_Z`](../../Reference/3dmet/M3dmetControl.md) | [`M_ABSOLUTE`](../../Reference/3dmet/M3dmetControl.md) or [`M_SIGNED`](../../Reference/3dmet/M3dmetControl.md). | | Box. | [`M_MANHATTAN`](../../Reference/3dmet/M3dmetControl.md) | [`M_ABSOLUTE`](../../Reference/3dmet/M3dmetControl.md) | | Point cloud container. | [`M_EUCLIDEAN`](../../Reference/3dmet/M3dmetControl.md) or [`M_DIRECTION_NORMAL`](../../Reference/3dmet/M3dmetControl.md). | [`M_ABSOLUTE`](../../Reference/3dmet/M3dmetControl.md) | |

---

### `M_SAVE_PAIRED_REFERENCE_FACET`

Sets whether to save the index of the reference facet (face) paired to each source point. This control type is only available when [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetControl.md), and the reference object is a geometry with multiple facets (for example, a box has 6 facets) or a meshed point cloud.  If the reference object is a meshed point cloud, the index is the index of the paired mesh triangle of the [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufControlContainer.md) component.  If the reference object is a box, the surface index indicates the face of the box. The sides are indexed as follows:  *[Image: box_faces.png]*  If the reference object is a finite cylinder, index 0 is the start face, index 1 is the end face, and index 2 is the cylindrical face.  Note, setting is only available if [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetControl.md) and the reference object is a box, finite cylinder, or point cloud; otherwise, an error is thrown.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not save the paired reference facet. |
| `M_ENABLE` | Specifies to save the paired reference facet. |

---

### `M_SAVE_PAIRED_REFERENCE_PIXEL`

Sets whether to save the index of the paired reference pixel to each source point.  Note, this setting is only available if [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md); otherwise, an error is thrown.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not save the paired reference pixel. |
| `M_ENABLE` | Specifies to save the paired reference pixel. |

---

### `M_SAVE_PAIRED_REFERENCE_POINT`

Sets whether to save the coordinates of the paired reference point to each source point.  Note, this setting is only available if [`M_PAIRING_TYPE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetControl.md) or [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetControl.md), otherwise, an error is thrown.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not save the coordinates of the paired reference points. |
| `M_ENABLE` | Specifies to save the coordinates of the paired reference points. |
