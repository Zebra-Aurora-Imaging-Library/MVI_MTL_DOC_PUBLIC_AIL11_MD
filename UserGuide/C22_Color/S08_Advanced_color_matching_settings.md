---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Advanced_color_matching_settings
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Advanced color matching settings"
---

# Advanced color matching settings and concepts

In addition to applying fundamental color matching specifications, you can also use advanced settings to write highly customized color matching applications. This includes color band selection, distance normalization, color space encoding, and the histogram match mode.

## Color band specification settings

By default, all bands are used when performing the match operation. You can, however, specify one or two specific bands, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_BAND_MODE`](../../Reference/col/McolControl.md).

For example, if you are using HSL images, you can match with only the hue (H) band by setting [`M_BAND_MODE`](../../Reference/col/McolControl.md) to [`M_COLOR_BAND_0`](../../Reference/col/McolControl.md). This can be useful if your image has non-uniform lighting, shadows, or highlights. A similar match can be performed if you are matching in CIELAB and match with only the chrominance bands (bands A and B), by setting [`M_BAND_MODE`](../../Reference/col/McolControl.md) to [`M_COLOR_BAND_1 + M_COLOR_BAND_2`](../../Reference/col/McolControl.md).

## Distance normalization settings

Aurora Imaging Library calculates color distances (with [`McolMatch`](../../Reference/col/McolMatch.md) or [`McolDistance`](../../Reference/col/McolDistance.md)) as numerical _floating-point_ values and internally stores them as such. These exact distance values are then written to the destination buffer, provided that this buffer is of type _floating-point_. If the buffer is of type _integer_, distance values are truncated; that is, due to the constraint of the buffer type, the fractional (decimal) portion of the distance result is not written.

However, before color distance results are written to the destination buffer, you can normalize them according to a multiplicative factor, using either the [`NormalizeValue`](../../Reference/col/McolDistance.md) parameter (for [`McolDistance`](../../Reference/col/McolDistance.md)) or [`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_IMAGE_NORMALIZE`](../../Reference/col/McolControl.md) (for [`McolMatch`](../../Reference/col/McolMatch.md)). You can specify a specific normalization value, or use [`M_MAX_NORMALIZE`](../../Reference/col/McolControl.md) to normalize distances with the greatest calculated distance. This value corresponds to the result [`M_MAX_DISTANCE`](../../Reference/col/McolGetResult.md) ([`McolGetResult`](../../Reference/col/McolGetResult.md)). To specify no normalization, use [`M_NO_NORMALIZE`](../../Reference/col/McolControl.md).

The normalization performed is as follows (when using [`M_MAX_NORMALIZE`](../../Reference/col/McolControl.md)):

- `(_Distance_/_GreatestCalculatedDistance_) x _MaximumPossibleValueOfDestBuf_` (for destination buffer's of type _integer_).
- `(_Distance_/_GreatestCalculatedDistance_)` (for destination buffer's of type _floating-point_).
  If the destination buffer is of type _floating-point_, you would typically not normalize the distance results, since all this will do is return a value between 0.0 and 1.0.

The following example shows the distance between two colors (_FirstSourceImage_ - _SecondSourceImage_) using no normalization ([`M_NO_NORMALIZE`](../../Reference/col/McolControl.md)), and using maximum normalization ([`M_MAX_NORMALIZE`](../../Reference/col/McolControl.md)).

*[Image: Color_DistanceNoramlize.png]*

Distance results are always clipped at the destination buffer's maximal possible value, which can occur when specifying [`M_NO_NORMALIZE`](../../Reference/col/McolControl.md) or a specific normalization factor. For example, when using an 8-bit unsigned destination buffer, distances higher than 255 will be written as 255. Note that if you specify a destination buffer of type _floating-point_, distance results would almost never be greater than the buffer's maximal possible value, since its limit is extremely high.

Once normalized, the resulting value (grayscale) written in the destination buffer does not represent a precise distance (between colors) and should never be interpreted as such. You can however make the general conclusion that the higher (brighter) this grayscale value is, the greater the color distance was. Normalization can therefore be seen as a way to remap distance values according to the destination buffer's dynamic range to minimize the loss of data and to obtain a buffer that you can then use for other types of grayscale processing, such as thresholding and blob analysis.

Normalization is applied when drawing the distance image ([`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)), using either [`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md). This can typically improve the display and visualization of this image. Note that since Aurora Imaging Library internally stores distance values as 32-bit _float_, you can still draw the distance image in an unsigned image buffer of type _integer_.

## Color space encoding

Color space encoding (of the source color space) determines how color is transformed from the range represented in an image buffer to its native (theoretical) range, which is device-independent. For example, RGB color space data is represented in an image buffer as values between 0 and 255 (8-bit); these values are then mapped to their native data range, which consists of all real numbers between 0 and 1.

You must set the color space encoding according to the actual dynamic range of your color data, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_ENCODING`](../../Reference/col/McolControl.md). By default, the Color Analysis module assumes that you are using an 8-bit color space encoding, regardless of the depth of your buffers. If this default is not appropriate, you should modify it with [`M_ENCODING`](../../Reference/col/McolControl.md) accordingly. For example, if your color data was acquired with a 16-bit camera, you should set [`M_ENCODING`](../../Reference/col/McolControl.md) to [`M_16BIT`](../../Reference/col/McolControl.md).

A number of predefined color space encoding settings are provided ([`M_nBIT`](../../Reference/col/McolControl.md)). These are typically sufficient for most applications. You can even use image buffers that exceed the actual dynamic range of your color data, provided that their content respects this range. For example, if you set [`M_ENCODING`](../../Reference/col/McolControl.md) to [`M_8BIT`](../../Reference/col/McolControl.md), you can use 16-bit image buffers that contain values between 0 and 255 (8-bit); however, if they contain values outside this range, results will be inconsistent.

If required, you can explicitly set, for each band, specific offset ([`M_OFFSET_BAND_n`](../../Reference/col/McolControl.md)) and scale ([`M_SCALE_BAND_n`](../../Reference/col/McolControl.md)) values that specify how the color data should be transformed from the range represented in an image buffer to its native (theoretical) range. In this case, you must set [`M_ENCODING`](../../Reference/col/McolControl.md) to [`M_USER_DEFINED`](../../Reference/col/McolControl.md).

### Performing the encoding

To actually encode the data, the following native range is used for the color spaces:

|   |   |   |   |
| --- | --- | --- | --- |
| RGB | Color space band | Native Min | Native max |
| R | 0.0 | 1.0 |
| G | 0.0 | 1.0 |
| B | 0.0 | 1.0 |
| LAB | Color space band | Native Min | Native max |
| L | 0.0 | 100.0 |
| A | -128.0 | 127.0 |
| B | -128.0 | 127.0 |
| HSL | Color space band | Native Min | Native max |
| H | 0.0 | 1.0 (normalized angle) |
| S | 0.0 | 1.0 |
| L | 0.0 | 1.0 |

If you use [`M_nBIT`](../../Reference/col/McolControl.md), the Color Analysis module takes the color space's native range, and the range represented in the image buffer, and uses it to internally apply the following offset and scale, to perform the encoding:

|   |   |   |   |   |   |
| --- | --- | --- | --- | --- | --- |
| Offset | Color Space | [`M_ENCODING`](../../Reference/col/McolControl.md) | [`M_OFFSET_BAND_0`](../../Reference/col/McolControl.md) | [`M_OFFSET_BAND_1`](../../Reference/col/McolControl.md) | [`M_OFFSET_BAND_2`](../../Reference/col/McolControl.md) |
| RGB | [`M_nBIT`](../../Reference/col/McolControl.md) | 0 | 0 | 0 |
| CIELAB | [`M_8BIT`](../../Reference/col/McolControl.md) | 0 | 128 | 128 |
| [`M_nBIT`](../../Reference/col/McolControl.md) | 0 | `128 x (2<sup> _n_ </sup>-1) / 255` | `128 x (2<sup> _n_ </sup>-1) / 255` |
| HSL | [`M_nBIT`](../../Reference/col/McolControl.md) | 0 | 0 | 0 |
| Scale |
| Color Space | [`M_ENCODING`](../../Reference/col/McolControl.md) | [`M_SCALE_BAND_0`](../../Reference/col/McolControl.md) | [`M_SCALE_BAND_1`](../../Reference/col/McolControl.md) | [`M_SCALE_BAND_2`](../../Reference/col/McolControl.md) |
| RGB | [`M_nBIT`](../../Reference/col/McolControl.md) | `1.0 / (2<sup> _n_ </sup>-1)` | `1.0 / (2<sup> _n_ </sup>-1)` | `1.0 / (2<sup> _n_ </sup>-1)` |
| CIELAB | [`M_nBIT`](../../Reference/col/McolControl.md) | `100.0 / (2<sup> _n_ </sup>-1)` | `255 / (2<sup> _n_ </sup>-1)` | `255 / (2<sup> _n_ </sup>-1)` |
| HSL | [`M_nBIT`](../../Reference/col/McolControl.md) | `1.0 / (2<sup> _n_ </sup>)` | `1.0 / (2<sup> _n_ </sup>-1)` | `1.0 / (2<sup> _n_ </sup>-1)` |
| In this table, `_n_` refers to the buffer's depth. Note that the H band in HSL is represented by a normalized angle, which is transformed according to the Aurora Imaging Library angle convention. For example, in an 8-bit image buffer, 360° is mapped to 256. Since 0° and 360° are the same angle, both use the same numerical value, which is 0. |

If you use [`M_USER_DEFINED`](../../Reference/col/McolControl.md), the following encoding is applied using the specified offset ([`M_OFFSET_BAND_n`](../../Reference/col/McolControl.md)) and scale ([`M_SCALE_BAND_n`](../../Reference/col/McolControl.md)), for each band:

1. The offset ([`M_OFFSET_BAND_n`](../../Reference/col/McolControl.md)) is subtracted from the color value represented by the image buffer.
2. The resulting color value, as modified by [`M_OFFSET_BAND_n`](../../Reference/col/McolControl.md), is then multiplied by [`M_SCALE_BAND_n`](../../Reference/col/McolControl.md).

To determine the scale and offset values that you should use with [`M_USER_DEFINED`](../../Reference/col/McolControl.md), you can perform the following calculations:

*[Image: Color_ScaleAndOffsetEqns.png]*

Where:

- `[_n1_, _n2_]` is the native range of the color space.
- `[_e1_, _e2_]` is the numerical range of the image buffer.

## Histogram matching concepts

[`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) mode uses histogram bins as the basis of the matching operation. A frequency is associated with each bin, corresponding to the number of occurrences of the bin's color triplets within the image or color-sample.

You must specify the number of bins for each color band using [`McolControl`](../../Reference/col/McolControl.md) with [`M_NB_BINS_BAND_n`](../../Reference/col/McolControl.md). The total number of histogram bins contained within the color space is the product of the number of bins located along each color band. For example, if [`M_NB_BINS_BAND_0`](../../Reference/col/McolControl.md) is set to 3, [`M_NB_BINS_BAND_1`](../../Reference/col/McolControl.md) is set to 4, and [`M_NB_BINS_BAND_2`](../../Reference/col/McolControl.md) is set to 5, the total number of histogram bins is `3 * 4 * 5 = 60` bins.

*[Image: Color_HistogramMatching2.png]*

The histogram matching is performed according to the steps described below:

1. The color histograms of the target image and predefined color-samples are generated in the color space used to perform the matching.
2. The frequency of each bin is calculated based on the individual frequencies of its constituent pixels. The bin frequencies are then normalized between 0.0 and 1.0, with 1.0 representing the sum of the frequencies of all bins.
3. Matching occurs between the two bins with the highest frequencies, provided their color distance is within the distance specified using [`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md). The match score _S_ is incremented according to the formula `_S<sub>n</sub> _ = _S<sub>n-1</sub> _ + min(_f_ <sub>bin1</sub>, _f_ <sub>bin2</sub>)*(1 - colorDistance(color<sub>bin1</sub>, color<sub>bin2</sub>))`, where:
   - _f_ represents a normalized bin frequency (`0.0 &lt;= _f_ &lt;= 1.0`).
   - colorDistance corresponds to the normalized color distance between the two specified colors, within the color space used for matching (`0.0 &lt;= colorDistance &lt;= 1.0`).
4. The lesser of the two bin frequencies is then subtracted from both bin frequencies, resulting in the lesser bin being completely emptied.
5. Steps 3 and 4 are repeated until no more bins can be matched.
6. The final match score is normalized as a percentage between 0.0 and 100.0, with a score of 100.0 indicating a perfect match.

The relevance score for the matching operation is calculated according to the formula `_RelevanceScore_ = (100.0 - _BestScore_)<sup>-1</sup> / sum((100.0 - _Score_)<sup>-1</sup>)`, where:

- _BestScore_ refers to the score of the winning color-sample.
- The sum is taken over all the color-samples, and _Score_ refers to the score of the sample being summed.
