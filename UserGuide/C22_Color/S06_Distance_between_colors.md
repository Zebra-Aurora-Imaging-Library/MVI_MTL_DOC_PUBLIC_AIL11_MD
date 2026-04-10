---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Distance_between_colors
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Distance between colors"
---

# Distance between colors

It might be useful to calculate the distance between colors, for example, to find flaws in an image by comparing it to a perfect version of the image (golden template), or to a color constant.

*[Image: Color_DistanceBetweenImagesAndColorConstant.png]*

Distances between colors can either be calculated using [`McolDistance`](../../Reference/col/McolDistance.md), or when matching colors with [`McolMatch`](../../Reference/col/McolMatch.md). If required, you can normalize distance results. For more information, see [Distance normalization settings](S08_Advanced_color_matching_settings.md).

Using [`McolDistance`](../../Reference/col/McolDistance.md), you can calculate the point-to-point distance between colors in two sources. The first source must be an image, while the second source can be: an image, a color constant, a covariance matrix, or the covariance of a specified image. Results are written to the destination image.

*[Image: Color_DistanceBetweenTwoImagesPens.png]*

To ignore unwanted pixels in the distance calculation, you can use [`McolDistance`](../../Reference/col/McolDistance.md) with a mask image, within which you must identify the masked (non-zero) pixels. Set unmasked pixels to 0 in the mask. The color distance is calculated only for the masked (non-zero) pixels within the intersection of the source, destination, and mask images, with the assumption of a common origin at the top-left corner.

Color distances are also calculated when determining which color-sample best matches an image area. Therefore, you can retrieve distance results, after calling [`McolMatch`](../../Reference/col/McolMatch.md). The following image shows how distances between the target areas and multiple color-samples can be drawn. The distances drawn correspond to the distances between the target areas and their best-matched color-samples. Note that this is unlike [`McolDistance`](../../Reference/col/McolDistance.md), which can only calculate the distance between 2 sources.

*[Image: Color_DistanceMatching.png]*

For more information on retrieving color distances when matching colors, see [Image results](S07_Color_matching.md).

When calculating the color distance, make sure the color data that you provide is compatible. For example, you will not receive an error if you try to calculate the distance between an RGB and an HSL image, or a 16-bit and an 8-bit image. If the color data is not compatible, it is still processed as is (no error), which can produce misleading results. Note that when calculating color distance using [`McolMatch`](../../Reference/col/McolMatch.md), you must also consider the context's source color space. For more information, see [Source color space](S03_Color_spaces_and_converting_between_them.md).

Various conditions, such as different cameras and illuminants, can cause color from identical images to appear dissimilar. If this occurs, you can call [`McolTransform`](../../Reference/col/McolTransform.md) to perform a relative color calibration before calculating the color distance. Relative color calibration ensures all colors are consistent according to a specified reference color. For more information, see [Relative color calibration](S04_Relative_color_calibration.md).

## Differences in distances

The primary differences between distances calculated with [`McolMatch`](../../Reference/col/McolMatch.md) and [`McolDistance`](../../Reference/col/McolDistance.md) are:

- When the sources are images, [`McolDistance`](../../Reference/col/McolDistance.md) calculates a point-to-point distance, while [`McolMatch`](../../Reference/col/McolMatch.md) uses color statistics (average color). [`McolMatch`](../../Reference/col/McolMatch.md) calculates the distance between each color-sample's statistic and each target area's statistic, or between each color-sample's statistic and each pixel in each target area, depending on the operation mode specified with [`McolSetMethod`](../../Reference/col/McolSetMethod.md).
- [`McolDistance`](../../Reference/col/McolDistance.md) results are from two specified sources. However, [`McolMatch`](../../Reference/col/McolMatch.md) results can come from several color-samples, or even from background and outlying regions.
- If you are only interested in distance values, [`McolDistance`](../../Reference/col/McolDistance.md) can be more convenient to use than [`McolMatch`](../../Reference/col/McolMatch.md), since [`McolDistance`](../../Reference/col/McolDistance.md) requires less setup, does not perform a matching operation, and results are returned directly to the function.
- Unlike [`McolDistance`](../../Reference/col/McolDistance.md), [`McolMatch`](../../Reference/col/McolMatch.md) takes the specified context's color space into account; it also offers more options than [`McolDistance`](../../Reference/col/McolDistance.md), such as converting to the CIELAB color space and operating on specific color bands.

## Color distance types

Whether you are matching colors ([`McolMatch`](../../Reference/col/McolMatch.md)) or using [`McolDistance`](../../Reference/col/McolDistance.md), the color distance can be calculated using one of the following distance types (unless otherwise specified):

- Euclidean ([`M_EUCLIDEAN`](../../Reference/col/McolSetMethod.md)).
- Mahalanobis ([`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md)).
- Manhattan ([`M_MANHATTAN`](../../Reference/col/McolSetMethod.md)).
- Delta-E ([`M_DELTA_E`](../../Reference/col/McolSetMethod.md)).
- Advanced distance types, as established by the standards of the International Commission on Illumination (CIE).
  These distance types are only available for color matching ([`McolMatch`](../../Reference/col/McolMatch.md)).

When calculating the distance between colors, set the distance type with [`McolDistance`](../../Reference/col/McolDistance.md). When matching colors with [`McolMatch`](../../Reference/col/McolMatch.md), set the distance type with [`McolSetMethod`](../../Reference/col/McolSetMethod.md).

### Euclidean distance

A Euclidean distance is the square root of the sum of the squared differences between the color of the first source and the color of the second source. A Euclidean distance is generally regarded as a well-known standard distance calculation.

The following example illustrates how the distance between a green point, indicated by a circle, and two other green points, indicated by a triangle and a square, is measured with a Euclidean calculation.

*[Image: Color_ColorGraphEuclidean.png]*

A Euclidean distance can be represented with the following formula, where:

|   |   |
| --- | --- |
| *[Image: Color_FormulaEuclidean.png]* | - _r<sub>1</sub> _ and _r<sub>2</sub> _ represent the first color band of the first and second source color.<br/>- _g<sub>1</sub> _ and _g<sub>2</sub> _ represent the second color band of the first and second source color.<br/>- _b<sub>1</sub> _ and _b<sub>2</sub> _ represent the third color band of the first and second source color. |

### Manhattan distance

A Manhattan distance (also known as a City Block distance) is the sum of the absolute value of the differences between the color of the first source and the color of the second source. A Manhattan distance is generally considered the simplest distance calculation and is typically appropriate for calculating color distances between hue (H) bands in HSL. For example, using [`McolMatch`](../../Reference/col/McolMatch.md) with an HSL color space, and calculating the distance between the zero (H) bands ([`McolControl`](../../Reference/col/McolControl.md) with [`M_BAND_MODE`](../../Reference/col/McolControl.md) set to [`M_COLOR_BAND_0`](../../Reference/col/McolControl.md)).

The following example illustrates how the distance between a green point, indicated by a circle, and two other green points, indicated by a triangle and a square, is measured with a Manhattan calculation.

*[Image: Color_ColorGraphManhattan.png]*

A Manhattan distance can be represented with the following formula, where:

|   |   |
| --- | --- |
| *[Image: Color_FormulaManhattan.png]* | - _r<sub>1</sub> _ and _r<sub>2</sub> _ represent the first color band of the first and second source color.<br/>- _g<sub>1</sub> _ and _g<sub>2</sub> _ represent the second color band of the first and second source color.<br/>- _b<sub>1</sub> _ and _b<sub>2</sub> _ represent the third color band of the first and second source color. |

Note that in HSL the color's hue is stored as a separate band (H) represented as an angular position on a circular color disk. Therefore the distance between colors is equal to the smallest angular difference, rather than the absolute value of the difference.

### Mahalanobis distance

A Mahalanobis distance is calculated between the color of the first source and the covariance of the second source. If you are using [`McolDistance`](../../Reference/col/McolDistance.md) and the second source is an image, the covariance of that image, rather than the color of each pixel, is used to calculate the distance. If you are using [`McolSetMethod`](../../Reference/col/McolSetMethod.md), the covariance of the color-sample is used. A Mahalanobis distance is generally regarded as a slower, though more robust distance calculation for elongated color-samples.

The following example illustrates how the distance between a green point, indicated by a circle, and two other green points, indicated by a triangle and a square, is measured with a Mahalanobis calculation.

*[Image: Color_ColorGraphMahalanobis.png]*

A Mahalanobis distance can be represented with the following formula, where:

|   |   |
| --- | --- |
| *[Image: Color_FormulaMahalanobis.png]* | - _x_ represents the first source color.<br/>- _u_ represents the average of the second source color.<br/>- _sigma_ is for the covariance matrix of the second source color. |

The distance calculated for Mahalanobis, between a color and a distribution of colors (covariance), is similar to a Euclidean distance between the mean of the two colors, but weighted by the inverse of the covariance of the distribution. This implies that the more a color distribution varies in a direction within the color space, the less significant the distance is in that direction.

Since the covariance matrix of the second source is used, the second source should typically be a distribution of colors, such as an image, and not a single solid color (a color constant). However, if you provide a color constant as the second source, Mahalanobis behaves very much like Euclidean and will yield similar results.

### Delta-E distance

A Delta-E color distance is similar to a Euclidean color distance, but has been generally adjusted for the LAB (CIELAB) color space. This function assumes that the data is in the LAB color space; it does not convert the data before computing the distance. To convert the data, use [`MimConvert`](../../Reference/im/MimConvert.md)with [`M_SRGB_LINEAR_TO_LAB`](../../Reference/im/MimConvert.md) or [`M_SRGB_TO_LAB`](../../Reference/im/MimConvert.md).

### Advanced CIE distance types

In addition to [`M_DELTA_E`](../../Reference/col/McolSetMethod.md), Aurora Imaging Library offers more specialized types of CIE color distances, which you can specify using [`McolSetMethod`](../../Reference/col/McolSetMethod.md). These types are recommended for advanced users dealing with minor color variances in industrial color difference evaluation.

- CMC ([`M_CMC_ACCEPTABILITY`](../../Reference/col/McolSetMethod.md) and[`M_CMC_PERCEPTIBILITY`](../../Reference/col/McolSetMethod.md)).
  These distance types are generally intended for the textile industry and allow for lightness and chroma factors based on either acceptability or perceptibility requirements.
- CIE94 ([`M_CIE94_GRAPHIC_ARTS`](../../Reference/col/McolSetMethod.md) and [`M_CIE94_TEXTILE`](../../Reference/col/McolSetMethod.md)).
  These distance types are similar to CMC but allow for weighting factors based on color tolerances for either the graphic arts industry or the textile industry.
- CIEDE2000 ([`M_CIEDE2000`](../../Reference/col/McolSetMethod.md)).
  This distance type is similar to CIE94, but is generally more robust regarding the effect of lightness on color. If [`M_DELTA_E`](../../Reference/col/McolSetMethod.md) is proving ineffective, you might want to try [`M_CIEDE2000`](../../Reference/col/McolSetMethod.md) as a first alternative.

These distance types follow the standards of the CIE, as specified in their technical report on Colorimetry (CIE 15:2004). Refer to this document for more information.

## Choosing a distance type

Choosing the most appropriate distance type with which to calculate color distances depends on many factors, including the color space of your data, the background, and the particularities of your application. Typically, a Euclidean distance should be used for RGB and CIELAB color spaces, while a Manhattan distance should be used for HSL. A Mahalanobis color distance should be used when dealing with closely-related colors that are not expressed in HSL.

The following example illustrates an RGB source image of a grapefruit, which is to be used in a matching operation. Although for RGB colors a Euclidean distance is typically sufficient, in this case a Mahalanobis distance is preferable.

*[Image: Color_EuclideanVersusMahalanobis.png]*

In the source image, the color of the background and some parts of the grapefruit are similar; this makes Mahalanobis yield better results, since the covariance of the image is used. The pixels of the grapefruit correspond roughly to a distribution of shades of yellow, therefore, with a Mahalanobis distance, shades of yellow are considered to be closer to the grapefruit than other colors. To illustrate this point, the following image (a plotting of the color histogram) shows two separate groups of pixels displayed in RGB; one group is from the image's background, and the other is from the grapefruit.

*[Image: Color_RGBGraphEuclideanVersusMahalanobis.png]*

For each group of pixels (background and grapefruit), this image shows:

- The first principal component, indicated by the red lines. Each first principal component represents the direction of greatest standard deviation for its group of pixels.
- The mean color, indicated by the intersection of the blue line with the first principal component.

Encircled in black, on the left, are the background pixels that will match the grapefruit, being closer by Euclidean distance to the grapefruit's mean color. Encircled in black, on the right, are the grapefruit's pixels that will match the background, being closer by Euclidean distance to the background's mean color. However, with a Mahalanobis distance, any distance oriented parallel to the principal component (the red lines) will be scaled by the inverse of the standard deviation. Therefore, the encircled pixels will match with the correct group (background or grapefruit), yielding a better matching result.
