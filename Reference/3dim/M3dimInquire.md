---
doctype: Reference
module: 3dim
function: M3dimInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimInquire"
---

# M3dimInquire

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

> Inquire about a 3D image processing context or result buffer.

## Syntax

```c
AIL_INT64 M3dimInquire(
    AIL_ID    Context3dimId,  //in
    AIL_INT64 InquireType,    //in
    void *    UserVarPtr      //out
)
```

## Description

This function inquires about a specified setting of a 3D image processing context or lattice 3D image processing result buffer.

## Parameters

### `Context3dimId` *(in, AIL_ID)*

Specifies the identifier of the 3D image processing context or result buffer about which to inquire information.

### `InquireType` *(in, AIL_INT64)*

Specifies the setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dimInquire`](../../Reference/3dim/M3dimInquire.md) function also returns the requested information, you can set this parameter to `M_NULL` when the return type is _AIL_INT_.

## Parameter Associations

### For inquiring about a 3D image processing context or result buffer

To inquire about a 3D image processing context or lattice 3D image processing result buffer, set the [`Context3dimId`](../../Reference/3dim/M3dimInquire.md) and [`InquireType`](../../Reference/3dim/M3dimInquire.md) parameters to one of the following.

---

### `Calculate map size context ID`

Specifies a calculate map size 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_CALCULATE_MAP_SIZE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) operations.

#### `M_PIXEL_ASPECT_RATIO`

Inquires the pixel aspect ratio of the depth map.

| Value | Description |
| --- | --- |
| *(see [`M_PIXEL_ASPECT_RATIO`](Reference/3dim/M3dimControl.md))* |  |
| `M_NULL` | Specifies that other data determines the pixel aspect ratio, such as the values set with [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimInquire.md) and [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimInquire.md). |

#### `M_PIXEL_SIZE_CONSTRAINT`

Inquires how to calculate the depth map pixel sizes to fit the required pixel aspect ratio ([`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_PIXEL_SIZE_CONSTRAINT`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PIXEL_SIZE_X`

Inquires the length in X of one pixel in the depth map.

| Value | Description |
| --- | --- |
| *(see [`M_PIXEL_SIZE_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PIXEL_SIZE_Y`

Inquires the length in Y of one pixel in the depth map.

| Value | Description |
| --- | --- |
| *(see [`M_PIXEL_SIZE_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SIZE_X`

Inquires the image size of the depth map along the X dimension.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SIZE_Y`

Inquires the image size of the depth map along the Y dimension.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y`](Reference/3dim/M3dimControl.md))* |  |

---

### `Fill gaps context ID`

Specifies a fill gaps 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_FILL_GAPS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimFillGaps`](../../Reference/3dim/M3dimFillGaps.md) operations.

#### `M_FILL_BORDER`

Inquires whether to propagate the boundary value across each gap that touches the image border.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_BORDER`](Reference/3dim/M3dimControl.md))* |  |

#### `M_FILL_MODE`

Inquires the mode in which to fill gaps in depth maps.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_FILL_SHARP_ELEVATION`

Inquires which boundary to use to fill sharp elevation gaps in depth maps.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_SHARP_ELEVATION`](Reference/3dim/M3dimControl.md))* |  |

#### `M_FILL_SHARP_ELEVATION_DEPTH`

Inquires the threshold used to differentiate between gradual elevation gaps and sharp elevation gaps in the depth map.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_SHARP_ELEVATION_DEPTH`](Reference/3dim/M3dimControl.md))* |  |

#### `M_FILL_THRESHOLD_X`

Inquires the maximum X-size of gaps that will be filled.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_THRESHOLD_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_FILL_THRESHOLD_Y`

Inquires the maximum Y-size of gaps that will be filled.

| Value | Description |
| --- | --- |
| *(see [`M_FILL_THRESHOLD_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_INPUT_UNITS`

Inquires the units with which to interpret [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimInquire.md), [`M_FILL_THRESHOLD_X`](../../Reference/3dim/M3dimInquire.md), and [`M_FILL_THRESHOLD_Y`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_INPUT_UNITS`](Reference/3dim/M3dimControl.md))* |  |

---

### `Filter context ID`

Specifies a filter 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_FILTER_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md) operations.

#### `M_DISTANCE_WEIGHT`

Inquires the amount of smoothing. A larger value applies more smoothing than a smaller value.

| Value | Description |
| --- | --- |
| *(see [`M_DISTANCE_WEIGHT`](Reference/3dim/M3dimControl.md))* |  |

#### `M_FILTER_MODE`

Inquires the type of smoothing operation to perform.

| Value | Description |
| --- | --- |
| *(see [`M_FILTER_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NORMALS_CONTEXT_ID`

Inquires the identifier of the internal normals context. The internal context is used to set the neighborhood of a point, and, if normals are computed, it specifies the normals' orientation.

#### `M_NORMALS_WEIGHT_FACTOR`

Inquires how well edges are preserved for an [`M_SMOOTH_BILATERAL`](../../Reference/3dim/M3dimInquire.md) operation. A smaller value results in more edge preservation.

| Value | Description |
| --- | --- |
| *(see [`M_NORMALS_WEIGHT_FACTOR`](Reference/3dim/M3dimControl.md))* |  |

#### `M_USE_SOURCE_NORMALS`

Inquires whether to use the source point cloud's normals information, stored in the source container's [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component.

| Value | Description |
| --- | --- |
| *(see [`M_USE_SOURCE_NORMALS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_WEIGHT_MODE`

Inquires how the Gaussian weights are determined in the smoothing calculation set with [`M_FILTER_MODE`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_WEIGHT_MODE`](Reference/3dim/M3dimControl.md))* |  |

---

### `Lattice context ID`

Specifies a lattice 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_LATTICE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) operations.

#### `M_FRACTION_OF_POINTS`

Inquires the ratio of the total number of required (non-empty) cells to the total number of valid points in the point cloud.

| Value | Description |
| --- | --- |
| *(see [`M_FRACTION_OF_POINTS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_FRACTION_OF_POINTS_TOLERANCE`

Inquires the tolerance to use for [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimInquire.md) when calculating the lattice.

| Value | Description |
| --- | --- |
| *(see [`M_FRACTION_OF_POINTS_TOLERANCE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_LATTICE_MODE`

Inquires the mode with which to calculate the lattice.

| Value | Description |
| --- | --- |
| *(see [`M_LATTICE_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_RELATIVE_CELL_SIZE_X`

Inquires the proportion of the cell size along the X dimension relative to the cell sizes along the Y and Z dimensions.

| Value | Description |
| --- | --- |
| *(see [`M_RELATIVE_CELL_SIZE_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_RELATIVE_CELL_SIZE_Y`

Inquires the proportion of the cell size along the Y dimension relative to the cell sizes along the X and Z dimensions.

| Value | Description |
| --- | --- |
| *(see [`M_RELATIVE_CELL_SIZE_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_RELATIVE_CELL_SIZE_Z`

Inquires the proportion of the cell size along the Z dimension relative to the cell sizes along the X and Y dimensions.

| Value | Description |
| --- | --- |
| *(see [`M_RELATIVE_CELL_SIZE_Z`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SPARSITY_DIMENSION_TOLERANCE`

Inquires the minimum difference in dimensionality between consecutive grid sizes when calculating the lattice.

| Value | Description |
| --- | --- |
| *(see [`M_SPARSITY_DIMENSION_TOLERANCE`](Reference/3dim/M3dimControl.md))* |  |

---

### `Lattice result buffer ID`

Specifies a lattice 3D image processing result buffer, allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_LATTICE_RESULT`](../../Reference/3dim/M3dimAllocResult.md), and used to store [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) results.

#### `M_GRID_SIZE_INDEX`

Inquires the grid size for which to return all lattice results.

| Value | Description |
| --- | --- |
| *(see [`M_GRID_SIZE_INDEX`](Reference/3dim/M3dimControl.md))* |  |

---

### `Mesh context ID`

Specifies a mesh 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_MESH_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md) operations.

#### `M_MAX_DISTANCE`

Inquires the maximum distance at which to connect 2 neighboring points.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_DISTANCE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MAXIMUM_NUMBER_NEIGHBORS`

Inquires the maximum number of neighbors to consider when computing a local plane at each point.

| Value | Description |
| --- | --- |
| *(see [`M_MAXIMUM_NUMBER_NEIGHBORS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MESH_MODE`

Inquires the surface reconstruction mode to use for mesh generation.

| Value | Description |
| --- | --- |
| *(see [`M_MESH_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MESH_STEP_X`

Inquires the step interval between points, along the X-axis of the range component, in pixels.

| Value | Description |
| --- | --- |
| *(see [`M_MESH_STEP_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MESH_STEP_Y`

Inquires the step interval between points, along the Y-axis of the range component, in pixels.

| Value | Description |
| --- | --- |
| *(see [`M_MESH_STEP_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MESH_TRIMMING_LEVEL`

Inquires the trimming level for a mesh.

| Value | Description |
| --- | --- |
| *(see [`M_MESH_TRIMMING_LEVEL`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBOR_SEARCH_MODE`

Inquires the search mode for finding the neighbors of a point.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBOR_SEARCH_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_DISTANCE`

Inquires the distance that defines the neighborhood of a point; only points within this distance are considered when calculating the local plane of the point.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_DISTANCE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Inquires the neighborhood size when finding the neighbors of a point in an organized point cloud.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NUMBER_POINTS_PER_CELL`

Inquires the number of points within a single octree cell at which the cell will not be split further.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_POINTS_PER_CELL`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SURFACE_ANGLE_MAX`

Inquires the maximum acceptable angle, in degrees, between the normal of a point and that of a neighboring point for the neighbor to be considered for triangulation.

| Value | Description |
| --- | --- |
| *(see [`M_SURFACE_ANGLE_MAX`](Reference/3dim/M3dimControl.md))* |  |

#### `M_TREE_MAX_DEPTH`

Inquires the maximum allowed depth of the octree.

| Value | Description |
| --- | --- |
| *(see [`M_TREE_MAX_DEPTH`](Reference/3dim/M3dimControl.md))* |  |

#### `M_TRIANGLE_ANGLE_MAX`

Inquires the maximum interior angle, in degrees, for each mesh triangle.

| Value | Description |
| --- | --- |
| *(see [`M_TRIANGLE_ANGLE_MAX`](Reference/3dim/M3dimControl.md))* |  |

#### `M_TRIANGLE_ANGLE_MIN`

Inquires the minimum interior angle, in degrees, for each mesh triangle.

| Value | Description |
| --- | --- |
| *(see [`M_TRIANGLE_ANGLE_MIN`](Reference/3dim/M3dimControl.md))* |  |

---

### `Normals context ID`

Specifies a normals 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_NORMALS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md) operations.

#### `M_DIRECTION_MODE`

Inquires the orientation of the normal vectors.

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_DIRECTION_REFERENCE_X`

Inquires the X-coordinate or vector component along X for the reference point or vector (respectively) that determines normal vector orientation.

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_REFERENCE_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_DIRECTION_REFERENCE_Y`

Inquires the Y-coordinate or vector component along Y for the reference point or vector (respectively) that determines normal vector orientation.

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_REFERENCE_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_DIRECTION_REFERENCE_Z`

Inquires the Z-coordinate or vector component along Z for the reference point or vector (respectively) that determines normal vector orientation.

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_REFERENCE_Z`](Reference/3dim/M3dimControl.md))* |  |

#### `M_ENFORCE_CONTINUITY`

Inquires whether to maintain the general direction of the normals, which prevents large, discontinuous jumps between normal vectors.

| Value | Description |
| --- | --- |
| *(see [`M_ENFORCE_CONTINUITY`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MAXIMUM_NUMBER_NEIGHBORS`

Inquires the maximum number of neighbors to consider when computing a point's normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_MAXIMUM_NUMBER_NEIGHBORS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBOR_SEARCH_MODE`

Inquires the search mode for finding the neighbors of a point, when calculating a point's surface normal vector.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBOR_SEARCH_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_DISTANCE`

Inquires the distance that defines the neighborhood of a point; only points within this distance are considered for normal vector calculations.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_DISTANCE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Inquires the neighborhood size when finding the neighbors of a point in an organized point cloud.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](Reference/3dim/M3dimControl.md))* |  |

---

### `Outliers context ID`

Specifies an outliers 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_OUTLIERS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md) operations.

#### `M_DISTANCE_THRESHOLD_MODE`

Inquires the threshold mode for a local distance operation.  > **Note:** This inquire type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_LOCAL_DISTANCE`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_DISTANCE_THRESHOLD_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MINIMUM_NUMBER_NEIGHBORS`

Inquires the minimum number of neighbors that a point must have to be considered an inlier, within the distance that defines the neighborhood.  > **Note:** This inquire type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_NUMBER_WITHIN_DISTANCE`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_MINIMUM_NUMBER_NEIGHBORS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBOR_SEARCH_MODE`

Inquires the search mode for finding the neighbors of a point.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBOR_SEARCH_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_DISTANCE`

Inquires the distance from a point within which other points are considered its neighbors.  > **Note:** This inquire type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_NUMBER_WITHIN_DISTANCE`](../../Reference/3dim/M3dimInquire.md) and [`M_NEIGHBORHOOD_DISTANCE_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_DISTANCE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_DISTANCE_METRIC`

Inquires how to establish the neighborhood of a point, either as a spherical volume around the point ([`M_EUCLIDEAN`](../../Reference/3dim/M3dimInquire.md)), or as a distance in the Z-direction ([`M_DIRECTION_Z`](../../Reference/3dim/M3dimInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_DISTANCE_METRIC`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_DISTANCE_MODE`

Inquires the mode for computing a point's neighborhood distance.  > **Note:** This inquire type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_NUMBER_WITHIN_DISTANCE`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| `M_AUTO` *(default)* | Specifies to automatically compute the neighborhood distance. |
| `M_USER_DEFINED` | Specifies to use the distance set with [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimInquire.md). |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Inquires the neighborhood size when finding the neighbors of a point in an organized point cloud.  > **Note:** This inquire type is supported only when using an organized point cloud and only if [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NUMBER_NEIGHBORS`

Inquires the number of neighboring points used to calculate a point's local average distance distribution.  > **Note:** This inquire type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_LOCAL_DENSITY_PROBABILITY`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_NEIGHBORS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_OUTLIER_MODE`

Inquires the mode for determining whether a point is an outlier.

| Value | Description |
| --- | --- |
| *(see [`M_OUTLIER_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROBABILITY_THRESHOLD_FACTOR`

Inquires the scale factor to use in the probability calculation that determines whether a point is an outlier, when working in the [`M_LOCAL_DENSITY_PROBABILITY`](../../Reference/3dim/M3dimInquire.md) outlier mode.  > **Note:** This inquire type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_LOCAL_DENSITY_PROBABILITY`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_PROBABILITY_THRESHOLD_FACTOR`](Reference/3dim/M3dimControl.md))* |  |

#### `M_STD_DEVIATION_FACTOR`

Inquires the scale factor to use when calculating the local outlier threshold, when working in the [`M_LOCAL_DISTANCE`](../../Reference/3dim/M3dimInquire.md) outlier mode. When [`M_DISTANCE_THRESHOLD_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_STD_DEVIATION`](../../Reference/3dim/M3dimInquire.md), it is the factor multiplying the standard deviation of the local average distance distribution. When [`M_DISTANCE_THRESHOLD_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_ROBUST_STD_DEVIATION`](../../Reference/3dim/M3dimInquire.md), it is the factor multiplying the mean absolute deviation (MAD) of the local average distance distribution.  > **Note:** This inquire type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_LOCAL_DISTANCE`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_STD_DEVIATION_FACTOR`](Reference/3dim/M3dimControl.md))* |  |

---

### `Profile context ID`

Specifies a profile 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_PROFILE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) operations.

#### `M_PROFILE_ACCUMULATE_TYPE`

Inquires how points in a given sample are accumulated to create a single profile point when there are multiple points within the thickness of the profile line.

| Value | Description |
| --- | --- |
| *(see [`M_PROFILE_ACCUMULATE_TYPE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROFILE_INTERPOLATION_AVERAGE_FRACTION`

Inquires the fraction of the sampling distance to use when taking a weighted average of the area surrounding the profile point.

| Value | Description |
| --- | --- |
| *(see [`M_PROFILE_INTERPOLATION_AVERAGE_FRACTION`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROFILE_INTERPOLATION_MODE`

Inquires the interpolation mode used to calculate the value for each point in the sample that will be used to create the profile point.

| Value | Description |
| --- | --- |
| *(see [`M_PROFILE_INTERPOLATION_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROFILE_MIN_VALID_PERCENTAGE`

Inquires the minimum percentage of points in a given sample that must be valid to set the corresponding profile point to a valid value.

| Value | Description |
| --- | --- |
| *(see [`M_PROFILE_MIN_VALID_PERCENTAGE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROFILE_SAMPLE_SIZE`

Inquires the sampling distance between two consecutive points along the profile line.

| Value | Description |
| --- | --- |
| *(see [`M_PROFILE_SAMPLE_SIZE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROFILE_SAMPLE_SIZE_MODE`

Inquires how to interpret the specified sampling distance ([`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dim/M3dimInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_PROFILE_SAMPLE_SIZE_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROFILE_THICKNESS`

Inquires the thickness of the profile line (slicing plane).

| Value | Description |
| --- | --- |
| *(see [`M_PROFILE_THICKNESS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROFILE_THICKNESS_MODE`

Inquires how to interpret the specified thickness value ([`M_PROFILE_THICKNESS`](../../Reference/3dim/M3dimInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_PROFILE_THICKNESS_MODE`](Reference/3dim/M3dimControl.md))* |  |

---

### `Project context ID`

Specifies a project 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_PROJECT_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) operations.

#### `M_CALCULATE_SIZE_MODE`

Inquires whether the source point cloud container is organized.

| Value | Description |
| --- | --- |
| *(see [`M_CALCULATE_SIZE_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_CALIBRATION_ALIGNMENT`

Inquires where to place valid values in the depth map.

| Value | Description |
| --- | --- |
| *(see [`M_CALIBRATION_ALIGNMENT`](Reference/3dim/M3dimControl.md))* |  |

#### `M_DEPTH_MAP_TYPE`

Inquires the data type and depth required for the destination container's range component.

| Value | Description |
| --- | --- |
| *(see [`M_DEPTH_MAP_TYPE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_OVERLAP_MODE`

Inquires the overlap mode, which determines a pixel's gray level when multiple points correspond to the same destination depth map pixel.

| Value | Description |
| --- | --- |
| *(see [`M_OVERLAP_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PIXEL_ASPECT_RATIO`

Inquires the pixel aspect ratio of the depth map.

| Value | Description |
| --- | --- |
| *(see [`M_PIXEL_ASPECT_RATIO`](Reference/3dim/M3dimControl.md))* |  |
| `M_NULL` | Specifies that other data determines the pixel aspect ratio, such as the values set with [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimInquire.md) and [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimInquire.md). |

#### `M_PIXEL_SIZE_X`

Inquires the length in X of one pixel in the depth map.

| Value | Description |
| --- | --- |
| *(see [`M_PIXEL_SIZE_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PIXEL_SIZE_Y`

Inquires the length in Y of one pixel in the depth map.

| Value | Description |
| --- | --- |
| *(see [`M_PIXEL_SIZE_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROJECTED_COMPONENTS`

Inquires which components to project into the destination depth map.

| Value | Description |
| --- | --- |
| *(see [`M_PROJECTED_COMPONENTS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PROJECTION_MODE`

Inquires the projection mode.

| Value | Description |
| --- | --- |
| *(see [`M_PROJECTION_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SATURATION`

Inquires whether to saturate pixels.

| Value | Description |
| --- | --- |
| *(see [`M_SATURATION`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SIGN_Z`

Inquires the sign of the Z-scale in the destination depth map.

| Value | Description |
| --- | --- |
| *(see [`M_SIGN_Z`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SIZE_X`

Inquires the size of the depth map along the X dimension.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SIZE_Y`

Inquires the size of the depth map along the Y dimension.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y`](Reference/3dim/M3dimControl.md))* |  |

---

### `Remap context ID`

Specifies a remap 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_REMAP_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimRemapDepthMap`](../../Reference/3dim/M3dimRemapDepthMap.md) operations.

#### `M_DIRECTION_Z`

Inquires the sign of the Z-scale in the destination depth map.

| Value | Description |
| --- | --- |
| *(see [`M_DIRECTION_Z`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MAX_Z`

Inquires the maximum Z-value for the destination depth map when [`M_REMAP_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_MAX_Z`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MIN_Z`

Inquires the minimum Z-value for the destination depth map when [`M_REMAP_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_MIN_Z`](Reference/3dim/M3dimControl.md))* |  |

#### `M_REMAP_MODE`

Inquires how to determine the Z-range of the destination depth map.

| Value | Description |
| --- | --- |
| *(see [`M_REMAP_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_STANDARD_DEVIATION_FACTOR`

Inquires the standard deviation factor, for when [`M_REMAP_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_GAUSSIAN`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_STANDARD_DEVIATION_FACTOR`](Reference/3dim/M3dimControl.md))* |  |

---

### `Statistics context ID`

Specifies a statistics 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_STATISTICS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimStat`](../../Reference/3dim/M3dimStat.md) operations.

#### `M_BOUNDING_BOX`

Inquires whether to enable bounding box statistics calculations.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dim/M3dimControl.md))* |  |

#### `M_BOUNDING_BOX_ALGORITHM`

Inquires the algorithm to use when bounding box calculations have been enabled ([`M_BOUNDING_BOX`](../../Reference/3dim/M3dimInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX_ALGORITHM`](Reference/3dim/M3dimControl.md))* |  |

#### `M_BOUNDING_BOX_OUTLIER_RATIO_X`

Inquires the fraction of points to exclude from the bounding box, based on X-coordinates. A point is excluded when its X-coordinate places the point in an outlier position, and therefore outside the bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX_OUTLIER_RATIO_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_BOUNDING_BOX_OUTLIER_RATIO_Y`

Inquires the fraction of points to exclude from the bounding box, based on Y-coordinates. A point is excluded when its Y-coordinate places the point in an outlier position, and therefore outside the bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX_OUTLIER_RATIO_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_BOUNDING_BOX_OUTLIER_RATIO_Z`

Inquires the fraction of points to exclude from the bounding box, based on Z-coordinates. A point is excluded when its Z-coordinate places the point in an outlier position, and therefore outside the bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX_OUTLIER_RATIO_Z`](Reference/3dim/M3dimControl.md))* |  |

#### `M_BOX_ORIENTATION`

Inquires whether to compute an axis-aligned or semi-oriented bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_ORIENTATION`](Reference/3dim/M3dimControl.md))* |  |

#### `M_BOX_SEMI_ORIENTED_ROTATION_AXIS`

Inquires the axis with which to align the semi-oriented bounding box, when [`M_BOX_ORIENTATION`](../../Reference/3dim/M3dimInquire.md) is set to [`M_SEMI_ORIENTED`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_CALCULATE_AVERAGE`

Inquires whether to calculate the average value for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| *(see [`M_CALCULATE_AVERAGE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_CALCULATE_MAX`

Inquires whether to calculate the maximum value for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| *(see [`M_CALCULATE_MAX`](Reference/3dim/M3dimControl.md))* |  |

#### `M_CALCULATE_MEDIAN`

Inquires whether to calculate the median value for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| *(see [`M_CALCULATE_MEDIAN`](Reference/3dim/M3dimControl.md))* |  |

#### `M_CALCULATE_MIN`

Inquires whether to calculate the minimum value for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| *(see [`M_CALCULATE_MIN`](Reference/3dim/M3dimControl.md))* |  |

#### `M_CALCULATE_ROBUST_STDEV`

Inquires whether to calculate the robust standard deviation for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations. This is computed using the median absolute deviation (MAD) and a scale factor of 1.4826.

| Value | Description |
| --- | --- |
| *(see [`M_CALCULATE_ROBUST_STDEV`](Reference/3dim/M3dimControl.md))* |  |

#### `M_CALCULATE_STDEV`

Inquires whether to calculate the standard deviation for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| *(see [`M_CALCULATE_STDEV`](Reference/3dim/M3dimControl.md))* |  |

#### `M_CENTROID`

Inquires whether to enable centroid statistics calculations.

| Value | Description |
| --- | --- |
| *(see [`M_CENTROID`](Reference/3dim/M3dimControl.md))* |  |

#### `M_COMPONENT_OF_INTEREST`

Inquires the component for which to calculate statistics.

| Value | Description |
| --- | --- |
| *(see [`M_COMPONENT_OF_INTEREST`](Reference/3dim/M3dimControl.md))* |  |

#### `M_DISTANCE_TO_NEAREST_NEIGHBOR`

Inquires whether to enable distance-to-nearest-neighbor statistics calculations.

| Value | Description |
| --- | --- |
| *(see [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MAX_DISTANCE`

Inquires the maximum distance used to find neighbors when calculating distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_DISTANCE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MAXIMUM_NUMBER_NEIGHBORS`

Inquires the maximum number of neighbors to consider when calculating distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics.

| Value | Description |
| --- | --- |
| *(see [`M_MAXIMUM_NUMBER_NEIGHBORS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MOMENT_ORDER`

Inquires the order up to which moments are calculated.

| Value | Description |
| --- | --- |
| *(see [`M_MOMENT_ORDER`](Reference/3dim/M3dimControl.md))* |  |

#### `M_MOMENTS`

Inquires whether to enable moments statistics calculations.

| Value | Description |
| --- | --- |
| *(see [`M_MOMENTS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBOR_SEARCH_MODE`

Inquires the search mode for finding the neighbors of a point when calculating distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBOR_SEARCH_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_ASPECT_RATIO`

Inquires whether to enable local aspect ratio statistics calculations.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ASPECT_RATIO`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_ASPECT_RATIO_CLIPPING_VALUE`

Inquires the maximum value of the neighborhood aspect ratio, to avoid skewed statistics.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ASPECT_RATIO_CLIPPING_VALUE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_HEALTHY_THRESHOLD`

Inquires the minimum number of neighbors that constitute a healthy neighborhood for the [`M_NEIGHBORHOOD_HEALTHY_PERCENTAGE`](../../Reference/3dim/M3dimGetResult.md) result type ([`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md)).

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_HEALTHY_THRESHOLD`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Inquires the kernel size to use when using organized search mode for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NORMALIZATION_MODE`

Inquires whether the normalization matrix should transform the point cloud to fit a signed or unsigned unit box.

| Value | Description |
| --- | --- |
| *(see [`M_NORMALIZATION_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NORMALIZATION_SCALE`

Inquires how to apply scale values when transforming the point cloud or 3D geometry to fit a unit box. If set to [`M_UNIFORM`](../../Reference/3dim/M3dimInquire.md), each dimension is scaled using the same value such that the largest dimension fits a unit box, and the remaining dimensions will have the same or smaller lengths. If set to [`M_NON_UNIFORM`](../../Reference/3dim/M3dimInquire.md), scale values are applied such that all dimensions have the same length and fit a unit cube.

| Value | Description |
| --- | --- |
| *(see [`M_NORMALIZATION_SCALE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NUMBER_OF_POINTS`

Inquires whether to enable number-of-points statistics calculations.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_POINTS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NUMBER_OF_SAMPLES`

Inquires the number of sample points to use when calculating distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_SAMPLES`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NUMBER_OF_VALID_NEIGHBORS`

Inquires whether to enable number-of-valid-neighbors statistics calculations.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_VALID_NEIGHBORS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PCA`

Inquires whether to enable principal component analysis (PCA) statistics calculations.

| Value | Description |
| --- | --- |
| *(see [`M_PCA`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PCA_MODE`

Inquires whether to calculate the principal component analysis (PCA) around the origin ([`M_ORDINARY`](../../Reference/3dim/M3dimInquire.md)) or around the centroid ([`M_CENTRAL`](../../Reference/3dim/M3dimInquire.md)).

| Value | Description |
| --- | --- |
| *(see [`M_PCA_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SURFACE_AREA`

Inquires whether to calculate the surface area of the mesh.

| Value | Description |
| --- | --- |
| *(see [`M_SURFACE_AREA`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SURFACE_VARIATION`

Inquires whether to enable surface variation statistics calculations.

| Value | Description |
| --- | --- |
| *(see [`M_SURFACE_VARIATION`](Reference/3dim/M3dimControl.md))* |  |

---

### `Stitch context ID`

Specifies a stitch 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_STITCH_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) operations.

#### `M_CALIBRATION_MODE`

Inquires the calibration mode with which to map depth map pixel coordinates to real-world coordinates.

| Value | Description |
| --- | --- |
| *(see [`M_CALIBRATION_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_INTERPOLATION_FALLBACK_MODE`

Inquires the type of interpolation to use when stitching the depth maps and there are invalid pixels in the neighborhood of a point.

| Value | Description |
| --- | --- |
| *(see [`M_INTERPOLATION_FALLBACK_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_OFFSET_X`

Inquires the X-coordinate of the top-left corner of the destination depth map.  > **Note:** This inquire type is only used when [`M_CALIBRATION_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_OFFSET_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_OFFSET_Y`

Inquires the Y-coordinate of the top-left corner of the destination depth map.  > **Note:** This inquire type is only used when [`M_CALIBRATION_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_OFFSET_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_OVERLAP_MODE`

Inquires how to determine the pixel value when two or more depth maps overlap.

| Value | Description |
| --- | --- |
| *(see [`M_OVERLAP_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SIZE_X`

Inquires the size of the stitched destination depth map along the X dimension.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SIZE_Y`

Inquires the size of the stitched destination depth map along the Y dimension.

| Value | Description |
| --- | --- |
| *(see [`M_SIZE_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_STATIC_INDEX`

Inquires the source depth map to which all other depth map pixels will be stitched.

| Value | Description |
| --- | --- |
| *(see [`M_STATIC_INDEX`](Reference/3dim/M3dimControl.md))* |  |

#### `M_STITCH_DIRECTION`

Inquires the direction in which to stitch the point clouds' component values.

| Value | Description |
| --- | --- |
| *(see [`M_STITCH_DIRECTION`](Reference/3dim/M3dimControl.md))* |  |

#### `M_STITCH_MODE`

Inquires the algorithm to use for stitching depth maps.

| Value | Description |
| --- | --- |
| *(see [`M_STITCH_MODE`](Reference/3dim/M3dimControl.md))* |  |

---

### `Subsample context ID`

Specifies a subsample 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimSample`](../../Reference/3dim/M3dimSample.md) subsampling operations.

#### `M_DISTINCT_ANGLE_DIFFERENCE`

Inquires the minimum angle difference between the normal vector of a point and that of its neighbors, for the point to be kept in the subsample.

| Value | Description |
| --- | --- |
| *(see [`M_DISTINCT_ANGLE_DIFFERENCE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_FRACTION_OF_POINTS`

Inquires the fraction of all valid points to select from the source point cloud.  This only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_SUBSAMPLE_RANDOM`](../../Reference/3dim/M3dimInquire.md) or [`M_SUBSAMPLE_GEOMETRIC`](../../Reference/3dim/M3dimInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_FRACTION_OF_POINTS`](Reference/3dim/M3dimControl.md))* |  |

#### `M_GRID_SIZE_X`

Inquires the cell size in the X-direction.

| Value | Description |
| --- | --- |
| *(see [`M_GRID_SIZE_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_GRID_SIZE_Y`

Inquires the cell size in the Y-direction.

| Value | Description |
| --- | --- |
| *(see [`M_GRID_SIZE_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_GRID_SIZE_Z`

Inquires the cell size in the Z-direction.

| Value | Description |
| --- | --- |
| *(see [`M_GRID_SIZE_Z`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NEIGHBORHOOD_DISTANCE`

Inquires the distance that defines the neighborhood of a point; for a point to be kept in the subsample, it must be distinct in this neighborhood.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_DISTANCE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_ORGANIZATION_TYPE`

Inquires the resulting subsampled point cloud's organizational type.

| Value | Description |
| --- | --- |
| *(see [`M_ORGANIZATION_TYPE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_POINT_SELECTED`

Inquires which point to choose when multiple points occupy the same cell.

| Value | Description |
| --- | --- |
| `M_CENTER` | Specifies to select the point closest to the center of each 2D cell, as if the point cloud is collapsed onto the plane formed by the 2 non-infinite dimensions. |
| `M_MAX` | Specifies to select the highest point along the [`M_INFINITE`](../../Reference/3dim/M3dimInquire.md) axis, for each cell. This is the default setting for an [`M_ORGANIZED`](../../Reference/3dim/M3dimInquire.md) destination point cloud. |
| `M_MIN` | Specifies to select the lowest point along the [`M_INFINITE`](../../Reference/3dim/M3dimInquire.md) axis, for each cell. |

#### `M_SEED_VALUE`

Inquires the seed for the random selection of points, for an [`M_SUBSAMPLE_RANDOM`](../../Reference/3dim/M3dimInquire.md) operation.

| Value | Description |
| --- | --- |
| *(see [`M_SEED_VALUE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_STEP_SIZE_X`

Inquires the step interval between points, along the X dimension.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_SIZE_X`](Reference/3dim/M3dimControl.md))* |  |

#### `M_STEP_SIZE_Y`

Inquires the step interval between points, along the Y dimension.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_SIZE_Y`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SUBSAMPLE_MODE`

Inquires the type of subsampling operation to perform.

| Value | Description |
| --- | --- |
| *(see [`M_SUBSAMPLE_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SUBSAMPLE_NORMAL_MODE`

Inquires the algorithm to use for subsampling based on normals.

| Value | Description |
| --- | --- |
| *(see [`M_SUBSAMPLE_NORMAL_MODE`](Reference/3dim/M3dimControl.md))* |  |

---

### `Surface sample context ID`

Specifies a surface sample 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SURFACE_SAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimSample`](../../Reference/3dim/M3dimSample.md) surface sampling operations.

#### `M_RESOLUTION`

Inquires the resolution at which to sample the mesh or 3D geometry. This represents the distance between neighboring points on the surface; the smaller the resolution, the more points generated.

| Value | Description |
| --- | --- |
| *(see [`M_RESOLUTION`](Reference/3dim/M3dimControl.md))* |  |

#### `M_SAMPLE_MODE`

Inquires the mode of the surface sampling operation. The mode establishes whether the destination point cloud will have a mesh structure.

| Value | Description |
| --- | --- |
| *(see [`M_SAMPLE_MODE`](Reference/3dim/M3dimControl.md))* |  |

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

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

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

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.

> **Note:** This inquire type is available only when [`M_MESH_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimInquire.md).

> **Note:** This inquire type is only available when [`M_MESH_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_MESH_ORGANIZED`](../../Reference/3dim/M3dimInquire.md).

> **Note:** This inquire type is only available when [`M_MESH_MODE`](../../Reference/3dim/M3dimInquire.md) is set to [`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimInquire.md).
