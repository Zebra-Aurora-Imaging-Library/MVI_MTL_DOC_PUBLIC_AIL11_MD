---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Inquiring_about_your_calibration
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Inquiring about your calibration"
---

# Inquiring about your camera calibration

After camera calibration, you can inquire about the status and accuracy of your calibration, as well as the position and orientation of your coordinate systems. You can also inquire the intrinsic and extrinsic attributes of the pinhole camera used to model your camera when working with 3D-based camera calibration modes.

## Camera calibration status and errors

After calibrating with [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md), you can check the success of your camera calibration using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_CALIBRATION_STATUS`](../../Reference/cal/McalInquire.md). If calibration was successful, it will return [`M_CALIBRATED`](../../Reference/cal/McalInquire.md), otherwise an error code is returned. For example, if you calibrate using [`McalGrid`](../../Reference/cal/McalGrid.md) and the grid does not meet the required guidelines, [`M_CALIBRATION_STATUS`](../../Reference/cal/McalInquire.md) returns [`M_GRID_NOT_FOUND`](../../Reference/cal/McalInquire.md). If you try to perform a full calibration in [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) mode, and the camera is placed perpendicular to the grid, [`M_PLANE_ANGLE_TOO_SMALL`](../../Reference/cal/McalInquire.md) is returned.

[`M_NOT_INITIALIZED`](../../Reference/cal/McalInquire.md) is returned if [`M_CALIBRATION_STATUS`](../../Reference/cal/McalInquire.md) is inquired before calling [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md).

## Camera calibration accuracy

After a successful camera calibration, you can inquire about the accuracy of your calibration. From the pixel coordinates and corresponding world coordinates of the calibration points, [`McalGrid`](../../Reference/cal/McalGrid.md) and [`McalList`](../../Reference/cal/McalList.md) establishes a mapping function that can transform any pair of pixel coordinates to their corresponding world coordinates, and vice versa. Depending on several factors, such as the calibration mode, noise in the image, and camera distortion, the mapping function might not be perfectly accurate. After calibration, this slight inaccuracy can even apply to the original calibration points. For instance, the original pixel coordinates of the calibration points, when transformed into world coordinates, might not equal the original world coordinates of the calibration points. The difference between the transformed coordinates and the original coordinates of the calibration points is called camera calibration error.

In the default [`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md) calibration mode, the mapping function is created such that the pixel coordinates, when transformed into world coordinates, are nearly identical to the original world coordinates, and vice versa, for all calibration points. This results in a camera calibration that is nearly perfectly accurate for the calibration points. When calibrating using a grid, this implies a very high accuracy in the area of the image with the grid, and a lower accuracy for the rest of the image. In every other calibration mode, the mapping function is created, such that it is accurate more evenly across the entire image. Effectively, this means that in [`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md) calibration mode, real-world measurements will be more accurate the closer they are to the area of the image that had the grid; while in all other calibration modes, any inaccuracy of real-world measurements will be felt equally throughout the image.

You can inquire about the camera calibration error using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_AVERAGE_PIXEL_ERROR`](../../Reference/cal/McalInquire.md), [`M_MAXIMUM_PIXEL_ERROR`](../../Reference/cal/McalInquire.md), [`M_AVERAGE_WORLD_ERROR`](../../Reference/cal/McalInquire.md), or [`M_MAXIMUM_WORLD_ERROR`](../../Reference/cal/McalInquire.md).

Note that for [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), and [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) camera calibration modes, you can use [`McalInquireSingle`](../../Reference/cal/McalInquireSingle.md) to inquire about the calibration accuracy of each call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) made with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md).

There are two main causes of calibration error: problems finding calibration points and problems creating the mapping function.

### Addressing calibration error due to problems finding calibration points

[`McalGrid`](../../Reference/cal/McalGrid.md) takes an image of a grid and determines the pixel coordinates of the calibration points using feature extraction algorithms. You can assess the accuracy of the feature extraction using [`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_IMAGE_POINTS`](../../Reference/cal/McalDraw.md), which draws the extracted calibration points. You can overlay the extracted calibration points on the original image of the grid to ensure that there are no false calibration points or missing calibration points. It is also important that the pixel coordinates of the calibration points are directly in the middle of each circle, for a circle grid, or at the intersection of the grid squares, for a chessboard grid or fiducial grid. Any extra points, missing points, or imprecisely positioned points will result in calibration error.

You can typically improve the accuracy of the found calibration points by physically cleaning your grid or cleaning up the image of the grid. Cleaning up the image of the grid can include adjusting the focus of the camera, the saturation of the image, or the lighting in the setup. The contrast between the foreground and the background of the grid must be sharp.

### Addressing calibration error due to problems creating the mapping function

Camera calibration creates a mapping function that can transform pixel coordinates to world coordinates, and vice versa. This mapping function makes certain assumptions, based on the calibration mode. Any violation of the assumptions can result in a mapping function that imprecisely maps pixel coordinates to world coordinates. For instance, camera calibrations calculated in[`M_PERSPECTIVE_TRANSFORMATION`](../../Reference/cal/McalAlloc.md) mode cannot be used with cameras that have lens distortion. If you calibrated your camera setup in this mode and your camera has lens distortion, there would likely be mapping problems.

Often the violation of assumptions is a matter of degree. For instance, most cameras have at least a small amount of lens distortion, especially around the edge of the lens. This means that most calibrations calculated using [`M_PERSPECTIVE_TRANSFORMATION`](../../Reference/cal/McalAlloc.md) mode (which doesn't compensate for lens distortion) will have some degree of calibration error.

You can determine if the error is within tolerance using [`McalInquire`](../../Reference/cal/McalInquire.md). There are two types of calibration error measured: pixel error ([`M_AVERAGE_PIXEL_ERROR`](../../Reference/cal/McalInquire.md) or [`M_MAXIMUM_PIXEL_ERROR`](../../Reference/cal/McalInquire.md)) and world error ([`M_AVERAGE_WORLD_ERROR`](../../Reference/cal/McalInquire.md) and [`M_MAXIMUM_WORLD_ERROR`](../../Reference/cal/McalInquire.md)). The pixel error is the pixel difference between the original world coordinates of the calibration points transformed into pixel coordinates, and the original pixel coordinates of the calibration points. The world error is the world difference between the original pixel coordinates of the calibration points transformed into world coordinates, and the original world coordinates of the calibration points. In both cases, the calibration error is a measure of the quality of the mapping function as applied to the calibration points.

You can visually inspect the location and severity of the calibration error using [`McalDraw`](../../Reference/cal/McalDraw.md). You can draw the original pixel coordinates of the calibration points using [`M_DRAW_IMAGE_POINTS`](../../Reference/cal/McalDraw.md), and you can draw the original world coordinates of the calibration points, transformed into the pixel coordinates with the mapping function, using [`M_DRAW_WORLD_POINTS`](../../Reference/cal/McalDraw.md). By drawing the original and transformed coordinates together, you can see where the points differ. This difference is calibration error. Alternatively, you could draw just the original world coordinates (transformed into pixel coordinates with the mapping function), directly on the image of the original grid, as in the image below. This way you can see exactly where on the grid are the biggest differences. With this information, you can adjust the setup as needed.

*[Image: cal_draw_accuracy.png]*

To improve the accuracy of your calibration (reduce the calibration error caused by mapping problems), you can specify a calibration mode that better represents the assumptions of your camera setup. Generally speaking, you can reduce camera calibration error by calibrating again using a new camera calibration context that encompasses more assumptions. If your first calibration mode is [`M_UNIFORM_TRANSFORMATION`](../../Reference/cal/McalAlloc.md) and it produces a calibration with more calibration error than you can tolerate, try using a new calibration context in [`M_PERSPECTIVE_TRANSFORMATION`](../../Reference/cal/McalAlloc.md) mode. If this still produces a calibration with more error than is tolerable, try using a new calibration context in [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) mode. Note that [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) mode supports 3D calibration and has additional setup considerations.

In the default calibration mode, [`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md) mode, the mapping function is created so that comparing transformed coordinates (either pixel-to-world or world-to-pixel) to the original coordinates of the calibration points results in almost no measured calibration error. This produces increased calibration accuracy for areas of the image inside the grid and reduced accuracy for areas outside the grid. Because calibration error is measured using calibration points, there is no way to measure the inaccuracy of the areas outside of the grid. Also, if the pixel coordinates or world coordinates of any calibration point are incorrectly specified, the mapping will be significantly inaccurate in the area around the incorrectly specified calibration point. This inaccuracy also cannot measured.

For information on each calibration mode, including a list of supported features, see [Camera calibration modes](S05_Calibration_modes_overview.md).

Another way to improve the accuracy of the mapping function is to increase the number of calibration points used to calibrate your camera setup. When calibrating using a grid, this means you could use a grid with more columns or rows, possibly with smaller spacings between calibration points. The number of calibration points and the size of the spacings has limitations, which are discussed in [Calibrating using calibration points from a grid](S10_Calibrating_using_calibration_points_from_a_grid.md). To maximize grid coverage in your image, you can use a chessboard or fiducial grid that extends outside the image borders, using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRID_PARTIAL`](../../Reference/cal/McalControl.md)set to [`M_ENABLE`](../../Reference/cal/McalControl.md).

### Inquiring pixel and world calibration points

You can inquire the pixel coordinates and world coordinates of the calibration points that were used by [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) to create the camera calibration.

If you have calibrated using [`McalGrid`](../../Reference/cal/McalGrid.md), the pixel coordinates of the calibration points are those automatically extracted from the image of the grid, and the world coordinates of the calibration points are those calculated from the world description of the grid (number and spacings of the rows and columns of the grid). If you have calibrated using[`McalList`](../../Reference/cal/McalList.md), you have explicitly specified the pixel coordinates and world coordinates of the calibration points.

You can inquire the pixel coordinates of the calibration points, using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_CALIBRATION_IMAGE_POINTS_X`](../../Reference/cal/McalInquire.md) and [`M_CALIBRATION_IMAGE_POINTS_Y`](../../Reference/cal/McalInquire.md). When inquiring the pixel coordinates of calibration points from a grid, the coordinates correspond to the extracted center of the circles in a circle grid or to the extracted intersections of four corners in a chessboard grid or fiducial grid.

You can inquire the world coordinates of the calibration points, using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_CALIBRATION_WORLD_POINTS_X`](../../Reference/cal/McalInquire.md) and [`M_CALIBRATION_WORLD_POINTS_Y`](../../Reference/cal/McalInquire.md).

You can use these coordinates to independently calculate calibration error. To calculate the calibration error in world units, perform the following:

1. Inquire the original pixel coordinates of the calibration points using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_CALIBRATION_IMAGE_POINTS_X`](../../Reference/cal/McalInquire.md) and [`M_CALIBRATION_IMAGE_POINTS_Y`](../../Reference/cal/McalInquire.md).
2. Transform the original pixel coordinates to world coordinates using [`McalTransformCoordinate`](../../Reference/cal/McalTransformCoordinate.md) or [`McalTransformCoordinateList`](../../Reference/cal/McalTransformCoordinateList.md) with [`M_PIXEL_TO_WORLD`](../../Reference/cal/McalTransformCoordinate.md).
3. Inquire the original world coordinates of the calibration points using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_CALIBRATION_WORLD_POINTS_X`](../../Reference/cal/McalInquire.md) and [`M_CALIBRATION_WORLD_POINTS_Y`](../../Reference/cal/McalInquire.md).
4. Compare the transformed world coordinates to the original world coordinates to calculate your own world calibration error metrics. Calibrations in [`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md) mode should have two nearly identical sets of coordinates.

> **Note:** Note that to calculate the calibration error in pixel units, you must perform the same steps but in a different order. First you must inquire the original world coordinates, then transform them to pixel coordinates with[`M_WORLD_TO_PIXEL`](../../Reference/cal/McalTransformCoordinate.md), then inquire the original pixel coordinates, and finally compare the transformed pixel coordinates with the original pixel coordinates to calculate the pixel calibration error.

## Position and orientation of coordinate systems

You might need to inquire about the position and orientation of one coordinate system with respect to another. You can do so using [`McalInquire`](../../Reference/cal/McalInquire.md) for two-dimensional camera calibration modes or [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md) for three-dimensional camera calibration modes.

For two dimensional camera calibration modes, you can inquire about the position and orientation of the relative coordinate system and the position of the tool coordinate system:

- To retrieve the position of the relative coordinate system, use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_RELATIVE_ORIGIN_X`](../../Reference/cal/McalInquire.md), [`M_RELATIVE_ORIGIN_Y`](../../Reference/cal/McalInquire.md), and [`M_RELATIVE_ORIGIN_Z`](../../Reference/cal/McalInquire.md). To inquire about the rotation of the relative coordinate system, use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_RELATIVE_ORIGIN_ANGLE`](../../Reference/cal/McalInquire.md); this will return the counter-clockwise angle of rotation about the Z-axis as measured with respect to the absolute coordinate system.
- To retrieve the position of the tool coordinate system, use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_TOOL_POSITION_X`](../../Reference/cal/McalInquire.md), [`M_TOOL_POSITION_Y`](../../Reference/cal/McalInquire.md), [`M_TOOL_POSITION_Z`](../../Reference/cal/McalInquire.md), and [`M_TOOL_ROTATION_Z`](../../Reference/cal/McalControl.md).

For 3D-based camera calibration modes, use [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md) to inquire about the position and orientation of the absolute, relative, tool, or camera coordinate systems. For a robotics camera calibration mode, you can also inquire about the position and orientation of the robot base coordinate system.

[`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md) can return the relationship between these coordinate systems in three-dimensional space, in terms of a specified type of transformation. For example, after a successful Tsai-based camera calibration, you might need to know the calculated position and orientation of your camera with respect to the absolute coordinate system. To obtain this information as a translation and rotation of the camera coordinate system from the absolute coordinate system, you can use [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md) with [`M_CAMERA_COORDINATE_SYSTEM`](../../Reference/cal/McalGetCoordinateSystem.md) as the target coordinate system and [`M_ABSOLUTE_COORDINATE_SYSTEM`](../../Reference/cal/McalGetCoordinateSystem.md) as the reference coordinate system. The following code snippet shows how to retrieve this information using two consecutive calls to [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md). The first call returns the X-, Y-, and Z-coordinates of the camera coordinate system's origin, and the second call returns the three rotation angles of the camera coordinate system's axes around the X-, Y-, and Z-axes of the absolute coordinate system.

> **Code example:** [userguide.calibration.getting_camera_position_and_orientation](userguide.calibration.getting_camera_position_and_orientation)

For a robotics camera calibration mode, you can retrieve the position and orientation of any coordinate system with respect to the robot base coordinate system using [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md). The values returned can then be provided to a robot controller software to move the robot.

> **Note:** Note that the pixel coordinate system cannot be used with [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md). Refer to [Working with real-world units](S09_Working_with_realworld_units.md) for information on transforming values from world to pixel units.

Refer to [Moving coordinate systems to reflect camera setup changes](S15_Moving_coordinate_systems_to_reflect_camera_setup_changes.md) for more information on coordinate system transformations.

## Pinhole camera

You can retrieve the values of several intrinsic and extrinsic attributes of the pinhole camera used to model your camera when working in Tsai-based, Zhang-based, or a robotics camera calibration mode:

To retrieve the calculated effective focal length of the modeled pinhole camera, use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_FOCAL_LENGTH`](../../Reference/cal/McalInquire.md). The returned value is expressed in horizontal pixel units. You can use the following relation to convert the focal length from pixel units into real-world units:

*[Image: cal_focal_length_convert.png]*

Where *[Image: cal_delta_x.png]*

 specifies the distance between the centers of two horizontally adjacent CCD elements of your camera. Note that this is the focal length of the modeled pinhole camera; it will not necessarily match the focal length given by the lens manufacturer. You can override the value determined by the previous call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) if you want to use calibration values from another source (for example, the lens manufacturer) using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_FOCAL_LENGTH`](../../Reference/cal/McalControl.md).

To locate the camera's position in space, use [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md) with [`M_CAMERA_COORDINATE_SYSTEM`](../../Reference/cal/McalGetCoordinateSystem.md) as a target coordinate system.

### Radial distortion

To retrieve the second and fourth order radial distortion coefficients, use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DISTORTION_RADIAL_1`](../../Reference/cal/McalInquire.md) and [`M_DISTORTION_RADIAL_2`](../../Reference/cal/McalInquire.md), respectively. Radial distortion refers to image distortion caused by the camera's lens, namely, pin cushion and barrel distortions. It is calculated by measuring the distance from the principal point to any other point in the image plane.

> **Note:** Note that [`M_DISTORTION_RADIAL_2`](../../Reference/cal/McalInquire.md) is only available for Zhang-based and Zhang-based robotics camera calibration.

Let *[Image: cal_distorted_pixel_coordinates.png]*

 represent the distorted (acquired) pixel coordinates.

Coordinates are normalized and centered using *[Image: cal_focal_length_pixel_units.png]*

and the principal point *[Image: cal_principal_point.png]*

:

*[Image: cal_distorted_coordinates_normalized_centered.png]*

There are two distortion models that are used to calculate the radial distortion coefficients, where the distortion models are inverses of each other. This means that conversions between world points and pixels can be faster depending on the distortion model, but more specifically, the camera calibration mode being used.

The distortion model for [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) camera calibration is defined by the following equations:

*[Image: cal_undistorted_coordinates_calculations.png]*

Where*[Image: cal_kappa_one.png]*

 represent the second order radial distortion coefficient, and*[Image: cal_undistorted_pixel_coordinates.png]*

 represent the corrected, undistorted coordinates.

The distortion model for [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration is defined by the following equations:

*[Image: cal_undistorted_coordinates_calculations_two.png]*

Where*[Image: cal_kappa_one.png]*

 represents the second order radial distortion coefficient, *[Image: cal_kappa_two.png]*

 represents the fourth order radial distortion coefficient, and *[Image: cal_undistorted_pixel_coordinates.png]*

 represents the corrected, undistorted coordinates.

> **Note:** The fourth order radial distortion coefficient further increases the accuracy of the distortion model, which is used in calculations to remove lens distortion.
