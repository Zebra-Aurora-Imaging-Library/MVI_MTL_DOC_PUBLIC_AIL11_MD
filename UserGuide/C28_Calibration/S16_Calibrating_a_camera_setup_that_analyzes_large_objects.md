---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Calibrating_a_camera_setup_that_analyzes_large_objects
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Calibrating a camera setup that analyzes large objects"
---

# Calibrating a camera setup that analyzes large objects

The Camera Calibration module makes it possible to measure the length between various points on an object, even when the object is not entirely within the camera's field-of-view. The camera's field-of-view refers to the largest world region visible in an image at a given resolution. The approach to analyzing a large object involves acquiring images of the various parts of the object, getting real-world coordinates from each image, and then calculating the distance between the coordinates. Three types of applications are considered:

- A single camera is fixed on a manipulator and the manipulator is moved to different positions to acquire the different images.
- A single camera is fixed to a location and the object is moved to different positions to acquire the different images.
- Several cameras are used to acquire the different images.

## Single camera fixed on a movable tool (manipulator): tool coordinate system example

When a single camera is fixed on a manipulator, part of the object is grabbed, the camera is moved to a different position, a different part of the object is grabbed, and the process repeats until the entire object has been grabbed. For the coordinates from each image to be in the same absolute coordinate system, the tool coordinate system has to be updated each time the tool is moved and before associating the camera calibration context with each image. To change the tool coordinate system, use [`McalControl`](../../Reference/cal/McalControl.md) with [`M_TOOL_POSITION_X`](../../Reference/cal/McalControl.md), [`M_TOOL_POSITION_Y`](../../Reference/cal/McalControl.md), [`M_TOOL_POSITION_Z`](../../Reference/cal/McalControl.md), and [`M_TOOL_ROTATION_Z`](../../Reference/cal/McalControl.md) for camera calibration contexts using a two-dimensional camera calibration mode. For [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) and[`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration contexts, use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_TOOL_COORDINATE_SYSTEM`](../../Reference/cal/McalSetCoordinateSystem.md) as the target coordinate system.

*[Image: cal_singlecamera.png]*

## Single camera and movable object: relative coordinate system example

When a single camera is fixed to a location, part of the object is grabbed, the object is moved to a different position, another part of the object is grabbed, and the process repeats until the entire object has been grabbed. For the coordinates from each image to be in the same relative coordinate system, the relative coordinate system must be moved each time the object is moved and before associating the camera calibration context with each image. To move the relative coordinate system, use [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) for camera calibration contexts using a two-dimensional camera calibration mode. For [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) and [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration contexts, use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_RELATIVE_COORDINATE_SYSTEM`](../../Reference/cal/McalSetCoordinateSystem.md) as the target coordinate system.

*[Image: cal_singleview.png]*

## Several fixed cameras and fixed object: grid offset example

For the coordinates from each image to be in the same absolute coordinate system when several cameras are used, the camera calibration context used to calibrate each camera must use the same absolute coordinate system. When using [`McalGrid`](../../Reference/cal/McalGrid.md), you must use an offset to relate the origin of each grid to the same absolute coordinate system. When using [`McalList`](../../Reference/cal/McalList.md), all real-world coordinates used in each mapping must be coordinates from the same absolute coordinate system.

*[Image: cal_severalcameras.png]*
