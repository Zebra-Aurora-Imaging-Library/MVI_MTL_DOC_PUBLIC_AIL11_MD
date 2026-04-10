---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Registration
section: Basic_concepts
module_tag: 3dreg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Registration / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library 3D Registration module

The basic concepts and vocabulary conventions for using the Aurora Imaging Library 3D Registration module are:

- **Preregistration**. The first iteration of the registration operation, where a point cloud is moved to a specified rough location close to its reference.
- **Pose**. The established position and orientation of an object in space.
- **Reference point cloud**. The point cloud to which another point cloud is registered.
- **Registration**. The registration operation defines a transformation that aligns the working coordinate systems of different point clouds.
- **Registration divergence and sliding**. Depending on several factors (for example, the shape of the point clouds, their density, amount of overlap, initial rough location), with each iteration of the registration operation, a point cloud might not move closer to its reference, or ever find the optimal pose relative to its reference. Divergence occurs when the point clouds move apart. Sliding occurs when the registration algorithm cannot find a singular optimal pose for the point cloud, and instead shifts between several eligible poses.
- **Registration element**. A data structure that is located in a 3D registration context. Registration elements store the settings that control the registration of a specific point cloud. This includes information about the point cloud's reference and its rough location in that reference.
- **Registration element's point cloud**. The point cloud, whose index in the point cloud container array, is the same as the index of a particular registration element. The registration of a point cloud is controlled by the contents of its associated registration element.
- **Registration result element**. A data structure that is located in a 3D registration result buffer. Registration result elements store the results of a specific point cloud. This includes information about the result status of the registration operation for that specific point cloud, and the transformation matrix calculated in the registration operation.
- **Root-mean-square (RMS) error**. In the Aurora Imaging Library 3D Registration module, the RMS error is a measure of the overall distance between all paired points in two point clouds. A lower error indicates a smaller overall distance between the two point clouds, and vice versa.
  *[Image: 3dreg_RootMeanSquareError_eq.png]*
- **Topology**. In the Aurora Imaging Library 3D Registration module, topology refers to the defined relationship between all points clouds involved in the registration operation.
