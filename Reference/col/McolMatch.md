---
doctype: Reference
module: col
function: McolMatch
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolMatch"
---

# McolMatch

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

> Match target areas with color-samples.

## Syntax

```c
void McolMatch(
    AIL_ID    ContextId,                 //in
    AIL_ID    TargetColorImageId,        //in
    AIL_ID    TargetColorProfileId,      //in
    AIL_ID    AreaIdentifierImageId,     //in
    AIL_ID    ColorResultOrDestImageId,  //out
    AIL_INT64 ControlFlag                //in
)
```

## Description

This function matches the entire target image, or specific areas of the target image, with the color-samples defined in a color matching context. This function allows you to identify a target area's best-matched color-sample, or to determine the proportion of color in a target area, based on the color-samples provided.

To match multiple target areas, you must specify an area identifier image. Each unique, non-zero label in the area identifier image defines an independent target area. Touching target areas with different labels are considered distinct. If you use an area identifier image that does not contain any non-zero labels, the match is not performed, no resulting image is produced, and all results regarding target areas and color-samples are unavailable.

To retrieve the results of the match, you must set the [`ControlFlag`](../../Reference/col/McolMatch.md) parameter to [`M_DEFAULT`](../../Reference/col/McolMatch.md) and specify a result buffer using the [`ColorResultOrDestImageId`](../../Reference/col/McolMatch.md) parameter; you must then use this result buffer with [`McolGetResult`](../../Reference/col/McolGetResult.md). In this case, to draw specific features of a color-sample or of a color matching result, use [`McolDraw`](../../Reference/col/McolDraw.md).

You can also use [`McolMatch`](../../Reference/col/McolMatch.md) to retrieve a single image result, by setting the [`ControlFlag`](../../Reference/col/McolMatch.md) parameter to a drawing operation ([`M_DRAW_...`](../../Reference/col/McolMatch.md)) and specifying an image buffer with the [`ColorResultOrDestImageId`](../../Reference/col/McolMatch.md) parameter. In this case, since you do not have a result buffer, you will not be able to use [`McolGetResult`](../../Reference/col/McolGetResult.md).

By default, a minimum distance operation is used to match the color of the target area with the color of the color-sample. To change the default operation mode, use [`McolSetMethod`](../../Reference/col/McolSetMethod.md). You can also use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) to change the default distance type of the match operation.

For color-samples defined from an image, Aurora Imaging Library performs an estimation of the color. This estimate is based on color statistics, such as the mean; it is considered the color of the color-sample and is used in the match. The same kind of color estimation is also calculated for target areas, when matching with a minimum distance operation.

Aurora Imaging Library assumes the color you are using is in the context's source color space ([`McolAlloc`](../../Reference/col/McolAlloc.md)). Therefore all color data that you provide must be consistent. For example, if you allocate an RGB context, bands 0, 1, and 2 are assumed to be red, green, and blue; you will therefore not receive an error if you try to match RGB target areas with HSL color-samples. Similarly, you will not receive an error if you try to match a 16-bit target area with an 8-bit color-sample. Even when the color data is inconsistent, Aurora Imaging Library still processes it as is, which can produce misleading results.

You must set the color space encoding according to the actual dynamic range of your color data, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_ENCODING`](../../Reference/col/McolControl.md). By default, the Color Analysis module assumes an 8-bit color space encoding (regardless of the buffer depth).

Certain modifications to the color matching context, such as adding or deleting color-samples, or changing some control settings, require you to preprocess the color matching context ([`McolPreprocess`](../../Reference/col/McolPreprocess.md)) before any subsequent call to [`McolMatch`](../../Reference/col/McolMatch.md). To inquire whether you should preprocess, use [`McolInquire`](../../Reference/col/McolInquire.md) with [`M_PREPROCESSED`](../../Reference/col/McolInquire.md).

The function has been optimized for performance under the following conditions:

- Buffers must be of the same type, as specified using [`McolControl`](../../Reference/col/McolControl.md) with [`M_ENCODING`](../../Reference/col/McolControl.md), preferably 8-bit or 16-bit.
- The target image buffer must have a width greater than or equal to 8 pixels.
- The area identifier image buffer must not be compressed (you cannot use [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) with [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)).
- When using multiple samples, an 8-bit target image buffer, and [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with either [`M_EUCLIDEAN`](../../Reference/col/McolSetMethod.md) or [`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md), you should set the maximum tolerance for the color distance to a value less than 2<sup>24/2</sup>, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md).
  Note that 2<sup>24/2</sup> is an internally established limit required for an optimal match process.
- When using multiple samples, a 16-bit target image buffer, and [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with either [`M_EUCLIDEAN`](../../Reference/col/McolSetMethod.md) or [`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md), you should set the maximum tolerance for the color distance to a value less than 2<sup>53/2</sup>, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_TOLERANCE`](../../Reference/col/McolControl.md).
  Note that 2<sup>53/2</sup> is an internally established limit required for an optimal match process.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the color matching context. The color matching context must have been previously allocated on the required system using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md).

### `TargetColorImageId` *(in, AIL_ID)*

Specifies the identifier of the target image buffer with which to match the color-samples.

### `TargetColorProfileId` *(in, AIL_ID)*

Specifies the identifier of the target image's reference color space. This is the standard used to interpret the target's color data when performing an internal conversion before the match operation, using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the CIELAB conversion mode.

*For specifying the target image's color space profile*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use standard RGB specifications (sRGB), as defined by the International Electrotechnical Commission (IEC) Project Team 61966-2-1. |

### `AreaIdentifierImageId` *(in, AIL_ID)*

Specifies the identifier of the image buffer containing the target area labels. Area identifier images can be unsigned 1-band, packed binary, 8- or 16-bit grayscale images.

### `ColorResultOrDestImageId` *(out, AIL_ID)*

Specifies the identifier of the image buffer or result buffer in which to write the results of the color matching operation.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the results to produce.

*For specifying that results should be written in the result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Calculates all possible results and writes them in the specified result buffer ([`ColorResultOrDestImageId`](../../Reference/col/McolMatch.md)). |

*For specifying that a specific image result should be drawn in the image buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_AREA_MATCH_USING_COLOR` | Draws, for each target area, the color of the best-matched color-sample. The background color and outlier color are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md), respectively.

For [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolMatch.md), you must set the [`ColorResultOrDestImageId`](../../Reference/col/McolMatch.md) parameter to the identifier of a 3-band color image buffer. |
| `M_DRAW_AREA_MATCH_USING_LABEL` | Draws, for each target area, the label of the best-matched color-sample. The background color and outlier label are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_LABEL`](../../Reference/col/McolControl.md), respectively.

For [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolMatch.md), you must set the [`ColorResultOrDestImageId`](../../Reference/col/McolMatch.md) parameter to the identifier of a grayscale image buffer. To inquire the required buffer depth, use [`McolInquire`](../../Reference/col/McolInquire.md) with [`M_SAMPLE_LABEL_SIZE_BIT`](../../Reference/col/McolInquire.md). |
| `M_DRAW_DISTANCE` | Draws the distance between the estimated color (average color) of the target area (for an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode) or the color of the target pixel (for an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode), and the color of its best-matched color-sample. Operation modes are set with [`McolSetMethod`](../../Reference/col/McolSetMethod.md). The background color and outlier color are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md), respectively.

For [`M_DRAW_DISTANCE`](../../Reference/col/McolMatch.md), you must set the [`ColorResultOrDestImageId`](../../Reference/col/McolMatch.md) parameter to the identifier of a grayscale image buffer.

To draw normalized distances, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_IMAGE_NORMALIZE`](../../Reference/col/McolControl.md) before drawing.

The outlier color, which is also drawn with [`M_DRAW_DISTANCE`](../../Reference/col/McolMatch.md), can be difficult to identify if it is similar to the resulting distance. For example, if the outlier color is 0.0, you wouldn't be able to distinguish it if the resulting distance is also 0.0. In this case, you can use [`M_DRAW_..._USING_LABEL`](../../Reference/col/McolMatch.md), which draws the outlier label.

[`M_DRAW_DISTANCE`](../../Reference/col/McolMatch.md) is not available if the operation mode is set to [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)). |
| `M_DRAW_PIXEL_MATCH_USING_COLOR` | Draws, for each pixel in each target area, the color of the color-sample for which each pixel voted. The background color and outlier color are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md), respectively.

For [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolMatch.md), you must set the [`ColorResultOrDestImageId`](../../Reference/col/McolMatch.md) parameter to the identifier of a 3-band color image buffer, and you must use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md). |
| `M_DRAW_PIXEL_MATCH_USING_LABEL` | Draws, for each pixel in each target area, the label of the color-sample for which each pixel voted. The background color and outlier label are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_LABEL`](../../Reference/col/McolControl.md), respectively.

For [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolMatch.md), you must use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md), and you must set the [`ColorResultOrDestImageId`](../../Reference/col/McolMatch.md) parameter to the identifier of a grayscale image buffer. To inquire the required buffer depth, use [`McolInquire`](../../Reference/col/McolInquire.md) with [`M_SAMPLE_LABEL_SIZE_BIT`](../../Reference/col/McolInquire.md). |

*For specifying whether to invert colors*

| Value | Description |
| --- | --- |
| `M_INVERTED_COLORS` | Inverts the color of the best-matched color-sample in the specified image. |
