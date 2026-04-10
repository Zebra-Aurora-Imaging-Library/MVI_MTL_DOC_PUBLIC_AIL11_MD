---
doctype: Reference
module: pat
function: MpatInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / pat / MpatInquire"
---

# MpatInquire

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

> Inquire about a setting of a Pattern Matching context, one or all of the search models, or a Pattern Matching result buffer.

## Syntax

```c
AIL_INT MpatInquire(
    AIL_ID    ContextOrResultPatId,  //in
    AIL_INT   Index,                 //in
    AIL_INT64 InquireType,           //in
    void *    UserVarPtr             //out
)
```

## Description

This function inquires information about a specified Pattern Matching context, one or all of the search models contained therein, or a specified Pattern Matching result buffer. Most inquire type settings can be controlled using [`MpatControl`](../../Reference/pat/MpatControl.md). Note that Pattern Matching result buffer values can be obtained with [`MpatGetResult`](../../Reference/pat/MpatGetResult.md).

The position ([`M_ORIGINAL_X`](../../Reference/pat/MpatInquire.md) and [`M_ORIGINAL_Y`](../../Reference/pat/MpatInquire.md)) can be directly compared with search results (by [`MpatFind`](../../Reference/pat/MpatFind.md)), to calculate the displacement of target images relative to the model's source image.

## Parameters

### `ContextOrResultPatId` *(in, AIL_ID)*

Specifies the Pattern Matching context identifier or Pattern Matching result buffer identifier about which to inquire information. The Pattern Matching context or result buffer must have been previously allocated on the required system using [`MpatAlloc`](../../Reference/pat/MpatAlloc.md) or [`MpatAllocResult`](../../Reference/pat/MpatAllocResult.md), respectively.

### `Index` *(in, AIL_INT)*

Specifies that information will be inquired about the Pattern Matching context, an individual model, or a Pattern Matching result buffer. This parameter should be set to one of the following values.

*For specifying the index of a model or result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If a Pattern Matching context is specified, this parameter inquires information about model index 0.

If a Pattern Matching result buffer is specified, same as [`M_GENERAL`](../../Reference/pat/MpatInquire.md). |
| `M_CONTEXT` | Inquires information about a Pattern Matching context, if one is specified. |
| `M_GENERAL` | Inquires information about a Pattern Matching result buffer, if one is specified. |
| `Value >= 0` | Inquires information about a model, if a Pattern Matching context is specified. Set the [`Index`](../../Reference/pat/MpatInquire.md) parameter to the index of the required model. |

### `InquireType` *(in, AIL_INT64)*

Specifies the parameter about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MpatInquire`](../../Reference/pat/MpatInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about the system on which the model or result buffer is allocated

For the following inquire type, the [`ContextOrResultPatId`](../../Reference/pat/MpatInquire.md) can specify a Pattern Matching context or result buffer and the [`Index`](../../Reference/pat/MpatInquire.md) parameter must be set to [`M_CONTEXT`](../../Reference/pat/MpatInquire.md) or[`M_GENERAL`](../../Reference/pat/MpatInquire.md), respectively.

---

### `M_OWNER_SYSTEM`

Inquires the system on which the model or result buffer is allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, which you have allocated using the [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) function. |

### For inquiring about the general settings of the Pattern Matching context

The following inquire types and inquire values allows you to inquire about the settings for a Pattern Matching context. For inquiring about general context settings, the [`Index`](../../Reference/pat/MpatInquire.md) parameter must be set to [`M_CONTEXT`](../../Reference/pat/MpatInquire.md).

---

### `M_CONTEXT_TYPE`

Inquires the type of context used.

| Value | Description |
| --- | --- |
| `M_NORMALIZED` | Specifies that all search models within the Pattern Matching context should be normalized. |

---

### `M_NUMBER_MODELS`

Inquires the number of models in the context.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of models. |

---

### `M_PREPROCESSED`

Inquires whether the Pattern Matching context is preprocessed. The context must be preprocessed (using [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md)) before calling [`MpatFind`](../../Reference/pat/MpatFind.md). After certain settings of the Pattern Matching context are changed with [`MpatControl`](../../Reference/pat/MpatControl.md), or after a model is added or removed with [`MpatDefine`](../../Reference/pat/MpatDefine.md), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies the context is not preprocessed. |
| `M_TRUE` | Specifies the context is preprocessed. |

---

### `M_SEARCH_MODE`

Inquires the search mode to use when performing an [`MpatFind`](../../Reference/pat/MpatFind.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIND_ALL_MODELS` *(default)* | Specifies to search for all the specified models in the target image using their own search settings. |
| `M_FIND_BEST_MODELS` | Specifies to search for the best occurrences of any model in the target image, using the search settings from the first model in the Pattern Matching context. |

### For inquiring about an individual model in a Pattern Matching context

The following inquire types and inquire values allow you to inquire about the settings for an individual model in a Pattern Matching context. For inquiring about the settings of one model, the [`Index`](../../Reference/pat/MpatInquire.md) parameter must be set to the model index of a specific model in the Pattern Matching context.

---

### `M_ACCEPTANCE`

Inquires the acceptance level.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level, as a percentage. |

---

### `M_ACCURACY`

Inquires the required positional accuracy. Note that the search speed slightly affects the positional accuracy. To inquire the search speed, use [`M_SPEED`](../../Reference/pat/MpatInquire.md).

| Value | Description |
| --- | --- |
| `M_HIGH` | Specifies a high accuracy (typically ± 0.05 pixels). |
| `M_LOW` | Specifies a low accuracy (typically ± 0.20 pixels). |
| `M_MEDIUM` *(default)* | Specifies a medium accuracy (typically ± 0.10 pixels). |

---

### `M_ALLOC_OFFSET_X`

Inquires the X-offset of model's top-left corner relative to the top-left corner of the model's source image.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset, in pixels. |

---

### `M_ALLOC_OFFSET_Y`

Inquires the Y-offset of model's top-left corner relative to the top-left corner of the model's source image.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset, in pixels. |

---

### `M_ALLOC_SIZE_BIT`

Inquires the depth of the model image. Note that the model image is a single-band image.

| Value | Description |
| --- | --- |
| `1` | Specifies that the data depth is 1 bit. |
| `8` | Specifies that the data depth is 8 bits. |
| `16` | Specifies that the data depth is 16 bits. |
| `32` | Specifies that the data depth is 32 bits. |

---

### `M_ALLOC_SIZE_X`

Inquires the model's width.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width, in pixels. |

---

### `M_ALLOC_SIZE_Y`

Inquires the model's height.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height, in pixels. |

---

### `M_CERTAINTY`

Inquires the certainty level.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level, as a percentage. |

---

### `M_HAS_DONT_CARE_MASK`

Inquires whether the model has an [`M_DONT_CARE`](../../Reference/mod/MmodMask.md) mask applied to it.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that an [`M_DONT_CARE`](../../Reference/mod/MmodMask.md) mask has not been applied to the model. |
| `M_TRUE` | Specifies that an [`M_DONT_CARE`](../../Reference/mod/MmodMask.md) mask has been applied to the model. |

---

### `M_MODEL_TYPE`

Inquires the type of model used.

| Value | Description |
| --- | --- |
| `M_AUTO_MODEL` | Defines one or more models from the most-suitable distinct areas found, of the specified dimensions, in the model's source image. |
| `M_REGULAR_MODEL` | Defines a regular search model, using data from the specified area of the model's source image. |

---

### `M_NUMBER`

Inquires the maximum expected number of occurrences of a search model to find in a target image.

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to look for all matches that are greater than or equal to the acceptance level. |
| `Value >= 0` *(default)* | Specifies the maximum number of occurrences for which to look in the target image. |

---

### `M_ORIGINAL_X`

Inquires the X-offset of the model's reference position relative to the top-left corner of the model's source image (takes into account the [`M_REFERENCE_X`](../../Reference/pat/MpatControl.md)setting).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset. |

---

### `M_ORIGINAL_Y`

Inquires the Y-offset of the model's reference position relative to the top-left corner of the model's source image (takes into account the [`M_REFERENCE_Y`](../../Reference/pat/MpatControl.md)setting).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset. |

---

### `M_REFERENCE_X`

Inquires the x-offset of the origin of the model's reference position relative to the model origin.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the X-offset of the model's reference position relative to the model origin, in pixels. |

---

### `M_REFERENCE_Y`

Inquires the y-offset of the origin of the model's reference position relative to the model origin.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the Y-offset of the model's reference position relative to the model origin, in pixels. |

---

### `M_ROTATED_MODEL_MINIMUM_SCORE`

Inquires whether to use angle refinement mode as well as sets the score to use for this mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the angle refinement mode. |
| `0.0 <= Value <= 100.0` | Specifies the minimum score. |

---

### `M_SEARCH_ANGLE`

Inquires whether search for occurrences of the model at the nominal search angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the nominal search angle, in degrees. |

---

### `M_SEARCH_ANGLE_ACCURACY`

Inquires the required precision of the nominal angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle accuracy equals the tolerance. |
| `0.0 < Value <= 180.0` | Specifies the angle accuracy, in degrees. |

---

### `M_SEARCH_ANGLE_DELTA_NEG`

Inquires the lower limit for the angular range, relative to the nominal search angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `0.0 <= Value <= 180.0` | Specifies the angle in a clockwise rotation, in degrees. |

---

### `M_SEARCH_ANGLE_DELTA_POS`

Inquires the upper limit for the angular range, relative to the nominal search angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `0.0 <= Value <= 180.0` | Specifies the angle in a counter-clockwise rotation, in degrees. |

---

### `M_SEARCH_ANGLE_INTERPOLATION_MODE`

Inquires the type of interpolation to use when rotating the model to perform the search at an angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BICUBIC` | Specifies bicubic interpolation. |
| `M_BILINEAR` | Specifies bilinear interpolation. |
| `M_NEAREST_NEIGHBOR` *(default)* | Specifies nearest neighbor interpolation. |

---

### `M_SEARCH_ANGLE_MODE`

Inquires whether to perform calculations specific to angular-range search strategies.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable the angular search mode. |
| `M_ENABLE` | Specifies to enable the angular search mode. |

---

### `M_SEARCH_ANGLE_TOLERANCE`

Inquires the full range of degrees within which the pattern in the target image can be rotated from a model at a specific angle and still meet the acceptance level.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies that the tolerance will be automatically determined based on an analysis of the model. |
| `0.0 < Value <= 180.0` *(default)* | Specifies the step angle, in degrees. |

---

### `M_SEARCH_OFFSET_X`

Inquires the X-offset of the specified model's search region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the X-coordinate of the top-left corner of the target image. |
| `Value` | Specifies the X-coordinate, relative to the pixel coordinate system, of where to start the search. |

---

### `M_SEARCH_OFFSET_Y`

Inquires the Y-offset of the specified model's search region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the Y-coordinate of the top-left corner of the target image. |
| `Value` | Specifies the Y-coordinate, relative to the pixel coordinate system, of where to start the search. |

---

### `M_SEARCH_SIZE_X`

Inquires the width of the search region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to use the width of the full image. |
| `Value` | Specifies the width, in pixels. |

---

### `M_SEARCH_SIZE_Y`

Inquires the height of the search region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to use the height of the full image. |
| `Value` | Specifies the height, in pixels. |

---

### `M_SPEED`

Inquires the required search speed when using [`MpatFind`](../../Reference/pat/MpatFind.md).

| Value | Description |
| --- | --- |
| `M_HIGH` | Specifies a high search speed. |
| `M_LOW` | Specifies a low search speed. |
| `M_MEDIUM` *(default)* | Specifies a medium search speed. |
| `M_VERY_HIGH` | Specifies a very high search speed. |
| `M_VERY_LOW` | Specifies a very low search speed. |

### Combination Constants — Returns whether circular overscan data was extracted with the model

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether circular overscan data was extracted with the model.

| Value | Description |
| --- | --- |
| `M_CIRCULAR_OVERSCAN` | Specifies that the model is extracted from the source image with the circular overscan data. |

### For inquiring about the advanced settings

The following inquire types and inquire values are for advanced users of the Pattern Matching module. If the default settings do not satisfy the requirements of your application, these inquire types and inquire values can be used to fine tune the model's search settings. The default settings typically provide the best results for most search operations. To gain a clearer understanding of how the search algorithm works, see [Pattern matching algorithm (for advanced users)](../../UserGuide/C07_Pattern_matching/S10_Pattern_matching_algorithm_for_advanced_users.md).  For these inquire types, the [`Index`](../../Reference/pat/MpatInquire.md) parameter must be set to the model index of a specific model in the Pattern Matching context.

---

### `M_COARSE_SEARCH_ACCEPTANCE`

Inquires the coarse search acceptance threshold level to use for rejecting candidate model peaks at low resolution levels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to determine the coarse search acceptance threshold level automatically. |
| `1.0 <= Value <= 100.0` | Specifies the coarse search acceptance threshold level. |

---

### `M_EXTRA_CANDIDATES`

Inquires the number of extra candidates to consider.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of extra candidates. |

---

### `M_FAST_FIND`

Inquires whether to use fast peak finding.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use preprocessing to decide if fast peak finding is appropriate. |
| `M_DISABLE` | Specifies not to use fast peak finding. |
| `M_ENABLE` | Specifies to use fast peak finding. |

---

### `M_FIRST_LEVEL`

Inquires the resolution level for the initial stage (lowest resolution) of the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_CONTENT_BASED` | Specifies to automatically determine the first level based on an analysis of the model contents. |
| `M_AUTO_SIZE_BASED` *(default)* | Specifies to automatically determine the first level based on the model size. |
| `0 <= Value <= MaxLevel` | Specifies the first level, where MaxLevel is the maximum possible level. |

---

### `M_LAST_LEVEL`

Inquires the resolution level for the final stage (highest resolution) of the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the last level automatically. |
| `0 <= Value <= MaxLevel` | Specifies the last level, where MaxLevel is the maximum possible level. |

---

### `M_MAX_INITIAL_PEAKS`

Inquires the maximum number of returned peaks after an initial search at the first level for occurrences of multiple models, or a single model with circular overscan data (that is, [`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md) or [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md) with [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to limit the returned number of peaks for optimal performance. |
| `M_ALL` | Specifies all occurrences that pass the model acceptance threshold should be returned. |

---

### `M_MIN_SEPARATION_X`

Inquires the minimum separation (in X) between two occurrences of the same model in order for them to be considered distinct.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Sets the minimum separation to 75% of the model width ([`Param3`](../../Reference/pat/MpatDefine.md) of [`MpatDefine`](../../Reference/pat/MpatDefine.md)) when creating an [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md). |
| `Value` | Specifies the minimum separation as a percentage of the model's width. |

---

### `M_MIN_SEPARATION_Y`

Inquires the minimum separation (in Y) between two occurrences of the same model in order for them to be considered distinct.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Sets the minimum separation to 75% of the model height ( [`Param4`](../../Reference/pat/MpatDefine.md) of [`MpatDefine`](../../Reference/pat/MpatDefine.md)) when creating an [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md)). |
| `Value` | Specifies the minimum separation as a percentage of the model's height. |

---

### `M_MODEL_MAX_LEVEL`

Inquires the maximum search level possible for the model.

| Value | Description |
| --- | --- |
| `Value >= 7` | Specifies the maximum search level possible for the model. |

---

### `M_MODEL_STEP`

Inquires whether all or every second pixel in the model is used in the correlation during the high resolution stage of the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to automatically establish the model step. |
| `1` | Specifies to use all model pixels. |
| `2` | Specifies to use every second model pixel (in both the X and Y directions). |

---

### `M_PROC_FIRST_LEVEL`

Inquires the default resolution level for the initial stage (lowest resolution) of the search.

| Value | Description |
| --- | --- |
| `0 <= Value <= MaxLevel` | Specifies the lowest resolution level, where _MaxLevel_ is the maximum level. To establish the maximum level, use [`M_MODEL_MAX_LEVEL`](../../Reference/pat/MpatInquire.md). |

---

### `M_PROC_LAST_LEVEL`

Inquires the highest level for the final stage (highest resolution) of the search.

| Value | Description |
| --- | --- |
| `0 <= Value <= MaxLevel` | Specifies the lowest resolution level, where MaxLevel is the maximum level. To establish the maximum level, use [`M_MODEL_MAX_LEVEL`](../../Reference/pat/MpatInquire.md). |

---

### `M_SEARCH_ANGLE_ACCURACY_MODE`

Inquires the method for scanning the specified angular range for multiple occurrences, or a single occurrence with circular overscan data (that is, [`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md) or [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md) with[`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to return the closest occurrence to the approximate location within the specified angular range, for optimal performance. |
| `M_ALL` | Specifies that all possible occurrences that are found within the range of the accuracy specified with [`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatInquire.md) will be scanned, and the best will be returned. |

### For inquiring about the result buffer

For a result buffer identifier, the [`InquireType`](../../Reference/pat/MpatInquire.md) parameter can be set to one of the values below. For inquiring about the settings of a Pattern Matching result buffer, the [`Index`](../../Reference/pat/MpatInquire.md) parameter must be set to[`M_GENERAL`](../../Reference/pat/MpatInquire.md).

---

### `M_RESULT_OUTPUT_UNITS`

Inquires whether to return results in pixels or world units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, it specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. |

---

### `M_SAVE_SUMS`

Inquires whether to save the values of the sums used to compute the normalized correlation result for each model occurrence.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that the sums are not saved. |
| `M_ENABLE` | Specifies that the sums are saved. |

---

### `M_TARGET_CACHING`

Inquires whether the pyramidal representation of the target buffer is kept in the result buffer or generated each time [`MpatFind`](../../Reference/pat/MpatFind.md) is called.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the pyramidal representation of the target buffer is generated each time [`MpatFind`](../../Reference/pat/MpatFind.md) is called. |
| `M_ENABLE` *(default)* | Specifies that the pyramidal representation of the target buffer is kept in the result buffer. |

### Combination Constants — For casting information to the required data type.

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
