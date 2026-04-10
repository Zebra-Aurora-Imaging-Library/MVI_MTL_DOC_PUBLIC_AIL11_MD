---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_analysis_using_planar_views_of_an_object
section: Performing_stereo_vision_triangulation
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_analysis_using_planar_views_of_an_object / Performing stereo vision triangulation"
---

# Performing stereo vision triangulation

Stereo vision triangulation (stereophotogrammetry) is the process of finding an object's location in 3D space, using two or more planar views of the object. In stereo vision triangulation, an object's feature must be seen by at least two cameras. If the positions of the cameras in 3D space are known, the feature's location on each camera's image plane and the camera's location in the world are used to calculate the feature's location in the world. In Aurora Imaging Library, you can perform stereo vision triangulation using [`M3dmapTriangulate`](../../Reference/3dmap/M3dmapTriangulate.md).

The following image shows the projection of point A on the image plane of two cameras. The intersection of point B and C's projection line results in the location of point A in 3D space.

*[Image: 3dmap_triangulation_theory.png]*

However, in reality, the projection of a point on a camera's image plane is not perfect. Rounding and extraction errors prevent point A from being projected with accuracy on the cameras' image planes.

*[Image: 3dmap_triangulation_real.png]*

Points B' and C' will be the points used in the calculation and point D is the resulting approximation of A.

Additional cameras increase the accuracy of the result. However, not all cameras need to see the point; only two cameras are necessary for stereo vision triangulation, but the more cameras involved, the more accurate the result.

## Matching common points to perform stereo vision triangulation

To do stereo vision triangulation, [`M3dmapTriangulate`](../../Reference/3dmap/M3dmapTriangulate.md) requires that you match points (features) common in at least two images. That is, you must identify common points which are seen by at least two cameras, identify the points' coordinates in their respective images and supply the coordinates to [`M3dmapTriangulate`](../../Reference/3dmap/M3dmapTriangulate.md). For example, in the following image, the points (X<sub>A0</sub>, Y<sub>A0</sub>), (X<sub>B0</sub>, Y<sub>B0</sub>), and (X<sub>C0</sub>, Y<sub>C0</sub>) are paired together. (X<sub>A1</sub>, Y<sub>A1</sub>) and (X<sub>B1</sub>, Y<sub>B1</sub>) are paired together, but camera C does not see this point.

*[Image: 3dmap_triangulation_pair_points.png]*

You can pair multiple points with a single call to [`M3dmapTriangulate`](../../Reference/3dmap/M3dmapTriangulate.md) by specifying their coordinates. To pair the points given in the previous example, you would give the following arguments to [`M3dmapTriangulate`](../../Reference/3dmap/M3dmapTriangulate.md):

*[Image: 3dmap_triangulation_array.png]*

Since camera C cannot see one of the points, you need to specify **M_INVALID_POINT** for the appropriate indices.

Note that you also need to specify the identifier of the camera calibration contexts for the different camera setups. The order in which you specify the identifiers must match the order in which you specify the coordinates.

> **Note:** Note that stereo vision triangulation requires that all the cameras be calibrated in a 3D mode using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md). All cameras must also share a common 3D coordinate system. For more information on calibrating your camera setup, see [Calibrating your camera setup](../C28_Calibration/ChapterInformation.md).
