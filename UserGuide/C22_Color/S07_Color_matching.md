---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Color_matching
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Color matching"
---

# Color matching

Color matching is the process of finding a match between the color of a target area and one or more predefined color-samples. Color-samples are composed of color elements. These color elements can be added to or removed from the color-sample to control the color-sample's color properties during the matching process. When you first define a color-sample using [`McolDefine`](../../Reference/col/McolDefine.md), you are also automatically adding the first color element to the sample.

The match is performed, in part, by calculating the color distance between the target area and the color-sample. This information can be retrieved after calling [`McolMatch`](../../Reference/col/McolMatch.md). However, if you are only interested in color distances, you can use [`McolDistance`](../../Reference/col/McolDistance.md). For more information, including the differences in distances between [`McolMatch`](../../Reference/col/McolMatch.md) and [`McolDistance`](../../Reference/col/McolDistance.md), see [Distance between colors](S06_Distance_between_colors.md).

Various conditions, such as different cameras and illuminants, can cause color from identical images to appear dissimilar. If this occurs, you can call [`McolTransform`](../../Reference/col/McolTransform.md) to perform a relative color calibration before matching colors. Relative color calibration ensures all colors are consistent according to a specified reference color. For more information, see [Relative color calibration](S04_Relative_color_calibration.md).

## Steps to performing color matching

The following steps provide a basic methodology for using the Aurora Imaging Library Color Analysis module to match colors:

1. Allocate a color matching context to hold your color-samples and color matching settings, using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md).
2. Specify the source color space, using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_RGB`](../../Reference/col/McolAlloc.md), [`M_HSL`](../../Reference/col/McolAlloc.md), or [`M_CIELAB`](../../Reference/col/McolAlloc.md).
   For more information, see [Source color space](S03_Color_spaces_and_converting_between_them.md).
3. Allocate a color matching result buffer to hold the color matching results, using [`McolAllocResult`](../../Reference/col/McolAllocResult.md) with [`M_COLOR_MATCHING_RESULT`](../../Reference/col/McolAllocResult.md).
   This step is not required if you are calculating an image result directly, using [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_...`](../../Reference/col/McolMatch.md).
4. Optionally, convert your colors to the appropriate color space, using [`MimConvert`](../../Reference/im/MimConvert.md). For example, you can convert RGB color-samples to HSL.
   When using an RGB source color space, you can also use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) to convert your RGB data to CIELAB or HSL before the match. For more information, see [Converting between color spaces](S03_Color_spaces_and_converting_between_them.md).
5. Define the color-samples and the first color element of the sample using [`McolDefine`](../../Reference/col/McolDefine.md).
6. Optionally, add or remove color elements from the sample using [`McolDefine`](../../Reference/col/McolDefine.md) with [`M_ADD_COLOR_TO_SAMPLE`](../../Reference/col/McolDefine.md) or [`M_DELETE`](../../Reference/col/McolDefine.md).
7. Optionally, change the operation mode and distance type used to perform the match operation, using [`McolSetMethod`](../../Reference/col/McolSetMethod.md).
8. Optionally, control the settings of a color matching context or the color-samples contained therein, using [`McolControl`](../../Reference/col/McolControl.md).
   When controlling the settings of color-samples, you can specify a specific color-sample or [`M_ALL`](../../Reference/col/McolControl.md), which applies the control type setting to all the color-samples contained within the color matching context (when supported).
9. Preprocess the context, using [`McolPreprocess`](../../Reference/col/McolPreprocess.md).
10. Perform the color matching operation, using [`McolMatch`](../../Reference/col/McolMatch.md).
11. Retrieve the required results from the result buffer, using [`McolGetResult`](../../Reference/col/McolGetResult.md).
12. Draw image results, using [`McolDraw`](../../Reference/col/McolDraw.md).
13. Free all your allocated objects, using [`McolFree`](../../Reference/col/McolFree.md), unless [`M_UNIQUE_ID`](../../Reference/col/McolAlloc.md) was specified during allocation.

## Basics of color matching

Color matching can be used to perform one of two basic tasks: color identification or supervised color segmentation. In either case, the match is based on the defined color-samples, whose color elements can come from a source image or from explicit color values. For more information, see [Defining and adding color-samples and color elements](S07_Color_matching.md).

### Color identification

Color identification refers to matching the general color of each target area with the best color-sample within a group of predefined color-samples. When performing color identification, you might find it useful to locate the target areas with the Aurora Imaging Library Geometric Model Finder module before performing the match. To produce image results for a target area, use [`McolMatch`](../../Reference/col/McolMatch.md) or [`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md).

*[Image: Color_Identification.png]*

### Supervised color segmentation

Supervised color segmentation refers to matching the color value of each pixel in the target image (or target area) with the best predefined (supervising) color-sample. For supervised color segmentation, you must use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md), which matches colors on a pixel-by-pixel basis, and produce image results using [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md).

By matching the color value of each pixel with the best predefined color-sample, you can use the Color Analysis module to separate objects by their color information. This type of matching is useful to, for example, retrieve the relative presence of each color in an image (this is also referred to as the spatial coverage for each of the colors).

*[Image: Color_SupervisedSegmentation.png]*

## Defining and adding color-samples and color elements

As previously discussed, color-samples are composed of color elements. When you first define a color-sample, it is automatically considered to have a single color element. Therefore, when you add a color-sample to the color matching context, you are by default adding the first color element to the color-sample. You can define a color-sample (and consequently the first color-sample element) from either a region of an image ([`M_IMAGE`](../../Reference/col/McolDefine.md)), or from three explicit color band values ([`M_TRIPLET`](../../Reference/col/McolDefine.md)), using [`McolDefine`](../../Reference/col/McolDefine.md).

For image type color-samples, Aurora Imaging Library performs an estimation of the color based on color statistics, such as the mean. Aurora Imaging Library considers this the color of the color-sample and uses it whenever required (for example, [`McolTransform`](../../Reference/col/McolTransform.md)). The size of the color-sample does not typically affect speed or effectiveness.

*[Image: Color_DefineFromImage.png]*

To add color elements to color-samples that already exist in the context, use [`M_ADD_COLOR_TO_SAMPLE`](../../Reference/col/McolDefine.md). The color estimation that Aurora Imaging Library performs for the color-sample uses the new color element(s) that you specify for the color-sample, as well as the existing element(s) of the color-sample. Aurora Imaging Library now considers the resulting estimate the color of the color-sample.

You can also use [`McolControl`](../../Reference/col/McolControl.md)to control settings related to color-samples.

When you add, modify, or delete a color-sample, you must preprocess the color matching context ([`McolPreprocess`](../../Reference/col/McolPreprocess.md)) before any subsequent call to [`McolMatch`](../../Reference/col/McolMatch.md).

For more information about color-samples, see [Color-samples and color elements](S04_Relative_color_calibration.md). Defining color-samples for color matching is nearly identical to defining color-samples for relative color calibration.

## Area identifier image

The area identifier image is a grayscale image, specified with [`McolMatch`](../../Reference/col/McolMatch.md), that defines the independent target areas where color matching will occur in the target image. You must use an area identifier image to match multiple target areas. Each unique, non-zero label in the area identifier image defines an independent target area. Typically, the area identifier image is the same size as the target image.

In the following example, the 4 target areas in the area identifier image are defined according to the objects found with the Aurora Imaging Library Model Finder module. The color of each corresponding target area, in the actual target image, is then matched (identified) with the best color-sample.

*[Image: Color_AreaImageExample.png]*

To produce image results on an area basis (as illustrated above), use [`McolMatch`](../../Reference/col/McolMatch.md) or [`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md). For more information, see [Image results](S07_Color_matching.md).

## Acceptance

The acceptance levels determine the minimum scores required for a successful match between a target area and a color-sample. You can set an acceptance level for the color-sample's match score, and for the target area's relevance score, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_ACCEPTANCE`](../../Reference/col/McolControl.md) and [`M_ACCEPTANCE_RELEVANCE`](../../Reference/col/McolControl.md).

### Color-sample acceptance (for the color-sample's match score)

[`M_ACCEPTANCE`](../../Reference/col/McolControl.md) is applied to the match score ([`McolGetResult`](../../Reference/col/McolGetResult.md) with [`M_SCORE`](../../Reference/col/McolGetResult.md)), which indicates the similarity between the color of the color-sample and the color of the target area. The higher the acceptance, the closer the colors must be for them to match. For example, if you are using an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)), and you set [`M_ACCEPTANCE`](../../Reference/col/McolControl.md) to 100, the colors will only match if they are absolutely the same. For an acceptance of 100 while using an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode, all pixels in the target area that are not outliers must have voted for the same color-sample to have a successful match.

For more information on the actual match score calculated, see [Match score and relevance score](S07_Color_matching.md).

### Relevance acceptance (for the target area's relevance score)

[`M_ACCEPTANCE_RELEVANCE`](../../Reference/col/McolControl.md) is applied to the target area's relevance score ([`McolGetResult`](../../Reference/col/McolGetResult.md) with [`M_SCORE_RELEVANCE`](../../Reference/col/McolGetResult.md)), which indicates the significance (relevance) of the match score ([`M_SCORE`](../../Reference/col/McolGetResult.md)). In statistics, this is similar to the confidence level. A high relevance score indicates that the best-matched color-sample was a vastly superior match compared to the other color-samples, while a low relevance score indicates that another color-sample came very close to being the best-matched color-sample.

If you are using the [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)), and you set [`M_ACCEPTANCE_RELEVANCE`](../../Reference/col/McolControl.md) to 100, the match will only be successful if the best-matched color-sample was the only color-sample considered (this can occur if, for example, all the other color-samples fell outside of the distance tolerance ([`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md))). For a relevance acceptance of 100 while using an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode, all pixels in the target area must have voted for the same color-sample.

For more information on the actual relevance score calculated, see [Match score and relevance score](S07_Color_matching.md).

## Distance tolerance

The distance tolerance refers to the maximum color distance, between the color-sample and the target area, allowed for a successful match. To specify the distance tolerance, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) set to [`M_INFINITE`](../../Reference/col/McolControl.md), a specific value, or [`M_AUTO`](../../Reference/col/McolControl.md) (the default). The greater the distance tolerance, the greater the distance (difference) between matching colors can be. For example, if your target area is green, you can set the distance tolerance to 0 to only match with the exact same green. However, by increasing the tolerance, you can match with colors that are progressively different than the original.

*[Image: Color_ToleranceIntro.png]*

When setting the tolerance, you must consider the specified distance type, the color space, and the color space encoding. For example, a distance tolerance of 1.0 when using an [`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md) distance type is not the same as when using an [`M_MANHATTAN`](../../Reference/col/McolSetMethod.md) distance type.

### Tolerance mode

When setting [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) to a specific value or [`M_AUTO`](../../Reference/col/McolControl.md), Aurora Imaging Library determines the distance tolerance according to the tolerance mode, which you can set with [`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md).

If you set the tolerance mode to [`M_ABSOLUTE`](../../Reference/col/McolControl.md) (the default), Aurora Imaging Library applies the [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) value as is. For [`M_AUTO`](../../Reference/col/McolControl.md), the [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) value is infinite.

If you set the tolerance mode to [`M_RELATIVE`](../../Reference/col/McolControl.md), Aurora Imaging Library internally calculates the distance between each pair of color-samples and finds the smallest distance value; half this value is then multiplied by the [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) value. For example, if you have three different color-samples (sample 1, sample 2, and sample 3), the distances between each pair would be the distance between sample 1 and sample 2 (distance 12), sample 1 and sample 3 (distance 13), and sample 2 and sample 3 (distance 23). The tolerance value for sample 1 would then be the smaller value of distance 12 and distance 13, multiplied by [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md). The tolerances for each of the other samples are calculated the same way. For [`M_AUTO`](../../Reference/col/McolControl.md), the [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) value is 1.

If you set the tolerance mode to [`M_SAMPLE_STDDEV`](../../Reference/col/McolControl.md), Aurora Imaging Library specifies a tolerance strategy based on the color-sample's standard deviation.

For color-samples defined from images ([`McolDefine`](../../Reference/col/McolDefine.md) with [`M_IMAGE`](../../Reference/col/McolDefine.md)), the color-sample's standard deviation is computed as the distance between each pixel in the color-sample and the average color of the color-sample. Aurora Imaging Library calculates this distance according to the current distance type set with [`McolSetMethod`](../../Reference/col/McolSetMethod.md). The resulting distance, which in this case is considered to be the color-sample's standard deviation, is multiplied by [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) and is used as the acceptable distance tolerance. If the computed color-sample's standard deviation is less than 1.0, Aurora Imaging Library uses 1.0 as the standard deviation.

For color-samples composed of triplets ([`McolDefine`](../../Reference/col/McolDefine.md) with [`M_TRIPLET`](../../Reference/col/McolDefine.md)), Aurora Imaging Library uses 1.0 as the standard deviation. In this case, [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) remains unchanged and is used as the acceptable distance tolerance.

For [`M_AUTO`](../../Reference/col/McolControl.md), the [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) value for [`M_SAMPLE_STDDEV`](../../Reference/col/McolControl.md) is 3.

## Operation mode and distance type

Before calling [`McolMatch`](../../Reference/col/McolMatch.md), you can use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) to set the match's operation mode to one of the following:

- [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) (default), which uses the average color of the color-sample and target area for the match.
  [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) is the fastest operation mode and is typically sufficient for simple applications.
- [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md), which uses the average color of the color-sample and the color of each pixel in the target area for the match.
  Although typically slower than [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md), [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) is a good option when dealing with outliers or images containing unneeded pixels/colors. As previously discussed, you must use [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) to perform [supervised color segmentation](S07_Color_matching.md).
- [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md), which uses the color histogram of the color-sample and the target area for the match.
  You should generally use [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) when trying to match a mixture of colors.
- [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md), which uses the color histogram of the color-sample and the color of each pixel in the target area for the match. Each target pixel votes for the best color-sample and the color-sample with the most votes is considered the best match.

Color matching operation modes only take color information into account; target areas are therefore only distinguishable if their color composition differs. Further details for each operation are described later in this section.

You can also use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) to set the type of distance with which to perform the match. For RGB and CIELAB color spaces, a Euclidean color distance is calculated by default. For HSL color spaces, Manhattan is the default. For more information, see [Color distance types](S06_Distance_between_colors.md).

### M_STAT_MIN_DIST operation mode

When using the [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode, color statistics are calculated (typically the mean/average) for each target area and for each color-sample defined in the context, and the color distance between each target area and each color-sample is determined.

The resulting distances determine the score of the color-samples. The closer the colors, the higher the score. If a distance is not within a color-sample's distance tolerance, the color-sample's score is 0%. The color-sample with the highest score above the acceptance level is the target area's best-matched color-sample.

In the following example, the target area's average color is calculated, and then matched with color-sample 1, which is the closest (best-match) color.

*[Image: Color_OperationModeM_STAT_MIN_DIST.png]*

For [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md), you should accurately define the target area, and it should generally consist of the required color, as indicated in the example above.

### M_MIN_DIST_VOTE operation mode

When using the [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode, color statistics are calculated (typically the mean/average) for each color-sample defined in the context, and the color distance between each pixel in each target area and each color-sample is determined. Each target pixel then votes for the color-sample with the closest color, and which is also within the distance tolerance.

The number of votes that a color-sample accumulates determines its score. The greater the number of votes, the higher the score. The color-sample with the highest score above the acceptance level is the target area's best-matched color-sample.

In the following example, each target pixel votes for the color-sample that has the closest color. In general, grapefruit pixels will vote for color-sample 1, while background pixels will vote for sample 2. Since there are more grapefruit pixels, color-sample 1 is the best-match.

*[Image: Color_OperationModeM_MIN_DIST_VOTE.png]*

Since [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operates on a pixel-by-pixel basis, it provides more detailed results (for each pixel) than [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) and is generally more robust (but slower). [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) is also less sensitive towards the accuracy of the target areas; as indicated in the example above, the target need not consist of only the grapefruit.

### M_HISTOGRAM_MATCHING and M_HISTOGRAM_VOTE operation modes

Aurora Imaging Library offers two histogram operation modes: [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) and [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).

When using the [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) operation mode, color histograms are calculated for each target area and for each sample defined in the context. A match score is then calculated between each target area histogram and the sample histograms. The sample with the highest match score above the acceptance is the target area's best-matched sample. When using [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md), you must set [`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md) to [`M_ABSOLUTE`](../../Reference/col/McolControl.md).

In [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) mode, pixel values are grouped together by subdividing their color space into bins of equal size. You must specify the number of bins for each color band using [`McolControl`](../../Reference/col/McolControl.md) with [`M_NB_BINS_BAND_...`](../../Reference/col/McolControl.md). The match score is based on the comparison of histogram frequencies for each bin, rather than each individual pixel value.

*[Image: Color_M_HISTOGRAM_MATCHING_example.png]*

> **Note:** Note that the histograms shown above are approximations intended for explanatory purposes.

[`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) is generally the most robust operation mode when dealing with color-samples that contain a mixture of colors. For more detailed information on histogram matching, see [Histogram matching concepts](S08_Advanced_color_matching_settings.md).

The [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) operation mode uses a pixel voting process to find a target area's best-matched color-sample. When using this operation mode, Aurora Imaging Library computes color histograms for each sample defined in the context. Then, each pixel in the target area votes for the color-sample that has a non-empty histogram bin containing the pixel's color. The number of votes that a color-sample accumulates determines its score. The color-sample with the highest score above the acceptance ([`McolControl`](../../Reference/col/McolControl.md) with [`M_ACCEPTANCE`](../../Reference/col/McolControl.md)) is the target area's best-matched color-sample. In the case of a tie, either during the vote or after all votes have been cast, the color-sample with the highest label value is chosen as the best-match.

When using an [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) operation mode, Aurora Imaging Library does not perform typical distance calculations. When passing [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) to [`McolSetMethod`](../../Reference/col/McolSetMethod.md), you must also set the distance type to [`M_NONE`](../../Reference/col/McolSetMethod.md). Also, Aurora Imaging Library ignores distance settings such as [`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md) and [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md).

Like [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md), you should generally use [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) when color-samples are a mixture of colors. Although typically slower than [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md), [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) is a good option when dealing with outliers or images containing unneeded pixels/colors.

## Basic results

You can use [`McolGetResult`](../../Reference/col/McolGetResult.md) with the identifier of a color matching result buffer to retrieve context, target area, and color-sample results, such as the index of the target area's best-matched color-sample ([`M_BEST_MATCH_INDEX`](../../Reference/col/McolGetResult.md)). To retrieve such results, you must have first used [`McolMatch`](../../Reference/col/McolMatch.md) to write all results to a result buffer, by setting its [`ControlFlag`](../../Reference/col/McolMatch.md) parameter to [`M_DEFAULT`](../../Reference/col/McolMatch.md). If you used [`McolMatch`](../../Reference/col/McolMatch.md) to write the results to an image buffer ([`M_DRAW_...`](../../Reference/col/McolMatch.md)) instead, you will not have a result buffer to specify in [`McolGetResult`](../../Reference/col/McolGetResult.md). For more information on calculating image results, see [Image results](S07_Color_matching.md).

Depending on the type of result to retrieve, you must set the appropriate label or index value for the target area ([`AreaLabel`](../../Reference/col/McolGetResult.md)) and color-sample ([`ColorSampleIndexOrLabel`](../../Reference/col/McolGetResult.md)) parameters, and use the proper data type to hold the results. The following table lists various types of results and the settings you should specify to retrieve them.

| Type of result | [`AreaLabel`](../../Reference/col/McolGetResult.md) parameter | [`ColorSampleIndexOrLabel`](../../Reference/col/McolGetResult.md) parameter | Data type for result |
| --- | --- | --- | --- |
| A color-sample result for a specific color-sample. For example, you want to retrieve a color-sample's match score ([`M_SCORE`](../../Reference/col/McolGetResult.md)). Note that to get any color-sample result, you must specify the target area(s). | A specific target area. | A specific color-sample. | Data type: double. |
| A color-sample result for a specific color-sample in all target areas. For example, you want to retrieve the color distance of a color-sample in each target area ([`M_COLOR_DISTANCE`](../../Reference/col/McolGetResult.md)). | All target areas. | A specific color-sample. | Data type: array of type double. Array size: [`M_NUMBER_OF_AREAS`](../../Reference/col/McolGetResult.md). |
| A color-sample result for all color-samples in a specific target area. For example, you want to retrieve the color distance of all color-samples in a target area ([`M_COLOR_DISTANCE`](../../Reference/col/McolGetResult.md)). | A specific target area. | All color-samples. | Data type: array of type double. Array size: [`M_NUMBER_OF_SAMPLES`](../../Reference/col/McolGetResult.md). |
| A color-sample result for all color-samples in all target areas. For example, you want to retrieve whether each color-sample in each target area fulfills the color matching conditions ([`M_SAMPLE_MATCH_STATUS`](../../Reference/col/McolGetResult.md)). | All target areas. | All color-samples. | Data type: array of type double. Array size: [`M_NUMBER_OF_AREAS`](../../Reference/col/McolGetResult.md) x [`M_NUMBER_OF_SAMPLES`](../../Reference/col/McolGetResult.md). |
| A general target area result for one target area. For example, you want to retrieve the index of a target area's best-matched color-sample ([`M_BEST_MATCH_INDEX`](../../Reference/col/McolGetResult.md)). | A specific target area. | [`M_GENERAL`](../../Reference/col/McolGetResult.md). | Data type: double. |
| A general target area result for all target areas. For example, you want to retrieve the index of each target area's best-matched color-sample ([`M_BEST_MATCH_INDEX`](../../Reference/col/McolGetResult.md)). | All target areas. | [`M_GENERAL`](../../Reference/col/McolGetResult.md). | Data type: array of type double. Array size: [`M_NUMBER_OF_AREAS`](../../Reference/col/McolGetResult.md). |
| A general result for a color matching context. For example, you want to retrieve the number of target areas or color-samples ([`M_NUMBER_OF_AREAS`](../../Reference/col/McolGetResult.md) or [`M_NUMBER_OF_SAMPLES`](../../Reference/col/McolGetResult.md)). | [`M_GENERAL`](../../Reference/col/McolGetResult.md). | [`M_GENERAL`](../../Reference/col/McolGetResult.md). | Data type: double. |

### Best-matched color-sample

You can either return the index or the label of each target area's best-matched color-sample, using [`M_BEST_MATCH_INDEX`](../../Reference/col/McolGetResult.md) or [`M_BEST_MATCH_LABEL`](../../Reference/col/McolGetResult.md). If no color-sample has matched, -1 is returned for [`M_BEST_MATCH_INDEX`](../../Reference/col/McolGetResult.md); for [`M_BEST_MATCH_LABEL`](../../Reference/col/McolGetResult.md), the value set using [`McolControl`](../../Reference/col/McolControl.md) with [`M_OUTLIER_LABEL`](../../Reference/col/McolControl.md) is returned.

### Match status

You can either return the match status of a color-sample, using [`M_SAMPLE_MATCH_STATUS`](../../Reference/col/McolGetResult.md), or the match status of a target area, using [`M_STATUS`](../../Reference/col/McolGetResult.md).

The status of a color-sample ([`M_SAMPLE_MATCH_STATUS`](../../Reference/col/McolGetResult.md)) returns [`M_MATCH`](../../Reference/col/McolGetResult.md) if that color-sample fulfills the match conditions, with respect to the acceptance ([`M_ACCEPTANCE`](../../Reference/col/McolControl.md)) and the distance tolerance ([`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md)); otherwise, [`M_NO_MATCH`](../../Reference/col/McolGetResult.md) is returned. The status of a target area ([`M_STATUS`](../../Reference/col/McolGetResult.md)) returns [`M_SUCCESS`](../../Reference/col/McolGetResult.md) if at least one color-sample fulfills the match conditions, and if the target area's relevance score passes the relevance acceptance level ([`M_ACCEPTANCE_RELEVANCE`](../../Reference/col/McolControl.md)); otherwise, [`M_FAILURE`](../../Reference/col/McolGetResult.md) is returned.

The color-sample with the highest score ([`M_SCORE`](../../Reference/col/McolGetResult.md)) is referred to as the best-matched color-sample. To determine if a specific color-sample is the best-matched color-sample, compare the color-sample's index or label with [`M_BEST_MATCH_INDEX`](../../Reference/col/McolGetResult.md) or [`M_BEST_MATCH_LABEL`](../../Reference/col/McolGetResult.md).

### Match score and relevance score

The color-sample's match score is based on the operation mode specified, using [`McolSetMethod`](../../Reference/col/McolSetMethod.md):

- [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md).
  *[Image: MatchScore1EQ.png]*

   _Distance_ refers to the current color-sample distance and _MaxDistance_ refers to the maximum distance possible in the source color space.
- [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md).
  *[Image: MatchScore2EQ.png]*

   _NumberOfVotes_ refers to the current color-sample's number of votes, and the sum is over the number of votes for all color-samples.

Similarly, the target area's relevance score is also based on the operation mode specified, using [`McolSetMethod`](../../Reference/col/McolSetMethod.md):

- [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md).
  *[Image: ColorRelevanceScore1EQ.png]*

   _BestDistance_ is the distance of the best-matched color-sample, and the sum is over all color-samples that have been matched by the target area.
- [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md).
  *[Image: ColorRelevanceScore2EQ.png]*

   _NumberOfVotes_ refers to the current color-sample's number of votes, and the sum is over the number of votes for all color-samples.

A low relevance score indicates that you should be cautious about the match results, even if the match score is high. For example, a high match score and a low relevance score could mean that the target area matched very well with a color-sample, but came very close to matching with a different color-sample; this implies that a slightly different target image, or even a difference in lighting, could change your results. You should therefore set appropriate acceptance levels for each of these scores. For more information, see [Acceptance](S07_Color_matching.md).

The following example shows two cases illustrating the difference between match and relevance scores in [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) mode:

*[Image: Color_match_relevance_comparison.png]*

### Outlier coverage and color-sample coverage

The outlier coverage ([`M_OUTLIER_COVERAGE`](../../Reference/col/McolGetResult.md)) quantifies, as a percentage, the proportion of pixels in the target area that did not vote for any color-sample, while the sample coverage ([`M_SAMPLE_COVERAGE`](../../Reference/col/McolGetResult.md)) quantifies the proportion of pixels that did vote for a specific color-sample.

Since coverage results are based on pixel votes, they are only available when using an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode.

### Color distance

The color distance ([`M_COLOR_DISTANCE`](../../Reference/col/McolGetResult.md)) returns the distance (difference) between the color of the target area and the specified color-sample, when using an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode.

You can also return the maximum color distance ([`M_MAX_DISTANCE`](../../Reference/col/McolGetResult.md)), which is the greatest color distance between the target area and all its matching color-samples.

## Image results

To produce image results, you will typically use [`McolMatch`](../../Reference/col/McolMatch.md) with the [`ControlFlag`](../../Reference/col/McolMatch.md) parameter set to [`M_DEFAULT`](../../Reference/col/McolMatch.md), and pass the identifier of a result buffer in which to write the results of the color matching operation. You will then use this result buffer with [`McolDraw`](../../Reference/col/McolDraw.md) to draw any image result ([`M_DRAW_...`](../../Reference/col/McolMatch.md)). You can also use this result buffer to retrieve any other type of result, using [`McolGetResult`](../../Reference/col/McolGetResult.md).

In some cases you can avoid calling [`McolDraw`](../../Reference/col/McolDraw.md) and produce an image result directly using [`McolMatch`](../../Reference/col/McolMatch.md) by setting the [`ControlFlag`](../../Reference/col/McolMatch.md) parameter to [`M_DRAW_...`](../../Reference/col/McolDraw.md), and passing the identifier of an image buffer in which to write the results of the color matching operation. Since you do not have to call [`McolDraw`](../../Reference/col/McolDraw.md), producing an image this way is typically done to save time when you require just one specific image result. For example, when performing color segmentation, you might only be interested in the label of the color-sample for which each pixel voted; therefore, using [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolMatch.md) to get that one resulting image is sufficient. Note that when using [`McolMatch`](../../Reference/col/McolMatch.md) in this way, you are not producing a result buffer and therefore will not be able to retrieve results using [`McolGetResult`](../../Reference/col/McolGetResult.md).

If you use [`McolDraw`](../../Reference/col/McolDraw.md) to draw the images, you must first enable the required control types to save the resulting images in the result buffer, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_SAVE_AREA_IMAGE`](../../Reference/col/McolControl.md) and [`M_GENERATE_...`](../../Reference/col/McolControl.md). For example, to draw [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md), you must first enable [`M_SAVE_AREA_IMAGE`](../../Reference/col/McolControl.md) and [`M_GENERATE_SAMPLE_COLOR_LUT`](../../Reference/col/McolControl.md).

Unless otherwise specified, all [`M_DRAW_...`](../../Reference/col/McolDraw.md) images are available with either [`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md). Images drawn using individual pixel match results ([`M_DRAW_PIXEL_MATCH_...`](../../Reference/col/McolDraw.md)) can only be specified when using an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) distance type.

Note that when image results include color elements or entire color-samples, they are drawn using either their triplet values ([`M_TRIPLET`](../../Reference/col/McolDefine.md)) or their estimated values (for example, the average) taken from a source image ([`M_IMAGE`](../../Reference/col/McolDefine.md)), depending on how you defined them ([`McolDefine`](../../Reference/col/McolDefine.md)). Color elements and color-samples are not drawn using the color of the target area.

### Target areas

For each target area, you can draw:

- The best-matched color-sample. The image can contain either:
  - The color of the best-matched color-sample ([`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolMatch.md)).
  - The label of the best-matched color-sample ([`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolMatch.md)).
- The color-sample for which each pixel voted. The image can contain either:
  - The color of the color-sample for which each pixel voted ([`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolMatch.md)).
  - The label of the color-sample for which each pixel voted ([`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolMatch.md)).
- The distance image.
  The distance image ([`M_DRAW_DISTANCE`](../../Reference/col/McolMatch.md)) contains the distance between the color of the target area (for an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode) or target pixel (for an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode), and the color of its best-matched color-sample. This distance value is dependent of the type of distance calculated. For more information, see [Distance between colors](S06_Distance_between_colors.md).
  If required, you can normalize distance results. For more information, see [Distance normalization settings](S08_Advanced_color_matching_settings.md).

The distance image ([`M_DRAW_DISTANCE`](../../Reference/col/McolMatch.md)) and the label images ([`M_..._USING_LABEL`](../../Reference/col/McolMatch.md)) draw numerical values and are therefore grayscale images. The other images ([`M_..._USING_COLOR`](../../Reference/col/McolMatch.md)) draw colors. For information on the size and depth of the required image buffer, use [`McolGetResult`](../../Reference/col/McolGetResult.md) or [`McolInquire`](../../Reference/col/McolInquire.md), as required. For example, to return the depth per band (in bits) required for the image buffer in which to draw [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolMatch.md), use [`McolGetResult`](../../Reference/col/McolGetResult.md) with [`M_SAMPLE_COLOR_SIZE_BIT`](../../Reference/col/McolGetResult.md).

The following image illustrates an example case where color matching is performed and the five target area results are drawn:

*[Image: color_drawing_examples_v2.png]*

You can also further process the distance image using other Aurora Imaging Library modules. For example, you can use the Blob Analysis module to identify blobs using the grayscale distance image and compute their features, such as area, perimeter, and min/max diameter, or use the Edge Finder module to detect crests and contours (note that Edge Finder can also be used directly with color images).

*[Image: Color_distance_image_analysis.png]*

### Color-samples and color elements

For each color-sample, you can draw:

- A copy of one of the internal color element images defined or added with [`McolDefine`](../../Reference/col/McolDefine.md) ([`M_DRAW_SAMPLE`](../../Reference/col/McolDraw.md)).
- A copy of one of the color element's masks defined or added with [`McolMask`](../../Reference/col/McolMask.md) ([`M_DRAW_SAMPLE_MASK`](../../Reference/col/McolDraw.md)).
- A copy of the mosaic image of all the color elements found in a specified sample defined with [`McolDefine`](../../Reference/col/McolDefine.md) ([`M_DRAW_SAMPLE_MOSAIC`](../../Reference/col/McolDraw.md)).
- A copy of the mosaic mask of the specified color-sample defined with [`McolDefine`](../../Reference/col/McolDefine.md) ([`M_DRAW_SAMPLE_MOSAIC_DONT_CARE`](../../Reference/col/McolDraw.md)).
- The 3-band color-sample label LUT, where the label value of each color-sample is associated with its average color ([`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md)).
  This image can be useful for supervised color segmentation. For example, you can draw the grayscale label image ([`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolMatch.md)), and then apply the LUT ([`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md)) to draw each pixel's color. Note that this produces the same result as drawing the colored label image ([`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolMatch.md)).

These images are only available with [`McolDraw`](../../Reference/col/McolDraw.md); they cannot be produced directly with [`McolMatch`](../../Reference/col/McolMatch.md).

### Background and outliers

Background pixels are pixels that are outside the target areas and are not used in the matching operation, while outlier pixels are pixels inside a target area, but do not match with any color-sample.

*[Image: Color_BackgroundVersusOutlier.png]*

When drawing image results, the destination buffer's bit depth must account for background and outlier pixels, if they are drawn. When drawing the distance image, ([`M_DRAW_DISTANCE`](../../Reference/col/McolMatch.md)), outliers are also drawn and can be difficult to identify if the outlier color is similar to the resulting distance. For example, if the outlier color is 0.0, you wouldn't be able to distinguish it if the resulting distance is also 0.0. In this case, you can use [`M_DRAW_..._USING_LABEL`](../../Reference/col/McolMatch.md), which draws the outlier label.

To set the outlier and background pixel value, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md). To set the outlier label, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_OUTLIER_LABEL`](../../Reference/col/McolControl.md). Note that unlike the outlier label, the background label is not specified explicitly; instead, it is taken from the background pixel color.

### Inverting colors

You can add the combination value [`M_INVERTED_COLORS`](../../Reference/col/McolDraw.md) to certain [`M_DRAW_...`](../../Reference/col/McolDraw.md) settings in [`McolDraw`](../../Reference/col/McolDraw.md) and [`McolMatch`](../../Reference/col/McolMatch.md) to invert the color of the best-matched color-sample in the resulting image. For inversion, Aurora Imaging Library uses the difference between each band value of the best-matched color-sample and the maximum possible value in the image. For example, with an 8-bit image, the color-sample band values are modified as follows: [255 - ColorSampleBand0, 255 - ColorSampleBand1, 255 - ColorSampleBand2]. In this case, if you use [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) and the best-matched color-sample value is [240, 10, 255], that color becomes [15, 245, 0] when you invert it ([`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) + [`M_INVERTED_COLORS`](../../Reference/col/McolDraw.md)).

In general, you can use [`M_INVERTED_COLORS`](../../Reference/col/McolDraw.md) to help increase the contrast between the pixels of the best-matched color-sample and the pixels of the underlying destination image. This can prove especially useful when both [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md) are set to [`M_TRANSPARENT`](../../Reference/col/McolControl.md).
