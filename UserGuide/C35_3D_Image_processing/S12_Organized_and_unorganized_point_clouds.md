---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Organized_and_unorganized_point_clouds
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Organized and unorganized point clouds"
---

# Organized and unorganized point clouds

A point cloud can be organized or unorganized. In an organized point cloud, points close to each other in 3D space are stored close to each other in the pixel coordinate systems of the components. Typically, you can ignore whether a point cloud is organized; most operations are supported for both organized and unorganized point clouds. However, in some cases, you can improve performance with an organized or unorganized point cloud.

You can determine if a point cloud is organized using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)with [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md)and[`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md). If the returned value is [`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufInquireContainer.md), the point cloud is unorganized; otherwise, it is organized.

> **Note:** Setting the 3D representation of a range component to indicate that it is organized when the underlying data is unorganized can lead to unexpected results.

Some operations have modes that can only be specified for an organized point cloud. For example, for a normals 3D image processing context, you can use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with[`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md). In this case,[`M3dimNormals`](../../Reference/3dim/M3dimNormals.md) will use the organization of the point cloud to calculate the normals more efficiently.

> **Note:** Modes specific to organized point clouds are not available when the range component is a buffer with a single row ([`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md) is equal to 1), even if the data is organized.

If you have an organized point cloud with a large number of invalid points, it might be more efficient to convert it to an unorganized point cloud with only valid points (using [`M3dimRemovePoints`](../../Reference/3dim/M3dimRemovePoints.md)). This reduces the memory required to store the point cloud, and improves the performance of some operations. Typically, this will only improve the performance of your application if you need to perform multiple operations with the same point cloud.

Typically, 3D data grabbed from a 3D sensor is organized. A point cloud converted from a depth map using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)is always organized. You can create an organized point cloud that has a subset of the points of an unorganized point cloud by subsampling it using [`M3dimSample`](../../Reference/3dim/M3dimSample.md) with a subsample 3D image processing context that specifies an [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md)operation and an[`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md)organization type.
