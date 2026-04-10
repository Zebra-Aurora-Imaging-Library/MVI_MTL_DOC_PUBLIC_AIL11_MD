---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Converting_to_grayscale
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Converting to grayscale"
---

# Converting to grayscale

Converting color images to grayscale can be useful since many processing and analysis modules operate on grayscale images. For example, a picture of a license plate can be taken in color, but before it can be used in an Automatic Number Plate Recognition (ANPR) application written with the Aurora Imaging Library String Reader module, it must be converted to grayscale.

To convert color images to grayscale, you can either use [`MimConvert`](../../Reference/im/MimConvert.md) to extract the luminance, or [`McolProject`](../../Reference/col/McolProject.md) to perform a principal component projection. To enhance image quality and improve subsequent operations, you can also process your images by, for example, performing binary thresholding.

Various conditions, such as different cameras and illuminants, can cause color from identical images to appear dissimilar. If this occurs, you can call [`McolTransform`](../../Reference/col/McolTransform.md) to perform a relative color calibration before converting color images to grayscale. Relative color calibration ensures all colors are consistent according to a specified reference color. For more information, see [Relative color calibration](S04_Relative_color_calibration.md).

[`MimConvert`](../../Reference/im/MimConvert.md) is available with Aurora Imaging Library Lite. [`McolProject`](../../Reference/col/McolProject.md) and [`McolTransform`](../../Reference/col/McolTransform.md) require the full version of Aurora Imaging Library.

## Extracting the luminance

You can use [`MimConvert`](../../Reference/im/MimConvert.md) to convert color images to grayscale by extracting the luminance (intensity) from an RGB image ([`M_RGB_TO_L`](../../Reference/im/MimConvert.md)), or copying the luminance component of an image into a 3-band RGB buffer ([`M_L_TO_RGB`](../../Reference/im/MimConvert.md)), to create a monochromatic (gray) RGB buffer. For the YUV color space, you can use [`M_RGB_TO_Y`](../../Reference/im/MimConvert.md) to extract the Y-component from an RGB image.

You can also use [`MimConvert`](../../Reference/im/MimConvert.md) to convert from RGB to HSL, or vice versa ([`M_RGB_TO_HSL`](../../Reference/im/MimConvert.md) and [`M_HSL_TO_RGB`](../../Reference/im/MimConvert.md), respectively). This can be useful if you want to perform color analysis in a color space other than the one in which your color data is stored.

To convert between RGB and HSL color spaces, [`MimConvert`](../../Reference/im/MimConvert.md) uses the following algorithm:

> **Code example:** [userguide.color.performing_color_conversions](userguide.color.performing_color_conversions)

For more technical information, refer to _Computer Graphics: Principles and Practice in C, James D. Foley et al., Addison-Wesley Publishing Company, United States, 1995_.

## Principal component projection

You can convert a color image to grayscale using [`McolProject`](../../Reference/col/McolProject.md) with [`M_PRINCIPAL_COMPONENT_PROJECTION`](../../Reference/col/McolProject.md) which, unlike [`MimConvert`](../../Reference/im/MimConvert.md), does not simply extract a single component of the color space. Instead, it uses the distribution of the image's color data to calculate the best grayscale conversion possible, minimizing the loss of information. This results in a grayscale image that can better differentiate the color in the original source image.

*[Image: Color_LuminanceExtracted.png]*

*[Image: Color_PrincipalComponentProjection.png]*

As the example above illustrates, extracting just the luminance, as is done with [`MimConvert`](../../Reference/im/MimConvert.md), can cause different colors to look the same in grayscale, which can make analysis problematic in certain cases. However, with [`M_PRINCIPAL_COMPONENT_PROJECTION`](../../Reference/col/McolProject.md), Aurora Imaging Library performs a principal component analysis (PCA) to calculate the color image's strongest vector, referred to as the first principal component, which represents the greatest degree of color variation within the color space. Each color pixel is then projected to a point on this vector, resulting in a grayscale image that conveys more information than a luminance extraction.

*[Image: Color_ConvertingToGrayscaleLuminanceAndProjection.png]*

If the first principal component vector is the same as the luminance vector (black-to-white), the principal component projection will be very similar to extracting the luminance band. This can happen if, for example, the greatest variation in color in the image is roughly from black to white. In this case, it can still be advantageous to use the first principal component, since this vector was calculated from the data distribution itself and is optimally oriented (black-to-white/white-to-black) for the projection calculation.

### Using a principal component projection for converting to grayscale

For converting to grayscale, [`McolProject`](../../Reference/col/McolProject.md) calculates the first principal component vector using the entire source image or specific areas of the source image. Typically using the entire source image is sufficient.

To identify specific areas in the source image with which to calculate the first principal component, you must specify another image, referred to as a data identification image. In this image, you must set the corresponding pixels to **M_SOURCE_LABEL**. By specifying the exact colors (pixels) in the source image to use, you can increase the difference between grayscale values in some cases.

*[Image: Color_PrincipalComponentProjectionSourceLabel.png]*

When using a principal component projection to convert to grayscale, colors are automatically projected between the brightest (white) and darkest (black) side of the grayscale palette. If this is performed properly, the resulting status of [`McolProject`](../../Reference/col/McolProject.md) is [`M_SUCCESS`](../../Reference/col/McolProject.md). If Aurora Imaging Library cannot confidently determine which pixels to project to the bright side of the grayscale palette and which to project to the dark side, the direction of the projection is chosen arbitrarily and the resulting status is [`M_UNSTABLE_POLARITY`](../../Reference/col/McolProject.md). When this occurs, you can use the data identification image to identify the corresponding pixels in the source that you want to project to the bright side of the grayscale palette and which to project to the dark side, using [`McolProject`](../../Reference/col/McolProject.md) with **M_BRIGHT_LABEL** and **M_DARK_LABEL**.

*[Image: Color_PrincipalComponentProjectionDarkBrightLabel.png]*

By default, Aurora Imaging Library projects the result according to the dynamic range (minimum and maximum pixel values) established from all the pixels in the source image, even if you have set specific source pixels to **M_SOURCE_LABEL**. If necessary, you can use [`M_MASK_CONTRAST_ENHANCEMENT`](../../Reference/col/McolProject.md) to improve the conversion to grayscale by performing the projection according to the dynamic range established from only the specified source image pixels. The result is similar to increasing the contrast.

*[Image: Color_PrincipalComponentProjectionMaskEnhancement.png]*

When using [`M_MASK_CONTRAST_ENHANCEMENT`](../../Reference/col/McolProject.md), Aurora Imaging Library saturates the projected unidentified source image colors that are outside the dynamic range of the identified source image pixels' color (**M_SOURCE_LABEL**); that is, Aurora Imaging Library maps those colors to either the minimum or maximum allowable grayscale value. In this case, the projection result can contain sections that have lost a certain degree of contrast information.

*[Image: Color_Mask_Enhancement.png]*

This example illustrates how, given the dynamic range Aurora Imaging Library establishes from the identified source image pixels, Aurora Imaging Library projects to 0 any unidentified pixel value below 100, and projects to 255 any value above 200.

| Dynamic range of the identified source image pixels: 100 to 130 |
| --- |
| 5 | 100 |
| 10 | 100 |
| 20 | 100 |
| 40 | 100 |
| 195 | 200 |
| 200 | 200 |
| 220 | 200 |
| 240 | 200 |

When using [`McolProject`](../../Reference/col/McolProject.md), you can also specify a destination mask. The mask determines where to write the result of the projection operation and does not affect calculations. Results are only written to unmasked destination pixels. Do not confuse this mask with the data identification image containing **M_SOURCE_LABEL**, which is used to identify the source image pixels to use for the calculation.

[`McolProject`](../../Reference/col/McolProject.md) can either produce the resulting grayscale image, or the transformation matrix to convert the image to grayscale. You can use this matrix with [`MimConvert`](../../Reference/im/MimConvert.md) to perform an optimal color-to-grayscale transformation of any 3-band color image. Only use this matrix to convert images that have a similar color distribution, otherwise unpredictable results can occur.
