---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_processing_and_analysis_overview
section: Basic_concepts
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_processing_and_analysis_overview / Basic concepts"
---

# Basic concepts for 3D processing and analysis

The basic concepts and vocabulary conventions for 3D processing and analysis are:

- **3D-displayable container **. A container that has the[`M_DISP`](../../Reference/buf/MbufAllocContainer.md)attribute and components correctly formatted for it to be shown in a 3D display (using functions from the[`M3ddisp...`](../../Reference/3ddisp/M3ddispAlloc.md) module).
- **3D geometry object**. An Aurora Imaging Library object that stores one of several 3D shapes (box, cylinder, line, plane, or sphere). The Aurora Imaging Library object is allocated and defined for use in 3D functions to limit or define the processing region of a point cloud or depth map.
- **3D-processable point cloud container **. A point cloud container that can be passed as a source and/or destination to Aurora Imaging Library 3D processing functions. You can inquire if a point cloud container is 3D-processable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md).
- **3D-processable depth map container **. A depth map container that can be passed as a source and/or destination to Aurora Imaging Library 3D processing functions. You can inquire if a depth map container is 3D-processable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md).
- **3D reconstruction setup**. A physical setup used to extract 3D information from images.
- **3D virtual model**. A CAD software model of an object, where the dimensions of the model are to scale.
- **Camera calibration context**. An Aurora Imaging Library object that stores the mapping between the pixel coordinate system and the world coordinate system, as well as information about the camera calibration setup, and the initial locations of all the coordinate systems.
- **Camera-laser triangulation** (see laser line profiling).
- **Camera setup**. The camera's position relative to the object in the physical setup used to acquire images.
- **Depth**. A measurement extending from the surface of the object to the XY plane (that is, the z=0 plane), in world units.
- **Depth map**. An image where the gray value of a pixel represents its depth in the world.
- **Fully corrected depth map.** A depth map that accurately represents the height and shape of portrayed objects; its pixels represent a constant size in X and Y in the world. Note that, when dealing with depth maps, the size of the pixels is not necessarily square.
- **Laser line profiling**. A process whereby grabbed images of objects passing through a sheet of light or laser plane permit the reconstruction of 3D information about the objects in the form of a point cloud or depth map. The object under the laser light displaces the laser line, and the degree of displacement reveals height (depth) information about the object.
- **Partially corrected depth map**. A depth map that accurately represents the height of portrayed objects, but not their shape.
- **Point cloud**. A set of 3D points representing objects in a scene.
- **Pose**. The established position and orientation of an object in space.
- **Reference plane**. A plane with a known height and orientation, and used as a measure against which to compute distances, angles, volumes, or other calculations.
- **Stereo vision triangulation**. A process that calculates a point's (X, Y, Z) coordinates in the world if it is seen by at least two calibrated camera setups.
- **Transformation matrix object**. An Aurora Imaging Library object that stores a 4x4 matrix that defines a required modification, such as transforming coordinates to fixture a point cloud or 3D geometry to a required position and orientation.
- **XY-plane**. The plane defined by the equation Z=0 in a world coordinate system.
- **XZ-plane**. The plane defined by the equation Y=0 in a world coordinate system.
