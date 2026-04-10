---
doctype: Reference
module: col
function: McolGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolGetResult"
---

# McolGetResult

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

> Get the specified type of results from a color matching result buffer.

## Syntax

```c
void McolGetResult(
    AIL_ID    ResultId,                 //in
    AIL_INT   AreaLabel,                //in
    AIL_INT   ColorSampleIndexOrLabel,  //in
    AIL_INT64 ResultType,               //in
    void *    ResultArrayPtr            //out
)
```

## Description

This function retrieves the results of the specified type from a color matching result buffer. Results are only available after calling [`McolMatch`](../../Reference/col/McolMatch.md), and are organized by target area labels and color-sample indices. You can get results obtained from a specific target area or for all target areas. Similarly, you can get results obtained from a specific color-sample or for all color-samples.

To determine the order of the results, use [`M_AREA_LABEL_VALUE`](../../Reference/col/McolGetResult.md), and set the [`AreaLabel`](../../Reference/col/McolGetResult.md) parameter to [`M_ALL`](../../Reference/col/McolGetResult.md) and the [`ColorSampleIndexOrLabel`](../../Reference/col/McolGetResult.md) parameter to [`M_GENERAL`](../../Reference/col/McolGetResult.md).

## Parameters

### `ResultId` *(in, AIL_ID)*

Specifies the identifier of the color matching result buffer from which to get results.

### `AreaLabel` *(in, AIL_INT)*

Specifies the label of the target area(s) for which to get results.

*For specifying the target area's label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to retrieve results for each target area. |
| `M_GENERAL` | Specifies to retrieve general results relating to the color matching context. This type of result is not specific to any one target area or color-sample. |
| `Value > 0` | Specifies a specific target area's label. |

### `ColorSampleIndexOrLabel` *(in, AIL_INT)*

Specifies the color-sample(s) for which to get results. Set this parameter to one of the following values.

*For specifying the color-sample*

| Value | Description |
| --- | --- |
| `M_SAMPLE_INDEX` | Specifies the index of a color-sample. |
| `M_SAMPLE_LABEL` | Specifies the label of a color-sample. |
| `M_ALL` | Specifies to retrieve results for each color-sample. Results are ordered according to the color-sample's index value, in increasing order. |
| `M_GENERAL` | Specifies to retrieve general results not specific to any one color-sample. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the first element of the array in which to write the requested information.

## Parameter Associations

### For retrieving general results relating to the color matching context

To retrieve general results relating to the color matching context, the [`ResultType`](../../Reference/col/McolGetResult.md) parameter can be set to one of the following values. In this case, you must set the [`AreaLabel`](../../Reference/col/McolGetResult.md) and [`ColorSampleIndexOrLabel`](../../Reference/col/McolGetResult.md) parameters to [`M_GENERAL`](../../Reference/col/McolGetResult.md).

---

### `M_AREA_IMAGE_SIGN`

Retrieves the data type required for the image buffer in which to draw the target area(s) of the area identifier image ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_AREA`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `M_UNSIGNED` | Specifies unsigned data. |

---

### `M_AREA_IMAGE_SIZE_BAND`

Retrieves the number of surfaces (color bands) required for the image buffer in which to draw the target area(s) of the area identifier image ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_AREA`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `1` | Specifies 1 color band. |

---

### `M_AREA_IMAGE_SIZE_BIT`

Retrieves the depth per band required for the image buffer in which to draw the target area(s) of the area identifier image ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_AREA`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `1` | Specifies 1-bit data depth per band. |
| `8` | Specifies 8-bit data depth per band. |
| `16` | Specifies 16-bit data depth per band. |

---

### `M_AREA_IMAGE_SIZE_X`

Retrieves the width required for the image buffer in which to draw the target area(s) of the area identifier image ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_AREA`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the image buffer, in pixels. |

---

### `M_AREA_IMAGE_SIZE_Y`

Retrieves the height required for the image buffer in which to draw the target area(s) of the area identifier image ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_AREA`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the image buffer, in pixels. |

---

### `M_AREA_IMAGE_TYPE`

Retrieves the data type and depth required for the image buffer in which to draw the target area(s) of the area identifier image ([`McolDraw`](../../Reference/col/McolDraw.md) with [`M_DRAW_AREA`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 1` | Specifies 1-bit unsigned data. |
| `M_UNSIGNED + 8` | Specifies 8-bit unsigned data. |
| `M_UNSIGNED + 16` | Specifies 16-bit unsigned data. |

---

### `M_AREA_MATCH_IMAGE_SIZE_X`

Retrieves the width required for the image buffer in which to draw (for each target area) the color or label of the best-matched color-sample ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) or [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the image buffer, in pixels. |

---

### `M_AREA_MATCH_IMAGE_SIZE_Y`

Retrieves the height required for the image buffer in which to draw (for each target area) the color or label of the best-matched color-sample ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) or [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the image buffer, in pixels. |

---

### `M_DISTANCE_IMAGE_SIGN`

Retrieves the data type required for the image buffer in which to draw the distance between the color of the target area (for an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode) or target pixel (for an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode), and the color of its best-matched color-sample ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)). Operation modes are set with [`McolSetMethod`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_FLOAT` | Specifies floating-point data. |

---

### `M_DISTANCE_IMAGE_SIZE_BAND`

Retrieves the number of surfaces (color bands) required for the image buffer in which to draw the distance between the color of the target area (for an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode) or target pixel (for an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode), and the color of its best-matched color-sample ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)). Operation modes are set with [`McolSetMethod`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `1` | Specifies 1 color band. |

---

### `M_DISTANCE_IMAGE_SIZE_BIT`

Retrieves the depth per band required for the image buffer in which to draw the distance between the color of the target area (for an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode) or target pixel (for an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode), and the color of its best-matched color-sample ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)). Operation modes are set with [`McolSetMethod`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `32` | Specifies 32-bit data depth per band. |

---

### `M_DISTANCE_IMAGE_SIZE_X`

Retrieves the width required for the image buffer in which to draw the distance between the color of the target area (for an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode) or target pixel (for an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode), and the color of its best-matched color-sample ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)). Operation modes are set with [`McolSetMethod`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the image buffer, in pixels. |

---

### `M_DISTANCE_IMAGE_SIZE_Y`

Retrieves the height required for the image buffer in which to draw the distance between the color of the target area (for an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode) or target pixel (for an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode), and the color of its best-matched color-sample ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)). Operation modes are set with [`McolSetMethod`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the image buffer, in pixels. |

---

### `M_DISTANCE_IMAGE_TYPE`

Retrieves the data type and depth required for the image buffer in which to draw the distance between the color of the target area (for an [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode) or target pixel (for an [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) operation mode), and the color of its best-matched color-sample ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)). Operation modes are set with [`McolSetMethod`](../../Reference/col/McolSetMethod.md).

| Value | Description |
| --- | --- |
| `M_FLOAT + 32` | Specifies 32-bit floating-point data. |

---

### `M_MAX_DISTANCE`

Retrieves the maximum (largest) color distance among all distances between the target area and all matching color-samples.  When the operation mode is [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)), the largest color distance refers to the maximal distance among all areas that were matched with a color-sample.  When the operation mode is [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)), the largest color distance refers to the maximal distance among all the pixels that voted for a color-sample.  The maximum distance value is used internally when drawing the color distance of the area identifier image ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_DISTANCE`](../../Reference/col/McolDraw.md)).

---

### `M_NUMBER_OF_AREAS`

Retrieves the number of target areas in the area identifier image with which you performed the matching operation.

---

### `M_NUMBER_OF_SAMPLES`

Retrieves the number of color-samples defined in the context.

---

### `M_PIXEL_MATCH_IMAGE_SIZE_X`

Retrieves the width required for the image buffer in which to draw (for each pixel in each target area) the color or label of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).  This result is not available if the operation mode is set to [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the image buffer, in pixels. |

---

### `M_PIXEL_MATCH_IMAGE_SIZE_Y`

Retrieves the height required for the image buffer in which to draw (for each pixel in each target area) the color or label of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).  This result is not available if the operation mode is set to [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the image buffer, in pixels. |

---

### `M_SAMPLE_COLOR_SIGN`

Retrieves the data type required for the image buffer in which to draw (for each target area or for each pixel in each target area) the color of the best-matched color-sample or the color of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md)).

---

### `M_SAMPLE_COLOR_SIZE_BAND`

Retrieves the number of surfaces (color bands) required for the image buffer in which to draw (for each target area or for each pixel in each target area) the color of the best-matched color-sample or the color of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md)).

---

### `M_SAMPLE_COLOR_SIZE_BIT`

Retrieves the depth per band, in bits, required for the image buffer in which to draw (for each target area or for each pixel in each target area) the color of the best-matched color-sample or the color of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md)).

---

### `M_SAMPLE_COLOR_TYPE`

Retrieves the data type and depth required for the image buffer in which to draw (for each target area or for each pixel in each target area) the color of the best-matched color-sample or the color of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_COLOR`](../../Reference/col/McolDraw.md)).

---

### `M_SAMPLE_LABEL_SIGN`

Retrieves the data type required for the image buffer in which to draw (for each target area or for each pixel in each target area) the label of the best-matched color-sample or the label of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).

---

### `M_SAMPLE_LABEL_SIZE_BAND`

Retrieves the number of surfaces (color bands) required for the image buffer in which to draw (for each target area or for each pixel in each target area) the label of the best-matched color-sample or the label of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).

---

### `M_SAMPLE_LABEL_SIZE_BIT`

Retrieves the depth per band, in bits, required for the image buffer in which to draw (for each target area or for each pixel in each target area) the label of the best-matched color-sample or the label of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).

---

### `M_SAMPLE_LABEL_TYPE`

Retrieves the data type and depth required for the image buffer in which to draw (for each target area or for each pixel in each target area) the label of the best-matched color-sample or the label of the color-sample for which each pixel voted ([`McolDraw`](../../Reference/col/McolDraw.md) or [`McolMatch`](../../Reference/col/McolMatch.md) with [`M_DRAW_AREA_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md) or [`M_DRAW_PIXEL_MATCH_USING_LABEL`](../../Reference/col/McolDraw.md)).

### For retrieving general target area results

To retrieve general results relating to a target area, the [`ResultType`](../../Reference/col/McolGetResult.md) parameter can be set to one of the following values. In this case, you must set the [`AreaLabel`](../../Reference/col/McolGetResult.md) parameter to a specific value (or [`M_ALL`](../../Reference/col/McolGetResult.md)), and the [`ColorSampleIndexOrLabel`](../../Reference/col/McolGetResult.md) parameter to [`M_GENERAL`](../../Reference/col/McolGetResult.md).

---

### `M_AREA_LABEL_VALUE`

Retrieves the label of the target areas you used to obtain results. If you do not specify an area identifier image with [`McolMatch`](../../Reference/col/McolMatch.md), the whole target image is labeled as target area 1.

---

### `M_AREA_PIXEL_COUNT`

Retrieves the total number of pixels in the target area.

---

### `M_BEST_MATCH_INDEX`

Retrieves the index of the target area's best-matched color-sample. If there is none, -1 is returned.

---

### `M_BEST_MATCH_LABEL`

Retrieves the label of the target area's best-matched color-sample. If no color-samples are matched, the value you have set using [`McolControl`](../../Reference/col/McolControl.md) with [`M_OUTLIER_LABEL`](../../Reference/col/McolControl.md) is returned.

---

### `M_OUTLIER_COVERAGE`

Retrieves the outlier coverage, which is the percentage of the number of pixels in the target area that did not vote for any color-sample. Since this result is based on pixel votes, you must use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md).

---

### `M_SCORE_RELEVANCE`

Retrieves the relevance score of the target area, as a percentage. This value indicates the significance (relevance) of [`M_SCORE`](../../Reference/col/McolGetResult.md). In statistics, this is similar to the confidence level.  The target area's relevance score is based on the operation mode specified, using [`McolSetMethod`](../../Reference/col/McolSetMethod.md):  - [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md).   `_RelevanceScore_ = (_BestDistance_)<sup>-1</sup> / sum (_Distance_ <sup>-1</sup>)`, where _BestDistance_ is the distance of the best-matched color-sample, and the sum is over all color-samples that have been matched with the target area. - [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) .   `_RelevanceScore_ = sum(_NumberOfVotes_) / _NumberOfPixelsInTheTargetArea_`, where sum(_NumberOfVotes_) refers to the total number of votes accumulated by all color-samples. - [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md).   `_RelevanceScore_ = (100.0 -_BestScore_)<sup>-1</sup> / sum ((100.0 -_Score_)<sup>-1</sup>)`, where _BestScore_ refers to the winning sample score, and the sum is over all color-samples.

---

### `M_STATUS`

Retrieves the match status of the target area.

| Value | Description |
| --- | --- |
| `M_FAILURE` | Specifies that no color-samples have been matched. |
| `M_SUCCESS` | Specifies that at least one color-sample has been matched. |

### For retrieving color-sample results

To retrieve results relating to a color-sample, the [`ResultType`](../../Reference/col/McolGetResult.md) parameter can be set to one of the following values. In this case, you must set the [`AreaLabel`](../../Reference/col/McolGetResult.md) parameter to a specific target area (or [`M_ALL`](../../Reference/col/McolGetResult.md)) and the [`ColorSampleIndexOrLabel`](../../Reference/col/McolGetResult.md) parameter to a specific color-sample (or [`M_ALL`](../../Reference/col/McolGetResult.md)).  To retrieve color-sample results for all color-samples and all areas, set both the [`AreaLabel`](../../Reference/col/McolGetResult.md) and [`ColorSampleIndexOrLabel`](../../Reference/col/McolGetResult.md) parameter to [`M_ALL`](../../Reference/col/McolGetResult.md), and use [`M_NUMBER_OF_AREAS`](../../Reference/col/McolGetResult.md) and [`M_NUMBER_OF_SAMPLES`](../../Reference/col/McolGetResult.md) to set the size of the result array (_NumberOfTargetAreas_ x _NumberOfSamples_). Note that to get a color-sample result, you must first refer to its target area.

---

### `M_COLOR_DISTANCE`

Retrieves the color distance (difference) between the target area and the specified color-sample, when using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md) operation mode.  The significance of the distance value returned depends on the distance type that you have set, using [`McolSetMethod`](../../Reference/col/McolSetMethod.md). For example, a distance value of 1 when using an [`M_MAHALANOBIS`](../../Reference/col/McolSetMethod.md) distance type is not the same as when using an [`M_MANHATTAN`](../../Reference/col/McolSetMethod.md) distance type. Note that when matching in the CIELAB source color space, you will get distance results in the CIELAB native range.

---

### `M_SAMPLE_COVERAGE`

Retrieves the proportion of pixels within the target area that voted for the color-sample, as a percentage. Since this result is based on pixel votes, you must use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md).

---

### `M_SAMPLE_LABEL_VALUE`

Retrieves the label of the color-sample corresponding to the specified index.

---

### `M_SAMPLE_MATCH_STATUS`

Retrieves whether the specified color-sample fulfills the match conditions, with respect to the distance tolerance and acceptance. To determine if the color-sample is the best-matched color-sample, use [`M_BEST_MATCH_INDEX`](../../Reference/col/McolGetResult.md) or [`M_BEST_MATCH_LABEL`](../../Reference/col/McolGetResult.md).

| Value | Description |
| --- | --- |
| `M_MATCH` | Specifies that the color-sample fulfills the match conditions. |
| `M_NO_MATCH` | Specifies that the color-sample does not fulfill the match conditions. |

---

### `M_SAMPLE_PIXEL_COUNT`

Retrieves the number of pixels within the target area that voted for the color-sample, when using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md).

---

### `M_SCORE`

Retrieves the match score, as a percentage. This score indicates the similarity between the color of the color-sample and the color of the target area.  The color-sample's match score is based on the operation mode specified, using [`McolSetMethod`](../../Reference/col/McolSetMethod.md):  - [`M_STAT_MIN_DIST`](../../Reference/col/McolSetMethod.md).   `_MatchScore_ = [_k_ / (_k_ + _Distance_)] x [(_MaxDistance_ - _Distance_) / _MaxDistance_]`, where _Distance_ refers to the current color-sample distance and _MaxDistance_ refers to the maximum distance possible (in the color space). - [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md) .   `_MatchScore_ = _NumberOfVotes_ / sum (_NumberOfVotes_)`, where _NumberOfVotes_ refers to the current color-sample's number of votes, and the sum is over the number of votes for all color-samples. - [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md).   `_MatchScore_` refers to a measure of similarity between sample and target histograms.

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

Some results might be altered if they do not fit numerically in the data type you specify. For example, if you cast a result value of 300 to a _char_, information will be lost. The default data type, _double_, will never result in lost information.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

This result is available after you specify an area identifier image with [`McolMatch`](../../Reference/col/McolMatch.md), enable the [`McolControl`](../../Reference/col/McolControl.md) [`M_SAVE_AREA_IMAGE`](../../Reference/col/McolControl.md) control type, and find a match.

This result is available after you enable the [`McolControl`](../../Reference/col/McolControl.md) [`M_GENERATE_DISTANCE_IMAGE`](../../Reference/col/McolControl.md) control type (required for [`McolDraw`](../../Reference/col/McolDraw.md)), and find a match.

This result is available after you enable the [`McolControl`](../../Reference/col/McolControl.md) [`M_GENERATE_PIXEL_MATCH`](../../Reference/col/McolControl.md) control type (required for [`McolDraw`](../../Reference/col/McolDraw.md)), and find a match. You must also use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md).

This result is available after you enable the [`McolControl`](../../Reference/col/McolControl.md) [`M_GENERATE_PIXEL_MATCH`](../../Reference/col/McolControl.md) and [`M_GENERATE_SAMPLE_COLOR_LUT`](../../Reference/col/McolControl.md) control types (required for [`McolDraw`](../../Reference/col/McolDraw.md)), and find a match. You must also use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with [`M_MIN_DIST_VOTE`](../../Reference/col/McolSetMethod.md).

This result is available after you enable the [`McolControl`](../../Reference/col/McolControl.md) [`M_SAVE_AREA_IMAGE`](../../Reference/col/McolControl.md) control type (required for [`McolDraw`](../../Reference/col/McolDraw.md)), and find a match.

Note that if you have normalized distances by the maximum distance calculated, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_DISTANCE_IMAGE_NORMALIZE`](../../Reference/col/McolControl.md) set to [`M_MAX_NORMALIZE`](../../Reference/col/McolControl.md), you can draw the distance image in an unsigned image buffer.

This result is not available if the operation mode is set to [`M_HISTOGRAM_MATCHING`](../../Reference/col/McolSetMethod.md) or [`M_HISTOGRAM_VOTE`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)).
