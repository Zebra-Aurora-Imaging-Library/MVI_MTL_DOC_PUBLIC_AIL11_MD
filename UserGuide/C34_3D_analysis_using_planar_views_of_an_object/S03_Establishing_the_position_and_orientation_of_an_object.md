---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_analysis_using_planar_views_of_an_object
section: Establishing_the_position_and_orientation_of_an_object
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_analysis_using_planar_views_of_an_object / Establishing the position and orientation of an object"
---

# Establishing the position and orientation of an object

If you are trying to manipulate an object that can move freely in a camera's field of view, you might need to know its pose (that is, its position and orientation) in the world. Using 3D-based camera calibration modes, you can use the Camera Calibration module to establish the pose of an object in the absolute coordinate system, if you have a 3D virtual model of the object, defined using CAD software, and you can find characteristics on the object for which you know their position relative to the origin of the 3D virtual model.

You can search for the unique features on the object and then pass the pixel position of these features and the position that they should have in the relative coordinate system to [`McalList`](../../Reference/cal/McalList.md) with [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalGrid.md); the relative coordinate system will move so that the pixel positions correspond to the correct coordinates in the relative coordinate system. Note that in this case, the absolute coordinate system will not be displaced.

Since you are specifying the pixel positions of the 3D virtual model's features, the relative coordinate system will be placed where the reference coordinate system of the 3D virtual model (0,0) would be with respect to the feature locations provided. You can then establish how that relative coordinate system is situated in the absolute coordinate system using [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md).

To establish the pose of an object, you must satisfy the following conditions:

- The camera calibration context must be allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md).
- The camera calibration context must be configured using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md).
- The object must have at least 4 features that can be identified and whose feature positions are known in a 3D virtual model (CAD) relative to the model's origin.

The following is an image depicting unique features (attributes) that you could use to establish the pose of your object.

*[Image: 3dmap_feature_block.png]*

The following images depict a 3D virtual model of a car door, an image of the door, and the resulting position of the relative coordinate system with respect to the absolute coordinate system.

*[Image: 3dmap_displace_relative_coord.png]*

## Using a grid

If a grid is provided on your object, you can call [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalGrid.md) and specify the grid's physical dimensions.[`McalGrid`](../../Reference/cal/McalGrid.md) will perform the feature extraction and displace the relative coordinate system to align with the top row and left most column of the found grid points (unless an offset has been specified). Note that the function searches for an exact match to the number of rows and columns that have been specified; therefore, if the grid is only partially visible, it will not be found. For more information, see [Calibrating using calibration points from a grid](../C28_Calibration/S10_Calibrating_using_calibration_points_from_a_grid.md).

## Obtaining results

After the relative coordinate system has been moved to the object's position at the correct orientation (as defined, for example, by the coordinate system of the 3D virtual model of the object), you can inquire the new location of the relative coordinate system relative to any other world coordinate system. To do so, use [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md) with the target coordinate system set to [`M_RELATIVE_COORDINATE_SYSTEM`](../../Reference/cal/McalGetCoordinateSystem.md). For more information, see [Inquiring about your camera calibration](../C28_Calibration/S13_Inquiring_about_your_calibration.md).
