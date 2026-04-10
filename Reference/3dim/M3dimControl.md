---
doctype: Reference
module: 3dim
function: M3dimControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimControl"
---

# M3dimControl

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

> Control a 3D image processing context or result buffer.

## Syntax

```c
void M3dimControl(
    AIL_ID     Context3dimId,  //out
    AIL_INT64  ControlType,    //in
    AIL_DOUBLE ControlValue    //in
)
```

## Description

This function controls a general setting of a 3D image processing context or lattice 3D image processing result buffer. You can typically inquire these settings using [`M3dimInquire`](../../Reference/3dim/M3dimInquire.md).

## Parameters

### `Context3dimId` *(out, AIL_ID)*

Specifies the identifier of the 3D image processing context or result buffer to control. The context or result buffer must have been previously allocated on the system using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) or [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md).

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

## Parameter Associations

### For specifying the control type and control value for a 3D image processing context or result buffer

The following [`Context3dimId`](../../Reference/3dim/M3dimControl.md), [`ControlType`](../../Reference/3dim/M3dimControl.md), and [`ControlValue`](../../Reference/3dim/M3dimControl.md) parameter settings can be specified for different types of 3D image processing contexts or a lattice 3D image processing result buffer.

---

### `Calculate map size context ID`

Specifies a calculate map size 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_CALCULATE_MAP_SIZE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) operations.  The recommended size is useful for allocating a depth map image buffer (using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md)) before calling [`M3dimProject`](../../Reference/3dim/M3dimProject.md).

#### `M_CALCULATE_MODE`

Sets whether the source point cloud container is organized.[`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) calculations are faster with organized point clouds.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ORGANIZED` | Specifies an organized point cloud.  > **Note:** Note that using this value with an unorganized point will cause an error. |
| `M_UNORGANIZED` *(default)* | Specifies an unorganized point cloud. |

#### `M_PIXEL_ASPECT_RATIO`

Sets the pixel aspect ratio of the depth map.  > **Note:** This control type must be set to [`M_NULL`](../../Reference/3dim/M3dimControl.md) if you have already specified a size in X with [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md) and a size in Y with [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md). Otherwise, an error will occur.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` | Specifies that other data determines the pixel aspect ratio, such as the values set with [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) and [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md). |
| `Value > 0.0` *(default)* | Specifies an aspect ratio for the depth map's pixels. The pixel size is computed such that the pixel's length in X divided by its length in Y equals the specified [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) value. |

#### `M_PIXEL_SIZE_CONSTRAINT`

Sets how to calculate the depth map pixel sizes to fit the required pixel aspect ratio ([`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md)).  > **Note:** This control type is supported only when [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) is not set to [`M_NULL`](../../Reference/3dim/M3dimControl.md) and [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md), [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md), and [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md) are all set to [`M_NULL`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INCREASE_PIXEL_SIZE` | Specifies to calculate the pixel sizes such that the pixel size in one dimension is the same as in the source container and the pixel size in the other dimension is increased to fit the required pixel aspect ratio. The dimension chosen from the point cloud lattice is the one that results in the largest pixels given the required pixel aspect ratio. This generates a smaller depth map that might have missing data.  > **Note:** Note that this mode might overestimate the pixel size when an organized image has some pixel rotation, especially if the pixel aspect ratio is large. |
| `M_PRESERVE_WORLD_AREA` *(default)* | Specifies to calculate the pixel sizes such that the area of a pixel is the same as in the source container. For point clouds whose lattice aspect ratio is much different than the required depth map pixel aspect ratio, this can result in data loss in one dimension and gaps in the other dimension. |
| `M_REDUCE_PIXEL_SIZE` | Specifies to calculate the pixel sizes such that the pixel size in one dimension is the same as in the source container and the pixel size in the other dimension is decreased to fit the required pixel aspect ratio. The dimension chosen from the point cloud lattice is the one that results in the smallest pixels given the required pixel aspect ratio. This generates a larger depth map that might have gaps.  > **Note:** Note that this mode might overestimate the pixel size when an organized image has some pixel rotation, especially if the pixel aspect ratio is large. |

#### `M_PIXEL_SIZE_X`

Sets the length in X of one pixel in the depth map.  > **Note:** Note that you cannot set [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) at the same time as [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md), unless [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) is set to [`M_NULL`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that you cannot specify a specific value for [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) if you have done so for [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md) (and vice versa).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies that [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) will compute the pixel size in X. |
| `Value > 0.0` | Specifies the length in X of one pixel in the depth map, in world units. |

#### `M_PIXEL_SIZE_Y`

Sets the length in Y of one pixel in the depth map.  > **Note:** Note that you cannot set [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) at the same time as [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md), unless [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) is set to [`M_NULL`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that you cannot specify a specific value for [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) if you have done so for [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md) (and vice versa).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies that [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) will compute the pixel size in Y. |
| `Value > 0.0` | Specifies the length in Y of one pixel in the depth map, in world units. |

#### `M_SIZE_X`

Sets the image size of the depth map along the X dimension.  > **Note:** Note that you cannot set [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md) at the same time as [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md), unless [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) is set to [`M_NULL`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that you cannot specify a specific value for [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md) if you have done so for [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) (and vice versa).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies that [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) will compute the image size of the depth map in X. |
| `Value > 0.0` | Specifies the image size of the depth map in X, in pixel units. |

#### `M_SIZE_Y`

Sets the image size of the depth map along the Y dimension.  > **Note:** Note that you cannot set [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md) at the same time as [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md), unless [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) is set to [`M_NULL`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that you cannot specify a specific value for [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md) if you have done so for [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) (and vice versa).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies that [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) will compute the image size of the depth map in Y. |
| `Value > 0.0` | Specifies the image size of the depth map in Y, in pixel units. |

---

### `Fill gaps context ID`

Specifies a fill gaps 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_FILL_GAPS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimFillGaps`](../../Reference/3dim/M3dimFillGaps.md) operations. Gaps are missing or invalid data in a depth or intensity map.

#### `M_FILL_BORDER`

Sets whether to propagate the boundary value across each gap that touches the image border.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to fill gaps that touch the image border. |
| `M_REPLICATE` *(default)* | Specifies to propagate the unique gap boundary value across each gap that touches the image border, replacing gap pixels up to the image border. |

#### `M_FILL_MODE`

Sets the mode in which to fill gaps in depth maps.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_X_THEN_Y` *(default)* | Specifies that each depth map row is analyzed so that gap pixels in each row are filled, and then each column is analyzed to fill the remaining gaps. The filling operation used depends on the [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md) and [`M_FILL_SHARP_ELEVATION`](../../Reference/3dim/M3dimControl.md) control types.  You can specify how to fill gaps that touch the image border with [`M_FILL_BORDER`](../../Reference/3dim/M3dimControl.md). |
| `M_Y_THEN_X` | Specifies that each depth map column is analyzed so that gap pixels in each column are filled, and then each row is analyzed to fill the remaining gaps. The filling operation used depends on the [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md) and [`M_FILL_SHARP_ELEVATION`](../../Reference/3dim/M3dimControl.md) control types.  You can specify how to fill gaps that touch the image border with [`M_FILL_BORDER`](../../Reference/3dim/M3dimControl.md). |

#### `M_FILL_SHARP_ELEVATION`

Sets which boundary to use to fill sharp elevation gaps in depth maps. You can specify to propagate either the minimum or maximum gap border value, or not to fill sharp elevation gaps.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to fill sharp elevation gaps. |
| `M_MAX` | Specifies to set the destination pixel(s) to the maximum value of the two pixels on either side of the gap. |
| `M_MIN` | Specifies to set the destination pixel(s) to the minimum value of the two pixels on either side of the gap. |

#### `M_FILL_SHARP_ELEVATION_DEPTH`

Sets the threshold used to differentiate between gradual elevation gaps and sharp elevation gaps in the depth map. If the difference between a gap's boundary values is less than the specified threshold, the gap is considered a gradual elevation gap, otherwise it is considered a sharp elevation gap.  Gradual elevation gaps are filled using linear interpolation; sharp elevation gaps are either filled by propagating the value of one of the boundaries or are left unfilled, depending on the[`M_FILL_SHARP_ELEVATION`](../../Reference/3dim/M3dimControl.md) control type. It is often better to fill gradual elevation gaps using linear interpolation since the underlying surface of such a gap is considered to be part of the same surface as its boundaries. Whereas for sharp elevation gaps, the underlying surface is considered to be discontinuous at least at one of its boundaries, so propagating the value of one of the boundaries is recommended.  If a gap is greater than [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md), it is replaced using the secondary algorithm described in[`M_FILL_SHARP_ELEVATION`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the threshold is infinite.  When[`M_INFINITE`](../../Reference/3dim/M3dimControl.md) is specified, all gaps are considered gradual elevation gaps, so linear interpolation is always used and[`M_FILL_SHARP_ELEVATION`](../../Reference/3dim/M3dimControl.md) is ignored. |
| `Value >= 0.0` | Specifies the threshold, in pixel or world units, depending on the units specified with [`M_INPUT_UNITS`](../../Reference/3dim/M3dimControl.md).  When the threshold is set to 0.0, all gaps are considered sharp elevation gaps, so linear interpolation is never used and all gaps are filled according to[`M_FILL_SHARP_ELEVATION`](../../Reference/3dim/M3dimControl.md). |

#### `M_FILL_THRESHOLD_X`

Sets the maximum X-size of gaps that will be filled.  If a gap is larger than the fill threshold in the X or Y direction, it is not replaced.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no maximum in the X-direction. |
| `Value >= 0.0` | Specifies the maximum X-size, in pixel or world units, depending on the units specified with [`M_INPUT_UNITS`](../../Reference/3dim/M3dimControl.md). |

#### `M_FILL_THRESHOLD_Y`

Sets the maximum Y-size of gaps that will be filled.  If a gap is larger than the fill threshold in the X or Y direction, it is not replaced.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no maximum in the Y-direction. |
| `Value >= 0.0` | Specifies the maximum Y-size, in pixel or world units, depending on the units specified with [`M_INPUT_UNITS`](../../Reference/3dim/M3dimControl.md). |

#### `M_INPUT_UNITS`

Sets the units with which to interpret the [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md), [`M_FILL_THRESHOLD_X`](../../Reference/3dim/M3dimControl.md), and [`M_FILL_THRESHOLD_Y`](../../Reference/3dim/M3dimControl.md) control types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` | Specifies to interpret the values in pixel units. |
| `M_WORLD` *(default)* | Specifies to interpret the values in world units. |

---

### `Filter context ID`

Specifies a filter 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_FILTER_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md) operations.

#### `M_DISTANCE_WEIGHT`

Sets the amount of smoothing. A larger value applies more smoothing than a smaller value.  This control type affects the calculation of the distance Gaussian weight, which is used in the smoothing computation and applies to the distance between points in a neighborhood; the exact calculation depends on the specified weight mode.  If [`M_WEIGHT_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ABSOLUTE`](../../Reference/3dim/M3dimControl.md), a point's distance Gaussian weight is set equal to [`M_DISTANCE_WEIGHT`](../../Reference/3dim/M3dimControl.md).  If [`M_WEIGHT_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_RELATIVE`](../../Reference/3dim/M3dimControl.md), the distance Gaussian weight is set to `_M_DISTANCE_WEIGHT_ * _d_`, where _d_ is an internal value calculated from a point's distance to its neighbors and the number of neighborhood points.  If you are unsure of exact values to use, you can set [`M_WEIGHT_MODE`](../../Reference/3dim/M3dimControl.md) to [`M_RELATIVE`](../../Reference/3dim/M3dimControl.md), and Aurora Imaging Library will find a heuristic value as a function of the calculated point density, and then you can increase or decrease the multiplicative factors ([`M_NORMALS_WEIGHT_FACTOR`](../../Reference/3dim/M3dimControl.md) and [`M_DISTANCE_WEIGHT`](../../Reference/3dim/M3dimControl.md)) to control the degree of smoothing.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the amount of smoothing. |

#### `M_FILTER_MODE`

Sets the type of smoothing operation to perform.  Deciding which mode to use depends on your data and application. In general, [`M_SMOOTH_MLS`](../../Reference/3dim/M3dimControl.md) performs a simple averaging, while [`M_SMOOTH_BILATERAL`](../../Reference/3dim/M3dimControl.md) conserves edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SMOOTH_BILATERAL` *(default)* | Specifies to smooth the point cloud using a bilateral filter.  A bilateral filter smooths a point cloud while preserving its features; it is effectively an edge-preserving filter. This operation examines a point's distance to its neighbors as well as its distance along a normal direction, to which Gaussian weights are applied. You can control the degree of smoothing and edge preservation with [`M_DISTANCE_WEIGHT`](../../Reference/3dim/M3dimControl.md) and [`M_NORMALS_WEIGHT_FACTOR`](../../Reference/3dim/M3dimControl.md), respectively. |
| `M_SMOOTH_MLS` | Specifies to smooth the point cloud using a moving least squares (MLS) filter.  MLS performs a local plane fit at each point, projecting the point onto the locally fitted tangent plane. |

#### `M_NORMALS_WEIGHT_FACTOR`

Sets how well edges are preserved for an [`M_SMOOTH_BILATERAL`](../../Reference/3dim/M3dimControl.md) operation. A smaller value results in more edge preservation. This control type sets the point normal Gaussian weight (_σ_ <sub>normal</sub>) relative to the distance Gaussian weight (_σ_ <sub>distance</sub>), according to the following equation: `_σ_ <sub>normal</sub> = [`M_NORMALS_WEIGHT_FACTOR`](../../Reference/3dim/M3dimControl.md) * _σ_ <sub>distance</sub>`.  > **Note:** This control type is ignored when [`M_FILTER_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SMOOTH_MLS`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies how well edges are preserved for a bilateral smoothing operation. |

#### `M_USE_SOURCE_NORMALS`

Sets whether to use the source point cloud's normals information, stored in the source container's [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component.  Note that point normal values are necessary for the filter operation. Setting this control type to [`M_FALSE`](../../Reference/3dim/M3dimControl.md) means that a source normals component is not used, but that normals are freshly calculated.  When [`M_TRUE`](../../Reference/3dim/M3dimControl.md) is specified, point normals are not recalculated if they exist in the source container.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` | Specifies not to use source normals information. |
| `M_TRUE` *(default)* | Specifies to use source normals information. |

#### `M_WEIGHT_MODE`

Sets how the Gaussian weights are determined in the smoothing operation set with [`M_FILTER_MODE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the distance Gaussian weight directly, without applying a multiplying factor. |
| `M_RELATIVE` *(default)* | Specifies to compute and apply a multiplying factor to the distance Gaussian weight. The relative weight (multiplying factor) is computed as a function of point distances to their neighbors and the number of points in the neighborhood. |

---

### `Lattice context ID`

Specifies a lattice 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_LATTICE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) operations.

#### `M_FRACTION_OF_POINTS`

Sets the ratio of the total number of required (non-empty) cells to the total number of valid points in the point cloud. Aurora Imaging Library will try to divide the point cloud into cells such that the required number of cells, +/- the specified tolerance ([`M_FRACTION_OF_POINTS_TOLERANCE`](../../Reference/3dim/M3dimControl.md)), contain at least 1 valid point each. Depending on the spacing between points, the lattice might have extra cells that are empty or that contain only invalid points.  If your point cloud contains 100 valid points and you specify a fraction of 0.25, [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) will attempt to divide the valid points into approximately 25 cells. You can specify the tolerance for this amount using [`M_FRACTION_OF_POINTS_TOLERANCE`](../../Reference/3dim/M3dimControl.md). Given the example above, if you specify a tolerance of 0.1, [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) will attempt to calculate a lattice such that the number of cells with valid points is between 15 and 35.  [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md) determines the number of valid points that will be left in the point cloud after subsampling and the number of valid depth map pixels after projecting. When using the calculated lattice to subsample the point cloud and the resulting point cloud is organized ([`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md)), [`M3dimSample`](../../Reference/3dim/M3dimSample.md) selects 1 point per cell, where empty cells and cells containing only invalid points result in points with 0 confidence. When the resulting point cloud is unorganized ([`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_UNORGANIZED`](../../Reference/3dim/M3dimControl.md)), [`M3dimSample`](../../Reference/3dim/M3dimSample.md) ignores empty cells and cells containing only invalid points. When using the calculated lattice to project the point cloud, [`M3dimProject`](../../Reference/3dim/M3dimProject.md) projects all the points in a cell to the same depth map pixel. Empty cells and cells containing only invalid points will result in invalid pixels.  This control type is only available when [`M_LATTICE_MODE`](../../Reference/3dim/M3dimControl.md) is set to[`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 1.0` *(default)* | Specifies the ratio of required (non-empty) cells to the total number of valid points. |

#### `M_FRACTION_OF_POINTS_TOLERANCE`

Sets the tolerance to use for [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md) when calculating the lattice. [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) will attempt to calculate a lattice where the minimum number of cells containing valid points is the total number of valid points in the point cloud * ([`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md) - [`M_FRACTION_OF_POINTS_TOLERANCE`](../../Reference/3dim/M3dimControl.md)) and the maximum is the total number of valid points in the point cloud * ([`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md) + [`M_FRACTION_OF_POINTS_TOLERANCE`](../../Reference/3dim/M3dimControl.md)).  Note that the lattice is bound by the structure of the points. Some fractions are not always possible. Note also that the lower the tolerance, the longer[`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) will take to complete.  This control type is only available when [`M_LATTICE_MODE`](../../Reference/3dim/M3dimControl.md) is set to[`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 1.0` *(default)* | Specifies the tolerance for the number of required (non-empty) cells. |

#### `M_LATTICE_MODE`

Sets the mode with which to calculate the lattice.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FRACTION_OF_POINTS` *(default)* | Specifies to calculate a lattice that divides points into cells based on the required ratio of non-empty cells to the number of valid points in the point cloud. Specify the required ratio using [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md) and [`M_FRACTION_OF_POINTS_TOLERANCE`](../../Reference/3dim/M3dimControl.md). |
| `M_SPARSITY_CHANGE` | Specifies to calculate a lattice that divides points into cells based on a plateau of sparsity changes as the grid size increases. If there are many grid sizes in which the ratio of non-empty cells to empty cells becomes stable, use[`M_GRID_SIZE_INDEX`](../../Reference/3dim/M3dimControl.md) to specify which grid size to use. |

#### `M_RELATIVE_CELL_SIZE_X`

Sets the proportion of the cell size along the X dimension relative to the cell sizes along the Y and Z dimensions. For example, if you set [`M_RELATIVE_CELL_SIZE_X`](../../Reference/3dim/M3dimControl.md) to 1.0, [`M_RELATIVE_CELL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md), and [`M_RELATIVE_CELL_SIZE_Z`](../../Reference/3dim/M3dimControl.md) to 2.0, the cells will be twice as long in the Z direction compared to the X direction.  > **Note:** Note that when subsampling using the calculated lattice, calling [`M3dimSample`](../../Reference/3dim/M3dimSample.md) generates an error if more than 1 of [`M_RELATIVE_CELL_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_RELATIVE_CELL_SIZE_Y`](../../Reference/3dim/M3dimControl.md), or [`M_RELATIVE_CELL_SIZE_Z`](../../Reference/3dim/M3dimControl.md) is set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that when projecting using the calculated lattice, [`M_RELATIVE_CELL_SIZE_Z`](../../Reference/3dim/M3dimControl.md) must be set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md) and [`M_RELATIVE_CELL_SIZE_X`](../../Reference/3dim/M3dimControl.md) and [`M_RELATIVE_CELL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) must be set to non-infinite values.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies an infinite cell size along X. |
| `Value > 0.0` *(default)* | Specifies the relative cell size along X. |

#### `M_RELATIVE_CELL_SIZE_Y`

Sets the proportion of the cell size along the Y dimension relative to the cell sizes along the X and Z dimensions. For example, if you set [`M_RELATIVE_CELL_SIZE_X`](../../Reference/3dim/M3dimControl.md) to 1.0, [`M_RELATIVE_CELL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md), and [`M_RELATIVE_CELL_SIZE_Z`](../../Reference/3dim/M3dimControl.md) to 2.0, the cells will be twice as long in the Z direction compared to the X direction.  > **Note:** Note that when subsampling using the calculated lattice, calling [`M3dimSample`](../../Reference/3dim/M3dimSample.md) generates an error if more than 1 of [`M_RELATIVE_CELL_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_RELATIVE_CELL_SIZE_Y`](../../Reference/3dim/M3dimControl.md), or [`M_RELATIVE_CELL_SIZE_Z`](../../Reference/3dim/M3dimControl.md) is set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that when projecting using the calculated lattice, [`M_RELATIVE_CELL_SIZE_Z`](../../Reference/3dim/M3dimControl.md) must be set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md) and [`M_RELATIVE_CELL_SIZE_X`](../../Reference/3dim/M3dimControl.md) and [`M_RELATIVE_CELL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) must be set to non-infinite values.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies an infinite cell size along Y. |
| `Value > 0.0` *(default)* | Specifies the relative cell size along Y. |

#### `M_RELATIVE_CELL_SIZE_Z`

Sets the proportion of the cell size along the Z dimension relative to the cell sizes along the X and Y dimensions. For example, if you set [`M_RELATIVE_CELL_SIZE_X`](../../Reference/3dim/M3dimControl.md) to 1.0, [`M_RELATIVE_CELL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md), and [`M_RELATIVE_CELL_SIZE_Z`](../../Reference/3dim/M3dimControl.md) to 2.0, the cells will be twice as long in the Z direction compared to the X direction.  > **Note:** Note that when subsampling using the calculated lattice, calling [`M3dimSample`](../../Reference/3dim/M3dimSample.md) generates an error if more than 1 of [`M_RELATIVE_CELL_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_RELATIVE_CELL_SIZE_Y`](../../Reference/3dim/M3dimControl.md), or [`M_RELATIVE_CELL_SIZE_Z`](../../Reference/3dim/M3dimControl.md) is set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that when projecting using the calculated lattice, [`M_RELATIVE_CELL_SIZE_Z`](../../Reference/3dim/M3dimControl.md) must be set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md) and [`M_RELATIVE_CELL_SIZE_X`](../../Reference/3dim/M3dimControl.md) and [`M_RELATIVE_CELL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) must be set to non-infinite values.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies an infinite cell size along Z. |
| `Value > 0.0` *(default)* | Specifies the relative cell size along Z. |

#### `M_SPARSITY_DIMENSION_TOLERANCE`

Sets the tolerance to use for identifying sparsity changes in grid sizes when calculating the lattice. [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) will attempt to calculate a lattice where the minimum difference in dimensionality between consecutive grid sizes is at least [`M_SPARSITY_DIMENSION_TOLERANCE`](../../Reference/3dim/M3dimControl.md). This ensures that sparsity changes are significant enough to distinguish meaningful changes.  Note that the lattice is bound by the density and distribution of the points. If the tolerance is too low, the calculation might include insignificant sparsity changes, potentially increasing processing time and noise. Conversely, setting a higher tolerance will result in fewer but more distinct sparsity changes.  This control type is only available when [`M_LATTICE_MODE`](../../Reference/3dim/M3dimControl.md) is set to[`M_SPARSITY_CHANGE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum allowable difference in dimensionality between consecutive grid sizes used in the lattice calculation. |

---

### `Lattice result buffer ID`

Specifies a lattice 3D image processing result buffer, allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_LATTICE_RESULT`](../../Reference/3dim/M3dimAllocResult.md), and used to store [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) results.

#### `M_GRID_SIZE_INDEX`

Sets the grid size for which to return all lattice results. You can retrieve the number of grid sizes using [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_NUMBER_OF_GRID_SIZES`](../../Reference/3dim/M3dimGetResult.md).  > **Note:** Note that as the grid size increases the number of points within each cell grows according to the power of the number of dimensions that are merged.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIRST` *(default)* | Specifies to use the first grid size. |
| `M_LAST` | Specifies to use the last grid size. |
| `Value >= 0` | Specifies the index of the grid size to use. This value must be less than the number of grid sizes ([`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_NUMBER_OF_GRID_SIZES`](../../Reference/3dim/M3dimGetResult.md)). If the index is invalid, an error will occur when calling [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) or [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md). Note, the smallest grid size is at index 0. |

---

### `Mesh context ID`

Specifies a mesh 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_MESH_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md) operations.  > **Note:** When setting mesh control types, first use [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) to set the surface reconstruction mode, and then specify appropriate settings as per the specified mode. Not all control types are available for each mode.

#### `M_MAX_DISTANCE`

Sets the maximum distance at which to connect 2 neighboring points.  > **Note:** This control type is only available when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_ORGANIZED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no distance constraint. |
| `Value` | Specifies the maximum distance. Any 2 points separated by a distance greater than the specified value will not be connected. |

#### `M_MAXIMUM_NUMBER_NEIGHBORS`

Sets the maximum number of neighbors to consider when computing a local plane at each point.  > **Note:** This control type is available only when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the maximum number of neighbor points. |

#### `M_MESH_MODE`

Sets the surface reconstruction mode to use for mesh generation. When you set the mode to [`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimControl.md), you can trim the mesh to discard outlier points with [`M_MESH_TRIMMING_LEVEL`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MESH_LOCAL_PLANE` | Specifies to generate the mesh from local planes that are fit to neighboring points. For each point, neighborhood points are projected to the local plane and the most suitable points in the source point cloud are chosen to form a mesh triangle with the point in question. With this mode, the mesh can have holes. Normal vectors at each point are required. To calculate normal vectors, use [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md). |
| `M_MESH_ORGANIZED` | Specifies to generate the mesh from an organized point cloud or depth map. This mode connects neighboring points and can result in a mesh with holes due to invalid points in the point cloud or depth map. |
| `M_MESH_SMOOTHED` *(default)* | Specifies to generate a mesh with a smoothed surface. With this mode, Aurora Imaging Library creates a new point cloud that has been smoothed to reduce noise; missing data is approximated when possible, resulting in significantly fewer holes. The operation uses a Poisson surface reconstruction algorithm. Normal vectors at each point are required. To calculate normal vectors, use [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md). |

#### `M_MESH_STEP_X`

Sets the step interval between points, along the X-axis of the range component, in pixels. The [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md) function connects points that are separated by the specified interval, when creating triangular planes for surface reconstruction.  [`M_MESH_STEP_X`](../../Reference/3dim/M3dimControl.md) specifies an interval in X for each existing band.  > **Note:** Note that, if an invalid point is located at the specified interval (that is, the point's confidence score is 0), no vertex of a triangular plane is formed at this point, and a gap is possible in the resulting mesh.  > **Note:** This control type is only available when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_ORGANIZED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the step interval in the X direction, in pixel units. |

#### `M_MESH_STEP_Y`

Sets the step interval between points, along the Y-axis of the range component, in pixels. The [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md) function connects points that are separated by the specified interval, when creating triangular planes for surface reconstruction.  [`M_MESH_STEP_Y`](../../Reference/3dim/M3dimControl.md) specifies an interval in Y for each existing band.  > **Note:** Note that, if an invalid point is located at the specified interval (that is, the point's confidence score is 0), no vertex of a triangular plane is formed at this point, and a gap is possible in the resulting mesh.  > **Note:** This control type is only available when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_ORGANIZED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the step interval in the Y direction, in pixel units. |

#### `M_MESH_TRIMMING_LEVEL`

Sets the trimming level for a mesh, which is applied as a threshold to discard outlier points. Setting a higher value will trim more from the mesh. Note that Aurora Imaging Library applies the threshold to an internal measurement that is related to the density of the vertices in the mesh. A typical value for this control type is 7.0.  To disable mesh trimming, set [`M_MESH_TRIMMING_LEVEL`](../../Reference/3dim/M3dimControl.md) to 0.0.  > **Note:** This control type is available only when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically calculate the trimming level. |
| `Value >= 0.0` | Specifies the trimming level. |

#### `M_NEIGHBOR_SEARCH_MODE`

Sets the search mode for finding the neighbors of a point.  > **Note:** This control type is available only when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that [`M_TREE`](../../Reference/3dim/M3dimControl.md) is typically the most precise because it chooses points that are closest in 3D space, using the Euclidean distance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ORGANIZED` | Specifies to use the point cloud's organizational structure to determine the neighbors of a point. This option is supported only for an organized point cloud. |
| `M_TREE` *(default)* | Specifies to use a KD tree search mode to determine a point's neighbors. |

#### `M_NEIGHBORHOOD_DISTANCE`

Sets the distance that defines the neighborhood of a point; only points within this distance are considered when calculating the local plane of the point.  > **Note:** This control type is available only when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no distance constraint. |
| `Value > 0.0` | Specifies the neighborhood distance limit, in world units. |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Sets the neighborhood size when finding the neighbors of a point in an organized point cloud. The size determines the number of elements along a row or column of the 2D buffer of the range and confidence components. For example, if you set [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](../../Reference/3dim/M3dimControl.md) to 7, the neighborhood is a 7-element by 7-element region in the source container's 2D grid organization (specifically, in the organized point cloud's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components).  > **Note:** This control type is supported only when using an organized point cloud and only if [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md).  > **Note:** This control type is available only when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the organized neighborhood size, in number of pixels; the specified value must be an odd number. |

#### `M_NUMBER_POINTS_PER_CELL`

Sets the number of points within a single octree cell at which the cell will not be split further.  > **Note:** This control type is only available when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimControl.md).  For example, if [`M_NUMBER_POINTS_PER_CELL`](../../Reference/3dim/M3dimControl.md) is set to 3, any octree cell with 3 or fewer points is not divided further.  A larger [`M_NUMBER_POINTS_PER_CELL`](../../Reference/3dim/M3dimControl.md) can minimize effects due to noise in the source cloud data.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the number of points, within a single octree cell, at which the cell will not be split further. |

#### `M_SURFACE_ANGLE_MAX`

Sets the maximum acceptable angle between the normal of a point and that of a neighboring point for the neighbor to be considered for triangulation. If the angle is within this limit, the neighboring point could be part of a triangle for the mesh.  > **Note:** This control type is available only when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 180.0` *(default)* | Specifies the maximum angle, in degrees. |

#### `M_TREE_MAX_DEPTH`

Sets the maximum allowed depth of the octree. The octree partitions the 3D space for the mesh calculation.  > **Note:** This control type is only available when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimControl.md).  The maximum tree depth determines the resolution of the 3D mesh. For example, an [`M_TREE_MAX_DEPTH`](../../Reference/3dim/M3dimControl.md) set to 8 gives a mesh with a resolution of `2<sup>8</sup> x 2<sup>8</sup> x 2<sup>8</sup>`.  > **Note:** Note that a higher octree value gives a higher resolution, but a slower calculation time.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `5 <= Value <= 12` *(default)* | Specifies the maximum octree depth. |

#### `M_TRIANGLE_ANGLE_MAX`

Sets the maximum interior angle for each mesh triangle. The specified angle must be greater than or equal to the minimum interior angle ([`M_TRIANGLE_ANGLE_MIN`](../../Reference/3dim/M3dimControl.md)). Note that this maximum is guaranteed; larger angles will not occur.  > **Note:** This control type is available only when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 180.0` *(default)* | Specifies the maximum angle, in degrees. |

#### `M_TRIANGLE_ANGLE_MIN`

Sets the minimum interior angle for each mesh triangle. The specified angle must be less than or equal to the maximum interior angle ([`M_TRIANGLE_ANGLE_MAX`](../../Reference/3dim/M3dimControl.md)). Note that this minimum is not guaranteed; smaller angles might occur, due to the greedy nature of the meshing algorithm.  > **Note:** This control type is available only when [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 180.0` *(default)* | Specifies the minimum angle, in degrees. |

---

### `Normals context ID`

Specifies a normals 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_NORMALS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md) operations.

#### `M_DIRECTION_MODE`

Sets the orientation of the normal vectors, towards a reference point or vector set with [`M_DIRECTION_REFERENCE_...`](../../Reference/3dim/M3dimControl.md), which are either the X-, Y-, and Z-coordinates of the reference point, or the X-, Y-, and Z-components of the reference vector.  A point on a 3-dimensional surface has 2 normal vectors, each perpendicular to the surface, but pointing in opposite directions. [`M_DIRECTION_MODE`](../../Reference/3dim/M3dimControl.md) sets a preferred orientation for the normals, and therefore sets which normal vector to choose for each point. For example, you can specify a reference point inside your 3D object and use [`M_AWAY_FROM_POSITION`](../../Reference/3dim/M3dimControl.md) to ensure that all normal vectors point outwards from the object's surface.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AWAY_FROM_POSITION` | Specifies to choose, for each point, the normal vector that points away from the reference point specified with coordinates [`M_DIRECTION_REFERENCE_X`](../../Reference/3dim/M3dimControl.md), [`M_DIRECTION_REFERENCE_Y`](../../Reference/3dim/M3dimControl.md), and [`M_DIRECTION_REFERENCE_Z`](../../Reference/3dim/M3dimControl.md). |
| `M_TOWARDS_DIRECTION` *(default)* | Specifies to choose, for each point, the normal vector that most closely aligns with the reference vector specified with vector components [`M_DIRECTION_REFERENCE_X`](../../Reference/3dim/M3dimControl.md), [`M_DIRECTION_REFERENCE_Y`](../../Reference/3dim/M3dimControl.md), and [`M_DIRECTION_REFERENCE_Z`](../../Reference/3dim/M3dimControl.md). |
| `M_TOWARDS_POSITION` | Specifies to choose, for each point, the normal vector that points towards the reference point specified with coordinates [`M_DIRECTION_REFERENCE_X`](../../Reference/3dim/M3dimControl.md), [`M_DIRECTION_REFERENCE_Y`](../../Reference/3dim/M3dimControl.md), and [`M_DIRECTION_REFERENCE_Z`](../../Reference/3dim/M3dimControl.md). |

#### `M_DIRECTION_REFERENCE_X`

Sets the X-coordinate or vector component along X for the reference point or vector (respectively) that determines normal vector orientation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate or vector component along X. |

#### `M_DIRECTION_REFERENCE_Y`

Sets the Y-coordinate or vector component along Y for the reference point or vector (respectively) that determines normal vector orientation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate or vector component along Y. |

#### `M_DIRECTION_REFERENCE_Z`

Sets the Z-coordinate or vector component along Z for the reference point or vector (respectively) that determines normal vector orientation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Z-coordinate or vector component along Z. |

#### `M_ENFORCE_CONTINUITY`

Sets whether to maintain the general direction of the normals, which prevents large, discontinuous jumps between normal vectors. For example, if 2 neighboring points have normal vectors (0,0,1) and (0,0,-0.9), 1 of the vectors will be flipped so that both point roughly in the same direction. This means that some normals will not respect the specified [`M_DIRECTION_MODE`](../../Reference/3dim/M3dimControl.md), but, on average, most normals will respect the specified orientation of the normal vectors.  > **Note:** This option is supported only when [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_TREE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not enforce continuity of normal vectors. |
| `M_ENABLE` | Specifies to enforce continuity of normal vectors. |

#### `M_MAXIMUM_NUMBER_NEIGHBORS`

Sets the maximum number of neighbors to consider when computing a point's normal vector. You must specify [`M_MAXIMUM_NUMBER_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) when using an [`M_TREE`](../../Reference/3dim/M3dimControl.md) neighbor search mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number of neighbor points. |

#### `M_NEIGHBOR_SEARCH_MODE`

Sets the search mode for finding the neighbors of a point, when calculating a point's surface normal vector.  > **Note:** Note that [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md) is usually faster, but is less robust to noise. [`M_TREE`](../../Reference/3dim/M3dimControl.md) is typically the most precise because it chooses points that are closest in 3D space, using the Euclidean distance. You can optionally specify [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimControl.md) to reduce the effect of outlier neighborhood points.  Invalid points in a neighborhood are not considered when calculating normal vectors.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MESH` | Specifies to use the point cloud's mesh component to determine the neighbors of a point. A mesh's triangular faces are composed of connected neighboring points.  > **Note:** This option is available only when the source container has an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component. |
| `M_ORGANIZED` | Specifies to use the point cloud's organizational structure to determine the neighbors of a point. This option is supported only for an organized point cloud.  When [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md) is specified, you must set either [`M_MAXIMUM_NUMBER_NEIGHBORS`](../../Reference/3dim/M3dimControl.md) or [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](../../Reference/3dim/M3dimControl.md) to determine the extent of the neighborhood. |
| `M_TREE` *(default)* | Specifies to use a KD tree search mode to determine a point's neighbors.  When [`M_TREE`](../../Reference/3dim/M3dimControl.md) is specified, you must also set [`M_MAXIMUM_NUMBER_NEIGHBORS`](../../Reference/3dim/M3dimControl.md). |

#### `M_NEIGHBORHOOD_DISTANCE`

Sets the distance that defines the neighborhood of a point; only points within this distance are considered for normal vector calculations.  > **Note:** This option is not supported when [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_MESH`](../../Reference/3dim/M3dimControl.md)

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no distance constraint. |
| `Value >= 0.0` | Specifies the neighborhood distance limit, in world units. |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Sets the neighborhood size when finding the neighbors of a point in an organized point cloud. The size determines the number of elements along a row or column of the 2D buffer of the range and confidence components. For example, if you set [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](../../Reference/3dim/M3dimControl.md) to 7, the neighborhood is a 7-element by 7-element region in the source container's 2D grid organization (specifically, in the organized point cloud's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components).  Normal calculations for points close to the edge of the 2D buffer use a clipped neighborhood.  This control type is supported only when using an organized point cloud and only if [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the organized neighborhood size, in number of pixels; the specified value must be an odd number. |

---

### `Outliers context ID`

Specifies an outliers 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_OUTLIERS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md) operations.

#### `M_DISTANCE_THRESHOLD_MODE`

Sets the threshold mode for a local distance operation.  The specified mode establishes how to define the cutoff point (threshold) for determining which points are outliers, from a local average distance distribution across all points in the point cloud. A local average distance is the average distance of a point to other points in its neighborhood.  To affect the threshold itself, set a scale factor with [`M_STD_DEVIATION_FACTOR`](../../Reference/3dim/M3dimControl.md).  > **Note:** This control type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_LOCAL_DISTANCE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ROBUST_STD_DEVIATION` *(default)* | Specifies to determine outliers using a threshold based on the median and the mean absolute deviation (MAD) of the local average distance distribution of points in the point cloud. This mode is more robust to extreme outliers. Since such outliers can skew the mean, using the median is often a more accurate middle value. |
| `M_STD_DEVIATION` | Specifies to determine outliers using a threshold based on the mean and the standard deviation of the local average distance distribution. |

#### `M_MINIMUM_NUMBER_NEIGHBORS`

Sets the minimum number of neighbors that a point must have to be considered an inlier, within the distance that defines the neighborhood. The neighborhood distance is either user-defined or automatically set, depending on the [`M_NEIGHBORHOOD_DISTANCE_MODE`](../../Reference/3dim/M3dimControl.md) setting.  > **Note:** This control type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_NUMBER_WITHIN_DISTANCE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the minimum number of neighbors. |

#### `M_NEIGHBOR_SEARCH_MODE`

Sets the search mode for finding the neighbors of a point.  > **Note:** Note that [`M_TREE`](../../Reference/3dim/M3dimControl.md) is typically the most precise because it chooses points that are closest in 3D space, using the Euclidean distance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ORGANIZED` | Specifies to use the point cloud's organizational structure to determine the neighbors of a point.  > **Note:** This option is supported only for an organized point cloud. |
| `M_TREE` *(default)* | Specifies to use a KD tree search mode to determine a point's neighbors. |

#### `M_NEIGHBORHOOD_DISTANCE`

Sets the distance that defines the neighborhood of a point; only points within this distance are considered for outlier calculations.  > **Note:** This control type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_NUMBER_WITHIN_DISTANCE`](../../Reference/3dim/M3dimControl.md) and [`M_NEIGHBORHOOD_DISTANCE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the distance. |

#### `M_NEIGHBORHOOD_DISTANCE_METRIC`

Sets how to establish the neighborhood of a point, either as a spherical volume around the point ([`M_EUCLIDEAN`](../../Reference/3dim/M3dimControl.md)), or as a distance in the Z-direction ([`M_DIRECTION_Z`](../../Reference/3dim/M3dimControl.md)).  To set the distance limit of the neighborhood, use [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DIRECTION_Z` | Specifies to use a distance in the Z-direction.  > **Note:** This option is supported only when [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md) and [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_NUMBER_WITHIN_DISTANCE`](../../Reference/3dim/M3dimControl.md). |
| `M_EUCLIDEAN` *(default)* | Specifies to use a Euclidean distance. |

#### `M_NEIGHBORHOOD_DISTANCE_MODE`

Sets the mode for computing a point's neighborhood distance.  > **Note:** This control type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_NUMBER_WITHIN_DISTANCE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically compute the neighborhood distance. |
| `M_USER_DEFINED` | Specifies to use the distance set with [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimControl.md). |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Sets the neighborhood size when finding the neighbors of a point in an organized point cloud. The size determines the number of elements along a row or column of the 2D buffer of the range and confidence components. For example, if you set [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](../../Reference/3dim/M3dimControl.md) to 7, the neighborhood is a 7-element by 7-element region in the source container's 2D grid organization (specifically, in the organized point cloud's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components).  > **Note:** This control type is supported only when using an organized point cloud and only if [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NUMBER_NEIGHBORS`

Sets the number of neighboring points used to calculate a point's local average distance distribution.  > **Note:** This control type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_LOCAL_DISTANCE`](../../Reference/3dim/M3dimControl.md) or [`M_LOCAL_DENSITY_PROBABILITY`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of neighbors. |

#### `M_OUTLIER_MODE`

Sets the mode for determining whether a point is an outlier.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LOCAL_DENSITY_PROBABILITY` | Specifies to use the density of neighboring points and a probability calculation to determine whether a point is an outlier. With this mode, an outlier probability (_P<sub>outlier_i</sub> _) is assigned to each point, based on the local density of its neighborhood. This probability is then compared to a local outlier threshold (below). If a point's local outlier probability is larger than the calculated local outlier threshold, it is considered an outlier.  `_P<sub>outlier_i</sub> _ > _scale_factor_ * [_local_avg_distance_i_ / _mean(local_avg_distance)_]`.  The local outlier threshold calculation incorporates a point's local average distance to its neighbors (_local_avg_distance_i_) divided by the mean local average distance calculated from all points (_mean(local_avg_distance)_), all multiplied by a scale factor.  Set the scale factor with [`M_PROBABILITY_THRESHOLD_FACTOR`](../../Reference/3dim/M3dimControl.md). A smaller scale factor rejects more outliers.  This mode is useful when point cloud density is variable. For example, if your point cloud has a sparsely populated region (compared to the whole point cloud), other outlier modes might reject points in that region. Using [`M_LOCAL_DENSITY_PROBABILITY`](../../Reference/3dim/M3dimControl.md) can reduce this risk. |
| `M_LOCAL_DISTANCE` *(default)* | Specifies to use a statistical calculation that considers the average distance of a point to other points in its neighborhood to determine whether the point is an outlier. With this mode, a point is deemed an outlier if its average distance to neighboring points is greater than a calculated threshold. The threshold is a function of the mean or median, the standard deviation or the MAD, and the scale factor. Set the number of neighboring points to consider with [`M_NUMBER_NEIGHBORS`](../../Reference/3dim/M3dimControl.md). Set the threshold mode and a scale factor for the calculation with [`M_DISTANCE_THRESHOLD_MODE`](../../Reference/3dim/M3dimControl.md) and [`M_STD_DEVIATION_FACTOR`](../../Reference/3dim/M3dimControl.md), respectively. |
| `M_NUMBER_WITHIN_DISTANCE` | Specifies to use the number of points within a defined neighborhood to determine whether a point is an outlier. With this mode, if the number of neighboring points is less than [`M_MINIMUM_NUMBER_NEIGHBORS`](../../Reference/3dim/M3dimControl.md), it is considered an outlier. Use [`M_NEIGHBORHOOD_DISTANCE_METRIC`](../../Reference/3dim/M3dimControl.md) to set either a spherical neighborhood ([`M_EUCLIDEAN`](../../Reference/3dim/M3dimControl.md)) or one defined along Z ([`M_DIRECTION_Z`](../../Reference/3dim/M3dimControl.md)). Define the limits of the neighborhood with [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimControl.md).  This is typically the fastest outlier mode. |

#### `M_PROBABILITY_THRESHOLD_FACTOR`

Sets the scale factor to use in the probability calculation that determines whether a point is an outlier, when working in the [`M_LOCAL_DENSITY_PROBABILITY`](../../Reference/3dim/M3dimControl.md) outlier mode. A smaller scale factor rejects more outliers.  > **Note:** This control type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_LOCAL_DENSITY_PROBABILITY`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the scale factor. |

#### `M_STD_DEVIATION_FACTOR`

Sets the scale factor to use when calculating the local outlier threshold, when working in the [`M_LOCAL_DISTANCE`](../../Reference/3dim/M3dimControl.md) outlier mode.  When [`M_DISTANCE_THRESHOLD_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_STD_DEVIATION`](../../Reference/3dim/M3dimControl.md), [`M_STD_DEVIATION_FACTOR`](../../Reference/3dim/M3dimControl.md) is the factor multiplying the standard deviation of the local average distance distribution.  To illustrate, if, in the [`M_STD_DEVIATION`](../../Reference/3dim/M3dimControl.md) mode, the mean local average distance is 1.0, the standard deviation is 0.3, and [`M_STD_DEVIATION_FACTOR`](../../Reference/3dim/M3dimControl.md) is set to 3.0, then Aurora Imaging Library calculates the local outlier threshold as follows:  `_mean_ + _std_deviation_factor_ * _standard_deviation_` = 1.0 + 3.0 * 0.3 = 1.9.  For this example, points with local average distances larger than 1.9 are considered outliers.  When [`M_DISTANCE_THRESHOLD_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ROBUST_STD_DEVIATION`](../../Reference/3dim/M3dimControl.md), [`M_STD_DEVIATION_FACTOR`](../../Reference/3dim/M3dimControl.md) is the factor multiplying the mean absolute deviation (MAD) of the local average distance distribution.  For either mode, a smaller factor defines a smaller threshold, thereby more aggressively rejecting outliers.  > **Note:** This control type is supported only when [`M_OUTLIER_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_LOCAL_DISTANCE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the scale factor. |

---

### `Profile context ID`

Specifies a profile 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_PROFILE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) operations.

#### `M_PROFILE_ACCUMULATE_TYPE`

Sets how points in a given sample are accumulated to create a single profile point when there are multiple points within the thickness of the profile line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX` | Specifies to use the largest value of all the points in the sample.  > **Note:** This option is not supported when [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_AVERAGE`](../../Reference/3dim/M3dimControl.md). |
| `M_MEAN` *(default)* | Specifies to use the mean value of all the points in the sample. |
| `M_MIN` | Specifies to use the smallest value of all the points in the sample.  > **Note:** This option is not supported when [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_AVERAGE`](../../Reference/3dim/M3dimControl.md). |

#### `M_PROFILE_INTERPOLATION_AVERAGE_FRACTION`

Sets the fraction of the sampling distance to use when taking a weighted average of the area surrounding the profile point. For example, a value of 0.5 indicates that 50 percent of the pixel area that surrounds the profile point (defined by 50% of the sampling distance along the line) is used to calculate a weighted average for the profile point.  > **Note:** This control type is only available when [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_AVERAGE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 1.0` *(default)* | Specifies the fraction. |

#### `M_PROFILE_INTERPOLATION_MODE`

Sets the interpolation mode used to calculate the value for each point in the sample that will be used to create the profile point.  Note that when averaging interpolation is used ([`M_AVERAGE`](../../Reference/3dim/M3dimControl.md)), the profile point is calculated without calculating the value for each point in the sample.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` | Specifies averaging interpolation. The value of the profile point is determined by taking a weighted average of the pixel area that surrounds the profile point. You can use [`M_PROFILE_INTERPOLATION_AVERAGE_FRACTION`](../../Reference/3dim/M3dimControl.md) to specify how much of the pixel area to consider. |
| `M_BILINEAR` *(default)* | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the point in the sample. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the point in the sample. |

#### `M_PROFILE_MIN_VALID_PERCENTAGE`

Sets the minimum percentage of points in a given sample that must be valid to set the corresponding profile point to a valid value. When there are multiple points within the thickness for the sample that will be used to create the profile point, the profile point will be invalid if the minimum valid percentage is not met. Note that if results are stored in a profile 3D image processing result buffer, invalid points are not included in the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the percentage of points in the sample. Note that a value of 0.0% means there are no constraints on the number of valid points, but if all points contributing to the calculation of a profile point are invalid, the profile point will be invalid in the destination. |

#### `M_PROFILE_SAMPLE_SIZE`

Sets the sampling distance between two consecutive points along the profile line. When the pixel size in X is not equal to the pixel size in Y in the source depth map, this ensures the extracted points are at a uniform distance.  Note that this will be the pixel size of the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the distance. Use [`M_PROFILE_SAMPLE_SIZE_MODE`](../../Reference/3dim/M3dimControl.md) to specify how to interpret the specified sampling distance. |

#### `M_PROFILE_SAMPLE_SIZE_MODE`

Sets how to interpret the specified sampling distance ([`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dim/M3dimControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the sampling distance by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` *(default)* | Specifies to multiply the sampling distance by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the sampling distance by the pixel size in X. For example, if you set [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dim/M3dimControl.md) to 2.0 and the pixel size in X is 0.5 mm, the sampling distance will be 1.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the sampling distance by the pixel size in Y. For example, if you set [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dim/M3dimControl.md) to 2.0 and the pixel size in Y is 0.6 mm, the sampling distance will be 1.2 mm. |
| `M_WORLD` | Specifies that the sampling distance be interpreted in world units. For example, if you set [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dim/M3dimControl.md) to 2.0, the sampling distance will be 2.0 world units. |

#### `M_PROFILE_THICKNESS`

Sets the thickness of the profile line (slicing plane). Pixels along the line, within the specified thickness, are sampled and corresponding points are considered for the profile.  Note that a thick profile line typically results in multiple points contributing to the same profile point. You can use [`M_PROFILE_ACCUMULATE_TYPE`](../../Reference/3dim/M3dimControl.md) to determine how multiple point values are combined to set the value of the corresponding profile point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the thickness. Use [`M_PROFILE_THICKNESS_MODE`](../../Reference/3dim/M3dimControl.md) to specify how to interpret the specified thickness. |

#### `M_PROFILE_THICKNESS_MODE`

Sets how to interpret the specified thickness value ([`M_PROFILE_THICKNESS`](../../Reference/3dim/M3dimControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the thickness value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` *(default)* | Specifies to multiply the thickness value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the thickness value by the pixel size in X. For example, if you set [`M_PROFILE_THICKNESS`](../../Reference/3dim/M3dimControl.md) to 2.0 and the pixel size in X is 0.5 mm, the profile line will be 1.0 mm thick. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the thickness value by the pixel size in Y. For example, if you set [`M_PROFILE_THICKNESS`](../../Reference/3dim/M3dimControl.md) to 2.0 and the pixel size in Y is 0.6 mm, the profile line will be 1.2 mm thick. |
| `M_WORLD` | Specifies that the thickness value be interpreted in world units. For example, if you set [`M_PROFILE_THICKNESS`](../../Reference/3dim/M3dimControl.md) to 2.0, the profile line will be 2.0 world units thick. |

---

### `Project context ID`

Specifies a project 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_PROJECT_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) operations.

#### `M_CALCULATE_SIZE_MODE`

Sets whether the source point cloud container is organized.[`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) calculations are faster with organized point clouds.  > **Note:** This control type is ignored when the source is a 3D geometry object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ORGANIZED` | Specifies an organized point cloud.  > **Note:** Note that using this value with an unorganized point cloud will cause an error. |
| `M_UNORGANIZED` *(default)* | Specifies an unorganized point cloud. |

#### `M_CALIBRATION_ALIGNMENT`

Sets where to place valid values in the depth map.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER` | Specifies to project the data such that it is centered horizontally or vertically in the buffer. Pixels in unused rows or columns are set to the invalid pixel value. The source data maintains its aspect ratio. |
| `M_TOP_LEFT` *(default)* | Specifies to project the data such that it aligns to the top left of the buffer, and the data extends at least the breadth of one dimension, leaving space at the bottom or right. Pixels in unused rows or columns are set to the invalid pixel value. The source data maintains its aspect ratio. |

#### `M_DEPTH_MAP_TYPE`

Sets the data type and depth required for the destination container's range component.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNSIGNED + 8` | Specifies 8-bit unsigned data. |
| `M_UNSIGNED + 16` *(default)* | Specifies 16-bit unsigned data. |
| `M_UNSIGNED + 32` | Specifies 32-bit unsigned data. |

#### `M_OVERLAP_MODE`

Sets the overlap mode, which determines a pixel's gray level when multiple points correspond to the same destination depth map pixel.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` | Specifies to use the average gray level of all the 3D points projected on a single depth map pixel for custom components.  Note that for range components, the average Z-coordinate is used. For reflectance and intensity components, the average value is used. For normals components, the average vector is renormalized and flipped towards the most common direction. Note that no other components are projected.  > **Note:** Note that this value is not available when the source is a 3D geometry object. |
| `M_MAX_INTENSITY` | Specifies to use the gray level of the 3D point with the highest intensity. The source container must have a reflectance or intensity component. If both reflectance and intensity components exist, only the reflectance component is considered.  > **Note:** Note that this value is not available when the source is a 3D geometry object. |
| `M_MAX_Z` *(default)* | Specifies to use the gray level of the 3D point with the largest Z-value, of all the 3D points projected on a single depth map pixel. Note that this value can be bright or dark, depending on whether the Z-scale is positive or negative ([`M_SIGN_Z`](../../Reference/3dim/M3dimControl.md) set to [`M_POSITIVE`](../../Reference/3dim/M3dimControl.md) or [`M_NEGATIVE`](../../Reference/3dim/M3dimControl.md)). |
| `M_MIN_Z` | Specifies to use the gray level of the 3D point with the smallest Z-value, of all the 3D points projected on a single depth map pixel. Note that this value can be dark or bright, depending on whether the Z-scale is positive or negative ([`M_SIGN_Z`](../../Reference/3dim/M3dimControl.md) set to [`M_POSITIVE`](../../Reference/3dim/M3dimControl.md) or [`M_NEGATIVE`](../../Reference/3dim/M3dimControl.md)). |
| `M_MOST_FREQUENT` | Specifies to use the most frequent gray level for custom components of type[`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md) or [`M_SIGNED`](../../Reference/buf/MbufAlloc2d.md), or the average gray level for custom components of type [`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md), of all the 3D points projected on a single depth map pixel.  Note that for range components, the average Z-coordinate is used. For reflectance and intensity components, the average value is used. For normals components, the average vector is renormalized and flipped towards the most common direction. Note that no other components are projected. |
| `M_OVERWRITE` | Specifies to use the last gray level of all the 3D points projected on a single depth map pixel. This mode continually overwrites the previous gray value for the pixel. [`M_OVERWRITE`](../../Reference/3dim/M3dimControl.md) can be marginally faster than the other overlap modes. |

#### `M_PIXEL_ASPECT_RATIO`

Sets the pixel aspect ratio of the depth map.  The pixel aspect ratio is used to calculate the depth map buffer size when the [`Options`](../../Reference/3dim/M3dimProjectEx.md) parameter of [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) is set to [`M_USE_BOX_WORLD_SIZE`](../../Reference/3dim/M3dimProjectEx.md) or [`M_USE_SRC_DATA`](../../Reference/3dim/M3dimProjectEx.md), and to apply the calibration when not using the [`M_ACCUMULATE`](../../Reference/3dim/M3dimProjectEx.md) option.  > **Note:** This control type must be set to [`M_NULL`](../../Reference/3dim/M3dimControl.md) if you have already specified a size in X with [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md) and a size in Y with [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md). Otherwise, an error will occur.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` | Specifies that other data determines the pixel aspect ratio, such as the values set with [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) and [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md). |
| `Value > 0.0` *(default)* | Specifies an aspect ratio for the depth map's pixels. The pixel size is computed such that the pixel's length in X divided by its length in Y equals the specified [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) value. |

#### `M_PIXEL_SIZE_X`

Sets the length in X of one pixel in the depth map.  The pixel size is used to calculate the depth map buffer size when the [`Options`](../../Reference/3dim/M3dimProjectEx.md) parameter of [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) is set to [`M_USE_BOX_WORLD_SIZE`](../../Reference/3dim/M3dimProjectEx.md) or [`M_USE_SRC_DATA`](../../Reference/3dim/M3dimProjectEx.md), and to apply the calibration when not using the [`M_ACCUMULATE`](../../Reference/3dim/M3dimProjectEx.md) option.  > **Note:** Note that you cannot set [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) at the same time as [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md), unless [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) is set to [`M_NULL`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that you cannot specify a specific value for [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) if you have done so for [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md) (and vice versa).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies that [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) will compute the pixel size in X. |
| `Value > 0.0` | Specifies the length in X of one pixel in the depth map, in world units. |

#### `M_PIXEL_SIZE_Y`

Sets the length in Y of one pixel in the depth map.  The pixel size is used to calculate the depth map buffer size when the [`Options`](../../Reference/3dim/M3dimProjectEx.md) parameter of [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) is set to [`M_USE_BOX_WORLD_SIZE`](../../Reference/3dim/M3dimProjectEx.md) or [`M_USE_SRC_DATA`](../../Reference/3dim/M3dimProjectEx.md), and to apply the calibration when not using the [`M_ACCUMULATE`](../../Reference/3dim/M3dimProjectEx.md) option.  > **Note:** Note that you cannot set [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) at the same time as [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md), unless [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) is set to [`M_NULL`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that you cannot specify a specific value for [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) if you have done so for [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md) (and vice versa).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies that [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) will compute the pixel size in Y. |
| `Value > 0.0` | Specifies the length in Y of one pixel in the depth map, in world units. |

#### `M_PROJECTED_COMPONENTS`

Sets which components to project into the destination depth map.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COMPONENT_ALL` | Specifies to project all components that have the same dimensions as the source container's range component.  > **Note:** Note that using this value with a destination image buffer will cause an error. |
| `M_COMPONENT_RANGE` *(default)* | Specifies to project the range component. |
| `M_COMPONENT_RANGE_AND_REFLECTANCE` | Specifies to project the range component and the reflectance or intensity component. If both reflectance and intensity components exist, the intensity component is not projected.  > **Note:** Note that using this value with a destination image buffer will cause an error. |

#### `M_PROJECTION_MODE`

Sets the projection mode, which determines how points are projected.  > **Note:** This control type is ignored when the source is a 3D geometry object.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MESH_BASED` | Specifies to consider the source container's mesh component when extracting 3D points for projection into the depth map. For a meshed point cloud, each triangular face has 3 vertices, and a vertex's Z-value determines the gray level of its corresponding pixel in the depth map. The remaining pixels (in the depth map) are assigned interpolated gray levels, since they correspond to the triangular faces between the vertices (in the point cloud's mesh). The container must have an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component before using the [`M_MESH_BASED`](../../Reference/3dim/M3dimControl.md) mode. |
| `M_POINT_BASED` *(default)* | Specifies to project the 3D points directly into the depth map. |

#### `M_SATURATION`

Sets whether to saturate pixels. Note that saturation can occur, depending on the Z-scale and offset of the destination depth map buffer. If, for a given pixel of the destination buffer, the corresponding gray level of the 3D geometry or point of the point cloud falls outside the buffer's grayscale range, the pixel will be saturated to either 0 or the buffer's maximum value minus 1 (the maximum value is reserved to represent missing data).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to saturate pixels. |
| `M_ENABLE` | Specifies to saturate pixels. |

#### `M_SIGN_Z`

Sets the sign of the Z-scale in the destination depth map. The sign applies to [`M_3D_SCALE_Z`](../../Reference/buf/MbufControlContainer.md), which sets the length, in world units, of one gray level in a depth map container.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NEGATIVE` | Specifies a negative value for the Z-scale. A negative value indicates that larger Z-values are represented by darker depth map pixels, and smaller Z-values are represented by brighter depth map pixels. |
| `M_POSITIVE` *(default)* | Specifies a positive value for the Z-scale. A positive value indicates that larger Z-values are represented by brighter depth map pixels, and smaller Z-values are represented by darker depth map pixels. |

#### `M_SIZE_X`

Sets the size of the depth map along the X dimension.  > **Note:** Note that you cannot set [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md) at the same time as [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md), unless [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) is set to [`M_NULL`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that you cannot specify a specific value for [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md) if you have done so for [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) (and vice versa).  > **Note:** This control type is only used when the [`Options`](../../Reference/3dim/M3dimProjectEx.md) parameter of [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) is set to [`M_USE_BOX_WORLD_SIZE`](../../Reference/3dim/M3dimProjectEx.md) or [`M_USE_SRC_DATA`](../../Reference/3dim/M3dimProjectEx.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies that [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) will compute the size of the depth map in X. |
| `Value > 0.0` | Specifies the size of the depth map in X, in pixel units. |

#### `M_SIZE_Y`

Sets the size of the depth map along the Y dimension.  > **Note:** Note that you cannot set [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md) at the same time as [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md) or [`M_SIZE_X`](../../Reference/3dim/M3dimControl.md), unless [`M_PIXEL_ASPECT_RATIO`](../../Reference/3dim/M3dimControl.md) is set to [`M_NULL`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that you cannot specify a specific value for [`M_SIZE_Y`](../../Reference/3dim/M3dimControl.md) if you have done so for [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md) (and vice versa).  > **Note:** This control type is only used when the [`Options`](../../Reference/3dim/M3dimProjectEx.md) parameter of [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) is set to [`M_USE_BOX_WORLD_SIZE`](../../Reference/3dim/M3dimProjectEx.md) or [`M_USE_SRC_DATA`](../../Reference/3dim/M3dimProjectEx.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies that [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) will compute the size of the depth map in Y. |
| `Value > 0.0` | Specifies the size of the depth map in Y, in pixel units. |

---

### `Remap context ID`

Specifies a remap 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_REMAP_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimRemapDepthMap`](../../Reference/3dim/M3dimRemapDepthMap.md) operations.

#### `M_DIRECTION_Z`

Sets the sign of the Z-scale in the destination depth map. The sign applies to [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md), which sets the length, in world units, of one gray level in a fully-corrected depth map.  > **Note:** Note that when [`M_REMAP_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_USE_DESTINATION_CALIBRATION`](../../Reference/3dim/M3dimControl.md), this control type has no effect.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NEGATIVE` | Specifies a negative value for the Z-scale. A negative value indicates that larger Z-values are represented by darker depth map pixels, and smaller Z-values are represented by brighter depth map pixels. |
| `M_POSITIVE` | Specifies a positive value for the Z-scale. A positive value indicates that larger Z-values are represented by brighter depth map pixels, and smaller Z-values are represented by darker depth map pixels. |
| `M_REVERSE` | Specifies that the sign of the Z-scale is opposite that of the source depth map. |
| `M_SAME` *(default)* | Specifies that the sign of the Z-scale is the same as that of the source depth map. |

#### `M_MAX_Z`

Sets the maximum Z-value for the destination depth map when [`M_REMAP_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the maximum Z-value. |

#### `M_MIN_Z`

Sets the minimum Z-value for the destination depth map when [`M_REMAP_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the minimum Z-value. |

#### `M_REMAP_MODE`

Sets how to determine the Z-range of the destination depth map.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BUFFER_LIMITS` *(default)* | Specifies to remap the source depth map using its [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) settings as the values to define the destination's Z-range. |
| `M_DATA_EXTREMES` | Specifies to remap the source depth map using its maximum and minimum pixel values to define the destination's Z-range. |
| `M_GAUSSIAN` | Specifies to remap the source depth map using a specified number of standard deviations from the mean Z-value to define the destination's Z-range. Set the number of standard deviations with [`M_STANDARD_DEVIATION_FACTOR`](../../Reference/3dim/M3dimControl.md), which is applied using the following formula: `_Source mean_ +/- _Source standard deviation_` * [`M_STANDARD_DEVIATION_FACTOR`](../../Reference/3dim/M3dimControl.md). |
| `M_USE_DESTINATION_CALIBRATION` | Specifies to remap the source depth map using the destination image buffer's calibration information to define the destination's Z-range. |
| `M_USER_DEFINED` | Specifies to remap the source depth map using custom values to define the destination's Z range; use [`M_MAX_Z`](../../Reference/3dim/M3dimControl.md) and [`M_MIN_Z`](../../Reference/3dim/M3dimControl.md) to set the range. |

#### `M_STANDARD_DEVIATION_FACTOR`

Sets the standard deviation factor, for when [`M_REMAP_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_GAUSSIAN`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the standard deviation factor. |

---

### `Statistics context ID`

Specifies a statistics 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_STATISTICS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimStat`](../../Reference/3dim/M3dimStat.md) operations.

#### `M_BOUNDING_BOX`

Sets whether to enable bounding box statistics calculations. The bounding box is an axis-aligned or semi-oriented box that contains all or most of the points in the 3D scene, or it encloses a 3D geometry object.  To compute a bounding box (for a point cloud or depth map) that contains most of the valid points instead of all of them, set [`M_BOUNDING_BOX_ALGORITHM`](../../Reference/3dim/M3dimControl.md) to [`M_ROBUST`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable bounding box calculations. |
| `M_ENABLE` | Specifies to enable bounding box calculations. |

#### `M_BOUNDING_BOX_ALGORITHM`

Sets the algorithm to use when bounding box calculations have been enabled ([`M_BOUNDING_BOX`](../../Reference/3dim/M3dimControl.md)).  The default setting ([`M_ALL_POINTS`](../../Reference/3dim/M3dimControl.md)) includes all valid points; no valid points exist outside the bounding box. Invalid points can exist inside or outside the bounding box.  > **Note:** This setting is not available for 3D geometries.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL_POINTS` *(default)* | Specifies to compute the axis-aligned bounding box that contains all the valid points. |
| `M_ROBUST` | Specifies to compute an axis-aligned box that contains most of the points, but rejects outliers. |

#### `M_BOUNDING_BOX_OUTLIER_RATIO_X`

Sets the fraction of points to exclude from the bounding box, based on X-coordinates. A point is excluded when its X-coordinate places the point in an outlier position, and therefore outside the bounding box. The specified value is a decimal fraction between 0.0 and 1.0. For example, if set to 0.01, the furthest 1% of points along X are excluded.  Set this control type when bounding box calculations have been enabled ([`M_BOUNDING_BOX`](../../Reference/3dim/M3dimControl.md)) and [`M_BOUNDING_BOX_ALGORITHM`](../../Reference/3dim/M3dimControl.md) is set to [`M_ROBUST`](../../Reference/3dim/M3dimControl.md).  > **Note:** This setting is not available for 3D geometries.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 1.0` *(default)* | Specifies the fraction. |

#### `M_BOUNDING_BOX_OUTLIER_RATIO_Y`

Sets the fraction of points to exclude from the bounding box, based on Y-coordinates. A point is excluded when its Y-coordinate places the point in an outlier position, and therefore outside the bounding box. The specified value is a decimal fraction between 0.0 and 1.0. For example, if set to 0.01, the furthest 1% of points along Y are excluded.  Set this control type when bounding box calculations have been enabled ([`M_BOUNDING_BOX`](../../Reference/3dim/M3dimControl.md)) and [`M_BOUNDING_BOX_ALGORITHM`](../../Reference/3dim/M3dimControl.md) is set to [`M_ROBUST`](../../Reference/3dim/M3dimControl.md).  > **Note:** This setting is not available for 3D geometries.

#### `M_BOUNDING_BOX_OUTLIER_RATIO_Z`

Sets the fraction of points to exclude from the bounding box, based on Z-coordinates. A point is excluded when its Z-coordinate places the point in an outlier position, and therefore outside the bounding box. The specified value is a decimal fraction between 0.0 and 1.0. For example, if set to 0.01, the furthest 1% of points along Z are excluded.  Set this option when bounding box calculations have been enabled ([`M_BOUNDING_BOX`](../../Reference/3dim/M3dimControl.md)) and when [`M_BOUNDING_BOX_ALGORITHM`](../../Reference/3dim/M3dimControl.md) is set to [`M_ROBUST`](../../Reference/3dim/M3dimControl.md).  > **Note:** This setting is not available for 3D geometries.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 1.0` *(default)* | Specifies the fraction. |

#### `M_BOX_ORIENTATION`

Sets whether to compute an axis-aligned or semi-oriented bounding box.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_ALIGNED` *(default)* | Specifies to compute the minimum volume axis-aligned bounding box. |
| `M_SEMI_ORIENTED` | Specifies to compute the minimum volume bounding box that is, by default, axis-aligned in Z but not in X and Y. You can change the axis in which the semi-oriented bounding box is aligned, using [`M_BOX_SEMI_ORIENTED_ROTATION_AXIS`](../../Reference/3dim/M3dimControl.md). |

#### `M_BOX_SEMI_ORIENTED_ROTATION_AXIS`

Sets the axis with which to align the semi-oriented bounding box, when [`M_BOX_ORIENTATION`](../../Reference/3dim/M3dimControl.md) is set to [`M_SEMI_ORIENTED`](../../Reference/3dim/M3dimControl.md). The minimum volume bounding box is computed such that it is aligned with the specified axis and not constrained by the other two axes. This affects the plane to which the semi-oriented bounding box is parallel. For example, if you set this control type to [`M_AXIS_X`](../../Reference/3dim/M3dimControl.md), the semi-oriented bounding box will be axis-aligned in X and parallel to the YZ-plane.  > **Note:** This control type is only used when [`M_BOX_ORIENTATION`](../../Reference/3dim/M3dimControl.md) is set to [`M_SEMI_ORIENTED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_X` | Specifies to align the semi-oriented bounding box with the X-axis. |
| `M_AXIS_Y` | Specifies to align the semi-oriented bounding box with the Y-axis. |
| `M_AXIS_Z` *(default)* | Specifies to align the semi-oriented bounding box with the Z-axis. |
| `M_PRINCIPAL_AXIS_1` | Specifies to align the semi-oriented bounding box with the first principal axis. The first principal axis is the direction along which there is the most variance. |
| `M_PRINCIPAL_AXIS_2` | Specifies to align the semi-oriented bounding box with the second principal axis. The second principal axis is the direction along which there is the second most variance, perpendicular to the first principal axis. |
| `M_PRINCIPAL_AXIS_3` | Specifies to align the semi-oriented bounding box with the third principal axis. The third principal axis is perpendicular to the first and second principal axes. |

#### `M_CALCULATE_AVERAGE`

Sets whether to calculate the average value for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to calculate the average value. |
| `M_ENABLE` *(default)* | Specifies to calculate the average value. |

#### `M_CALCULATE_MAX`

Sets whether to calculate the maximum value for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to calculate the maximum value. |
| `M_ENABLE` *(default)* | Specifies to calculate the maximum value. |

#### `M_CALCULATE_MEDIAN`

Sets whether to calculate the median value for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the median. |
| `M_ENABLE` | Specifies to calculate the median. |

#### `M_CALCULATE_MIN`

Sets whether to calculate the minimum value for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to calculate the minimum value. |
| `M_ENABLE` *(default)* | Specifies to calculate the minimum value. |

#### `M_CALCULATE_ROBUST_STDEV`

Sets whether to calculate the robust standard deviation for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations. This is computed using the median absolute deviation (MAD) and a scale factor of 1.4826.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the robust standard deviation. |
| `M_ENABLE` | Specifies to calculate the robust standard deviation. |

#### `M_CALCULATE_STDEV`

Sets whether to calculate the standard deviation for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to calculate the standard deviation. |
| `M_ENABLE` *(default)* | Specifies to calculate the standard deviation. |

#### `M_CENTROID`

Sets whether to enable centroid statistics calculations. The centroid is the center of mass of a point cloud, depth map, or 3D geometry.  Centroid statistics include the X-, Y-, and Z-coordinates of the centroid.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable centroid calculations. |
| `M_ENABLE` | Specifies to enable centroid calculations. |

#### `M_COMPONENT_OF_INTEREST`

Sets the component for which to calculate statistics, when computing statistics on a point cloud.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COMPONENT_NORMALS_AIL` | Specifies to calculate the statistics on the normals component.  > **Note:** Note that surface variation, aspect ratio, number-of-valid-neighbors, bounding box, and nearest-neighbor statistics are not supported on the normals component, and will generate an error. |
| `M_COMPONENT_RANGE` *(default)* | Specifies to calculate the statistics on the range component. |

#### `M_DISTANCE_TO_NEAREST_NEIGHBOR`

Sets whether to enable distance-to-nearest-neighbor statistics calculations.  Distance-to-nearest-neighbor statistics include the average distance, the maximum distance, and the minimum distance of 3D points or real world coordinates to their nearest neighbor.  > **Note:** Note that distance-to-nearest-neighbor statistics are not available for 3D geometries.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable distance-to-nearest-neighbor calculations. |
| `M_ENABLE` | Specifies to enable distance-to-nearest-neighbor calculations. |

#### `M_MAX_DISTANCE`

Sets the maximum distance from a point within which other points are considered its neighbors when calculating distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that there is no maximum distance. |
| `Value > 0.0` | Specifies the maximum distance. |

#### `M_MAXIMUM_NUMBER_NEIGHBORS`

Sets the maximum number of neighbors to consider when calculating distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the maximum number of neighbors. |

#### `M_MOMENT_ORDER`

Sets the order up to which moments are calculated. For example, setting this to 3 will calculate third order moments, but also second and first order moments.  Note that there are `(_N_+1)(_N_+2)/2` moments for order _N_. For example, if [`M_MOMENT_ORDER`](../../Reference/3dim/M3dimControl.md) is set to 3, the number of computed moments are 10, 6, and 3 for third, second, and first order moments, respectively.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 15` *(default)* | Specifies the moment order. |

#### `M_MOMENTS`

Sets whether to enable moments statistics calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable moments calculations. |
| `M_ENABLE` | Specifies to enable moments calculations. |

#### `M_NEIGHBOR_SEARCH_MODE`

Sets the search mode for finding the neighbors of a point when calculating distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics.  > **Note:** Note that [`M_TREE`](../../Reference/3dim/M3dimControl.md) is typically the most precise because it chooses points that are closest in 3D space, using the Euclidean distance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to automatically set the search mode based on the point cloud's organization, either [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md) if organized or [`M_TREE`](../../Reference/3dim/M3dimControl.md) if unorganized. |
| `M_ORGANIZED` | Specifies to use the point cloud's organizational structure to determine the neighbors of a point. This option is supported only for an organized point cloud. |
| `M_TREE` *(default)* | Specifies to use a KD tree search mode to determine a point's neighbors. |

#### `M_NEIGHBORHOOD_ASPECT_RATIO`

Sets whether to enable local aspect ratio statistics calculations. For each point, Aurora Imaging Library fits a local plane to neighboring points and determines that plane's aspect ratio. The aspect ratio is calculated by dividing the central point's distance to its closest neighbor along one axis by the distance to its closest neighbor along the other axis. Local aspect ratio statistics describe how skewed the point density is in one direction compared to the other.  You typically do not want an aspect ratio higher than 5. Neighborhood-based functions do not benefit from the point cloud being denser in only one direction. In the worst case, a neighborhood with only a single row of points will produce faulty results. You can use [`M3dimSample`](../../Reference/3dim/M3dimSample.md) to subsample the point cloud to help correct aspect ratios that are too high.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable local aspect ratio calculations. |
| `M_ENABLE` | Specifies to enable local aspect ratio calculations. |

#### `M_NEIGHBORHOOD_ASPECT_RATIO_CLIPPING_VALUE`

Sets the maximum value of the neighborhood aspect ratio, to avoid skewed statistics. For example, a neighborhood that looks like a line might have an infinite aspect ratio, which will disproportionately affect statistics such as [`M_NEIGHBORHOOD_ASPECT_RATIO_AVERAGE`](../../Reference/3dim/M3dimGetResult.md) and [`M_NEIGHBORHOOD_ASPECT_RATIO_STDEV`](../../Reference/3dim/M3dimGetResult.md). To avoid this, it can be clipped to a maximum value using this control.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 100.0. This means that when the aspect ratio exceeds 100:1 (that is, 100 times more points in one direction), the aspect ratio will be clipped to 100 in statistic calculations. |
| `Value > 1.0` | Specifies the maximum value. |

#### `M_NEIGHBORHOOD_HEALTHY_THRESHOLD`

Sets the minimum number of neighbors that constitute a healthy neighborhood for the [`M_NEIGHBORHOOD_HEALTHY_PERCENTAGE`](../../Reference/3dim/M3dimGetResult.md) result type ([`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md)). Most neighborhood-based functions (for example, [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md)) will work poorly if the percentage of points with a healthy neighborhood is too low.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of neighbors. |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Sets the neighborhood size when finding the neighbors of a point in an organized point cloud for distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors calculations. The size determines the number of elements along a row or column of the 2D buffer of the range and confidence components. For example, if you set [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](../../Reference/3dim/M3dimControl.md) to 7, the neighborhood is a 7-element by 7-element region in the source container's 2D grid organization (specifically, in the organized point cloud's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components).  This control type is supported only when using an organized point cloud and only if [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](Reference/3dim/M3dimControl.md))* |  |

#### `M_NORMALIZATION_MODE`

Sets whether the normalization matrix should transform the point cloud or 3D geometry to fit a signed ([`M_NORMALIZE_SIGNED`](../../Reference/3dim/M3dimControl.md)) or unsigned ([`M_NORMALIZE_UNSIGNED`](../../Reference/3dim/M3dimControl.md)) unit box. A signed unit box has a maximum length of 2 (stretches from -1 to +1 along at least one dimension) and is centered at (0,0,0). An unsigned unit box has a maximum length of 1 (stretches from 0 to +1 along at least one dimension) and is centered at (0.5,0.5,0.5).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NORMALIZE_SIGNED` | Specifies to transform the point cloud or 3D geometry to fit a signed unit box. |
| `M_NORMALIZE_UNSIGNED` *(default)* | Specifies to transform the point cloud or 3D geometry to fit an unsigned unit box. |

#### `M_NORMALIZATION_SCALE`

Sets how to apply scale values when transforming the point cloud or 3D geometry to fit a unit box. If set to [`M_UNIFORM`](../../Reference/3dim/M3dimControl.md), each dimension is scaled using the same value such that the largest dimension fits the unit box, and the remaining dimensions will have the same or smaller lengths. If set to [`M_NON_UNIFORM`](../../Reference/3dim/M3dimControl.md), scale values are applied such that all dimensions have the same length and fit a unit cube.  > **Note:** A normalization matrix computed using non-uniform scaling cannot be applied to 3D geometries of type [`M_SPHERE`](../../Reference/3dgeo/M3dgeoInquire.md), [`M_BOX`](../../Reference/3dgeo/M3dgeoInquire.md), or [`M_CYLINDER`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NON_UNIFORM` | Applies scale values so that the transformed point cloud or 3D geometry occupies a unit cube. |
| `M_UNIFORM` *(default)* | Applies the same scale value to all dimensions. |

#### `M_NUMBER_OF_POINTS`

Sets whether to enable number-of-points statistics calculations.  Number-of-points statistics include the total number of points, the number of points that are missing data, and the number of valid points.  > **Note:** Note that number-of-points statistics are not available for 3D geometries.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable number-of-points calculations. |
| `M_ENABLE` | Specifies to enable number-of-points calculations. |

#### `M_NUMBER_OF_SAMPLES`

Sets the number of sample points to use when calculating distance-to-nearest-neighbor, surface variation, aspect ratio, and number-of-valid-neighbors statistics. By default, Aurora Imaging Library uses all points for the calculation. However, if speed is an issue, you can try specifying a fraction of the total points, which could yield a satisfactory approximate calculation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies to use all points when calculating distance-to-nearest-neighbor and surface variation statistics. |
| `Value > 0` | Specifies the number of sample points to use when calculating distance-to-nearest-neighbor and surface variation statistics. If the specified value is greater than the number of valid points, all points are used. |

#### `M_NUMBER_OF_VALID_NEIGHBORS`

Sets whether to enable number-of-valid-neighbors statistics calculations. Number-of-valid-neighbors statistics allow you to check for degenerate neighborhoods, which negatively affect neighborhood-based functions (for example, [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable number-of-valid-neighbors calculations. |
| `M_ENABLE` | Specifies to enable number-of-valid-neighbors calculations. |

#### `M_PCA`

Sets whether to enable principal component analysis (PCA) statistics calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable PCA calculations. |
| `M_ENABLE` | Specifies to enable PCA calculations. |

#### `M_PCA_MODE`

Sets whether to calculate the principal component analysis (PCA) around the origin ([`M_ORDINARY`](../../Reference/3dim/M3dimControl.md)) or around the centroid ([`M_CENTRAL`](../../Reference/3dim/M3dimControl.md)). Ordinary mode is usually only used on data that is already centralized, such as normals.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTRAL` *(default)* | Specifies to calculate the PCA around the centroid. |
| `M_ORDINARY` | Specifies to calculate the PCA around the origin. |

#### `M_SURFACE_AREA`

Sets whether to calculate the surface area of the mesh. The surface area is the sum of the area of all the mesh triangles in the container.  > **Note:** This setting is only available for 3D-processable point cloud and depth map containers that have an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable surface area statistics calculations. |
| `M_ENABLE` | Specifies to enable surface area statistics calculations. |

#### `M_SURFACE_VARIATION`

Sets whether to enable surface variation statistics calculations. For each point, Aurora Imaging Library fits a local plane to neighboring points and measures the perpendicular distance of the point to the plane. Surface variation statistics are a measure of a point cloud's noise level.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable surface variation statistics calculations. |
| `M_ENABLE` | Specifies to enable surface variation statistics calculations. |

---

### `Stitch context ID`

Specifies a stitch 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_STITCH_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) operations.

#### `M_CALIBRATION_MODE`

Sets the calibration mode with which to map depth map pixel coordinates to real-world coordinates.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically calibrate the depth maps so that the destination depth map includes all the source depth maps. |
| `M_USER_DEFINED` | Specifies to calibrate the depth maps using the values set with [`M_OFFSET_X`](../../Reference/3dim/M3dimControl.md) and [`M_OFFSET_Y`](../../Reference/3dim/M3dimControl.md). |

#### `M_INTERPOLATION_FALLBACK_MODE`

Sets the type of interpolation to use when stitching the depth maps and there are invalid pixels in the neighborhood of a point. Note that bilinear interpolation is used when all pixels in the neighborhood of a point are valid.  Interpolation is only done when [`M_STITCH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_PRECISE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INTERPOLATION` *(default)* | Specifies interpolated resizing. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

#### `M_OFFSET_X`

Sets the X-coordinate of the top-left corner of the destination depth map.  > **Note:** This control type is only used when [`M_CALIBRATION_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate, in world units. |

#### `M_OFFSET_Y`

Sets the Y-coordinate of the top-left corner of the destination depth map.  > **Note:** This control type is only used when [`M_CALIBRATION_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate, in world units. |

#### `M_OVERLAP_MODE`

Sets how to determine the pixel value when two or more depth maps overlap.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` *(default)* | Specifies to use the average value of the pixels in the overlapping region. |
| `M_MAX` | Specifies to use the maximum value of the pixels in the overlapping region. |
| `M_MIN` | Specifies to use the minimum value of the pixels in the overlapping region. |

#### `M_SIZE_X`

Sets the size of the stitched destination depth map along the X dimension.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the size of the depth map in X, in world units. |

#### `M_SIZE_Y`

Sets the size of the stitched destination depth map along the Y dimension.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the size of the depth map in Y, in world units. |

#### `M_STATIC_INDEX`

Sets the source depth map to which all other depth map pixels will be stitched. For example, if depth maps 1 and 2's pixel size=1, depth map 1 starts at world x=3, and depth map 2 starts at world x=5.25, depth map 2 will be stitched to the nearest depth map 1 pixel, such that the intensity values of the destination pixels at world x=5 and world x=6 will be those of the source pixels at world x=5.25 and world x=6.25, or their intensity values will be interpolated (depending on the value of [`M_STITCH_MODE`](../../Reference/3dim/M3dimControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to use the first depth map in the array. |
| `Value > 0` | Specifies the index of the depth map to use. This value must be less than the value passed to the [`NumContainers`](../../Reference/3dim/M3dimMerge.md) parameter of [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md). |

#### `M_STITCH_DIRECTION`

Sets the direction in which to stitch the point clouds' component values. Note that stitching does not depend on the coordinates of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DIRECTION_X` | Specifies to stitch the component values of each successive point cloud to the right of the previous point cloud's component values. |
| `M_DIRECTION_Y` *(default)* | Specifies to stitch the component values of each successive point cloud below the previous point cloud's component values. |

#### `M_STITCH_MODE`

Sets the algorithm to use for stitching depth maps.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FAST` *(default)* | Specifies that the depth maps will be stitched quickly, but less accurately. The depth maps are stitched to the nearest pixel. No interpolation is done, even if the source depth maps are misaligned. |
| `M_PRECISE` | Specifies that the depth maps will be accurately stitched. Interpolation is done to accurately align the depth maps when stitching them. |

---

### `Subsample context ID`

Specifies a subsample 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimSample`](../../Reference/3dim/M3dimSample.md) subsampling operations.  You can set the type of subsampling operation to perform with [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md).

#### `M_DISTINCT_ANGLE_DIFFERENCE`

Sets the minimum angle difference between the normal vector of a point and that of its neighbors, for the point to be kept in the subsample. If a point's normal vector angle is distinct from that of all of its neighbors, the point is kept. Use [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimControl.md) to define the neighborhood of a point.  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_NORMAL`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 180.0` *(default)* | Specifies the minimum angle difference, in degrees. |

#### `M_FRACTION_OF_POINTS`

Sets the fraction of all valid points to select from the source point cloud. Non-valid points are not considered in the calculation.  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_RANDOM`](../../Reference/3dim/M3dimControl.md) or [`M_SUBSAMPLE_GEOMETRIC`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 1.0` *(default)* | Specifies the fraction of all valid points to select randomly from the source point cloud. |

#### `M_GRID_SIZE_X`

Sets the cell size in the X-direction.  > **Note:** Note that, when [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md), exactly 1 of [`M_GRID_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_GRID_SIZE_Y`](../../Reference/3dim/M3dimControl.md), or [`M_GRID_SIZE_Z`](../../Reference/3dim/M3dimControl.md) must be set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies an infinite cell size along X. |
| `Value > 0.0` *(default)* | Specifies the cell size along X, in world units. |

#### `M_GRID_SIZE_Y`

Sets the cell size in the Y-direction.  > **Note:** Note that, when [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md), exactly 1 of [`M_GRID_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_GRID_SIZE_Y`](../../Reference/3dim/M3dimControl.md), or [`M_GRID_SIZE_Z`](../../Reference/3dim/M3dimControl.md) must be set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies an infinite cell size along Y. |
| `Value > 0.0` *(default)* | Specifies the cell size along Y, in world units. |

#### `M_GRID_SIZE_Z`

Sets the cell size in the Z-direction.  > **Note:** Note that, when [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md), exactly 1 of [`M_GRID_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_GRID_SIZE_Y`](../../Reference/3dim/M3dimControl.md), or [`M_GRID_SIZE_Z`](../../Reference/3dim/M3dimControl.md) must be set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies an infinite cell size along Z. |
| `Value > 0.0` *(default)* | Specifies the cell size along Z, in world units. |

#### `M_NEIGHBORHOOD_DISTANCE`

Sets the distance that defines the neighborhood of a point; for a point to be kept in the subsample, it must be distinct in this neighborhood. Use [`M_DISTINCT_ANGLE_DIFFERENCE`](../../Reference/3dim/M3dimControl.md) to set whether a point is distinct from other points in its neighborhood.  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_NORMAL`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the neighborhood distance, in world units. |

#### `M_ORGANIZATION_TYPE`

Sets the resulting subsampled point cloud's organizational type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ORGANIZED` | Specifies an organized resulting subsampled point cloud.  > **Note:** This control value only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md) or [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md). Note also that, when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md), [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md) is supported only when the source point cloud is organized. |
| `M_UNORGANIZED` *(default)* | Specifies an unorganized resulting subsampled point cloud. |

#### `M_POINT_SELECTED`

Sets which point to choose when multiple points occupy the same cell, and when 1 of [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) is set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that this control type is ignored when none of [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) is set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default point selection mode. |
| `M_CENTER` | Specifies to select the point closest to the center of each 2D cell, as if the point cloud is collapsed onto the plane formed by the 2 non-infinite dimensions. |
| `M_MAX` *(default)* | Specifies to select the highest point along the [`M_INFINITE`](../../Reference/3dim/M3dimControl.md) axis, for each cell. |
| `M_MIN` | Specifies to select the lowest point along the [`M_INFINITE`](../../Reference/3dim/M3dimControl.md) axis, for each cell. |

#### `M_SEED_VALUE`

Sets the seed for the random selection of points, for an [`M_SUBSAMPLE_RANDOM`](../../Reference/3dim/M3dimControl.md) operation.  When set to [`M_DEFAULT`](../../Reference/3dim/M3dimControl.md), the same sequence of random samples is generated with each call to [`M3dimSample`](../../Reference/3dim/M3dimSample.md) (for the same source point cloud).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the seed for the random selection of points. |

#### `M_STEP_SIZE_X`

Sets the step interval between points, along the X dimension.  Points are selected from the source point cloud's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components, where the point cloud's X-, Y-, and Z-coordinates are stored in separate bands of a 2D (organized) or 1D (unorganized) buffer. [`M_STEP_SIZE_X`](../../Reference/3dim/M3dimControl.md) specifies an interval in X for each band. If the resulting point cloud is organized, invalid points are included; otherwise they are ignored.  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the step interval value. |

#### `M_STEP_SIZE_Y`

Sets the step interval between points, along the Y dimension.  Points are selected from the source point cloud's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components, where the point cloud's X-, Y-, and Z-coordinates are stored in separate bands of a 2D (organized) or 1D (unorganized) buffer.  > **Note:** Note that, if the source point cloud is unorganized, [`M_STEP_SIZE_Y`](../../Reference/3dim/M3dimControl.md) is ignored.  [`M_STEP_SIZE_Y`](../../Reference/3dim/M3dimControl.md) specifies an interval in Y for each band in the range and confidence components. If the resulting point cloud is organized, invalid points are included; otherwise they are ignored.  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the step interval value. |

#### `M_SUBSAMPLE_MODE`

Sets the type of subsampling operation to perform.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SUBSAMPLE_DECIMATE` *(default)* | Specifies to subsample using a decimation operation, which selects points at regular intervals. Set the interval size with [`M_STEP_SIZE_X`](../../Reference/3dim/M3dimControl.md) and [`M_STEP_SIZE_Y`](../../Reference/3dim/M3dimControl.md).  You can set the resulting subsampled point cloud's organizational type with [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md) can sometimes generate undesirable regular patterns. To avoid such patterns, use random mode ([`M_SUBSAMPLE_RANDOM`](../../Reference/3dim/M3dimControl.md)) instead. |
| `M_SUBSAMPLE_GEOMETRIC` | Specifies to perform a geometrically stable subsampling, which selects points that help 3D registration converge faster and avoid local minima. The underlying algorithm identifies and rejects points that are likely to cause registration to slide away from convergence, while retaining points that constrain rotational and translational transforms during registration.  You can set the fraction of points that are retained from the source point cloud with [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md). |
| `M_SUBSAMPLE_GRID` | Specifies to subsample using a grid operation, which divides the 3D space into cells and, for each subgroup of points, selects a single point per cell.  If you want the grid operation to select a particular fraction of points from the source point cloud, you can use [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) to calculate the cell size required. Storing or copying the lattice results into a subsample 3D image processing context automatically sets [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) to [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md), [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) to the calculated lattice's cell sizes, and, if 1 of the cell dimensions is infinite, [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md). Otherwise, set the cell size with [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) and the resulting subsampled point cloud's organizational type with [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md).  > **Note:** Note that, when [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md), exactly 1 of [`M_GRID_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_GRID_SIZE_Y`](../../Reference/3dim/M3dimControl.md), or [`M_GRID_SIZE_Z`](../../Reference/3dim/M3dimControl.md) must be set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md). When [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_UNORGANIZED`](../../Reference/3dim/M3dimControl.md), you can set at most 1 of [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).  > **Note:** When one of [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) is set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md), the point selected for a cell depends on the mode specified with [`M_POINT_SELECTED`](../../Reference/3dim/M3dimControl.md). When none of [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) is set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md), the point selected for a cell is the point closest to the cell's volumetric center. |
| `M_SUBSAMPLE_NORMAL` | Specifies to subsample based on normal vector angles and a neighborhood distance. A neighborhood point is selected only if its surface normal vector is distinct from that of all of its neighbors ([`M_DISTINCT_ANGLE_DIFFERENCE`](../../Reference/3dim/M3dimControl.md)) within the neighborhood defined with [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimControl.md). |
| `M_SUBSAMPLE_RANDOM` | Specifies to subsample using an operation that randomly selects a specified fraction of points from the source point cloud. Set the seed for the random number generation with [`M_SEED_VALUE`](../../Reference/3dim/M3dimControl.md); set the fraction with [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md). |

#### `M_SUBSAMPLE_NORMAL_MODE`

Sets the algorithm to use for subsampling based on normals.  This control type only applies when [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_SUBSAMPLE_NORMAL`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FAST` *(default)* | Specifies faster but approximate subsampling based on normals. Some extra, non-distinct points might be selected in the same neighborhood. |
| `M_PRECISE` | Specifies exact subsampling based on normals. All selected points will be distinct in their neighborhood. |

---

### `Surface sample context ID`

Specifies a surface sample 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SURFACE_SAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md), and used in [`M3dimSample`](../../Reference/3dim/M3dimSample.md) surface sampling operations.

#### `M_RESOLUTION`

Sets the resolution at which to sample the mesh or 3D geometry. This represents the distance between neighboring points on the surface; the smaller the resolution, the more points generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the distance between newly added points on the surface, in world units. |

#### `M_SAMPLE_MODE`

Sets the mode of the surface sampling operation. The mode establishes whether the destination point cloud will have a mesh structure.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SURFACE_GRID` *(default)* | Specifies that surface sampling invalidates the mesh component and the destination container will contain a denser non-meshed point cloud. If an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the destination container, it is deleted. |
| `M_TESSELLATION` | Specifies that surface sampling preserves the mesh component and the destination container will contain a denser meshed point cloud. |
