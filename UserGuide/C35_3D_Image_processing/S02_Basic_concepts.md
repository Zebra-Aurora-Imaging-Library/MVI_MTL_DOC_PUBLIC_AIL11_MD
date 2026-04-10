---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Basic_concepts
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Basic concepts"
---

# Basic concepts for 3D image processing

The basic concepts and vocabulary conventions for 3D image processing are:

- **Bounding box**. The smallest axis-aligned box containing all the valid points in a point cloud. For a depth map, the bounding box encompasses real world coordinates that correspond to valid depth map pixels.
- **Depth map**. An image where the gray value of a pixel represents its depth in the world.
- **Extent box**. The maximum box that a depth map spans, irrespective of its data. This is the minimum/maximum X-, Y-, and Z-coordinates that would be representable in the depth map image buffer, according to its size and calibration information.
- **Invalid point**. A data value that cannot be resolved to a valid point in a point cloud or a valid pixel in a depth map. For a point cloud container, invalid data correspond to a confidence score of 0. For a depth map, missing or otherwise unresolvable pixel data are referred to as invalid points.
- **Mesh**. A triangulated surface generated for a point cloud or depth map.
- **Organized point cloud**. A point cloud whose data is stored in a container with 2D components. This 2D grid organization reflects spatial proximity between nearby elements; that is, points in the point cloud that are near each other are also stored near each other in the 2D component of the container. Processing of an organized point cloud is usually more efficient; some 3D image processing operations require organized point clouds.
- **Unorganized point cloud**. A point cloud whose data is stored in a container with 1D components. There is no spatial correspondence between nearby elements in the buffer.
- **Working coordinate system**. The implicit real world coordinate system used to express the coordinates of 3D points in a point cloud or 3D geometry object. Each point cloud container has its own working coordinate system. You might need to transform the points in the point cloud so that they are expressed in the same working coordinate system as another point cloud.
