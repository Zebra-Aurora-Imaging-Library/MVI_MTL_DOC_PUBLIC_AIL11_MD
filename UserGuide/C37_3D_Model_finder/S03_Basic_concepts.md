---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Model_finder
section: Basic_concepts
module_tag: 3dmod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Model_finder / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library 3D Model Finder module

The basic concepts and vocabulary conventions for the Aurora Imaging Library 3D Model Finder module are:

- **3D model finder context.** An Aurora Imaging Library object that stores the model for which to search. The 3D model finder context also stores global search settings that apply to the search algorithm.
- **Bounding box.** The smallest axis-aligned box that contains the ideal geometric shape of the fitted model at the location of an occurrence.
- **Central axis unit vector.** A vector of unit length that points along a cylinder model's central axis. It points from the center of the cylinder's first base towards the center of its second base.
- **Fit distance.** The maximum distance from the occurrence at which a point can be included in the fit.
- **Inlier points.** Points in the point cloud that are considered part of a model occurrence. A point must be within the fit distance from the surface of the occurrence to be considered an inlier.
- **Model.** The information that defines the pattern of points to find in the point cloud. The 3D Model Finder module supports box, rectangular plane, cylinder, sphere, and surface models.
- **Model coverage.** The percentage of the model's surface found in the occurrence.
- **Occurrence.** An instance of the model found in the point cloud.
- **Point cloud.**A set of 3D points representing objects in a scene.
- **Reference direction.** The position of the 3D sensor or the direction of the 3D sensor's line of sight relative to the acquired point cloud.
- **Reserved points.** Points that are reserved in an area around an occurrence; they are considered part of the occurrence but are not used when calculating the fit. They cannot be considered part of any other occurrence.
- **Root-mean-square (RMS) error.**A measure of how well the points in an occurrence fit the model. A perfect fit gives a root-mean-square error of 0.0.
  *[Image: 3dmod_rms-error-eq.png]*
- **Score.** A measure of how well an occurrence matches a model. The score is defined as the model coverage.
- **Working coordinate system.**The implicit real world coordinate system used to express the coordinates of 3D points in a point cloud. Each point cloud container has its own working coordinate system.
