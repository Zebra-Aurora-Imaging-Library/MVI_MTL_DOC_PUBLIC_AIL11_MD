---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Arithmetic_with_images
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Arithmetic with images"
---

# Mathematics with images

It is often useful to perform mathematical operations on images. The Aurora Imaging Library Image Processing module allows you to perform mathematical operations on images using [`MimArith`](../../Reference/im/MimArith.md). If [`MimArith`](../../Reference/im/MimArith.md)does not support a required operation, you can map pixel values to their expected result, using [`MimLutMap`](../../Reference/im/MimLutMap.md).

## Basic mathematical operations

You can apply the following basic mathematical operations, using [`MimArith`](../../Reference/im/MimArith.md):

- You can add, subtract, multiply, divide, AND, NAND, OR, XOR, NOR, or XNOR two images or an image and a constant.
- You can negate, take the absolute value, NOT, square, square root, or cube the image.
- You can perform rounding operations such as the ceiling, floor, round, round half up, and truncation operations on the image.
- You can perform the trigonometric `atan2(_x_, _y_)` function using the first image for the X-coordinates and the second image for the Y-coordinates.
- You can copy a constant to the entire result buffer.

For example, for a surveillance application, you can extract the constant background from the grabbed image and display only changes in the image. The following example shows how this can be done.

> **Code example:** [userguide.image_processing.arithmetic_with_images](userguide.image_processing.arithmetic_with_images)

The basic mathematical operations are point-to-point, that is, they apply the specified operator on individual pixel values in a source image or on pixels at corresponding locations in two source images and do not depend on neighboring values.

## Computing the integral of an image

You can also use [`MimArith`](../../Reference/im/MimArith.md) to take the integral of an image. Calculating the integral of the image allows you to quickly calculate the summation of pixels in a rectangular area. Each pixel in the integral image contains the total sum of the pixels in the rectangular section of the source defined by the pixel (0, 0) and itself, inclusively. This is known as the inclusive integral of the image. The following image demonstrates a source image and its corresponding integral image. Notice how the value at (2, 2) in the integral image equals the sum of pixels in the source image from (0, 0) to (2, 2).

*[Image: ImArithImageIntegralIndividualPixel.png]*

Once the integral image has been computed, you can use it to quickly calculate the summation of pixel values in a selected area of pixels in the source image. To do so, you subtract the summation of the area above and to the left of the selected area from the summation of the area (0, 0) to the bottom-right pixel of the selected area; then, add the summation of the area until the top left-most pixel bordering the selected area. The latter is done to account for the overlap of the subtracted regions. Essentially, this means looking at four points of reference in the integral image. The following image illustrates how you can perform this quick calculation, as well as a selected area in the source image, and its corresponding location in the integral image.

*[Image: ImArithImageIntegralQuickCalculation.png]*

Therefore, the sum of the selected area above is 31.

The calculation of the integral image can be done in constant time, and in some cases, it is more efficient to calculate the integral of the entire image when you need to obtain the sum of multiple rectangular regions of pixels in an image, instead of calculating the sum of those specific areas individually.

## Mapping an image

You can perform complex operations (such as scaling and logarithms) on an image buffer, using [`MimLutMap`](../../Reference/im/MimLutMap.md). This function performs the operation simply by mapping the source image buffer through a specified lookup table (LUT) and storing results in the specified destination image buffer.

You allocate a LUT buffer, using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md), specifying the buffer attribute as [`M_LUT`](../../Reference/buf/MbufAlloc1d.md). You can assign mapping values to it by copying custom data from an array into it, using [`MbufPut1d`](../../Reference/buf/MbufPut1d.md). You can also generate data directly into a LUT buffer according to a specified function, using [`MgenLutFunction`](../../Reference/gen/MgenLutFunction.md). If you simply want to invert the image or set the image to a constant, you can alternatively use [`MgenLutRamp`](../../Reference/gen/MgenLutRamp.md).
