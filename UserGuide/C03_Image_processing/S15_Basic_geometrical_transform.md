---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Basic_geometrical_transform
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Basic geometrical transform"
---

# Basic geometric transforms

Image distortions can affect application results. For example, in a medical application that analyzes blood cells, if the camera does not have a one-to-one aspect ratio and no correction is performed, the cells appear distorted and elongated, and incorrect interpretations might result. Rotating such an image causes even more serious object distortion.

To resolve distortion problems, the Aurora Imaging Library Image Processing module offers basic, as well as advanced, geometric functions. Since the advanced geometric functions ([`MimPolarTransform`](../../Reference/im/MimPolarTransform.md) and [`MimWarp`](../../Reference/im/MimWarp.md)) are slower than the basic geometric functions, they should only be used when the required transform cannot be performed using a basic geometric function. For information on the advanced geometric functions, see [Advanced image processing](../C04_Advanced_image_processing/ChapterInformation.md).

> **Note:** Note that Aurora Imaging Library Lite does not support advanced geometric functions, nor does it support rotating, translating, or flipping the image.

In some instances, the orientation of an image can also cause erroneous conclusions. When an object is rotated from its original position, you can realign it by the required angle, using [`MimRotate`](../../Reference/im/MimRotate.md). If the image has prominent edges that identify the right orientation, you can have Aurora Imaging Library automatically calculate the required angle of rotation for a proper orientation using [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md). [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) establishes the best orientations based on the edges in your image and calculates the angles at which they occur. You can then call [`MimRotate`](../../Reference/im/MimRotate.md) with the angle that was returned with the best score to rotate your image. For more information on finding the dominant orientations of your image, see [Finding dominant image orientations](../C04_Advanced_image_processing/S11_Finding_Optimal_Image_Orientations.md).

[`MimTranslate`](../../Reference/im/MimTranslate.md) displaces an image by a specified number of pixels in the X and/or Y direction, with subpixel accuracy.

[`MimFlip`](../../Reference/im/MimFlip.md) flips an image horizontally (left to right) or vertically (top to bottom). Note that flipping horizontally allows you to get a mirror copy of the original image.

The [`MimResize`](../../Reference/im/MimResize.md) function resizes an image along the horizontal and/or vertical axis. This can help resolve aspect-ratio problems. If both the horizontal and vertical resizing factors are set to the same value, this function can reduce or magnify an image to an appropriate size.

## Interpolation modes

When you perform a geometric transformation, each pixel position in the destination buffer `(x<sub>d</sub>, y<sub>d</sub>)`, gets associated with a specific point in the source buffer `(x<sub>s</sub>, y<sub>s</sub>)`, and a computed intensity value for `(x<sub>s</sub>, y<sub>s</sub>)` is then copied to `(x<sub>d</sub>, y<sub>d</sub>)`. The destination coordinates have integer values but the source coordinates, in general, do not. Therefore, the pixel value at `(x<sub>d</sub>, y<sub>d</sub>)` is determined from several source pixels that are near `(x<sub>s</sub>, y<sub>s</sub>)`, according to a specified interpolation mode.

The various interpolation modes available in Aurora Imaging Library are:

- A standard nearest neighbor interpolation ([`M_NEAREST_NEIGHBOR`](../../Reference/im/MimResize.md)).
- A standard bilinear interpolation ([`M_BILINEAR`](../../Reference/im/MimResize.md)).
- A standard bicubic interpolation ([`M_BICUBIC`](../../Reference/im/MimResize.md)).
- A weighted average interpolation ([`M_AVERAGE`](../../Reference/im/MimResize.md)).
- An interpolation based on the minimum or maximum pixel value in the neighborhood of the source point ([`M_MIN`](../../Reference/im/MimResize.md) or [`M_MAX`](../../Reference/im/MimResize.md)).

### Nearest-neighbor interpolation

Nearest-neighbor interpolation mode determines the intensity value of the point based on the intensity of the nearest source pixel in the source image. No other neighboring values are taken into account. In the following image, a source image goes through a resize operation ([`MimResize`](../../Reference/im/MimResize.md)) using a nearest neighbor interpolation:

*[Image: NearestNeighborGraphic.png]*

### Bilinear interpolation

Bilinear interpolation mode calculates an intensity value for the source point by taking a weighted average of the four source pixels nearest to the source point; it takes the 2 nearest pixels in the X-direction, and the 2 nearest pixels in the Y-direction. The pixels closest to the point are given the most weight.

Although processing is slightly slower, this results in a smoother and more accurate interpolation than when using a nearest neighbor operation. In the following image, a resize operation ([`MimResize`](../../Reference/im/MimResize.md)) is performed on the source image using a bilinear interpolation:

*[Image: BilinearGraphic.png]*

### Bicubic interpolation

Bicubic interpolation mode calculates an intensity value for the source point by taking a weighted average of the sixteen source pixels nearest to the source point (a 4x4 area); it takes an area established by the 4 nearest pixels in the X-direction, and the 4 nearest pixels in the Y-direction. Again, the pixels closest to the point are given the most weight.

Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs, the result is saturated according to the depth and data type of the destination buffer.

This interpolation mode requires a relatively large amount of processing time, but yields the best and the most accurate results of all the general interpolation modes.

### Average interpolation

Average interpolation mode calculates an intensity value for the source point by taking a weighted average of the pixel area that surrounds the source point, whereby the area size is established from the scaling factor and the size of the source image. This mode is only available when using [`MimResize`](../../Reference/im/MimResize.md)and can only be used to reduce the size of an image (a scaling factor smaller than or equal to 1).

Essentially, this mode takes a weighted average of the source pixel area that the destination pixel represents. For example, if the source buffer is a 25x25 buffer and you are using a scale factor of 0.2, the destination buffer required will be a 5x5 buffer. Therefore, each destination pixel represents a 5x5 area in the source buffer, so a weighted average of the 25 source pixels nearest to the source point `(x<sub>s</sub>, y<sub>s</sub>)` is taken. The weight of each source pixel is determined by the area it contributes to the destination pixel. The greater the area that a source pixel contributes to a destination pixel, the larger the weight of the source pixel. This interpolation mode performs the most accurate interpolation for resizing; however, depending on the scaling factor, the operation can be more processing intensive than other interpolation modes, due to the fact that the neighborhood being evaluated could be large. In the image below, an image is resized ([`MimResize`](../../Reference/im/MimResize.md)) using an [`M_AVERAGE`](../../Reference/im/MimResize.md) interpolation mode):

*[Image: AverageGraphic.png]*

In this case, the source point _Sp_ is an average of the source pixels _S1_, _S2_, _S3_, and _S4_. The weights of each source pixel depends on its contributing area, and therefore the intensity of _D1_ will be:

*[Image: UGBasicGeoTrans-ExEq01.png]*

> **Note:** Note that 2.25 is the total area contributed by the source pixels.

In the above example, when solving for the source point's resulting intensity, the following result is obtained:

*[Image: UGBasicGeoTrans-ExEq02.png]*

### M_INTERPOLATE interpolation

[`M_INTERPOLATE`](../../Reference/im/MimResize.md) interpolation mode calculates an intensity value for the source point based on the scaling operation. When performing a "zoom in" resizing operation, a bilinear interpolation will be used. When performing a "zoom out" resizing operation, averaging interpolation will be used. This mode is only available when using [`MimResize`](../../Reference/im/MimResize.md). This provides a good quality resulting image and an adequate processing speed.

### Maximum/minimum interpolation

Maximum and minimum interpolation modes calculate an intensity value for the source point by taking the maximum or minimum pixel value in the source pixel area that surrounds the source point, whereby the area's size is established from the scaling factor and the size of the source image. These modes are only available when using [`MimResize`](../../Reference/im/MimResize.md)and can only be used when reducing the size of an image (a scaling factor smaller than or equal to 1).

For example, if the source image is 4x4 and the scaling factor is 0.5, each destination pixel represents a 2x2 area in the source buffer. If using an [`M_MAX`](../../Reference/im/MimResize.md) interpolation mode, the highest pixel value in the 2x2 area around the source point, is used as the intensity value of the source point.

*[Image: MimResizeMin.png]*

If performing a resize operation to reduce the size of an image using other interpolation modes (for example, [`M_AVERAGE`](../../Reference/im/MimResize.md)) introduces blurriness in certain features you want to preserve, such as text and edges, try using [`M_MIN`](../../Reference/im/MimResize.md) or [`M_MAX`](../../Reference/im/MimResize.md). These interpolations are similar to morphological operations. Although they can decrease an object's thickness and amplify noise, they can be effective at maintaining the distinctiveness of certain features.
