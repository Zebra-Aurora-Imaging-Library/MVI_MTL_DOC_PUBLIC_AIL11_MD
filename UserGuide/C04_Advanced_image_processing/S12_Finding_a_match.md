---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_image_processing
section: Finding_a_match
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-image / Finding a match"
---

# Establishing a correlation

One way to find a model image in your source image is to use [`MimMatch`](../../Reference/im/MimMatch.md). This function generates a series of correlation values (match scores) resulting from the model image being compared to every pixel in your image. The [`MimMatch`](../../Reference/im/MimMatch.md) operation results in an image of correlation data. Other Aurora Imaging Library match operations (such as a pattern match using [`MpatFind`](../../Reference/pat/MpatFind.md) or [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_MATCH`](../../Reference/im/MimMorphic.md)) don't produce the same types of results and don't necessarily use the same type of underlying correlation algorithm.

To store processing settings, [`MimMatch`](../../Reference/im/MimMatch.md) uses a match image processing context. You must set up the match image processing context with a model image to use for the match, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODEL_IMAGE`](../../Reference/im/MimControl.md). All other settings within the match image processing context are optional.

There are four different types of computation (modes) that can be performed when trying to match a model image to a source image neighborhood. You can specify one of the following modes, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODE`](../../Reference/im/MimControl.md):

- **Normalized grayscale correlation** ([`M_CORRELATE_NORMALIZED`](../../Reference/im/MimControl.md)). Calculates the correlation using a normalized grayscale correlation. The closer the resulting pixel value is to the specified maximum possible score, the closer the match between the pixel's neighborhood in the target area and the model image. This is the default mode.
- **Grayscale correlation** ([`M_CORRELATE`](../../Reference/im/MimControl.md)). Calculates the correlation using a grayscale correlation. The closer the resulting pixel value is to the specified maximum possible score, the closer the match between the pixel's neighborhood in the target area and the model image.
- **Sum of the absolute differences** ([`M_SUM_OF_ABS_DIFFERENCES`](../../Reference/im/MimControl.md)). Calculates the correlation using the sum of the absolute differences between the source image and the model image, within the neighborhood. The closer the resulting pixel value is to 0, the closer the match between the pixel's neighborhood in the target area and the model image.
- **Mean of the absolute differences** ([`M_MEAN_OF_ABS_DIFFERENCES`](../../Reference/im/MimControl.md)). Calculates the correlation using the sum of absolute differences divided by the number of pixels used to compute it, resulting in a 8-bit range result. The closer the resulting pixel value is to 0, the closer the match between the pixel's neighborhood in the target area and the model image.

When using the normalized grayscale correlation mode, you can specify how to compute the final score, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_SCORE_TYPE`](../../Reference/im/MimControl.md). The options are:

- To perform a normalized grayscale correlation.
  *[Image: IM-MatchEquation-NGS.png]*
- To perform a square of the normalized grayscale correlation.
  *[Image: IM-MatchEquation-SQofNGS.png]*
- To perform one of the above and then clip the resulting value so that values less than 0 are set to 0.

Before calling [`MimMatch`](../../Reference/im/MimMatch.md), set up a match image processing context by performing the following:

1. Allocate a match image processing context, using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_MATCH_CONTEXT`](../../Reference/im/MimAlloc.md).
2. Specify the Aurora Imaging Library image buffer containing the model image, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODEL_IMAGE`](../../Reference/im/MimControl.md).
3. Optionally, to reduce the size of the model image matched against the neighborhood of each pixel in the source image, specify a mask, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MASK_IMAGE`](../../Reference/im/MimControl.md). The part of the model that should be used in the match should contain zero values. All non-zero values are ignored.
4. Optionally, set the type of computation to perform, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODE`](../../Reference/im/MimControl.md) (for example, grayscale correlation mode).
5. Optionally, if using normalized grayscale correlation mode, specify whether to square or clip the final correlation score, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_SCORE_TYPE`](../../Reference/im/MimControl.md).
6. Optionally, specify whether to examine every pixel or every other pixel in the model image when matching, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODEL_STEP`](../../Reference/im/MimControl.md). The former is slower but offers a more complete correlation.
7. Optionally, if using the normalized grayscale correlation mode, specify the maximum final correlation score, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MAX_SCORE`](../../Reference/im/MimControl.md).
8. Preprocess the context by calling [`MimMatch`](../../Reference/im/MimMatch.md) with [`M_PREPROCESS`](../../Reference/im/MimMatch.md). Both the source and/or destination image buffers can be set to [`M_NULL`](../../Reference/im/MimMatch.md). If, however, the source or destination image buffer is provided, it should be a typical source or destination image buffer, respectively, and it will be used by the preprocess operation to better optimize future calls. If the preprocess operation is not done explicitly, it will be done when [`MimMatch`](../../Reference/im/MimMatch.md) is first called.
