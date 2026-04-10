---
doctype: UserGuide
part: "3D related information"
chapter: 3D_reconstruction_using_laser_line_profiling
section: Basic_concepts
module_tag: 3dmap
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_reconstruction_using_laser_line_profiling / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library 3D Reconstruction module using laser line profiling

The basic concepts and vocabulary conventions for the Aurora Imaging Library 3D Reconstruction module using laser line or sheet of light profiling are:

- **3D camera capable of laser line extraction**. A camera with integrated processing capabilities that extracts laser line or sheet of light information from sequential scans and generates an uncorrected depth map, and depending on the camera, a point cloud.
- **3D reconstruction context**. An Aurora Imaging Library object that stores information regarding the reconstruction setup, such as the 3D reconstruction calibration information, camera position, and object speed.
- **3D reconstruction setup**. A physical setup used to extract 3D information from 2D images.
- **Camera calibration context**. An Aurora Imaging Library object that stores the mapping between the pixel coordinate system and the absolute world coordinate system, as well as information about the camera calibration setup, including the initial locations of all associated world coordinate systems.
- **Camera setup**. The camera's position relative to the object in the physical setup used to acquire images.
- **Depth**. A measurement extending from the surface of the object to its base, in world units.
- **Depth map**. An image where the gray value of a pixel represents its depth in the world.
- **Fully corrected depth map.** A depth map that accurately represents the height and shape of objects; its pixels represent a constant size in X and Y in the world. If the size of the pixels is the same in X and Y, the fully corrected depth map is also considered a corrected image.
- **Gap**. A range of data which is missing in the depth map because the data could not be extracted from the image at this location.
- **Intensity map**. An image where the gray value of each pixel represents the luminous intensity of the laser line at this point.
- **Laser line coordinate system**. A static coordinate system with the X-axis along the laser line, the Y-axis along the conveyor movement, and the Z-axis perpendicular to the conveyor. While respecting these constraints, the direction of the axes and the position of the origin are as close as possible to the absolute coordinate system.
- **Laser line profiling**. A process which extracts 3D spatial information from the position and intensity of a laser line in an image.
- **Partially corrected depth map**. A depth map that accurately represents the height of objects, but not their shape.
- **Uncorrected depth map**. A depth map where the gray level and shape of the scanned object(s) are not corrected. An uncorrected depth map contains the raw accumulated laser line data from the different laser line images. If the laser line appears horizontally in the laser line images, the pixel values in a given row (_m_) of the uncorrected depth map equal the vertical distance of the laser line for each column in the corresponding laser line image m. If the laser line appears vertically in the laser line images, the pixel values in a given row (_m_) of the uncorrected depth map equal the horizontal distance of the laser line for each row in the corresponding laser line image _m_.
- **XY-plane**. The plane defined by the equation Z=0. This is typically the conveyor in the laser line coordinate system.
- **XZ-plane**. The plane defined by the equation Y=0.
