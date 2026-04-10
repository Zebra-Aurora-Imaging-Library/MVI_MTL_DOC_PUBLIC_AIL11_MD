---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: 3D_Calibration_modes
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / 3D Calibration modes"
---

# 3D camera calibration modes

Aurora Imaging Library's Camera Calibration module ([`Mcal...`](../../Reference/cal/McalAlloc.md)) allows you to use true 3D camera calibration techniques to map pixel coordinates to real-world coordinates to determine the distance and orientation between the modeled camera and the image plane. This allows for results to be inquired with respect to any defined plane, which can be useful in both robotic and non-robotic applications. For more information on inquiring results with respect to any defined plane, see [Inquiring pixel and world calibration points](S13_Inquiring_about_your_calibration.md).

## Tsai-based mode

Tsai-based mode is a camera calibration mode based on Roger Y. Tsai's algorithm. This mode models your camera as a pinhole camera and can compensate for rotation, translation, scale, perspective, and radial lens distortions.

This mode allows for camera rotation about any axis without re-calibrating; you only need to specify the camera movement which has occurred. The mode requires a minimum of 6 points to perform a successful calibration and accuracy increases with the number of points. To allocate a Tsai-based camera calibration context, use [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md).

- Calibrates with 1 grid image or list of calibration points. That is, you calibrate with one call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md).
- The single grid image or list of calibration points must provide the best possible FOV coverage.
- Requires that the optical axis of the camera be at least 30º away from the axis perpendicular to the camera calibration plane.

### Intrinsic and extrinsic attributes

Tsai-based mode calculates several intrinsic and extrinsic attributes of the pinhole camera that is used to model the real-world camera. Intrinsic attributes characterize properties of the camera itself (such as the focal length and the radial distortion), whereas extrinsic attributes characterize the camera's position and orientation.

One radial distortion coefficient is used to correct camera distortion ([`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DISTORTION_RADIAL_1`](../../Reference/cal/McalInquire.md)). For more information on the distortion model used for the Tsai-based calibration mode, see [Radial distortion](S13_Inquiring_about_your_calibration.md).

Tsai-based mode does not estimate the following two intrinsic attributes that are needed for accurate calibration; you can adjust their values set by default if not correct:

- **Aspect ratio** (of the camera's charge-coupled device (CCD) elements). This refers to the ratio of the horizontal dimension of a single CCD element to its vertical dimension. Tsai-based mode sets this ratio to 1 by default. If the aspect ratio given by the camera manufacturer is not 1, specify its actual aspect ratio using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_CCD_ASPECT_RATIO`](../../Reference/cal/McalControl.md).
  *[Image: cal_CCD.png]*
- **Principal point**. This refers to the point of intersection of the camera's optical axis with the image plane. You can specify the principal point using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_PRINCIPAL_POINT_X`](../../Reference/cal/McalControl.md) and [`M_PRINCIPAL_POINT_Y`](../../Reference/cal/McalControl.md), prior to calibrating. If [`McalGrid`](../../Reference/cal/McalGrid.md) is used for calibration, the principal point will default to the central point of the image plane. If [`McalList`](../../Reference/cal/McalList.md) is used for calibration, it is mandatory to set the principal point prior to calibrating. In this case, it is recommended to set the principal point to the central pixel of the captured image.

### Calibrating with Tsai-based mode

To successfully perform a full calibration using Tsai-based mode, you must ensure that the optical axis of your camera is at least 30º away from the axis perpendicular to the camera calibration plane. This angle provides the image perspective needed by Tsai-based mode to estimate some intrinsic and extrinsic attributes.

For camera setups that require a camera whose optical axis is perpendicular to the camera calibration plane, you must perform two calibrations: the first calibration must be performed with the camera inclined with respect to the camera calibration plane, using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md); the second calibration must be performed after repositioning the camera, such that its optical axis is perpendicular to the camera calibration plane, using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md). Instead of calibrating a second time, you can, alternatively, move the camera coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) if the movement is known with great precision. If you want to move the camera coordinate system by moving the tool coordinate system, you must first move the tool coordinate system as described in [Special considerations concerning the tool and camera coordinate systems](S15_Moving_coordinate_systems_to_reflect_camera_setup_changes.md).

This mode requires that the image has perspective distortion. For this reason, you should not use this mode when using a camera with a telecentric lens; telecentric lenses negate perspective effects.

> **Note:** Note that if you are using a robotics setup and need to transform positional results such that they are in the robot base coordinate system, you should calibrate using robotics mode. Alternatively, if you have already calibrated in Tsai-based mode, you can use[`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) to calculate the transformation required.

## Zhang-based mode

Zhang-based mode is a camera calibration mode based on Z. Zhang's algorithm. This mode models your camera as a pinhole camera and can compensate for rotation, translation, scale, perspective, and radial lens distortions.

This mode allows for camera rotation about any axis without re-calibrating; you only need to specify the camera movement which has occurred. The mode requires a minimum of 8 points to perform a successful calibration and accuracy increases with the number of points. To allocate a Zhang-based camera calibration context, use [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md).

- Calibrates with a minimum of 3 grid images or lists of calibration points at different points of view. That is, you calibrate with multiple calls to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md).
- The accumulation of the different grid images or list of calibration points together provide the total FOV coverage. This means that the calibration data used to create your 3D camera model is not limited to a single view and the requirements for a large calibration grid are not as critical.
- No restrictions on the angle of the optical axis of the camera relative to calibration plane for a single point of view (that is, it can be perfectly perpendicular). However, the accumulation of the different grid images or list of calibration points must include calibration data at different depths.

### Intrinsic and extrinsic attributes

Zhang-based mode calculates several intrinsic and extrinsic attributes of the pinhole camera that is used to model the real-world camera. Intrinsic attributes characterize properties of the camera itself (such as the focal length and the radial distortion), whereas extrinsic attributes characterize the camera's position and orientation.

Two radial distortion coefficients are used to correct camera distortion ( [`McalInquire`](../../Reference/cal/McalInquire.md) with[`M_DISTORTION_RADIAL_1`](../../Reference/cal/McalInquire.md) and [`M_DISTORTION_RADIAL_2`](../../Reference/cal/McalInquire.md)). For more information on the distortion model used for the Zhang-based calibration mode see [Radial distortion](S13_Inquiring_about_your_calibration.md).

Zhang-based mode does not estimate the following intrinsic attribute needed for accurate calibration; you can adjust its value set by default if not correct:

- **Aspect ratio** (of the camera's charge-coupled device (CCD) elements). This refers to the ratio of the horizontal dimension of a single CCD element to its vertical dimension. Zhang-based mode sets this ratio to 1 by default. If the aspect ratio given by the camera manufacturer is not 1, specify its actual aspect ratio using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_CCD_ASPECT_RATIO`](../../Reference/cal/McalControl.md).

Zhang-based mode will estimate the following intrinsic attribute needed for accurate calibration:

- **Principal point** (the point of intersection of the camera's optical axis with the image plane). The principal point can be inquired using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_PRINCIPAL_POINT_X`](../../Reference/cal/McalControl.md) and [`M_PRINCIPAL_POINT_Y`](../../Reference/cal/McalControl.md).

### Calibrating with Zhang-based mode

To successfully perform a full calibration using the Zhang-based mode, make at least three calls to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalList.md) before performing a full calibration with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md).

Calling [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) more than three times can greatly improve the accuracy of the camera calibration.

Before each call, move the grid or camera such that the grid is not viewed on the same plane by the camera as a previous call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalList.md). It will automatically establish the relative location of each grid image (unlike [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) mode). It is recommended to accumulate calibration data at various perceived depths, and at the edges of the working area of your application to accurately estimate the focal length and the radial distortion coefficients. The pose provided in the last call to [`M_ACCUMULATE`](../../Reference/cal/McalList.md) will be used to automatically set the position of the absolute and camera coordinate systems.

This mode requires that the image has perspective distortion. For this reason, you should not use this mode when using a camera with a telecentric lens; telecentric lenses negate perspective effects.

> **Note:** Note that if you are using a robotics setup and need to transform positional results such that they are in the robot base coordinate system, you should calibrate using robotics mode. Alternatively, if you have already calibrated in Zhang-based mode, you can use[`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) to calculate the transformation required.

### Zhang-based mode example

The example _2dcamerato3dcameramapping.cpp_ demonstrates in part how to calibrate a camera using the Zhang-based mode and calls [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) twelve times before performing a full calibration.

*[Image: zhang_example.png]*

To run/view this and other examples, use Aurora Imaging Example Launcher.

## Robotics mode

Robotics mode is a true 3D camera calibration technique that supports the robot base coordinate system. In addition to estimating the camera's intrinsic and extrinsic attributes, this mode also estimates the position and orientation of the robot base coordinate system with respect to the absolute coordinate system. This mode can use either the Tsai-based camera model or the Zhang-based camera model. When using the Tsai-based camera model in robotics mode, all calibration values are determined together. When using the Zhang-based camera model in robotics mode, the camera's intrinsic attributes are determined separately.

Like the other 3D camera calibration modes, robotics mode models the camera as an ideal pinhole camera. Depending on the camera model, the robotics mode estimates the same camera attributes estimated by Tsai-based mode or Zhang-based mode. It also allows for results to be inquired with respect to any defined plane. After 3D camera calibration, it is possible to move the relative coordinate system to another plane without having to calibrate again. For an example of how to move the relative coordinate system, see [Moving the relative coordinate system to account for height](../C34_3D_analysis_using_planar_views_of_an_object/S02_Moving_the_relative_coordinate_system_to_account_for_height.md).

The robotics mode is comprised of two distinct types of camera setups:

- [`M_MOVING_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) (also known as **eye-in-hand calibration**).
- [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) (also known as **eye-to-hand calibration**).

### Moving camera robotics mode

This calibration mode is used for camera setups where the camera is mounted on the last joint of a robot arm, while the calibration grid is stationary and in the camera's field of view. In this mode, a rigid link exists between the tool coordinate system and the camera coordinate system by default. You can disable the link between the coordinate systems by setting [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md) to [`M_DISABLE`](../../Reference/cal/McalControl.md) in[`McalControl`](../../Reference/cal/McalControl.md). The image below provides a visualization of what a typical [`M_MOVING_CAMERA`](../../Reference/cal/McalAlloc.md) setup looks like.

*[Image: Eye_in_hand_calibration.png]*

### Stationary camera robotics mode

The second robotics calibration mode is an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or **eye-to-hand calibration**. This calibration mode is used for a camera setup where the camera is stationary and the robot arm, with a calibration grid mounted on the last joint, is moving. By default, this calibration mode does not have a rigid link between the camera coordinate system and the tool coordinate system.[`McalControl`](../../Reference/cal/McalControl.md) with [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md) cannot be set to [`M_ENABLE`](../../Reference/cal/McalControl.md) because moving the tool coordinate system should not affect the camera coordinate system in this camera setup. The image below provides a visualization of what a typical [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) setup looks like. Note that the placement of the stationary camera is not required to be overhead as depicted in the image.

*[Image: Hand_on_eye_calibration.png]*

> **Note:** Note that in both [`McalGrid`](../../Reference/cal/McalGrid.md) and [`McalList`](../../Reference/cal/McalList.md),[`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) is not supported for an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) camera calibration.

### Steps to perform camera calibration in a 3D robotics mode

The following steps provide a basic methodology for performing camera calibration in a 3D robotics mode:

1. Allocate a camera calibration context in a robotics mode, use [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) and set the [`ModeFlag`](../../Reference/cal/McalAlloc.md) parameter to specify your camera setup ([`M_MOVING_CAMERA`](../../Reference/cal/McalAlloc.md) or [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md)).
2. Get the position and orientation of the tool holding the camera or grid, with respect to the robot base, using the robot software.
3. Set the position and orientation for the tool coordinate system in the camera calibration context with respect to the robot base coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md).
4. Call [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) to store calibration points in the camera calibration context.
5. Physically move the camera to a new position over the working area. You must rotate the tool along at least two non-parallel axes.
   You must also move the grid or camera such that the grid is not viewed at the same plane by the camera as a previous call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalList.md). It is recommended to accumulate calibration data at various perceived depths, and at the edges of the working area of your application to accurately estimate the focal length and the radial distortion coefficients.
6. Repeat steps 2 to 5 at least twice, to make a minimum total of three calls to [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md). Calling this function with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) more than 3 times improves the accuracy of the camera calibration.
7. When you are done accumulating data, you must call [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) and no image/calibration points to perform the full camera calibration. Once you perform a full camera calibration, you can no longer accumulate camera calibration poses.

> **Note:** Note that, you cannot set [`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md) to [`M_ABSOLUTE_COORDINATE_SYSTEM`](../../Reference/cal/McalControl.md) for an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context.

Once your 3D-based camera calibration context is calibrated, you can move your camera to a new location and automatically reposition the camera coordinate system, assuming that you are not calibrating an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md). To automatically reposition the coordinate system, you can use [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md). This determines and displaces the camera coordinate system to its new location. Note that besides the camera coordinate system, this also displaces the tool coordinate system (if it is not an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)); no other coordinate system is affected.

[`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) calibration modes are more efficient than calibrating with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) mode and then using the results of [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) to transform coordinates between the robot base and the absolute coordinate system.
