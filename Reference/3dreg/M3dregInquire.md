---
doctype: Reference
module: 3dreg
function: M3dregInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregInquire"
---

# M3dregInquire

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

> Inquire information about a pairwise 3D registration context or a pairwise 3D registration result buffer.

## Syntax

```c
AIL_INT64 M3dregInquire(
    AIL_ID    ContextOrResult3dregId,  //in
    AIL_INT64 Index,                   //in
    AIL_INT64 InquireType,             //in
    void *    UserVarPtr               //out
)
```

## Description

This function inquires information about a pairwise 3D registration context or a pairwise 3D registration result buffer.

In addition, this function can inquire the settings for stop conditions for the registration operation executed by [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md). To inquire whether a stop condition was met, call [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) with [`M_STATUS`](../../Reference/3dreg/M3dregGetResult.md).

Note that for a 3D pairwise registration result buffer, this function only retrieves information about the buffer's settings (set using [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md) or [`M3dregControl`](../../Reference/3dreg/M3dregControl.md)). To retrieve results from the registration result buffer, use [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md).

## Parameters

### `ContextOrResult3dregId` *(in, AIL_ID)*

Specifies the identifier of the 3D pairwise registration context or a pairwise 3D registration result buffer to inquire. The pairwise 3D registration context is allocated with [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md), and the pairwise 3D registration result buffer is allocated with [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md).

### `Index` *(in, AIL_INT64)*

Specifies to inquire a pairwise 3D registration context, an individual registration element (or all registration elements), or a pairwise 3D registration result buffer. Set this parameter to one of the following values:

*For specifying the registration element*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior.

If [`ContextOrResult3dregId`](../../Reference/3dreg/M3dregInquire.md) specifies a pairwise 3D registration context, then [`M_DEFAULT`](../../Reference/3dreg/M3dregInquire.md) is the same as [`M_CONTEXT`](../../Reference/3dreg/M3dregInquire.md).

If [`ContextOrResult3dregId`](../../Reference/3dreg/M3dregInquire.md) specifies a pairwise 3D registration result buffer, then [`M_DEFAULT`](../../Reference/3dreg/M3dregInquire.md) is the same as [`M_GENERAL`](../../Reference/3dreg/M3dregInquire.md). |
| `M_ALL` | Specifies to inquire all registration elements of a pairwise 3D registration context or all registration result elements of a pairwise 3D registration result buffer. |
| `M_CONTEXT` | Specifies to inquire a pairwise 3D registration context. |
| `M_GENERAL` | Specifies to inquire a pairwise 3D registration result buffer. |
| `0 <= Value < M3dregInquire(M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the registration element or registration result element to inquire. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dregInquire`](../../Reference/3dreg/M3dregInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring a pairwise 3D registration context or a pairwise 3D registration result buffer

For the following inquire types, the [`ContextOrResult3dregId`](../../Reference/3dreg/M3dregInquire.md) parameter must specify a 3D registration context or a 3D registration result buffer. The [`Index`](../../Reference/3dreg/M3dregInquire.md)parameter must be set to [`M_CONTEXT`](../../Reference/3dreg/M3dregInquire.md) or [`M_GENERAL`](../../Reference/3dreg/M3dregInquire.md), for a 3D registration context or 3D registration result buffer respectively.

---

### `M_NUMBER_OF_REGISTRATION_ELEMENTS`

Inquires the number of registration elements in the 3D registration context, or the number of registration result elements in the 3D registration result buffer.

| Value | Description |
| --- | --- |
| `2 <= Value <= 65535` *(default)* | Specifies the number of registration elements or registration result elements. |

### For inquiring a pairwise 3D registration context

For a pairwise 3D registration context, when [`Index`](../../Reference/3dreg/M3dregInquire.md)is set to [`M_CONTEXT`](../../Reference/3dreg/M3dregInquire.md), the [`InquireType`](../../Reference/3dreg/M3dregInquire.md)parameter can be set to one of the following:

---

### `M_ERROR_MINIMIZATION_METRIC`

Inquires the technique for performing the pairwise comparison during each iteration of the registration operation, and establishes how to compute the RMS error.

| Value | Description |
| --- | --- |
| `M_POINT_TO_PLANE` *(default)* | Specifies to calculate the RMS error using the shortest distance between the point (in the reference point cloud), and the plane tangent to the other paired point. |
| `M_POINT_TO_POINT` | Specifies to calculate the RMS error using the distance between the paired points. |

---

### `M_MAX_ITERATIONS`

Inquires a stop condition for the iterative registration operation. If the maximum number of iterations is reached, the registration operation stops.  To inquire whether this stop condition was met, call [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) with [`M_STATUS`](../../Reference/3dreg/M3dregGetResult.md).  > **Note:** Note that preregistration constitutes the first iteration.

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the maximum number of iterations in the registration process. |

---

### `M_PAIRS_CREATION_FROM_TARGET`

Inquires whether to first find matching points in the target point cloud relative to the reference, and then vice versa. You can enable such matchings so that they are performed from the first iteration or only for specific iterations.

| Value | Description |
| --- | --- |
| `M_AUTO` | Specifies to find point pairs in both directions for specific iterations based on an internal heuristic. |
| `M_DISABLE` *(default)* | Specifies not to find point pairs in both directions. |
| `M_ENABLE` | Specifies to find point pairs in both directions. |

---

### `M_PAIRS_CREATION_MAX_POINT_DISTANCE`

Inquires the maximum Euclidean distance within which two points are considered paired.

| Value | Description |
| --- | --- |
| `M_INFINITE` *(default)* | Specifies that there is no maximum distance. |
| `Value > 0.0` | Specifies the maximum distance. |

---

### `M_PAIRS_CREATION_PER_REFERENCE_POINT_MODE`

Inquires whether to pair each point in the reference point cloud with a single point or multiple points in the target point cloud. You can also choose to allow multiple pairings for specific iterations only.

| Value | Description |
| --- | --- |
| `M_AUTO` | Specifies to allow multiple point pairings for specific iterations; the operation adjusts the number of paired points per reference point for each iteration based on an internal heuristic. |
| `M_MULTIPLE` | Specifies to pair each reference point with up to 6 neighbors in the target point cloud. |
| `M_SINGLE` *(default)* | Specifies to pair each reference point with a single point in the target point cloud. |

---

### `M_PAIRS_LIMIT_PER_TARGET_POINT_MODE`

Inquires whether to limit the number of point pairs to which a target point can belong.

| Value | Description |
| --- | --- |
| `M_AUTO` | Specifies to allow every target point to be part of a variable number of point pairs, based on an internal heuristic. |
| `M_DISABLE` *(default)* | Specifies not to limit the number of point pairs to which a target point can belong. |
| `M_SINGLE` | Specifies to allow every target point to be part of 1 point pair. |

---

### `M_PREREGISTRATION_MODE`

Inquires how to perform the preregistration step.

| Value | Description |
| --- | --- |
| `M_CENTROID` | Specifies to apply the preregistration transformation matrix, specified using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md); it then automatically registers the centroids (center of mass) of the two point clouds. |
| `M_USER_DEFINED` *(default)* | Specifies to apply the preregistration transformation matrix in the first iteration of the registration operation. |

---

### `M_RMS_ERROR_RELATIVE_THRESHOLD`

Inquires the stop condition for the registration operation that tests the percentage change of the root-mean-square (RMS) error of successive iterations. The RMS error is calculated from the distance between all paired points. You can control how the distance is calculated using [`M_ERROR_MINIMIZATION_METRIC`](../../Reference/3dreg/M3dregInquire.md).  If this stop condition is met, the registration is typically considered correct.  > **Note:** Note that the specified value represents a percentage, so the default value of 0.1 represents 0.1%, not 10%.

| Value | Description |
| --- | --- |
| `Value >= 0.0` *(default)* | Specifies the relative RMS error threshold. |

---

### `M_RMS_ERROR_THRESHOLD`

Inquires the stop condition for the registration operation that tests the root-mean-square (RMS) error of successive iterations. The RMS error is calculated from the distance between all paired points. You can control how the distance is calculated using [`M_ERROR_MINIMIZATION_METRIC`](../../Reference/3dreg/M3dregInquire.md).  If this stop condition is met, the registration is typically considered correct.

| Value | Description |
| --- | --- |
| `Value >= 0.0` *(default)* | Specifies the RMS error threshold. |

---

### `M_SAVE_PAIRS_INFO`

Inquires whether to save point pairs at each iteration.

| Value | Description |
| --- | --- |
| `M_FALSE` *(default)* | Specifies not to save point pairs at each iteration. |
| `M_TRUE` | Specifies to save point pairs at each iteration. |

---

### `M_SUBSAMPLE`

Inquires whether to subsample all point clouds before applying the registration. This subsampling is applied before any point cloud-specific subsampling is applied by [`M_SUBSAMPLE_REFERENCE`](../../Reference/3dreg/M3dregControl.md) or [`M_SUBSAMPLE_TARGET`](../../Reference/3dreg/M3dregControl.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable subsampling before applying the registration. |
| `M_ENABLE` | Specifies to enable subsampling before applying the registration. |

---

### `M_SUBSAMPLE_CONTEXT_ID`

Inquires the ID of the subsampling context within the specified registration context.

| Value | Description |
| --- | --- |
| `Subsample Context ID` | Specifies the Aurora Imaging Library identifier of the subsampling context. |

### For inquiring the registration elements of a 3D pairwise registration context

For a 3D pairwise registration context, when [`Index`](../../Reference/3dreg/M3dregInquire.md)is set to an index value or [`M_ALL`](../../Reference/3dreg/M3dregInquire.md), the [`InquireType`](../../Reference/3dreg/M3dregInquire.md)parameter can be set to one of the following:

---

### `M_OVERLAP`

Inquires the percentage of all possible point pairings to use in a single iteration of the registration operation.

| Value | Description |
| --- | --- |
| `0.0 < Value <= 100.0` *(default)* | Specifies the percentage of possible paired points included in each iteration of the registration operation. |

---

### `M_PAIRS_REJECTION_FACTOR`

Inquires the rejection factor when calculating the distance threshold ([`M_PAIRS_REJECTION_MODE`](../../Reference/3dreg/M3dregInquire.md) set to [`M_ROBUST_STANDARD_DEVIATION`](../../Reference/3dreg/M3dregInquire.md)). A larger rejection factor sets a larger distance threshold.

| Value | Description |
| --- | --- |
| `Value >= 0.0` *(default)* | Specifies the rejection factor. |

---

### `M_PAIRS_REJECTION_MODE`

Inquires whether to reject point pairs that are too far apart, according to a distance threshold established from all point pair distances.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies not to use pairs rejection mode. |
| `M_ROBUST_STANDARD_DEVIATION` | Specifies to use pairs rejection mode with a robust estimate of the variance (standard deviation) of the distance between point pairs. |

---

### `M_SET_LOCATION_REFERENCE`

Inquires the index of the reference registration element, as set using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md) when setting the rough location of a point cloud's working coordinate system.

| Value | Description |
| --- | --- |
| `M_REGISTRATION_GLOBAL` | Specifies the global coordinate system. |
| `0 <= Value < M3dregInquire(M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the reference registration element. |

---

### `M_SUBSAMPLE_REFERENCE`

Inquires whether to subsample the reference point cloud associated with the specified registration element before applying the registration. This subsampling is applied to the point cloud after any subsampling is applied by [`M_SUBSAMPLE`](../../Reference/3dreg/M3dregControl.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable subsampling for the reference point cloud before applying the registration. |
| `M_ENABLE` | Specifies to enable subsampling for the reference point cloud before applying the registration. |

---

### `M_SUBSAMPLE_REFERENCE_CONTEXT_ID`

Inquires the ID of the subsampling context of the reference point cloud for the specified registration element.

| Value | Description |
| --- | --- |
| `Subsample Context ID` | Specifies the Aurora Imaging Library identifier of the subsampling context. |

---

### `M_SUBSAMPLE_TARGET`

Inquires whether to subsample the target point cloud associated with the specified registration element before applying the registration. This subsampling is applied to the point cloud after any subsampling is applied by [`M_SUBSAMPLE`](../../Reference/3dreg/M3dregControl.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable subsampling for the target point cloud before applying the registration. |
| `M_ENABLE` | Specifies to enable subsampling for the target point cloud before applying the registration. |

---

### `M_SUBSAMPLE_TARGET_CONTEXT_ID`

Inquires the ID of the subsampling context of the target point cloud for the specified registration element.

| Value | Description |
| --- | --- |
| `Subsample Context ID` | Specifies the Aurora Imaging Library identifier of the subsampling context. |

### For inquiring the registration elements of a 3D pairwise registration result buffer

For a 3D pairwise registration result buffer, when [`Index`](../../Reference/3dreg/M3dregInquire.md)is set to an index value or [`M_ALL`](../../Reference/3dreg/M3dregInquire.md), the [`InquireType`](../../Reference/3dreg/M3dregInquire.md)parameter can be set to one of the following:

---

### `M_ITERATION_INDEX`

Inquires the index of the iteration from which to copy, get, or draw results.

| Value | Description |
| --- | --- |
| `M_LAST_ITERATION` *(default)* | Specifies the index of the final iteration of the registration process. |
| `0 <= Value < M3dregGetResult(M_NB_ITERATIONS)` | Specifies the index of the iteration from which to copy, get, or draw results. |

---

### `M_PAIRS_RANK`

Inquires the point pair rank when using [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) to create a distance, overlap, or pair index image. Note that pairings are ranked from nearest (rank 0) to farthest.

| Value | Description |
| --- | --- |
| `0 <= Value < M3dregGetResult(M_MAX_PAIRS_RANK)` *(default)* | Specifies the rank of the pairing from which to copy results. |

### For inquiring a pairwise 3D registration result buffer

For a 3D pairwise registration result buffer, when [`Index`](../../Reference/3dreg/M3dregInquire.md)is set to [`M_GENERAL`](../../Reference/3dreg/M3dregInquire.md), the [`InquireType`](../../Reference/3dreg/M3dregInquire.md) parameter can be set to one of the following:

---

### `M_MERGE_LOCATION`

Inquires whether to transform the point clouds such that their working coordinate systems are aligned to the global coordinate system, or to transform (fixture) them such that their working coordinate systems are aligned with a specified point cloud's working coordinate system, for an [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md) operation.

| Value | Description |
| --- | --- |
| `M_REGISTRATION_GLOBAL` *(default)* | Specifies to transform the point clouds to the global coordinate system for the merging operation. |
| `0 <= Value < M3dregInquire(M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the point cloud to which to fixture all point clouds; the point clouds will be aligned to the specified point cloud's working coordinate system. |

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

Inquires whether the specified inquire type is supported for the 3D registration context.

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

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
