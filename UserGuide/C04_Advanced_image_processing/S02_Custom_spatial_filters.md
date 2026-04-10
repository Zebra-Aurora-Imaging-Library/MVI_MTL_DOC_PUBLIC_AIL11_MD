---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_image_processing
section: Custom_spatial_filters
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-image / Custom spatial filters"
---

# Custom spatial filters

Spatial filtering operations include operations such as smoothing, denoising, edge enhancement, and edge detection.

Spatial filtering operations compute results based on an underlying neighborhood process: the weighted sum of a pixel value and its neighbors' values. There are two types of spatial filters: Finite Impulse Response (FIR) filters and Infinite Impulse Response (IIR) filters. FIR filters operate on a finite neighborhood, while IIR filters take into account all values in an image. In Aurora Imaging Library, you can specify either a predefined FIR spatial filter or a custom FIR or IIR spatial filter.

To apply custom FIR filters, you use [`MimConvolve`](../../Reference/im/MimConvolve.md) or [`MimFilterBinomial`](../../Reference/im/MimFilterBinomial.md). To apply custom IIR filters, you use [`MimConvolve`](../../Reference/im/MimConvolve.md) or [`MimDifferential`](../../Reference/im/MimDifferential.md), depending on the filter operation.

## Finite Impulse Response (FIR) filters

For FIR filters, the weights are known as the kernel values. These kernel values determine the operation type of the spatial filter. For example, applying the following FIR filter results in a sharpening of the image:

*[Image: kernel1.png]*

Whereas, applying the following FIR filter smooths an image (it also increases the intensity of the image by a factor of 16, so you will need to normalize the convolution result):

*[Image: kernel2.png]*

For smoothing operations on 8-bit unsigned images that require larger or rectangular kernels, you can use [`MimFilterBinomial`](../../Reference/im/MimFilterBinomial.md) to approximate a Gaussian filter with custom kernel dimensions. For more information, see [Binomial filter](../C03_Image_processing/S05_Denoising.md).

When using FIR filters, the dimensions of the kernel determine the size of the neighborhood that is used in the operation. The result of the operation is stored in the destination buffer at the location corresponding to the kernel's center pixel. When the kernel has an even number of rows and/or columns, the center pixel is considered to be the top-left pixel of the central elements in the neighborhood.

*[Image: struc_el.png]*

Calculate the X-coordinate of the top-left pixel of the central elements, as follows:

- If the width (SizeX) of the kernel is an odd number, the X-coordinate is `(SizeX-1)/2`.
- If the width (SizeX) of the kernel is an even number, the X-coordinate is `(SizeX/2)-1`.

To calculate the Y-coordinate of the top-left pixel of the central elements, the same rules apply.

Regardless of the location of the center pixel, there will be some border pixels that have an incomplete neighborhood. To deal with this issue, the image buffer is overscanned. There are several types of overscan. A transparent overscan uses the parent buffer to provide the overscan pixels needed for the border calculation. If the parent buffer is not available, a mirror overscan is performed. A mirror overscan specifies that the overscan pixels will be a mirror copy of the source buffer's border pixels. A replicate overscan specifies to repeat border pixel values along each row and column of the overscan region. A replacement overscan allows you to specify a specific value for the overscan pixel values during processing. It is recommended that you experiment to find the best overscan option for your application. For more information on overscan, see [Buffer overscan region](../C23_Data_buffers/S17_Buffer_overscan.md).

If the predefined FIR filters provided by [`MimConvolve`](../../Reference/im/MimConvolve.md) do not meet your requirements, you can create your own custom FIR filter. Before resorting to a custom filter, if speed is not an issue, try the bilateral adaptive filter [`M_BILATERAL`](../../Reference/im/MimFilterAdaptive.md), available using [`MimFilterAdaptive`](../../Reference/im/MimFilterAdaptive.md), which uses a FIR filter. For more information, see [Adaptive filters](../C03_Image_processing/S05_Denoising.md).

To define your own custom FIR filter:

1. Allocate a kernel buffer, using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md). The kernel size is specified when allocating the kernel buffer. Note that the kernel size can be constrained by the available resources.
2. Load the required kernel values into this kernel buffer, using [`MbufPut`](../../Reference/buf/MbufPut.md) or [`MbufPut2d`](../../Reference/buf/MbufPut2d.md).
3. If required, modify the setting of the operation control types associated with your custom filter, using [`MbufControl`](../../Reference/buf/MbufControl.md). The operation control types determine how the convolution operation will be performed. You can control:
   - Whether or not the absolute value of the result is taken ([`M_ABSOLUTE_VALUE`](../../Reference/buf/MbufControl.md)).
   - The division (normalization) factor to apply to the result ([`M_NORMALIZATION_FACTOR`](../../Reference/buf/MbufControl.md)).
   - Whether or not to saturate the result ([`M_SATURATION`](../../Reference/buf/MbufControl.md)).
   - The position of the center pixel ([`M_OFFSET_CENTER_X`](../../Reference/buf/MbufControl.md) and [`M_OFFSET_CENTER_Y`](../../Reference/buf/MbufControl.md)).
   - How the operation handles the borders (overscan) of the source buffer ([`M_OVERSCAN`](../../Reference/buf/MbufControl.md) and [`M_OVERSCAN_REPLACE_VALUE`](../../Reference/buf/MbufControl.md)). If overscan is disabled, the border pixels of the source image are not processed if additional processing time is needed.
     *[Image: over.png]*

To apply your own custom FIR filter, call the [`MimConvolve`](../../Reference/im/MimConvolve.md) function, specifying the identifier of the required kernel buffer ([`FilterContextImOrKernelBufId`](../../Reference/im/MimConvolve.md)).

To increase the speed of the convolution operation when using your custom FIR filter, [`MimConvolve`](../../Reference/im/MimConvolve.md) will automatically separate large kernels, if separable, into two 1-dimensional kernels (`a<sub>ij</sub> = h<sub>i</sub>v<sub>j</sub>`). Performing two separate convolutions, once with `H<sub>mx1</sub>` and once with `V<sub>1xn</sub>`, can be faster and is equivalent to performing one convolution operation using the original `A<sub>mxn</sub>` kernel. Aurora Imaging Library will internally separate large kernels when it detects that the separation results in better performance. The following displays an `A<sub>mxn</sub>` kernel separated into two 1-dimensional kernels (`H<sub>m</sub>` and `V<sub>n</sub>`).

*[Image: sepkernel.png]*

## Infinite Impulse Response (IIR) filters

When using IIR filters, the weights are automatically determined by the type of filter, the mode of the filter, the type of operation to perform, and the degree of smoothness (strength of denoising) applied by the filter.

The type of filter determines the distribution of the neighborhoods' influence. Aurora Imaging Library supports three types of IIR spatial filters: Deriche filter, Vliet filter, and Shen-Castan filter. The Deriche and Vliet filters are accurate approximations of the Gaussian function, with the Vliet filter being more accurate for a large sigma coefficient ([`MimControl`](../../Reference/im/MimControl.md) with [`M_FILTER_SMOOTHNESS`](../../Reference/im/MimControl.md)). The Shen-Castan filter, on the other hand, is an accurate approximation of the Exponential function.

*[Image: GaussianvsExponentialFilters.png]*

For the Deriche and Vliet filters, the neighborhoods' influence decreases much slower as the distance from the central pixel increases, compared to the Shen-Castan filter. For more information on the Deriche and Shen-Castan filters in the context of edge detection, see [Customizing the edge extraction settings](../C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

To define and use a custom IIR filter:

1. Allocate a linear IIR filter context, using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md).
2. Use [`MimControl`](../../Reference/im/MimControl.md) and specify appropriate [`M_FILTER_TYPE`](../../Reference/im/MimControl.md), [`M_FILTER_OPERATION`](../../Reference/im/MimControl.md), [`M_FILTER_RESPONSE_TYPE`](../../Reference/im/MimControl.md), [`M_FILTER_SMOOTHNESS_TYPE`](../../Reference/im/MimControl.md), and [`M_FILTER_SMOOTHNESS`](../../Reference/im/MimControl.md) operation control type settings.
3. Use [`MimConvolve`](../../Reference/im/MimConvolve.md) to specify the source and destination image buffers, as well as to specify a linear IIR filter context. Alternatively, use [`MimDifferential`](../../Reference/im/MimDifferential.md) to specify the source and destination image buffers for specific IIR operations that require more than one convolution.

The following code snippet performs a smoothing operation using a Deriche custom IIR filter:

> **Code example:** [userguide.image_processing.smoothing_with_custom_iir_filter](userguide.image_processing.smoothing_with_custom_iir_filter)

This second snippet performs first derivative calculations:

> **Code example:** [userguide.image_processing.derivatives_with_custom_iir_filter](userguide.image_processing.derivatives_with_custom_iir_filter)

## An example

The _MImConvolve.cpp_ example shows a spatial filtering operation using a custom FIR filter with a 3 by 3 kernel. To run/view this and other examples, use Aurora Imaging Example Launcher. You can also view this example here:

> **Code example:** [MImConvolve.cpp](MImConvolve.cpp)
