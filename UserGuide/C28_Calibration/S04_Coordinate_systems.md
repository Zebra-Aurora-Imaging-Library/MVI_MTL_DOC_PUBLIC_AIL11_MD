---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Coordinate_systems
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Coordinate systems"
---

# Coordinate systems

Aurora Imaging Library supports different coordinate systems to describe positions in your camera setup. The following diagram shows these coordinate systems using a sample camera setup. Notice that the tool's position is assigned to the last joint of the robot arm. The Z-axis for these 3D coordinate systems follow the right-hand rule; with the thumb, index, and middle fingers of your right hand at right angles to each other, the middle finger points in the direction of the positive Z-axis when the thumb represents positive X-axis and the index finger represents positive Y-axis.

*[Image: UR3_0.png]*

| Legend |
| --- |
| A | Origin of the absolute coordinate system | (0, 0, 0) |
| B | Camera field of view from which image is captured | (180, 130, 0) |
| C | Origin of the relative coordinate system | (190, 145, 0) |
| D | Origin of the tool coordinate system | (190, 155, -75) |
| E | Origin of the camera coordinate system [^val] | (190, 155, -60) |
| F | Origin of robot base coordinate system | (160, 360, -20) |

[^val]: In the diagram above the camera is represented as an integrated wrist camera that moves with the robot arm.

> **Note:** Note that Aurora Imaging Library also supports alternative camera setups where the camera is not moving with the robot arm, and is fixed in a position where the camera's field of view is in range with the arm.

## Absolute world coordinate system

The absolute world coordinate system is implicitly defined from the calibration points when calibrating the camera setup. The absolute world coordinate system is unmovable. Its unit of measure is user-defined (for example, mm, cm, or inches). Calibration relates the pixel coordinate system to the absolute world coordinate system.

For the sake of simplicity, the absolute world coordinate system will be known as the absolute coordinate system.

## Relative world coordinate system

The relative world coordinate system is the coordinate system used to locate and/or measure objects in the real world. By default, after calibration, positional results will be returned with respect to the relative world coordinate system, rather than the pixel coordinate system. Initially, the relative world coordinate system has the same position and orientation as the absolute coordinate system. However, to get results relative to some object, the relative coordinate system can be moved anywhere within the absolute coordinate system and rotated by any angle. Its unit of measure is the same as the absolute coordinate system. For the sake of simplicity, the relative world coordinate system will be known as the relative coordinate system.

*[Image: cal_relative_coordinate_system.png]*

## Tool coordinate system

For most applications, the tool coordinate system can be ignored unless, for example, analyzing an object that cannot fit in a single image or working with 3D camera calibration modes. As such, by default, the tool coordinate system has the same position and orientation as the absolute coordinate system. Typically, moving the origin of the tool coordinate system is an indirect means of positioning the camera coordinate system. Since moving the camera coordinate system implies a different field of view, moving the origin of the tool coordinate system affects positional results taken from a calibrated image. The world coordinates of an image pixel taken before and after moving the tool coordinate system are not the same.

*[Image: cal_relcamera.png]*

For information about adjusting the tool position when analyzing an object that cannot fit in a single image, see [Calibrating a camera setup that analyzes large objects](S16_Calibrating_a_camera_setup_that_analyzes_large_objects.md).

In 3D robotics, the tool coordinate system relates to the center-point of the tool that is located on the last joint of a robot arm. Depending on your type of camera setup, the robot arm also has either a camera or reference object/calibration grid mounted on the last joint of the robot arm. Moving the origin of the tool coordinate system is an indirect means of positioning the camera coordinate system, unless it is a stationary camera robotics setup where the camera coordinate system must remain fixed; for more information on robotics mode calibration, see [Robotics mode](S07_3D_Calibration_modes.md).

## Camera coordinate system

The camera coordinate system is the coordinate system with an origin positioned at the center of the camera's lens and is only useful for 3D camera calibration modes. Its X-axis is oriented towards the right of the image, its Y-axis is oriented downwards in the image, and its Z-axis is oriented in the direction the camera is pointing (into the image). The 2D camera calibration modes do not estimate the position of the camera coordinate system; the movement of the camera coordinate system is accomplished by moving the tool coordinate system. The 3D camera calibration modes estimate the orientation and distance between the camera and the camera calibration plane as part of their algorithmic calculations; so when using these modes, the camera position is the position of the modeled pinhole camera calculated using Tsai-based or Zhang-based algorithms.

When dealing with a robotics setup where your camera is not stationary, moving the camera coordinate system is usually performed by moving the tool coordinate system. See [Moving coordinate systems to reflect camera setup changes](S15_Moving_coordinate_systems_to_reflect_camera_setup_changes.md) for more information on moving your camera. Moving the origin of the camera coordinate system affects positional results taken from a calibrated image. The world coordinates of an image pixel taken before and after moving the camera coordinate system are not the same.

## Robot base coordinate system

The robot base coordinate system is the coordinate system with an origin positioned at the base of the robot arm. It is used by the robot software to set the tool coordinate system with respect to the robot's base and can be used to provide the Camera Calibration module with values returned by the robot encoder. This coordinate system is only available for robotics camera calibration modes; in these modes, you must define the relationship between the tool coordinate system and the robot base coordinate system before performing camera calibration. Even if its position and orientation with respect to the absolute coordinate system is originally unknown, the tool coordinate system can still be defined with respect to the robot base coordinate system. After performing full calibration, the relation between the robot base coordinate system and absolute coordinate system is established, as well as the relation between tool and absolute coordinate systems.

> **Note:** Note that usually, you cannot move the robot base coordinate system unless the robotic encoders are set on a mobile robot base.

## Pixel coordinate system

The pixel coordinate system is the coordinate system used to locate and/or measure objects in a non-calibrated image. Its unit of measure is pixels. Its origin, (0, 0), is the center of the image's top-left pixel. Its X-axis follows the first row of pixels, pointing to the right and the Y-axis follows the first column of pixels downwards. For more information, see [Pixel conventions and subpixel accuracy](../C23_Data_buffers/S15_Pixel_conventions.md).
