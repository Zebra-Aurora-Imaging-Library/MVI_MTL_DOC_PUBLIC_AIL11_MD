---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Defining_which_points_to_use
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Defining which points to use"
---

# Defining which points to use

When working with a point cloud, you can typically specify whether to use a 3D box geometry object to delimit which points to use for processing and analysis operations. You can also remove invalid points. By default, all valid points are used. You can use [`M3dimGet`](../../Reference/3dim/M3dimGet.md) to retrieve the component values that correspond to the valid points in the source point cloud container.

*[Image: 3dmap_extraction_box_basic.png]*

You can define the size of the 3D box geometry by specifying coordinates and/or dimensions, such as X, Y, or Z lengths. See [Steps to defining and using a 3D geometry object](../C44_Using_the_3D_Geometry_module/S02_Defining_3D_geometries.md) for more information. Alternatively, you can use 3D box geometry results from other modules.

The following code snippet is an example of the steps required to generate a fully corrected depth map from a box region of a point cloud:

> **Code example:** [userguide.3d_processing.generating_fully_corrected_depth_intensity_maps01](userguide.3d_processing.generating_fully_corrected_depth_intensity_maps01)

## Specifying a bounding box excluding outliers

To exclude outliers, you can use [`M3dimStat`](../../Reference/3dim/M3dimStat.md) to calculate the bounding box of a point cloud excluding outliers. In this case, [`M3dimStat`](../../Reference/3dim/M3dimStat.md) calculates the smallest axis-aligned box that contains all the valid points in a point cloud except those suspected to be outliers. You can then copy the resulting bounding box into a 3D box geometry, using [`M3dimCopyResult`](../../Reference/3dim/M3dimCopyResult.md) with [`M_BOUNDING_BOX`](../../Reference/3dim/M3dimCopyResult.md). For more information, see [Determining the bounding box of a point cloud](S13_Calculating_statistics_on_a_point_cloud_or_depth_map.md).

## Specifying the extent box

To include only points in the maximum area that a depth map can span given the image buffer size and the calibration information of that buffer, you can copy the depth map's extent box into a 3D geometry object. In this case, the resulting 3D box geometry's dimensions are calculated to represent this maximum area. To do so, use [`M3dimCopy`](../../Reference/3dim/M3dimCopy.md) with [`M_EXTENT_BOX`](../../Reference/3dim/M3dimCopy.md). [`M3dimCopy`](../../Reference/3dim/M3dimCopy.md) uses the following information from the depth map: the position of the origin of the pixel coordinate system in the relative coordinate system, the three dimensional scales ([`M_PIXEL_SIZE_X`](../../Reference/cal/McalInquire.md), [`M_PIXEL_SIZE_Y`](../../Reference/cal/McalInquire.md), and [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalInquire.md)), and the dimensions of the depth map image buffer.

## Removing invalid points

You can remove invalid points from a point cloud. For example, if you require only valid points for a processing operation, you can call [`M3dimGet`](../../Reference/3dim/M3dimGet.md), which will retrieve a component's values that correspond only to valid points.

The following code snippet shows how to retrieve the coordinates of only valid points from a container:

> **Code example:** [userguide.3d_processing.generating_fully_corrected_depth_intensity_maps02](userguide.3d_processing.generating_fully_corrected_depth_intensity_maps02)

You can also call [`M3dimRemovePoints`](../../Reference/3dim/M3dimRemovePoints.md) to remove invalid points from the point cloud before performing another operation. For example, the following code snippet shows how to keep only points whose normals point upward:

> **Code example:** [userguide.3d_processing.generating_fully_corrected_depth_intensity_maps03](userguide.3d_processing.generating_fully_corrected_depth_intensity_maps03)

Note that the above snippet assumes that the normals are of unit length, and considers a point's normal to point upward if its Z-component has a value greater than or equal to 0.9.

To remove points whose confidence is equal to zero, use [`M3dimRemovePoints`](../../Reference/3dim/M3dimRemovePoints.md) with [`M_INVALID_POINTS_ONLY`](../../Reference/3dim/M3dimRemovePoints.md). To remove both these points and points whose normal vector values are zero, use [`M_INVALID_NORMALS`](../../Reference/3dim/M3dimRemovePoints.md) instead. Note that, when using [`M3dimRemovePoints`](../../Reference/3dim/M3dimRemovePoints.md), the resulting point cloud is always unorganized.

## Copying points to user-defined arrays

You can use [`M3dimGetList`](../../Reference/3dim/M3dimGetList.md) to copy a source container's values into destination arrays, while also choosing to keep or exclude invalid points. When calling [`M3dimGetList`](../../Reference/3dim/M3dimGetList.md) with a point cloud container, you must specify from which component to copy values. The function has options for casting, storing the data in packed or planar format, and returns the number of points written to the arrays. You can also pass a depth map to [`M3dimGetList`](../../Reference/3dim/M3dimGetList.md).
