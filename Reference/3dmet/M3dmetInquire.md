---
doctype: Reference
module: 3dmet
function: M3dmetInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetInquire"
---

# M3dmetInquire

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

> Inquire information about a 3D metrology context.

## Syntax

```c
AIL_INT64 M3dmetInquire(
    AIL_ID    ContextOrResult3dmetId,  //in
    AIL_INT64 InquireType,             //in
    void *    UserVarPtr               //out
)
```

## Description

This function inquires information about a 3D metrology context.

To retrieve results from a 3D metrology result buffer, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md).

To inquire about draw 3D metrology settings for [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md), use [`M3dmetInquireDraw`](../../Reference/3dmet/M3dmetInquireDraw.md) instead.

## Parameters

### `ContextOrResult3dmetId` *(in, AIL_ID)*

Specifies the identifier of the 3D metrology context or result buffer to inquire. The 3D metrology context or result buffer must have been previously allocated on the required system using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) or [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md), respectively.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of information about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address of the variable in which to write the requested information. Since the [`M3dmetInquire`](../../Reference/3dmet/M3dmetInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a statistics 3D metrology context

For a statistics 3D metrology context, the [`InquireType`](../../Reference/3dmet/M3dmetInquire.md) parameter can be set to one of the following.

---

### `M_STAT_MAX`

Inquires whether the maximum distance between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the maximum distance. |
| `M_ENABLE` | Specifies to calculate the maximum distance. |

---

### `M_STAT_MAX_ABS`

Inquires whether the maximum absolute distance between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the maximum absolute distance. |
| `M_ENABLE` | Specifies to calculate the maximum absolute distance. |

---

### `M_STAT_MEAN`

Inquires whether the average distance between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the average distance. |
| `M_ENABLE` | Specifies to calculate the average distance. |

---

### `M_STAT_MEAN_ABS`

Inquires whether to calculate the average absolute distance between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the average absolute distance. |
| `M_ENABLE` | Specifies to calculate the average absolute distance. |

---

### `M_STAT_MIN`

Inquires whether the minimum distance between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the minimum distance. |
| `M_ENABLE` | Specifies to calculate the minimum distance. |

---

### `M_STAT_MIN_ABS`

Inquires whether the minimum absolute distance between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the minimum absolute distance. |
| `M_ENABLE` | Specifies to calculate the minimum absolute distance. |

---

### `M_STAT_NUMBER`

Inquires whether to record the number of points that meet the condition specified when calling [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md) (using the [`Condition`](../../Reference/3dmet/M3dmetStat.md), [`CondLow`](../../Reference/3dmet/M3dmetStat.md) and [`CondHigh`](../../Reference/3dmet/M3dmetStat.md) parameters).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to record the number of points that meet the specified condition. |
| `M_ENABLE` | Specifies to record the number of points that meet the specified condition. |

---

### `M_STAT_RMS`

Inquires whether the root-mean-square (RMS) error between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the RMS error. |
| `M_ENABLE` | Specifies to calculate the RMS error. |

---

### `M_STAT_STANDARD_DEVIATION`

Inquires whether the standard deviation of all the distances calculated between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the standard deviation. |
| `M_ENABLE` | Specifies to calculate the standard deviation. |

---

### `M_STAT_SUM`

Inquires whether the sum of all the distances calculated between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the sum of all the distances. |
| `M_ENABLE` | Specifies to calculate the sum of all the distances. |

---

### `M_STAT_SUM_ABS`

Inquires whether the sum of all the absolute distances calculated between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the sum of all absolute distances. |
| `M_ENABLE` | Specifies to calculate the sum of all absolute distances. |

---

### `M_STAT_SUM_OF_SQUARES`

Inquires whether the sum of squared distances between the point cloud or depth map, and the specified reference object will be calculated.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the sum of squared distances. |
| `M_ENABLE` | Specifies to calculate the sum of squared distances. |

### For inquiring about a fit 3D metrology context

For a fit 3D metrology context, the [`InquireType`](../../Reference/3dmet/M3dmetInquire.md) parameter can be set to one of the following:

---

### `M_ESTIMATION_MODE`

Inquires how to compute an initial fit estimate between the point cloud or depth map, and the specified reference object.

| Value | Description |
| --- | --- |
| `M_FROM_GEOMETRY` | Specifies that an initial fit estimate is determined using a geometry object. |
| `M_NO_SAMPLING` | Specifies that an initial fit estimate is calculated using all available points in a point cloud or depth map. |
| `M_RANDOM_SAMPLING` | Specifies that an initial fit estimate is determined using a random sampling consensus (RANSAC) algorithm. |

---

### `M_EXPECTED_OUTLIER_PERCENTAGE`

Inquires the expected percentage of outliers among the points of the point cloud or depth map to be fitted.  Note, this is only used for [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md) operations with [`OutlierDistance`](../../Reference/3dmet/M3dmetFit.md) set to [`M_AUTO_VALUE`](../../Reference/3dmet/M3dmetFit.md).

| Value | Description |
| --- | --- |
| `0.0 < Value < 100.0` | Specifies the expected percentage of outliers among the points of the point cloud or depth map to be fitted. |

---

### `M_FIT_ITERATIONS_MAX`

Inquires the maximum number of iterations to use during the fit operation.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the maximum number of iterations to use during the fit operation. |

---

### `M_INLIER_AMOUNT_THRESHOLD`

Inquires the minimum number of inliers required for the fit operation to end.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies that there is no minimum number of inliers. |
| `Value >= 0` | Specifies the minimum number of inliers. |

---

### `M_RMS_ERROR_THRESHOLD`

Inquires the maximum RMS error required for the fit operation to end.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the maximum RMS error. |

### For inquiring about a volume 3D metrology context

For a volume 3D metrology context, the [`InquireType`](../../Reference/3dmet/M3dmetInquire.md) parameter can be set to one of the following:

---

### `M_SAVE_VOLUME_INFO`

Inquires whether to save information from the volume computation for use with [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) or [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md) operations.

| Value | Description |
| --- | --- |
| `M_FALSE` *(default)* | Specifies that the volume information is not saved. |
| `M_TRUE` | Specifies that the volume information is saved. |

---

### `M_VOLUME_MODE`

Inquires how to calculate the total volume, given the position of the source Aurora Imaging Library object relative to the reference object.

| Value | Description |
| --- | --- |
| `M_COMPLETE` *(default)* | Specifies to calculate all volume results, which can then be retrieved, copied, or drawn, according to the specified [`M_VOLUME_OUTPUT_MODE`](../../Reference/3dmet/M3dmetControl.md). |

### For inquiring about a calculate 3D metrology result buffer that holds results of an M3dmetVolumeEx operation

For a calculate 3D metrology result buffer that holds results of an [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation, the [`InquireType`](../../Reference/3dmet/M3dmetInquire.md) parameter can be set to one of the following:

---

### `M_VOLUME_OUTPUT_MODE`

Inquires the volume mode for which to retrieve, copy, or draw results, using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md), [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md), or [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md), respectively, when [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_COMPLETE`](../../Reference/3dmet/M3dmetControl.md).

| Value | Description |
| --- | --- |
| `M_AUTO` *(default)* | Specifies to match the [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) setting, unless [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_COMPLETE`](../../Reference/3dmet/M3dmetControl.md), in which case [`M_TOTAL`](../../Reference/3dmet/M3dmetInquire.md) is used. |

### For inquiring about a distance 3D metrology context

For a distance 3D metrology context, the [`InquireType`](../../Reference/3dmet/M3dmetInquire.md) parameter can be set to one of the following:

---

### `M_DISTANCE_METRIC_OPERATOR`

Inquires the operator applied to the distance from [`M_PAIRING_DISTANCE_METRIC`](../../Reference/3dmet/M3dmetInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` *(default)* | Specifies to use an absolute metric when calculating the distance. |
| `M_SIGNED` | Specifies to use a signed metric when calculating the distance. |
| `M_SQUARED` | Specifies to use a squared metric when calculating the distance. |

---

### `M_DISTANCE_NORMAL`

Inquires whether to compute the normal distance as the dot product between the paired source point's normal and the reference point's normal.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to not compute the normal distance. |
| `M_ENABLE` | Specifies to compute the normal distance. |

---

### `M_PAIRING_DISTANCE_MAX`

Inquires the maximum distance between a source point and a reference point to be paired. Distances larger than this are invalid.  Note, this is only used when the reference object is a depth map or a point cloud.

| Value | Description |
| --- | --- |
| `M_INFINITE` *(default)* | Specifies no maximum distance. |
| `Value > 0.0` | Specifies the maximum distance. |

---

### `M_PAIRING_DISTANCE_METRIC`

Inquires the metric used when choosing a source point's paired reference element. The paired reference element is that which is closest to a source point using the specified metric.

| Value | Description |
| --- | --- |
| `M_AUTO` *(default)* | Specifies to automatically choose the metric based on the reference object. |
| `M_DIRECTION_NORMAL` | Specifies to use the distance in the normal direction of the source point. |
| `M_DIRECTION_Z` | Specifies to use the distance in the Z-direction. |
| `M_EUCLIDEAN` | Specifies to use the Euclidean distance. |
| `M_MANHATTAN` | Specifies to use the Manhattan distance. |

---

### `M_PAIRING_PERPENDICULAR_DISTANCE_MAX`

Inquires the maximum perpendicular distance between a source point and a reference point to be paired when using [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetInquire.md). Distances greater than this are invalid.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the maximum perpendicular distance. |

---

### `M_PAIRING_PERPENDICULAR_DISTANCE_MAX_MODE`

Inquires whether an automatic value or a user-defined value is used for the maximum perpendicular distance between a source point and a reference point to be paired.

| Value | Description |
| --- | --- |
| `M_AUTO` *(default)* | Specifies an automatic value. |
| `M_USER_DEFINED` | Specifies to use the user-defined value specified using [`M_PAIRING_PERPENDICULAR_DISTANCE_MAX`](../../Reference/3dmet/M3dmetInquire.md). |

---

### `M_PAIRING_TYPE`

Inquires against which part of the reference object to measure when performing an [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md) operation.

| Value | Description |
| --- | --- |
| `M_AUTO` *(default)* | [`M_AUTO`](../../Reference/3dmet/M3dmetInquire.md) is the same as the first available setting: [`M_DISTANCE_TO_SURFACE`](../../Reference/3dmet/M3dmetInquire.md), [`M_DISTANCE_TO_NEIGHBOR`](../../Reference/3dmet/M3dmetInquire.md), [`M_DISTANCE_TO_REFERENCE`](../../Reference/3dmet/M3dmetInquire.md) in decreasing preference. |
| `M_DISTANCE_TO_NEIGHBOR` | Specifies to calculate the distance between the points of the source point cloud or depth map, and their nearest point in the specified reference point cloud or depth map. |
| `M_DISTANCE_TO_REFERENCE` | Specifies to calculate the distance between the points of the source point cloud or depth map and the specified reference 3D geometry. |
| `M_DISTANCE_TO_SURFACE` | Specifies to calculate the distance between the points of the source point cloud or depth map, and the surface of the specified reference object. |

---

### `M_SAVE_PAIRED_REFERENCE_FACET`

Inquires whether to save the index of the reference facet (face) to each point.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to not save the paired reference facet. |
| `M_ENABLE` | Specifies to save the paired reference facet. |

---

### `M_SAVE_PAIRED_REFERENCE_PIXEL`

Inquires whether to save the index of the paired reference pixel to each source point.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to not save the paired reference pixel. |
| `M_ENABLE` | Specifies to save the paired reference pixel. |

---

### `M_SAVE_PAIRED_REFERENCE_POINT`

Inquires whether to save the coordinates of the paired reference point to each source point.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to not save the coordinates of the paired reference points. |
| `M_ENABLE` | Specifies to save the coordinates of the paired reference points. |

### Combination Constants — For inquiring about the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get information about the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

#### `M_IS_SET_TO_DEFAULT`

Inquires whether the specified inquire type is set to its default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not set to its default value. |
| `M_TRUE` | Specifies that the inquire type is set to its default value. |

### Combination Constants — To inquire whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported for the 3D metrology context.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — To specify the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

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

**Type:** `AIL_INT64`

The returned value is the requested information, if it is an_AIL_INT_. If the requested information is an _AIL_DOUBLE_ or if it is an_AIL_INT64_ on a 32-bit system, this function will return `M_NULL`.
