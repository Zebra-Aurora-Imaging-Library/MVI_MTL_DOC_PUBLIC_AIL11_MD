---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: 2D_Calibration_modes
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / 2D Calibration modes"
---

# Uniform camera calibration and other 2D camera calibration modes

Aurora Imaging Library's Camera Calibration module ([`Mcal...`](../../Reference/cal/McalAlloc.md)) allows you to map pixel coordinates to real-world coordinates. The following modes range from the absolute basics of camera calibration (uniform mode) to the more advanced 2D camera calibration modes that can compensate for different types of distortion.

## Uniform mode

The most basic camera calibration mode available is the uniform mode. This simple, uniform camera calibration can only map a linear translation, rotation, and scaling between the pixel and world coordinate systems.

For a uniform camera calibration, you do not need to allocate a camera calibration context or call [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md), making it the fastest to set up. To provide uniform camera calibration information to an image, call [`McalUniform`](../../Reference/cal/McalUniform.md) and specify the scale, rotation, and offset between pixel and world coordinate systems. The following illustrations show the position of the pixel and absolute coordinate systems after three example uniform camera calibrations.

*[Image: cal_uniform_calibration.png]*

You can also allocate a camera calibration context using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_UNIFORM_TRANSFORMATION`](../../Reference/cal/McalAlloc.md). The context can be calibrated by calling[`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md), which will determine the best linear mapping between the pixel and world coordinate system.

If you pass a uniformly calibrated image to [`McalTransformImage`](../../Reference/cal/McalTransformImage.md), the image will be rotated to remove the angle between the relative and pixel coordinate systems, and the scales will be adjusted to produce square pixels.

For depth maps, a uniform camera calibration can include the mapping of a third dimension. You can call [`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md) to specify how the gray level of each pixel maps to world coordinates, and with [`M_WORLD_POS_Z`](../../Reference/cal/McalControl.md) to specify what gray level corresponds to a height of 0 in world units.

## Piecewise linear interpolation mode

Piecewise linear interpolation mode can compensate for any kind of distortion. In general, you should use this mode. It is very accurate for points located inside the working area. However, it is less accurate for points outside the working area. The piecewise linear interpolation mode fits a piecewise linear interpolation function to the specified set of images coordinates and their real-world equivalents. It performs a mapping of each point in the image to a point in the real world using bilinear interpolation between adjacent points.

To allocate a piecewise linear interpolation camera calibration context, use [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md).

> **Note:** Note that piecewise linear interpolation mode does not support changing the distance between the camera and the camera calibration plane. Re-calibration is required in this case.

## Perspective transformation mode

The perspective transformation mode can compensate for rotation, translation, scale, and perspective distortions. For such distortions, the perspective transformation mode is accurate for points inside and outside the working area. This mode cannot compensate for non-linear distortions such as lens distortions. The perspective transformation mode best fits a global perspective transformation function to the set of image coordinates and their real-world equivalents. It finds a projection of an image in a field of view into a 2D plane.

To allocate a perspective transformation camera calibration context, use [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_PERSPECTIVE_TRANSFORMATION`](../../Reference/cal/McalAlloc.md).

> **Note:** Note that perspective transformation mode does not support changing the distance between the camera and the camera calibration plane. Re-calibration is required in this case.
