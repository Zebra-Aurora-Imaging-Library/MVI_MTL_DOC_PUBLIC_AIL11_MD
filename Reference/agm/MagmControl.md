---
doctype: Reference
module: agm
function: MagmControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmControl"
---

# MagmControl

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

> Control a model in an AGM context or a train AGM result buffer setting.

## Syntax

```c
void MagmControl(
    AIL_ID     ContextOrResultId,  //out
    AIL_INT64  Index,              //in
    AIL_INT64  ControlType,        //in
    AIL_DOUBLE ControlValue        //in
)
```

## Description

This function allows you to control a setting of a single-definition or composite-definition model in a find AGM context, of a composite-definition model in a train AGM context, or of a train AGM result buffer. These settings control the execution of [`MagmFind`](../../Reference/agm/MagmFind.md) or [`MagmTrain`](../../Reference/agm/MagmTrain.md) operations. Most of the control type settings can be inquired using [`MagmInquire`](../../Reference/agm/MagmInquire.md).

## Parameters

### `ContextOrResultId` *(out, AIL_ID)*

Specifies the identifier of a find or train AGM context or train AGM result buffer to control.

### `Index` *(in, AIL_INT64)*

Specifies what to control. Set this parameter to one of the following values:

*For specifying a model or result buffer*

| Value | Description |
| --- | --- |
| `M_AGM_MODEL_INDEX` | Specifies to control an individual model in an AGM context, if one is specified. |
| `M_GENERAL` | Specifies to control a general setting of an AGM result buffer, if one is specified. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For controlling a model in an AGM context

The following [`ContextOrResultId`](../../Reference/agm/MagmControl.md), [`ControlType`](../../Reference/agm/MagmControl.md), and [`ControlValue`](../../Reference/agm/MagmControl.md) parameter settings can be specified for models in AGM contexts. In this case, you must set the [`Index`](../../Reference/agm/MagmControl.md) parameter to the index of a specific model, using **M_AGM_MODEL_INDEX()**.

---

### `Find AGM context ID with a composite-definition model`

Specifies a find AGM context, allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md) and used in [`MagmFind`](../../Reference/agm/MagmFind.md) operations. It must contain a trained composite-definition model.

#### `M_ACCEPTANCE_COVERAGE`

Sets the minimum coverage score required for an occurrence to be considered a match.  The coverage score is the percentage of the total length of the model's edgels found in the occurrence. 100% indicates that for each of the model's edgels, a corresponding edgel was found in the occurrence.  Note, a model's edgel corresponds to an edgel in the occurrence if the distance between them is less than or equal to [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum acceptable coverage score, as a percentage. |

#### `M_ACCEPTANCE_DETECTION`

Sets the minimum detection score required for an occurrence to be considered a match.  The detection score represents how confident you should be that Aurora Imaging Library found a true occurrence.  Note, this value is only used if [`M_ACCEPTANCE_DETECTION_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum acceptable detection score, as a percentage. |

#### `M_ACCEPTANCE_DETECTION_SOURCE`

Sets whether to use the trained acceptance level or a custom acceptance level for the detection score. You can typically start with the acceptance level established during training and specify a higher acceptance level if the search speed is too slow, or if the search finds false occurrences. You should specify a lower acceptance level if the search does not find all model occurrences.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FROM_TRAIN` *(default)* | Specifies to use the detection score's acceptance level established during the training of the model. |
| `M_USER_DEFINED` | Specifies to use the value set with [`M_ACCEPTANCE_DETECTION`](../../Reference/agm/MagmControl.md). |

#### `M_ACCEPTANCE_FIT`

Sets the minimum fit score required for an occurrence to be considered a match.  The fit score is a measure of the correlation of the edgels in the model to their corresponding edgels in the occurrence. It is based on the distance between the model's edgels and the occurrence's edgels.  Note, only those model edgels that are less than or equal to [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/agm/MagmControl.md) from an occurrence edgel are used in the fit score calculation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum acceptable fit score, as a percentage. |

#### `M_ACCEPTANCE_FIT_OVERALL`

Sets the minimum overall fit score required for an occurrence to be considered a match.  The overall fit score is a measure of the correlation of all edgels in the model to those of the occurrence. It is based on the distance between the model's edgels and the occurrence's edgels.  Note, all model edgels are used in the overall fit score calculation, including those without a corresponding edgel in the occurrence.  This control type is only available when [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum acceptable overall fit score, as a percentage. |

#### `M_DETECTION_DELTA_ANGLE_SOURCE`

Sets whether to use the trained detection angle range or a custom detection angle range to limit the angle of detection.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FROM_TRAIN` *(default)* | Specifies to use the detection angle range established during the training of the model. If [`M_TRAIN_DETECTION_ANGLE`](../../Reference/agm/MagmControl.md) was set to [`M_ENABLE`](../../Reference/agm/MagmControl.md), the detection angle range is 360.0°. If [`M_TRAIN_DETECTION_ANGLE`](../../Reference/agm/MagmControl.md) was set to [`M_DISABLE`](../../Reference/agm/MagmControl.md), the detection angle range is 0.0°. |
| `M_USER_DEFINED` | Specifies to use the values set with [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md) and [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md). |

#### `M_DETECTION_DELTA_NEG_ANGLE`

Sets the lower limit of the detection angle range, relative to the nominal search angle ([`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md)). That is, _LowerLimit_ = [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md)  - [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md). The detection angle range should be used to cover the expected variance in the angle of detection.  Note that the detection angle range only limits the angle of detection. The angle at which an occurrence is detected might not be exact; its true angle can typically be within 10° of the angle of detection, even if that falls outside of the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is disabled, the returned angle of an occurrence will always be the angle at which it was detected and will therefore always be within the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is enabled, a fit operation is performed starting from the angle of detection. In refining the angle so that it is more accurate, the angle of the occurrence's reference axis returned by [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_ANGLE`](../../Reference/agm/MagmGetResult.md) can end up being outside of the detection angle range.  Note that the greater the range of angles to be searched, the slower the search.  To set the upper limit for the detection angle range, use [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md).  This control type is only used when [`M_DETECTION_DELTA_ANGLE_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the lower limit of the detection angle range, in degrees. Note that [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md) + [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md) cannot be greater than 360.0°. |

#### `M_DETECTION_DELTA_POS_ANGLE`

Sets the upper limit of the detection angle range, relative to the nominal search angle ([`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md)). That is, _UpperLimit_ = [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md)  + [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md). The detection angle range should be used to cover the expected variance in the angle of detection.  Note that the detection angle range only limits the angle of detection. The angle at which an occurrence is detected might not be exact; its true angle can typically be within 10° of the angle of detection, even if that falls outside of the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is disabled, the returned angle of an occurrence will always be the angle at which it was detected and will therefore always be within the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is enabled, a fit operation is performed starting from the angle of detection. In refining the angle so that it is more accurate, the angle of the occurrence's reference axis returned by [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_ANGLE`](../../Reference/agm/MagmGetResult.md) can end up being outside of the detection angle range.  Note that the greater the range of angles to be searched, the slower the search.  To set the lower limit for the detection angle range, use [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md).  This control type is only used when [`M_DETECTION_DELTA_ANGLE_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the upper limit of the detection angle range, in degrees. Note that [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md) + [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md) cannot be greater than 360.0°. |

#### `M_DETECTION_MULTI_ANGLE_MODE`

Sets the mode with which to detect occurrences when searching at multiple angles.  The [`M_GLOBAL`](../../Reference/agm/MagmControl.md) mode can accelerate search speed, but can also increase the risk of missing overlapping occurrences that have close centers. You should specify [`M_INDIVIDUAL`](../../Reference/agm/MagmControl.md) if the search is not finding all model occurrences, but note that this will slow down the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GLOBAL` *(default)* | Specifies to detect occurrences across all search angles globally. |
| `M_INDIVIDUAL` | Specifies to detect occurrences at each search angle individually. |

#### `M_DETECTION_REF_ANGLE`

Sets the nominal search angle; this is the angle at which you expect to find the model's reference axis in the target. Note that occurrences can be slightly rotated from the search angle and still be detected. The exact degree varies depending on your model, but the [`MagmFind`](../../Reference/agm/MagmFind.md) operation will typically detect occurrences within 10° of the search angle. If you expect greater degrees in variation, you should specify a detection angle range, using [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md) and [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md).  Note that specifying a nominal search angle does not decrease the search speed.  This control type is only used when [`M_DETECTION_DELTA_ANGLE_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 360.0` *(default)* | Specifies the nominal search angle, in degrees. |

#### `M_DETECTION_STEP_ANGLE`

Sets the step angle at which to search for occurrences of the model within the specified detection angle range. Note that the smaller the step angle, the slower the search. The search is performed at the nominal search angle and at every +/- [`M_DETECTION_STEP_ANGLE`](../../Reference/agm/MagmControl.md) degrees within the detection angle range. Note that occurrences can be slightly rotated from the search angle and still be detected. You should typically set the step angle to this rotation * 2. For example, if your model can tolerate the occurrence being offset from its search angle by ±5°, you should set the step angle to 10°.  This control type is only used when [`M_DETECTION_DELTA_ANGLE_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `5.0 <= Value <= 180.0` *(default)* | Specifies the step angle, in degrees. |

#### `M_FIT_MODE`

Sets the mode of the fit operation. The fit mode affects positional accuracy and search speed. The positional results are most accurate when the fit operation uses subpixel precision, but this can reduce the speed of the search.  Note that the fit can only be calculated with subpixel precision when you have explicitly specified an image of the model to use for the fit calculations ([`M_MODEL_IMAGE`](../../Reference/agm/MagmControl.md)) and the edges of the target images are extracted ([`M_USE_MAGNITUDE_TARGET`](../../Reference/agm/MagmControl.md) is disabled).  You can disable the fit operation to speed up the search, in which case Aurora Imaging Library uses the features of the model and target to find occurrences, but it doesn't do any fitting. This results in a lower accuracy because the positional results are simply the positions of detection.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the fit operation; the positional results are determined by detection only. |
| `M_PIXEL_PRECISION` *(default)* | Specifies to calculate the fit with pixel precision. |
| `M_SUBPIXEL_PRECISION` | Specifies to calculate the fit with subpixel precision.  This value is only available when [`M_USE_MAGNITUDE_TARGET`](../../Reference/agm/MagmControl.md) is set to [`M_DISABLE`](../../Reference/agm/MagmControl.md) and [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md). |

#### `M_MAX_ACCEPTED_OVERLAP`

Sets the maximum allowable overlap between found occurrences, as a percentage.  Note that [`M_MAX_ACCEPTED_OVERLAP`](../../Reference/agm/MagmControl.md) is applied after possible occurrences are sorted.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the acceptable overlap, as a percentage. If set to 100%, found occurrences can overlap completely. If set to 0%, no found occurrence can overlap another by any amount. |

#### `M_MAX_ASSOCIATION_DISTANCE`

Sets the maximum distance between a model edgel and the nearest occurrence edgel. Model edgels falling outside the specified maximum distance are not used to perform the occurrence's regular fit operation, but will decrease the overall fit score and the coverage score.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1.0 <= Value <= 4.0` *(default)* | Specifies the maximum association distance, in pixels. |

#### `M_MODEL_IMAGE`

Sets the image buffer that contains the image of the model to use for the fit and coverage calculations. The model image should have a prototypical appearance with well-defined edges. You can prepare the image manually or define a child buffer within a training image. You must specify a model image when [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md). Specifying a prototypical image of the model allows for higher fit and coverage scores when model appearances are varied and the trained composite-definition model contains more edgels than a typical appearance.

| Value | Description |
| --- | --- |
| `ModelImageId` | Specifies the identifier of the image buffer containing the model image. Note that this buffer must be a 1-band, 8-bit unsigned image buffer.  This image buffer must be the same size as the trained composite-definition model. You can inquire the size of the trained composite-definition model, using [`MagmInquire`](../../Reference/agm/MagmInquire.md) with [`M_SIZE_X`](../../Reference/agm/MagmInquire.md) and [`M_SIZE_Y`](../../Reference/agm/MagmInquire.md). |

#### `M_MODEL_SOURCE`

Sets whether to use the trained composite-definition model or a prototypical image of the model for the fit and coverage calculations. You should use the trained composite-definition model if the labeled model appearances in your training images are uniform. When model appearances are varied, the trained composite-definition model contains additional edges. You can view the edges of the trained composite-definition model using [`MagmDraw`](../../Reference/agm/MagmDraw.md) with [`M_DRAW_TRAINED_MODEL`](../../Reference/agm/MagmDraw.md). If the trained composite-definition model contains a lot of edges, you should choose a model image for the fit and coverage.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FROM_TRAIN` *(default)* | Specifies to use the trained composite-definition model. |
| `M_USER_IMAGE` | Specifies to use the image set with [`M_MODEL_IMAGE`](../../Reference/agm/MagmControl.md). |

#### `M_PERSEVERANCE_DETECTION`

Sets the algorithm's perseverance when searching for occurrences. This affects the number of candidates the algorithm attempts to validate. Increasing the perseverance will increase robustness, but will increase search time.  You should try increasing the perseverance if the search is not finding all model occurrences, or if the target is complex and decreasing [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) is insufficient.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the perseverance, as a percentage. |

#### `M_REFERENCE_X`

Sets the X-coordinate of the origin of the model's reference axis, relative to the model origin. Position results return the X-coordinate of the model's reference axis origin transformed at the model occurrence. Position results are relative to the target origin.  Note that the reference axis origin need not be in the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default X-coordinate will be used.  For a composite-definition model, the default value is the center of the model. |
| `Value` | Specifies the X-coordinate, in pixels. This value can be specified with subpixel accuracy. |

#### `M_REFERENCE_Y`

Sets the Y-coordinate of the origin of the model's reference axis, relative to the model origin. Position results return the Y-coordinate of the model's reference axis origin transformed at the model occurrence. Position results are relative to the target origin.  Note that the reference axis origin need not be in the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default Y-coordinate will be used.  For a composite-definition model, the default value is the center of the model. |
| `Value` | Specifies the Y-coordinate, in pixels. This value can be specified with subpixel accuracy. |

#### `M_SMOOTHNESS`

Sets the degree of smoothness (noise reduction) applied to the model image and target image(s) during the edge extraction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value applied to the images, as a percentage.  A setting of 0.0 indicates almost no noise reduction effect, while a setting of 100.0 indicates a very strong noise reduction effect. |

#### `M_SMOOTHNESS_SOURCE`

Sets whether to use the smoothness value set during training or a custom smoothness value for the degree of noise reduction applied to the model image and target image(s) during the edge extraction. You should customize the smoothness value if your training and target images have dissimilar noise levels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FROM_TRAIN` *(default)* | Specifies to use the smoothness value set during the training of the model.  This value was set with [`M_SMOOTHNESS`](../../Reference/agm/MagmControl.md) in a train AGM context. |
| `M_USER_DEFINED` | Specifies to use the smoothness value set with [`M_SMOOTHNESS`](../../Reference/agm/MagmControl.md). |

#### `M_SORT_CANDIDATES_SCORE`

Sets the score used to sort candidates. Candidates are sorted in decreasing score order.  The specified score affects which possible occurrences are filtered out. When two possible occurrences overlap above the value specified with [`M_MAX_ACCEPTED_OVERLAP`](../../Reference/agm/MagmControl.md), the possible occurrence with the lower score is removed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SCORE_COVERAGE` | Specifies to use the coverage score to sort candidates. |
| `M_SCORE_DETECTION` *(default)* | Specifies to use the detection score to sort candidates. |
| `M_SCORE_FIT` | Specifies to use the fit score to sort candidates. |
| `M_SCORE_FIT_OVERALL` | Specifies to use the overall fit score to sort candidates.  This value is only available when [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md). |

#### `M_STABLE_EDGELS_MODE`

Sets whether to use only stable edgels when searching for occurrences. A stable edgel is an edgel that remains persistent when extracting edges with slightly different smoothness values.  This control type is only used and should only be enabled when [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md). Otherwise, an error is generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to use only stable edgels. All edgels are considered when searching for occurrences. |
| `M_SMOOTHNESS_STABLE` | Specifies to use only stable edgels. Unstable (noisy) edgels are ignored when searching for occurrences. |

#### `M_STEP_X`

Sets the X step size of explored positions to search for occurrences. Increasing [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) can accelerate search speed, but can also increase the risk of missing favorable occurrences.  You can typically start with a value of 1 and increase [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) if the speed is too slow. Increasing these values when you have a large model can decrease the search time while minimally affecting the occurrences' detection, fit, and coverage scores.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the X step size. |

#### `M_STEP_Y`

Sets the Y step size of explored positions to search for occurrences. Increasing [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) can accelerate search speed, but can also increase the risk of missing favorable occurrences.  You can typically start with a value of 1 and increase [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) if the speed is too slow. Increasing these values when you have a large model can decrease the search time while minimally affecting the occurrences' detection, fit, and coverage scores.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the Y step size. |

#### `M_THRESHOLD_MODE`

Sets the threshold mode of the edge extraction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies no threshold. All edges are extracted. |
| `M_HIGH` *(default)* | Specifies a high threshold. [`M_HIGH`](../../Reference/agm/MagmControl.md) should be used for images with some contrast variations, noise, and non-uniform illumination. [`M_HIGH`](../../Reference/agm/MagmControl.md) always results in a lower (or equal) number of edgels than [`M_MEDIUM`](../../Reference/agm/MagmControl.md). |
| `M_LOW` | Specifies a low threshold. All edges over a minimum noise-based estimated threshold are extracted. [`M_LOW`](../../Reference/agm/MagmControl.md) always results in a lower (or equal) number of edgels than [`M_DISABLE`](../../Reference/agm/MagmControl.md). |
| `M_MEDIUM` | Specifies a medium threshold. [`M_MEDIUM`](../../Reference/agm/MagmControl.md) should be used for multi-contrast images, or for images with a lot of noise or non-uniform illumination. [`M_MEDIUM`](../../Reference/agm/MagmControl.md) always results in a lower (or equal) number of edgels than [`M_LOW`](../../Reference/agm/MagmControl.md). |
| `M_VERY_HIGH` | Specifies a very high threshold. Only the strongest edges in the image are extracted. [`M_VERY_HIGH`](../../Reference/agm/MagmControl.md) always results in a lower (or equal) number of edgels than [`M_HIGH`](../../Reference/agm/MagmControl.md). |

#### `M_THRESHOLD_MODE_SOURCE`

Sets whether to use the threshold mode set during training or a custom threshold mode for the edge extraction. You can use the threshold mode set during training if your training and target images have similar illumination and contrast. If your training and target images were captured in different conditions, the illumination and contrast might vary and you should customize the threshold mode accordingly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FROM_TRAIN` *(default)* | Specifies to use the threshold mode set during the training of the model.  This value was set with [`M_THRESHOLD_MODE`](../../Reference/agm/MagmControl.md) in a train AGM context. |
| `M_USER_DEFINED` | Specifies to use the threshold mode set with [`M_THRESHOLD_MODE`](../../Reference/agm/MagmControl.md). |

---

### `Find AGM context ID with a single-definition model`

Specifies a find AGM context, allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md) and used in [`MagmFind`](../../Reference/agm/MagmFind.md) operations. It must contain a single-definition model.

#### `M_ACCEPTANCE_COVERAGE`

Sets the minimum coverage score required for an occurrence to be considered a match.  The coverage score is the percentage of the total length of the model's edgels found in the occurrence. 100% indicates that for each of the model's edgels, a corresponding edgel was found in the occurrence.  Note, a model's edgel corresponds to an edgel in the occurrence if the distance between them is less than or equal to [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum acceptable coverage score, as a percentage. |

#### `M_ACCEPTANCE_DETECTION`

Sets the minimum detection score required for an occurrence to be considered a match.  The detection score represents how confident you should be that Aurora Imaging Library found a true occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_DETECTION`](Reference/agm/MagmControl.md))* |  |

#### `M_ACCEPTANCE_FIT`

Sets the minimum fit score required for an occurrence to be considered a match.  The fit score is a measure of the correlation of the edgels in the model to their corresponding edgels in the occurrence. It is based on the distance between the model's edgels and the occurrence's edgels.  Note, only those model edgels that are less than or equal to [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/agm/MagmControl.md) from an occurrence edgel are used in the fit score calculation.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_FIT`](Reference/agm/MagmControl.md))* |  |

#### `M_ACCEPTANCE_FIT_OVERALL`

Sets the minimum overall fit score required for an occurrence to be considered a match.  The overall fit score is a measure of the correlation of all edgels in the model to those of the occurrence. It is based on the distance between the model's edgels and the occurrence's edgels.  Note, all model edgels are used in the overall fit score calculation, including those without a corresponding edgel in the occurrence.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_FIT_OVERALL`](Reference/agm/MagmControl.md))* |  |

#### `M_BOX_MARGIN_BOTTOM`

Sets a margin at the bottom of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  The bounding box of the model's active edges plus the specified margins define the size of the model box.  This control type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the height of the bounding box of the model's active edges. |
| `0.0 <= Value <= 163840000.0` | Specifies the margin, in model units. |

#### `M_BOX_MARGIN_LEFT`

Sets a margin at the left side of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  The bounding box of the model's active edges plus the specified margins define the size of the model box.  This control type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the width of the bounding box of the model's active edges. |
| `0.0 <= Value <= 163840000.0` | Specifies the margin, in model units. |

#### `M_BOX_MARGIN_RIGHT`

Sets a margin at the right side of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  The bounding box of the model's active edges plus the specified margins define the size of the model box.  This control type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the width of the bounding box of the model's active edges. |
| `0.0 <= Value <= 163840000.0` | Specifies the margin, in model units. |

#### `M_BOX_MARGIN_TOP`

Sets a margin at the top of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  The bounding box of the model's active edges plus the specified margins define the size of the model box.  This control type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 10.0% of the height of the bounding box of the model's active edges. |
| `0.0 <= Value <= 163840000.0` | Specifies the margin, in model units. |

#### `M_CAD_Y_AXIS`

Sets the direction of the Y-axis for a model of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).  When the model is added to the AGM context from a CAD DXF file, the model will retain its coordinate system (the origin and the axes). Most CAD DXF files are oriented so that the Y-axis is positive going up, whereas the coordinate system Aurora Imaging Library uses is oriented with the Y-axis positive going down. So if you take the edge coordinates of an object from a CAD DXF file and put them in an image, the imaged object will look flipped when compared to the original object. This is also the case for a model defined from a CAD DXF file. This means that, unless the object is symmetrical, the match will not be made.  This control type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FLIP` *(default)* | Specifies to flip the Y-axis for the model so that the Y-axis is positive going down. |
| `M_NO_FLIP` | Specifies not to flip the Y-axis for the model. |

#### `M_DETECTION_DELTA_NEG_ANGLE`

Sets the lower limit of the detection angle range, relative to the nominal search angle ([`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md)). That is, _LowerLimit_ = [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md)  - [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md). The detection angle range should be used to cover the expected variance in the angle of detection.  Note that the detection angle range only limits the angle of detection. The angle at which an occurrence is detected might not be exact; its true angle can typically be within 10° of the angle of detection, even if that falls outside of the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is disabled, the returned angle of an occurrence will always be the angle at which it was detected and will therefore always be within the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is enabled, a fit operation is performed starting from the angle of detection. In refining the angle so that it is more accurate, the angle of the occurrence's reference axis returned by [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_ANGLE`](../../Reference/agm/MagmGetResult.md) can end up being outside of the detection angle range.  Note that the greater the range of angles to be searched, the slower the search.  To set the upper limit for the detection angle range, use [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the lower limit of the detection angle range, in degrees. Note that [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md) + [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md) cannot be greater than 360.0°. |

#### `M_DETECTION_DELTA_POS_ANGLE`

Sets the upper limit of the detection angle range, relative to the nominal search angle ([`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md)). That is, _UpperLimit_ = [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md)  + [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md). The detection angle range should be used to cover the expected variance in the angle of detection.  Note that the detection angle range only limits the angle of detection. The angle at which an occurrence is detected might not be exact; its true angle can typically be within 10° of the angle of detection, even if that falls outside of the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is disabled, the returned angle of an occurrence will always be the angle at which it was detected and will therefore always be within the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is enabled, a fit operation is performed starting from the angle of detection. In refining the angle so that it is more accurate, the angle of the occurrence's reference axis returned by [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_ANGLE`](../../Reference/agm/MagmGetResult.md) can end up being outside of the detection angle range.  Note that the greater the range of angles to be searched, the slower the search.  To set the lower limit for the detection angle range, use [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the upper limit of the detection angle range, in degrees. Note that [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md) + [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md) cannot be greater than 360.0°. |

#### `M_DETECTION_MULTI_ANGLE_MODE`

Sets the mode with which to detect occurrences when searching at multiple angles.  The [`M_GLOBAL`](../../Reference/agm/MagmControl.md) mode can accelerate search speed, but can also increase the risk of missing overlapping occurrences that have close centers. You should specify [`M_INDIVIDUAL`](../../Reference/agm/MagmControl.md) if the search is not finding all model occurrences, but note that this will slow down the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GLOBAL` *(default)* | Specifies to detect occurrences across all search angles globally. |
| `M_INDIVIDUAL` | Specifies to detect occurrences at each search angle individually. |

#### `M_DETECTION_REF_ANGLE`

Sets the nominal search angle; this is the angle at which you expect to find the model's reference axis in the target. Note that occurrences can be slightly rotated from the search angle and still be detected. The exact degree varies depending on your model, but the [`MagmFind`](../../Reference/agm/MagmFind.md) operation will typically detect occurrences within 10° of the search angle. If you expect greater degrees in variation, you should specify a detection angle range, using [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md) and [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md).  Note that specifying a nominal search angle does not decrease the search speed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 360.0` *(default)* | Specifies the nominal search angle, in degrees. |

#### `M_DETECTION_STEP_ANGLE`

Sets the step angle at which to search for occurrences of the model within the specified detection angle range. Note that the smaller the step angle, the slower the search. The search is performed at the nominal search angle and at every +/- [`M_DETECTION_STEP_ANGLE`](../../Reference/agm/MagmControl.md) degrees within the detection angle range. Note that occurrences can be slightly rotated from the search angle and still be detected. You should typically set the step angle to this rotation * 2. For example, if your model can tolerate the occurrence being offset from its search angle by ±5°, you should set the step angle to 10°.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `5.0 <= Value <= 180.0` *(default)* | Specifies the step angle, in degrees. |

#### `M_EDGEL_ANGLE_MODE`

Sets whether to consider edgel orientation, defined by a gradient angle, when searching for occurrences. A smaller difference in orientation between model and occurrence edgels produces a closer match.  The gradient angle is the direction of maximum change in brightness at the edgel's position. Note that edgel orientation is indifferent to whether the brightness changes from dark to light or the reverse. This means that the maximum difference in orientation between two edgels is 90 degrees.  Disabling [`M_EDGEL_ANGLE_MODE`](../../Reference/agm/MagmControl.md) will increase the search speed. You can typically disable this control when your target images are uncluttered with minimal noise.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to consider edgel orientation. |
| `M_ORIENTATION` *(default)* | Specifies to consider edgel orientation.  The difference in orientation between model and occurrence edgels is negatively correlated with the occurrence's detection, fit, and coverage scores. Smaller comparative differences result in higher scores (and vice versa). |

#### `M_EMPTY_REGIONS_MODE`

Sets whether to consider the regions of the model that do not contain edges when searching for model occurrences. If the model has a consistent region without edges, you can enable this control type to improve the detection score of favorable occurrences. If edges are present in this region of the occurrence, the detection score will decrease.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to automatically determine the empty regions. If no empty regions are found, the outcome is the same as disabling empty regions. |
| `M_DISABLE` *(default)* | Specifies not to consider empty regions. |

#### `M_FIRST_LEVEL`

Sets the resolution level for the initial stage (lowest resolution) of the search when [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md).  Level 0 is the original target and each higher level is half the size (and resolution) of the previous one. Note that very small models and search regions cannot be subsampled as many times as large ones. If the specified level results in a model that is too small or does not have enough edgels, an error will occur when calling [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md).  A higher first level speeds up the initial search but might make it less reliable because the model might not retain enough distinctive features at a lower resolution.  [`M_FIRST_LEVEL`](../../Reference/agm/MagmControl.md) is for advanced users of the AGM module. The default setting usually provides the best results for the search operation.  Note, [`M_FIRST_LEVEL`](../../Reference/agm/MagmControl.md) cannot be lower than [`M_LAST_LEVEL`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 7` *(default)* | Specifies the resolution level. Only integer values are accepted. |

#### `M_FIT_MODE`

Sets the mode of the fit operation. The fit mode affects positional accuracy and search speed. The positional results are most accurate when the fit operation uses subpixel precision, but this can reduce the speed of the search.  Note that the fit can only be calculated with subpixel precision when the edges of the target images are extracted ([`M_USE_MAGNITUDE_TARGET`](../../Reference/agm/MagmControl.md) is disabled).  You can disable the fit operation to speed up the search, in which case Aurora Imaging Library uses the features of the model and target to find occurrences, but it doesn't do any fitting. This results in a lower accuracy because the positional results are simply the positions of detection.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the fit operation; the positional results are determined by detection only. |
| `M_PIXEL_PRECISION` *(default)* | Specifies to calculate the fit with pixel precision. |
| `M_SUBPIXEL_PRECISION` | Specifies to calculate the fit with subpixel precision.  This value is only available when [`M_USE_MAGNITUDE_TARGET`](../../Reference/agm/MagmControl.md) is set to [`M_DISABLE`](../../Reference/agm/MagmControl.md). |

#### `M_LAST_LEVEL`

Sets the resolution level for the final stage (the highest resolution) of the search when [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md).  Level 0 is the original target and each higher level is half the size (and resolution) of the previous one. Note that very small models and search regions cannot be subsampled as many times as large ones. If the specified level results in a model that is too small or does not have enough edgels, an error will occur when calling [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md).  A higher last level speeds up the initial search but might make it less reliable and less accurate because the model might not retain enough distinctive features at a low resolution.  [`M_LAST_LEVEL`](../../Reference/agm/MagmControl.md) is for advanced users of the AGM module. The default setting usually provides the best results for the search operation.  Note, [`M_LAST_LEVEL`](../../Reference/agm/MagmControl.md) cannot be higher than [`M_FIRST_LEVEL`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 7` *(default)* | Specifies the resolution level. Only integer values are accepted. |

#### `M_MAX_ACCEPTED_OVERLAP`

Sets the maximum allowable overlap between found occurrences, as a percentage.  Note that [`M_MAX_ACCEPTED_OVERLAP`](../../Reference/agm/MagmControl.md) is applied after possible occurrences are sorted.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_ACCEPTED_OVERLAP`](Reference/agm/MagmControl.md))* |  |

#### `M_MAX_ASSOCIATION_DISTANCE`

Sets the maximum distance between a model edgel and the nearest occurrence edgel. Model edgels falling outside the specified maximum distance are not used to perform the occurrence's regular fit operation, but will decrease the overall fit score and the coverage score.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_ASSOCIATION_DISTANCE`](Reference/agm/MagmControl.md))* |  |

#### `M_MODEL_PRECISION`

Sets the percentage of model edgels to use when searching for occurrences.  Specifying a high value can increase the detection, fit, and coverage scores, which increases the likelihood of finding all occurrences, but can also decrease the search speed. Specifying a low value can decrease the detection, fit, and coverage scores, which increases the likelihood of missing favorable occurrences or finding false occurrences, but can also increase the search speed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the amount of model edgels, as a percentage. A value of 100% indicates that all model edgels are used. |

#### `M_PERSEVERANCE_DETECTION`

Sets the algorithm's perseverance when searching for occurrences. This affects the number of candidates the algorithm attempts to validate. Increasing the perseverance will increase robustness, but will increase search time.  You should try increasing the perseverance if the search is not finding all model occurrences, or if the target is complex and decreasing [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) is insufficient.

| Value | Description |
| --- | --- |
| *(see [`M_PERSEVERANCE_DETECTION`](Reference/agm/MagmControl.md))* |  |

#### `M_PIXEL_SCALE`

Sets the pixel scale of the model, if the model was defined from a CAD DXF file. This value is the scale to apply to the model to go from DXF units to pixel units. For example, if [`M_BOX_SIZE_X`](../../Reference/agm/MagmInquire.md) is 50.0 DXF units and [`M_PIXEL_SCALE`](../../Reference/agm/MagmControl.md) is set to 100.0, the model box will be 5000.0 pixels wide.  This control type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 10000.0` *(default)* | Specifies the pixel scale. [`M_BOX_SIZE_...`](../../Reference/agm/MagmInquire.md) * [`M_PIXEL_SCALE`](../../Reference/agm/MagmControl.md) must be less than or equal to 32768 pixels and [`M_EDGES_BOX_SIZE_...`](../../Reference/agm/MagmInquire.md) * [`M_PIXEL_SCALE`](../../Reference/agm/MagmControl.md) must be between 6 and 32768 pixels to avoid an Aurora Imaging Library error when calling [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md) or [`MagmDraw`](../../Reference/agm/MagmDraw.md). |

#### `M_POLARITY`

Sets the expected polarity of occurrences, compared to that of the model.  This control type is not available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the occurrences can be any polarity. That is, they can be the same or reverse of that of the model, or a mixture of polarities. |
| `M_REVERSE` | Specifies that the polarity of occurrences is the reverse of that of the model. |
| `M_SAME` | Specifies that the polarity of occurrences is the same as that of the model. |

#### `M_PYRAMID_LEVELS_MODE`

Sets how to establish the resolution levels for the search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_CONTENT_BASED` *(default)* | Specifies to automatically determine the levels according to the model content. |
| `M_USER_DEFINED` | Specifies to use the values set with [`M_FIRST_LEVEL`](../../Reference/agm/MagmControl.md) and [`M_LAST_LEVEL`](../../Reference/agm/MagmControl.md). |

#### `M_REFERENCE_X`

Sets the X-coordinate of the origin of the model's reference axis, relative to the model origin. Position results return the X-coordinate of the model's reference axis origin transformed at the model occurrence. Position results are relative to the target origin.  Note that the reference axis origin need not be in the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default X-coordinate will be used.  For a model of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md), the default value is the origin of the CAD DXF file. For other types of models, the default value is the center of the model. |
| `Value` | Specifies the X-coordinate, in pixels. This value can be specified with subpixel accuracy. |

#### `M_REFERENCE_Y`

Sets the Y-coordinate of the origin of the model's reference axis, relative to the model origin. Position results return the Y-coordinate of the model's reference axis origin transformed at the model occurrence. Position results are relative to the target origin.  Note that the reference axis origin need not be in the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default Y-coordinate will be used.  For a model of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md), the default value is the origin of the CAD DXF file. For other types of models, the default value is the center of the model. |
| `Value` | Specifies the Y-coordinate, in pixels. This value can be specified with subpixel accuracy. |

#### `M_SMOOTHNESS`

Sets the degree of smoothness (noise reduction) applied to the model image and target image(s) during the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_SMOOTHNESS`](Reference/agm/MagmControl.md))* |  |

#### `M_SORT_CANDIDATES_SCORE`

Sets the score used to sort candidates. Candidates are sorted in decreasing score order.  The specified score affects which possible occurrences are filtered out. When two possible occurrences overlap above the value specified with [`M_MAX_ACCEPTED_OVERLAP`](../../Reference/agm/MagmControl.md), the possible occurrence with the lower score is removed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SCORE_COVERAGE` *(default)* | Specifies to use the coverage score to sort candidates. |
| `M_SCORE_DETECTION` | Specifies to use the detection score to sort candidates. |
| `M_SCORE_FIT` | Specifies to use the fit score to sort candidates. |
| `M_SCORE_FIT_OVERALL` | Specifies to use the overall fit score to sort candidates. |

#### `M_STABLE_EDGELS_MODE`

Sets whether to use only stable edgels when searching for occurrences. A stable edgel is an edgel that remains persistent when extracting edges with slightly different smoothness values.  This control type is only available for models of type [`M_IMAGE`](../../Reference/agm/MagmDefine.md) and [`M_IMAGE_REGION`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to use only stable edgels. All edgels are considered when searching for occurrences. |
| `M_SMOOTHNESS_STABLE` *(default)* | Specifies to use only stable edgels. Unstable (noisy) edgels are ignored when searching for occurrences. |

#### `M_STEP_X`

Sets the X step size of explored positions to search for occurrences. Increasing [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) can accelerate search speed, but can also increase the risk of missing favorable occurrences.  You can typically start with a value of 1 and increase [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) if the speed is too slow. Increasing these values when you have a large model can decrease the search time while minimally affecting the occurrences' detection, fit, and coverage scores.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_X`](Reference/agm/MagmControl.md))* |  |

#### `M_STEP_Y`

Sets the Y step size of explored positions to search for occurrences. Increasing [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) can accelerate search speed, but can also increase the risk of missing favorable occurrences.  You can typically start with a value of 1 and increase [`M_STEP_X`](../../Reference/agm/MagmControl.md) and [`M_STEP_Y`](../../Reference/agm/MagmControl.md) if the speed is too slow. Increasing these values when you have a large model can decrease the search time while minimally affecting the occurrences' detection, fit, and coverage scores.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_Y`](Reference/agm/MagmControl.md))* |  |

#### `M_THRESHOLD_MODE`

Sets the threshold mode of the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_THRESHOLD_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_USE_MAGNITUDE_TARGET`

Sets whether to use the gradient magnitude or to use extracted edges when searching for occurrences.  When this control type is set to [`M_ENABLE`](../../Reference/agm/MagmControl.md), thresholding is not applied to the target images, and the magnitude of the gradient is used instead. This is useful when there are contrast variations within or between images, such that there is no single threshold capable of detecting all pertinent edgels without introducing noisy background edges, leading to false positives.  When set to [`M_DISABLE`](../../Reference/agm/MagmControl.md), edges are extracted from the target images based on a thresholding of the magnitude values. When the occurrences have strong contrasted edges that can be detected without introducing noisy background edges, using edges can lead to more robust results.  Note that even if this control type is enabled, [`M_THRESHOLD_MODE`](../../Reference/agm/MagmControl.md) is used to extract the edges of the model.  This control type is not available when [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_SUBPIXEL_PRECISION`](../../Reference/agm/MagmControl.md) or when using an Edge Finder result buffer as the target.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to extract and use edges. |
| `M_ENABLE` | Specifies to use the magnitude of the gradient. |

---

### `Train AGM context ID with a composite-definition model`

Specifies a train AGM context, allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_TRAIN`](../../Reference/agm/MagmAlloc.md) and used in [`MagmTrain`](../../Reference/agm/MagmTrain.md) operations. It must contain a composite-definition model.

#### `M_EMPTY_REGIONS_MODE`

Sets whether to consider the regions of the model appearances that do not contain edges when training the composite-definition model. This will slow down training, but if the model has a consistent region without edges, using empty regions can improve the detection score of favorable occurrences. If edges are present in this region of the occurrence, the detection score will decrease.  The setting of this control type is also used for the [`MagmFind`](../../Reference/agm/MagmFind.md) operation when searching for the trained composite-definition model.

| Value | Description |
| --- | --- |
| *(see [`M_EMPTY_REGIONS_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_MODEL_PRECISION`

Sets the percentage of the trained composite-definition model's edgels to use when searching for occurrences.  Specifying a high value can increase the detection, fit, and coverage scores, which increases the likelihood of finding all occurrences, but can also decrease the search speed. Specifying a low value can decrease the detection, fit, and coverage scores, which increases the likelihood of missing favorable occurrences or finding false occurrences, but can also increase the search speed.

| Value | Description |
| --- | --- |
| *(see [`M_MODEL_PRECISION`](Reference/agm/MagmControl.md))* |  |

#### `M_NEGATIVE_LABELS_MAX_OVERLAP`

Sets the maximum allowable overlap between two automatically generated negative (red) rectangles.  This control type is only used when [`M_NEGATIVE_LABELS_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_AUTO`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 90.0` *(default)* | Specifies the maximum allowable overlap, as a percentage. |

#### `M_NEGATIVE_LABELS_MODE`

Sets whether to automatically label negative model locations in the training images or to use manually defined labels. If set to [`M_AUTO`](../../Reference/agm/MagmControl.md), [`MagmTrain`](../../Reference/agm/MagmTrain.md) will automatically identify and label background areas with red rectangles.  Note that the number of automatically generated red rectangles depends on the complexity of the training images. You can use [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_NUMBER_NEG_RECTANGLES`](../../Reference/agm/MagmGetResult.md) to retrieve the number of red rectangles across all training images. You can use [`MagmDraw`](../../Reference/agm/MagmDraw.md) to view the rectangles.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_LABELS` *(default)* | Specifies to use manually defined negative (red) rectangles if there is at least one red rectangle in the set of training images; otherwise, specifies to automatically label the negative model locations. |
| `M_AUTO` | Specifies to automatically label the negative model locations. At least one training image must have a minimum of one blue rectangle, and no training images can have any red rectangles. Note, there can be training images with no rectangles. |
| `M_USER_DEFINED` | Specifies to use manually defined negative (red) rectangles. At least one training image must have a minimum of one red rectangle, and at least one training image must have a minimum of one blue rectangle. Note, there can be training images with only red rectangles or only blue rectangles. You can also include training images with no rectangles, but they will not be used during training. |

#### `M_NEGATIVE_LABELS_POSITIVE_MAX_OVERLAP`

Sets the maximum allowable overlap between an automatically generated negative (red) rectangle and a positive (blue) rectangle.  This control type is only used when [`M_NEGATIVE_LABELS_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_AUTO`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 80.0` *(default)* | Specifies the maximum allowable overlap, as a percentage. |

#### `M_REALIGN_STRENGTH`

Sets the strength with which to displace the blue rectangles, which define the positive model locations in the training images, to better align them. The more aligned the blue rectangles are, the more accurate the search with the resulting trained composite-definition model will be.  You should attempt to label your training images such that all model occurrences appear in the exact same position within each blue rectangle. If the blue rectangles are not precise, you can use this control to allow the [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation to displace the blue rectangles. The higher the realignment strength, the further [`MagmTrain`](../../Reference/agm/MagmTrain.md) can displace the blue rectangles so that the model occurrences appear in the same position within each blue rectangle. When the realignment strength is set to 0.0, no realignment occurs.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the strength of the realignment. |

#### `M_SMOOTHNESS`

Sets the degree of smoothness (noise reduction) applied to the training images during the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_SMOOTHNESS`](Reference/agm/MagmControl.md))* |  |

#### `M_THRESHOLD_MODE`

Sets the threshold mode of the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_THRESHOLD_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_TRAIN_DETECTION_ANGLE`

Sets whether to train the composite-definition model using rotated labeled image regions (red and blue rectangles). You should enable this control to increase the likelihood of finding occurrences at any angle. A composite-definition model trained without rotation is more likely to miss rotated occurrences.  Note that if this control type is set to [`M_DISABLE`](../../Reference/agm/MagmControl.md), calling [`MagmTrain`](../../Reference/agm/MagmTrain.md) generates an error if your training images contain rotated labeled image regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_LABELS` *(default)* | Specifies to use rotated labeled image regions if there is at least one rotated labeled image region in the set of training images; otherwise, specifies not to use rotated labeled image regions. |
| `M_DISABLE` | Specifies not to use rotated labeled image regions. All labeled image regions must have an angle of 0.0°. |
| `M_ENABLE` | Specifies to use rotated labeled image regions. Labeled image regions can have any angle. |

#### `M_TRAIN_LABELS_NOISE_MODE`

Sets the mode with which to handle substandard labeled image regions when training the composite-definition model. If this control type is set to [`M_LABELS_CONFIDENCE`](../../Reference/agm/MagmControl.md), the [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation will be robust to substandard labeled image regions that might cause distortion due to the presence of a lot of noise, blue rectangles (positive model locations) that have missing or extra features in comparison to other blue rectangles, red rectangles (negative model locations) that contain similar features to the model, or incorrect labeling (model appearances labeled with negative (red) rectangles or background samples labeled with positive (blue) rectangles).  Disabling [`M_TRAIN_LABELS_NOISE_MODE`](../../Reference/agm/MagmControl.md) can increase the training speed, but can result in a more distorted trained composite-definition model if the training images contain any substandard labeled image regions, as listed above.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to handle substandard labeled image regions differently. [`MagmTrain`](../../Reference/agm/MagmTrain.md) will strongly attempt to correctly classify all labeled regions, including outlier labeled regions, incorrect labeled regions, or labeled regions with a lot of noise, which can cause the model to overfit on the training data. |
| `M_LABELS_CONFIDENCE` *(default)* | Specifies to be robust to substandard labels. To prevent overfitting on the training data, labels that are hard to correctly classify, such as outlier labels, incorrect labels, or labels with a lot of noise, will have less of an affect on the resulting trained composite-definition model. |

#### `M_USE_MAGNITUDE_TARGET`

Sets whether to use the gradient magnitude or to use extracted edges when training the composite-definition model and searching for occurrences.  When this control type is set to [`M_ENABLE`](../../Reference/agm/MagmControl.md), thresholding is not applied to the training images or target images, and the magnitude of the gradient is used instead. This is useful when there are contrast variations within or between images, such that there is no single threshold capable of detecting all pertinent edgels without introducing noisy background edges, leading to false positives.  When set to [`M_DISABLE`](../../Reference/agm/MagmControl.md), edges are extracted from the training and target images based on a thresholding of the magnitude values. When the occurrences have strong contrasted edges that can be detected without introducing noisy background edges, using edges can lead to more robust results.  Note that even if this control type is enabled, [`M_THRESHOLD_MODE`](../../Reference/agm/MagmControl.md) will be copied to the find AGM context, and used to extract the edges of the model when [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md).  This control type is not available when [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_SUBPIXEL_PRECISION`](../../Reference/agm/MagmControl.md) or when using an Edge Finder result buffer as the target.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to extract and use edges. |
| `M_ENABLE` | Specifies to use the magnitude of the gradient. |

### For controlling a train AGM result buffer

The following [`ContextOrResultId`](../../Reference/agm/MagmControl.md), [`ControlType`](../../Reference/agm/MagmControl.md), and [`ControlValue`](../../Reference/agm/MagmControl.md) parameter settings can be specified for a train AGM result buffer. In this case, you must set the [`Index`](../../Reference/agm/MagmControl.md) parameter to [`M_GENERAL`](../../Reference/agm/MagmControl.md).

---

### `Train AGM result buffer ID`

Specifies a train AGM result buffer, allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md).

#### `M_STOP_TRAIN`

Stops the current execution of [`MagmTrain`](../../Reference/agm/MagmTrain.md). This can only be done from another thread of higher priority. Results already available in the result buffer become invalid and will be discarded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

The margins add a black space around the model, which affect whether two occurrences are considered overlapping. For example, even if none of an occurrence's edges touch another occurrence, if the margin around the bounding box of its edges overlaps with the margin and/or bounding box of another occurrence by any amount, the occurrences will be considered overlapping, and might be filtered out according to [`M_MAX_ACCEPTED_OVERLAP`](../../Reference/agm/MagmControl.md). Note that the margins will be included when drawing the model using [`MagmDraw`](../../Reference/agm/MagmDraw.md).
