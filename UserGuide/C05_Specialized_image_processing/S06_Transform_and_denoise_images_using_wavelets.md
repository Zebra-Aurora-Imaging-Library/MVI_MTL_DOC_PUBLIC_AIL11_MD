---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Specialized_image_processing
section: Transform_and_denoise_images_using_wavelets
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Specialized-image / Transform and denoise images using wavelets"
---

# Transform and denoise images using wavelets

[`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) uses wavelets to decompose or recompose the specified source (typically image data). To remove noise, [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) also uses wavelets.

Wavelets refer to mathematical algorithms that process data in both the space and frequency domain, offering a compromise between each. By using wavelets, [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) results can therefore represent space and frequency. This makes [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) especially effective at filtering images. It is typically useful for signal coding, signal denoising, data compression, and pattern recognition.

Transformations conventionally represent either space or frequency data. For example, calling [`MimTransform`](../../Reference/im/MimTransform.md)with an image can produce a precise frequency result, but it lacks spatial information. Drawing such a result looks like a signal, with no resemblance to the original source image. Drawing a wavelet transformation result resembles the original source image, since some spatial information is kept. The following illustration shows a [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and a [`MimTransform`](../../Reference/im/MimTransform.md)(FFT) result of a source image representing several lead stripes. You can tell which is the wavelet transformation, since it resembles the source.

*[Image: MimWaveletTransformCompareToMimTransform.png]*

Numerous types of wavelet results are available. The one above represents a high-frequency wavelet transformation (vertical direction). For more information about wavelet results, see [Results and drawings](S06_Transform_and_denoise_images_using_wavelets.md).

Note that the uncertainty principal in mathematics proves that representing data with the same precision in both space and frequency impossible. Wavelets are recognized as offering an effective balance between these two domains.

## Wavelet context

Both [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md)and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) require a wavelet image processing context, which you must allocate by calling [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md) or [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md). With [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md), you have access to predefined wavelet filters. With [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md), you must provide your own wavelet filters. Information about wavelets applies to both contexts unless otherwise specified.

The wavelet context stores the filter and mode used to perform [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md)and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md).

To inquire about what is stored in a wavelet context, call [`MimInquire`](../../Reference/im/MimInquire.md). For example, you can inquire whether the context's filter uses complex numbers ([`M_TRANSFORMATION_DOMAIN`](../../Reference/im/MimInquire.md)), as well as retrieve the identifier of the internal buffer that contains the actual filter values ([`M_FILTER_..._ID`](../../Reference/im/MimInquire.md)).

Before calling a wavelet operation, you should also allocate a wavelet result buffer, using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md). Remember to free all image processing buffers with [`MimFree`](../../Reference/im/MimFree.md).

### Filters

To set a predefined wavelet filter (only for [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md)), call [`MimControl`](../../Reference/im/MimControl.md) with [`M_WAVELET_TYPE`](../../Reference/im/MimControl.md). The default is [`M_HAAR`](../../Reference/im/MimControl.md). This typically applies when transforming or denoising images with distinct intensity transitions. Other filter types are categorized as Daubechies (generally appropriate for intensity discontinuities) or Symmetry (a mathematically more symmetric version of Daubechies), and the number of precision points (vanishing moments) the filter employs (for example, [`M_DAUBECHIES_3`](../../Reference/im/MimControl.md)). More vanishing moments typically result in the representation of more complex features, and longer processing time.

A further distinction between filters is whether they use complex or real coefficients. The mathematical domain of complex filters (for example,[`M_DAUBECHIES_3_COMPLEX`](../../Reference/im/MimControl.md)) consists of wavelet coefficients that have a real part (real numbers) and an imaginary part (imaginary numbers). The mathematical domain of filters that are not complex consist of wavelet coefficients that have real numbers only. Results that include imaginary numbers are generally longer to calculate, but have more information (representing the signal's amplitude and phase).

To set custom wavelet filters (only for [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md)), call [`MimWaveletSetFilter`](../../Reference/im/MimWaveletSetFilter.md) and specify the filter values. The custom wavelet context must contain your filter values before calling [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md)or [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md). The custom filters that you specify must be separable 2D wavelets that are factorized in their 1D form. For more information, see *A Wavelet Tour of Signal Processing* (Stéphane Mallat., 2008, Academic Press).

When calling the wavelet function ([`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md)or [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md)), you must set the number of levels (iterations) with which to apply the filter, up to an internally established maximum. This is the level at which calculations at subsequent levels do not result in relevant data. The maximum level depends on the wavelet function, the source, and the wavelet image processing context. If the specified level exceeds the maximum level, Aurora Imaging Library uses the maximum level. You can determine the actual number of levels used to produce wavelet results by calling [`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_NUMBER_OF_LEVELS`](../../Reference/im/MimGetResult.md).

For more information on wavelet filters, see *A Practical Guide to Wavelet Analysis* (Christopher Torrence and Gilbert P. Compo., 1998, AMS).

### Operation mode

Aurora Imaging Library uses filters in dyadic (default) or undecimated mode. In dyadic mode, processing involves sampling the wavelet coefficients by a factor of 2 at each level. For example, when drawing dyadic results, they are at different sizes, at different levels. Undecimated processing does not sample. The filter values themselves are adjusted. When drawing undecimated results, they are at the same size regardless of level.

Dyadic generally applies to signal coding and data compression. Undecimated applies more to signal denoising and pattern recognition. To modify the mode, set the [`M_TRANSFORMATION_MODE`](../../Reference/im/MimControl.md)control to [`M_DYADIC`](../../Reference/im/MimControl.md) or [`M_UNDECIMATED`](../../Reference/im/MimControl.md).

Performing [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) with [`M_DYADIC`](../../Reference/im/MimControl.md) is typically faster than [`M_UNDECIMATED`](../../Reference/im/MimControl.md), though the quality of denoising is usually higher with [`M_UNDECIMATED`](../../Reference/im/MimControl.md). In general, [`M_DYADIC`](../../Reference/im/MimControl.md) uses less resources (processing time and memory) than [`M_UNDECIMATED`](../../Reference/im/MimControl.md).

For more information about dyadic and undecimated wavelet modes, see *A Wavelet Tour of Signal Processing* (Stéphane Mallat., 2008, Academic Press).

## Wavelet transformations

[`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) can perform a forward ([`M_FORWARD`](../../Reference/im/MimWaveletTransform.md)) or reverse ([`M_REVERSE`](../../Reference/im/MimWaveletTransform.md)) wavelet transformation on the specified source. Results are written in the specified destination. A forward transformation decomposes the source, which can be an image or wavelet result; the destination must be a wavelet result. Reverse transformations recompose the source, which must be a wavelet result; the destination can be an image or wavelet result.

The basic methodology for performing wavelet transformations typically requires calling [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md)with an image and [`M_FORWARD`](../../Reference/im/MimWaveletTransform.md), and then using the result with your own specialized operations, typically related to signal coding, signal denoising, data compression, and pattern recognition. The last step is to transform those results using [`M_REVERSE`](../../Reference/im/MimWaveletTransform.md) and produce the modified image. Since you must use [`M_REVERSE`](../../Reference/im/MimWaveletTransform.md) with a wavelet result (not an image), you must have at some previous point used [`M_FORWARD`](../../Reference/im/MimWaveletTransform.md) with an image to produce a result.

### Results and drawings

To retrieve general types of wavelet results, call [`MimGetResult`](../../Reference/im/MimGetResult.md). For example, to determine the actual number of transformation levels used, get the [`M_NUMBER_OF_LEVELS`](../../Reference/im/MimGetResult.md)result.

To retrieve information about a specific wavelet result, use [`MimGetResultSingle`](../../Reference/im/MimGetResultSingle.md). To identify the individual result, you must specify the resulting wavelet coefficient and transformation level with [`M_DIAGONAL_LEVEL(Level)`](../../Reference/im/MimGetResultSingle.md), [`M_HORIZONTAL_LEVEL(Level)`](../../Reference/im/MimGetResultSingle.md), or [`M_VERTICAL_LEVEL(Level)`](../../Reference/im/MimGetResultSingle.md).

These results represent the image separated into its high-frequency components at a specific level and oriented along a specific direction. For example, if you specify [`M_DIAGONAL_LEVEL(3)`](../../Reference/im/MimGetResultSingle.md) with the [`M_WAVELET_COEFFICIENTS_IMAGE_ID`](../../Reference/im/MimGetResultSingle.md) result, you will retrieve the identifier of the internal image buffer that holds the high-frequency results oriented along the diagonal plane at the third level of transformation.

You can also call [`MimGetResultSingle`](../../Reference/im/MimGetResultSingle.md) with [`M_APPROXIMATION`](../../Reference/im/MimGetResultSingle.md) to retrieve results about the approximation (the low frequency rendition) of the wavelet transformation at the last calculated level.

To perform drawing operations with wavelet results, call [`MimDraw`](../../Reference/im/MimDraw.md). The entire content of the wavelet result is drawn.

For dyadic modes ([`MimControl`](../../Reference/im/MimControl.md)with [`M_TRANSFORMATION_MODE`](../../Reference/im/MimControl.md) set to [`M_DYADIC`](../../Reference/im/MimControl.md)), drawings are in the top-right (vertical coefficient), bottom-right (diagonal coefficient), and bottom-left (horizontal coefficient) corners of the display. This drawing pattern repeats for each level calculated ([`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_NUMBER_OF_LEVELS`](../../Reference/im/MimGetResult.md)). Since dyadic transformations sample wavelet coefficients by a factor of 2 per level, drawings are resized at each level. Aurora Imaging Library also draws the approximation (the low frequency rendition) of the wavelet transformation at the last level, in the top-left corner of the display.

The following is an example of drawing dyadic results (three level transformation).

*[Image: MimWaveletTransformResultDyadic.png]*

For undecimated modes ([`M_UNDECIMATED`](../../Reference/im/MimControl.md)), drawings are in one row, per level. Each row is split into three columns, representing the horizontal (left column), diagonal (middle column), and vertical (right column) wavelet coefficients for that level. Since undecimated transformations are not sampled, drawings are all the same size, regardless of level. Aurora Imaging Library also draws the approximation (the low frequency rendition) of the wavelet transformation at the last level, in the first column of the first row. The middle and right columns in this row are blank.

The following is an example of drawing undecimated results (three level transformation).

*[Image: MimWaveletTransformResultUndecimated.png]*

Depending on the specifics of your application, such as the source and destination image, context type, transformation mode, and filter type, calculations can require Aurora Imaging Library to internally add padding data to the image's border. [`MimDraw`](../../Reference/im/MimDraw.md) allows you to draw without ([`M_DRAW_WAVELET`](../../Reference/im/MimDraw.md)) or with ([`M_DRAW_WAVELET_WITH_PADDING`](../../Reference/im/MimDraw.md)) this padding. You can also retrieve related results with or without padding. For example, to get the width required to draw wavelet results, you can call [`MimGetResult`](../../Reference/im/MimGetResult.md)with [`M_WAVELET_DRAW_SIZE_X`](../../Reference/im/MimGetResult.md)or [`M_WAVELET_DRAW_SIZE_X_WITH_PADDING`](../../Reference/im/MimGetResult.md).

The _WaveletTransformation.cpp_ example performs a wavelet transformation and then displays the resulting wavelet transforms. To run this and other examples, use Aurora Imaging Example Launcher.

## Wavelet denoising

[`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) uses wavelet denoising techniques to remove noise from the specified source, which must be an image or wavelet result.

If the source is an image, [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) produces the actual denoised image in the specified destination. Since the destination is an image (not a result), you cannot perform result type operations with it, such as drawing the calculated wavelet coefficients using [`MimDraw`](../../Reference/im/MimDraw.md).

If the source is a result, it must come from [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md). The specified destination must also be a result, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md).

As previously discussed, the denoising process uses the filter type and mode indicated in the specified wavelet context. Specifically, Aurora Imaging Library decomposes the image according to the specified number of levels, performs the denoising, and recomposes the image using the same number of levels. The maximum level depends on the source image and the wavelet image processing context.

When calling [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md), you must also specify a specific wavelet shrinkage process: [`M_BAYES_SHRINK`](../../Reference/im/MimWaveletDenoise.md), [`M_NEIGH_SHRINK`](../../Reference/im/MimWaveletDenoise.md), or [`M_SURE_SHRINK`](../../Reference/im/MimWaveletDenoise.md).

[`M_BAYES_SHRINK`](../../Reference/im/MimWaveletDenoise.md) is good at minimizing Gaussian noise. For minimizing Mean Square Errors (MSE), [`M_SURE_SHRINK`](../../Reference/im/MimWaveletDenoise.md) is better. When noise is difficult to characterize, try NeighShrink. This setting mimics the general behavior of wavelet coefficients by taking neighborhood statistics and dismissing outliers. Though NeighShrink is less precise than the others, it can yield the best results under uncertain conditions. NeighShrink is usually the fastest setting.

If you are having difficulty denoising with [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md), try using some of Aurora Imaging Library's other denoising techniques, such as spatial filtering (in particular for removing salt-and-pepper noise). For more information, see [Denoise using spatial filtering and area open and close operations](../C03_Image_processing/S05_Denoising.md).

The following illustration shows a member of the Periphrastic family of near passenger birds (that is, a Toucan). The first image has no noise, while the second has white Gaussian noise.

*[Image: MimWaveletDenoiseComparisonSourceAndNoise.png]*

The first two images below show the removal of noise using median ranking and smoothing. The third image shows the removal of noise using [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md) with [`M_BAYES_SHRINK`](../../Reference/im/MimWaveletDenoise.md).

*[Image: MimWaveletDenoiseComparisonResultAndNoiseRemoved.png]*

To remove Gaussian noise, a BayesShrink wavelet process proves most effective.

For more information about wavelet denoising techniques, see *Wavelet Thresholding for Image DE-noising* (Rohit Sihag, Rakesh Sharma, and Varun Setia., 2011, IJCA Proceedings on International Conference on VLSI, Communications and Instrumentation (ICVCI) (14):20–24)and *Adapting to Unknown Smoothness via Wavelet Shrinkage* (David L. Donoho and Iain M. Johnstone., 1995, Journal of the American Statistical Association, Vol. 90, No. 432).

The _VariousDenoising.cpp_ example performs a wavelet denoising, and compares it with other conventional denoising operations, such as rank and spatial filtering. To run this and other examples, use Aurora Imaging Example Launcher.
