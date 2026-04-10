---
doctype: Reference
module: pat
function: MpatControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / pat / MpatControl"
---

# MpatControl

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

> Control a setting of a Pattern Matching context, one or all models, or Pattern Matching result buffer.

## Syntax

```c
void MpatControl(
    AIL_ID     ContextOrResultPatId,  //out
    AIL_INT    Index,                 //in
    AIL_INT64  ControlType,           //in
    AIL_DOUBLE ControlValue           //in
)
```

## Description

This function allows you to control a setting of a Pattern Matching context, at least one (or all) of the models contained therein, or a Pattern Matching result buffer. These settings control the execution of [`MpatFind`](../../Reference/pat/MpatFind.md) operations. Most of the control type settings can be inquired using [`MpatInquire`](../../Reference/pat/MpatInquire.md).

Changing certain control type settings might require preprocessing of the Pattern Matching context again, using [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md). To know if the Pattern Matching context needs to be preprocessed, call [`MpatInquire`](../../Reference/pat/MpatInquire.md) with [`M_PREPROCESSED`](../../Reference/mod/MmodInquire.md), after adjusting all required control types.

Any value that you provide to the Aurora Imaging Library Pattern Matching module (for example, position and width) must be specified in the pixel coordinate system, even if the target image is calibrated.

You can change the search region at any time without preprocesing the Pattern Matching context again (for example, when tracking an object, the search region could be changed before each call to [`MpatFind`](../../Reference/pat/MpatFind.md)).

## Parameters

### `ContextOrResultPatId` *(out, AIL_ID)*

Specifies the Pattern Matching context or Pattern Matching result buffer whose settings you want to modify. The Pattern Matching context or result buffer must have been previously allocated on the system using [`MpatAlloc`](../../Reference/pat/MpatAlloc.md) or [`MpatAllocResult`](../../Reference/pat/MpatAllocResult.md), respectively.

### `Index` *(in, AIL_INT)*

Specifies to control a Pattern Matching context, an individual model, or a Pattern Matching result buffer. Set this parameter to one of the following values:

*For specifying a context, or a model index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default.

If a Pattern Matching context is specified, same as [`M_ALL`](../../Reference/pat/MpatControl.md).

If a Pattern Matching result buffer is specified, same as [`M_GENERAL`](../../Reference/pat/MpatControl.md). |
| `M_ALL` | Applies the specified control setting to all models, if a Pattern Matching context is specified. |
| `M_CONTEXT` | Controls a general setting of a specified Pattern Matching context, if one is specified. |
| `M_GENERAL` | Controls a general setting of a Pattern Matching result buffer when [`ContextOrResultPatId`](../../Reference/pat/MpatControl.md) is a result buffer. |
| `Value >= 0` | Specifies the index of the individual model to control, if a Pattern Matching context is specified. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For the general settings of the Pattern Matching context

The following control type and control value allows you specify general settings for a Pattern Matching context. For controlling general context settings, the [`Index`](../../Reference/pat/MpatControl.md) parameter must be set to [`M_CONTEXT`](../../Reference/pat/MpatControl.md).

---

### `M_SEARCH_MODE`

Sets the search mode to use when performing an [`MpatFind`](../../Reference/pat/MpatFind.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIND_ALL_MODELS` *(default)* | Specifies to search for all the specified models in the target image using their own search settings. Note that, all models must use the same search region. Each model's search settings also determine the maximum number of occurrences of that model to search for in the target image ([`M_NUMBER`](../../Reference/pat/MpatControl.md)). |
| `M_FIND_BEST_MODELS` | Specifies to search for the best occurrences of any model in the target image, using the search settings from the first model in the Pattern Matching context. Consequently, the search settings of the other models are ignored.  Note that, all models must be of the same size.  The number of different matches that are found depends on [`M_NUMBER`](../../Reference/pat/MpatControl.md), [`M_CERTAINTY`](../../Reference/pat/MpatControl.md), and[`M_ACCEPTANCE`](../../Reference/pat/MpatControl.md), but cannot exceed 100 occurrences, unless [`M_MAX_INITIAL_PEAKS`](../../Reference/pat/MpatControl.md) is set to [`M_ALL`](../../Reference/pat/MpatControl.md).  You cannot use [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md) to perform an angular search ([`M_SEARCH_ANGLE`](../../Reference/pat/MpatControl.md)).  > **Note:** Note that, if the first model in the Pattern Matching context is to be ignored (that is, the number of occurrences for that model is set to 0), the settings of the first model whose number of occurrences is not set to 0 will be used instead. |

### For an individual model or all models in a Pattern Matching context

The following control types and control values allow you to control the settings for an individual model or all models in a Pattern Matching context. For controlling the settings of one or all models, the [`Index`](../../Reference/pat/MpatControl.md) parameter must be set to the model index of a specific model in the Pattern Matching context or to [`M_ALL`](../../Reference/pat/MpatControl.md), respectively.

---

### `M_ACCEPTANCE`

Specifies the acceptance level. If the correlation (match score) between the target image and the model is less than this level, it is not considered a match (an occurrence).  A perfect match is 100%, whereas no correlation is 0%. The match score depends on the image quality. You should experiment to decide what is a typical match score for your application.  Note that the acceptance level must be less than or equal to the certainty level ([`M_CERTAINTY`](../../Reference/pat/MpatControl.md)).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptance level, as a percentage. |

---

### `M_ACCURACY`

Specifies the required positional accuracy. You can enhance speed performance by selecting a lower positional accuracy.  Note that the search speed slightly affects the positional accuracy. To change the search speed, use [`M_SPEED`](../../Reference/pat/MpatControl.md).

| Value | Description |
| --- | --- |
| `M_HIGH` | Specifies a high accuracy (typically ± 0.05 pixels). |
| `M_LOW` | Specifies a low accuracy (typically ± 0.20 pixels). Note that when using this setting, the match scores can be slightly lower than usual. If a precise match score is important to you, use at least medium accuracy. |
| `M_MEDIUM` *(default)* | Specifies a medium accuracy (typically ± 0.10 pixels). |

---

### `M_CERTAINTY`

Specifies the certainty level. If the correlation (match score) between the target image and the model is greater than or equal to the certainty level, a match is assumed without looking elsewhere in the image for a better match.  If you set the certainty level too high (close to 100%), you slow down the search because you force the search algorithm to check the whole search region for the best match. A good certainty level is slightly lower than the expected score, so that the search can finish as soon as the match is found. However, if you set the certainty level too low, false matches might be found. Note that the certainty level must be greater than or equal to the acceptance level ([`M_ACCEPTANCE`](../../Reference/pat/MpatControl.md)).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the certainty level, as a percentage. |

---

### `M_NUMBER`

Specifies the maximum expected number of occurrences of a model in a target image. This number is used when searching, using [`MpatFind`](../../Reference/pat/MpatFind.md).  Occurrences with match scores that are greater than or equal to the certainty level (set with [`M_CERTAINTY`](../../Reference/pat/MpatControl.md))) are returned, up to the number specified by [`M_NUMBER`](../../Reference/pat/MpatControl.md). If such occurrences are fewer than the specified number, the remaining occurrences returned are the best of those that are greater than or equal to the acceptance level (set with [`M_ACCEPTANCE`](../../Reference/pat/MpatControl.md)).  Note that, if a model in the Pattern Matching context is to be ignored, set the number of occurrences for that model ([`M_NUMBER`](../../Reference/pat/MpatControl.md)) to 0.

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to look for all matches that are greater than or equal to the acceptance level. |
| `Value >= 0` *(default)* | Specifies the maximum number of occurrences for which to look in the target image. Only integer values are accepted. |

---

### `M_REFERENCE_X`

Sets the X-offset of the model's reference position, relative to the model origin. Position results return the X-coordinate of the model's reference position transformed at the model occurrence. Position results are relative to the target origin.  If you redefine the model's reference position (with [`M_REFERENCE_...`](../../Reference/pat/MpatControl.md)), make sure that the search region (defined by[`M_SEARCH_OFFSET_...`](../../Reference/pat/MpatControl.md) and [`M_SEARCH_SIZE_...`](../../Reference/pat/MpatControl.md) or the target image's ROI) covers this new reference position and takes into account the angular search range of the model (specified using [`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md)).  > **Note:** Note that although the reference position need not be in the model, it should be inside the search region. If, in the target image, the reference position falls outside target image, Aurora Imaging Library returns a negative position.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the X-offset of the model's reference position relative to the model origin, in pixels. |

---

### `M_REFERENCE_Y`

Sets the Y-offset of the model's reference position, relative to the model origin. Position results return the Y-coordinate of the model's reference position transformed at the model occurrence. Position results are relative to the target origin.  If you redefine the model's reference position (with [`M_REFERENCE_...`](../../Reference/pat/MpatControl.md)), make sure that the search region (defined by the [`M_SEARCH_OFFSET_...`](../../Reference/pat/MpatControl.md) and [`M_SEARCH_SIZE_...`](../../Reference/pat/MpatControl.md) or the target image's ROI) covers this new reference position and takes into account the angular search range of the model (specified using [`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md)).  > **Note:** Note that although the reference position need not be in the model, it should be inside the search region. If, in the target image, the reference position falls outside target image, Aurora Imaging Library returns a negative position.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the Y-offset of the model's reference position relative to the model origin, in pixels. |

---

### `M_ROTATED_MODEL_MINIMUM_SCORE`

Sets whether to use angle refinement mode, as well as sets the score to use for this mode. During preprocessing, this control type helps establish the full range of degrees within which a rotated version of the model can be rotated from the model at a specific angle and still match with at least the score specified by this control type (the angle refinement score). The higher the angle refinement score is set, the smaller the angle refinement step will be.  The lowest value between the determined angle refinement step and the specified angle accuracy (set using [`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatControl.md)) is the refinement angle used when searching for the pattern in the target image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the angle refinement mode. |
| `0.0 <= Value <= 100.0` | Specifies the minimum score. |

---

### `M_SEARCH_ANGLE`

Sets the nominal search angle at which to search for occurrences of the model.  For best results, if there is a region of interest (ROI) in your target image, set this control's value to 0.0. Note that there will be an increase in processing time if the model is at an angle.  Angles are interpreted with respect to the pixel coordinate system, so they are always measured counter-clockwise.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the nominal search angle, in degrees. |

---

### `M_SEARCH_ANGLE_ACCURACY`

Sets the required precision with which to find the angle of the occurrence. The precision defines an angular range in which to fine-tune the search after the approximate location of the occurrence of the model is found. To be effective, you must set the degree of accuracy to a value smaller than that of the rotation tolerance ([`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/pat/MpatControl.md)). This angular range is further restricted by [`M_ROTATED_MODEL_MINIMUM_SCORE`](../../Reference/pat/MpatControl.md). For more information, refer to [Search constraints](../../UserGuide/C07_Pattern_matching/S07_Search_constraints.md).  The lower the accuracy, the higher the precision of the occurrence angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle accuracy equals the tolerance. |
| `0.0 < Value <= 180.0` | Specifies the angle accuracy, in degrees. |

---

### `M_SEARCH_ANGLE_DELTA_NEG`

Sets the lower limit for the angular range, relative to the nominal search angle (set using [`M_SEARCH_ANGLE`](../../Reference/pat/MpatControl.md)). The angular range should be used to cover the expected variance in angle. Occurrences with angles outside the angular-range, +/- a tolerance, cannot be returned as results. If [`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatControl.md) is set to [`M_DISABLE`](../../Reference/pat/MpatControl.md), the tolerance is equal to [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/pat/MpatControl.md). Otherwise, the tolerance is equal to 1.5 * [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/pat/MpatControl.md).  Note that the greater the range of angles to be searched, the slower the search.  To set the upper limit for the angular range, use [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/pat/MpatControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default value is 180.0 when the model is of type [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md), and 0.0 otherwise. |
| `0.0 <= Value <= 180.0` | Specifies the angle in a clockwise rotation, in degrees. |

---

### `M_SEARCH_ANGLE_DELTA_POS`

Sets the upper limit for the angular range, relative to the nominal search angle (set using [`M_SEARCH_ANGLE`](../../Reference/pat/MpatControl.md)). The angular range should be used to cover the expected variance in angle. Occurrences with angles outside the angular-range, +/- a tolerance, cannot be returned as results. If [`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatControl.md) is set to [`M_DISABLE`](../../Reference/pat/MpatControl.md), the tolerance is equal to [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/pat/MpatControl.md). Otherwise, the tolerance is equal to 1.5 * [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/pat/MpatControl.md).  Note that the greater the range of angles to be searched, the slower the search.  To set the lower limit for the angular range, use [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/pat/MpatControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default value is 180.0 when the model is of type [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md), and 0.0 otherwise. |
| `0.0 <= Value <= 180.0` | Specifies the angle in a counter-clockwise rotation, in degrees. |

---

### `M_SEARCH_ANGLE_INTERPOLATION_MODE`

Sets the type of interpolation to use when rotating the model to perform the search at an angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated. |
| `M_BILINEAR` | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` *(default)* | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

---

### `M_SEARCH_ANGLE_MODE`

Sets whether to perform calculations specific to angular-range search strategies.  To search for models at an angle (within their specified angular range [`M_SEARCH_ANGLE`](../../Reference/pat/MpatControl.md)- [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/pat/MpatControl.md) to [`M_SEARCH_ANGLE`](../../Reference/pat/MpatControl.md) + [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/pat/MpatControl.md) in the target) this control type must be enabled. In addition, you can limit the number of angles searched using[`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatControl.md) and [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/pat/MpatControl.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable the angular search mode. |
| `M_ENABLE` | Specifies to enable the angular search mode. |

---

### `M_SEARCH_ANGLE_TOLERANCE`

Sets the full range of degrees within which the pattern in the target image can be rotated from a model at a specific angle and still meet the acceptance level. For example, if a model can tolerate the target image being offset from its search angle by ±2.5°, specify 5°. The specified tolerance determines the step angle. Note that very small tolerance values should only be used within small angular ranges; otherwise, application performance can be adversely affected. For more information, refer to [Finding models when they are at an angle](../../UserGuide/C07_Pattern_matching/S05_Rotation.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies that the tolerance will be automatically determined based on an analysis of the model. |
| `0.0 < Value <= 180.0` *(default)* | Specifies the step angle, in degrees. |

---

### `M_SEARCH_OFFSET_X`

Specifies the X-offset of the specified model's search region. It limits the area in the target image in which to find the reference position of the model (set with [`M_REFERENCE_...`](../../Reference/pat/MpatControl.md)), and consequently increases the search speed.  Note that, if searching for multiple models and your [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md), the offset of the search region must be the same for all the models.  The search region can fall outside the target area, specified when calling [`MpatFind`](../../Reference/pat/MpatFind.md).  When dealing with a target image with an ROI, this value is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the X-coordinate of the top-left corner of the target image. By default, the entire target image is searched. |
| `Value` | Specifies the X-coordinate, relative to the pixel coordinate system, of where to start the search. Only integer values are accepted. |

---

### `M_SEARCH_OFFSET_Y`

Specifies the Y-offset of the specified model's search region. It limits the area in the target image in which to find the reference position of the model (set with [`M_REFERENCE_...`](../../Reference/pat/MpatControl.md)), and consequently increases the search speed.  Note that, if searching for multiple models and your [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md), the offset of the search region must be the same for all the models.  The search region can fall outside the target area, specified when calling [`MpatFind`](../../Reference/pat/MpatFind.md).  When dealing with a target image with an ROI, this value is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the Y-coordinate of the top-left corner of the target image. By default, the entire target image is searched. |
| `Value` | Specifies the Y-coordinate, relative to the pixel coordinate system, of where to start the search. Only integer values are accepted. |

---

### `M_SEARCH_SIZE_X`

Specifies the width of the search region. Note that the size of the search region can be smaller than the size of the model because the search region only defines where to find the reference position.  Note that, if searching for multiple models and your [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md), the width of the search region must be the same for all the models.  The search region can fall outside the target image, specified when calling [`MpatFind`](../../Reference/pat/MpatFind.md).  When dealing with a target image with an ROI, this value is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to use the width of the full image. |
| `Value` | Specifies the width, in pixels. Only integer values are accepted. |

---

### `M_SEARCH_SIZE_Y`

Specifies the height of the search region. Note that the size of the search region can be smaller than the size of the model because the search region only defines where to find the reference position.  Note that, if searching for multiple models and your [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md), the height of the search region must be the same for all the models.  The search region can fall outside the target image, specified when calling [`MpatFind`](../../Reference/pat/MpatFind.md).  When dealing with a target image with an ROI, this value is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to use the height of the full image. |
| `Value` | Specifies the height, in pixels. Only integer values are accepted. |

---

### `M_SPEED`

Specifies the required search speed when using [`MpatFind`](../../Reference/pat/MpatFind.md). At a higher speed, the search takes all reasonable short cuts; therefore, the search should be performed faster than at lower settings. Generally, the high speed setting should be used for better quality images or when using a simple model. Note, the high speed setting reduces positional accuracy very slightly. Try the low speed settings only if your image quality is particularly poor and you have encountered problems at higher speeds.

| Value | Description |
| --- | --- |
| `M_HIGH` | Specifies a high search speed. |
| `M_LOW` | Specifies a low search speed. |
| `M_MEDIUM` *(default)* | Specifies a medium search speed. |
| `M_VERY_HIGH` | Specifies a very high search speed. |
| `M_VERY_LOW` | Specifies a very low search speed. |

### For controlling advanced settings

The following control types and control values are for advanced users of the Pattern Matching module. If the default settings do not satisfy the requirements of your application, these control types and control values can be used to fine tune the model's search settings. The default settings typically provide the best results for most search operations. To gain a clearer understanding of how the search algorithm works, see [Pattern matching algorithm (for advanced users)](../../UserGuide/C07_Pattern_matching/S10_Pattern_matching_algorithm_for_advanced_users.md).  For these control types, the [`Index`](../../Reference/pat/MpatControl.md) parameter must be set to the model index of a specific model in the Pattern Matching context or [`M_ALL`](../../Reference/pat/MpatControl.md).

---

### `M_COARSE_SEARCH_ACCEPTANCE`

Sets the coarse search acceptance threshold level to use for rejecting candidate model peaks at low resolution levels.  Note that this value is not saved with the Pattern Matching context. Instead, this value must be set each time the Pattern Matching context is restored.  Changing the coarse search acceptance to a specified value might increase the search time when some of the matches you request do not reach the certainty level, or when you request more matches than are really present in the image. The coarse search acceptance level should usually be set much lower than the search acceptance level. For example, a good level to set it to is about 20% to 30%. Note that if it is too low, you will not see any increase in speed. However, if it is too high, you risk rejecting real match peaks. Note that the coarse search acceptance threshold level is not saved and restored with the Pattern Matching context. So, if you always want to use your own coarse search acceptance level, you should set it each time after restoring your Pattern Matching context and before calling [`MpatFind`](../../Reference/pat/MpatFind.md). Otherwise, the default coarse search acceptance level is used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to determine the coarse search acceptance threshold level automatically. |
| `1.0 <= Value <= 100.0` | Specifies the coarse search acceptance threshold level. |

---

### `M_EXTRA_CANDIDATES`

Sets the number of extra candidates to consider.  Normally, the search algorithm considers only a limited number of (best) scores as possible candidates to match when proceeding at the most subsampled stage. This setting allows you to add robustness to the algorithm, by considering more candidates, without overly compromising the search speed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of extra candidates. Only integer values are accepted. |

---

### `M_FAST_FIND`

Sets whether to use fast peak finding.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use preprocessing to decide if fast peak finding is appropriate. |
| `M_DISABLE` | Specifies not to use fast peak finding.  The initial search (at the resolution level determined by [`M_FIRST_LEVEL`](../../Reference/pat/MpatControl.md)) computes the correlation at every position in the search region. This guarantees that the biggest match peak will be found, and that it will be investigated first. |
| `M_ENABLE` | Specifies to use fast peak finding.  The search algorithm attempts to find the peaks without checking every point. This is safe in most cases, but can cause matches to be missed for models that produce very narrow peaks. |

---

### `M_FIRST_LEVEL`

Sets the resolution level for the initial stage (lowest resolution) of the search.  Note that level 0 is the original target image and each higher level is half the size (and resolution) of the previous one. If the specified level is not supported by the search algorithm, the highest possible level will be used. A higher first level speeds up the initial search but makes it less reliable, because the model might not retain enough distinctive features at such a low resolution. After preprocessing your Pattern Matching context, use [`MpatInquire`](../../Reference/pat/MpatInquire.md) with [`M_PROC_FIRST_LEVEL`](../../Reference/pat/MpatInquire.md) to inquire about the default value of the lowest resolution level.  > **Note:** Note that if you set a lower value for [`M_FIRST_LEVEL`](../../Reference/pat/MpatControl.md) than the one assigned to [`M_LAST_LEVEL`](../../Reference/pat/MpatControl.md), [`MpatControl`](../../Reference/pat/MpatControl.md) will override your settings and automatically assign [`M_LAST_LEVEL`](../../Reference/pat/MpatControl.md) the same value as [`M_FIRST_LEVEL`](../../Reference/pat/MpatControl.md). This implies that only one resolution level will be used for the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_CONTENT_BASED` | Specifies to automatically determine the first level based on an analysis of the model contents. |
| `M_AUTO_SIZE_BASED` *(default)* | Specifies to automatically determine the first level based on the model size. |
| `0 <= Value <= MaxLevel` | Specifies the first level, where MaxLevel is the maximum possible level. After preprocessing your Pattern Matching context, use [`MpatInquire`](../../Reference/pat/MpatInquire.md) with [`M_MODEL_MAX_LEVEL`](../../Reference/pat/MpatInquire.md) to establish the maximum possible level. Only integer values are accepted. |

---

### `M_LAST_LEVEL`

Sets the resolution level for the final stage (highest resolution) of the search.  Note that if the specified level is not supported by the search algorithm, the highest acceptable level will be used. Search score can also be less reliable for levels above 0, depending on the characteristics of the model. After preprocessing your Pattern Matching context, use [`MpatInquire`](../../Reference/pat/MpatInquire.md) with [`M_PROC_LAST_LEVEL`](../../Reference/pat/MpatInquire.md) to inquire about the default value of the highest resolution level.  > **Note:** Note that if you set a higher value for [`M_LAST_LEVEL`](../../Reference/pat/MpatControl.md) than the one assigned to [`M_FIRST_LEVEL`](../../Reference/pat/MpatControl.md), [`MpatControl`](../../Reference/pat/MpatControl.md) will override your settings and automatically assign [`M_LAST_LEVEL`](../../Reference/pat/MpatControl.md) the same value as [`M_FIRST_LEVEL`](../../Reference/pat/MpatControl.md). This implies that only one resolution level will be used for the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the last level automatically. |
| `0 <= Value <= MaxLevel` | Specifies the last level, where MaxLevel is the maximum possible level. After preprocessing your Pattern Matching context, use [`MpatInquire`](../../Reference/pat/MpatInquire.md) with [`M_MODEL_MAX_LEVEL`](../../Reference/pat/MpatInquire.md) to establish the maximum possible level. Only integer values are accepted. |

---

### `M_MAX_INITIAL_PEAKS`

Sets the maximum number of returned peaks after an initial search at the first level for occurrences of multiple models, or a single model with circular overscan data (that is, [`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md) or [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md) with [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to limit the returned number of peaks for optimal performance. |
| `M_ALL` | Specifies all occurrences that pass the model acceptance threshold should be returned. Performance might be reduced.  It is recommended to use this setting when the target image has many potential false positive occurrences. |

---

### `M_MIN_SEPARATION_X`

Sets the minimum separation (in X) between two occurrences of the same model in order for them to be considered distinct.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Sets the minimum separation to 75% of the model width ([`Param3`](../../Reference/pat/MpatDefine.md) of [`MpatDefine`](../../Reference/pat/MpatDefine.md)) when creating an [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md). |
| `Value` | Specifies the minimum separation as a percentage of the model's width. This value can be greater than 100%. |

---

### `M_MIN_SEPARATION_Y`

Sets the minimum separation (in Y) between two occurrences of the same model in order for them to be considered distinct.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Sets the minimum separation to 75% of the model height ( [`Param4`](../../Reference/pat/MpatDefine.md) of [`MpatDefine`](../../Reference/pat/MpatDefine.md)) when creating an [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md)). |
| `Value` | Specifies the minimum separation as a percentage of the model's height. This value can be greater than 100%. |

---

### `M_MODEL_STEP`

Sets whether all or every second pixel in the model is used in the correlation during the high resolution stage of the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to automatically establish the model step. |
| `1` | Specifies to use all model pixels. |
| `2` | Specifies to use every second model pixel (in both the X and Y directions).  This speeds up the last (high resolution) stage of the search, particularly for large models. The match score can be affected if the model has many fine features, but will tend not to be affected if the model has mainly coarse features. |

---

### `M_SEARCH_ANGLE_ACCURACY_MODE`

Sets the method for scanning the specified angular range for multiple occurrences, or a single occurrence with circular overscan data (that is, [`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md) or [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md) with[`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to return the closest occurrence to the approximate location within the specified angular range, for optimal performance. |
| `M_ALL` | Specifies that all possible occurrences that are found within the range of the accuracy specified with [`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatControl.md) will be scanned, and the best will be returned. Performance might be reduced.  It is recommended to use this setting when the target image has many potential false positive occurrences. |

### For the result buffer identifier

The following control types and control values settings can be specified for a Pattern Matching result buffer. For controlling the settings of a Pattern Matching result buffer, the [`Index`](../../Reference/pat/MpatControl.md) parameter must be set to[`M_GENERAL`](../../Reference/pat/MpatControl.md).

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, it specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MpatGetResult`](../../Reference/pat/MpatGetResult.md) generates an error if the result was not calculated on a calibrated image. |

---

### `M_SAVE_SUMS`

Sets whether to save the values of the sums used to compute the normalized correlation result for each model occurrence.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that the sums are not saved. |
| `M_ENABLE` | Specifies that the sums are saved. |

---

### `M_TARGET_CACHING`

Sets whether the pyramidal representation of the target buffer is kept in the result buffer or generated each time [`MpatFind`](../../Reference/pat/MpatFind.md) is called.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the pyramidal representation of the target buffer is generated each time [`MpatFind`](../../Reference/pat/MpatFind.md) is called. |
| `M_ENABLE` *(default)* | Specifies that the pyramidal representation of the target buffer is kept in the result buffer. Note that the pyramidal representation is generated upon the first call to [`MpatFind`](../../Reference/pat/MpatFind.md).  This pyramidal representation is re-used by consecutive calls to [`MpatFind`](../../Reference/pat/MpatFind.md) as long as the same result buffer is used and the image, search region, and model size are not modified. |

[`MpatFind`](../../Reference/pat/MpatFind.md) does not support angular searches when [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md).

When[`M_SEARCH_ANGLE_MODE`](../../Reference/pat/MpatControl.md) is disabled, this setting has no effect.
