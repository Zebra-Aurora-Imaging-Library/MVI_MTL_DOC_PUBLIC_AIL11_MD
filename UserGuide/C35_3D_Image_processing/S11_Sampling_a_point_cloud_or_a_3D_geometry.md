---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Sampling_a_point_cloud_or_a_3D_geometry
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Sampling a point cloud or a 3D geometry"
---

# Sampling a point cloud or a 3D geometry

You can use the [`M3dimSample`](../../Reference/3dim/M3dimSample.md) function to reduce the number of points in a point cloud (subsampling), or increase the number of points in a meshed point cloud (surface sampling). You can also surface sample a 3D geometry to generate a point cloud. Subsampling is useful, for example, to maintain an approximately similar distance between points after a merge operation, or when compressing point cloud data. You can also subsample an unorganized point cloud to obtain an organized point cloud.

## Subsampling a point cloud

To subsample, allocate a subsample 3D image processing context using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md) and set the type of subsampling operation to perform using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md). Choose from the modes below.

> **Note:** Note that you can specify the organizational type of the resulting point cloud ([`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md)). An [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md) output point cloud is available only for [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md) or [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md) operations. When [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_UNORGANIZED`](../../Reference/3dim/M3dimControl.md), invalid points are removed.

For information on point cloud organization, see [Organized and unorganized point clouds](S12_Organized_and_unorganized_point_clouds.md).

### Decimate mode

Decimate mode ([`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md)) selects points at regular intervals (set with [`M_STEP_SIZE_X`](../../Reference/3dim/M3dimControl.md) and [`M_STEP_SIZE_Y`](../../Reference/3dim/M3dimControl.md)). This is the default mode and is typically used with organized point clouds. This mode is typically faster than the other modes.

The following code snippet is an example of the steps required to subsample a point cloud in decimate mode:

> **Code example:** [userguide.3d_processing.working_with_points_in_a_point_cloud03](userguide.3d_processing.working_with_points_in_a_point_cloud03)

> **Note:** Note that [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md) can sometimes generate undesirable regular patterns. To avoid such patterns, use random mode ([`M_SUBSAMPLE_RANDOM`](../../Reference/3dim/M3dimControl.md)) instead.

### Random mode

Random mode ([`M_SUBSAMPLE_RANDOM`](../../Reference/3dim/M3dimControl.md)) selects points using an operation that randomly selects a specified fraction of points from the source point cloud. Set the seed for the random number generator with [`M_SEED_VALUE`](../../Reference/3dim/M3dimControl.md); set the fraction with [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md).

### Geometric mode

Geometric mode ([`M_SUBSAMPLE_GEOMETRIC`](../../Reference/3dim/M3dimControl.md)) selects points using a geometrically stable subsampling operation. This mode is intended to help 3D registration converge faster and avoid local minima. The underlying algorithm identifies and rejects points that are likely to cause registration to slide away from convergence, while retaining points that constrain rotational and translational transforms during registration. You can set the fraction of points that are retained from the source point cloud with [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md).

### Grid mode

Grid mode ([`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md)) selects points using a grid operation, which divides the 3D space into cells. For each cell, [`M3dimSample`](../../Reference/3dim/M3dimSample.md) selects one point: either the point closest to the cell's center, or the selection is determined using the mode specified with [`M_POINT_SELECTED`](../../Reference/3dim/M3dimControl.md).

Note that if you want the grid operation to select a particular fraction of points from the source point cloud, or you want to use a grid in which sparsity changes plateau as the grid size increases, you can use [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) to calculate the cell size required. Use [`M_LATTICE_MODE`](../../Reference/3dim/M3dimControl.md) to set the mode. You can set the fraction of points with [`M_FRACTION_OF_POINTS`](../../Reference/3dim/M3dimControl.md) and [`M_FRACTION_OF_POINTS_TOLERANCE`](../../Reference/3dim/M3dimControl.md), or set the tolerance to use for identifying sparsity changes with [`M_SPARSITY_DIMENSION_TOLERANCE`](../../Reference/3dim/M3dimControl.md), before calling [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md). You can store the lattice results in a subsample 3D image processing context, or copy the lattice results into a subsample 3D image processing context, using [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimCopyResult.md). This sets the [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) control type of the subsample 3D image processing context to [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md), its [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) control types to the calculated lattice's cell sizes, and, if 1 of the cell dimensions is infinite, its [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) control type to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md).

If you do not use [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) to calculate a lattice, set the cell size with [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md).

You can use grid mode to organize an unorganized point cloud (set [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md)). Grid mode is also useful after merging point clouds (using [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md)) to ensure an approximately constant spacing between points.

The following code snippet is an example of the steps required to subsample an unorganized point cloud using the grid mode, and generate a new, organized point cloud:

> **Code example:** [userguide.3d_processing.working_with_points_in_a_point_cloud01](userguide.3d_processing.working_with_points_in_a_point_cloud01)

> **Note:** Note that, when [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md), exactly 1 of [`M_GRID_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_GRID_SIZE_Y`](../../Reference/3dim/M3dimControl.md), or [`M_GRID_SIZE_Z`](../../Reference/3dim/M3dimControl.md) must be set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md). When [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_UNORGANIZED`](../../Reference/3dim/M3dimControl.md), you can set at most 1 of [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md).

> **Note:** When one of [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) is set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md), the point selected for a cell depends on the mode specified with [`M_POINT_SELECTED`](../../Reference/3dim/M3dimControl.md), which is, by default, the highest point along the [`M_INFINITE`](../../Reference/3dim/M3dimControl.md) axis ([`M_MAX`](../../Reference/3dim/M3dimControl.md)). When none of [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) is set to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md), the point selected for a cell is the point closest to the cell's volumetric center.

The following image shows options for the [`M_POINT_SELECTED`](../../Reference/3dim/M3dimControl.md) control type. Note that the [`M_CENTER`](../../Reference/3dim/M3dimControl.md) option selects the point closest to the center of each 2D cell, as if the point cloud is collapsed onto the plane formed by the 2 non-infinite dimensions.

*[Image: 3dim_subsample_grid.png]*

### Normal mode

Normal mode ([`M_SUBSAMPLE_NORMAL`](../../Reference/3dim/M3dimControl.md)) selects points based on normal vector angles and neighborhood distance (set with [`M_DISTINCT_ANGLE_DIFFERENCE`](../../Reference/3dim/M3dimControl.md) and [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimControl.md)). This mode is best used when compressing the source point cloud. Points around key features like corners and edges are kept, while the number of points on flat surfaces is reduced.

The following image shows the result of using the normal mode with [`M_DISTINCT_ANGLE_DIFFERENCE`](../../Reference/3dim/M3dimControl.md) and [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimControl.md).

*[Image: 3dim_subsample_normal.png]*

The following code snippet is an example of the steps required to subsample a point cloud in normal mode. Note the specified [`M_DISTINCT_ANGLE_DIFFERENCE`](../../Reference/3dim/M3dimControl.md) and [`M_NEIGHBORHOOD_DISTANCE`](../../Reference/3dim/M3dimControl.md) values. The 12 degree distinct angle difference specifies that the point is kept if all neighboring points' normal vectors differ by more than 12 degrees from the examined point's normal vector angle. The neighborhood within which to consider the normal vector of surrounding points is limited to 10 world units from the examined point.

> **Code example:** [userguide.3d_processing.working_with_points_in_a_point_cloud02](userguide.3d_processing.working_with_points_in_a_point_cloud02)

## Surface sampling a meshed point cloud

You can use [`M3dimSample`](../../Reference/3dim/M3dimSample.md) to surface sample a meshed point cloud and generate a denser point cloud. This is useful, for example, when performing 3D registration with an acquired point cloud and a CAD model. A point cloud restored from a CAD file is often sparse, with large surface triangles on flat surfaces. You can apply a surface sample operation to the CAD-sourced point cloud to increase its density and subsequently improve registration results, since both point clouds should have a similar point density. Note that you can use [`MbufImport`](../../Reference/buf/MbufImport.md) or [`MbufRestore`](../../Reference/buf/MbufRestore.md) to import/restore an acquired point cloud or a CAD file; for a CAD file, the data must have been saved in the PLY or STL file format.

To perform surface sampling, use [`M3dimSample`](../../Reference/3dim/M3dimSample.md) with a surface sample 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SURFACE_SAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). [`M3dimSample`](../../Reference/3dim/M3dimSample.md) samples the surface, and places a denser point cloud into the destination container. Use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_RESOLUTION`](../../Reference/3dim/M3dimControl.md) to control the density of newly added points.

*[Image: 3dim_sampling_a_mesh.png]*

An [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component must exist in the source point cloud. You can choose whether a mesh is generated for the resulting point cloud using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SAMPLE_MODE`](../../Reference/3dim/M3dimControl.md). Grid mode ([`M_SURFACE_GRID`](../../Reference/3dim/M3dimControl.md)) divides each triangle into a grid and adds a point for each cell within the triangle. Tessellation mode ([`M_TESSELLATION`](../../Reference/3dim/M3dimControl.md)) subdivides the triangles until the required resolution is reached.

## Surface sampling a 3D geometry

You can also sample a finite 3D geometry to generate a point cloud. To do so, use [`M3dimSample`](../../Reference/3dim/M3dimSample.md) with a surface sample 3D image processing context, allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SURFACE_SAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). You must pass a 3D geometry object that you have previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). Note that infinite 3D geometries, such as infinite cylinders, lines, or planes are not supported. [`M3dimSample`](../../Reference/3dim/M3dimSample.md) generates the components [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md). The function also generates an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component for box, cylinder, and sphere 3D geometries, but not for points or lines.
