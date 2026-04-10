---
doctype: Reference
module: 3dim
function: M3dimGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimGetResult"
---

# M3dimGetResult

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

> Get the specified type of result from a 3D image processing result buffer.

## Syntax

```c
AIL_DOUBLE M3dimGetResult(
    AIL_ID    Result3dimId,   //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the result(s) of the specified type from a result buffer.

## Parameters

### `Result3dimId` *(in, AIL_ID)*

Specifies the identifier of the 3D image processing result buffer from which to retrieve results. The result buffer must have been previously allocated on the system using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_FIND_TRANSFORMATION_RESULT`](../../Reference/3dim/M3dimAllocResult.md), [`M_LATTICE_RESULT`](../../Reference/3dim/M3dimAllocResult.md), [`M_PROFILE_RESULT`](../../Reference/3dim/M3dimAllocResult.md), or [`M_STATISTICS_RESULT`](../../Reference/3dim/M3dimAllocResult.md).

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the array in which to write the requested information.

## Parameter Associations

### For specifying the type of result to retrieve from a find transformation, profile, or statistics 3D image processing result buffer

The following [`Result3dimId`](../../Reference/3dim/M3dimGetResult.md), [`ResultType`](../../Reference/3dim/M3dimGetResult.md), and [`ResultArrayPtr`](../../Reference/3dim/M3dimGetResult.md) parameter settings can be specified for different types of 3D image processing result buffers.

---

### `Find transformation 3D image processing result ID`

Specifies the identifier of a find transformation 3D image processing result buffer.  For an [`M_FIND_TRANSFORMATION_RESULT`](../../Reference/3dim/M3dimAllocResult.md) result buffer, results are available after using [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md) to fill the result buffer with the rigid or affine transformation matrix.

#### `M_NUMBER_OF_POINT_PAIRS`

Retrieves the number of valid point pairs used when finding the transformation.

#### `M_RMS_ERROR`

Retrieves the root-mean-square (RMS) error between the point pairs if the matrix was applied. Aurora Imaging Library calculates the RMS error using the following formula:  *[Image: 3dim_FindTransformation_RMS_error.png]*

#### `M_STATUS`

Retrieves the status of the find transformation operation.

| Value | Description |
| --- | --- |
| `M_ALL_POINTS_COPLANAR` | Specifies that all point pairs are in the same plane and that the operation to find an affine transformation could not proceed. |
| `M_NOT_ENOUGH_POINT_PAIRS` | Specifies that there are either less than 3 point pairs (for a rigid transformation) or 4 point pairs (for an affine transformation) and that the operation could not proceed. |
| `M_NOT_INITIALIZED` | Specifies that the operation was not initialized. |
| `M_SUCCESS` | Specifies that the transformation matrix was successfully found. |

---

### `Lattice 3D image processing result ID`

Specifies the identifier of a lattice 3D image processing result buffer.  For an [`M_LATTICE_RESULT`](../../Reference/3dim/M3dimAllocResult.md) result buffer, results are available after using [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) to fill the result buffer with a lattice.

#### `M_CELL_SIZE_X`

Retrieves the cell size in the X-direction.  Note that if [`M_NUMBER_OF_GRID_SIZES`](../../Reference/3dim/M3dimGetResult.md) is greater than 1, this result depends on the grid size specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_GRID_SIZE_INDEX`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies an infinite cell size along X. |
| `Value > 0` | Specifies the cell size along X, in world units. |

#### `M_CELL_SIZE_Y`

Retrieves the cell size in the Y-direction.  Note that if [`M_NUMBER_OF_GRID_SIZES`](../../Reference/3dim/M3dimGetResult.md) is greater than 1, this result depends on the grid size specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_GRID_SIZE_INDEX`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies an infinite cell size along Y. |
| `Value > 0` | Specifies the cell size along Y, in world units. |

#### `M_CELL_SIZE_Z`

Retrieves the cell size in the Z-direction.  Note that if [`M_NUMBER_OF_GRID_SIZES`](../../Reference/3dim/M3dimGetResult.md) is greater than 1, this result depends on the grid size specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_GRID_SIZE_INDEX`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies an infinite cell size along Z. |
| `Value > 0` | Specifies the cell size along Z, in world units. |

#### `M_END_POINT_X`

Retrieves the X-coordinate of the lattice's end point, expressed in the working coordinate system.

#### `M_END_POINT_Y`

Retrieves the Y-coordinate of the lattice's end point, expressed in the working coordinate system.

#### `M_END_POINT_Z`

Retrieves the Z-coordinate of the lattice's end point, expressed in the working coordinate system.

#### `M_NUMBER_OF_CELLS_X`

Retrieves the number of cells in the X-direction.  Note that if [`M_NUMBER_OF_GRID_SIZES`](../../Reference/3dim/M3dimGetResult.md) is greater than 1, this result depends on the grid size specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_GRID_SIZE_INDEX`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of cells along X. |

#### `M_NUMBER_OF_CELLS_Y`

Retrieves the number of cells in the Y-direction.  Note that if [`M_NUMBER_OF_GRID_SIZES`](../../Reference/3dim/M3dimGetResult.md) is greater than 1, this result depends on the grid size specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_GRID_SIZE_INDEX`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of cells along Y. |

#### `M_NUMBER_OF_CELLS_Z`

Retrieves the number of cells in the Z-direction.  Note that if [`M_NUMBER_OF_GRID_SIZES`](../../Reference/3dim/M3dimGetResult.md) is greater than 1, this result depends on the grid size specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_GRID_SIZE_INDEX`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of cells along Z. |

#### `M_NUMBER_OF_GRID_SIZES`

Retrieves the number of possible grid sizes for the lattice. You can specify the index of the grid size to use, using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_GRID_SIZE_INDEX`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of grid sizes. |

#### `M_SIZE_X`

Retrieves the size of the lattice along the X-direction, in world units.

#### `M_SIZE_Y`

Retrieves the size of the lattice along the Y-direction, in world units.

#### `M_SIZE_Z`

Retrieves the size of the lattice along the Z-direction, in world units.

#### `M_SPARSITY_DIMENSION`

Retrieves the approximate number of dimensions in which points from the source point cloud are merged into each cell of the lattice. For example, if the source point cloud is much denser in X than in Y and Z, merging might only occur along the X-dimension when the grid size is small. As the grid size increases, the cells merge points along more dimensions.  Note that if [`M_NUMBER_OF_GRID_SIZES`](../../Reference/3dim/M3dimGetResult.md) is greater than 1, this result depends on the grid size specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_GRID_SIZE_INDEX`](../../Reference/3dim/M3dimControl.md).  This result type is only available when [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_LATTICE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SPARSITY_CHANGE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the dimensions could not be calculated because a bounding box containing a single point was supplied to [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md). |
| `Value > 0` | Specifies the approximate number of dimensions. |

#### `M_START_POINT_X`

Retrieves the X-coordinate of the lattice's start point, expressed in the working coordinate system.

#### `M_START_POINT_Y`

Retrieves the Y-coordinate of the lattice's start point, expressed in the working coordinate system.

#### `M_START_POINT_Z`

Retrieves the Z-coordinate of the lattice's start point, expressed in the working coordinate system.

#### `M_STATUS`

Retrieves the status of the lattice operation.

| Value | Description |
| --- | --- |
| `M_NOT_ENOUGH_VALID_DATA` | Specifies that the lattice operation failed because a bounding box was not provided and could not be calculated from the point cloud because there were not enough valid points (less than 2). |
| `M_NOT_INITIALIZED` | Specifies that the lattice 3D image processing result buffer was not used in a call to [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md), and contains no results. |
| `M_SUCCESS` | Specifies that the lattice operation completed successfully. |

---

### `Profile 3D image processing result ID`

Specifies the identifier of a profile 3D image processing result buffer.  For an [`M_PROFILE_RESULT`](../../Reference/3dim/M3dimAllocResult.md) result buffer, results are available after using [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) or [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) to fill the result buffer with a profile.  > **Note:** Note that you can transform coordinates from [`M_PROFILE_PLANE_...`](../../Reference/3dim/M3dimGetResult.md) to [`M_WORLD_...`](../../Reference/3dim/M3dimGetResult.md) (and vice versa). To do so, copy profile results into a transformation matrix, using [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_MATRIX_PROFILE_PLANE_TO_WORLD`](../../Reference/3dim/M3dimCopyResult.md) or [`M_MATRIX_WORLD_TO_PROFILE_PLANE`](../../Reference/3dim/M3dimCopyResult.md), and then pass the resulting transformation matrix to [`M3dimMatrixTransformList`](../../Reference/3dim/M3dimMatrixTransformList.md).  > **Note:** For depth maps, transforming [`M_WORLD_...`](../../Reference/3dim/M3dimGetResult.md) point coordinate values using [`McalTransformCoordinate3dList`](../../Reference/cal/McalTransformCoordinate3dList.md) will yield values close to those obtained with [`M_PROFILE_PLANE_...`](../../Reference/3dim/M3dimGetResult.md), but exactly on the profile line.

#### `M_NUMBER_OF_POINTS_MISSING_DATA`

Retrieves the number of missing data points (gap pixels) encountered when creating the depth map profile; for other profiles, this result type always returns 0.  This result type is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation.

#### `M_NUMBER_OF_POINTS_VALID`

Retrieves the number of valid points along the profile. Note that all extracted profile points are valid; therefore, this number equals the total number of points in the profile.  This result type is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation.

#### `M_PIXEL_X`

Retrieves the pixel X-coordinates for valid points included in the profile.  Each 3D point has a corresponding pixel in the depth map image buffer or point cloud range component. [`M_PIXEL_X`](../../Reference/3dim/M3dimGetResult.md) returns an integer value, for each profile point, which corresponds to the pixel's position along X in the depth map or range component.  > **Note:** Note that [`M_PIXEL_X`](../../Reference/3dim/M3dimGetResult.md) is unavailable for profiles of type [`M_PROFILE_MESH`](../../Reference/3dim/M3dimGetResult.md) and [`M_PROFILE_GEOMETRY`](../../Reference/3dim/M3dimGetResult.md).  This result type is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation.

#### `M_PIXEL_Y`

Retrieves the pixel Y-coordinates for valid points included in the profile.  Each 3D point has a corresponding pixel in the depth map image buffer or point cloud range component. [`M_PIXEL_Y`](../../Reference/3dim/M3dimGetResult.md) returns an integer value, for each profile point, which corresponds to the pixel's position along Y in the depth map or range component.  > **Note:** Note that [`M_PIXEL_Y`](../../Reference/3dim/M3dimGetResult.md) is unavailable for profiles of type [`M_PROFILE_MESH`](../../Reference/3dim/M3dimGetResult.md).  This result type is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation.

#### `M_PROFILE_PLANE_X`

Retrieves the X-coordinates from the resulting profile, expressed in the coordinate system of the slicing plane itself, in world units.  For a depth map profile, points are extracted from pixels along the specified line. [`M_PROFILE_PLANE_X`](../../Reference/3dim/M3dimGetResult.md) returns the real world distance along that line, for each extracted point. Note that the line's start point (set using [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) with [`M_PROFILE_DEPTH_MAP`](../../Reference/3dim/M3dimProfile.md) and [`M_PROFILE_DEPTH_MAP`](../../Reference/3dim/M3dimProfile.md) and [`M_PROFILE_DEPTH_MAP`](../../Reference/3dim/M3dimProfile.md), or using [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) with [`Param2`](../../Reference/3dim/M3dimProfileEx.md) and [`Param3`](../../Reference/3dim/M3dimProfileEx.md)) represents a distance of 0. The line is effectively the X-axis for a plot of [`M_PROFILE_PLANE_X`](../../Reference/3dim/M3dimGetResult.md) and [`M_PROFILE_PLANE_Y`](../../Reference/3dim/M3dimGetResult.md) values, which sit on the slicing plane.  For other profile types, the returned values correspond to the X-coordinates of the profile's points, expressed in the specified slicing plane's coordinate system.  You can draw the [`M_PROFILE_PLANE_X`](../../Reference/3dim/M3dimGetResult.md) and [`M_PROFILE_PLANE_Y`](../../Reference/3dim/M3dimGetResult.md) values in a 2D display using [`MgraDots`](../../Reference/gra/MgraDots.md). Alternatively, you can draw the profile points using [`M3dimDraw3d`](../../Reference/3dim/M3dimDraw3d.md).

#### `M_PROFILE_PLANE_Y`

Retrieves the Y-coordinates from the resulting profile, expressed in the coordinate system of the slicing plane itself, in world units.  For a depth map profile, the returned values correspond to the depth at each pixel underneath the specified line.  For other profile types, the returned values correspond to the Y-coordinates of the profile's points, expressed in the specified slicing plane's coordinate system.  You can draw the [`M_PROFILE_PLANE_X`](../../Reference/3dim/M3dimGetResult.md) and [`M_PROFILE_PLANE_Y`](../../Reference/3dim/M3dimGetResult.md) values in a 2D display using [`MgraDots`](../../Reference/gra/MgraDots.md). Alternatively, you can draw the profile points using [`M3dimDraw3d`](../../Reference/3dim/M3dimDraw3d.md).

#### `M_PROFILE_TYPE`

Retrieves the profile type.

| Value | Description |
| --- | --- |
| `M_NOT_INITIALIZED` | Specifies that a profile type has not been defined for the profile result. |
| `M_PROFILE_DEPTH_MAP` | Specifies a depth map profile.  This result is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation. |
| `M_PROFILE_DEPTH_MAP_UNIFORM` | Specifies a uniform depth map profile, where the points are at equal distances from each other along the profile line defined by the start and end points.  This result is available only after an [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) operation. |
| `M_PROFILE_GEOMETRY` | Specifies a 3D geometry profile.  This result is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation. |
| `M_PROFILE_MESH` | Specifies a mesh profile.  This result is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation. |
| `M_PROFILE_POINT_CLOUD` | Specifies a point cloud profile.  This result is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation. |

#### `M_STATUS`

Retrieves the status of the profile operation.

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the profile operation completed successfully. |
| `M_INFINITE_PROFILE` | Specifies that the profile operation failed and yielded an infinite result.  This result is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation. |
| `M_MISSING_COMPONENT_MESH_AIL` | Specifies that the profile operation failed because the source point cloud container does not have an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component.  This result is available only after an [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) operation. |
| `M_NO_PROFILE` | Specifies that no profile was found; the slicing plane did not intersect the source. |
| `M_NOT_INITIALIZED` | Specifies that the profile operation was not initialized. |

#### `M_WORLD_X`

Retrieves the world X-coordinates for the extracted points.  For a depth map, the returned values correspond to each extracted point's pixel position along X, in world units.  For a point cloud, mesh, or 3D geometry profile, the returned values correspond to each extracted point's position in the point cloud along X, in world units.  You can draw the [`M_WORLD_X`](../../Reference/3dim/M3dimGetResult.md), [`M_WORLD_Y`](../../Reference/3dim/M3dimGetResult.md), and [`M_WORLD_Z`](../../Reference/3dim/M3dimGetResult.md) values in a 3D display using [`M3dgraDots`](../../Reference/3dgra/M3dgraDots.md).

#### `M_WORLD_Y`

Retrieves the world Y-coordinates for the extracted points.  For a depth map, the returned values correspond to each extracted point's pixel position along Y, in world units.  For a point cloud, mesh, or 3D geometry profile, the returned values correspond to each extracted point's position in the point cloud along Y, in world units.  You can draw the [`M_WORLD_X`](../../Reference/3dim/M3dimGetResult.md), [`M_WORLD_Y`](../../Reference/3dim/M3dimGetResult.md), and [`M_WORLD_Z`](../../Reference/3dim/M3dimGetResult.md) values in a 3D display using [`M3dgraDots`](../../Reference/3dgra/M3dgraDots.md).

#### `M_WORLD_Z`

Retrieves the world Z-coordinates for the extracted points.  For a depth map, the returned values correspond to each extracted point's depth value (distance along Z), in world units.  For a point cloud, mesh, or 3D geometery profile, the returned values correspond to each extracted point's position in the point cloud along Z, in world units.  You can draw the [`M_WORLD_X`](../../Reference/3dim/M3dimGetResult.md), [`M_WORLD_Y`](../../Reference/3dim/M3dimGetResult.md), and [`M_WORLD_Z`](../../Reference/3dim/M3dimGetResult.md) values in a 3D display using [`M3dgraDots`](../../Reference/3dgra/M3dgraDots.md).

---

### `Statistics 3D image processing result ID`

Specifies the identifier of a statistics 3D image processing result buffer.  For an [`M_STATISTICS_RESULT`](../../Reference/3dim/M3dimAllocResult.md) result buffer, results are available after using [`M3dimStat`](../../Reference/3dim/M3dimStat.md) to fill the result buffer with statistics on a container's point cloud, on a depth map, or on a 3D geometry.

#### `?`

#### `?`

#### `M_BOX_CENTER_X`

Retrieves the X-coordinate of the center of the bounding box.

#### `M_BOX_CENTER_Y`

Retrieves the Y-coordinate of the center of the bounding box.

#### `M_BOX_CENTER_Z`

Retrieves the Z-coordinate of the center of the bounding box.

#### `M_CENTROID_X`

Retrieves the X-coordinate of the centroid.  This result type is available for centroid, moments, or PCA statistics results; moments results must be of order 1 or greater.

#### `M_CENTROID_Y`

Retrieves the Y-coordinate of the centroid.  This result type is available for centroid, moments, or PCA statistics results; moments results must be of order 1 or greater.

#### `M_CENTROID_Z`

Retrieves the Z-coordinate of the centroid.  This result type is available for centroid, moments, or PCA statistics results; moments results must be of order 1 or greater.

#### `M_COMPONENT_OF_INTEREST`

Retrieves the component for which the statistics were calculated.  This result type is available for centroid, moments, or PCA statistics results; moments results must be of order 1 or greater.

| Value | Description |
| --- | --- |
| `M_COMPONENT_NORMALS_AIL` | Specifies the normals component. |
| `M_COMPONENT_RANGE` | Specifies the range component. |

#### `M_COVARIANCE_MATRIX`

Retrieves the 9 entries that make up the covariance matrix, in row-major order. The covariance matrix is defined as a 3x3 matrix of second order moments:  |   |   |   | | --- | --- | --- | | xx | xy | xz | | xy | yy | yz | | xz | yz | zz |  The moments are central or ordinary, according to [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_PCA_MODE`](../../Reference/3dim/M3dimControl.md).  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_DISTANCE_TO_NEAREST_NEIGHBOR_AVERAGE`

Retrieves the average distance to the nearest neighbor.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_AVERAGE`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_DISTANCE_TO_NEAREST_NEIGHBOR_MAX`

Retrieves the maximum distance to the nearest neighbor.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MAX`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_DISTANCE_TO_NEAREST_NEIGHBOR_MEDIAN`

Retrieves the median distance to the nearest neighbor.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MEDIAN`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_DISTANCE_TO_NEAREST_NEIGHBOR_MIN`

Retrieves the minimum distance to the nearest neighbor.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MIN`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_DISTANCE_TO_NEAREST_NEIGHBOR_ROBUST_STDEV`

Retrieves the robust standard deviation of distances between each point and its nearest neighbor. [`M_DISTANCE_TO_NEAREST_NEIGHBOR_ROBUST_STDEV`](../../Reference/3dim/M3dimGetResult.md) is computed using the median absolute deviation (MAD) and a scale factor of 1.4826.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_ROBUST_STDEV`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_DISTANCE_TO_NEAREST_NEIGHBOR_STDEV`

Retrieves the standard deviation of distances between each point and its nearest neighbor.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_STDEV`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_EIGENVALUE_1`

Retrieves the eigenvalue corresponding to the first principal axis. The first principal axis is the direction along which there is the most variance.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_EIGENVALUE_2`

Retrieves the eigenvalue corresponding to the second principal axis. The second principal axis is the direction along which there is the second most variance, perpendicular to the first principal axis.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_EIGENVALUE_3`

Retrieves the eigenvalue corresponding to the third principal axis. The third principal axis is perpendicular to the first and second principal axes.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_MAX_X`

Retrieves the maximum X-coordinate of the bounding box.  > **Note:** This result type is not available for a semi-oriented bounding box, unless [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](../../Reference/3dim/M3dimControl.md) is set to [`M_AXIS_X`](../../Reference/3dim/M3dimControl.md).

#### `M_MAX_Y`

Retrieves the maximum Y-coordinate of the bounding box.  > **Note:** This result type is not available for a semi-oriented bounding box, unless [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](../../Reference/3dim/M3dimControl.md) is set to [`M_AXIS_Y`](../../Reference/3dim/M3dimControl.md).

#### `M_MAX_Z`

Retrieves the maximum Z-coordinate of the bounding box.  > **Note:** This result type is not available for a semi-oriented bounding box, unless [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](../../Reference/3dim/M3dimControl.md) is set to [`M_AXIS_Z`](../../Reference/3dim/M3dimControl.md).

#### `M_MIN_X`

Retrieves the minimum X-coordinate of the bounding box.  > **Note:** This result type is not available for a semi-oriented bounding box, unless [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](../../Reference/3dim/M3dimControl.md) is set to [`M_AXIS_X`](../../Reference/3dim/M3dimControl.md).

#### `M_MIN_Y`

Retrieves the minimum Y-coordinate of the bounding box.  > **Note:** This result type is not available for a semi-oriented bounding box, unless [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](../../Reference/3dim/M3dimControl.md) is set to [`M_AXIS_Y`](../../Reference/3dim/M3dimControl.md).

#### `M_MIN_Z`

Retrieves the minimum Z-coordinate of the bounding box.  > **Note:** This result type is not available for a semi-oriented bounding box, unless [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](../../Reference/3dim/M3dimControl.md) is set to [`M_AXIS_Z`](../../Reference/3dim/M3dimControl.md).

#### `M_NEIGHBORHOOD_ASPECT_RATIO_AVERAGE`

Retrieves the average local aspect ratio, across all points.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_AVERAGE`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NEIGHBORHOOD_ASPECT_RATIO_MAX`

Retrieves the maximum local aspect ratio, across all points. Note that this value is limited by [`M_NEIGHBORHOOD_ASPECT_RATIO_CLIPPING_VALUE`](../../Reference/3dim/M3dimControl.md).  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MAX`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NEIGHBORHOOD_ASPECT_RATIO_MEDIAN`

Retrieves the median local aspect ratio, across all points.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MEDIAN`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NEIGHBORHOOD_ASPECT_RATIO_MIN`

Retrieves the minimum local aspect ratio, across all points.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MIN`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NEIGHBORHOOD_ASPECT_RATIO_ROBUST_STDEV`

Retrieves the robust standard deviation of local aspect ratios, across all points. [`M_NEIGHBORHOOD_ASPECT_RATIO_ROBUST_STDEV`](../../Reference/3dim/M3dimGetResult.md) is computed using the median absolute deviation (MAD) and a scale factor of 1.4826.  For each point, Aurora Imaging Library fits a local plane to neighboring points, determines the plane's aspect ratio, and incorporates the local aspect ratio into the robust standard deviation calculation.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_ROBUST_STDEV`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NEIGHBORHOOD_ASPECT_RATIO_STDEV`

Retrieves the standard deviation of local aspect ratios, across all points.  For each point, Aurora Imaging Library fits a local plane to neighboring points, determines the plane's aspect ratio, and incorporates the local aspect ratio into the standard deviation calculation.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_STDEV`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NEIGHBORHOOD_HEALTHY_PERCENTAGE`

Retrieves the percentage of points that have a healthy neighborhood. This is the percentage of points with at least [`M_NEIGHBORHOOD_HEALTHY_THRESHOLD`](../../Reference/3dim/M3dimControl.md) valid neighbors. Most neighborhood-based functions (for example, [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md)) will work poorly if this value is too low.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) control type.

#### `M_NORMALIZATION_MODE`

Retrieves the mode used to compute the normalization matrix.  This result type is available for bounding box statistics results; the bounding box must be axis-aligned ([`M_BOX_ORIENTATION`](../../Reference/3dim/M3dimControl.md) set to [`M_AXIS_ALIGNED`](../../Reference/3dim/M3dimControl.md)).

| Value | Description |
| --- | --- |
| `M_NORMALIZE_SIGNED` | Specifies that the point cloud fits a signed unit box. |
| `M_NORMALIZE_UNSIGNED` | Specifies that the point cloud fits an unsigned unit box. |

#### `M_NORMALIZATION_SCALE`

Retrieves the scale mode used in the statistics context to compute the normalization matrix.  This result type is available for bounding box statistics results; the bounding box must be axis-aligned ([`M_BOX_ORIENTATION`](../../Reference/3dim/M3dimControl.md) set to [`M_AXIS_ALIGNED`](../../Reference/3dim/M3dimControl.md)).

| Value | Description |
| --- | --- |
| `M_NON_UNIFORM` | Specifies that each dimension was scaled differently. |
| `M_UNIFORM` | Specifies that the same scale value was used for all dimensions. This means that the largest dimension was scaled to fit the unit box, and the remaining dimensions have the same or smaller lengths. |

#### `M_NUMBER_OF_POINTS_ISOLATED`

Retrieves the number of isolated points found during the distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics calculations. Isolated points are points with no neighbors except themselves (as defined by the neighborhood controls, [`M_MAXIMUM_NUMBER_NEIGHBORS`](../../Reference/3dim/M3dimControl.md), [`M_MAX_DISTANCE`](../../Reference/3dim/M3dimControl.md), [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md), and [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](../../Reference/3dim/M3dimControl.md)).  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md), [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md), [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md), or [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NUMBER_OF_POINTS_MISSING_DATA`

Retrieves the number of points that are missing data.  This result type is available for centroid, moments, number-of-points, or PCA statistics results.

#### `M_NUMBER_OF_POINTS_REPLICATED`

Retrieves the number of replicated points found during the distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics calculations. Replicated points are distinct points with the same XYZ-coordinates.  Note that replicated points are automatically ignored when calculating distance-to-nearest-neighbor statistics; however, replicated points can have a significant effect on surface variation, aspect ratio, and number-of-valid-neighbor results.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md), [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md), [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md), or [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NUMBER_OF_POINTS_TOTAL`

Retrieves the total number of points.  This result type is available for centroid, moments, number-of-points, or PCA statistics results.

#### `M_NUMBER_OF_POINTS_VALID`

Retrieves the number of valid points. Valid points are those with non-zero confidence.  This result type is available for centroid, moments, number-of-points, or PCA statistics results.

#### `M_NUMBER_OF_VALID_NEIGHBORS_AVERAGE`

Retrieves the average number of valid neighbors, across all points. Valid neighbors are those with non-zero confidence.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_AVERAGE`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NUMBER_OF_VALID_NEIGHBORS_MAX`

Retrieves the maximum number of valid neighbors, across all points. Valid neighbors are those with non-zero confidence.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MAX`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NUMBER_OF_VALID_NEIGHBORS_MEDIAN`

Retrieves the median number of valid neighbors, across all points. Valid neighbors are those with non-zero confidence.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MEDIAN`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NUMBER_OF_VALID_NEIGHBORS_MIN`

Retrieves the minimum number of valid neighbors, across all points. Valid neighbors are those with non-zero confidence.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MIN`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NUMBER_OF_VALID_NEIGHBORS_ROBUST_STDEV`

Retrieves the robust standard deviation of each point's number of valid neighbors. Valid neighbors are those with non-zero confidence. [`M_NUMBER_OF_VALID_NEIGHBORS_ROBUST_STDEV`](../../Reference/3dim/M3dimGetResult.md) is computed using the median absolute deviation (MAD) and a scale factor of 1.4826.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_ROBUST_STDEV`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_NUMBER_OF_VALID_NEIGHBORS_STDEV`

Retrieves the standard deviation of each point's number of valid neighbors. Valid neighbors are those with non-zero confidence.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_STDEV`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_PCA_MODE`

Retrieves whether the results for principal component analysis (PCA) or moments statistics were measured relative to the object's centroid ([`M_CENTRAL`](../../Reference/3dim/M3dimControl.md), default) or the origin ([`M_ORDINARY`](../../Reference/3dim/M3dimControl.md)).  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

| Value | Description |
| --- | --- |
| *(see [`M_PCA_MODE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_PRINCIPAL_AXIS_1_X`

Retrieves the X-component of the normalized vector along the first principal axis, which is the axis with the largest eigenvalue.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_PRINCIPAL_AXIS_1_Y`

Retrieves the Y-component of the normalized vector along the first principal axis, which is the axis with the largest eigenvalue.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_PRINCIPAL_AXIS_1_Z`

Retrieves the Z-component of the normalized vector along the first principal axis, which is the axis with the largest eigenvalue.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_PRINCIPAL_AXIS_2_X`

Retrieves the X-component of the normalized vector along the second principal axis, which is the axis with the second largest eigenvalue. The second principal axis is perpendicular to the first principal axis.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_PRINCIPAL_AXIS_2_Y`

Retrieves the Y-component of the normalized vector along the second principal axis, which is the axis with the second largest eigenvalue. The second principal axis is perpendicular to the first principal axis.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_PRINCIPAL_AXIS_2_Z`

Retrieves the Z-component of the normalized vector along the second principal axis, which is the axis with the second largest eigenvalue. The second principal axis is perpendicular to the first principal axis.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_PRINCIPAL_AXIS_3_X`

Retrieves the X-component of the normalized vector along the third principal axis, which is the axis perpendicular to the first and second principal axes.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_PRINCIPAL_AXIS_3_Y`

Retrieves the Y-component of the normalized vector along the third principal axis, which is the axis perpendicular to the first and second principal axes.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_PRINCIPAL_AXIS_3_Z`

Retrieves the Z-component of the normalized vector along the third principal axis, which is the axis perpendicular to the first and second principal axes.  This result type is available for moments or PCA statistics results; moments results must be of order 2 or greater.

#### `M_SEMI_ORIENTED_BOX_ANGLE`

Retrieves the angle of the semi-oriented bounding box's rotation around the axis specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](../../Reference/3dim/M3dimControl.md), respecting the right-hand rule.  This result type is available for bounding box statistics results.

#### `M_SIZE_X`

Retrieves the length along X of the 3D scene, in world units.  This result type is available for bounding box statistics results.

#### `M_SIZE_Y`

Retrieves the length along Y of the 3D scene, in world units.  This result type is available for bounding box statistics results.

#### `M_SIZE_Z`

Retrieves the length along Z of the 3D scene, in world units.  This result type is available for bounding box statistics results.

#### `M_STANDARD_DEVIATION_X`

Retrieves the standard deviation in X.  This result type is available for moments statistics results of order 2 or greater.

#### `M_STANDARD_DEVIATION_Y`

Retrieves the standard deviation in Y.  This result type is available for moments statistics results of order 2 or greater.

#### `M_STANDARD_DEVIATION_Z`

Retrieves the standard deviation in Z.  This result type is available for moments statistics results of order 2 or greater.

#### `M_STATUS_NEIGHBOR_STATS`

Retrieves whether replicated points were found while calculating neighborhood statistics, as such these statistics might be skewed. Replicated points are distinct points with the same XYZ-coordinates.  Note that replicated points are automatically ignored when calculating distance-to-nearest-neighbor statistics; however, replicated points can have a significant effect on surface variation, aspect ratio, and number-of-valid-neighbors results. If you have enabled the [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md), [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md), or [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) control type and replicated points were found, you can use [`M3dimFix`](../../Reference/3dim/M3dimFix.md) with [`M_UNREPLICATED_POINTS`](../../Reference/3dim/M3dimFix.md) to remove the replicated points, and then recalculate the statistics.  You can use [`M_NUMBER_OF_POINTS_REPLICATED`](../../Reference/3dim/M3dimGetResult.md) to check the number of replicated points that were found.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dim/M3dimControl.md), [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md), [`M_NEIGHBORHOOD_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md), or [`M_NUMBER_OF_VALID_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) control types.

| Value | Description |
| --- | --- |
| `M_REPLICATED_POINTS` | Specifies that replicated points were found; the calculated statistics might be skewed. |
| `M_SUCCESS` | Specifies that no replicated points were found. |

#### `M_SURFACE_AREA`

Retrieves the surface area of the mesh.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_SURFACE_AREA`](../../Reference/3dim/M3dimControl.md) control type.

#### `M_SURFACE_VARIATION_AVERAGE`

Retrieves the average distance to the local plane, across all points. This is the average of each point's calculated distance to its local plane, not an average of neighborhood point distances to a single plane.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_AVERAGE`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_SURFACE_VARIATION_MAX`

Retrieves the maximum distance to the local plane, across all points.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MAX`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_SURFACE_VARIATION_MEDIAN`

Retrieves the median distance to the local plane, across all points.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MEDIAN`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_SURFACE_VARIATION_MIN`

Retrieves the minimum distance to the local plane, across all points.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_MIN`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_SURFACE_VARIATION_ROBUST_STDEV`

Retrieves the robust standard deviation of point distances to their local plane. For each point, Aurora Imaging Library fits a local plane to neighboring points, measures the perpendicular distance of the point to the plane, and incorporates the distance into the robust standard deviation calculation. [`M_SURFACE_VARIATION_ROBUST_STDEV`](../../Reference/3dim/M3dimGetResult.md) is computed using the median absolute deviation (MAD) and a scale factor of 1.4826.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_ROBUST_STDEV`](../../Reference/3dim/M3dimControl.md) control types.

#### `M_SURFACE_VARIATION_STDEV`

Retrieves the standard deviation of point distances to their local plane. For each point, Aurora Imaging Library fits a local plane to neighboring points, measures the perpendicular distance of the point to the plane, and incorporates the distance into the standard deviation calculation.  This result is available only when you have enabled the [`M3dimControl`](../../Reference/3dim/M3dimControl.md) [`M_SURFACE_VARIATION`](../../Reference/3dim/M3dimControl.md) and [`M_CALCULATE_STDEV`](../../Reference/3dim/M3dimControl.md) control types.

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

**Type:** `AIL_DOUBLE`

The returned value is the requested information, cast to an _AIL_DOUBLE_. If the requested information does not fit into an _AIL_DOUBLE_, this function will return `M_NULL` or truncate the information.

This result type is available for bounding box statistics results. The bounding box is an axis-aligned or semi-oriented box that contains all or most of the points in the 3D scene, or it encloses a 3D geometry object.

For each point, Aurora Imaging Library fits a local plane to neighboring points and measures the perpendicular distance of the point to the plane.

For each point, Aurora Imaging Library fits a local plane to neighboring points and determines the plane's aspect ratio.
