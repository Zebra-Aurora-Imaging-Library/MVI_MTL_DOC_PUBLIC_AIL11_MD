---
doctype: Reference
module: col
function: McolControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolControl"
---

# McolControl

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

> Control a color matching context, its color-samples, or the color-samples of a relative color calibration context.

## Syntax

```c
void McolControl(
    AIL_ID     ContextId,     //out
    AIL_INT    IndexOrLabel,  //in
    AIL_INT64  ControlType,   //in
    AIL_DOUBLE ControlValue   //in
)
```

## Description

This function sets the specified control type for a color matching context, its color-samples, or the color-samples of a relative color calibration context. These control type settings affect the execution of [`McolPreprocess`](../../Reference/col/McolPreprocess.md), [`McolMatch`](../../Reference/col/McolMatch.md), [`McolTransform`](../../Reference/col/McolTransform.md), and [`McolDraw`](../../Reference/col/McolDraw.md). Typically, you can inquire the control type setting, using [`McolInquire`](../../Reference/col/McolInquire.md).

Changing certain control type settings require you to preprocess the context again, using [`McolPreprocess`](../../Reference/col/McolPreprocess.md). To know if the context needs to be preprocessed, call [`McolInquire`](../../Reference/col/McolInquire.md) with the [`M_PREPROCESSED`](../../Reference/col/McolInquire.md) inquire type.

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the identifier of the color matching or relative color calibration context. The context must have been previously allocated on the required system using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md) or [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md).

### `IndexOrLabel` *(in, AIL_INT)*

Specifies what to control. Set this parameter to one of the following values. Unless otherwise specified, these values are only available for color matching contexts.

*For specifying what to control*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CONTEXT`](../../Reference/col/McolControl.md). |
| `M_SAMPLE_INDEX` | Specifies the index of the color-sample to control. This value is available for color matching and relative color calibration contexts. |
| `M_SAMPLE_LABEL` | Specifies the label of the color-sample to control. This value is available for color matching and relative color calibration contexts. |
| `M_ALL` | Specifies to control all color-samples within the color matching context. The specified control type setting must be supported by all the color-samples. |
| `M_CONTEXT` | Specifies to control a global setting of the color matching context. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For controlling general color matching context settings

The following [`ControlType`](../../Reference/col/McolControl.md) and corresponding [`ControlValue`](../../Reference/col/McolControl.md) parameter settings are used to control general color matching context settings. In this case, you must set the [`IndexOrLabel`](../../Reference/col/McolControl.md) parameter to [`M_CONTEXT`](../../Reference/col/McolControl.md). These values are only available for color matching contexts.

---

### `M_ACCEPTANCE`

Sets the acceptance level for the color-sample's score ([`M_SCORE`](../../Reference/col/McolGetResult.md) with [`McolGetResult`](../../Reference/col/McolGetResult.md)). This score indicates the similarity between the color of the color-sample and the color of the target area. The higher the acceptance, the closer the colors must be for them to match.  For a match, the color-sample must have a score that is greater than or equal to this level.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. |

---

### `M_ACCEPTANCE_RELEVANCE`

Sets the acceptance level for the target area's relevance score ([`M_SCORE_RELEVANCE`](../../Reference/col/McolGetResult.md)). This score indicates the significance (relevance) of the match score ([`M_SCORE`](../../Reference/col/McolGetResult.md)). In statistics, this is similar to the confidence level; that is, it represents how confident you can be that the best-matched color-sample actually is the best possible match. For example, a high relevance acceptance indicates that the best-matched color-sample must be vastly superior to all other color-samples, while a low relevance acceptance indicates that the best-matched color-sample only needs to be marginally superior to the other color-samples.  A target area can only have a successful matching status ([`M_STATUS`](../../Reference/col/McolGetResult.md) with [`McolGetResult`](../../Reference/col/McolGetResult.md)) if its relevance score is above the relevance acceptance level. A target area with a successful matching status also requires at least one color-sample score to be above [`M_ACCEPTANCE`](../../Reference/col/McolControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable relevance score, as a percentage. |

---

### `M_BACKGROUND_DRAW_COLOR`

Sets the color to use to draw background pixels when drawing or generating the color images ([`M_DRAW_..._USING_COLOR`](../../Reference/col/McolMatch.md)), the distance image ([`M_DRAW_DISTANCE`](../../Reference/col/McolMatch.md)), or the grayscale label images ([`M_DRAW_..._USING_LABEL`](../../Reference/col/McolDraw.md)), using [`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md).  A pixel is considered background when it is outside the target areas. For background pixels to exist, you must specify an area identifier image when calling [`McolMatch`](../../Reference/col/McolMatch.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COLOR888` | Specifies a 24-bit color value (eight bits per band) with which to set the background pixels. The color value is taken as is (no conversion), according to the source color space. Typically, this should be RGB, since the background color is meant for display.  When [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) is set with **M_COLOR888()**, only the first component (for example, red) will be used to draw or generate [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md), which is a grayscale image. |
| `M_TRANSPARENT` *(default)* | Specifies not to change the background. Therefore, the background pixels will be those of the specified destination image. |
| `Value > 0` | Specifies a grayscale value for the background pixels, as a _double_ or _integer_.  When drawing the distance image ([`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)) in a 32-bit integer buffer, you should keep the [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) value under 24-bit (that is, less than 16,777,215). |

---

### `M_BAND_MODE`

Specifies the color bands to use when performing a match. This applies to all color-samples in the context and to the target areas.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL_BANDS` *(default)* | Specifies to use all bands. |
| `M_COLOR_BAND_0` | Specifies to use band 0. |
| `M_COLOR_BAND_0 + M_COLOR_BAND_1` | Specifies to use bands 0 and 1. |
| `M_COLOR_BAND_0 + M_COLOR_BAND_2` | Specifies to use bands 0 and 2. |
| `M_COLOR_BAND_1` | Specifies to use band 1. |
| `M_COLOR_BAND_1 + M_COLOR_BAND_2` | Specifies to use bands 1 and 2. |
| `M_COLOR_BAND_2` | Specifies to use band 2. |

---

### `M_CONVERSION_GAMMA`

Specifies whether to remove gamma correction when internally converting RGB source colors before the match using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the CIELAB conversion mode. Gamma correction refers to color data that has been processed, typically during data acquisition (that is, by the camera doing the grab), to compensate for a non-linear transformation between a pixel's value and its displayed intensity on the display device.  When converting RGB source colors to CIELAB, Aurora Imaging Library requires linear color data; that is, data that is uncorrected. Therefore, if gamma correction has been applied on the RGB source color data, you must remove it. Unless you are using the CIELAB conversion mode in an RGB source color space, [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to remove gamma correction. In this case, your RGB color data should not be in a corrected state (it should be linear). For example, the camera capturing the RGB color data has not applied a gamma correction. |
| `M_ENABLE` *(default)* | Specifies to remove gamma correction. In this case, your RGB color data should be in a corrected state (it should be non-linear). For example, the camera capturing the RGB color data has applied a gamma correction.  Aurora Imaging Library assumes that your color data has been corrected according to the standards of the context's reference color space, as specified using [`McolAlloc`](../../Reference/col/McolAlloc.md) with the [`ColorSpaceProfileId`](../../Reference/col/McolAlloc.md) parameter. Aurora Imaging Library uses these standards (for example, sRGB) to remove the gamma correction. |

---

### `M_DISTANCE_IMAGE_NORMALIZE`

Sets the normalization factor when drawing or generating [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md), using [`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md).  Distance results are always clipped at the distance buffer's maximal possible value, which can occur when specifying [`M_NO_NORMALIZE`](../../Reference/col/McolControl.md) or a specific normalization factor. For example, when using an 8-bit unsigned distance buffer, distances higher than 255 will be written as 255. Note that if you specify a distance buffer of type _floating-point_, distance results would almost never be greater than the buffer's maximal possible value, since its limit is extremely high.  Aurora Imaging Library ignores [`M_DISTANCE_IMAGE_NORMALIZE`](../../Reference/col/McolControl.md) when using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX_NORMALIZE` | Specifies to normalize distance values according to the greatest calculated distance. This value corresponds to the result [`M_MAX_DISTANCE`](../../Reference/col/McolGetResult.md) ([`McolGetResult`](../../Reference/col/McolGetResult.md)). The normalization performed is:  - `(_Distance_/_GreatestCalculatedDistance_) x _MaximumPossibleValueOfDistBuf_` (for distance buffer's of type _integer_). - `(_Distance_/_GreatestCalculatedDistance_)` (for distance buffer's of type _floating-point_).   Note that if the distance buffer is of type _floating-point_, you would typically not normalize the distance results, since all this will do is return a value between 0.0 and 1.0. |
| `M_NO_NORMALIZE` *(default)* | Specifies no normalization. For distance buffers of type _integer_, distance values are truncated (no decimals). For distance buffers of type _floating-point_, distance values are complete (with decimals). |
| `Value > 0.0` | Specifies to normalize distance values according to the specified normalization value. The normalization performed is:  - `(_Distance_/_NormalizationValue_) x _MaximumPossibleValueOfDistBuf_` (for distance buffer's of type _integer_). - `(_Distance_/_NormalizationValue_)` (for distance buffer's of type _floating-point_).   Note that if the distance buffer is of type _floating-point_, you would typically not normalize the distance results, since all this will do is return a value between 0.0 and 1.0. |

---

### `M_DISTANCE_TOLERANCE_MODE`

Sets the strategy with which to use the tolerance value ([`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md)) to calculate the acceptable (matching) color distance (tolerance) between the color-sample and a target area.  When using an [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) operation mode ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)), you must set [`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md) to [`M_ABSOLUTE`](../../Reference/col/McolControl.md). When using an [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) operation mode, Aurora Imaging Library ignores [`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` *(default)* | Specifies that the [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) value will be untouched and used as the acceptable distance tolerance. |
| `M_RELATIVE` |  |
| `M_SAMPLE_STDDEV` |  |

---

### `M_GENERATE_DISTANCE_IMAGE`

Sets whether to generate the distance image and store it in the result buffer. This image contains the difference between the color of the target area and the color of its best-matched color-sample.  Aurora Imaging Library ignores [`M_GENERATE_DISTANCE_IMAGE`](../../Reference/col/McolControl.md) when using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to generate the image. |
| `M_ENABLE` | Specifies to generate the image. |

---

### `M_GENERATE_PIXEL_MATCH`

Sets whether to generate the image containing the label of the color-sample for which each pixel voted. This image is only available if you use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to generate the image. |
| `M_ENABLE` | Specifies to generate the image. |

---

### `M_GENERATE_SAMPLE_COLOR_LUT`

Sets whether to generate the color-sample label LUT and store it in the result buffer. This LUT contains the label value of each color-sample and its associated average color.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to generate the LUT. |
| `M_ENABLE` | Specifies to generate the LUT. |

---

### `M_NB_BINS_BAND_0`

Sets the number of histogram bins for color band 0. Relevant only when matching colors using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).  The number of bins situated along the axis of band 0 determine the size of the bins for that band. If you multiply the number of bins situated along the axis of each band ([`M_NB_BINS_BAND_0`](../../Reference/col/McolControl.md), [`M_NB_BINS_BAND_1`](../../Reference/col/McolControl.md), and [`M_NB_BINS_BAND_2`](../../Reference/col/McolControl.md)) you obtain the total number of bins.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of histogram bins for color band 0. |

---

### `M_NB_BINS_BAND_1`

Sets the number of histogram bins for color band 1. Relevant only when matching colors using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).  The number of bins situated along the axis of band 1 determine the size of the bins for that band. If you multiply the number of bins situated along the axis of each band ([`M_NB_BINS_BAND_0`](../../Reference/col/McolControl.md), [`M_NB_BINS_BAND_1`](../../Reference/col/McolControl.md), and [`M_NB_BINS_BAND_2`](../../Reference/col/McolControl.md)) you obtain the total number of bins.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Number of histogram bins for color band 1. |

---

### `M_NB_BINS_BAND_2`

Sets the number of histogram bins for color band 2. Relevant only when matching colors using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).  The number of bins situated along the axis of band 2 determine the size of the bins for that band. If you multiply the number of bins situated along the axis of each band ([`M_NB_BINS_BAND_0`](../../Reference/col/McolControl.md), [`M_NB_BINS_BAND_1`](../../Reference/col/McolControl.md), and [`M_NB_BINS_BAND_2`](../../Reference/col/McolControl.md)) you obtain the total number of bins.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Number of histogram bins for color band 2. |

---

### `M_OUTLIER_DRAW_COLOR`

Sets the color to use to draw outlier pixels when drawing or generating the color images ([`M_DRAW_..._USING_COLOR`](../../Reference/col/McolDraw.md)) or the distance image ([`M_DRAW_DISTANCE`](../../Reference/col/McolMatch.md)), using [`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md).  Outlier pixels are pixels inside a target area that do not match with any color-sample.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COLOR888` | Specifies a 24-bit color value (eight bits per band) with which to set the outlier pixels. The color value is taken as is (no conversion), according to the source color space. Typically, this should be RGB, since the outlier color is meant for display.  When [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md) is set with **M_COLOR888()**, only the first component (for example, red) will be used to draw or generate [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md), which is a grayscale image. |
| `M_TRANSPARENT` | Specifies not to change the value of the outlier pixels. Therefore, the outlier pixels will be those of the specified image. |
| `Value > 0` *(default)* | Specifies a grayscale value for the outlier pixels, as a _double_ or _integer_.  When drawing the distance image ([`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)) in a 32-bit _integer_ buffer, you should keep the [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md) value under 24-bit (that is, less than 16,777,215). |

---

### `M_OUTLIER_LABEL`

Sets the label to use for outlier pixels when drawing or generating the label images ([`M_DRAW_..._USING_LABEL`](../../Reference/col/McolDraw.md)) using [`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md).  Outlier pixels are pixels inside a target area that do not match with any color-sample.  Note that the [`M_OUTLIER_LABEL`](../../Reference/col/McolControl.md) setting does not represent a valid color-sample and you cannot pass it as a color-sample label to [`McolInquire`](../../Reference/col/McolInquire.md), [`McolGetResult`](../../Reference/col/McolGetResult.md), or [`McolDraw`](../../Reference/col/McolDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the label of the outlier pixels, as an _integer_. The label must be unique among existing labels, and must be less than _2<sup>24</sup> _. |

---

### `M_SAVE_AREA_IMAGE`

Sets whether to save the area identifier image in the result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to save the image. |
| `M_ENABLE` | Specifies to save the image. |

### For controlling color matching context settings that are used for color space encoding

The following [`ControlType`](../../Reference/col/McolControl.md) and corresponding [`ControlValue`](../../Reference/col/McolControl.md) parameter settings are used to control color space encoding, according to the actual dynamic range of your color data. In this case, you must set the [`IndexOrLabel`](../../Reference/col/McolControl.md) parameter to [`M_CONTEXT`](../../Reference/col/McolControl.md). These values are only available for color matching contexts.  Color space encoding defines how color is transformed from the range represented in an image buffer to its native (theoretical) range. For example, RGB color space data is represented in an image buffer as values between 0 and 255 (8-bit); these values are then mapped to their native data range, which consists of all real numbers between 0 and 1.  The Color Analysis module allows you to perform this transformation automatically, using [`M_ENCODING`](../../Reference/col/McolControl.md) with [`M_nBIT`](../../Reference/col/McolControl.md), which is typically sufficient. You can also use [`M_USER_DEFINED`](../../Reference/col/McolControl.md) to explicitly control the transformation according to offset ([`M_OFFSET_BAND_n`](../../Reference/col/McolControl.md)) and scale ([`M_SCALE_BAND_n`](../../Reference/col/McolControl.md)) values. For more information, see [Color space encoding](../../UserGuide/C22_Color/S08_Advanced_color_matching_settings.md).

---

### `M_ENCODING`

Sets the color space encoding, which defines how color is transformed from the range represented in an image buffer to its native (theoretical) range.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_8BIT` *(default)* | Specifies an internally defined color space encoding for 8-bit images. |
| `M_10BIT` | Specifies an internally defined color space encoding for 10-bit images. |
| `M_12BIT` | Specifies an internally defined color space encoding for 12-bit images. |
| `M_14BIT` | Specifies an internally defined color space encoding for 14-bit images. |
| `M_16BIT` | Specifies an internally defined color space encoding for 16-bit images. |
| `M_USER_DEFINED` | Specifies that the color space encoding is defined using [`M_OFFSET_BAND_n`](../../Reference/col/McolControl.md) and [`M_SCALE_BAND_n`](../../Reference/col/McolControl.md). |

---

### `M_OFFSET_BAND_0`

Sets the offset for the color space encoding, for band 0.

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset, as a _double_. |

---

### `M_OFFSET_BAND_1`

Sets the offset for the color space encoding, for band 1.

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset, as a _double_. |

---

### `M_OFFSET_BAND_2`

Sets the offset for the color space encoding, for band 2.

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset, as a _double_. |

---

### `M_SCALE_BAND_0`

Sets the scale for the color space encoding, for band 0.

| Value | Description |
| --- | --- |
| `Value` | Specifies the scale, as a _double_. |

---

### `M_SCALE_BAND_1`

Sets the scale for the color space encoding, for band 1.

| Value | Description |
| --- | --- |
| `Value` | Specifies the scale, as a _double_. |

---

### `M_SCALE_BAND_2`

Sets the scale for the color space encoding, for band 2.

| Value | Description |
| --- | --- |
| `Value` | Specifies the scale, as a _double_. |

### For controlling color-samples

The following [`ControlType`](../../Reference/col/McolControl.md) and corresponding [`ControlValue`](../../Reference/col/McolControl.md) parameter settings are used to control color-samples defined in the context. In this case, you must set the [`IndexOrLabel`](../../Reference/col/McolControl.md) parameter to **M_SAMPLE_INDEX()** or **M_SAMPLE_LABEL()**. Unless otherwise specified, these values are only available for color matching contexts.

---

### `M_DISTANCE_TOLERANCE`

Sets the acceptable tolerance for the color distance between the color-sample and a target area. The tolerance ranges from no color difference to [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md). The greater the tolerance, the greater the distance (difference) between the colors can be, for them to match.  Aurora Imaging Library ignores [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md) when using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies the tolerance automatically.  If you set [`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md) to [`M_ABSOLUTE`](../../Reference/col/McolControl.md), [`M_AUTO`](../../Reference/col/McolControl.md) specifies an infinite ([`M_INFINITE`](../../Reference/col/McolControl.md)) tolerance.  If you set [`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md) to [`M_RELATIVE`](../../Reference/col/McolControl.md), [`M_AUTO`](../../Reference/col/McolControl.md) specifies a tolerance of 1.  If you set [`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md) to [`M_SAMPLE_STDDEV`](../../Reference/col/McolControl.md), [`M_AUTO`](../../Reference/col/McolControl.md) specifies a tolerance of 3. |
| `M_INFINITE` | Specifies an infinite tolerance. |
| `Value >= 0.0` | Specifies the tolerance value.  Set the tolerance according to the specified tolerance mode ([`M_DISTANCE_TOLERANCE_MODE`](../../Reference/col/McolControl.md)) and distance type ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)). For example, a tolerance value of 1 when using an [`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md) distance type is not the same as when using an [`M_MANHATTAN`](../../Reference/col/McolSetMethod.md) distance type, even if the tolerance mode is [`M_ABSOLUTE`](../../Reference/col/McolControl.md). |

---

### `M_HISTOGRAM_FREQUENCY_THRESHOLD`

Sets the minimal frequency, in pixels, for a histogram bin to count in the matching process. This setting is only valid when using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).  Increasing the minimal frequency can lead to a faster match while discarding noise in the sample histogram. Bins with a frequency less than the threshold are considered empty. When using triplet color-samples with [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md), it is not recommended to set the frequency threshold to a value other than 1, which is the default.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the minimal frequency, as an _integer_. |

---

### `M_SAMPLE_LABEL_VALUE`

Sets the color-sample's label. This value is available for color matching and relative color calibration contexts.  Initially, you specify a color-sample's label value when adding a color-sample to the context, using [`McolDefine`](../../Reference/col/McolDefine.md). You can use [`M_SAMPLE_LABEL_VALUE`](../../Reference/col/McolControl.md) to change the label afterward.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label, as an _integer_. The label must be unique among existing labels, and must be less than _2<sup>24</sup> _. |

This only applies if [`M_ENCODING`](../../Reference/col/McolControl.md) is set to [`M_USER_DEFINED`](../../Reference/col/McolControl.md).
