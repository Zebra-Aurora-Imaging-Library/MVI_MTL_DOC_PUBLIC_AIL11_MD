---
doctype: Reference
module: 3dreg
function: M3dregControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregControl"
---

# M3dregControl

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

> Control a pairwise 3D registration context or a pairwise 3D registration result buffer.

## Syntax

```c
void M3dregControl(
    AIL_ID     ContextOrResult3dregId,  //out
    AIL_INT64  Index,                   //in
    AIL_INT64  ControlType,             //in
    AIL_DOUBLE ControlValue             //in
)
```

## Description

This function controls the settings of a pairwise 3D registration context, one of its registration elements, or a pairwise 3D registration result buffer. These settings specify how to start and stop the registration operation (preregistration and stop conditions), and how [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) performs the pairwise comparison of points.

To inquire about these control type settings, call [`M3dregInquire`](../../Reference/3dreg/M3dregInquire.md).

## Parameters

### `ContextOrResult3dregId` *(out, AIL_ID)*

Specifies the identifier of the pairwise 3D registration context or a pairwise 3D registration result buffer to control. The pairwise 3D registration context must be allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md), and the pairwise 3D registration result buffer must be allocated using [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md).

### `Index` *(in, AIL_INT64)*

Specifies what to control. Set this parameter to one of the following values:

*For specifying the registration element*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior.

If [`ContextOrResult3dregId`](../../Reference/3dreg/M3dregControl.md) specifies a pairwise 3D registration context, then [`M_DEFAULT`](../../Reference/3dreg/M3dregControl.md) is the same as [`M_CONTEXT`](../../Reference/3dreg/M3dregControl.md).

If [`ContextOrResult3dregId`](../../Reference/3dreg/M3dregControl.md) specifies a pairwise 3D registration result buffer, then [`M_DEFAULT`](../../Reference/3dreg/M3dregControl.md) is the same as [`M_GENERAL`](../../Reference/3dreg/M3dregControl.md). |
| `M_ALL` | Specifies to apply the control settings to all registration elements of a pairwise 3D registration context or all registration result elements of a pairwise 3D registration result buffer. |
| `M_CONTEXT` | Specifies to apply the control settings to a pairwise 3D registration context. |
| `M_GENERAL` | Specifies to apply the control settings to a pairwise 3D registration result buffer. |
| `0 <= Value < M3dregInquire(M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the registration element or registration result element to apply the control settings. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For controlling a pairwise 3D registration context

The following [`ControlType`](../../Reference/3dreg/M3dregControl.md)and corresponding[`ControlValue`](../../Reference/3dreg/M3dregControl.md) parameter settings are used to control the pairwise 3D registration context specified with the [`ContextOrResult3dregId`](../../Reference/3dreg/M3dregControl.md)parameter. [`Index`](../../Reference/3dreg/M3dregControl.md) must be set to [`M_CONTEXT`](../../Reference/3dreg/M3dregControl.md).

---

### `M_ERROR_MINIMIZATION_METRIC`

Sets the technique for performing the pairwise comparison during each iteration of the registration operation, and establishes how to compute the RMS error.  In each iteration, points in the reference point cloud are paired with the closest points in the other point cloud. The points are always paired in the same manner (independent of the setting of [`M_ERROR_MINIMIZATION_METRIC`](../../Reference/3dreg/M3dregControl.md)).  Once paired, the RMS error is calculated using the specified distance measurement or error metric.  The best technique for a given registration depends on how many large and flat surfaces the object represented by the point cloud has. Using the correct setting can help avoid registration sliding (when multiple different registration are possible because of large planar surfaces) or diverging (when the registration operation results in the point clouds moving apart).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_POINT_TO_PLANE` *(default)* | Specifies to calculate the RMS error using the shortest distance between the point (in the reference point cloud), and the plane tangent to the other paired point. The tangent plane is calculated using the normal of the latter point.  *[Image: 3dreg_PointToPlaneDiagram.png]*  This setting improves the speed of registering organized point clouds, where the normal at a given point is not similar to the normal at nearby points. This implies that the object represented by the point cloud has a large range of surface directions, and no relatively large flat surfaces. Large surfaces can result in the registration sliding or diverging. |
| `M_POINT_TO_POINT` | Specifies to calculate the RMS error using the distance between the paired points.  *[Image: 3dreg_PointToPointDiagram.png]*  This setting is better suited to registering point clouds that represent objects with large planar surfaces, and will avoid registration sliding or diverging. |

---

### `M_MAX_ITERATIONS`

Sets a stop condition for the iterative registration operation. If the maximum number of iterations is reached, the registration operation stops.  To inquire whether this stop condition was met, call [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) with [`M_STATUS`](../../Reference/3dreg/M3dregGetResult.md).  > **Note:** Note that preregistration constitutes the first iteration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the maximum number of iterations in the registration process. |

---

### `M_NUMBER_OF_REGISTRATION_ELEMENTS`

Sets the number of registration elements in the pairwise 3D registration context. The number of registration elements should be equal to, or greater than, the number of point clouds to register.  If you reduce the number of registration elements, the settings of the ones that remain are kept unchanged. If you increase the number of registration elements, the new ones are initialized with the default settings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `2 <= Value <= 65535` *(default)* | Specifies the number of registration elements in a pairwise 3D registration context. |

---

### `M_PAIRS_CREATION_FROM_TARGET`

Sets whether to begin pairing points from the reference to target point cloud, and then vice versa. You can enable such pairings so that they are performed for all iterations or only for specific iterations.  When this control type is enabled, the registration operation finds point pairs in both directions. After looping over the reference point cloud to find, for every reference point, its closest pair in the target point cloud, the operation then loops over the target point cloud to find, for every target point, its closest pair in the reference point cloud. Using [`M_PAIRS_CREATION_FROM_TARGET`](../../Reference/3dreg/M3dregControl.md) can help prevent divergence when the reference and target point clouds are far apart.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to find point pairs in both directions for specific iterations based on an internal heuristic. |
| `M_DISABLE` *(default)* | Specifies not to find point pairs in both directions. The operation finds matches in the target point cloud relative to reference points, and won't search for matches in the reference point cloud relative to target points. |
| `M_ENABLE` | Specifies to find point pairs in both directions. |

---

### `M_PAIRS_CREATION_MAX_POINT_DISTANCE`

Sets the maximum Euclidean distance within which two points could be paired. For any given reference point, [`M_PAIRS_CREATION_MAX_POINT_DISTANCE`](../../Reference/3dreg/M3dregControl.md) does not limit how many pairings are possible within the specified distance.  If you know beforehand how far the target is from the reference, you can set this control type to an appropriate value and eliminate plainly irrelevant outliers. You can also use this setting to prevent divergence when there are multiple occurrences of the target in your scene. Place the reference over a rough estimate of an occurrence and set [`M_PAIRS_CREATION_MAX_POINT_DISTANCE`](../../Reference/3dreg/M3dregControl.md) to prevent convergence towards a different occurrence.  > **Note:** [`M_PAIRS_CREATION_MAX_POINT_DISTANCE`](../../Reference/3dreg/M3dregControl.md) is independent from [`M_PAIRS_REJECTION_MODE`](../../Reference/3dreg/M3dregControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that there is no maximum distance. |
| `Value > 0.0` | Specifies the maximum distance. |

---

### `M_PAIRS_CREATION_PER_REFERENCE_POINT_MODE`

Sets whether to pair each point in the reference point cloud with a single point or multiple points in the target point cloud. You can also choose to allow multiple pairings for specific iterations only.  When the reference and target point clouds are far apart, choosing [`M_MULTIPLE`](../../Reference/3dreg/M3dregControl.md) makes the point clouds less likely to diverge. When the two point clouds are very close, [`M_SINGLE`](../../Reference/3dreg/M3dregControl.md) can give a more refined registration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to allow multiple point pairings for specific iterations; the operation adjusts the number of paired points per reference point for each iteration based on an internal heuristic. |
| `M_MULTIPLE` | Specifies to pair each reference point with up to 6 neighbors in the target point cloud. |
| `M_SINGLE` *(default)* | Specifies to pair each reference point with a single point in the target point cloud. |

---

### `M_PAIRS_LIMIT_PER_TARGET_POINT_MODE`

Specifies whether to limit the number of point pairs to which a target point can belong. Once the operation determines each reference point's matching pair in the target point cloud, setting this control to [`M_SINGLE`](../../Reference/3dreg/M3dregControl.md) checks all target points to ensure that each is matched to no more than one point in the reference point cloud. Set this control to [`M_AUTO`](../../Reference/3dreg/M3dregControl.md) to let Aurora Imaging Library determine the number of pairings per target point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to allow every target point to be part of a variable number of point pairs, based on an internal heuristic. |
| `M_DISABLE` *(default)* | Specifies not to limit the number of point pairs to which a target point can belong. |
| `M_SINGLE` | Specifies to allow every target point to be part of 1 point pair. |

---

### `M_PREREGISTRATION_MODE`

Sets how to perform the preregistration step.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTROID` | Specifies to apply the preregistration transformation matrix, specified using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md); it then automatically registers the centroids (center of mass) of the two point clouds.  By default, the registration of the centroids is a translation only and does not include any rotation.  To only register the centroids, you can specify the identity matrix as the transformation matrix of the preregistration; to do so, call [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md) with [`M_IDENTITY_MATRIX`](../../Reference/3dreg/M3dregSetLocation.md). Alternatively, you can delegate the translation to the centroid registration, and define the rotation using a user-defined transformation matrix specified using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md). |
| `M_USER_DEFINED` *(default)* | Specifies to apply the preregistration transformation matrix in the first iteration of the registration operation. Specify the transformation matrix using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md). |

---

### `M_RMS_ERROR_RELATIVE_THRESHOLD`

Sets the stop condition for the registration operation that tests the percentage change of the root-mean-square (RMS) error of successive iterations. The RMS error is calculated from the distance between all paired points. You can control how the distance is calculated using [`M_ERROR_MINIMIZATION_METRIC`](../../Reference/3dreg/M3dregControl.md).  Specifically, where _E<sub>i</sub> _ is the RMS error of iteration _i_, the relative RMS error is calculated as:  `[(_E<sub>i-1</sub> _ - _E<sub>i</sub> _)/_E<sub>i-1</sub> _]*100`  For instance, if the RMS error of the first iteration is 2.0 and the RMS error of the second iteration is 0.8, the relative RMS error of the second iteration is equal to [(2.0-0.8)/2.0]*100, which is 60%. If the relative RMS error is positive, this means the RMS error is decreasing, and the point clouds are becoming increasingly aligned.  If the change in the calculated RMS error between successive iterations is less than or equal to the specified value, the registration operation stops.  If this stop condition is met, the registration is typically considered correct.  > **Note:** Note that the specified value represents a percentage, so the default value of 0.1 represents 0.1%, not 10%.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the relative RMS error threshold. |

---

### `M_RMS_ERROR_THRESHOLD`

Sets the stop condition for the registration operation that tests the root-mean-square (RMS) error of successive iterations. The RMS error is calculated from the distance between all paired points. You can control how the distance is calculated using [`M_ERROR_MINIMIZATION_METRIC`](../../Reference/3dreg/M3dregControl.md).  This value should be set to an acceptable average RMS error for a pair of points.  If the calculated RMS error goes under or equals the specified value, the registration operation stops.  If this stop condition is met, the registration is typically considered correct.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the RMS error threshold. |

---

### `M_SAVE_PAIRS_INFO`

Sets whether to save point pairs at each iteration.  When pairs information is saved, you can retrieve diagnostic information using [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) and draw this diagnostic information using [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` *(default)* | Specifies not to save point pairs at each iteration. |
| `M_TRUE` | Specifies to save point pairs at each iteration. |

---

### `M_SUBSAMPLE`

Sets whether to subsample all point clouds before applying the registration. This subsampling is applied before any point cloud-specific subsampling is applied by [`M_SUBSAMPLE_REFERENCE`](../../Reference/3dreg/M3dregControl.md) or [`M_SUBSAMPLE_TARGET`](../../Reference/3dreg/M3dregControl.md).  Enabling this control type associates the registration context with an internal subsampling context used when subsampling all point clouds. The subsampling context defines the subsampling technique to use.  To change the settings of the subsampling context, call [`M3dregInquire`](../../Reference/3dreg/M3dregInquire.md) with [`M_SUBSAMPLE_CONTEXT_ID`](../../Reference/3dreg/M3dregInquire.md) to first get the Aurora Imaging Library identifier of the subsampling context. The subsampling context can then be controlled using [`M3dimControl`](../../Reference/3dim/M3dimControl.md)(see the settings available for [`Subsample_context_ID`](../../Reference/3dim/M3dimControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable subsampling before applying the registration. |
| `M_ENABLE` | Specifies to enable subsampling before applying the registration. |

---

### `M_TIMEOUT`

Sets the maximum amount of time for [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) to complete the registration operation before generating a time-out error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no timeout value. |
| `Value > 0.0` | Specifies the timeout value, in msec. |

### For controlling the registration elements of a pairwise 3D registration context

The following [`ControlType`](../../Reference/3dreg/M3dregControl.md)and corresponding[`ControlValue`](../../Reference/3dreg/M3dregControl.md) parameter settings are used to control the registration elements of the pairwise 3D registration context specified with the [`ContextOrResult3dregId`](../../Reference/3dreg/M3dregControl.md)parameter. [`Index`](../../Reference/3dreg/M3dregControl.md) must be set to an index value or [`M_ALL`](../../Reference/3dreg/M3dregControl.md):

---

### `M_OVERLAP`

Sets the percentage of point pairings to use in a single iteration of the registration operation.  At the start of an iteration, the points of the reference point cloud are individually paired with the closest points in the other point cloud. [`M_OVERLAP`](../../Reference/3dreg/M3dregControl.md) specifies the percentage of those paired points that are used in the registration operation, prioritizing the paired points with the lowest RMS error.  For example, if you specify 80%, then during each iteration, 80% of paired points (with the lowest calculated RMS error as defined by[`M_ERROR_MINIMIZATION_METRIC`](../../Reference/3dreg/M3dregControl.md)) are used to calculate the average RMS error of the iteration, and a new transformation. This excludes the 20% worst point pairings, which often contain outliers in the reference point cloud. These outliers are typically caused by image noise, occlusion, object deformity, or misalignment.  Typically, if there are fewer points in the reference point cloud than in the point cloud that will be registered to it, the overlap should roughly equal the ratio of the points in the reference point cloud to the points in the other point cloud.  If the density of points is the same in both point clouds, this ratio could be set to the exact ratio between the two. For example, if there are 100 points in the reference point cloud, 200 points in the point cloud that will be registered to it, and they have a similar density, then the overlap can be set to 50%. This can occur because of occlusion of the object while generating the scene point cloud.  If there are more points in the reference point cloud, this value should be set to a little under 100%. In this case, it is recommended that you use as many of the points in the reference point cloud as possible while still accounting for outliers.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the percentage of possible paired points included in each iteration of the registration operation. |

---

### `M_PAIRS_REJECTION_FACTOR`

Sets the rejection factor, which is a constant value with which to multiply the robust standard deviation when calculating the distance threshold ([`M_PAIRS_REJECTION_MODE`](../../Reference/3dreg/M3dregControl.md) set to [`M_ROBUST_STANDARD_DEVIATION`](../../Reference/3dreg/M3dregControl.md)). A larger rejection factor sets a larger distance threshold.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the rejection factor. |

---

### `M_PAIRS_REJECTION_MODE`

Sets whether to reject point pairs that are too far apart, according to a distance threshold established from all point pair distances. When using this mode, you can set [`M_PAIRS_REJECTION_FACTOR`](../../Reference/3dreg/M3dregControl.md) to adjust the threshold, which is calculated as follows: `_distance threshold_ = _median(all pair distances)_ + _rejection factor_ * _robust standard deviation(all pair distances)_`.  Note that [`M_PAIRS_REJECTION_MODE`](../../Reference/3dreg/M3dregControl.md) applies the threshold to each iteration. Note also that, for this control setting, a point pair's distance is that defined by [`M_ERROR_MINIMIZATION_METRIC`](../../Reference/3dreg/M3dregControl.md).  > **Note:** [`M_PAIRS_REJECTION_MODE`](../../Reference/3dreg/M3dregControl.md) is independent from [`M_PAIRS_CREATION_MAX_POINT_DISTANCE`](../../Reference/3dreg/M3dregControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to use pairs rejection mode. |
| `M_ROBUST_STANDARD_DEVIATION` | Specifies to use pairs rejection mode with a robust estimate of the variance (standard deviation) of the distance between point pairs. Set the multiplicative factor used in the calculation with [`M_PAIRS_REJECTION_FACTOR`](../../Reference/3dreg/M3dregControl.md). |

---

### `M_SUBSAMPLE_REFERENCE`

Sets whether to subsample the reference point cloud associated with the specified registration element, before applying the registration. This subsampling is applied to the point cloud after any subsampling is applied by [`M_SUBSAMPLE`](../../Reference/3dreg/M3dregControl.md).  Enabling this control type associates the registration element with an internal subsampling context used when subsampling the reference point cloud. The subsampling context defines the subsampling technique to use. This allows you to specify a different subsampling method for each registration element within a registration context.  To change the settings of the subsampling context, call [`M3dregInquire`](../../Reference/3dreg/M3dregInquire.md) with [`M_SUBSAMPLE_REFERENCE_CONTEXT_ID`](../../Reference/3dreg/M3dregInquire.md) to first get the Aurora Imaging Library identifier of the subsampling context. The subsampling context can then be controlled using [`M3dimControl`](../../Reference/3dim/M3dimControl.md)(see the settings available for [`Subsample_context_ID`](../../Reference/3dim/M3dimControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable subsampling for the reference point cloud before applying the registration. |
| `M_ENABLE` | Specifies to enable subsampling for the reference point cloud before applying the registration. |

---

### `M_SUBSAMPLE_TARGET`

Sets whether to subsample the target point cloud associated with the specified registration element, before applying the registration. This subsampling is applied to the point cloud after any subsampling is applied by [`M_SUBSAMPLE`](../../Reference/3dreg/M3dregControl.md).  Enabling this control type associates the registration element with an internal subsampling context used when subsampling the reference point cloud. The subsampling context defines the subsampling technique to use. This allows you to specify a different subsampling method for each registration element within a registration context.  To change the settings of the subsampling context, call [`M3dregInquire`](../../Reference/3dreg/M3dregInquire.md) with [`M_SUBSAMPLE_TARGET_CONTEXT_ID`](../../Reference/3dreg/M3dregInquire.md) to first get the Aurora Imaging Library identifier of the subsampling context. The subsampling context can then be controlled using [`M3dimControl`](../../Reference/3dim/M3dimControl.md)(see the settings available for [`Subsample_context_ID`](../../Reference/3dim/M3dimControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable subsampling for the target point cloud before applying the registration. |
| `M_ENABLE` | Specifies to enable subsampling for the target point cloud before applying the registration. |

### For controlling the registration elements of a pairwise 3D registration result buffer

The following [`ControlType`](../../Reference/3dreg/M3dregControl.md)and corresponding[`ControlValue`](../../Reference/3dreg/M3dregControl.md) parameter settings are used to control the registration elements of the pairwise 3D result buffer specified with the [`ContextOrResult3dregId`](../../Reference/3dreg/M3dregControl.md)parameter. [`Index`](../../Reference/3dreg/M3dregControl.md) must be set to an index value or [`M_ALL`](../../Reference/3dreg/M3dregControl.md).

---

### `M_ITERATION_INDEX`

Sets the index of the iteration from which to copy, get, or draw results.  Functions such as [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) and[`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) use this setting to retrieve results from intermediate iterations ([`M_INTERMEDIATE_ITERATION`](../../Reference/3dreg/M3dregCopyResult.md)), which is useful when you want to debug the registration operation or understand it in greater detail. [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md) uses this setting when drawing the result of an intermediate iteration of the registration process.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LAST_ITERATION` *(default)* | Specifies the index of the final iteration of the registration process. |
| `0 <= Value < M3dregGetResult(M_NB_ITERATIONS)` | Specifies the index of the iteration from which to copy, get, or draw results. |

---

### `M_PAIRS_RANK`

Sets the point pair rank when using [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) to create a distance, overlap, or pair index image. When a single reference point is paired to multiple points in the registration result element's point cloud, [`M_PAIRS_RANK`](../../Reference/3dreg/M3dregControl.md) sets from which point pair to copy results. Note that pairings are ranked from nearest (rank 0) to farthest.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value < M3dregGetResult(M_MAX_PAIRS_RANK)` *(default)* | Specifies the rank of the pairing from which to copy results. If a pair with the specified rank does not exist, Aurora Imaging Library writes an appropriate invalid value to the destination image buffer, depending on the type of image. For example, the invalid value for an [`M_OVERLAP_MASK`](../../Reference/3dreg/M3dregCopyResult.md) image is zero. |

### For controlling a pairwise 3D registration result buffer

The following [`ControlType`](../../Reference/3dreg/M3dregControl.md)and corresponding[`ControlValue`](../../Reference/3dreg/M3dregControl.md) parameter settings are used to control the pairwise 3D registration result buffer specified with the [`ContextOrResult3dregId`](../../Reference/3dreg/M3dregControl.md)parameter. [`Index`](../../Reference/3dreg/M3dregControl.md) must be set to [`M_GENERAL`](../../Reference/3dreg/M3dregControl.md).

---

### `M_MERGE_LOCATION`

Sets whether to transform the point clouds such that their working coordinate systems are aligned to the global coordinate system, or to transform (fixture) them such that their working coordinate systems are aligned with a specified point cloud's working coordinate system, for an [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REGISTRATION_GLOBAL` *(default)* | Specifies to transform the point clouds to the global coordinate system for the merging operation. |
| `0 <= Value < M3dregInquire(M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the point cloud to which to fixture all point clouds; the point clouds will be aligned to the specified point cloud's working coordinate system. |

---

### `M_RESET`

Removes all results from the pairwise 3D registration result buffer. This does not delete the pairwise 3D registration result buffer, as is the case with [`M3dregFree`](../../Reference/3dreg/M3dregFree.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_STOP_CALCULATE`

Stops the current execution of [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) from another thread of higher priority. Any results from previous registrations become invalid and are discarded, and any completed results from the current registration operation will be available.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |
