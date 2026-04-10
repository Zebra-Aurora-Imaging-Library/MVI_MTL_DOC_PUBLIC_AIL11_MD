---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Thresholding_your_images
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Thresholding your images"
---

# Thresholding your images

Thresholding images means reducing each pixel to a certain range of values. Some operations can be performed more efficiently on thresholded images. Images with full grayscale levels are useful for some tasks, but have redundant information for others. The Aurora Imaging Library package provides the following types of threshold operations:

- Binarizing, using [`MimBinarize`](../../Reference/im/MimBinarize.md).
  - Adaptive binarizing, using [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md).
- Clipping, using [`MimClip`](../../Reference/im/MimClip.md).

## Binarizing

A binarizing operation reduces an image to two grayscale values by comparing each pixel value in the image against one or two specified threshold values (for example, whether each pixel value is above a threshold value, or within the range of two threshold values). Pixels that meet the specified condition are typically set to the maximum possible value of the image (for example, 255 if the image is 8-bit or 1 if the image is a floating-point buffer), while other pixels are set to 0. Binary images are useful when trying to identify geometric patterns and objects in your image since they are not cluttered with shading information.

The following animation illustrates the difference between the grayscale and binary version of an image.

*[Image: IM_Thresholding_your_images]*

When using [`MimBinarize`](../../Reference/im/MimBinarize.md), it is important to select threshold values that preserves the required information. For example, in the previous image, an incorrect threshold value might inappropriately change pixels of particles into background pixels, resulting in fewer particles than the number that actually exist.

If you are unable to properly preserve the required information when binarizing your images with [`MimBinarize`](../../Reference/im/MimBinarize.md), try an adaptive binarization with[`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md). Though typically longer to execute, an adaptive binarization is more robust than a conventional binarization, particularly when dealing with poor or varying illumination or contrast. For more information, see [Adaptive binarizing](S11_Adaptive_binarizing.md).

### Determining the threshold value from the histogram

If the binarizing condition uses only one threshold value, [`MimBinarize`](../../Reference/im/MimBinarize.md) can automatically determine the threshold value from the source image's characteristics. It can establish the threshold using one of the following threshold modes:

- Bimodal.
- Dominant.
- Triangular bisection.

In bimodal threshold mode (using [`M_BIMODAL`](../../Reference/im/MimBinarize.md)), a histogram of the source image is internally generated. Then, if the histogram contains only two peaks, the threshold value is set to the intensity value at the lowest point between these two peaks, on the assumption that these peaks represent the object and the background. If the histogram contains more than two peaks, the threshold value will typically be set between the intensity values at the two principal peaks, although exceptions exist for the situations where the principal peaks occur closest to the minimum intensity value (pure black) or the maximum intensity value (full saturation). Note that the bimodal threshold mode works best when the histogram contains two visible peaks. *[Image: auto_threshold.png]*

In dominant threshold mode (using [`M_DOMINANT`](../../Reference/im/MimBinarize.md)), a histogram of the source image is internally generated. If the histogram contains only two peaks, the threshold is set to the intensity value at the lowest point between these two peaks. If the histogram contains more than two peaks, the threshold value is set at the lowest point between the dominant (highest) peak and its nearest peak on either the left or right side, depending on the pixel distribution in the histogram. If there are more pixels represented on the left side of the dominant peak, the threshold will be on the left side. If there are more pixels represented on the right side of the dominant peak, the threshold will be on the right side.

In the example histogram below, [`M_DOMINANT`](../../Reference/im/MimBinarize.md) places the threshold on the right side of the dominant peak because there is a greater pixel density (indicated by the area under the curve) on the right side of the peak.*[Image: auto_threshold_dominant.png]*

In triangular bisection threshold mode (using either [`M_TRIANGLE_BISECTION_DARK`](../../Reference/im/MimBinarize.md) or [`M_TRIANGLE_BISECTION_BRIGHT`](../../Reference/im/MimBinarize.md)), Aurora Imaging Library internally generates an intensity histogram of the source image. A line is drawn between the highest peak in the histogram and the lowest point either heading towards the minimum intensity value (in the dark region of the histogram) or heading towards the maximum intensity value (in the bright region of the histogram), depending on whether performing a dark or bright bisection, respectively. The threshold value is the intensity value at the inflection point (that is, the point furthest below the line). Note that all points above the line are ignored. *[Image: auto_threshold_bisection_dark.png]*

Triangular bisection threshold mode works best when there is only one peak in the histogram. To determine whether to use triangular bisection dark or bright mode, examine the distance between the most commonly-occurring pixel value (the top of the peak) and the least commonly-occurring pixel value (the bottom of the peak). If the distance between the most commonly-occurring pixel value and the least commonly-occurring pixel value is greatest as the values go towards the minimum intensity value, try the triangular bisection dark mode. If, however, the distance is greatest as the values go towards the maximum intensity value, try the triangular bisection bright mode.

### Specifying the threshold value(s) manually

Regardless of the number of threshold values that the binarize condition uses, you can pass [`MimBinarize`](../../Reference/im/MimBinarize.md) the threshold values manually. You can manually pass them in one of the following threshold modes:

- Percentile.
- Fixed.

The percentile threshold mode (specified using [`M_PERCENTILE_VALUE`](../../Reference/im/MimBinarize.md)) allows you to specify the threshold value(s) for the condition as a percentage of the histogram data. This mode uses an internally-generated cumulative histogram, whereby the X-axis represents the intensity value and the Y-axis represents the percentage of pixels equal to or less than the intensity value.

In this mode, you specify a threshold value by specifying the percentage of pixels that must be equal to or less than the required threshold value. For example, if the specified value is 90, the cumulative histogram shown below depicts that 90% of the values are below 170. Therefore, the threshold value is 170.

*[Image: IM-binarize-percentile-value.png]*

Percentile thresholds are typically used when the histogram varies greatly from image to image. Using a percentage instead of a fixed value, leads the threshold to be linked to the histogram rather than a fixed value.

Alternatively, you can use the fixed threshold mode (with [`M_FIXED`](../../Reference/im/MimBinarize.md)) to explicitly specify the threshold value(s) manually.

## Clipping

Clipping changes the image data less dramatically than binarizing. It changes the data to include only the range of pixel values in which you are interested. [`MimClip`](../../Reference/im/MimClip.md) takes a condition with at most two threshold points and replaces only those pixels that meet the condition with given values. Pixels that do not meet the condition are unaffected.

This can be useful to change data from one data type to another. For example, if you have a 16-bit result, but most of the pixels are less than 256, you could clip the result into an 8-bit buffer, and set all the pixels that are too big to the largest possible value, that is, 255.
