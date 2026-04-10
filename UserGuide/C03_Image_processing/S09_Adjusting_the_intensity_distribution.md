---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Adjusting_the_intensity_distribution
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Adjusting the intensity distribution"
---

# Adjusting the intensity distribution

There are two ways to adjust the intensity distribution of an image:

- Window leveling.
- Histogram equalization mapping.

## Window leveling

Window leveling takes a range of pixel values in the source image and linearly maps these values to a different range in the destination image.

By stretching the range of values actually present in an image to the full range available, window leveling allows you to use all available intensities. Window leveling is especially useful when examining 10- or 12-bit images that have been grabbed into 16-bit images so that the image can occupy all of the available intensity range.

When using Aurora Imaging Library, you will sometimes be limited to using 8-bit image buffers. If your images have a greater bit depth, you can use window leveling to map your images to an 8-bit range. For example, to convert a 10-bit image into an 8-bit image, you can set the destination to an 8-bit image buffer and set the output range of the window leveling as 0 to 255.

### Window leveling using MimRemap()

The function [`MimRemap`](../../Reference/im/MimRemap.md) performs a point-to-point linear remapping of pixel values, from the source range to the destination range, according to the specified mode. For the source range, you can specify to use the minimum and maximum values supported by the source image buffer ([`M_FIT_SRC_RANGE`](../../Reference/im/MimRemap.md)), or you can have Aurora Imaging Library find the pixel value extremes of the source image ([`M_FIT_SRC_DATA`](../../Reference/im/MimRemap.md)). In either case, source values are mapped to the destination buffer's specified range of possible values. To control the pixel value range of the source or destination buffer, use [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md).

When you use [`MimRemap`](../../Reference/im/MimRemap.md) with [`M_FIT_SRC_RANGE`](../../Reference/im/MimRemap.md), and the source buffer has data below the specified [`M_MIN`](../../Reference/buf/MbufControl.md) or above the specified [`M_MAX`](../../Reference/buf/MbufControl.md), this data will be mapped to unpredictable values. If the goal is to clip this data to the [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) values of the destination buffer, use [`MimClip`](../../Reference/im/MimClip.md) with [`M_OUT_RANGE`](../../Reference/im/MimClip.md) on the source buffer first to set the data outside the range to the specified source [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) values.

You can choose to keep the destination range centered to zero ([`M_CENTERED`](../../Reference/im/MimRemap.md)). In the centered case, the values are remapped such that the value zero in the source buffer maps to the value zero in the destination buffer.

You can use [`MimRemap`](../../Reference/im/MimRemap.md) to compress or expand an image to a different bit depth, since the bit depth can differ between the specified source and destination image buffers. For example, you can use [`MimRemap`](../../Reference/im/MimRemap.md) with [`M_FIT_SRC_RANGE`](../../Reference/im/MimRemap.md) to convert down to an 8-bit depth for display purposes or to be able to use analysis modules that only accept 8-bit input.

The table below shows the formulas used by [`MimRemap`](../../Reference/im/MimRemap.md), for both the centered and non-centered cases.

*[Image: MimRemap_Formulas.png]*

Note that, in the centered case, some exceptions to the multiplier calculation apply. If both SrcMax and DstMax are less than zero, the multiplier is `_DstMin_/_SrcMin_`. If both SrcMin and DstMin are greater than or equal to zero, the multiplier is `_DstMax_/_SrcMax_`.

[`MimRemap`](../../Reference/im/MimRemap.md) can also be used with a custom remap image processing context, previously allocated with [`MimAlloc`](../../Reference/im/MimAlloc.md). In this case, you can set bounds for the remap operation independent of the range of the source and destination buffer. For example, you can use [`MimControl`](../../Reference/im/MimControl.md) with [`M_SRC_START_VALUE`](../../Reference/im/MimControl.md) and [`M_SRC_END_VALUE`](../../Reference/im/MimControl.md) to set the source range. You can also set [`M_DST_RANGE`](../../Reference/im/MimControl.md) to [`M_USE_BUFFER`](../../Reference/im/MimControl.md) to ensure that the result is mapped to the range of the result buffer. If [`M_DST_START_VALUE`](../../Reference/im/MimControl.md) or [`M_DST_END_VALUE`](../../Reference/im/MimControl.md) are set outside of the range of the destination buffer, results that overflow or underflow will be set to the maximum or minimum value (respectively) that can be represented in the destination buffer ([`M_MAX`](../../Reference/buf/MbufControl.md) and [`M_MIN`](../../Reference/buf/MbufControl.md)).

> **Note:** [`MimRemap`](../../Reference/im/MimRemap.md) is not available with Aurora Imaging Library Lite.

### Window leveling using MimLutMap()

You can use a LUT when performing window leveling. In this case, use [`MimLutMap`](../../Reference/im/MimLutMap.md) after generating a LUT and associated ramp. You can choose specific intensity ranges within both the source and destination images.

The image below illustrates the effects of window leveling using [`MimLutMap`](../../Reference/im/MimLutMap.md).

*[Image: window_leveling_example1.png]*

To implement window leveling using [`MimLutMap`](../../Reference/im/MimLutMap.md), you must first allocate a LUT buffer using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) with a size equivalent to the maximum possible value in the source image. Generate a ramp for the LUT using [`MgenLutRamp`](../../Reference/gen/MgenLutRamp.md), specifying the [`StartValue`](../../Reference/gen/MgenLutRamp.md) and [`EndValue`](../../Reference/gen/MgenLutRamp.md) as the required range of the destination image buffer. Finally, map the source image buffer through the LUT using [`MimLutMap`](../../Reference/im/MimLutMap.md).

You can use window leveling if you want to improve the contrast in your images and highlight distinct areas. Stretching a particular range of values in an image will make some features of the image closely resemble other features or the background, which can be useful for analysis. You can also stretch a portion of an image's intensity range to see certain details (for example, in medical x-ray applications, various tissue types occupy different value ranges). The following image illustrates how you can highlight different features using window leveling:

*[Image: Window_leveling.png]*

The following code snippet demonstrates how to perform window leveling using a LUT.

> **Code example:** [userguide.image_processing.simple_window_leveling01](userguide.image_processing.simple_window_leveling01)

For a more interactive example, see _MdispWindowLeveling.cpp_. To run this and other examples, use Aurora Imaging Example Launcher.

### Window leveling using MimMorphic()

To perform window leveling on a pixel neighborhood basis, rather than point-to-point, use [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_LEVEL`](../../Reference/im/MimMorphic.md). This operation finds the minimum and maximum pixel values in a neighborhood, and then creates a linear mapping such that these map to the minimum and maximum possible values of the image buffer. It then replaces the center pixel of the neighborhood with its corresponding value in the mapping. Similar to using [`MimRemap`](../../Reference/im/MimRemap.md) with [`M_FIT_SRC_DATA`](../../Reference/im/MimRemap.md), the [`MimMorphic`](../../Reference/im/MimMorphic.md) operation is like an adaptive window leveling, since only local pixel extremes are calculated.

> **Note:** [`MimMorphic`](../../Reference/im/MimMorphic.md) is not available with Aurora Imaging Library Lite.

## Histogram equalization

A histogram equalization can be performed to obtain a more uniform distribution of the grayscale values in your image. For example, if the intensity distribution of an image results in a clump in one area of the grayscale range, there might be objects that are not easily distinguished because of their similarity in color. You might want to adjust the image's intensity distribution to solve this problem by giving it a more uniform ([`M_UNIFORM`](../../Reference/im/MimHistogramEqualize.md)) distribution, using [`MimHistogramEqualize`](../../Reference/im/MimHistogramEqualize.md).

*[Image: hist.png]*

The [`MimHistogramEqualize`](../../Reference/im/MimHistogramEqualize.md) function first generates a histogram of the source image buffer. The histogram and a selected density function are then used to calculate a transformation LUT. If the destination buffer is an image, the transformation LUT is applied to the source buffer to produce the destination image. If the destination buffer is an LUT, the transformation LUT is copied into the destination LUT; this LUT can then be used to enhance the source image, either permanently (using [`MimLutMap`](../../Reference/im/MimLutMap.md)) or upon display (using [`MdispLut`](../../Reference/disp/MdispLut.md)). The transformation LUT can also be applied directly to images as they are being grabbed; to do so, first associate the LUT buffer with the digitizer, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_LUT_ID`](../../Reference/dig/MdigControl.md) and then grab the image.

### Adaptive histogram equalization

For a more refined histogram equalization, use [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md), which enhances an image using a contrast limited adaptive histogram equalization (CLAHE). In this case, you must allocate an adaptive histogram equalization context, using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_HISTOGRAM_EQUALIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md).

> **Note:** Adaptive histogram equalization is not available with Aurora Imaging Library Lite.

Unlike a conventional histogram equalization ([`MimHistogramEqualize`](../../Reference/im/MimHistogramEqualize.md)), which generates a histogram for the entire image as a whole, [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) partitions the specified image into a set of rectangular sections of equal size, referred to as tiles, and calculates histograms for each one. The multiple histograms, along with the specified equalization operation, are then used to transform the image and produce an enhanced version of it. Though this can take slightly longer than a conventional histogram equalization, it can help minimize the over-amplification of noise and the potential for saturation, particularly in images with inconsistent lighting where some sections are significantly brighter than others.

*[Image: MimHistogramEqualizeAdaptive.png]*

By default, [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) uses 8 tiles along the image's X- and Y-direction. The specified image is therefore processed as 64 congruent tiles. To modify this behavior, use [`MimControl`](../../Reference/im/MimControl.md) with [`M_NUMBER_OF_TILES_X`](../../Reference/im/MimControl.md) and [`M_NUMBER_OF_TILES_Y`](../../Reference/im/MimControl.md).

Every tile of the image has its own histogram, and every histogram has a set of bins, each indicating the number of pixels with the same intensity. The maximum percentage of pixels that a bin can represent is limited, by default, to 1% of all the pixels in that tile. To modify the maximum percentage, use the [`M_CLIP_LIMIT`](../../Reference/im/MimControl.md) control. This essentially limits the contrast. Exceeding values are distributed evenly among the tile's other histogram bins.

You can modify the number of histogram bins for each tile, as well as the equalization operation to perform, using the [`M_HIST_SIZE`](../../Reference/im/MimControl.md) and [`M_OPERATION`](../../Reference/im/MimControl.md) control types, respectively. By default, [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) determines the number of histogram bins automatically according to the specified source image, and performs a uniform-type of equalization.

Theoretically, the equalization can process each pixel and its neighborhood for every tile. To avoid this lengthy calculation, [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) actually performs a type of interpolation between sample pixel regions of adjacent tiles to determine pixel value results, which significantly improves efficiency, while only negligibly diminishing the quality of the transformed image. For more information about this, and other specifics regarding CLAHE, refer to *Computer Vision, Graphics, and Image Processing: Adaptive Histogram Equalization and Its Variations* (S. M. Pizer, E. P. Amburn, J. D. Austin, et al., 1986, Elsevier).

The _HistogramEqualizeAdaptive.cpp_ example demonstrates how to perform an adaptive histogram equalization. To run/view this and other examples, use Aurora Imaging Example Launcher.
