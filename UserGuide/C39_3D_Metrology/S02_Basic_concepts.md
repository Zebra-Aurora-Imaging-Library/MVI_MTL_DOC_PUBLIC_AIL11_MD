---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Metrology
section: Basic_concepts
module_tag: 3dmet
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Metrology / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library 3D Metrology module

The basic concepts and vocabulary conventions for the Aurora Imaging Library 3D Metrology module are:

- **Depth**. A measurement extending from the surface of the object to the XY-plane (that is, the Z=0 plane), in world units.
- **Depth map**. An image where the gray value of a pixel represents its depth in the world.
- **Fully corrected depth map**. A depth map that accurately represents the height and shape of portrayed objects; its pixels represent a constant size in X and Y in the world. Note that, when dealing with depth maps, the size of the pixels is not necessarily square.
- **Initial fit estimate**. Specifies an initial estimate for the position of a 3D geometry, relative to the point cloud or depth map, at the start of the fitting operation.
- **Mesh**. A triangulated surface generated for a point cloud.
- **Pose**. The established position and orientation of an object in space.
- **Reference plane**. A plane with a known height and orientation, and used as a measure against which to compute distances, angles, volumes, or other calculations.
- **Root-mean-square (RMS) error**. In the Aurora Imaging Library 3D Metrology module, the RMS error is a measure of the overall distance between two Aurora Imaging Library objects. A lower error indicates a smaller overall distance between the two Aurora Imaging Library objects, and vice versa.
  *[Image: 3dreg_RootMeanSquareError_eq.png]*
- **XY-plane**. The plane defined by the equation Z=0 in a world coordinate system.
- **XZ-plane**. The plane defined by the equation Y=0 in a world coordinate system.
