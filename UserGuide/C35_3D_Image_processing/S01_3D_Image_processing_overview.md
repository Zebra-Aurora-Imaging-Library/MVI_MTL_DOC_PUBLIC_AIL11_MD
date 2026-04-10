---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: 3D_Image_processing_overview
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / 3D Image processing overview"
---

# 3D Image Processing module overview

The 3D Image Processing module supports processing and basic analysis of 3D data.

Using this module, you can perform arithmetic and statistical operations on point clouds, depth maps, and 3D geometries. You can scale, rotate, and translate 3D data, compute 3D profiles to examine a cross-section of data, and sample a subset of points or reconstruct surfaces for point clouds. You can also merge multiple point clouds or depth maps, crop or mask points, and calculate normal vectors for each point based on neighboring points. Additionally, the 3D image processing module allows you to project point cloud or 3D geometry data into a fully corrected image buffer, thereby generating a depth map.

To operate on depth maps, they must be stored in an image buffer and be fully corrected (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)) or be stored in an Aurora Imaging Library 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)). All 3D-processable depth map containers have a range component ([`M_COMPONENT_RANGE`](../../Reference/buf/MbufControlContainer.md)). The range component stores the Z-coordinate of each point; the XY-coordinates are stored implicitly by the point's column and row position in the pixel coordinate system, multiplied by [`M_3D_SCALE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_3D_SCALE_Y`](../../Reference/buf/MbufInquireContainer.md), respectively.

To operate on point clouds, they must be stored in an Aurora Imaging Library 3D-processable point cloud format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). All 3D-processable point cloud containers have the 2 components [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md). The range component stores coordinates for 3D points and the confidence component stores a confidence score for each point, where a confidence score of 0 indicates an invalid point. If a confidence is not 0, the point is valid.

Note that, for source containers, points in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component whose corresponding confidence score is 0 have no effect on the computation performed. For destination containers, the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component value of a point with 0 confidence can be any value, and that value might change when the range component is copied, or when read on a different system.

Some 3D image processing functions create and add new components to the destination container, or copy source components into the destination. Typically, previously existing components in the destination container are overwritten.

Some functions might free and reallocate components, which can change a component's Aurora Imaging Library identifier.

In-place processing is supported. A temporary internal container is used if necessary.
