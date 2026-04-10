---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Propagating_calibration_information_after_performing_a_geometric_operation
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Propagating calibration information after performing a geometric operation"
---

# Propagating camera calibration information after performing a geometric operation

When you have a calibrated image and perform a geometric operation, the resulting image does not retain its camera calibration information. However, you can create an appropriately warped version of the camera calibration context and associate it with the uncalibrated geometrically transformed image.

[`McalWarp`](../../Reference/cal/McalWarp.md)allows you to apply a warping to the pixel-to-world mapping of a camera calibration context and use it to configure a specified camera calibration context. The newly configured context can then be associated with a similarly warped image. [`McalWarp`](../../Reference/cal/McalWarp.md) is meant to be used in conjunction with [`MimWarp`](../../Reference/im/MimWarp.md). As such, it takes the same warping parameter values used to transform the image. Therefore, [`McalWarp`](../../Reference/cal/McalWarp.md) can perform first-order polynomial warpings, perspective polynomial warpings, and custom warpings using LUTs.

If you want to apply a flip, resize, rotate, or translate operation on an image and wanted the destination image to be appropriately calibrated, you must perform the transformation of the image using [`MimWarp`](../../Reference/im/MimWarp.md) instead of [`MimFlip`](../../Reference/im/MimFlip.md), [`MimResize`](../../Reference/im/MimResize.md), [`MimRotate`](../../Reference/im/MimRotate.md), and [`MimTranslate`](../../Reference/im/MimTranslate.md); you can then use [`McalWarp`](../../Reference/cal/McalWarp.md) followed by [`McalAssociate`](../../Reference/cal/McalAssociate.md) to appropriately calibrate the transformed image. Note that, [`MimWarp`](../../Reference/im/MimWarp.md) and [`McalWarp`](../../Reference/cal/McalWarp.md) use a matrix of coefficients (or LUTs) to describe its transformations. To generate the coefficients that represent the required transform, you can use [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md) to first generate the required transformation matrix (or LUTs) and subsequently call [`MimWarp`](../../Reference/im/MimWarp.md) and [`McalWarp`](../../Reference/cal/McalWarp.md).

To perform a geometric operation on an image and preserve the camera calibration information, perform the following:

1. Call [`MimWarp`](../../Reference/im/MimWarp.md) on the calibrated image to warp your image.
2. Allocate a new camera calibration context using [`McalAlloc`](../../Reference/cal/McalAlloc.md) and specify the camera calibration mode best suited for the geometric transform.
   | Source camera calibration mode | Destination's camera calibration mode after a linear geometric transform (translation, rotation, resize) | Destination's camera calibration mode after a non-linear geometric transform (perspective, LUTs) |
   | --- | --- | --- |
   | Uniform | Uniform | Piecewise linear interpolation |
   | Piecewise linear interpolation | Piecewise linear interpolation | Piecewise linear interpolation |
   | Perspective transformation | Perspective transformation | Piecewise linear interpolation |
   | Tsai-based/Zhang-based/Robotics | Piecewise linear interpolation | Piecewise linear interpolation |
   > **Note:** Note that, the piecewise linear interpolation mode can accurately represent all geometric transforms and [`McalWarp`](../../Reference/cal/McalWarp.md) does not generate 3D-based camera calibration contexts.
3. Call [`McalWarp`](../../Reference/cal/McalWarp.md) as follows:
   1. Set the original calibrated image as the source image for [`McalWarp`](../../Reference/cal/McalWarp.md), and set the newly allocated camera calibration context as the destination.
   2. Use the same matrix coefficients or LUTs used by [`MimWarp`](../../Reference/im/MimWarp.md) for [`McalWarp`](../../Reference/cal/McalWarp.md), and set the transformation type to [`M_WARP_LUT`](../../Reference/cal/McalWarp.md) or [`M_WARP_POLYNOMIAL`](../../Reference/cal/McalWarp.md) based on the warping applied to the image.
4. Associate the newly configured camera calibration context with the warped image of [`MimWarp`](../../Reference/im/MimWarp.md).

When warping the pixel-to-world mapping of a camera calibration context, the destination camera calibration context need not be in the same camera calibration mode as the source context. [`McalWarp`](../../Reference/cal/McalWarp.md) approximates the equivalent calibration when converting from one mode to the other.

The following code snippet shows how to perform a rotation using [`MimWarp`](../../Reference/im/MimWarp.md). It generates the required transformation matrices for the rotation using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md). It then shows how to warp the pixel-to-world mapping of the existing camera calibration context and propagate the new camera calibration information to the warped image using [`McalWarp`](../../Reference/cal/McalWarp.md) and then [`McalAssociate`](../../Reference/cal/McalAssociate.md) respectively.

> **Code example:** [userguide.calibration.propogating_calibration_information_after_performing_a_geometric_operation](userguide.calibration.propogating_calibration_information_after_performing_a_geometric_operation)
