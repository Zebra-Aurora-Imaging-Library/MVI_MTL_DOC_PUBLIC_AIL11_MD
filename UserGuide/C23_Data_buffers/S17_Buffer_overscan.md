---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Buffer_overscan
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Buffer overscan"
---

# Buffer overscan region

Buffer overscan pixels are pixels added around an image. Aurora Imaging Library neighborhood operations, such as [`MimConvolve`](../../Reference/im/MimConvolve.md) or [`MimFilterBinomial`](../../Reference/im/MimFilterBinomial.md), use these pixels to process the border pixels of an image since these pixels don't have a complete neighborhood.

*[Image: overscan_2.png]*

For example, in the image above, if [`MimConvolve`](../../Reference/im/MimConvolve.md) is called with the [`M_HORIZONTAL_EDGE_PREWITT`](../../Reference/im/MimConvolve.md) FIR filter (which operates on 3 by 3 neighborhoods), the neighborhoods of the image border pixels would be incomplete. In such cases, overscan pixels might be used.

> **Note:** Note that Aurora Imaging Library recursive neighborhood operations ignore the values of overscan pixels.

To set the values of overscan pixels, use [`MbufControl`](../../Reference/buf/MbufControl.md)with [`M_OVERSCAN`](../../Reference/buf/MbufControl.md). However, some Aurora Imaging Library neighborhood operations with predefined kernels or structuring elements do not require that you set the values of overscan pixels. For example, if you call [`MimErode`](../../Reference/im/MimErode.md), the overscan pixels are automatically set to the highest possible buffer value, which will produce the most accurate possible results for the image border pixels.

Set the number of overscan pixels that should be added to an image buffer during allocation, using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_ALLOCATION_OVERSCAN_SIZE`](../../Reference/sys/MsysControl.md). By default, the size of the overscan is 8 pixels. The specified size represents the number of rows/columns added to each side of the image buffer. For example, in the image above, the size of the overscan is 2. Each buffer allocated on a particular system will have the same overscan size. You can inquire the size of the overscan using either the [`MbufInquire`](../../Reference/buf/MbufInquire.md) or [`MsysInquire`](../../Reference/sys/MsysInquire.md) function.

In cases where the overscan is not sufficient for the size of the kernel, two separate buffers are used to handle the overscans (one for left/right and one for top/bottom). Note that if the buffers are too small to use Intel's single-instruction, multiple-data (SIMD) technologies, the results of the neighborhood operation will not be saturated, unless the operation forces saturation. Allocating two buffers for a neighborhood operation is slow, so it is recommended that you instead increase the size of the overscan to suit the kernel. Note that increasing the overscan size increases the pitch of the buffer, which might slightly degrade the performance of the neighborhood operation.

When the results of operations performed on image border pixels are not important, and you want to speed up your application, you can use the [`MbufControl`](../../Reference/buf/MbufControl.md) function to disable the use of overscan pixels. In addition, several functions allow you to disable the use of overscan pixels for neighborhood operations.

## Overscan operation with multiple iterations

To control how the overscan operation is handled when calling [`MimMorphic`](../../Reference/im/MimMorphic.md) with a custom structuring element and specifying multiple iterations, use [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md).

> **Note:** Note that you must still set the type of overscan used to handle border pixels of a source image, using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_OVERSCAN`](../../Reference/buf/MbufControl.md).

[`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md) sets how to handle the overscan operations when there are multiple iterations. By default, [`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md) is set to[`M_PER_ITER`](../../Reference/buf/MbufControl.md), where the number of overscan pixels and their values are established per iteration according to the structuring element's size and center pixel. You can also set [`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md) to [`M_GLOBAL`](../../Reference/buf/MbufControl.md) to establish the number of overscan pixels and their value at the beginning of the first iteration, based on all the iterations that need to be performed. For example, if using a transparent overscan and performing 5 iterations with a 3x3 structuring element, 5 overscan pixels of the ancestor buffer will be used on each side (instead of 1).

For some [`MimMorphic`](../../Reference/im/MimMorphic.md) operations, you can combine the iterations into a larger equivalent structuring element using [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_ITER_COMBINED`](../../Reference/im/MimMorphic.md). In this case, [`MimMorphic`](../../Reference/im/MimMorphic.md) is essentially performed with one iteration but with a larger structuring element, so [`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md) does not have an effect.

For example, given the following image:

*[Image: Overscan_iterations_1.png]*

The following example images are transformed with an [`M_DILATE`](../../Reference/im/MimMorphic.md) morphological operation; an [`M_TRANSPARENT`](../../Reference/buf/MbufControl.md) type of overscan is used to establish how overscan pixels are handled. In the example images, the presence of a white border helps visualize that the overscan pixel values across all iterations are handled according to the value set by[`M_OVERSCAN`](../../Reference/buf/MbufControl.md). The following explains what occurs given the different options when performing the operation iteratively:

- A default Aurora Imaging Library overscan operation with multiple iterations,where [`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md) is set to [`M_PER_ITER`](../../Reference/buf/MbufControl.md). The result contains no white border since the ancestor image is used only for the first iteration ([`M_TRANSPARENT`](../../Reference/buf/MbufControl.md)). The following iterations are mirrors ([`M_MIRROR`](../../Reference/buf/MbufControl.md)) of the result of the previous iteration. That is, the overscan pixel values will be a mirror copy of the previous iteration's resulting buffer borders due to limitations imposed by the buffer's size.
  *[Image: Overscan_iterations_2.png]*
- A combined iteration operation. In this case, [`MimMorphic`](../../Reference/im/MimMorphic.md) is called with [`M_DILATE`](../../Reference/im/MimMorphic.md) +[`M_ITER_COMBINED`](../../Reference/im/MimMorphic.md). The result contains a white border since the iterations are combined and replaced by a single larger equivalent structuring element. In this case, since there is only one iteration, the overscan only occurs once meaning[`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md) has no affect.
  *[Image: Overscan_iterations_3.png]*
- A global overscan with multiple iterations, where [`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md) is set to [`M_GLOBAL`](../../Reference/buf/MbufControl.md). The result contains a white border since an overscan size equivalent to all iterations is used for each of the individual iterations.
  *[Image: Overscan_iterations_4.png]*

As shown, the default Aurora Imaging Library overscan gives a different result than when combining the iterations of a [`MimMorphic`](../../Reference/im/MimMorphic.md) operation with[`M_ITER_COMBINED`](../../Reference/im/MimMorphic.md), or when using an overscan size equivalent to all iterations ( [`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md) set to [`M_GLOBAL`](../../Reference/buf/MbufControl.md)). The execution time of these morphological transformations is relative to the structuring element size and number of iterations; depending of each of these factors, one mode can be faster than another.

The _mimmorphiciterationoverscan.cpp_ example shows the use of [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_ITER_COMBINED`](../../Reference/im/MimMorphic.md), as well using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_ITER_OVERSCAN`](../../Reference/buf/MbufControl.md) set to [`M_GLOBAL`](../../Reference/buf/MbufControl.md). This example also shows the visual differences as well as benchmarking the execution time for each operation type.

To run this and other examples, use Aurora Imaging Example Launcher.
