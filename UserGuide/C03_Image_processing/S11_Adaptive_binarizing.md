---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Adaptive_binarizing
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Adaptive binarizing"
---

# Adaptive binarizing

To perform an adaptive binarization, call [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md). Similar to a conventional binarization ([`MimBinarize`](../../Reference/im/MimBinarize.md)), an adaptive binarization reduces an image to two grayscale values (such as white and black) by comparing each source pixel value against its corresponding threshold value. Pixels greater than their threshold are set to the highest unsigned destination buffer value. For example, in an 8-bit destination buffer, the highest value is 0xFF (255). Pixels less than or equal to their corresponding threshold are set to 0. This process can help identify pixels that are part of the object (pixels set to the highest value) and pixels that aren't (pixels set to 0).

[`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) establishes threshold values according to the threshold mode control ([`MimControl`](../../Reference/im/MimControl.md) with [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md)). Available threshold modes refer to adaptive algorithms, which perform multiple calculations using numerous sections of an image. When compared to a conventional binarization, which calculates threshold values using an image as a whole, an adaptive binarization is more robust at processing images containing pixels that are difficult to identify. Such difficulties typically come from poor or varying illumination or contrast. This type of robustness can cause an adaptive binarization to take longer to execute than a conventional one.

The following illustration shows that an adaptive binarization can better identify a crack in a cup, when compared to a conventional binarization, given poor lighting and contrast.

*[Image: MimBinarizeAdaptiveVersusConventional.png]*

In this source image, pixel intensities transition from black (background) to gray (cup) to white (crack) to less white (reflectance on cup). Despite these complex shifts in intensity, an adaptive binarization is able to discern which pixels do not belong to the general quality of the image (the crack). Conventional binarizations are typically less robust at handling such intensity shifts.

The _MimBinarizeAdaptive.cpp_ example illustrates adaptive binarizations, and also compares them with conventional binarizations. To run/view this and other examples, use Aurora Imaging Example Launcher.

By default, Aurora Imaging Library binzarizes objects that are lighter than the background. To binarize objects that are darker than the background, set the [`M_FOREGROUND_VALUE`](../../Reference/im/MimControl.md) control to [`M_FOREGROUND_BLACK`](../../Reference/im/MimControl.md). You would change the default to, for example, binarize a black cup on a white background.

## Adaptive binarize contexts

When you call [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md), you must specify an adaptive binarize type of image processing context, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md)with [`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md) or [`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md).

Although both contexts perform an adaptive binarization, the threshold algorithms available for an [`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md)context type use seeds in its calculations. Seeds refer to positions of significance, in the source, that affect resulting threshold values, so required information is preserved. You can either specify your own seed images or let Aurora Imaging Library determine the seeds. [`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md) typically applies when there is some foreknowledge about how images should look. Without such information, an [`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md) type of context is usually best.

Customizing an adaptive binarization generally requires changing the context's threshold mode, and its related controls, using [`MimControl`](../../Reference/im/MimControl.md). Available controls typically depend on the type of the context. Use [`MimInquire`](../../Reference/im/MimInquire.md) to inquire the control's settings.

Some threshold modes use morphological processes, such as erode and dilate or open and close. For more information, see [Erosion and dilation](S06_Erosion_and_dilation.md) and [Opening and closing](S07_Opening_and_closing.md).

After Aurora Imaging Library establishes the threshold values, you can adjust them for binarization, using [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md). For example, if you set [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md) to 1, Aurora Imaging Library increases all threshold values by 1, and then binarizes the source image. The specified offset is reflected in the threshold destination image, which you can retrieve using [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) and the [`ThresholdImageBufId`](../../Reference/im/MimBinarizeAdaptive.md) parameter. The default global offset is 0 (no adjustment).

For an adaptive binarize context that does not use seeds, you can use [`M_GLOBAL_MIN`](../../Reference/im/MimControl.md) and [`M_GLOBAL_MAX`](../../Reference/im/MimControl.md) to specify a minimum and maximum threshold value with which to binarize. The threshold destination image will not hold values that surpass these limits. Exceeding values are clipped. By default, Aurora Imaging Library binarizes pixels with an intensity higher than the maximum as part of the foreground (object). To change this behavior, use the [`M_FOREGROUND_VALUE`](../../Reference/im/MimControl.md) control. By default, there is no maximum threshold value restriction.

For any adaptive binarize context type, the source image can have a minimum or maximum value restriction ([`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MIN`](../../Reference/buf/MbufControl.md) or [`M_MAX`](../../Reference/buf/MbufControl.md)). These limits are reflected in the resulting threshold destination image. If the source image has a minimum or maximum value, and you are using [`M_GLOBAL_MIN`](../../Reference/im/MimControl.md) or [`M_GLOBAL_MAX`](../../Reference/im/MimControl.md)(for an adaptive binarize context that does not use seeds), Aurora Imaging Library uses the greater minimum value, or the lesser maximum value, as the actual limits.

## Controlling an adaptive binarize context

To control how Aurora Imaging Library establishes the threshold values for an [`M_BINARIZE_ADAPTIVE_CONTEXT`](../../Reference/im/MimAlloc.md) context type, you can set [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md) to [`M_NIBLACK`](../../Reference/im/MimControl.md), [`M_LOCAL_MEAN`](../../Reference/im/MimControl.md), [`M_BERNSEN`](../../Reference/im/MimControl.md), or [`M_PSEUDOMEDIAN`](../../Reference/im/MimControl.md).

You can specify to use a hysteresis process by setting [`M_THRESHOLD_TYPE`](../../Reference/im/MimControl.md) to [`M_HYSTERESIS`](../../Reference/im/MimControl.md). For more information, see [Hysteresis](S11_Adaptive_binarizing.md).

### Threshold with Niblack

By default, Aurora Imaging Library uses Niblack's adaptive threshold algorithm ([`M_NIBLACK`](../../Reference/im/MimControl.md)). This setting offers the highest precision. The processing time is usually quite quick.

The following represents the general logic for calculating threshold values using Niblack.

- If ([`M_NIBLACK_BIAS`](../../Reference/im/MimControl.md) x _StandardDeviationToLocalMean_) &lt; [`M_MINIMUM_CONTRAST`](../../Reference/im/MimControl.md):
  - _ThresholdValue_ = [`M_GLOBAL_MIN`](../../Reference/im/MimControl.md).
- Otherwise:
  - _ThresholdValue_ = _LocalMean_+ ([`M_NIBLACK_BIAS`](../../Reference/im/MimControl.md) x _StandardDeviationToLocalMean_) + [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md).

To specify the size of the neighborhood around each source pixel with which to calculate the local mean (_LocalMean_) and the standard deviation (_StandardDeviationToLocalMean_), use [`M_LOCAL_DIMENSION`](../../Reference/im/MimControl.md). The size should be set to the largest square that represents a uniform background, and is bigger than the expected thickness of the object.

[`M_MINIMUM_CONTRAST`](../../Reference/im/MimControl.md)represents the minimum distance between a pixel and the local mean, for [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) to consider the pixel a part of the object. [`M_NIBLACK_BIAS`](../../Reference/im/MimControl.md) gives you some global control over the thresholding. A higher bias classifies fainter values as part of the object; as a result, noise can be more likely classified as part of the object. A lower bias is more likely to ignore noise, though it can also ignore values you consider part of the object.

### Threshold with local mean

A local mean adaptive threshold algorithm ([`M_LOCAL_MEAN`](../../Reference/im/MimControl.md)) is a simplified version of Niblack ([`M_NIBLACK`](../../Reference/im/MimControl.md)). The differences are: [`M_NIBLACK_BIAS`](../../Reference/im/MimControl.md) must be 0 and [`M_MINIMUM_CONTRAST`](../../Reference/im/MimControl.md) is ignored. A local mean threshold usually results in a faster, though less precise, binarization than Niblack.

### Threshold with Bernsen

Bernsen's adaptive threshold algorithm ([`M_BERNSEN`](../../Reference/im/MimControl.md)) represents a type of morphological erosion and dilation. This threshold results in the fastest process.

The following represents the general logic for calculating threshold values using Bernsen.

- If (_MaxNeighborhoodValue_ - _MinNeighborhoodValue_) &lt; [`M_MINIMUM_CONTRAST`](../../Reference/im/MimControl.md):
  - _ThresholdValue_ = [`M_GLOBAL_MIN`](../../Reference/im/MimControl.md).
- Otherwise:
  - _ThresholdValue_ = (_MaxNeighborhoodValue_ + _MinNeighborhoodValue_) / 2 + [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md).

To specify the size of the neighborhood around each source pixel with which to calculate the minimum (_MinNeighborhoodValue_) and maximum (_MaxNeighborhoodValue_) neighborhood value, use [`M_LOCAL_DIMENSION`](../../Reference/im/MimControl.md). The size should be set to a value that is somewhat wider than the thickness of objects. This ensures that both background and object pixels are present in the neighborhood of any object pixel.

When using [`M_BERNSEN`](../../Reference/im/MimControl.md), Aurora Imaging Library compares the minimum contrast ([`M_MINIMUM_CONTRAST`](../../Reference/im/MimControl.md)) to the difference between maximum and minimum values of the neighborhood. This guards against over binarizing relatively homogeneous regions. Typically, you should set the minimum contrast to a value above 5 (the default).

### Threshold with pseudomedian

A pseudomedian adaptive threshold algorithm ([`M_PSEUDOMEDIAN`](../../Reference/im/MimControl.md)) is similar to a Bernsen threshold, except it represents a type of morphological open and close process instead of erosion and dilation.

The following represents the general logic for calculating threshold values using [`M_PSEUDOMEDIAN`](../../Reference/im/MimControl.md).

- If (_MorphOpenValue_ - _MorphCloseValue_) &lt; [`M_MINIMUM_CONTRAST`](../../Reference/im/MimControl.md):
  - _ThresholdValue_ = [`M_GLOBAL_MIN`](../../Reference/im/MimControl.md).
- Otherwise:
  - _ThresholdValue_ = (_MorphOpenValue_ + _MorphCloseValue_) / 2 + [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md).

With pseudomedian, Aurora Imaging Library determines the threshold value for each pixel using the mean of grayscale values obtained by performing a morphological open (_MorphOpenValue_) and close (_MorphCloseValue_) operation on the source image. To specify the size of the neighborhood around each source pixel with which to perform the morphological open and close operations, use [`M_LOCAL_DIMENSION`](../../Reference/im/MimControl.md). Set the size to a value that is approximately half the expected thickness of objects.

When using [`M_PSEUDOMEDIAN`](../../Reference/im/MimControl.md), Aurora Imaging Library compares the minimum contrast ([`M_MINIMUM_CONTRAST`](../../Reference/im/MimControl.md)) to the difference between the opened and closed values of pixels. This guards against over binarizing relatively homogeneous regions.

### Hysteresis

The adaptive binarization threshold that you specify with [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md) uses a hysteresis process if you specify a threshold type (with [`M_THRESHOLD_TYPE`](../../Reference/im/MimControl.md)) other than [`M_SINGLE`](../../Reference/im/MimControl.md). In this case, a seed type thresholding (geodesic reconstruction) is performed after a second pass of the specified threshold. Specifically, Aurora Imaging Library internally calls [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) twice. The first pass processes as usual, according to the [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md) setting. For the second pass, [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) uses the [`M_..._SECOND_PASS`](../../Reference/im/MimControl.md) settings while reapplying the same threshold. Aurora Imaging Library then performs a geodesic reconstruction (a type of morphological erosion or dilation), using the results of the first pass as the source data and the results of the second pass as the seed data. This is similar to an [`M_RECONSTRUCT`](../../Reference/im/MimControl.md) threshold mode (available with an [`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md) context type). For more information, see [Threshold with geodesic reconstruction](S11_Adaptive_binarizing.md).

Regardless of threshold mode, Aurora Imaging Library uses [`M_GLOBAL_OFFSET_SECOND_PASS`](../../Reference/im/MimControl.md) instead of [`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md) during the second pass of [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md). For an [`M_NIBLACK`](../../Reference/im/MimControl.md) threshold mode, the second pass also uses [`M_NIBLACK_BIAS_SECOND_PASS`](../../Reference/im/MimControl.md) instead of [`M_NIBLACK_BIAS`](../../Reference/im/MimControl.md). Aurora Imaging Library generates an error if every [`M_..._SECOND_PASS`](../../Reference/im/MimControl.md) value it uses is the same as its first pass counterpart.

You can set [`M_THRESHOLD_TYPE`](../../Reference/im/MimControl.md) to specify which values to use as foreground. Setting [`M_THRESHOLD_TYPE`](../../Reference/im/MimControl.md) to [`M_IN_RANGE`](../../Reference/im/MimControl.md) specifies to use as foreground the values inside the range defined by the two hysteresis passes. The [`M_OUT_RANGE`](../../Reference/im/MimControl.md) setting specifies to use the values outside this range as the foreground.

## Controlling an adaptive binarize context that uses seeds

To control how Aurora Imaging Library establishes the threshold values for an [`M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT`](../../Reference/im/MimAlloc.md) context type, you can set [`M_THRESHOLD_MODE`](../../Reference/im/MimControl.md) to [`M_RECONSTRUCT`](../../Reference/im/MimControl.md), [`M_LEVEL`](../../Reference/im/MimControl.md), or [`M_TOGGLE`](../../Reference/im/MimControl.md).

By default, Aurora Imaging Library applies the specified threshold mode iteratively until the process reaches idempotence. Idempotence refers to the number of iterations at which subsequent iterations do not alter results. To specify a specific number of iterations, use [`M_NB_ITERATIONS`](../../Reference/im/MimControl.md). The following illustrates how the threshold value is determined iteratively from the seed image.

*[Image: MimBinarizeAdaptiveSeedIteration.png]*

### Threshold with geodesic reconstruction

By default, Aurora Imaging Library uses an adaptive geodesic reconstruction threshold ([`M_RECONSTRUCT`](../../Reference/im/MimControl.md)). This represents a type of morphological erosion or dilation, with seeds. When you call [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md), you can provide a seed image. If you do not, Aurora Imaging Library internally establishes the seed data.

Aurora Imaging Library performs the geodesic reconstruction according to the foreground ([`M_FOREGROUND_VALUE`](../../Reference/im/MimControl.md)). If you specify an image and the foreground is white (default), Aurora Imaging Library dilates the pixels of the seed until they reach darker pixels in the source. For a black foreground, Aurora Imaging Library erodes the pixels of the seed until they reach lighter pixels in the source. Aurora Imaging Library then applies the offset ([`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md)) to the resulting image and uses it as the threshold with which to perform the binarization. The following illustrates a one-dimensional grayscale profile of a source image and a seed image that you can use with a geodesic reconstruction threshold.

*[Image: MimBinarizeAdaptiveReconstruct.png]*

If you do not specify a seed image, Aurora Imaging Library either erodes (for a white foreground) or dilates (for a black foreground) the source image, according to the number of seed iterations ([`M_NB_SEED_ITERATIONS`](../../Reference/im/MimControl.md)).

The processing time resulting from an [`M_RECONSTRUCT`](../../Reference/im/MimControl.md) threshold is typically faster than [`M_LEVEL`](../../Reference/im/MimControl.md) and slower than [`M_TOGGLE`](../../Reference/im/MimControl.md).

### Threshold with leveling

A level threshold ([`M_LEVEL`](../../Reference/im/MimControl.md)) represents a type of morphological erosion and dilation, with seeds. When you call [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md), you can provide a seed image. If you do not, Aurora Imaging Library internally establishes the seed data.

Leveling essentially performs two geodesic reconstructions ([`M_RECONSTRUCT`](../../Reference/im/MimControl.md)). One that processes the foreground as white, and the other that processes the foreground as black. For the white foreground, Aurora Imaging Library dilates the pixels of the seed until they reach darker pixels in the source. For the black foreground, Aurora Imaging Library erodes the pixels of the seed until they reach lighter pixels in the source. Aurora Imaging Library then applies the offset ([`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md)) to the resulting image and uses it as the threshold with which to perform the binarization. The following illustrates a one-dimensional grayscale profile of a source image and a seed image that you can use with a level threshold.

*[Image: MimBinarizeAdaptiveLevel.png]*

If you do not specify a seed image, Aurora Imaging Library establishes them by performing a convolution on the source image using a smoothing filter (kernel), according to the number of seed iterations ([`M_NB_SEED_ITERATIONS`](../../Reference/im/MimControl.md)).

The processing time resulting from an [`M_LEVEL`](../../Reference/im/MimControl.md) threshold is typically longer than [`M_RECONSTRUCT`](../../Reference/im/MimControl.md) or [`M_TOGGLE`](../../Reference/im/MimControl.md). [`M_LEVEL`](../../Reference/im/MimControl.md) generally takes twice as long as [`M_RECONSTRUCT`](../../Reference/im/MimControl.md).

### Threshold with toggling

A toggle threshold ([`M_TOGGLE`](../../Reference/im/MimControl.md)) depends on two types of seed data (typically min and max values), each of which Aurora Imaging Library can use to establish the threshold values with which to binarize. When you call [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md), you can provide two seed images. If you do not, Aurora Imaging Library internally establishes the seed data.

For every source pixel, Aurora Imaging Library applies the offset ([`M_GLOBAL_OFFSET`](../../Reference/im/MimControl.md)) and compares the result to both seed values. The threshold with which to binarize is the seed to which the result is closest. This can be seen as snapping each threshold value to one of the two seed values. Unlike other threshold modes, this is a single iteration process ([`M_NB_ITERATIONS`](../../Reference/im/MimControl.md) is always one). If the source pixel is perfectly equidistant from both seeds, Aurora Imaging Library uses the source pixel value itself as the threshold value.

The following illustrates a one-dimensional grayscale profile of a source image and two seed images that you can use with a toggle threshold. The threshold is the closest seed.

*[Image: MimBinarizeAdaptiveToggle.png]*

If you do not specify a seed image, Aurora Imaging Library performs an erosion on its automatically established first seeds, and a dilation on its automatically established second seeds, according to the number of seed iterations ([`M_NB_SEED_ITERATIONS`](../../Reference/im/MimControl.md)).

The processing time resulting from an [`M_TOGGLE`](../../Reference/im/MimControl.md) threshold is typically faster than [`M_RECONSTRUCT`](../../Reference/im/MimControl.md) or [`M_LEVEL`](../../Reference/im/MimControl.md). Since toggling categorizes threshold values as one of two possibilities, it typically implies that you have already preprocessed your images.
