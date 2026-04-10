---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_analysis_using_planar_views_of_an_object
section: Moving_the_relative_coordinate_system_to_account_for_height
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_analysis_using_planar_views_of_an_object / Moving the relative coordinate system to account for height"
---

# Moving the relative coordinate system to account for height

When using 3D camera calibration modes, the Z-coordinate of the relative and absolute coordinate system can have a physical meaning in terms of a height, even if all points in an image are assumed to be on the same plane (even if that plane is slanted) and all results (except those from the Camera calibration module and the 3D reconstruction module) are returned to a 2D coordinate system. For example, the area of an object in real-world units will be different if the relative coordinate system is moved to a different height. Consequently, it is important to consider where the absolute coordinate system is placed and how measurements are taken.

## Measuring two features displaced by a known height

All results are returned in the relative coordinate system, so you must move it to the location where you need to measure. However, two objects can be located in different planes and suffer from perspective distortion. Although the camera calibration context will account for two dimensional perspective distortion in a single plane, it cannot account for a difference in height as well. For example, two identical rings sitting on boxes of different heights will appear to have a different size and perhaps shape from the camera's view point. To ensure that measurements are comparable, the relative coordinate system can be moved to the correct plane before a measurement is taken for each object.

To displace the relative coordinate system, perform the following:

1. Determine the location of the object to be measured.
2. Displace the relative coordinate system to its corresponding plane using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md). For information, see [Moving coordinate systems to reflect camera setup changes](../C28_Calibration/S15_Moving_coordinate_systems_to_reflect_camera_setup_changes.md).
3. Measure the feature(s) of interest.
4. Repeat steps 1 through 3 for each object to be measured that is on a different plane.

The following images depict two rings sitting on boxes of different height, from the camera and side view respectively.

*[Image: 3dmap_displace_height.png]*

Note that if your object is located on a plane that is on a slant, the relative coordinate system must be positioned on that slant.

## Accounting for thickness of camera calibration grids in 3D setups

Even when using a 3D-based camera calibration context, you can use grids to provide calibration points. However, the grid used in the camera calibration has a certain thickness. Depending on your application, you should correct for this thickness and ensure that the XY (Z=0) plane of the absolute coordinate system is well placed. This is important if you are using the camera calibration context with a 3D reconstruction setup where measurements in height will be taken.

To account for the thickness of the camera calibration grid, specify the height of the grid using [`McalGrid`](../../Reference/cal/McalGrid.md) with [`GridOffsetZ`](../../Reference/cal/McalGrid.md).
