---
doctype: Reference
module: 3dim
function: M3dimCopyResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimCopyResult"
---

# M3dimCopyResult

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

> Copy a group of results into a 3D geometry object, transformation matrix object, subsample 3D image processing context, or container.

## Syntax

```c
void M3dimCopyResult(
    AIL_ID    SrcResult3dimId,  //in
    AIL_ID    DstAilObjectId,   //out
    AIL_INT64 CopyType,         //in
    AIL_INT64 ControlFlag       //in
)
```

## Description

This function copies a group of 3D image processing results into a 3D geometry object, transformation matrix object, subsample 3D image processing context, or container, according to the specified copy operation. You can copy statistics results, profile results, lattice results, or find transformation results (calculated using [`M3dimStat`](../../Reference/3dim/M3dimStat.md), [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md), [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md), or [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md), respectively).

## Parameters

### `SrcResult3dimId` *(in, AIL_ID)*

Specifies the identifier of a 3D image processing result buffer.

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of a 3D geometry object, transformation matrix object, subsample 3D image processing context, or container.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the copy type and destination object for a 3D image processing result buffer

---

### `Find transformation 3D image processing result buffer ID from which to copy`

Specifies the identifier of a find transformation 3D image processing result buffer from which to copy. The result buffer must have been previously allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_FIND_TRANSFORMATION_RESULT`](../../Reference/3dim/M3dimAllocResult.md). The result buffer must contain the results of a call to [`M3dimFindTransformation`](../../Reference/3dim/M3dimFindTransformation.md), which finds the transformation between a source set of points and a target set of points.

#### `M_TRANSFORMATION_MATRIX`

Specifies to copy, into the transformation matrix, find transformation results that transform source coordinate values into target coordinate values as best as possible, when used with [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md).

| Value | Description |
| --- | --- |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the transformation matrix object in which to copy the find transformation results. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `Lattice 3D image processing result buffer ID from which to copy`

Specifies the identifier of a lattice 3D image processing result buffer from which to copy. The result buffer must have been previously allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_LATTICE_RESULT`](../../Reference/3dim/M3dimAllocResult.md). The result buffer must contain the results of a call to [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md), which calculates the lattice for a point cloud.

#### `M_BOUNDING_BOX`

Specifies to copy the lattice's bounding box into the specified 3D geometry object, establishing a 3D box geometry.

| Value | Description |
| --- | --- |
| `3D geometry object ID in which to copy` | Specifies the identifier of a 3D geometry object in which to copy the lattice results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).  Note that, after calling [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md), the destination 3D geometry object will resturn type [`M_BOX`](../../Reference/blob/MblobControl.md) if inquired using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_GEOMETRY_TYPE`](../../Reference/3dgeo/M3dgeoInquire.md). |

#### `M_DEPTH_MAP_CONTAINER + Depth value`

Specifies to copy lattice results into a container, establishing a 3D-processable depth map container with the same size and calibration of the lattice. This sets the size of the container's range component to [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_START_POINT_...`](../../Reference/3dim/M3dimGetResult.md) and the scales to [`M_CELL_SIZE_...`](../../Reference/3dim/M3dimGetResult.md).  Note that you must also specify the bit depth (8, 16, or 32).

| Value | Description |
| --- | --- |
| `Container ID in which to copy` | Specifies the container in which to copy the lattice results. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container. |

#### `M_SUBSAMPLE_CONTEXT`

Specifies to copy lattice results into the subsample 3D image processing context. This sets the [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) control type of the subsample 3D image processing context to [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md) and its [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) control types to the cell sizes of the lattice results ([`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_CELL_SIZE_...`](../../Reference/3dim/M3dimGetResult.md)). If 1 of the cell dimensions is infinite, [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md).

| Value | Description |
| --- | --- |
| `Subsample 3D image processing context ID in which to copy` | Specifies the identifier of the subsample 3D image processing context in which to copy the lattice results. The context must have been previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |

---

### `Profile 3D image processing result buffer ID from which to copy`

Specifies the identifier of a profile 3D image processing result buffer from which to copy. The result buffer must have been previously allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_PROFILE_RESULT`](../../Reference/3dim/M3dimAllocResult.md). The result buffer must contain the results of a call to [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md), which takes the profile of a depth map, 3D geometry, mesh, or point cloud.

#### `M_MATRIX_PROFILE_PLANE_TO_WORLD`

Specifies to copy, into the transformation matrix, profile results that transform [`M_PROFILE_PLANE_...`](../../Reference/3dim/M3dimGetResult.md) coordinate values into [`M_WORLD_...`](../../Reference/3dim/M3dimGetResult.md) coordinate values, when used with [`M3dimMatrixTransformList`](../../Reference/3dim/M3dimMatrixTransformList.md).  Note that [`M_PROFILE_PLANE_...`](../../Reference/3dim/M3dimGetResult.md) coordinates are expressed in the slicing plane's coordinate system, while [`M_WORLD_...`](../../Reference/3dim/M3dimGetResult.md) coordinates are expressed in the working coordinate system.

| Value | Description |
| --- | --- |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the transformation matrix object in which to copy the profile results. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_MATRIX_WORLD_TO_PROFILE_PLANE`

Specifies to copy, into the transformation matrix, profile results that transform [`M_WORLD_...`](../../Reference/3dim/M3dimGetResult.md) coordinate values into [`M_PROFILE_PLANE_...`](../../Reference/3dim/M3dimGetResult.md) coordinate values, when used with [`M3dimMatrixTransformList`](../../Reference/3dim/M3dimMatrixTransformList.md).  Note that [`M_WORLD_...`](../../Reference/3dim/M3dimGetResult.md) coordinates are expressed in the working coordinate system, while [`M_PROFILE_PLANE_...`](../../Reference/3dim/M3dimGetResult.md) coordinates are expressed in the slicing plane's coordinate system.

| Value | Description |
| --- | --- |
| *(see [`M_MATRIX_PROFILE_PLANE_TO_WORLD`](Reference/3dim/M3dimCopyResult.md))* |  |

---

### `Statistics 3D image processing result buffer ID from which to copy`

Specifies the identifier of a statistics 3D image processing result buffer from which to copy. The result buffer must have been previously allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_STATISTICS_RESULT`](../../Reference/3dim/M3dimAllocResult.md). The result buffer must contain the results returned by a call to [`M3dimStat`](../../Reference/3dim/M3dimStat.md), which computes statistics on a point cloud, depth map, or 3D geometry.

#### `M_BOUNDING_BOX`

Specifies to copy bounding box statistics results into the specified 3D geometry object, establishing a 3D box geometry.

| Value | Description |
| --- | --- |
| `3D geometry object ID in which to copy` | Specifies the identifier of a 3D geometry object in which to copy the statistics results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).  Note that, after calling [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md), the destination 3D geometry object will return type [`M_BOX`](../../Reference/3dgeo/M3dgeoInquire.md) if inquired using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_GEOMETRY_TYPE`](../../Reference/3dgeo/M3dgeoInquire.md). |

#### `M_BOX_CENTER`

Specifies to copy bounding box center coordinate statistics results into the specified 3D geometry object, establishing a 3D point geometry.

| Value | Description |
| --- | --- |
| `3D geometry object ID in which to copy` | Specifies the identifier of a 3D geometry object in which to copy the statistics results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).  Note that, after calling [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md), the destination 3D geometry object will return type [`M_POINT`](../../Reference/3dgeo/M3dgeoInquire.md) if inquired using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_GEOMETRY_TYPE`](../../Reference/3dgeo/M3dgeoInquire.md). |

#### `M_CENTROID`

Specifies to copy centroid statistics results into the specified 3D geometry object, establishing a 3D point geometry.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_CENTER`](Reference/3dim/M3dimCopyResult.md))* |  |

#### `M_FIXTURING_MATRIX`

Specifies to copy, into the transformation matrix, principal component analysis (PCA) statistics results that can bring points along the object's principal axes to the X-, Y-, and Z-axes. This matrix is the inverse of the PCA matrix ([`M_PCA_MATRIX`](../../Reference/3dim/M3dimCopyResult.md)).

| Value | Description |
| --- | --- |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the transformation matrix object in which to copy the principal component analysis (PCA) statistics results. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `M_NORMALIZATION_MATRIX`

Specifies to copy, into the transformation matrix, bounding box statistics results that can transform the bounding box into a unit box. The unit box's maximum length is 1, except in the case of a signed unit box, whose maximum length is 2.  Use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_NORMALIZATION_MODE`](../../Reference/3dim/M3dimControl.md) to specify whether the transformed point cloud or 3D geometry should fit a signed or unsigned unit box.  When [`M_NORMALIZATION_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_NORMALIZE_SIGNED`](../../Reference/3dim/M3dimControl.md), the unit box of the transformed cloud is signed, centered at (0,0,0), and its unit length is at most 2 (stretches from -1 to +1).  When [`M_NORMALIZATION_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_NORMALIZE_UNSIGNED`](../../Reference/3dim/M3dimControl.md), the unit box of the transformed cloud is unsigned, centered at (0.5,0.5,0.5), and its unit length is at most 1 (stretches from 0 to +1).  When [`M_NORMALIZATION_SCALE`](../../Reference/3dim/M3dimControl.md) is set to [`M_UNIFORM`](../../Reference/3dim/M3dimControl.md), the same scale factor is applied to all dimensions. This means that the largest dimension is scaled to fit the unit box, and the remaining dimensions will have the same or smaller lengths.  When [`M_NORMALIZATION_SCALE`](../../Reference/3dim/M3dimControl.md) is set to [`M_NON_UNIFORM`](../../Reference/3dim/M3dimControl.md), a different scale factor is applied along each dimension and the resulting box is a unit cube.  > **Note:** This copy operation is not available for a semi-oriented bounding box.

| Value | Description |
| --- | --- |
| *(see [`M_FIXTURING_MATRIX`](Reference/3dim/M3dimCopyResult.md))* |  |

#### `M_PCA_MATRIX`

Specifies to copy, into the transformation matrix, PCA statistics results that can bring points on the X-, Y-, and Z-axes to the object's principal axes.  > **Note:** If [`M_PCA_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_CENTRAL`](../../Reference/3dim/M3dimControl.md), the matrix contains a translation and rotation to the centroid; otherwise, it is a pure rotation. The centroid is that of the original depth map or point cloud from which the statistics were calculated. If the source was a point cloud, it is the centroid of the points or the points' unit normal vectors, depending on whether the statistics were calculated on the range or normals component, respectively.

| Value | Description |
| --- | --- |
| *(see [`M_FIXTURING_MATRIX`](Reference/3dim/M3dimCopyResult.md))* |  |

#### `M_PRINCIPAL_AXIS_1`

Specifies to copy PCA statistics results into the specified 3D geometry object, establishing a 3D line geometry; the line marks the first principal axis and has a length of 1 unit.  > **Note:** If [`M_PCA_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_CENTRAL`](../../Reference/3dim/M3dimControl.md), the line starts at the centroid; otherwise, it starts at the origin. The centroid is that of the original depth map or point cloud from which the statistics were calculated. If the source was a point cloud, it is the centroid of the points or the points' unit normal vectors, depending on whether the statistics were calculated on the range or normals component, respectively.

| Value | Description |
| --- | --- |
| `3D geometry object ID in which to copy` | Specifies the identifier of a 3D geometry object in which to copy the statistics results. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).  Note that, after calling [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md), the destination 3D geometry object will return type [`M_LINE`](../../Reference/3dgeo/M3dgeoInquire.md) if inquired using [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_GEOMETRY_TYPE`](../../Reference/3dgeo/M3dgeoInquire.md). |

#### `M_PRINCIPAL_AXIS_2`

Specifies to copy PCA statistics results into the specified 3D geometry object, establishing a 3D line geometry; the line marks the second principal axis and has a length of 1 unit.  > **Note:** If [`M_PCA_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_CENTRAL`](../../Reference/3dim/M3dimControl.md), the line starts at the centroid; otherwise, it starts at the origin. The centroid is that of the original depth map or point cloud from which the statistics were calculated. If the source was a point cloud, it is the centroid of the points or the points' unit normal vectors, depending on whether the statistics were calculated on the range or normals component, respectively.

| Value | Description |
| --- | --- |
| *(see [`M_PRINCIPAL_AXIS_1`](Reference/3dim/M3dimCopyResult.md))* |  |

#### `M_PRINCIPAL_AXIS_3`

Specifies to copy PCA statistics results into the specified 3D geometry object, establishing a 3D line geometry; the line marks the third principal axis and has a length of 1 unit.  > **Note:** If [`M_PCA_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_CENTRAL`](../../Reference/3dim/M3dimControl.md), the line starts at the centroid; otherwise, it starts at the origin. The centroid is that of the original depth map or point cloud from which the statistics were calculated. If the source was a point cloud, it is the centroid of the points or the points' unit normal vectors, depending on whether the statistics were calculated on the range or normals component, respectively.

| Value | Description |
| --- | --- |
| *(see [`M_PRINCIPAL_AXIS_1`](Reference/3dim/M3dimCopyResult.md))* |  |

#### `M_STANDARDIZATION_MATRIX`

Specifies to copy, into the transformation matrix, moments statistics results that can scale the point cloud to dimensions at or near one unit. This means that the centroid is positioned at (0,0,0) and the point cloud is transformed so that it has unit variance along each axis. Therefore, on average, each point's X-coordinate will be one unit away from the origin, and similarly for each Y- and Z-coordinate.  > **Note:** Note that the moments results must be of order 2 or greater.  To transform the point cloud to fit inside a unit box, copy bounding box statistic results using [`M_NORMALIZATION_MATRIX`](../../Reference/3dim/M3dimCopyResult.md).

| Value | Description |
| --- | --- |
| *(see [`M_FIXTURING_MATRIX`](Reference/3dim/M3dimCopyResult.md))* |  |
