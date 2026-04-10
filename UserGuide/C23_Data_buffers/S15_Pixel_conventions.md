---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Pixel_conventions
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Pixel conventions"
---

# Pixel conventions and subpixel accuracy

When working in pixel units, you are using the pixel coordinate system, where the center of the top-left pixel corresponds to (0,0), the X-axis follows the first row of pixels, and the Y-axis follows the first column of pixels. Knowing what is considered the origin of the pixel coordinate system is important for all Aurora Imaging Library functions. Understanding Aurora Imaging Library's angle convention is also important for functions that can perform or deal with rotations.

## Dealing with subpixel accuracy

For Aurora Imaging Library functions that calculate with subpixel accuracy, it is important to keep in mind that since the center of the pixel is used as its reference position, the top-left corner of the first pixel is considered (-0.5, -0,5) and the bottom-right corner of the last pixel is considered (ImageSizeX - 0.5, ImageSizeY -0.5).

The following image shows the pixel coordinate system; the dots indicate the center of the pixels.

*[Image: cal_pixel_coordinate_system.png]*

With this in mind, the coordinates of the center of an image can always be found using the following formula:

*[Image: sizex-1.png]*

For example, the following image contains 4 pixels. If the formula is applied, the center of the image is found at (1.5, 0.0).

*[Image: pixelcenter.png]*

When a function works with subpixel accuracy, you must be especially careful when specifying end points and bounds. For example, to draw a rectangle around the border of an image of width SizeX and height SizeY, using [`MgraRect`](../../Reference/gra/MgraRect.md), you must take into account the 0.5 distance from the center of the pixel to its edges. The following images show how a vector based rectangle will be drawn using [`MgraRect`](../../Reference/gra/MgraRect.md) with and without the correction for subpixel accuracy.

*[Image: cal_draw_incorrect_border.png]*

The following code snippet shows the correct positions to specify when drawing a box that completly encloses an image.

> **Code example:** [userguide.data_buffers.pixel_conventions](userguide.data_buffers.pixel_conventions)

Note that when drawing with [`MgraRect`](../../Reference/gra/MgraRect.md) directly in an image, the annotations are scaled to the size of a pixel since they are drawn in a rasterized, destructive, way.

## Angle convention in Aurora Imaging Library

When working in pixel units, you define angles (in degrees) with respect to the pixel coordinate system. In this case, positive angles are always interpreted to be counterclockwise with respect to the positive X-axis.

*[Image: misc_pixel_angle_convention.png]*

Therefore, when rotating a point (x, y) around the origin, the 2D rotation matrix that Aurora Imaging Library uses is always:

*[Image: Rotation_matrix.png]*

Note that the direction of rotation is inversed from the typical mathematical convention since the Y-axis is oriented downwards. To determine the angle that follows the typical mathematical convention, subtract the angle from 360 (that is, 360 - angle). If you don't convert the angle, the sign of the sine function is inversed from the typical mathematical convention.
