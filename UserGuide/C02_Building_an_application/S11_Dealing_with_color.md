---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Dealing_with_color
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Dealing with color"
---

# Dealing with color

Aurora Imaging Library supports grabbing, displaying, accessing, and processing color images.

Aurora Imaging Library can represent an object in color with a single color buffer, allocated with [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md).

## Grabbing

You grab from a color video source (typically a camera) into a color image buffer, as you would into a grayscale image buffer, by calling [`MdigGrab`](../../Reference/dig/MdigGrab.md) or [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md).

Before performing a color grab, you must allocate a digitizer, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) (or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md)), specifying a digitizer configuration format (DCF) appropriate for your color camera. In addition, the digitizer and the image buffer must be allocated on the same system and have compatible dimensions. Once you have finished using the digitizer, you should free it, using [`MdigFree`](../../Reference/dig/MdigFree.md).

When grabbing from a color digitizer, each color band is transmitted simultaneously. The destination buffer must have the same number of color bands as the digitizer. The data is simultaneously stored in the appropriate band of the image buffer. When grabbing RGB, the red band is stored in the first color band, the green band is stored in the second color band, and the blue band is stored in the third color band.

If the hardware permits, you can control the digitization reference level of each acquisition path of the digitizer, using [`MdigControl`](../../Reference/dig/MdigControl.md) with the values in the [`digRef`](../../Reference/dig/MdigControl.md) table.

> **Note:** Note, upon installation, if you specified a color camera, the default image buffer, allocated with [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md), will be a three-band color image buffer. If you didn't specify a color camera, but would now prefer to use one, you can update your defaults using the Aurora Imaging Configurator utility.

### Mapping grabbed data through a LUT

You can also correct or precondition input data by mapping it through a lookup table (LUT) upon acquisition (if the hardware permits). This requires that you associate a LUT buffer with the digitizer, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_LUT_ID`](../../Reference/dig/MdigControl.md).

You can associate either a single-band LUT buffer or a LUT buffer that has the same number of color bands as the digitizer. If you associate a single-band LUT buffer with the digitizer, each band of the digitizer LUT is loaded with the same data. If you associate a multi-band LUT buffer with the digitizer, each band of the digitizer LUT is loaded with the data from corresponding color band of the LUT buffer.

To disassociate the LUT buffer from the digitizer, you need to re-associate the default LUT with the digitizer, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_LUT_ID`](../../Reference/dig/MdigControl.md) set to [`M_DEFAULT`](../../Reference/dig/MdigControl.md).

## Displaying

You display a color-image buffer as you would a two-dimensional grayscale image buffer. You must first allocate the image buffer with a displayable attribute ([`M_DISP`](../../Reference/buf/MbufAllocColor.md)), and then select it for display, using [`MdispSelect`](../../Reference/disp/MdispSelect.md). To stop displaying the image buffer and leave the display blank, use [`MdispSelect`](../../Reference/disp/MdispSelect.md) with [`M_NULL`](../../Reference/disp/MdispSelect.md).

Before you can display a buffer, the display must be allocated, using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md) (or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md)). The image buffer and the display should be allocated on the same system.

When you display a color image buffer (usually RGB), the first band is routed to the first output channel (usually red), the second band is routed to the second output channel (usually green), while the third band is routed to the third output channel (usually blue).

## Managing color images

Aurora Imaging Library supports the saving and loading of color images from disk in different file formats. See the [`MbufSave`](../../Reference/buf/MbufSave.md), [`MbufLoad`](../../Reference/buf/MbufLoad.md), [`MbufRestore`](../../Reference/buf/MbufRestore.md), [`MbufImport`](../../Reference/buf/MbufImport.md), and [`MbufExport`](../../Reference/buf/MbufExport.md) function reference descriptions for more details.

Note, all the Aurora Imaging Library data allocation, access, and generation ([`Mbuf...`](../../Reference/buf/MbufAlloc1d.md) and [`MgenLut...`](../../Reference/gen/MgenLutFunction.md)) functions can handle color image buffers.

## Color processing and analysis

To process and analyze color images, Aurora Imaging Library offers many solutions. The Color Analysis module, for example, accepts color images and allows you to perform color-based procedures such as transforming color data using relative color calibration, matching colors, calculating the difference between colors, and separating colors. The Image Processing module also accepts color images and offers a wide variety of operations that allow you to perform image enhancements, distortion corrections, and basic analysis. Unlike the Color Analysis module, the Image Processing module processes each band one at a time.

Most analysis modules usually require 1-band monochrome images. If you have a color image and you want to use it with a module that does not support color, you must do one of the following: convert the color image to grayscale, create a child buffer from one of the three color bands, or copy one of the three color bands to a 1-band buffer. After performing these operations and storing the transformed data in a monochrome buffer, you can pass it to an analysis module.

### Converting color images to grayscale

You can use [`MimConvert`](../../Reference/im/MimConvert.md) to convert color images to grayscale using [`M_RGB_TO_L`](../../Reference/im/MimConvert.md), where L represents the luminance (L) band of the HSL color space, or using [`M_RGB_TO_Y`](../../Reference/im/MimConvert.md), where Y represents the luminance (Y) band of the YUV color space. You can then use the converted image with an analysis module that does not support color.

You can also convert color images to grayscale using [`McolProject`](../../Reference/col/McolProject.md) with [`M_PRINCIPAL_COMPONENT_PROJECTION`](../../Reference/col/McolProject.md) which, unlike [`MimConvert`](../../Reference/im/MimConvert.md), does not simply extract a single band of the color space. Instead, it uses the distribution of the image's color data to calculate the best grayscale conversion possible, minimizing the loss of information. This results in a grayscale image that can better differentiate the color in the original source image.

*[Image: Color_LuminanceExtracted.png]*

*[Image: Color_PrincipalComponentProjection.png]*

For more information on converting color images to grayscale, see [Converting to grayscale](../C22_Color/S05_Converting_to_grayscale.md).

### Creating a child buffer from one of the three color bands

Since most analysis modules usually require 1-band monochrome images, you can use [`MbufChildColor`](../../Reference/buf/MbufChildColor.md) to allocate a 1-band child buffer within a 3-band color parent buffer. For example, if you have an RGB parent buffer, and use [`MbufChildColor`](../../Reference/buf/MbufChildColor.md) with [`M_RED`](../../Reference/buf/MbufChildColor.md), you will have a 1-band child buffer which contains the red color band of its parent. You can also use [`MbufChildColor2d`](../../Reference/buf/MbufChildColor2d.md) to select a 2-dimensional region in one of the color bands of the parent buffer.

For more information, see [Child buffers](../C23_Data_buffers/S06_Using_child_buffers_ROIs_or_a_copy_to_manipulate_specific_data_areas.md).

### Copying one of the three color bands to a 1-band buffer

Instead of using a child buffer to create a 1-band monochrome image from a color image, you can use [`MbufCopyColor`](../../Reference/buf/MbufCopyColor.md) to copy one band of a 3-band color image buffer. For example, if you have an RGB source image buffer, and use [`MbufCopyColor`](../../Reference/buf/MbufCopyColor.md) with [`M_RED`](../../Reference/buf/MbufCopyColor.md), you will have a 1-band destination buffer which contains the red color band of its source. You can also use [`MbufCopyColor2d`](../../Reference/buf/MbufCopyColor2d.md) to select a 2-dimensional region of the source buffer from which to copy.

For more information, see [Copying](../C23_Data_buffers/S07_Managing_data_buffers.md).

### Color space conversion

You can convert color data between HSL and RGB color spaces using [`MimConvert`](../../Reference/im/MimConvert.md) with [`M_HSL_TO_RGB`](../../Reference/im/MimConvert.md) or [`M_RGB_TO_HSL`](../../Reference/im/MimConvert.md). If using the Aurora Imaging Library Color Analysis module, you can call [`McolSetMethod`](../../Reference/col/McolSetMethod.md) to convert RGB to LAB or HSL before performing [`McolMatch`](../../Reference/col/McolMatch.md).

You can also use [`MimConvert`](../../Reference/im/MimConvert.md) to convert sRGB to LAB ([`M_SRGB_TO_LAB`](../../Reference/im/MimConvert.md)), and to convert LAB to sRGB ([`M_LAB_TO_SRGB`](../../Reference/im/MimConvert.md)). In these cases, your RGB data must adhere to standard RGB specifications, referred to as sRGB, as defined by the International Electrotechnical Commission (IEC) Project Team 61966-2-1.[`MimConvert`](../../Reference/im/MimConvert.md) allows many variations with which to convert to and from sRGB. For example, you can specify the LCH color space (a reinterpretation of LAB) and you can specify whether color data is linear (gamma corrected). For more information, see [Converting between color spaces](../C22_Color/S03_Color_spaces_and_converting_between_them.md).

> **Note:** [`MimConvert`](../../Reference/im/MimConvert.md) is available with Aurora Imaging Library Lite, while [`McolSetMethod`](../../Reference/col/McolSetMethod.md) requires the full version of Aurora Imaging Library.
