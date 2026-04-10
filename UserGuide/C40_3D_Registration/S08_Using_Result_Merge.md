---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Registration
section: Using_Result_Merge
module_tag: 3dreg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Registration / Using Result Merge"
---

# Merging point clouds, retrieving specific results, and drawing results

From a group of point clouds, the Aurora Imaging Library 3D Registration module can create a single point cloud that contains all the transformed points of the original point clouds using the [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md) function. Essentially, the working coordinate systems of all point clouds are aligned using the results of the registration operation, and their points are merged in a single point cloud container. By default, they are aligned to the global coordinate system; you can optionally specify to align the working coordinate systems of all point clouds to a specified point cloud using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_MERGE_LOCATION`](../../Reference/3dreg/M3dregControl.md).

> **Note:** If you want to merge multiple point clouds without performing any transformations, or without using any registration results, you can use [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md) instead.

Merging point clouds with [`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md) requires the registration results of a successful call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md). These results are stored in a pairwise 3D registration result buffer; the results specify the transformations that will be applied to the point clouds during the merge operation.

Optionally, you can specify a subsample 3D image processing context to subsample the merged point cloud. This is useful for removing any duplicate points in overlapping regions of the merged point cloud. For this application, it is recommended that you specify [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md) as the subsampling operation (using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md)), and specify values for [`M_GRID_SIZE_...`](../../Reference/3dim/M3dimControl.md) according to your application.

## Retrieving specific registration results

After a successful call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md), you can retrieve any of the individual transformations from the pairwise 3D registration result buffer, using [`M3dregCopyResult`](../../Reference/3dreg/M3dregCopyResult.md) with [`M_REGISTRATION_MATRIX`](../../Reference/3dreg/M3dregCopyResult.md). These transformations specify how to align the working coordinate system of one point cloud to another point cloud.

For example, in the following topology, each point cloud is the reference for only one other point cloud:

*[Image: 3dreg_PointCloudTopology_Inefficient.png]*

However, it is possible to retrieve the transformation that aligns the working coordinate system of any point cloud, to any other point cloud in the topology:

*[Image: 3dreg_PointCloudTopology_RetrieveResults.png]*

These individual transformations can be useful if, for example, you want to use one of them for the preregistration iteration in a different registration operation.

## Drawing results

The [`M3dregControlDraw`](../../Reference/3dreg/M3dregControlDraw.md) function provides several operations for drawing results into a 3D graphics list; for each registration result element, you can draw:

- Points that overlap reference points.
- Excluded points.
- Lines between paired points.

By default, Aurora Imaging Library draws all overlapping and excluded points for the specified registration result element(s). To draw other results (such as lines between paired points), you must explicitly enable the relevant draw operation (using [`M3dregControlDraw`](../../Reference/3dreg/M3dregControlDraw.md)).

You can specify how to draw the above graphics. For example, you can set the color, thickness, and opacity of most features. Optionally, for overlapping points, you can use one of the point cloud container's components to set the color of the points. When doing so, you can also map the specified component's values to a color in a LUT. For a complete description of all possible options for the draw, refer to the description of [`M3dregControlDraw`](../../Reference/3dreg/M3dregControlDraw.md) in the Aurora Imaging Library Reference.

Note that points are represented as squares in a 3D display. You can specify the thickness of overlapping or excluded points in number of pixels, and they will always be shown with sides of this length when displayed, regardless of their distance from the 3D display's viewpoint.

To perform a draw operation, you typically allocate a draw 3D registration context to hold the settings for the draw, using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dreg/M3dregAlloc.md). You then specify draw operations and options for the draw with successive calls to [`M3dregControlDraw`](../../Reference/3dreg/M3dregControlDraw.md). Finally, you call [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md) to perform the draw.

Alternatively, you can skip allocating a draw 3D registration context and instead use a predefined draw 3D registration context with default draw settings. To do so, directly call [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md) with [`M_DEFAULT`](../../Reference/3dreg/M3dregDraw3d.md). Note that the default settings specify to draw points with a thickness of one pixel.
