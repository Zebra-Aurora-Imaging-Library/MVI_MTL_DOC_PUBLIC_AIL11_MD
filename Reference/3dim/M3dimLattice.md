---
doctype: Reference
module: 3dim
function: M3dimLattice
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimLattice"
---

# M3dimLattice

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

> Calculate a lattice structure for a container's point cloud.

## Syntax

```c
void M3dimLattice(
    AIL_ID    LatticeContext3dimId,          //in
    AIL_ID    ContainerBufId,                //in
    AIL_ID    Box3dgeoId,                    //in
    AIL_ID    LatticeResultOrContext3dimId,  //out
    AIL_INT64 ControlFlag                    //in
)
```

## Description

This function calculates a lattice that divides points into cells. You can use the calculated lattice to subsample or project the point cloud, where one point is selected per cell, or all points in a cell are projected into the same depth map pixel, respectively.

Use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) to set constraints on the lattice before calling this function. For example, you can specify the relative cell dimensions (that is, the sizes of X, Y, and Z relative to each other) using [`M_RELATIVE_CELL_SIZE_...`](../../Reference/3dim/M3dimControl.md).

Results are stored in the specified lattice 3D image processing result buffer or subsample 3D image processing context. You can retrieve results from the lattice 3D image processing result buffer, using [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md).

To use the lattice to subsample the point cloud, specify a subsample 3D image processing context as the destination for results. Alternatively, you can store results in a lattice 3D image processing result buffer, and then copy them into a subsample 3D image processing context, using [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimCopyResult.md). When you call [`M3dimSample`](../../Reference/3dim/M3dimSample.md) with a subsample 3D image processing context containing lattice results, a grid operation is performed where one point is selected per cell in the lattice. Empty cells or cells containing only invalid points are ignored when the resulting point cloud is unorganized, whereas they result in points with 0 confidence when the resulting point cloud is organized.

You can use this function instead of [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) to calculate a recommended depth map buffer size for projection. [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) recommends a size such that most points map to their own pixel. You should use [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) if you want multiple points to map to the same pixel. Use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_LATTICE_MODE`](../../Reference/3dim/M3dimControl.md) to specify the mode with which to calculate the lattice. If using [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md), you can specify the required ratio of valid pixels in the depth map to valid points in the point cloud, using [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md) and [`M_FRACTION_OF_POINTS_TOLERANCE`](../../Reference/3dim/M3dimControl.md). If using [`M_SPARSITY_CHANGE`](../../Reference/3dim/M3dimControl.md), you can specify the tolerance to use for identifying sparsity changes in grid sizes, using [`M_SPARSITY_DIMENSION_TOLERANCE`](../../Reference/3dim/M3dimControl.md). In this case, when calling [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md), you must store results in a lattice 3D image processing result buffer, so that you can then retrieve the results to set up an appropriate depth map image buffer or container for [`M3dimProject`](../../Reference/3dim/M3dimProject.md); for more information, see [Generating fully corrected depth and intensity maps](../../UserGuide/C35_3D_Image_processing/S15_Generating_a_fully_corrected_depth_map.md). When you call [`M3dimProject`](../../Reference/3dim/M3dimProject.md), all the points in a cell are projected into the same pixel of the depth map image buffer or depth map container. Empty cells or cells containing only invalid points will result in invalid pixels.

## Parameters

### `LatticeContext3dimId` *(in, AIL_ID)*

Specifies the identifier of the lattice 3D image processing context. The context must have been previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_LATTICE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

### `ContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the source container containing a 3D-processable point cloud. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)).

### `Box3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the axis-aligned 3D box geometry object that defines the space for which to calculate the lattice. Only points inside the box are used to calculate the lattice. The 3D box geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

### `LatticeResultOrContext3dimId` *(out, AIL_ID)*

Specifies the identifier of the lattice 3D image processing result buffer or subsample 3D image processing context in which to write the results.

*For specifying the lattice 3D image processing result buffer or subsample 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `Lattice 3D image processing result buffer identifier` | Specifies the identifier of a lattice 3D image processing result buffer, previously allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_LATTICE_RESULT`](../../Reference/3dim/M3dimAllocResult.md). |
| `Subsample 3D image processing context identifier` | Specifies the identifier of a subsample 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). This sets the [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) control type of the subsample 3D image processing context to [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md) and its [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) control types to the cell sizes of the lattice results ([`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_CELL_SIZE_...`](../../Reference/3dim/M3dimGetResult.md)). If 1 of the cell dimensions is infinite, [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md).

Note that this is equivalent to specifying a lattice 3D image processing result buffer and using [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimCopyResult.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
