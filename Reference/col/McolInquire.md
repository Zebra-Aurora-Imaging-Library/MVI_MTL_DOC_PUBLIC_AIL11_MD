---
doctype: Reference
module: col
function: McolInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolInquire"
---

# McolInquire

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

> Inquire information about a color matching or relative color calibration context, color-sample, color element, or a color matching result.

## Syntax

```c
AIL_INT McolInquire(
    AIL_ID    ContextOrResultId,  //in
    AIL_INT   IndexOrLabel,       //in
    AIL_INT64 InquireType,        //in
    void *    UserVarPtr          //out
)
```

## Description

This function inquires information about a specified color matching or relative color calibration context, a color-sample contained within the context, a color element contained within a specified color-sample, or a color matching result buffer.

If the inquired setting is set to [`M_DEFAULT`](../../Reference/col/McolControl.md) (for example, in [`McolControl`](../../Reference/col/McolControl.md)), [`McolInquire`](../../Reference/col/McolInquire.md) will return [`M_DEFAULT`](../../Reference/col/McolInquire.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/col/McolInquire.md) to the [`InquireType`](../../Reference/col/McolInquire.md) parameter.

## Parameters

### `ContextOrResultId` *(in, AIL_ID)*

Specifies the identifier of the context or result buffer about which to inquire information. Both the context and the result buffer must have been previously allocated on the required system using [`McolAlloc`](../../Reference/col/McolAlloc.md) (with [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md) or [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md)) or [`McolAllocResult`](../../Reference/col/McolAllocResult.md).

### `IndexOrLabel` *(in, AIL_INT)*

Specifies what to inquire.

*For inquiring about a context or result*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default.

If [`ContextOrResultId`](../../Reference/col/McolInquire.md) is set to a context, the default is the same as [`M_CONTEXT`](../../Reference/col/McolInquire.md).

If [`ContextOrResultId`](../../Reference/col/McolInquire.md) is set to a color matching result, the default is the same as [`M_GENERAL`](../../Reference/col/McolInquire.md). |
| `M_CONTEXT` | Inquires information about a global setting of either a color matching or relative color calibration context, depending on the [`ContextOrResultId`](../../Reference/col/McolInquire.md) parameter setting. |
| `M_GENERAL` | Inquires information about a setting of a color matching result buffer. In this case, [`ContextOrResultId`](../../Reference/col/McolInquire.md) must specify a color matching result buffer. |

*For inquiring about a color-sample or color element*

| Value | Description |
| --- | --- |
| `M_REFERENCE_SAMPLE_ITEM` | Specifies the index of a color element (item), relative to the reference color-sample. This value only applies to relative color calibration contexts ([`ContextOrResultId`](../../Reference/col/McolInquire.md)). |
| `M_SAMPLE_INDEX` | Specifies the index of a color-sample. |
| `M_SAMPLE_INDEX_ITEM` | Specifies the index of a color element (item), relative to a color-sample index. |
| `M_SAMPLE_LABEL` | Specifies the label of a color-sample. |
| `M_SAMPLE_LABEL_ITEM` | Specifies the index of a color element (item), relative to a color-sample label. |
| `M_REFERENCE_SAMPLE` | Specifies the reference color-sample. This value only applies to relative color calibration contexts ([`ContextOrResultId`](../../Reference/col/McolInquire.md)). |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`McolInquire`](../../Reference/col/McolInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring general settings about a context

To inquire about general settings of a context, set the [`InquireType`](../../Reference/col/McolInquire.md) parameter to one of the following values. In this case, you must set the [`IndexOrLabel`](../../Reference/col/McolInquire.md) parameter to [`M_CONTEXT`](../../Reference/col/McolInquire.md) and the [`ContextOrResultId`](../../Reference/col/McolInquire.md) parameter to a color matching or relative color calibration context, as required by the setting. Unless otherwise specified, the following settings only apply to color matching contexts.

---

### `M_ACCEPTANCE`

Inquires the acceptance level for the color-sample's score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable score, as a percentage. |

---

### `M_ACCEPTANCE_RELEVANCE`

Inquires the acceptance level for the target area's relevance score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies an acceptable relevance score, as a percentage. |

---

### `M_BACKGROUND_DRAW_COLOR`

Inquires the background pixel color, when applicable. This value applies to [`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md), [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md), or [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COLOR888` | Specifies a 24-bit color value (eight bits per band) with which to set the background pixels. |
| `M_TRANSPARENT` *(default)* | Specifies not to change the background. |
| `Value > 0` | Specifies a grayscale value for the background pixels, as a _double_ or _integer_. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_BAND_MODE`

Inquires the color bands to use when performing [`McolMatch`](../../Reference/col/McolMatch.md).

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

### `M_CALIBRATION_COMPUTE_OPTION`

Inquires whether the relative color calibration was performed using pixel values or color statistics.

| Value | Description |
| --- | --- |
| *(see [`ConversionModeOrComputeOption`](Reference/col/McolSetMethod.md))* |  |

---

### `M_CALIBRATION_INTENT`

Inquires the transformation precision with which to perform relative color calibration.

| Value | Description |
| --- | --- |
| *(see [`DistTypeOrCalIntent`](Reference/col/McolSetMethod.md))* |  |

---

### `M_CALIBRATION_MODE`

Inquires the transformation operation with which to perform relative color calibration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_HISTOGRAM_BASED`](../../Reference/col/McolInquire.md). |
| `M_COLOR_TO_COLOR` | Specifies that a color-sample's mapping is based on transforming its color to the color of the reference color-sample on a point-to-point basis. |
| `M_GLOBAL_MEAN_VARIANCE` | Specifies that a color-sample's mapping is based on transforming its mean and variance (standard deviation) to resemble the mean and variance of the reference color-sample. |
| `M_HISTOGRAM_BASED` | Specifies that a color-sample's mapping is based on using enhanced color distribution information to transform its color data to better resemble the color data of the reference color-sample. |

---

### `M_COLOR_SPACE`

Inquires the context's color space.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_RGB`](../../Reference/col/McolInquire.md). |
| `M_CIELAB` | Specifies a CIELAB (or LAB) color space. |
| `M_HSL` | Specifies an HSL color space. |
| `M_RGB` | Specifies an RGB color space. |

---

### `M_CONTEXT_TYPE`

Inquires the type of context used.

| Value | Description |
| --- | --- |
| `M_COLOR_CALIBRATION_RELATIVE` | Allocates a relative color calibration context for use with [`McolTransform`](../../Reference/col/McolTransform.md). |
| `M_COLOR_MATCHING` | Allocates a color matching context for use with [`McolMatch`](../../Reference/col/McolMatch.md). |

---

### `M_CONVERSION_GAMMA`

Inquires whether gamma correction is performed when converting colors before the match operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to remove gamma correction. |
| `M_ENABLE` *(default)* | Specifies to remove gamma correction. |

---

### `M_CONVERSION_MODE`

Inquires the mode used to internally convert colors before the matching operation.

| Value | Description |
| --- | --- |
| *(see [`ConversionModeOrComputeOption`](Reference/col/McolSetMethod.md))* |  |

---

### `M_DISTANCE_IMAGE_NORMALIZE`

Inquires the normalization factor when drawing or generating [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md), using [`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX_NORMALIZE` | Specifies to normalize distance values according to the greatest calculated distance. |
| `M_NO_NORMALIZE` *(default)* | Specifies no normalization. |
| `Value > 0.0` | Specifies to normalize distance values according to the specified normalization value. |

---

### `M_DISTANCE_TOLERANCE_MODE`

Inquires the strategy used to calculate the tolerance for an acceptable color distance between the color-sample and a target area.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` *(default)* | Specifies that the [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolInquire.md) value will be untouched and used as the acceptable distance tolerance. |
| `M_RELATIVE` |  |
| `M_SAMPLE_STDDEV` |  |

---

### `M_DISTANCE_TYPE`

Inquires the type of distance calculated when matching colors.

| Value | Description |
| --- | --- |
| *(see [`DistTypeOrCalIntent`](Reference/col/McolSetMethod.md))* |  |

---

### `M_GENERATE_DISTANCE_IMAGE`

Inquires whether the distance image was generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to generate the image. |
| `M_ENABLE` | Specifies to generate the image. |

---

### `M_GENERATE_PIXEL_MATCH`

Inquires whether the image containing the color-sample label for which each pixel voted was generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to generate the image. |
| `M_ENABLE` | Specifies to generate the image. |

---

### `M_GENERATE_SAMPLE_COLOR_LUT`

Inquires whether the color-sample label LUT was generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to generate the LUT. |
| `M_ENABLE` | Specifies to generate the LUT. |

---

### `M_MATCH_MODE`

Inquires the operation mode used when matching colors.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_STAT_MIN_DIST`](../../Reference/col/McolInquire.md). |
| `M_HISTOGRAM_MATCHING` | Specifies a minimum distance operation, based on color histograms. |
| `M_HISTOGRAM_VOTE` | Specifies a match operation based on histogram voting. |
| `M_MIN_DIST_VOTE` | Specifies a minimum distance operation, based on pixel voting. |
| `M_STAT_MIN_DIST` | Specifies a minimum distance operation, based on pixel statistics. |

---

### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time settings for the context are modified.  Although you cannot identify the modification counter's contents, you can compare them throughout your application to inquire if the context has been altered. If the modification counter has changed you can, for example, prompt the user to save before closing the application.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

---

### `M_NB_BINS_BAND_0`

Inquires the number of histogram bins for color band 0. Relevant only when matching colors using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of histogram bins for color band 0. |

---

### `M_NB_BINS_BAND_1`

Inquires the number of histogram bins for color band 1. Relevant only when matching colors using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Number of histogram bins for color band 1. |

---

### `M_NB_BINS_BAND_2`

Inquires the number of histogram bins for color band 2. Relevant only when matching colors using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Number of histogram bins for color band 2. |

---

### `M_NUMBER_OF_SAMPLES`

Inquires the number of color-samples defined in the context ([`McolDefine`](../../Reference/col/McolDefine.md)).  For relative color calibration contexts, the number of color-samples does not include the reference color-sample.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of color-samples. |

---

### `M_OUTLIER_DRAW_COLOR`

Inquires the color used for outlier pixels when drawing the color of the best-matched color-sample, the color of the color-sample for which each pixel voted, or the distance between the color of the target area/target pixel and the color of its best-matched color-sample ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md), [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) or [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COLOR888` | Specifies a 24-bit color value (eight bits per band) with which to set the outlier pixels. |
| `M_TRANSPARENT` | Specifies not to change the value of the outlier pixels. |
| `Value > 0` *(default)* | Specifies a grayscale value for the outlier pixels, as a _double_ or _integer_. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_OUTLIER_LABEL`

Inquires the label used for outlier pixels when drawing the label of the best-matched color-sample or the label of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the label of the outlier pixels, as an _integer_. |

---

### `M_PREPROCESSED`

Inquires whether the context is preprocessed. The context must be preprocessed (using [`McolPreprocess`](../../Reference/col/McolPreprocess.md)) before calling [`McolMatch`](../../Reference/col/McolMatch.md) or [`McolTransform`](../../Reference/col/McolTransform.md). After certain settings of the context are changed (for example, with [`McolControl`](../../Reference/col/McolControl.md) or [`McolDefine`](../../Reference/col/McolDefine.md)), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

---

### `M_SAMPLE_LABEL_SIZE_BIT`

Inquires the depth per band required for the image buffer in which to draw the label of the best-matched color-sample or the label of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `1` | Specifies that the data depth is 1 bit per band. |
| `8` | Specifies that the data depth is 8 bits per band. |
| `16` | Specifies that the data depth is 16 bits per band. |
| `32` | Specifies that the data depth is 32 bits per band. |

---

### `M_SAMPLE_LUT_SIGN`

Inquires the data type required for the image buffer in which to draw the 3-band color-sample label LUT ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md)). This LUT associates the label value of each color-sample with its average color.

| Value | Description |
| --- | --- |
| `M_UNSIGNED` | Specifies that the data type is unsigned. |

---

### `M_SAMPLE_LUT_SIZE_BAND`

Inquires the number of color bands that are required for the image buffer in which to draw the 3-band color-sample label LUT ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md)). This LUT associates the label value of each color-sample with its average color.

| Value | Description |
| --- | --- |
| `3` | Specifies that 3 color bands are required. |

---

### `M_SAMPLE_LUT_SIZE_BIT`

Inquires the depth per band required for the image buffer in which to draw the 3-band color-sample label LUT ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md)). This LUT associates the label value of each color-sample with its average color.

| Value | Description |
| --- | --- |
| `8` | Specifies that the data depth is 8 bits per band. |

---

### `M_SAMPLE_LUT_SIZE_X`

Inquires the width required for the image buffer in which to draw the 3-band color-sample label LUT ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md)). This LUT associates the label value of each color-sample with its average color.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the image buffer, in pixels. |

---

### `M_SAMPLE_LUT_SIZE_Y`

Inquires the height required for the image buffer in which to draw the 3-band color-sample label LUT ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md)). This LUT associates the label value of each color-sample with its average color.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the image buffer, in pixels. |

---

### `M_SAMPLE_LUT_TYPE`

Inquires the data type and depth required for the image buffer in which to draw the 3-band color-sample label LUT ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md)). This LUT associates the label value of each color-sample with its average color.

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 8` | Specifies that the data is 8-bit unsigned. |

---

### `M_SAVE_AREA_IMAGE`

Inquires whether the area identifier image was saved.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to save the image. |
| `M_ENABLE` | Specifies to save the image. |

### For inquiring color matching context settings that are used for color space encoding

To inquire about color matching context settings that are used for color space encoding, set the [`InquireType`](../../Reference/col/McolInquire.md) parameter to one of the following values. In this case, you must set the [`IndexOrLabel`](../../Reference/col/McolInquire.md) parameter to [`M_CONTEXT`](../../Reference/col/McolInquire.md).

---

### `M_ENCODING`

Inquires the color space encoding.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_8BIT` *(default)* | Specifies an internally defined color space encoding for 8-bit images. |
| `M_10BIT` | Specifies an internally defined color space encoding for 10-bit images. |
| `M_12BIT` | Specifies an internally defined color space encoding for 12-bit images. |
| `M_14BIT` | Specifies an internally defined color space encoding for 14-bit images. |
| `M_16BIT` | Specifies an internally defined color space encoding for 16-bit images. |
| `M_USER_DEFINED` | Specifies that the color space encoding is defined using [`M_OFFSET_BAND_n`](../../Reference/col/McolInquire.md) and [`M_SCALE_BAND_n`](../../Reference/col/McolInquire.md). |

---

### `M_OFFSET_BAND_0`

Inquires the offset for the color space encoding, for band 0.

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset, as a _double_. |

---

### `M_OFFSET_BAND_1`

Inquires the offset for the color space encoding, for band 1.

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset, as a _double_. |

---

### `M_OFFSET_BAND_2`

Inquires the offset for the color space encoding, for band 2.

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset, as a _double_. |

---

### `M_SCALE_BAND_0`

Inquires the scale for the color space encoding, for band 0.

| Value | Description |
| --- | --- |
| `Value` | Specifies the scale, as a _double_. |

---

### `M_SCALE_BAND_1`

Inquires the scale for the color space encoding, for band 1.

| Value | Description |
| --- | --- |
| `Value` | Specifies the scale, as a _double_. |

---

### `M_SCALE_BAND_2`

Inquires the scale for the color space encoding, for band 2.

| Value | Description |
| --- | --- |
| `Value` | Specifies the scale, as a _double_. |

### For inquiring about a general setting of a color-sample defined in a context

To inquire about a general setting of a color-sample defined in a context, set the [`InquireType`](../../Reference/col/McolInquire.md) parameter to one of the following values. In this case, you must set the [`IndexOrLabel`](../../Reference/col/McolInquire.md) parameter to the index or label of a color-sample (not an element of a color-sample). Unless otherwise specified, the following settings apply to color matching and relative color calibration contexts.

---

### `M_DISTANCE_TOLERANCE`

Inquires the acceptable tolerance for the color distance between the color-sample and a target area.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies the tolerance automatically. |
| `M_INFINITE` | Specifies an infinite tolerance. |
| `Value >= 0.0` | Specifies the tolerance value. |

---

### `M_HISTOGRAM_FREQUENCY_THRESHOLD`

Inquires the minimal frequency, in pixels, for a histogram bin to count in the matching process.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the minimal frequency, as an _integer_. |

---

### `M_MATCH_SAMPLE_COLOR_BAND_0`

Inquires the band 0 value of the color-sample that will be used with [`McolMatch`](../../Reference/col/McolMatch.md).  If you specified a color conversion mode using [`McolSetMethod`](../../Reference/col/McolSetMethod.md), [`M_MATCH_SAMPLE_COLOR_BAND_0`](../../Reference/col/McolInquire.md) returns the converted value. To return the unconverted value, use [`M_SAMPLE_AVERAGE_COLOR_BAND_0`](../../Reference/col/McolInquire.md). For example, if you are using RGB images in an RBG color matching context, and you specify an HSL conversion mode ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)), [`M_MATCH_SAMPLE_COLOR_BAND_0`](../../Reference/col/McolInquire.md) returns the average value for band 0 (hue) of the HSL color (as used by the match operation), while [`M_SAMPLE_AVERAGE_COLOR_BAND_0`](../../Reference/col/McolInquire.md) returns the value for band 0 (red) of the RGB color (as defined in [`McolDefine`](../../Reference/col/McolDefine.md)). If you have not performed a color conversion, [`M_MATCH_SAMPLE_COLOR_BAND_0`](../../Reference/col/McolInquire.md) returns the same value as [`M_SAMPLE_AVERAGE_COLOR_BAND_0`](../../Reference/col/McolInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the band 0 value of the color-sample. |

---

### `M_MATCH_SAMPLE_COLOR_BAND_1`

Inquires the band 1 value of the color-sample that will be used with [`McolMatch`](../../Reference/col/McolMatch.md). For more information, see [`M_MATCH_SAMPLE_COLOR_BAND_0`](../../Reference/col/McolInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the band 1 value of the color-sample. |

---

### `M_MATCH_SAMPLE_COLOR_BAND_2`

Inquires the band 2 value of the color-sample that will be used with [`McolMatch`](../../Reference/col/McolMatch.md). For more information, see [`M_MATCH_SAMPLE_COLOR_BAND_0`](../../Reference/col/McolInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the band 2 value of the color-sample. |

---

### `M_NUMBER_OF_COLOR_ITEMS`

Inquires the number of color elements items in the specified color-sample.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of color elements in the color-sample. |

---

### `M_SAMPLE_8BIT_AVERAGE_COLOR_BAND_0`

Inquires the average color of the color-sample for band 0 (as an 8-bit value). This inquire type is useful for displaying the sample's color. This value is not used in the operation (for example, [`McolMatch`](../../Reference/col/McolMatch.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the average color of the color-sample for band 0. |

---

### `M_SAMPLE_8BIT_AVERAGE_COLOR_BAND_1`

Inquires the average color of the color-sample for band 1 (as an 8-bit value). This inquire type is useful for displaying the sample's color. This value is not used in the operation (for example, [`McolMatch`](../../Reference/col/McolMatch.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the average color of the color-sample for band 1. |

---

### `M_SAMPLE_8BIT_AVERAGE_COLOR_BAND_2`

Inquires the average color of the color-sample for band 2 (as an 8-bit value). This inquire type is useful for displaying the sample's color. This value is not used in the operation (for example, [`McolMatch`](../../Reference/col/McolMatch.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the average color of the color-sample for band 2. |

---

### `M_SAMPLE_ABSOLUTE_TOLERANCE`

Inquires the maximum distance a color-sample can have for it to be matched. To retrieve this information, you must first preprocess the context, using [`McolPreprocess`](../../Reference/col/McolPreprocess.md).

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies that the maximum distance is infinite. This value is returned when only one color-sample has been defined. |
| `Value` | Specifies the maximum distance. |

---

### `M_SAMPLE_AVERAGE_COLOR_BAND_0`

Inquires the band 0 value of the color-sample, as it was defined using [`McolDefine`](../../Reference/col/McolDefine.md). For color-samples defined from an image ([`M_IMAGE`](../../Reference/col/McolDefine.md)), the band 0 value returned refers to the estimated version of the color (for example, the average).  If you specified a color conversion mode before the match operation using [`McolSetMethod`](../../Reference/col/McolSetMethod.md), [`M_SAMPLE_AVERAGE_COLOR_BAND_0`](../../Reference/col/McolInquire.md) returns the unconverted value. To return the converted value, use [`M_MATCH_SAMPLE_COLOR_BAND_0`](../../Reference/col/McolInquire.md). For example, if you are using RGB images in an RBG color matching context, and you specify an HSL conversion mode ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)), [`M_SAMPLE_AVERAGE_COLOR_BAND_0`](../../Reference/col/McolInquire.md) returns the value for band 0 (red) of the RGB color (as defined in [`McolDefine`](../../Reference/col/McolDefine.md)), while [`M_MATCH_SAMPLE_COLOR_BAND_0`](../../Reference/col/McolInquire.md) returns the average value for band 0 (hue) of the HSL color (as used by the match operation). If you have not performed a color conversion, [`M_SAMPLE_AVERAGE_COLOR_BAND_0`](../../Reference/col/McolInquire.md) returns the same value as [`M_MATCH_SAMPLE_COLOR_BAND_0`](../../Reference/col/McolInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the band 0 value of the color-sample. |

---

### `M_SAMPLE_AVERAGE_COLOR_BAND_1`

Inquires the band 1 value of the color-sample, as it was defined using [`McolDefine`](../../Reference/col/McolDefine.md). For more information, see [`M_SAMPLE_AVERAGE_COLOR_BAND_0`](../../Reference/col/McolInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the band 1 value of the color-sample. |

---

### `M_SAMPLE_AVERAGE_COLOR_BAND_2`

Inquires the band 2 value of the color-sample, as it was defined using [`McolDefine`](../../Reference/col/McolDefine.md). For more information, see [`M_SAMPLE_AVERAGE_COLOR_BAND_0`](../../Reference/col/McolInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the band 2 value of the color-sample. |

---

### `M_SAMPLE_LABEL_VALUE`

Inquires the current label of the color-sample.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current label of the color-sample. |

---

### `M_SAMPLE_MOSAIC_DONT_CARE_SIGN`

Inquires the data type of the color-sample's mask mosaic buffer.

| Value | Description |
| --- | --- |
| `M_UNSIGNED` | Specifies that the color-sample's mask mosaic buffer is of data type unsigned. |

---

### `M_SAMPLE_MOSAIC_DONT_CARE_SIZE_BAND`

Inquires the number of bands of the color-sample's mask mosaic buffer.

| Value | Description |
| --- | --- |
| `1` | Specifies that the color-sample's mask mosaic buffer has 1 band. |

---

### `M_SAMPLE_MOSAIC_DONT_CARE_SIZE_BIT`

Inquires the depth per band of the color-sample's mask mosaic buffer.

| Value | Description |
| --- | --- |
| `1` | Specifies that the data depth is 1 bit per band. |
| `8` | Specifies that the data depth is 8 bits per band. |

---

### `M_SAMPLE_MOSAIC_DONT_CARE_SIZE_X`

Inquires the X-size (width) of the color-sample's mask mosaic buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-size (width) of the color-sample's mask mosaic buffer, in pixels. |

---

### `M_SAMPLE_MOSAIC_DONT_CARE_SIZE_Y`

Inquires the Y-size (height) of the color-sample's mask mosaic buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-size (height) of the color-sample's mask mosaic buffer, in pixels. |

---

### `M_SAMPLE_MOSAIC_DONT_CARE_TYPE`

Inquires the data type and depth of the color-sample's mask buffer.

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 1` | Specifies that the data is 1-bit unsigned. |
| `M_UNSIGNED + 8` | Specifies that the data is 8-bit unsigned. |

---

### `M_SAMPLE_MOSAIC_SIGN`

Inquires the data type of the color-sample's mosaic buffer.

| Value | Description |
| --- | --- |
| `M_UNSIGNED` | Specifies that the color-sample's mosaic buffer is of data type unsigned. |

---

### `M_SAMPLE_MOSAIC_SIZE_BAND`

Inquires the number of bands of the color-sample's mosaic buffer.

| Value | Description |
| --- | --- |
| `3` | Specifies that the color-sample's mosaic buffer has 3 bands. |

---

### `M_SAMPLE_MOSAIC_SIZE_BIT`

Inquires the depth per band of the color-sample's mosaic buffer.

| Value | Description |
| --- | --- |
| `8` | Specifies that the data depth is 8 bits per band. |
| `16` | Specifies that the data depth is 16 bits per band. |

---

### `M_SAMPLE_MOSAIC_SIZE_X`

Inquires the X-size (width) of the color-sample's mosaic buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-size (width) of the color-sample's mosaic buffer, in pixels. |

---

### `M_SAMPLE_MOSAIC_SIZE_Y`

Inquires the Y-size (height) of the color-sample's mosaic buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-size (height) of the color-sample's mosaic buffer, in pixels. |

---

### `M_SAMPLE_MOSAIC_TYPE`

Inquires the type of the color-sample's mosaic buffer.

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 8` | Specifies that the data is 8-bit unsigned. |
| `M_UNSIGNED + 16` | Specifies that the data is 16-bit unsigned. |

---

### `M_SAMPLE_TYPE`

Inquires the type of the color-sample.

| Value | Description |
| --- | --- |
| `M_IMAGE` | Specifies that the color-sample was added from an image. |
| `M_TRIPLET` | Specifies that the color-sample was added from three explicit color band values (RGB, HSL, or CIELAB). |

### For inquiring about a general setting of a color element (item) defined in a context

To inquire about a general setting of a color element defined in a color-sample, set the [`InquireType`](../../Reference/col/McolInquire.md) parameter to one of the following values. In this case, you must set the [`IndexOrLabel`](../../Reference/col/McolInquire.md) parameter to the index or label of a color-sample element.

---

### `M_SAMPLE_COLOR_BAND_0`

Inquires the color value for band 0 of a triplet color element.

| Value | Description |
| --- | --- |
| `Value` | Specifies the color value for band 0 of a triplet color element. |

---

### `M_SAMPLE_COLOR_BAND_1`

Inquires the color value for band 1 of a triplet color element.

| Value | Description |
| --- | --- |
| `Value` | Specifies the color value for band 1 of a triplet color element. |

---

### `M_SAMPLE_COLOR_BAND_2`

Inquires the color value for band 2 of a triplet color element.

| Value | Description |
| --- | --- |
| `Value` | Specifies the color value for band 2 of a triplet color element. |

---

### `M_SAMPLE_IMAGE_SIGN`

Inquires the data type of the image from which the color element was defined.

| Value | Description |
| --- | --- |
| `M_UNSIGNED` | Specifies that the data type is unsigned. |

---

### `M_SAMPLE_IMAGE_SIZE_BAND`

Inquires the number of color bands of the image buffer from which the color element was defined.

| Value | Description |
| --- | --- |
| `3` | Specifies that the image buffer has 3 color bands. |

---

### `M_SAMPLE_IMAGE_SIZE_BIT`

Inquires the depth per band of the image buffer from which the color element was defined.

| Value | Description |
| --- | --- |
| `8` | Specifies that the data depth is 8 bits per band. |
| `16` | Specifies that the data depth is 16 bits per band. |

---

### `M_SAMPLE_IMAGE_SIZE_X`

Inquires the width of the image from which the color element was defined.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the image, in pixels. |

---

### `M_SAMPLE_IMAGE_SIZE_Y`

Inquires the height of the image from which the color element was defined.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the image, in pixels. |

---

### `M_SAMPLE_IMAGE_TYPE`

Inquires the data type and depth of the image from which the color element was defined.

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 8` | Specifies that the data is 8-bit unsigned. |
| `M_UNSIGNED + 16` | Specifies that the data is 16-bit unsigned. |

---

### `M_SAMPLE_MASK_SIGN`

Inquires the data type of the color element's mask buffer.

| Value | Description |
| --- | --- |
| `M_UNSIGNED` | Specifies that the data type is unsigned. |

---

### `M_SAMPLE_MASK_SIZE_BAND`

Inquires the number of color bands of the color element's mask buffer.

| Value | Description |
| --- | --- |
| `1` | Specifies that there is 1 band in the color element's mask buffer. |

---

### `M_SAMPLE_MASK_SIZE_BIT`

Inquires the depth per band of the color element's mask buffer.

| Value | Description |
| --- | --- |
| `1` | Specifies that the data depth is 1 bit per band. |
| `8` | Specifies that the data depth is 8 bits per band. |

---

### `M_SAMPLE_MASK_SIZE_X`

Inquires the width of the color element's mask buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the color element's mask buffer, in pixels. |

---

### `M_SAMPLE_MASK_SIZE_Y`

Inquires the height of the color element's mask buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the color element's mask buffer, in pixels. |

---

### `M_SAMPLE_MASK_TYPE`

Inquires the data type and depth of the color element's mask buffer.

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 1` | Specifies that the data is 1-bit unsigned. |
| `M_UNSIGNED + 8` | Specifies that the data is 8-bit unsigned. |

---

### `M_SAMPLE_MASKED`

Inquires whether a mask for the color element was defined.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that a mask was not defined. |
| `M_TRUE` | Specifies that a mask was defined. |

### For inquiring information about the PCA of a color-sample in a color matching context

To inquire information about the principal component analysis (PCA) of a color-sample in a color matching context, set the [`InquireType`](../../Reference/col/McolInquire.md) parameter to one of the following values. In this case, you must set the [`IndexOrLabel`](../../Reference/col/McolInquire.md) parameter to **M_SAMPLE_INDEX()** or **M_SAMPLE_LABEL()**; you must have also preprocessed the context, using [`McolPreprocess`](../../Reference/col/McolPreprocess.md).  A PCA is a mathematical analysis which, when applied to a color image's data, results in vectors pointing towards the direction of maximum color variance. Each vector is considered a principal component, and there are as many principal components as there are color bands. Since color-samples are based on three bands, the PCA results in three vectors about which to inquire. These PCA values are not applied in the actual operation (for example, [`McolMatch`](../../Reference/col/McolMatch.md) and should only be inquired by advanced users who might, for example, want to perform some statistical analysis. Typically, color operations such as [`McolMatch`](../../Reference/col/McolMatch.md) and [`McolProject`](../../Reference/col/McolProject.md) use what is referred to as the first principal component, which is the strongest principal component computed from a PCA. If no principal component is the strongest, one is arbitrarily considered the first principal component.  Aurora Imaging Library calculates the PCA values according to the context's source color space; these values are not available when using [`M_HSL`](../../Reference/col/McolAlloc.md).

---

### `M_SAMPLE_PCA_VAR1`

Inquires the variance in the direction of the color-sample's first principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the variance. |

---

### `M_SAMPLE_PCA_VAR2`

Inquires the variance in the direction of the color-sample's second principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the variance. |

---

### `M_SAMPLE_PCA_VAR3`

Inquires the variance in the direction of the color-sample's third principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the variance. |

---

### `M_SAMPLE_PCA_VECTOR1_1`

Inquires the first element of the color-sample's first principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vector element. |

---

### `M_SAMPLE_PCA_VECTOR1_2`

Inquires the second element of the color-sample's first principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vector element. |

---

### `M_SAMPLE_PCA_VECTOR1_3`

Inquires the third element of the color-sample's first principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vector element. |

---

### `M_SAMPLE_PCA_VECTOR2_1`

Inquires the first element of the color-sample's second principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vector element. |

---

### `M_SAMPLE_PCA_VECTOR2_2`

Inquires the second element of the color-sample's second principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vector element. |

---

### `M_SAMPLE_PCA_VECTOR2_3`

Inquires the third element of the color-sample's second principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vector element. |

---

### `M_SAMPLE_PCA_VECTOR3_1`

Inquires the first element of the color-sample's third principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vector element. |

---

### `M_SAMPLE_PCA_VECTOR3_2`

Inquires the second element of the color-sample's third principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vector element. |

---

### `M_SAMPLE_PCA_VECTOR3_3`

Inquires the third element of the color-sample's third principal component vector.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vector element. |

### For inquiring context or result buffer settings

To inquire about context or result buffer settings, the [`InquireType`](../../Reference/col/McolInquire.md) parameter can be set to one of the following values. When inquiring about a context, set the [`IndexOrLabel`](../../Reference/col/McolInquire.md) parameter to [`M_CONTEXT`](../../Reference/col/McolInquire.md). When inquiring about a result buffer, set the [`IndexOrLabel`](../../Reference/col/McolInquire.md) parameter to [`M_GENERAL`](../../Reference/col/McolInquire.md).

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which either the context or result buffer has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### Combination Constants — For inquiring the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

Some inquired information might be altered if it does not fit numerically in the data type that you specify. For example, if you cast a value of 300 to a _char_, information will be lost. The default data type, _double_, will never result in lost information.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

This inquire type is only available for relative color calibration contexts.

This inquire type is available for color matching and relative color calibration contexts.

This inquire is only applicable if you use [`McolControl`](../../Reference/col/McolControl.md) with [`M_ENCODING`](../../Reference/col/McolControl.md) set to [`M_USER_DEFINED`](../../Reference/col/McolControl.md).

This inquire type is only available if you have defined a mask for the color-sample, using [`McolMask`](../../Reference/col/McolMask.md).

This inquire type is only available if you have selected the band ([`McolControl`](../../Reference/col/McolControl.md) with [`M_BAND_MODE`](../../Reference/col/McolControl.md)) and have preprocessed the context ([`McolPreprocess`](../../Reference/col/McolPreprocess.md)).

This inquire type is only available if you have preprocessed the context ([`McolPreprocess`](../../Reference/col/McolPreprocess.md)).

This inquire type is not available if the operation mode is set to [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)).

This inquire type is only available for color matching contexts.

This inquire type is only available for color elements that you define using [`McolDefine`](../../Reference/col/McolDefine.md) with [`M_TRIPLET`](../../Reference/col/McolDefine.md).

This inquire type is only available for color elements that you define using [`McolDefine`](../../Reference/col/McolDefine.md) with [`M_IMAGE`](../../Reference/col/McolDefine.md).
