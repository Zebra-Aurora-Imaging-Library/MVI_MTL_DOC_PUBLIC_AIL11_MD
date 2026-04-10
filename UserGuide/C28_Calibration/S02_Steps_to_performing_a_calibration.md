---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Steps_to_performing_a_calibration
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Steps to performing a calibration"
---

# Steps to performing a camera calibration

The following steps provide a basic methodology for a typical camera calibration:

1. Physically place the camera so that the working area is in the camera's field of view.
2. Allocate a camera calibration context, using [`McalAlloc`](../../Reference/cal/McalAlloc.md). The camera calibration context will store the non-linear mapping between the pixel coordinate system and the world coordinate system, as well as information about the calibration setup, and the positions of all the coordinate systems. When allocating the context, specify the camera calibration mode.
3. Calibrate your camera setup, using either [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md) to map image pixels to world points in the camera calibration context.
4. Associate the camera calibration context with an image or digitizer, using [`McalAssociate`](../../Reference/cal/McalAssociate.md), to enable working in real-world units. Once an image or digitizer is associated with a camera calibration context, it becomes known as a calibrated image or calibrated digitizer, respectively.
5. Physically correct an image using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md).
6. Save your camera calibration context using [`McalSave`](../../Reference/cal/McalSave.md) or [`McalStream`](../../Reference/cal/McalStream.md).
7. Free the camera calibration context using [`McalFree`](../../Reference/cal/McalFree.md), unless [`M_UNIQUE_ID`](../../Reference/cal/McalAlloc.md) was specified during allocation.

You can skip all of the steps above if non-linear distortion is not present and uniform camera calibration mode is sufficient; simply call [`McalUniform`](../../Reference/cal/McalUniform.md) to associate the default uniform camera calibration context with an image. Whereas, additional steps are required in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), and [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) mode. For more information on the robotics and Zhang-based camera calibration modes, see [3D camera calibration modes](S07_3D_Calibration_modes.md).

> **Note:** Note that typically, if you physically move your camera, you will have to recalibrate. However, if you are using a 3D-based camera calibration mode, you can remain calibrated in the following two cases: - If the movement is known, specify the transformation using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with the camera coordinate system as the target coordinate system.
> - If the movement is not known, you can have the new position calculated automatically using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md). Note that [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) is not supported for an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration.
