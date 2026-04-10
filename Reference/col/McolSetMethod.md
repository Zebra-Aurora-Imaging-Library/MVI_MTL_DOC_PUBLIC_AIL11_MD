---
doctype: Reference
module: col
function: McolSetMethod
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolSetMethod"
---

# McolSetMethod

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Set the color matching or relative color calibration strategy.

## Syntax

```c
void McolSetMethod(
    AIL_ID    ContextId,                      //in
    AIL_INT64 OperationMode,                  //in
    AIL_INT64 DistTypeOrCalIntent,            //in
    AIL_INT64 ConversionModeOrComputeOption,  //in
    AIL_INT64 ControlFlag                     //in
)
```

## Description

This function sets the strategy with which to perform color matching or relative color calibration, when calling [`McolMatch`](../../Reference/col/McolMatch.md) or [`McolTransform`](../../Reference/col/McolTransform.md), respectively. The strategy is based on the operation and either the color distance (for color matching) or the color calibration intent (for relative color calibration). By manipulating these settings, you can affect the resulting color-samples that Aurora Imaging Library matches to the target areas, or the color-sample mappings that Aurora Imaging Library uses to transform color data with relative color calibration.

If you perform color matching using an RGB source color space, you can use this function to convert your RGB color data to a different color space before the match operation. To do so, explicitly specify a conversion mode ([`ConversionModeOrComputeOption`](../../Reference/col/McolSetMethod.md)).

The strategy set with this function is part of the specified context. For this function's settings to take effect, you must preprocess the context ([`McolPreprocess`](../../Reference/col/McolPreprocess.md)).

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the color matching or relative color calibration context. The context must have been previously allocated on the required system using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md) or [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md).

### `OperationMode` *(in, AIL_INT64)*

Specifies the operation.

*For specifying the distance operation (color matching)*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md). |
| `M_HISTOGRAM_MATCHING` | Specifies a minimum distance operation, based on color histograms. When using [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md), you must set [`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md) to [`M_ABSOLUTE`](../../Reference/col/McolControl.md).

Color histograms are computed for each target area and defined color-sample, and a match score between each target area histogram and the color-sample histograms is generated. The color-sample with the highest score above the acceptance ([`McolControl`](../../Reference/col/McolControl.md) with [`M_ACCEPTANCE`](../../Reference/col/McolControl.md)) is the target area's best-matched color-sample.

Note that you cannot use [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) if the distance type is [`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md). |
| `M_HISTOGRAM_VOTE` | Specifies a match operation based on histogram voting.

Aurora Imaging Library computes color histograms for each defined color-sample and, for each target pixel, identifies its histogram bin. Each target pixel votes for the best color-sample, provided its histogram bin is not empty. The number of votes that a color-sample accumulates determines its score. The color-sample with the highest score above the acceptance ([`McolControl`](../../Reference/col/McolControl.md) with [`M_ACCEPTANCE`](../../Reference/col/McolControl.md)) is the target area's best-matched color-sample. In the case of a tie, where more than one color-sample receives the same highest number of votes, the color-sample with the highest label value is chosen as the best-match.

With histogram voting, you must set the distance type to [`M_NONE`](../../Reference/col/McolSetMethod.md). |
| `M_MIN_DIST_VOTE` | Specifies a minimum distance operation, based on pixel voting.

Color statistics are calculated (typically the mean) for each color-sample defined in the context, and the color distance between each pixel in each target area and each color-sample is determined. Each target pixel then votes for the closest color-sample, which is also within the distance tolerance ([`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md)).

The number of votes that a color-sample accumulates determines its score. The color-sample with the highest score above the acceptance ([`McolControl`](../../Reference/col/McolControl.md) with [`M_ACCEPTANCE`](../../Reference/col/McolControl.md)) is the target area's best-matched color-sample. In the case of a tie, where more than one color-sample receives the same highest number of votes, the color-sample with the highest label value is chosen as the best-match. |
| `M_STAT_MIN_DIST` | Specifies a minimum distance operation, based on pixel statistics.

Color statistics are calculated (typically the mean) for each target area and for each color-sample defined in the context, and the color distance between each target area and each color-sample is determined.

The resulting distances determine the score of the color-samples. If a distance is not within a color-sample's distance tolerance, this color-sample's score is 0%. The color-sample with the highest score above the acceptance ([`McolControl`](../../Reference/col/McolControl.md) with [`M_ACCEPTANCE`](../../Reference/col/McolControl.md)) is the target area's best-matched color-sample. |

*For specifying the mapping operation (relative color calibration)*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md). |
| `M_COLOR_TO_COLOR` | Specifies that a color-sample's mapping is based on transforming its color to the color of the reference color-sample on a point-to-point basis. It is recommended to have a pixel-wise correspondence between the color-sample and the reference color-sample.

[`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) is the most precise operation, and the least flexible. Minor color variations and outlier color data can have a major impact. Use this operation when the conditions of your application are stable and you require an exact reproduction of color. |
| `M_GLOBAL_MEAN_VARIANCE` | Specifies that a color-sample's mapping is based on transforming its mean and variance (standard deviation) to resemble the mean and variance of the reference color-sample.

This is the fastest operation, and the least precise (most general). Use it when the color distribution of the reference color-sample and the color-samples is dissimilar, when conditions are not stable, or when other operations fail. [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) can avoid the introduction of false colors if the acquisition scene (image and color data) varies dramatically between the reference color-sample and the color-samples. |
| `M_HISTOGRAM_BASED` | Specifies that a color-sample's mapping is based on using enhanced color distribution information to transform its color data to better resemble the color data of the reference color-sample. Use this operation when the color distribution between the reference color-sample and the color-samples is similar.

[`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md) is a compromise between point-to-point (high precision, low flexibility) and global mean variance (low precision, high flexibility) operations. It offers good precision without requiring a point-to-point correspondence. It also offers good flexibility, provided that colors are somewhat comparable. This operation is typically the longest to process. |

### `DistTypeOrCalIntent` *(in, AIL_INT64)*

Specifies the type of distance (for color matching) or color calibration intent (for relative color calibration) that the operation uses. Distance is the difference in color between the color-sample and the target area. Except for the [`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md) distance type, which uses the covariance of the color-sample, Aurora Imaging Library uses the mean of the colors to calculate the distance. Color calibration intent is the extent to which the color data of the color-samples must resemble the color data of the reference color-sample.

### `ConversionModeOrComputeOption` *(in, AIL_INT64)*

Specifies the conversion mode (for color matching) or compute option (for relative color calibration) that the operation uses. Conversion mode refers to the color space to which the RGB source color data will be converted before performing [`McolMatch`](../../Reference/col/McolMatch.md). You can only convert RGB source color data to CIELAB or HSL when matching colors. By default, this type of conversion is not performed (or allowed), and you should set this parameter to `M_DEFAULT` (same as [`M_NONE`](../../Reference/col/McolSetMethod.md)). Compute option indicates whether the operation uses all pixel values or color statistics.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the type of distance calculation (color matching)

The following [`ContextId`](../../Reference/col/McolSetMethod.md), [`DistTypeOrCalIntent`](../../Reference/col/McolSetMethod.md), and [`ConversionModeOrComputeOption`](../../Reference/col/McolSetMethod.md) parameter settings specify the type of distance calculation to use when calling [`McolMatch`](../../Reference/col/McolMatch.md). The [`ContextId`](../../Reference/col/McolSetMethod.md) parameter must indicate the identifier of a color matching context.  Except for converting RGB source color data to CIELAB or HSL when matching colors, the [`ConversionModeOrComputeOption`](../../Reference/col/McolSetMethod.md) parameter is unused and should be set to [`M_DEFAULT`](../../Reference/col/McolSetMethod.md) (same as [`M_NONE`](../../Reference/col/McolSetMethod.md)).  The settings in the following table are restricted according to the color matching context's source color space, as set using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_RGB`](../../Reference/col/McolAlloc.md), [`M_HSL`](../../Reference/col/McolAlloc.md), or [`M_CIELAB`](../../Reference/col/McolAlloc.md). Note that the only distance type available for an [`M_HSL`](../../Reference/col/McolAlloc.md) source color space, as indicated in the table below, is [`M_MANHATTAN`](../../Reference/col/McolSetMethod.md) (which is also the default for HSL).  The [`M_CMC...`](../../Reference/col/McolSetMethod.md) and [`M_CIE...`](../../Reference/col/McolSetMethod.md) settings are recommended for advanced users and are typically applied to improve the Delta-E performance when dealing with minor color variances in industrial color difference evaluation. These distance types follow the standards of the International Commission on Illumination (CIE), as established in their technical report on Colorimetry (CIE 15:2004).

---

### `CIELAB color matching context ID`

Specifies a color matching context that is using an [`M_CIELAB`](../../Reference/col/McolAlloc.md) source color space.

#### `M_DEFAULT`

Specifies the default distance type. The default is the same as [`M_EUCLIDEAN`](../../Reference/col/McolSetMethod.md).

#### `M_CIE94_GRAPHIC_ARTS`

Specifies to use the CIE94 color distance with weighting factors pertaining to the graphic arts industry, as established by the CIE. The weighting factors are: _KL_ = 1, _K1_ = 0.045, and _K2_ = 0.015. For more information, including the relevant formulas, refer to the technical report on Colorimetry (CIE 15:2004).

#### `M_CIE94_TEXTILE`

Specifies to use the CIE94 color distance with weighting factors pertaining to the textile industry, as established by the CIE. The weighting factors are: _KL_ = 2, _K1_ = 0.048, _K2_ = 0.014. For more information, including the relevant formulas, refer to the technical report on Colorimetry (CIE 15:2004).

#### `M_CIEDE2000`

Specifies to use the CIEDE2000 color distance, as established by the CIE.

#### `M_CMC_ACCEPTABILITY`

Specifies to use the CMC(_l_:_c_) color distance that has been adjusted for acceptability evaluation, as established by the CIE. The adjusted parameters are: lightness (_l_) = 2 and chroma (_c_) = 1. For more information, including the relevant formulas, refer to the technical report on Colorimetry (CIE 15:2004).

#### `M_CMC_PERCEPTIBILITY`

Specifies to use the CMC(_l_:_c_) color distance that has been adjusted for perceptibility evaluation, as established by the CIE. The adjusted parameters are: lightness (_l_) = 1 and chroma (_c_) = 1. For more information, including the relevant formulas, refer to the technical report on Colorimetry (CIE 15:2004).

#### `M_DELTA_E`

Specifies to use a Delta-E color distance, as defined by the International Commission on Illumination (CIE).  A Delta-E color distance is the same as a Euclidean color distance ([`M_EUCLIDEAN`](../../Reference/col/McolSetMethod.md)), but specialized for CIELAB.

#### `M_EUCLIDEAN`

Specifies to use a Euclidean color distance.  A Euclidean distance is the square root of the sum of the squared differences between the color of the color-sample and the color of the target area.

#### `M_MAHALANOBIS`

Specifies to use a Mahalanobis color distance.  A Mahalanobis distance is calculated between the color of the target area and, typically, the covariance of the color-sample. This distance, between a color and a distribution of colors, is similar to a Euclidean distance between the mean of the two colors, but weighted by the inverse of the covariance of the distribution. This implies that the more a color distribution varies in a direction within the color space, the less important is the distance in that direction.  Since the covariance matrix of the color-sample is used, the color-sample should usually come from a an image, and not be defined as a triplet ([`McolDefine`](../../Reference/col/McolDefine.md) with [`M_IMAGE`](../../Reference/col/McolDefine.md)instead of [`M_TRIPLET`](../../Reference/col/McolDefine.md)). However, if you provide a color constant (triplet) as the second source, Mahalanobis behaves very much like Euclidean and will yield similar results.  [`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md) is not available if the operation mode is [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md).

#### `M_MANHATTAN`

Specifies to use a Manhattan color distance.  A Manhattan distance is the sum of the absolute value of the differences between the color of the color-sample and the color of the target area. For an HSL color space, the distance between angular coordinates is equal to the smallest angular difference, rather than the absolute value of the difference.

#### `M_NONE`

Specifies that no distance calculations are used. Instead, pixels in the target image vote for histogram bins. The histogram bins each have a range of values, and each pixel in the target image votes for the histogram bin whose range includes its value.  [`M_NONE`](../../Reference/col/McolSetMethod.md) can only be set with [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md), and vice versa.

---

### `HSL color matching context ID`

Specifies a color matching context that is using an [`M_HSL`](../../Reference/col/McolAlloc.md) source color space.

#### `M_DEFAULT`

Specifies the default distance type. The default is the same as [`M_MANHATTAN`](../../Reference/col/McolSetMethod.md).

#### `M_MANHATTAN`

Specifies to use a Manhattan color distance.  A Manhattan distance is the sum of the absolute value of the differences between the color of the color-sample and the color of the target area. For an HSL color space, the distance between angular coordinates is equal to the smallest angular difference, rather than the absolute value of the difference.

#### `M_NONE`

Specifies that no distance calculations are used. Instead, pixels in the target image vote for histogram bins. The histogram bins each have a range of values, and each pixel in the target image votes for the histogram bin whose range includes its value.  [`M_NONE`](../../Reference/col/McolSetMethod.md) can only be set with [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md), and vice versa.

---

### `RGB color matching context ID`

Specifies a color matching context that is using an [`M_RGB`](../../Reference/col/McolAlloc.md) source color space.

#### `M_DEFAULT`

Specifies the default distance type. The default is the same as [`M_EUCLIDEAN`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation.  When converting RGB source colors before the match using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the CIELAB conversion mode, Aurora Imaging Library requires uncorrected (linear) color data. Therefore, if gamma correction has been applied on the RGB source color data, you must remove it using [`McolControl`](../../Reference/col/McolControl.md) with [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) set to [`M_ENABLE`](../../Reference/col/McolControl.md). |
| `M_HSL` | Specifies to internally convert RGB colors to HSL before the match operation. |
| `M_NONE` *(default)* | Specifies no internal conversion. |

#### `M_CIE94_GRAPHIC_ARTS`

Specifies to use the CIE94 color distance with weighting factors pertaining to the graphic arts industry, as established by the CIE. The weighting factors are: _KL_ = 1, _K1_ = 0.045, and _K2_ = 0.015. For more information, including the relevant formulas, refer to the technical report on Colorimetry (CIE 15:2004).

| Value | Description |
| --- | --- |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation. |

#### `M_CIE94_TEXTILE`

Specifies to use the CIE94 color distance with weighting factors pertaining to the textile industry, as established by the CIE. The weighting factors are: _KL_ = 2, _K1_ = 0.048, _K2_ = 0.014. For more information, including the relevant formulas, refer to the technical report on Colorimetry (CIE 15:2004).

| Value | Description |
| --- | --- |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation. |

#### `M_CIEDE2000`

Specifies to use the CIEDE2000 color distance, as established by the CIE.

| Value | Description |
| --- | --- |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation. |

#### `M_CMC_ACCEPTABILITY`

Specifies to use the CMC(_l_:_c_) color distance that has been adjusted for acceptability evaluation, as established by the CIE. The adjusted parameters are: lightness (_l_) = 2 and chroma (_c_) = 1. For more information, including the relevant formulas, refer to the technical report on Colorimetry (CIE 15:2004).

| Value | Description |
| --- | --- |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation. |

#### `M_CMC_PERCEPTIBILITY`

Specifies to use the CMC(_l_:_c_) color distance that has been adjusted for perceptibility evaluation, as established by the CIE. The adjusted parameters are: lightness (_l_) = 1 and chroma (_c_) = 1. For more information, including the relevant formulas, refer to the technical report on Colorimetry (CIE 15:2004).

| Value | Description |
| --- | --- |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation. |

#### `M_DELTA_E`

Specifies to use a Delta-E color distance, as defined by the International Commission on Illumination (CIE).  A Delta-E color distance is the same as a Euclidean color distance ([`M_EUCLIDEAN`](../../Reference/col/McolSetMethod.md)), but specialized for CIELAB.

| Value | Description |
| --- | --- |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation. |

#### `M_EUCLIDEAN`

Specifies to use a Euclidean color distance.  A Euclidean distance is the square root of the sum of the squared differences between the color of the color-sample and the color of the target area.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation.  When converting RGB source colors before the match using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the CIELAB conversion mode, Aurora Imaging Library requires uncorrected (linear) color data. Therefore, if gamma correction has been applied on the RGB source color data, you must remove it using [`McolControl`](../../Reference/col/McolControl.md) with [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) set to [`M_ENABLE`](../../Reference/col/McolControl.md). |
| `M_NONE` *(default)* | Specifies no internal conversion. |

#### `M_MAHALANOBIS`

Specifies to use a Mahalanobis color distance.  A Mahalanobis distance is calculated between the color of the target area and, typically, the covariance of the color-sample. This distance, between a color and a distribution of colors, is similar to a Euclidean distance between the mean of the two colors, but weighted by the inverse of the covariance of the distribution. This implies that the more a color distribution varies in a direction within the color space, the less important is the distance in that direction.  Since the covariance matrix of the color-sample is used, the color-sample should usually come from a an image, and not be defined as a triplet ([`McolDefine`](../../Reference/col/McolDefine.md) with [`M_IMAGE`](../../Reference/col/McolDefine.md)instead of [`M_TRIPLET`](../../Reference/col/McolDefine.md)). However, if you provide a color constant (triplet) as the second source, Mahalanobis behaves very much like Euclidean and will yield similar results.  [`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md) is not available if the operation mode is [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation. |
| `M_NONE` *(default)* | Specifies no internal conversion. |

#### `M_MANHATTAN`

Specifies to use a Manhattan color distance.  A Manhattan distance is the sum of the absolute value of the differences between the color of the color-sample and the color of the target area. For an HSL color space, the distance between angular coordinates is equal to the smallest angular difference, rather than the absolute value of the difference.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation.  When converting RGB source colors before the match using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the CIELAB conversion mode, Aurora Imaging Library requires uncorrected (linear) color data. Therefore, if gamma correction has been applied on the RGB source color data, you must remove it using [`McolControl`](../../Reference/col/McolControl.md) with [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) set to [`M_ENABLE`](../../Reference/col/McolControl.md). |
| `M_HSL` | Specifies to internally convert RGB colors to HSL before the match operation. |
| `M_NONE` *(default)* | Specifies no internal conversion. |

#### `M_NONE`

Specifies that no distance calculations are used. Instead, pixels in the target image vote for histogram bins. The histogram bins each have a range of values, and each pixel in the target image votes for the histogram bin whose range includes its value.  [`M_NONE`](../../Reference/col/McolSetMethod.md) can only be set with [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md), and vice versa.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIELAB` | Specifies to internally convert RGB colors to CIELAB before the match operation.  When converting RGB source colors before the match using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the CIELAB conversion mode, Aurora Imaging Library requires uncorrected (linear) color data. Therefore, if gamma correction has been applied on the RGB source color data, you must remove it using [`McolControl`](../../Reference/col/McolControl.md) with [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) set to [`M_ENABLE`](../../Reference/col/McolControl.md). |
| `M_HSL` | Specifies to internally convert RGB colors to HSL before the match operation. |
| `M_NONE` *(default)* | Specifies no internal conversion. |

### For specifying the color calibration intent (quality) of the mapping operation (relative color calibration)

The following [`ContextId`](../../Reference/col/McolSetMethod.md), [`DistTypeOrCalIntent`](../../Reference/col/McolSetMethod.md), and [`ConversionModeOrComputeOption`](../../Reference/col/McolSetMethod.md) parameter settings specify the color calibration intent (quality) with which to perform the operation that Aurora Imaging Library uses to establish the mapping it associates to color-samples. These settings can increase or decrease the accuracy inherent in the operation. The [`ContextId`](../../Reference/col/McolSetMethod.md) parameter must indicate the identifier of a color matching context. Unless otherwise specified, values are available for all operations.  Except for converting RGB source color data to CIELAB or HSL when matching colors, the [`ConversionModeOrComputeOption`](../../Reference/col/McolSetMethod.md) parameter is unused and should be set to [`M_DEFAULT`](../../Reference/col/McolSetMethod.md) (same as [`M_NONE`](../../Reference/col/McolSetMethod.md)).

---

### `Relative color calibration context ID`

Specifies a relative color calibration context. If using [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md), set this parameter to [`M_DEFAULT`](../../Reference/col/McolSetMethod.md).

#### `M_DEFAULT`

Specifies the default color calibration intent. For [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) and [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md), the default is [`M_BALANCE`](../../Reference/col/McolSetMethod.md). For [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md), the default is [`M_GENERALIZATION`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COMPUTE_ITEM_PIXELS` *(default)* | Specifies that Aurora Imaging Library uses all pixel values (in the reference color-sample and the color-samples) to generate the color information that the operation requires. |
| `M_COMPUTE_ITEM_STAT` | Specifies that Aurora Imaging Library uses color statistics (in the reference color-sample and the color-samples) to generate the color information that the operation requires. You can only specify this value if using an [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) operation. |

#### `M_BALANCE`

Specifies that the operation considers a moderate amount of color information to establish the mapping. Though this setting offers less accuracy than [`M_PRECISION`](../../Reference/col/McolSetMethod.md), there is little risk of overfitting the color data.  You cannot specify [`M_BALANCE`](../../Reference/col/McolSetMethod.md) if you are using an [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COMPUTE_ITEM_PIXELS` *(default)* | Specifies that Aurora Imaging Library uses all pixel values (in the reference color-sample and the color-samples) to generate the color information that the operation requires. |
| `M_COMPUTE_ITEM_STAT` | Specifies that Aurora Imaging Library uses color statistics (in the reference color-sample and the color-samples) to generate the color information that the operation requires. You can only specify this value if using an [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) operation. |

#### `M_GENERALIZATION`

Specifies that the operation considers a minimal amount of color information to establish the mapping. This setting offers the least precise color mapping. However, there is little risk of overfitting the color data, and it is the fastest calculation, which can prove useful for large images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COMPUTE_ITEM_PIXELS` *(default)* | Specifies that Aurora Imaging Library uses all pixel values (in the reference color-sample and the color-samples) to generate the color information that the operation requires. |
| `M_COMPUTE_ITEM_STAT` | Specifies that Aurora Imaging Library uses color statistics (in the reference color-sample and the color-samples) to generate the color information that the operation requires. You can only specify this value if using an [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) operation. |

#### `M_PRECISION`

Specifies that the operation considers a high amount of color information to establish the mapping. This setting offers the most precise color mapping between the reference color-sample and the color-samples. However, it can potentially overfit the color data. For example, if you perform the relative color calibration on grabbed images containing outlier colors that were not considered during preprocessing, incorrect color transformations can occur.  You cannot specify [`M_PRECISION`](../../Reference/col/McolSetMethod.md) if you are using an [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COMPUTE_ITEM_PIXELS` *(default)* | Specifies that Aurora Imaging Library uses all pixel values (in the reference color-sample and the color-samples) to generate the color information that the operation requires. |
| `M_COMPUTE_ITEM_STAT` | Specifies that Aurora Imaging Library uses color statistics (in the reference color-sample and the color-samples) to generate the color information that the operation requires. You can only specify this value if using an [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) operation. |

This distance type does not support band selection; that is, [`McolControl`](../../Reference/col/McolControl.md) with [`M_BAND_MODE`](../../Reference/col/McolControl.md) must be set to [`M_ALL_BANDS`](../../Reference/col/McolControl.md) (or [`M_DEFAULT`](../../Reference/col/McolControl.md)).
