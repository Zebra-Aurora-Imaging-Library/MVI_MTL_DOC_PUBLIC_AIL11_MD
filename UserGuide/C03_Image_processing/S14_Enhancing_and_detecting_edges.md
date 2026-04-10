---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Enhancing_and_detecting_edges
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Enhancing and detecting edges"
---

# Enhancing and detecting edges

Many applications perform various edge operations on images to increase the quality of the image or to limit some other operation on the image.

In general, edges can be established from intensity transitions between two or more adjacent pixels in an image. Horizontal edges are created when horizontally connected pixels have values that are different from those immediately above or below them. Vertical edges are created when vertically connected pixels have values that are different from those immediately to the left or right of them. Oblique edges are created from a combination of horizontal and vertical components.

There are three main categories of edge operations:

- Operations that enhance edges to sharpen the image.
- Operations that detect edges in the image.
- Operations that extract edges in the image.

These edge operations are performed using convolutions (or neighborhood operations that replace each pixel with a weighted sum of each pixel's neighborhood). The weights applied to the neighborhood determine the type of operation that is performed. For example, certain weights produce a horizontal edge detection, while others produce a vertical one.

For the first two types of edge operations, the weights are specified using either a Finite or Infinite Impulse Response (FIR or IIR) filter. FIR filters operate on a finite neighborhood to calculate the value of a pixel. IIR filters take into account all values in an image. The [`MimConvolve`](../../Reference/im/MimConvolve.md) function offers predefined FIR filters for most common edge enhancement and detection operations.

If the predefined filters do not meet your needs, you can define a custom edge enhancement or detection FIR filter or a custom edge detection IIR filter to use with [`MimConvolve`](../../Reference/im/MimConvolve.md). You can also use a custom edge enhancement IIR filter to use with [`MimDifferential`](../../Reference/im/MimDifferential.md); this function combines multiple convolutions to perform the edge enhancing operation.

To define a custom FIR filter, allocate a kernel buffer using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) and load the kernel buffer with values, using [`MbufPut`](../../Reference/buf/MbufPut.md). To define a custom IIR filter, allocate a linear IIR filter image processing context, using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md), and then specify the appropriate operation control type settings using [`MimControl`](../../Reference/im/MimControl.md) with [`Linear_IIR_filter_context_ID`](../../Reference/im/MimControl.md). For more information on custom filters, see [Custom spatial filters](../C04_Advanced_image_processing/S02_Custom_spatial_filters.md).

For information on edge extraction operations, see [Aurora Imaging Library Edge Finder module](../C10_Edge_Finder/S01_Edge_Finder_module.md).

## Edge enhancement

Edge enhancement operations amplify edges, accentuating details in the image. Note that these operations might not produce good results for further processing because when you enhance edges, you also enhance noise pixels.

### Sharpening edge enhancement

Sharpening edges can be useful to enhance edges in the image while preserving other parts of the image. You can obtain approximately the same result by performing a Laplacian-based edge detection operation on the image and adding the found edges to the original image.

To sharpen edges in an image, you can use [`MimConvolve`](../../Reference/im/MimConvolve.md) with the [`M_SHARPEN_8`](../../Reference/im/MimConvolve.md) or [`M_SHARPEN_4`](../../Reference/im/MimConvolve.md) predefined FIR filters.

When using predefined FIR filters, the [`M_SHARPEN_8`](../../Reference/im/MimConvolve.md) filter tends to produce more enhanced or sharpened edges than the [`M_SHARPEN_4`](../../Reference/im/MimConvolve.md) filter.

You can also sharpen edges in an image using [`MimDifferential`](../../Reference/im/MimDifferential.md) with the [`Operation`](../../Reference/im/MimDifferential.md) parameter set to [`M_SHARPEN`](../../Reference/im/MimDifferential.md).

*[Image: BoardEdgeEnhance.png]*

### Filtering false edges

Applying spacial filters can be useful to filter out false edges in an image (for example, a spurious edge due to noise) while preserving major foreground edges.

To filter edges in an image, you can use [`MimFilterBinomial`](../../Reference/im/MimFilterBinomial.md). Generally, binomial filters with larger kernel sizes filter out more background and minor edges than binomial filters with smaller kernels. The following image, edge detection reveals preserved major edges on the foreground object after using [`MimFilterBinomial`](../../Reference/im/MimFilterBinomial.md).

*[Image: BoardEdgeBinomial.png]*

For more information, see [Binomial filter](S05_Denoising.md).

## Edge detection

Edge detection operations reveal intensity transitions in the image. The smoother the image, the more gradual the change in intensity, and the weaker the detection will be.

### Horizontal and vertical edge detection

Detecting the horizontal and vertical edges in the image can be useful to enhance edges in a certain direction and remove those in another.

To detect the horizontal edges, you can use [`MimConvolve`](../../Reference/im/MimConvolve.md) with the [`M_HORIZONTAL_EDGE_PREWITT`](../../Reference/im/MimConvolve.md) or [`M_HORIZONTAL_EDGE_SOBEL`](../../Reference/im/MimConvolve.md) predefined FIR filters. To detect the vertical edges, you can use [`MimConvolve`](../../Reference/im/MimConvolve.md) with the [`M_VERTICAL_EDGE_PREWITT`](../../Reference/im/MimConvolve.md)or [`M_VERTICAL_EDGE_SOBEL`](../../Reference/im/MimConvolve.md) predefined FIR filters.

Alternatively, you can use custom IIR filters with [`MimControl`](../../Reference/im/MimControl.md) to detect edges. To detect horizontal edges, you can use [`M_FIRST_DERIVATIVE_Y`](../../Reference/im/MimControl.md). To detect vertical edges, you can use [`M_FIRST_DERIVATIVE_X`](../../Reference/im/MimControl.md).

*[Image: Boardhorizver.png]*

### Laplacian-based edge detection

The Laplacian-based operations place emphasis on the maximum values, or peaks, within the image. The edge representation of the resulting image generally looks very similar to the actual image.

To detect the Laplacian-based edges from an image, you can use [`MimConvolve`](../../Reference/im/MimConvolve.md) with the [`M_LAPLACIAN_4`](../../Reference/im/MimConvolve.md) or [`M_LAPLACIAN_8`](../../Reference/im/MimConvolve.md) predefined FIR filters.

When using predefined FIR filters, the [`M_LAPLACIAN_8`](../../Reference/im/MimConvolve.md) filter tends to produce more enhanced or sharpened edges than the [`M_LAPLACIAN_4`](../../Reference/im/MimConvolve.md) filter.

You can also detect the Laplacian-based edges from an image using [`MimDifferential`](../../Reference/im/MimDifferential.md) with the[`Operation`](../../Reference/im/MimDifferential.md) parameter set to [`M_LAPLACIAN`](../../Reference/im/MimDifferential.md).

*[Image: BoardLapEdge.png]*

### Gradient-based edge detection

When performing a gradient-based edge detection operation, edges are determined from the rate of change between pixel values in the image, without regard to the direction of the edges. The resulting image contains only positive values.

To detect the gradient-based edges from an image, you can use [`MimConvolve`](../../Reference/im/MimConvolve.md) with the [`M_EDGE_DETECT_SOBEL_FAST`](../../Reference/im/MimConvolve.md) or [`M_EDGE_DETECT_PREWITT_FAST`](../../Reference/im/MimConvolve.md) predefined FIR filters.

You can also detect the gradient-based edges from an image using [`MimDifferential`](../../Reference/im/MimDifferential.md) with the [`Operation`](../../Reference/im/MimDifferential.md) parameter set to [`M_GRADIENT`](../../Reference/im/MimDifferential.md) or [`M_GRADIENT_SQR`](../../Reference/im/MimDifferential.md).

*[Image: BoardGradEdge.png]*

For an advanced gradient-based edge detection operation, you can use [`MimEdgeDetect`](../../Reference/im/MimEdgeDetect.md). This function produces a gradient intensity image and/or a gradient angle image.
