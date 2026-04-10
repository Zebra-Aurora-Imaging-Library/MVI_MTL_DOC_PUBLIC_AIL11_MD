---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_image_processing
section: Warping
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-image / Warping"
---

# Warping

In addition to functions which perform specific geometric transforms ([`MimFlip`](../../Reference/im/MimFlip.md), [`MimResize`](../../Reference/im/MimResize.md), [`MimRotate`](../../Reference/im/MimRotate.md), [`MimTranslate`](../../Reference/im/MimTranslate.md), and [`MimPolarTransform`](../../Reference/im/MimPolarTransform.md)), Aurora Imaging Library includes a more general geometric function, [`MimWarp`](../../Reference/im/MimWarp.md). It can perform any of the specific transforms, as well as complex warpings. Specifically, it can perform: first-order polynomial warpings, perspective polynomial warpings, polar-to-rectangular (and vice versa) warpings, and custom warpings.

*[Image: warping.png]*

Note that the functions which perform specific transforms are faster than [`MimWarp`](../../Reference/im/MimWarp.md). You should only use [`MimWarp`](../../Reference/im/MimWarp.md) when the required transform cannot be otherwise performed. In addition, geometric distortions can also be resolved using the Camera Calibration module. For more information, see [Calibrating your camera setup](../C28_Calibration/ChapterInformation.md).

[`MimWarp`](../../Reference/im/MimWarp.md) performs a warping by first associating each pixel position of the destination buffer, `(x<sub>d</sub>, y<sub>d</sub>)`, with a specific point (not necessarily a pixel) in the source buffer, `(x<sub>s</sub>, y<sub>s</sub>)`. The pixel value at `(x<sub>d</sub>, y<sub>d</sub>)` is then determined from an interpolation around its associated source point. Destination pixels can be associated with source points in two different ways:

- Using a 3x3 coefficients matrix that is used as follows: *[Image: warp3x3.png]*
- Using LUTs (an X-LUT and a Y-LUT) that are the same size as the destination image and are used as follows:
  `_X_ <sub>s</sub> = LUT<sub>X</sub>[_X_ <sub>d</sub>, _Y_ <sub>d</sub>]`
  `_Y_ <sub>s</sub> = LUT<sub>Y</sub>[_X_ <sub>d</sub>, _Y_ <sub>d</sub>]`

If you intend on performing the same warping operation on multiple images (which would require using the same LUTs), it might be faster to generate the LUTs for the operation once and repeatedly pass them to [`MimWarp`](../../Reference/im/MimWarp.md). However, if different warpings are required, or only one image is to be processed, it is faster to generate the coefficients and call [`MimWarp`](../../Reference/im/MimWarp.md) than it is to generate the LUTs and then call [`MimWarp`](../../Reference/im/MimWarp.md).

## First-order polynomial warpings

A first-order polynomial warping is equivalent to a combination of a linear translatation, rotation, resizing, and/or shearing of an image. First-order polynomial warpings are performed by associating points in the source buffer with pixels in the destination buffer according to the following equations:

`_X_ <sub>s</sub> = a<sub>0</sub> _X_ <sub>d</sub> + a<sub>1</sub> _Y_ <sub>d</sub> + a<sub>2</sub>`

`_Y_ <sub>s</sub> = b<sub>0</sub> _X_ <sub>d</sub> + b<sub>1</sub> _Y_ <sub>d</sub> + b<sub>2</sub>`

To perform a first-order polynomial warping using a 3x3 coefficients matrix, you must pass [`MimWarp`](../../Reference/im/MimWarp.md) the identifier of a single-band, 32-bit floating-point, 3x3 [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) buffer filled with the coefficients to [`MimWarp`](../../Reference/im/MimWarp.md). The elements of the last row of the coefficient matrix should be [0, 0, 1]. You can also specify a 3x2 [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) buffer filled with the coefficients, and the elements of the last row are assumed to be [0, 0, 1]. The coefficients of this matrix can be:

- Generated automatically using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md).
- Generated using the basic transformation functions[`MimResize`](../../Reference/im/MimResize.md), [`MimRotate`](../../Reference/im/MimRotate.md), and [`MimTranslate`](../../Reference/im/MimTranslate.md).
- User-established.

When using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md), specify how to perform the warping (for example, specify by how much to rotate and resize an image); the function then generates the coefficients required to produce such a warping. To combine coefficients, use separate calls to [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md). For example, to generate coefficients for a rotation and translation, call [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md) twice, using the output buffer of the first call as the input buffer of the second call. To create the LUTs using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md), you can either pass the same information as required to generate the coefficients or you can pass the 3x3 coefficients matrix itself.

When using the basic transformation functions to generate coefficients, specify the source image buffer as [`M_NULL`](../../Reference/im/MimResize.md), and provide transformation details. You must also provide a destination buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md)attribute and that has dimensions of 3x3 to store the generated coefficients. For more information, see the individual reference page for each function.

## Perspective polynomial warpings

A perspective polynomial warping is used to map an arbitrary quadrilateral onto a rectangle or to map a rectangle onto an arbitrary quadrilateral.

Perspective polynomial warpings are performed by associating points in the source buffer with pixels in the destination buffer according to the following equations:

`_X_ <sub>s</sub> = (a<sub>0</sub> _X_ <sub>d</sub> + a<sub>1</sub> _Y_ <sub>d</sub> + a<sub>2</sub>)/(c<sub>0</sub> _X_ <sub>d</sub> + c<sub>1</sub> _Y_ <sub>d</sub> + c<sub>2</sub>)`

`_Y_ <sub>s</sub> = (b<sub>0</sub> _X_ <sub>d</sub> + b<sub>1</sub> _Y_ <sub>d</sub> + b<sub>2</sub>)/(c<sub>0</sub> _X_ <sub>d</sub> + c<sub>1</sub> _Y_ <sub>d</sub> + c<sub>2</sub>)`

To perform a perspective polynomial warping using a 3x3 coefficients matrix, you must pass [`MimWarp`](../../Reference/im/MimWarp.md) the identifier of a single-band, 32-bit floating-point 3x3[`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) buffer filled with the coefficients. The coefficients of this matrix can be generated automatically using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md), or can be user established. When using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md), you must specify the coordinates of the four corners of the quadrilateral or the two opposite corners of the rectangle. The coordinates are illustrated in the image below.

*[Image: persp_warp.png]*

## Polar-to-rectangular, rectangular-to-polar, and custom warpings

In addition to first-order polynomial and perspective polynomial warpings, it is possible to perform more complex warping operations using [`MimWarp`](../../Reference/im/MimWarp.md), if used with two 2D LUTs. In this case, the LUTs map destination pixels (_X<sub>d</sub>, Y<sub>d</sub> _) to specified points (_X<sub>s</sub> _, _Y<sub>s</sub> _) in the source image buffer as follows: _X<sub>s</sub> _is determined from (_X<sub>d</sub>, Y<sub>d</sub> _) through one LUT (X-LUT) and _Y<sub>s</sub> _ is determined from (_X<sub>d</sub>, Y<sub>d</sub> _) through another LUT (Y-LUT). Both LUTs should have the same X and Y-size as the destination image.

You can generate these LUTs using [`MimPolarTransform`](../../Reference/im/MimPolarTransform.md), to warp from the polar to the rectangular coordinate system (or vice versa). Alternatively, you can fill the LUTs with a custom transformation generated from another source. Once the X-LUT and Y-LUT are loaded with your values, you can pass them to [`MimWarp`](../../Reference/im/MimWarp.md) to perform the warpings.

A polar-to-rectangular warping takes a polar image as is shown in the left-most image, and transforms it to a rectangular image, as shown in the right-most image. See [Polar-to-rectangular and rectangular-to-polar transforms](S08_Polartorectangular_and_rectangulartopolar_transform.md) for more information.

|   |   |
| --- | --- |
| *[Image: mimpolartransform_dia1.png]* | *[Image: mimpolartransform_dia2.png]* |

To perform polar-to-rectangular or rectangular-to-polar transformations using [`MimWarp`](../../Reference/im/MimWarp.md), first call [`MimPolarTransform`](../../Reference/im/MimPolarTransform.md) with the required information to perform the transformation. However, instead of specifying a source and destination image, specify a 2D LUT buffer ([`M_LUT`](../../Reference/buf/MbufAlloc2d.md)) to store the X-coordinate mapping, and a 2D LUT buffer to store the Y-coordinate mapping. Then, call [`MimWarp`](../../Reference/im/MimWarp.md) with the LUTs and the source and destination image buffers. The LUT buffers that you specify must be the same size as the destination image.

To perform a custom warping using [`MimWarp`](../../Reference/im/MimWarp.md), you must fill two 2D LUTs ([`M_LUT`](../../Reference/buf/MbufAlloc2d.md)) representing the X and Y-coordinates, using [`MbufPut2d`](../../Reference/buf/MbufPut2d.md). Every position in the X-LUT (X, Y) specifies the X-coordinates (_X<sub>s</sub> _) of the point in the source image every position in the Y-LUT (X, Y) specifies the Y-coordinates (_Y<sub>s</sub> _) in the source image. This type of warping arbitrarily maps the pixel position of the destination buffer to a specific point in the source buffer, based on what was passed to the LUTs in [`MbufPut2d`](../../Reference/buf/MbufPut2d.md). You can specify sub-pixel coordinates for the source point.

## Interpolation modes

When you perform a warping, each pixel position in the destination buffer `(X<sub>d</sub>, Y<sub>d</sub>)`, gets associated with a specific point in the source buffer `(X<sub>s</sub>, Y<sub>s</sub>)`, and a computed intensity value for `(X<sub>s</sub>, Y<sub>s</sub>)` is then copied to `(X<sub>d</sub>, Y<sub>d</sub>)`. The destination coordinates have integer values but the source coordinates, in general, do not. Therefore, the pixel value at `(X<sub>d</sub>, Y<sub>d</sub>)` is determined from several source pixels that are near `(X<sub>s</sub>, Y<sub>s</sub>)`, according to a specified interpolation mode.

The various interpolation modes available when using [`MimWarp`](../../Reference/im/MimWarp.md) are:

- A standard nearest neighbor interpolation ([`M_NEAREST_NEIGHBOR`](../../Reference/im/MimResize.md)).
- A standard bilinear interpolation ([`M_BILINEAR`](../../Reference/im/MimResize.md)).
- A standard bicubic interpolation ([`M_BICUBIC`](../../Reference/im/MimResize.md)).

In general, nearest-neighbor interpolation is the fastest to perform, and bicubic interpolation is the slowest. However, nearest-neighbor interpolation produces the least accurate results, and bicubic interpolation produces the most accurate. Bilinear interpolation is often the best compromise between speed and accuracy.

For more information on interpolation modes, see [Interpolation modes](../C03_Image_processing/S15_Basic_geometrical_transform.md).

## Points outside the source buffer

Sometimes, the point associated with a destination pixel will fall outside the source buffer. In such cases, the new value for the destination pixel can be determined in one of the following ways:

- You can use pixels from the source buffer's ancestor buffer. If the source buffer is not a child buffer or if the point falls outside the ancestor buffer, the destination pixel will be left as is.
- You can just leave the destination pixel as is.
- You can set the destination pixel to 0.

In general, you should use pixels from the source buffer's ancestor buffer when the source buffer is a child buffer. This will ensure that the pixels you use are related to the source buffer. If the source buffer is not a child buffer, use one of the other options.

Note that you can set the destination pixels that correspond to values outside the source buffer to a fixed value other than 0 by first clearing the destination buffer to that value.

## Transforming coordinate lists

If you only want to establish the location in the source image to which destination positions are mapped, you can use [`MimWarpList`](../../Reference/im/MimWarpList.md)to transform a list of coordinates (points) using a specified warping matrix. By default, [`MimWarpList`](../../Reference/im/MimWarpList.md)transforms the coordinates using the specified warping as is. This is referred to as a reverse warping transformation ([`M_REVERSE`](../../Reference/im/MimWarpList.md)) because this is how the [`MimWarp`](../../Reference/im/MimWarp.md) function associates each pixel position of the destination buffer with a specific point in the source buffer.

You can also have [`MimWarpList`](../../Reference/im/MimWarpList.md) use the inverse of the specified matrix ([`M_FORWARD`](../../Reference/im/MimWarpList.md)); this is referred to as a forward warping transformation. This type of transformation is useful if you need to determine if a specific pixel in the source image is actually mapped to a destination buffer pixel.

You can only pass[`MimWarpList`](../../Reference/im/MimWarpList.md)a coefficient matrix; this allows for either first-order polynomial warpings or perspective polynomial warpings. Regardless of whether performing an [`M_REVERSE`](../../Reference/im/MimWarpList.md) or an [`M_FORWARD`](../../Reference/im/MimWarpList.md) transformation, pass the points to transform in an array to the [`SrcCoordXArrayPtr`](../../Reference/im/MimWarpList.md) and [`SrcCoordYArrayPtr`](../../Reference/im/MimWarpList.md) parameters. This is because the operation is the same; only the coefficient matrix is affected.

## Warping example

To run/view the warping example._MimWarp.cpp_, use Aurora Imaging Example Launcher. You can also view this example here:

> **Code example:** [mimwarp.cpp](mimwarp.cpp)
