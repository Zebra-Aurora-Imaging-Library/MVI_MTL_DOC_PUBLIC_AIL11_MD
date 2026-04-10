---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Generating_a_mesh_for_a_point_cloud
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Generating a mesh for a point cloud"
---

# Generating a mesh for a point cloud or depth map

You can pass a point cloud or depth map container to [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md) to generate a mesh, which is a reconstructed triangulated surface for the point cloud or depth map.

Creating a mesh can improve results for some 3D operations. For example, when using [`M3dimProject`](../../Reference/3dim/M3dimProject.md) or [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) to generate a depth map from an organized point cloud, the source point cloud can have widely varying distances between points, depending on the position of objects and surfaces during acquisition. This can happen when using a snapshot type of 3D sensor, which captures the point cloud from a single point of view. If you first generate a mesh for the point cloud before projecting it, the resulting depth map is often more satisfactory, and eliminates the need to fill excessively large gaps. In other cases, you might have a point cloud generated from a 3D profile sensor, where you have regular gaps in one dimension. If you first create a mesh, projecting the mesh-based point cloud typically yields better results. In general, a depth map produced from a meshed point cloud results in a more natural-looking reconstructed surface.

When displaying point clouds, a meshed point cloud typically shows surface texture better than non-meshed point clouds that are displayed as individual points.

If you are using Microsoft 3D Viewer with PLY files, see [PLY files and 3D Viewer](../C02_Building_an_application/S12_Working_with_3D.md) for more information.

A mesh is required if you want to calculate the surface area of a point cloud or depth map.

## Surface reconstruction modes

The [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md) function generates a mesh according to the specified mesh 3D image processing context ([`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_MESH_CONTEXT`](../../Reference/3dim/M3dimAlloc.md)). Use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) to set up the context. Choose the reconstruction mode ([`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md)) with which to create the mesh. You can choose from the following modes:

- **Organized mode** ([`M_MESH_ORGANIZED`](../../Reference/3dim/M3dimControl.md)). Connects existing points according to organizational information, and can result in a mesh with holes, due to invalid points in the source point cloud or depth map.
- **Local plane mode** ([`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md)). Connects existing points, using local planes computed from each point's neighborhood to establish which neighbors to connect. With this mode, the source point cloud can be organized or unorganized and must have a normals component. The resulting mesh can have holes.
- **Smoothed mode** ([`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimControl.md)). Generates a smooth surface even when the source point cloud is noisy; missing data is approximated when possible, resulting in significantly fewer holes. This mode requires a normals component. Note that when using the smoothed mode, there will probably be more points in the resulting point cloud and the points will probably not be the same as that of the source point cloud.

The following images show the result of applying the organized, local plane, and smoothed mesh modes to the same point cloud.

*[Image: 3dim_mesh_modes.png]*

Note the regularity of the mesh produced using the organized mode. This is especially evident on flat or smoothly transitioning surfaces; however, where there are sharper transitions and edges, the mesh is not as smooth, since the operation is simply connecting neighboring points. The hole above the eye is due to the specified step interval ([`M_MESH_STEP_...`](../../Reference/3dim/M3dimControl.md)), which in this case is smaller than the point spacing in the original point cloud. Increasing the step interval might remove a few holes, at the cost of losing detail, since the triangular planes that form the mesh would become larger. Regardless, a mesh with holes remains a possibility, since the specified step interval can still land on a point whose confidence is 0 (an invalid point), preventing the formation of a mesh triangle at that point. Note that when creating a mesh for a depth map, only organized mode is supported.

For the local plane mesh, the surface triangles are less regular. This is due in large part to the relatively broad settings of the [`M_SURFACE_ANGLE_MAX`](../../Reference/3dim/M3dimControl.md) and [`M_TRIANGLE_ANGLE_...`](../../Reference/3dim/M3dimControl.md) control types specified for this example. Holes in the mesh appear where the specified angle constraints were not met (or where the resulting surface triangle would be too large). You can set stricter values for these control types to obtain a higher quality mesh; however, more holes are likely. Typically for the local plane mode, there is a trade-off between mesh quality and number of holes.

The smoothed mesh is much smoother than the others, since noise reduction was applied to the point cloud before reconstructing the surface. In this case, the operation has filled holes, although hole filling is not guaranteed when using the smoothed mesh mode. The suitability of this mode's smoothing properties depends on your application.

[`M3dimMesh`](../../Reference/3dim/M3dimMesh.md) stores the generated mesh in the mesh component of the destination container. Once you have generated a mesh for a point cloud, you can use [`M3dimRemovePoints`](../../Reference/3dim/M3dimRemovePoints.md) with [`M_NOT_MESH_POINTS`](../../Reference/3dim/M3dimRemovePoints.md) to remove from the point cloud all points whose confidence is equal to zero, as well as points that are not part of the mesh. Note that the resulting point cloud after an [`M3dimRemovePoints`](../../Reference/3dim/M3dimRemovePoints.md) operation is always unorganized.
