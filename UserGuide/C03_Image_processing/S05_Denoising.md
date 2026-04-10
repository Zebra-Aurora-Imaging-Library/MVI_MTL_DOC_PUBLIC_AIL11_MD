---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Denoising
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Denoising"
---

# Denoise using spatial filtering and area open and close operations

Spatial filtering and area open and close operations are effective ways to reduce noise. Spatial filtering operations determine each pixel's value based on its neighborhood values. They allow images to be separated into high-frequency and low-frequency components. There are two main types of spatial filters that can remove noise: low-pass linear filters and rank filters. Area open and close operations are efficient in removing salt-and-pepper noise in an image.

Spatial filtering and area open and close operations can reduce noise, but are not especially effective at removing mean square errors (MSE). To minimize MSE, try using [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md). For more information, see [Wavelet denoising](../C05_Specialized_image_processing/S06_Transform_and_denoise_images_using_wavelets.md). Note that to remove Gaussian noise, you can use several techniques, such as spatial filtering, wavelet denoising, or averaging a sequence of grabbed images (see[Averaging an input sequence](S04_Averaging_an_input_sequence.md)).

## Low-pass spatial linear filters

Low-pass spatial linear filters are effective in reducing Gaussian random noise (and high-frequency systematic noise), provided that the noise frequency is not too close to the spatial frequency of significant image data. These filters replace each pixel with a weighted sum of each pixel's neighborhood. Note, these filters have the side-effect of selectively smoothing your image and removing edge information. To reduce noise while also preserving edges, you can try using [`MimFilterAdaptive`](../../Reference/im/MimFilterAdaptive.md); for more information, see [Adaptive filters](S05_Denoising.md).

*[Image: BoardLowPassFilter.png]*

You can apply low-pass spatial linear filters using [`MimConvolve`](../../Reference/im/MimConvolve.md). Aurora Imaging Library provides a predefined low-pass linear filter called [`M_SMOOTH`](../../Reference/im/MimConvolve.md), which satisfies most applications (other predefined filters are also available). If you require more control over the filtering process, you can use [`MimConvolve`](../../Reference/im/MimConvolve.md) with a custom filter. For more information, see [Custom spatial filters](../C04_Advanced_image_processing/S02_Custom_spatial_filters.md).

## Binomial filter

The binomial filter is an approximation of a discrete Gaussian filter, used for smoothing 8-bit unsigned images. You can use [`MimFilterBinomial`](../../Reference/im/MimFilterBinomial.md) to apply a binomial filter to an image with a specified kernel size ([`KernelSizeX`](../../Reference/im/MimFilterBinomial.md) and [`KernelSizeY`](../../Reference/im/MimFilterBinomial.md)). A binomial filter with a 3X3 kernel is equivalent to using [`MimConvolve`](../../Reference/im/MimConvolve.md) with the [`M_SMOOTH`](../../Reference/im/MimConvolve.md) low-pass linear filter. The binomial filter is typically faster than other similar operations, such as adaptive filters ([`MimFilterAdaptive`](../../Reference/im/MimFilterAdaptive.md)) or a Vliet IIR filter ([`M_VLIET`](../../Reference/im/MimControl.md)), while preserving edges. For more information, see [Edge Enhancement](S14_Enhancing_and_detecting_edges.md).

*[Image: BoardBinomial.png]*

The kernel values of a binomial filter, approximate those of a Gaussian filter but only use integer values. This has the benefit of reducing the complexity of calculations since it does not require converting to a double or float and then back to an integer. Generally, the larger the kernel, the better the approximation of a Gaussian filter. For more information, see [Finite Impulse Response (FIR) filters](../C04_Advanced_image_processing/S02_Custom_spatial_filters.md).

## Rank filters

Rank-filter operations are more suitable for removing salt-and-pepper type noise since they replace each pixel with a pixel in its neighborhood rather than a weighted sum of its neighborhood. The weighted sum generally creates a blotchy effect around each noise pixel.

You can perform a rank-filter operation using [`MimRank`](../../Reference/im/MimRank.md). In most cases, it is best to use a rank that is half of the number of elements in the neighborhood. This effectively replaces each pixel with the median of the neighborhood and is therefore called a median filter. To perform a median filter, use [`MimRank`](../../Reference/im/MimRank.md) with [`M_MEDIAN`](../../Reference/im/MimRank.md). You will find that the median filter will most often suit your application needs.

*[Image: BoardRankFilter.png]*

For a rank filter operation that adapts to the content of the source image, while also preserving edge information, try using [`MimFilterAdaptive`](../../Reference/im/MimFilterAdaptive.md) with [`M_NOISE_PEAK_REMOVAL`](../../Reference/im/MimFilterAdaptive.md).

## Adaptive filters

To remove noise while preserving edge information, use [`MimFilterAdaptive`](../../Reference/im/MimFilterAdaptive.md) with one of its adaptive filter operations. These operations adapt to the content of the source image, The adaptive filter operations below use the following example source image:

*[Image: MimFilterAdaptive-Source.png]*

You can specify one of the following adaptive filter operations:

- A bilateral filter ([`M_BILATERAL`](../../Reference/im/MimFilterAdaptive.md)). This filter is based on spatial and intensity weights given to each pixel in a neighborhood.
  *[Image: MimFilterAdaptive-Bilateral.png]*
- A noise peak elimination filter ([`M_NOISE_PEAK_REMOVAL`](../../Reference/im/MimFilterAdaptive.md)). This filter iteratively replaces the central pixel in a 3x3 neighborhood, if the pixel is a minimum or maximum within that neighborhood, and differs from the replacement value by more than a specified threshold.
  *[Image: MimFilterAdaptive-NoisePeak.png]*
- A guided filter ([`M_GUIDED`](../../Reference/im/MimFilterAdaptive.md)). This filter separates image information into edges and homogenous areas. Two sub-images are created, one by blurring the homogenous areas of the image and the other containing the location of the edges. A new image is then constructed by combining these two sub-images with the original image.
  *[Image: MimFilterAdaptive-Guided.png]*
- A fast guided filter ([`M_FAST_GUIDED`](../../Reference/im/MimFilterAdaptive.md)). This filter separates image information into edges and homogenous areas. Two sub-images are created, one by blurring the homogenous areas of the image and the other containing the location of the edges. A new image is then constructed by combining these two sub-images with the original image. The fast guided filter improves the computational speed of the guided filter by sub-sampling the original image and then up-sampling before constructing the final image. This is at the cost of a slightly degraded result.
  *[Image: MimFilterAdaptive-FastGuided.png]*

All types of adaptive filters smooth an image while preserving edges as much as possible and adapt to the content of the source image, while allowing some attributes, such as kernel size or number of iterations, to be explicitly set for the filter operation.

Note that adaptive filters are slower than other similar operations, such as rank filters ([`MimRank`](../../Reference/im/MimRank.md)) and most of the filters available using [`MimConvolve`](../../Reference/im/MimConvolve.md). If edge preservation is less important than speed for your application, try the predefined or custom filters available in [`MimConvolve`](../../Reference/im/MimConvolve.md).

The _AdaptiveFiltering.cpp_ example shows [`M_BILATERAL`](../../Reference/im/MimFilterAdaptive.md), [`M_NOISE_PEAK_REMOVAL`](../../Reference/im/MimFilterAdaptive.md),[`M_GUIDED`](../../Reference/im/MimFilterAdaptive.md), and [`M_FAST_GUIDED`](../../Reference/im/MimFilterAdaptive.md) adaptive filtering on an image, and also shows a Deriche smoothing filter result (using [`MimConvolve`](../../Reference/im/MimConvolve.md)) for comparison. To run this example, use Aurora Imaging Example Launcher.

### Majority filter

You can use [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md) to replace pixels with the pixel value that appears most often in its neighborhood. Majority filter operations are more suitable for removing noise from images with large uniform areas because such images will likely have more than one pixel of the same value in each neighborhood. This is also useful, for example, to filter an index image (such as the segmentation map used in the Aurora Imaging Library Classification module). Filtering using an average does not make sense in this case. For example, if index 1 represents a dog, index 2 represents a monkey, and index 3 represents a cat, using an averaging filter would not produce the intended results. An averaging filter might change a pixel to index 2 (monkey) in a region with only indices 1 (dog) and 3 (cat), whereas [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md) would replace pixels with the most common pixel value in its neighborhood. Note that [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md) does not preserve edges. For more information about filtering index images such as depth maps, see [Retrieving and using depth-from-focus result](../C11_Registration/S04_Depth_from_focus.md).

## Area open and area close

To eliminate salt-and-pepper noise in your image, you can also perform an area open or area close operation, using [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_AREA_CLOSE`](../../Reference/im/MimMorphic.md) or [`M_AREA_OPEN`](../../Reference/im/MimMorphic.md). Area open can be used to eliminate small regions of light noise pixels, while area close can do the same to small regions of dark noise pixels.

To understand what the area open and area close operations do, it is useful to think of an image as a topographic surface with hills and valleys. The value of each pixel represents a certain height, with the lowest pixel value (the darkest pixel) representing the point of lowest elevation and the highest pixel value (the brightest pixel) representing the point of highest elevation. The area open operation clips the peaks of hills until each has a plateau with an area equal to or greater than the specified minimum area. If an area of this size never occurs, the peak will be clipped until all the pixels in the hill have the same intensity as the background pixels. The area close operation is similar to the area open operation, except the clipping of intensity levels begins at the lowest level. In this case, you can imagine the bottom of the basins being filled until they have a flat surface area equal to or greater than the specified minimum area.

*[Image: AreaOpen.png]*

To implement the area open operation, Aurora Imaging Library analyzes the image and clips off peaks one intensity level at a time, starting at the highest pixel intensity level (255 for an 8-bit image). It locks pixels at their current value if the specified minimum area is reached. At each intensity level, the following steps are performed:

1. Pixels with an intensity level greater than the current intensity level and that have not been previously locked are set to the current intensity level.
2. The algorithm then considers all pixels with a value equal to or greater than the current value to be foreground pixels, and connected foreground pixels as part of the same object.
3. The area of each object is compared to the specified minimum area. If it is greater or equal to the specified minimum area, the pixels in the object are locked at their current value for the remainder of the algorithm. The first pixels locked in any area form a plateau.
4. If the current intensity level is 0, the operation is complete. If not, the algorithm proceeds to the next lower intensity level, and returns to step 1.

In the event that the area of an object at every pixel intensity level never equals or exceeds the specified minimum area, its pixels will eventually be clipped to the background pixel intensity level.

Also, it is possible that distinct objects at one pixel intensity level merge together at a lower pixel intensity level. This situation is illustrated in the following images, which depict a few steps in the area open operation.

*[Image: AreaOpenPixels.png]*

As was mentioned earlier, an object is composed of connected foreground pixels. Determining which pixels are connected depends on the selected structuring element, [`M_3X3_RECT`](../../Reference/im/MimMorphic.md) or [`M_3X3_CROSS`](../../Reference/im/MimMorphic.md). These are the only two structuring elements that can be used with the area open and area close operations.
