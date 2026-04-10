---
doctype: Reference
module: col
function: McolDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolDraw"
---

# McolDraw

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

> Draw specific features of a color-sample, color element, or color matching result.

## Syntax

```c
void McolDraw(
    AIL_ID    GraphContId,         //in
    AIL_ID    ContextOrResultId,   //in
    AIL_ID    DestImageBufId,      //out
    AIL_INT64 DrawOperation,       //in
    AIL_INT   AreaLabel,           //in
    AIL_INT   SampleIndexOrLabel,  //in
    AIL_INT64 ControlFlag          //in
)
```

## Description

This function draws specific features of a selected color-sample (defined in a specified color matching or relative color calibration context), color element (defined in a specified color-sample), or of a color matching result, in the destination image buffer. Drawing operations for color matching results are only available after calling [`McolMatch`](../../Reference/col/McolMatch.md).

Color-samples or color elements are drawn using either their triplet values ([`M_TRIPLET`](../../Reference/col/McolDefine.md)) or their estimated values (for example, the average) taken from a source image ([`M_IMAGE`](../../Reference/col/McolDefine.md)), depending on how you defined them ([`McolDefine`](../../Reference/col/McolDefine.md)).

## Parameters

### `GraphContId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. Set this parameter to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResultId` *(in, AIL_ID)*

Specifies the context or result buffer for which to perform the drawing operation. The context or result buffer must have been previously allocated on the system using [`McolAlloc`](../../Reference/col/McolAlloc.md) (with [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md) or [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md)) or [`McolAllocResult`](../../Reference/col/McolAllocResult.md), respectively.

### `DestImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer in which to draw. The buffer can be any supported 1- or 3-band image buffer. By drawing into its display's overlay buffer, you can also annotate the image non-destructively.

### `DrawOperation` *(in, AIL_INT64)*

Specifies the type of drawing operation to perform.

*For drawing specific features of a color-sample*

| Value | Description |
| --- | --- |
| `M_DRAW_SAMPLE_MOSAIC` | Draws the mosaic of all the color elements found in the specified color-sample. If there is only one color element in the sample, [`M_DRAW_SAMPLE_MOSAIC`](../../Reference/col/McolDraw.md) will essentially perform the same operation as [`M_DRAW_SAMPLE`](../../Reference/col/McolDraw.md). |
| `M_DRAW_SAMPLE_MOSAIC_DONT_CARE` | Draws a mosaic mask of all "don't care" pixels in the specified color-sample. All non-zero destination pixels correspond to either a region in-between color data or to a color data mask. All destination pixels set to zero correspond to non-masked color data. |

*For drawing specific features of a color element*

| Value | Description |
| --- | --- |
| `M_DRAW_SAMPLE` | Draws the internal image of the specified color element in the destination image buffer. If you defined a triplet color element, [`M_DRAW_SAMPLE`](../../Reference/col/McolDraw.md) fills the destination image buffer with the color of the triplet. The triplet values are saturated according to the destination buffer depth. |
| `M_DRAW_SAMPLE_MASK` | Draws the mask image of the specified color element in the destination image buffer. You can only perform this operation on [`M_IMAGE`](../../Reference/col/McolDefine.md) color-samples masked with [`McolMask`](../../Reference/col/McolMask.md). To inquire if a color-sample has a mask, use [`McolInquire`](../../Reference/col/McolInquire.md) with [`M_SAMPLE_MASKED`](../../Reference/col/McolInquire.md). |

*For drawing specific features of a color matching result*

| Value | Description |
| --- | --- |
| `M_DRAW_AREA` | Draws the specified area(s) ([`AreaLabel`](../../Reference/col/McolDraw.md)) of the area identifier image.

If you specify a single target area, the background of the destination image ([`DestImageBufId`](../../Reference/col/McolDraw.md)) is untouched; if you specify [`M_ALL`](../../Reference/col/McolDraw.md), the whole area identifier image, including a 0 background, is copied to the destination image.

To perform this drawing operation, you must have first enabled [`McolControl`](../../Reference/col/McolControl.md) with [`M_SAVE_AREA_IMAGE`](../../Reference/col/McolControl.md), and called [`McolMatch`](../../Reference/col/McolMatch.md). |
| `M_DRAW_AREA_MATCH_USING_COLOR` | Draws, for each target area, the color of the best-matched color-sample. The background color and outlier color are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md), respectively.

To perform this drawing operation, you must have first enabled [`McolControl`](../../Reference/col/McolControl.md) with [`M_SAVE_AREA_IMAGE`](../../Reference/col/McolControl.md) and [`M_GENERATE_SAMPLE_COLOR_LUT`](../../Reference/col/McolControl.md), and called [`McolMatch`](../../Reference/col/McolMatch.md). |
| `M_DRAW_AREA_MATCH_USING_LABEL` | Draws, for each target area, the label of the best-matched color-sample. The background color and outlier label are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_LABEL`](../../Reference/col/McolControl.md), respectively.

To perform this drawing operation, you must have first enabled [`McolControl`](../../Reference/col/McolControl.md) with [`M_SAVE_AREA_IMAGE`](../../Reference/col/McolControl.md), and called [`McolMatch`](../../Reference/col/McolMatch.md). |
| `M_DRAW_DISTANCE` | Draws the distance between the estimated color (average color) of the target area (for an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode) or the color of the target pixel (for an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode), and the color of its best-matched color-sample. Operation modes are set with [`McolSetMethod`](../../Reference/col/McolSetMethod.md). The background color and outlier color are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md), respectively.

To perform this drawing operation, you must have first enabled [`McolControl`](../../Reference/col/McolControl.md) with [`M_GENERATE_DISTANCE_IMAGE`](../../Reference/col/McolControl.md), and called [`McolMatch`](../../Reference/col/McolMatch.md).

To draw normalized distances, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_IMAGE_NORMALIZE`](../../Reference/col/McolControl.md) before drawing.

The outlier color, which is also drawn with [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md), can be difficult to identify if it is similar to the resulting distance. For example, if the outlier color is 0.0, you wouldn't be able to distinguish it if the resulting distance is also 0.0. In this case, you can use [`M_DRAW_..._USING_LABEL`](../../Reference/col/McolDraw.md), which draws the outlier label.

[`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md) is not available if the operation mode is set to [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)). |
| `M_DRAW_PIXEL_MATCH_USING_COLOR` | Draws, for each pixel in each target area, the color of the color-sample for which each pixel voted. The background color and outlier color are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_DRAW_COLOR`](../../Reference/col/McolControl.md), respectively.

To perform this drawing operation, you must have first enabled [`McolControl`](../../Reference/col/McolControl.md) with [`M_GENERATE_PIXEL_MATCH`](../../Reference/col/McolControl.md) and [`M_GENERATE_SAMPLE_COLOR_LUT`](../../Reference/col/McolControl.md), and called [`McolMatch`](../../Reference/col/McolMatch.md). You must have also used [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md). |
| `M_DRAW_PIXEL_MATCH_USING_LABEL` | Draws, for each pixel in each target area, the label of the color-sample for which each pixel voted. The background color and outlier label are also drawn; to modify these values, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_BACKGROUND_DRAW_COLOR`](../../Reference/col/McolControl.md) and [`M_OUTLIER_LABEL`](../../Reference/col/McolControl.md), respectively.

To perform this drawing operation, you must have first enabled [`McolControl`](../../Reference/col/McolControl.md) with [`M_GENERATE_PIXEL_MATCH`](../../Reference/col/McolControl.md), and called [`McolMatch`](../../Reference/col/McolMatch.md). You must also use have used [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md). |

*For drawing specific features of either a color-sample or a color matching result (for color matching)*

| Value | Description |
| --- | --- |
| `M_DRAW_SAMPLE_COLOR_LUT` | Draws the 3-band color-sample label LUT, where the label value of each color-sample is associated with its average color.

The average colors are calculated using the color data that you provided when defining the color-sample ([`McolDefine`](../../Reference/col/McolDefine.md)). The calculation takes into account all three bands, regardless of the setting of [`McolControl`](../../Reference/col/McolControl.md) with [`M_BAND_MODE`](../../Reference/col/McolControl.md).

To perform this operation for color matching results, you must have first enabled [`McolControl`](../../Reference/col/McolControl.md) with [`M_GENERATE_SAMPLE_COLOR_LUT`](../../Reference/col/McolControl.md), and called [`McolMatch`](../../Reference/col/McolMatch.md).

[`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md) requires that you set the [`SampleIndexOrLabel`](../../Reference/col/McolDraw.md) parameter to [`M_ALL`](../../Reference/col/McolDraw.md) or [`M_DEFAULT`](../../Reference/col/McolDraw.md).

When using [`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md), the destination image buffer ([`DestImageBufId`](../../Reference/col/McolDraw.md)) must be a 3-band, 8-bit unsigned LUT buffer, with a size of _N_ x 1, where _N_ is equal to the [`M_SAMPLE_LUT_SIZE_X`](../../Reference/col/McolInquire.md) inquire type ([`McolInquire`](../../Reference/col/McolInquire.md)). You can associate this color-sample label LUT to either a display using [`MdispLut`](../../Reference/disp/MdispLut.md), or to an image buffer using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_ASSOCIATED_LUT`](../../Reference/buf/MbufControl.md). |

*For specifying whether to invert colors*

| Value | Description |
| --- | --- |
| `M_INVERTED_COLORS` | Inverts the color of the best-matched color-sample in the specified image. |

### `AreaLabel` *(in, AIL_INT)*

Specifies the label of the target area for which to perform the drawing operation, for color matching contexts. For a relative color calibration context, there are no target areas; you must specify [`M_DEFAULT`](../../Reference/col/McolDraw.md).

*For specifying the target area's label or index (for color matching)*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default. For a color matching context, the default is the same as [`M_ALL`](../../Reference/col/McolDraw.md). For a relative color calibration context, the default indicates that this information is not required. |
| `M_ALL` | Specifies all target areas. You should specify [`M_ALL`](../../Reference/col/McolDraw.md) if you did not use an area identifier image ([`M_NULL`](../../Reference/col/McolMatch.md)) with [`McolMatch`](../../Reference/col/McolMatch.md). This value is only available for color matching contexts. |
| `Value > 0` | Specifies the label of the target area, as it appears in the area identifier image.

When specifying a specific target area's label, you must use [`McolControl`](../../Reference/col/McolControl.md) with [`M_SAVE_AREA_IMAGE`](../../Reference/col/McolControl.md) set to [`M_ENABLE`](../../Reference/col/McolControl.md). This value is only available for color matching contexts. |

### `SampleIndexOrLabel` *(in, AIL_INT)*

Specifies the color-sample or color element therein for which to perform the drawing operation. Unless otherwise specified, values apply to both color matching and relative color calibration contexts. Set this parameter to one of the following values:

*For specifying the color-sample or color element*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ALL`](../../Reference/col/McolDraw.md). |
| `M_REFERENCE_SAMPLE_ITEM` | Specifies the index of a color element (item), relative to the reference color-sample. This value only applies to relative color calibration contexts ([`ContextOrResultId`](../../Reference/col/McolDraw.md)). |
| `M_SAMPLE_INDEX` | Specifies the color-sample's index, in a color context or result. |
| `M_SAMPLE_INDEX_ITEM` | Specifies the index of a color element (item), relative to a color-sample index. |
| `M_SAMPLE_LABEL` | Specifies the label of a color-sample.

To use this value, you must typically enable [`McolControl`](../../Reference/col/McolControl.md) with [`M_GENERATE_PIXEL_MATCH`](../../Reference/col/McolControl.md). However, you need not do this when performing an [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) drawing operation. |
| `M_SAMPLE_LABEL_ITEM` | Specifies the index of a color element (item), relative to a color-sample label. |
| `M_ALL` | Specifies all color-samples.

When specifying a color matching context, you can only use [`M_ALL`](../../Reference/col/McolDraw.md) with [`M_DRAW_SAMPLE_COLOR_LUT`](../../Reference/col/McolDraw.md).

When specifying a specific color-sample (in either a color matching or relative color calibration context), you can only use [`M_ALL`](../../Reference/col/McolDraw.md) with [`M_DRAW_SAMPLE`](../../Reference/col/McolDraw.md) and [`M_DRAW_SAMPLE_MASK`](../../Reference/col/McolDraw.md).

You can use [`M_ALL`](../../Reference/col/McolDraw.md) with any operation that a color matching result buffer supports. |
| `M_REFERENCE_SAMPLE` | Specifies the reference color-sample. This value only applies to relative color calibration contexts. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

> **Note:** If you do not specify the index or label of a specific color element, the first color element in the specified color-sample will be drawn.
