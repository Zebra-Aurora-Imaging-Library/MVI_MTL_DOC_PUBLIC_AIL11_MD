---
doctype: Reference
module: agm
function: MagmInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmInquire"
---

# MagmInquire

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

> Inquire information about a specified AGM context, model, or result buffer.

## Syntax

```c
AIL_INT64 MagmInquire(
    AIL_ID    ContextOrResultId,  //in
    AIL_INT64 Index,              //in
    AIL_INT64 InquireType,        //in
    void *    UserVarPtr          //out
)
```

## Description

This function inquires about a setting of an AGM context, a model contained therein, or an AGM result buffer.

## Parameters

### `ContextOrResultId` *(in, AIL_ID)*

Specifies the identifier of the find or train AGM context or result buffer about which to inquire information.

### `Index` *(in, AIL_INT64)*

Specifies that information will be inquired about an AGM context, an individual model, or an AGM result buffer. Set this parameter to one of the following values:

*For specifying a context, model, or result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default.

If an AGM context is specified, same as [`M_CONTEXT`](../../Reference/agm/MagmInquire.md).

If an AGM result buffer is specified, same as [`M_GENERAL`](../../Reference/agm/MagmInquire.md). |
| `M_AGM_MODEL_INDEX` | Inquires information about an individual model. |
| `M_CONTEXT` | Inquires information about an AGM context. |
| `M_GENERAL` | Inquires information about an AGM result buffer. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of information about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MagmInquire`](../../Reference/agm/MagmInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a find or train AGM context

To inquire information about a find or train AGM context, the [`InquireType`](../../Reference/agm/MagmInquire.md) parameter can be set to one of the following values. In this case, you must set the [`Index`](../../Reference/agm/MagmInquire.md) parameter to [`M_CONTEXT`](../../Reference/agm/MagmInquire.md) or [`M_DEFAULT`](../../Reference/agm/MagmInquire.md).

---

### `AGM context ID`

Specifies the identifier of a find or train AGM context, allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md).

#### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time settings for the context are modified.  Although you cannot identify the modification counter's contents, you can compare them throughout your application to know if the context has been altered. If the modification counter has changed you can, for example, prompt the user to save before closing the application.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

#### `M_NUMBER_OF_MODELS`

Inquires the number of models in the AGM context.

| Value | Description |
| --- | --- |
| `0 <= Value <= 1` | Specifies the number of models. |

#### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the AGM context was allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

#### `M_PREPROCESSED`

Inquires whether the AGM context is preprocessed. The context must be preprocessed (using [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md)) before calling [`MagmFind`](../../Reference/agm/MagmFind.md) or [`MagmTrain`](../../Reference/agm/MagmTrain.md). After certain settings of the AGM context are changed with [`MagmControl`](../../Reference/agm/MagmControl.md), a model is added or removed with [`MagmDefine`](../../Reference/agm/MagmDefine.md), or a composite-definition model is copied with [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md), this inquire type will indicate that the context is no longer in its preprocessed state.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context is not preprocessed. |
| `M_TRUE` | Specifies that the context is preprocessed. |

### For inquiring about the type of model in a find or train AGM context

To inquire information about the type of model in a find or train AGM context, the [`InquireType`](../../Reference/agm/MagmInquire.md) parameter can be set to the following value. In this case, you must set the [`Index`](../../Reference/agm/MagmInquire.md) parameter to the index of a specific model, using **M_AGM_MODEL_INDEX()**.

---

### `AGM context ID with a model`

Specifies a find or train AGM context, allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md). It must contain a model.

#### `M_MODEL_TYPE`

Inquires the type of the specified model.

| Value | Description |
| --- | --- |
| `M_COMPOSITE` | Specifies a composite-definition model. |
| `M_SINGLE` | Specifies a single-definition model. |

### For inquiring about a model in an AGM context

To inquire information about a model in an AGM context, the [`InquireType`](../../Reference/agm/MagmInquire.md) parameter can be set to one of the following values. In this case, you must set the [`Index`](../../Reference/agm/MagmInquire.md) parameter to the index of a specific model, using **M_AGM_MODEL_INDEX()**.

---

### `Find AGM context ID with a composite-definition model`

Specifies a find AGM context, allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md) and used in [`MagmFind`](../../Reference/agm/MagmFind.md) operations. It must contain a composite-definition model.

#### `M_ACCEPTANCE_COVERAGE`

Inquires the minimum coverage score required for an occurrence to be considered a match.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_COVERAGE`](Reference/agm/MagmControl.md))* |  |

#### `M_ACCEPTANCE_DETECTION`

Inquires the minimum detection score required for an occurrence to be considered a match.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_DETECTION`](Reference/agm/MagmControl.md))* |  |

#### `M_ACCEPTANCE_DETECTION_FROM_TRAIN`

Inquires the minimum detection score required for an occurrence to be considered a match; this score was automatically determined during the training of the composite-definition model.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the minimum acceptable detection score, as a percentage. |

#### `M_ACCEPTANCE_DETECTION_SOURCE`

Inquires whether the trained acceptance level or a custom acceptance level is used for the detection score.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_DETECTION_SOURCE`](Reference/agm/MagmControl.md))* |  |
| `M_USER_DEFINED` | Specifies to use the value specified with [`M_ACCEPTANCE_DETECTION`](../../Reference/agm/MagmInquire.md). |

#### `M_ACCEPTANCE_FIT`

Inquires the minimum fit score required for an occurrence to be considered a match.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_FIT`](Reference/agm/MagmControl.md))* |  |

#### `M_ACCEPTANCE_FIT_OVERALL`

Inquires the minimum overall fit score required for an occurrence to be considered a match.  This inquire type is only available when [`M_MODEL_SOURCE`](../../Reference/agm/MagmInquire.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_FIT_OVERALL`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_DELTA_ANGLE_SOURCE`

Inquires whether to use the trained detection angle range or a custom detection angle range to limit the angle of detection.

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_DELTA_ANGLE_SOURCE`](Reference/agm/MagmControl.md))* |  |
| `M_USER_DEFINED` | Specifies to use the values set with [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmInquire.md) and [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmInquire.md). |

#### `M_DETECTION_DELTA_NEG_ANGLE`

Inquires the lower limit of the detection angle range, relative to the nominal search angle ([`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmInquire.md)). That is, _LowerLimit_ = [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmInquire.md)  - [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_DELTA_NEG_ANGLE`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_DELTA_POS_ANGLE`

Inquires the upper limit of the detection angle range, relative to the nominal search angle ([`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmInquire.md)). That is, _UpperLimit_ = [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmInquire.md)  + [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_DELTA_POS_ANGLE`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_MULTI_ANGLE_MODE`

Inquires the mode with which to detect occurrences when searching at multiple angles.

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_MULTI_ANGLE_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_REF_ANGLE`

Inquires the nominal search angle; this is the angle at which you expect to find the model's reference axis in the target.

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_REF_ANGLE`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_STEP_ANGLE`

Inquires the step angle at which to search for occurrences of the model.

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_STEP_ANGLE`](Reference/agm/MagmControl.md))* |  |

#### `M_EMPTY_REGIONS_MODE`

Inquires whether the regions of the trained composite-definition model that do not contain edges are considered when searching for model occurrences. Note that you can set this in the train AGM context using [`M_EMPTY_REGIONS_MODE`](../../Reference/agm/MagmInquire.md) before training; once the model is trained and copied to a find AGM context, you cannot change this setting.

| Value | Description |
| --- | --- |
| *(see [`M_EMPTY_REGIONS_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_FIT_MODE`

Inquires the mode of the fit operation.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_HAS_USER_MODEL`

Inquires whether the model contains a separate image to use for the fit and coverage calculations when [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmInquire.md).  For a find AGM context containing a trained composite-definition model, you can specify a model image buffer using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_MODEL_IMAGE`](../../Reference/agm/MagmControl.md). You can then set [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) to use this model image for the fit and coverage calculations. If a model image buffer was not specified, this inquire type will return [`M_FALSE`](../../Reference/agm/MagmInquire.md), and the trained composite-definition model will be used for these calculations.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the model does not contain a separate image. |
| `M_TRUE` | Specifies that the model contains a separate image. |

#### `M_MAX_ACCEPTED_OVERLAP`

Inquires the maximum allowable overlap between found occurrences, as a percentage.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_ACCEPTED_OVERLAP`](Reference/agm/MagmControl.md))* |  |

#### `M_MAX_ASSOCIATION_DISTANCE`

Inquires the maximum distance that a model edgel can be from the nearest occurrence edgel for that edgel to count towards the regular fit operation and increase the coverage score. Note that the maximum association distance has no effect on the overall fit score.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_ASSOCIATION_DISTANCE`](Reference/agm/MagmControl.md))* |  |

#### `M_MODEL_PRECISION`

Inquires the percentage of the trained composite-definition model's edgels to use when searching for occurrences. Note that you can set this in the train AGM context using [`M_MODEL_PRECISION`](../../Reference/agm/MagmInquire.md) before training; once the model is trained and copied to a find AGM context, you cannot change this setting.

| Value | Description |
| --- | --- |
| *(see [`M_MODEL_PRECISION`](Reference/agm/MagmControl.md))* |  |

#### `M_MODEL_SOURCE`

Inquires whether the trained composite-definition model or a prototypical image of the model is used for the fit and coverage calculations.

| Value | Description |
| --- | --- |
| *(see [`M_MODEL_SOURCE`](Reference/agm/MagmControl.md))* |  |
| `M_USER_IMAGE` | Specifies to use the image set with [`M_MODEL_IMAGE`](../../Reference/agm/MagmControl.md). |

#### `M_PERSEVERANCE_DETECTION`

Inquires the algorithm's perseverance when searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_PERSEVERANCE_DETECTION`](Reference/agm/MagmControl.md))* |  |

#### `M_REFERENCE_X`

Inquires the X-coordinate of the origin of the model's reference axis, relative to the model origin.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_X`](Reference/agm/MagmControl.md))* |  |

#### `M_REFERENCE_Y`

Inquires the Y-coordinate of the origin of the model's reference axis, relative to the model origin.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_Y`](Reference/agm/MagmControl.md))* |  |

#### `M_SIZE_X`

Inquires the width of the model. The width of the trained composite-definition model is the same width as the training images' rectangles.

| Value | Description |
| --- | --- |
| `Value >= 6` | Specifies the width of the model, in pixels. |

#### `M_SIZE_Y`

Inquires the height of the model. The height of the trained composite-definition model is the same height as the training images' rectangles.

| Value | Description |
| --- | --- |
| `Value >= 6` | Specifies the height of the model, in pixels. |

#### `M_SMOOTHNESS`

Inquires the degree of smoothness (noise reduction) applied to the model image and target image(s) during the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_SMOOTHNESS`](Reference/agm/MagmControl.md))* |  |

#### `M_SMOOTHNESS_FROM_TRAIN`

Inquires the degree of smoothness (noise reduction) applied to the training images during the edge extraction. Note that you can set this in the train AGM context using [`M_SMOOTHNESS`](../../Reference/agm/MagmInquire.md) before training; once the model is trained and copied to a find AGM context, you cannot change this setting.

| Value | Description |
| --- | --- |
| *(see [`M_SMOOTHNESS`](Reference/agm/MagmControl.md))* |  |

#### `M_SMOOTHNESS_SOURCE`

Inquires whether the smoothness value set during training or a custom smoothness value is used for the degree of noise reduction applied to the model image and target image(s) during the edge extraction.

| Value | Description |
| --- | --- |
| `M_FROM_TRAIN` | Specifies to use the smoothness value set during the training of the model.  This value was set with [`M_SMOOTHNESS`](../../Reference/agm/MagmInquire.md) in a train AGM context. |
| `M_USER_DEFINED` | Specifies to use the smoothness value set with [`M_SMOOTHNESS`](../../Reference/agm/MagmInquire.md). |

#### `M_SORT_CANDIDATES_SCORE`

Inquires the type of score used to sort candidates.

| Value | Description |
| --- | --- |
| *(see [`M_SORT_CANDIDATES_SCORE`](Reference/agm/MagmControl.md))* |  |

#### `M_STABLE_EDGELS_MODE`

Inquires whether only stable edges are used when searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_STABLE_EDGELS_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_STEP_X`

Inquires the X step size of explored positions to search for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_X`](Reference/agm/MagmControl.md))* |  |

#### `M_STEP_Y`

Inquires the Y step size of explored positions to search for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_Y`](Reference/agm/MagmControl.md))* |  |

#### `M_THRESHOLD_MODE`

Inquires the threshold mode of the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_THRESHOLD_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_THRESHOLD_MODE_FROM_TRAIN`

Inquires the threshold mode of the edge extraction used during training.

| Value | Description |
| --- | --- |
| *(see [`M_THRESHOLD_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_THRESHOLD_MODE_SOURCE`

Inquires whether the threshold mode set during training or a custom threshold mode is used for the edge extraction.

| Value | Description |
| --- | --- |
| `M_FROM_TRAIN` | Specifies to use the threshold mode set during the training of the model.  This value was set with [`M_THRESHOLD_MODE`](../../Reference/agm/MagmInquire.md) in a train AGM context. |
| `M_USER_DEFINED` | Specifies to use the threshold mode set with [`M_THRESHOLD_MODE`](../../Reference/agm/MagmInquire.md). |

#### `M_TRAIN_DETECTION_ANGLE`

Inquires whether using rotated labeled image regions (red and blue rectangles) to train the composite-definition model was enabled.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the use of rotated labeled image regions was disabled. |
| `M_ENABLE` | Specifies that the use of rotated labeled image regions was enabled. |

---

### `Find AGM context ID with a single-definition model`

Specifies a find AGM context, allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md) and used in [`MagmFind`](../../Reference/agm/MagmFind.md) operations. It must contain a single-definition model.

#### `M_ACCEPTANCE_COVERAGE`

Inquires the minimum coverage score required for an occurrence to be considered a match.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_COVERAGE`](Reference/agm/MagmControl.md))* |  |

#### `M_ACCEPTANCE_DETECTION`

Inquires the minimum detection score required for an occurrence to be considered a match.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_DETECTION`](Reference/agm/MagmControl.md))* |  |

#### `M_ACCEPTANCE_FIT`

Inquires the minimum fit score required for an occurrence to be considered a match.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_FIT`](Reference/agm/MagmControl.md))* |  |

#### `M_ACCEPTANCE_FIT_OVERALL`

Inquires the minimum overall fit score required for an occurrence to be considered a match.

| Value | Description |
| --- | --- |
| *(see [`M_ACCEPTANCE_FIT_OVERALL`](Reference/agm/MagmControl.md))* |  |

#### `M_BOX_MARGIN_BOTTOM`

Inquires the margin at the bottom of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| *(see [`M_BOX_MARGIN_BOTTOM`](Reference/agm/MagmControl.md))* |  |

#### `M_BOX_MARGIN_LEFT`

Inquires the margin at the left of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| *(see [`M_BOX_MARGIN_LEFT`](Reference/agm/MagmControl.md))* |  |

#### `M_BOX_MARGIN_RIGHT`

Inquires the margin at the right of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| *(see [`M_BOX_MARGIN_RIGHT`](Reference/agm/MagmControl.md))* |  |

#### `M_BOX_MARGIN_TOP`

Inquires the margin at the top of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| *(see [`M_BOX_MARGIN_TOP`](Reference/agm/MagmControl.md))* |  |

#### `M_BOX_OFFSET_X`

Inquires the X-offset of the top-left corner of the model box from the model origin, if the model was defined from a CAD DXF file.  For [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md) models, the origin is the same as the one defined in the CAD DXF file.  The model box is defined by the bounding box of the model's active edges plus the specified margins. You can inquire the margins using [`M_BOX_MARGIN_...`](../../Reference/agm/MagmInquire.md).  [`M_BOX_OFFSET_X`](../../Reference/agm/MagmInquire.md) is updated when the margins that define the model box are changed.  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset, in model units. |

#### `M_BOX_OFFSET_Y`

Inquires the Y-offset of the top-left corner of the model box from the model origin, if the model was defined from a CAD DXF file.  For [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md) models, the origin is the same as the one defined in the CAD DXF file.  The model box is defined by the bounding box of the model's active edges plus the specified margins. You can inquire the margins using [`M_BOX_MARGIN_...`](../../Reference/agm/MagmInquire.md).  [`M_BOX_OFFSET_Y`](../../Reference/agm/MagmInquire.md) is updated when the margins that define the model box are changed.  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset, in model units. |

#### `M_BOX_SIZE_X`

Inquires the size along the X-axis of the model box, if the model was defined from a CAD DXF file.  The model box is defined by the bounding box of the model's active edges plus the specified margins. You can inquire the margins using [`M_BOX_MARGIN_...`](../../Reference/agm/MagmInquire.md).  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-size, in model units. |

#### `M_BOX_SIZE_Y`

Inquires the size along the Y-axis of the model box, if the model was defined from a CAD DXF file.  The model box is defined by the bounding box of the model's active edges plus the specified margins. You can inquire the margins using [`M_BOX_MARGIN_...`](../../Reference/agm/MagmInquire.md).  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-size, in model units. |

#### `M_CAD_Y_AXIS`

Inquires the direction of the Y-axis for a model of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| *(see [`M_CAD_Y_AXIS`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_DELTA_NEG_ANGLE`

Inquires the lower limit of the detection angle range, relative to the nominal search angle ([`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmInquire.md)). That is, _LowerLimit_ = [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmInquire.md)  - [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_DELTA_NEG_ANGLE`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_DELTA_POS_ANGLE`

Inquires the upper limit of the detection angle range, relative to the nominal search angle ([`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmInquire.md)). That is, _UpperLimit_ = [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmInquire.md)  + [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_DELTA_POS_ANGLE`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_MULTI_ANGLE_MODE`

Inquires the mode with which to detect occurrences when searching at multiple angles.

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_MULTI_ANGLE_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_REF_ANGLE`

Inquires the nominal search angle; this is the angle at which you expect to find the model's reference axis in the target.

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_REF_ANGLE`](Reference/agm/MagmControl.md))* |  |

#### `M_DETECTION_STEP_ANGLE`

Inquires the step angle at which to search for occurrences of the model.

| Value | Description |
| --- | --- |
| *(see [`M_DETECTION_STEP_ANGLE`](Reference/agm/MagmControl.md))* |  |

#### `M_EDGEL_ANGLE_MODE`

Inquires whether edgel orientation, defined by a gradient angle, is considered when searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_EDGEL_ANGLE_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_EDGES_BOX_SIZE_X`

Inquires the size along the X-axis of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-size, in model units. |

#### `M_EDGES_BOX_SIZE_Y`

Inquires the size along the Y-axis of the bounding box of the model's active edges, if the model was defined from a CAD DXF file.  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-size, in model units. |

#### `M_EMPTY_REGIONS_MODE`

Inquires whether the regions of the model that do not contain edges are considered when searching for model occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_EMPTY_REGIONS_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_FIRST_LEVEL`

Inquires the resolution level for the initial stage of the search when [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmInquire.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_FIRST_LEVEL`](Reference/agm/MagmControl.md))* |  |

#### `M_FIRST_LEVEL_USED`

Inquires the resolution level used for the initial stage of the search.  When [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmInquire.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmInquire.md), this value is equal to [`M_FIRST_LEVEL`](../../Reference/agm/MagmInquire.md). When [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmInquire.md) is set to [`M_AUTO_CONTENT_BASED`](../../Reference/agm/MagmInquire.md), this value is the first level that was automatically determined during preprocessing.  Note, this value is only available after preprocessing.

| Value | Description |
| --- | --- |
| `0 <= Value <= 7` | Specifies the resolution level used. |

#### `M_FIT_MODE`

Inquires the mode of the fit operation.

| Value | Description |
| --- | --- |
| *(see [`M_FIT_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_LAST_LEVEL`

Inquires the resolution level for the final stage of the search when [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmInquire.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_LAST_LEVEL`](Reference/agm/MagmControl.md))* |  |

#### `M_LAST_LEVEL_USED`

Inquires the resolution level used for the final stage of the search.  When [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmInquire.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmInquire.md), this value is equal to [`M_LAST_LEVEL`](../../Reference/agm/MagmInquire.md). When [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmInquire.md) is set to [`M_AUTO_CONTENT_BASED`](../../Reference/agm/MagmInquire.md), this value is the last level that was automatically determined during preprocessing.  Note, this value is only available after preprocessing.

| Value | Description |
| --- | --- |
| `0 <= Value <= 7` | Specifies the resolution level used. |

#### `M_MAX_ACCEPTED_OVERLAP`

Inquires the maximum allowable overlap between found occurrences, as a percentage.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_ACCEPTED_OVERLAP`](Reference/agm/MagmControl.md))* |  |

#### `M_MAX_ASSOCIATION_DISTANCE`

Inquires the maximum distance that a model edgel can be from the nearest occurrence edgel for that edgel to count towards the regular fit operation and increase the coverage score. Note that the maximum association distance has no effect on the overall fit score.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_ASSOCIATION_DISTANCE`](Reference/agm/MagmControl.md))* |  |

#### `M_MODEL_PRECISION`

Inquires the percentage of model edgels to use when searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_MODEL_PRECISION`](Reference/agm/MagmControl.md))* |  |

#### `M_PERSEVERANCE_DETECTION`

Inquires the algorithm's perseverance when searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_PERSEVERANCE_DETECTION`](Reference/agm/MagmControl.md))* |  |

#### `M_PIXEL_SCALE`

Inquires the pixel scale of the model, if the model was defined from a CAD DXF file.  This inquire type is only available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| *(see [`M_PIXEL_SCALE`](Reference/agm/MagmControl.md))* |  |

#### `M_POLARITY`

Inquires the expected polarity of occurrences, compared to that of the model.  This inquire type is not available for models of type [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| *(see [`M_POLARITY`](Reference/agm/MagmControl.md))* |  |

#### `M_PYRAMID_LEVELS_MODE`

Inquires how to establish the resolution levels for the search.

| Value | Description |
| --- | --- |
| *(see [`M_PYRAMID_LEVELS_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_REFERENCE_X`

Inquires the X-coordinate of the origin of the model's reference axis, relative to the model origin.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_X`](Reference/agm/MagmControl.md))* |  |

#### `M_REFERENCE_Y`

Inquires the Y-coordinate of the origin of the model's reference axis, relative to the model origin.

| Value | Description |
| --- | --- |
| *(see [`M_REFERENCE_Y`](Reference/agm/MagmControl.md))* |  |

#### `M_SMOOTHNESS`

Inquires the degree of smoothness (noise reduction) applied to the model image and target image(s) during the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_SMOOTHNESS`](Reference/agm/MagmControl.md))* |  |

#### `M_SORT_CANDIDATES_SCORE`

Inquires the type of score used to sort candidates.

| Value | Description |
| --- | --- |
| *(see [`M_SORT_CANDIDATES_SCORE`](Reference/agm/MagmControl.md))* |  |

#### `M_STABLE_EDGELS_MODE`

Inquires whether only stable edges are used when searching for occurrences.  This inquire type is only available for models of type [`M_IMAGE`](../../Reference/agm/MagmDefine.md) and [`M_IMAGE_REGION`](../../Reference/agm/MagmDefine.md).

| Value | Description |
| --- | --- |
| *(see [`M_STABLE_EDGELS_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_STEP_X`

Inquires the X step size of explored positions to search for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_X`](Reference/agm/MagmControl.md))* |  |

#### `M_STEP_Y`

Inquires the Y step size of explored positions to search for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_STEP_Y`](Reference/agm/MagmControl.md))* |  |

#### `M_THRESHOLD_MODE`

Inquires the threshold mode of the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_THRESHOLD_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_USE_MAGNITUDE_TARGET`

Inquires whether to use the gradient magnitude or to use extracted edges when searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_USE_MAGNITUDE_TARGET`](Reference/agm/MagmControl.md))* |  |

---

### `Train AGM context ID with a composite-definition model`

Specifies a train AGM context, allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_TRAIN`](../../Reference/agm/MagmAlloc.md) and used in [`MagmTrain`](../../Reference/agm/MagmTrain.md) operations. It must contain a composite-definition model.

#### `M_EMPTY_REGIONS_MODE`

Inquires whether the regions of the model appearances that do not contain edges are considered when training the composite-definition model.

| Value | Description |
| --- | --- |
| *(see [`M_EMPTY_REGIONS_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_MODEL_PRECISION`

Inquires the percentage of the trained composite-definition model's edgels to use when searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_MODEL_PRECISION`](Reference/agm/MagmControl.md))* |  |

#### `M_NEGATIVE_LABELS_MAX_OVERLAP`

Inquires the maximum allowable overlap between two automatically generated negative (red) rectangles.

| Value | Description |
| --- | --- |
| *(see [`M_NEGATIVE_LABELS_MAX_OVERLAP`](Reference/agm/MagmControl.md))* |  |

#### `M_NEGATIVE_LABELS_MODE`

Inquires whether to automatically label negative model locations in the training images or to use manually defined labels.

| Value | Description |
| --- | --- |
| *(see [`M_NEGATIVE_LABELS_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_NEGATIVE_LABELS_POSITIVE_MAX_OVERLAP`

Inquires the maximum allowable overlap between an automatically generated negative (red) rectangle and a positive (blue) rectangle.

| Value | Description |
| --- | --- |
| *(see [`M_NEGATIVE_LABELS_POSITIVE_MAX_OVERLAP`](Reference/agm/MagmControl.md))* |  |

#### `M_REALIGN_STRENGTH`

Inquires the strength with which to displace the blue rectangles, which define the positive model locations in the training images, to better align them.

| Value | Description |
| --- | --- |
| *(see [`M_REALIGN_STRENGTH`](Reference/agm/MagmControl.md))* |  |

#### `M_SMOOTHNESS`

Inquires the degree of smoothness (noise reduction) applied to the training images during the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_SMOOTHNESS`](Reference/agm/MagmControl.md))* |  |

#### `M_THRESHOLD_MODE`

Inquires the threshold mode of the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_THRESHOLD_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_TRAIN_DETECTION_ANGLE`

Inquires whether to train the composite-definition model using rotated labeled image regions (red and blue rectangles).

| Value | Description |
| --- | --- |
| *(see [`M_TRAIN_DETECTION_ANGLE`](Reference/agm/MagmControl.md))* |  |

#### `M_TRAIN_LABELS_NOISE_MODE`

Inquires the mode with which to handle substandard labeled image regions when training the composite-definition model.

| Value | Description |
| --- | --- |
| *(see [`M_TRAIN_LABELS_NOISE_MODE`](Reference/agm/MagmControl.md))* |  |

#### `M_USE_MAGNITUDE_TARGET`

Inquires whether to use the gradient magnitude or to use extracted edges when training the composite-definition model and searching for occurrences.

| Value | Description |
| --- | --- |
| *(see [`M_USE_MAGNITUDE_TARGET`](Reference/agm/MagmControl.md))* |  |

### For inquiring about a find or train AGM result buffer

To inquire information about a find or train AGM result buffer, the [`InquireType`](../../Reference/agm/MagmInquire.md) parameter can be set to one of the following values. In this case, you must set the [`Index`](../../Reference/agm/MagmInquire.md) parameter to [`M_GENERAL`](../../Reference/agm/MagmInquire.md) or [`M_DEFAULT`](../../Reference/agm/MagmInquire.md).

---

### `AGM result buffer ID`

Specifies the identifier of a find or train AGM result buffer, allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md).

#### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time that you call [`MagmFind`](../../Reference/agm/MagmFind.md) or [`MagmTrain`](../../Reference/agm/MagmTrain.md).  Although you cannot identify the modification counter's contents, you can compare them throughout your application to know if the result buffer has been altered.

| Value | Description |
| --- | --- |
| *(see [`M_MODIFICATION_COUNT`](Reference/agm/MagmInquire.md))* |  |

#### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the AGM result buffer was allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### Combination Constants — For inquiring about the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get information about the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

#### `M_IS_SET_TO_DEFAULT`

Inquires whether the specified inquire type is set to its default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not set to its default value. |
| `M_TRUE` | Specifies that the inquire type is set to its default value. |

### Combination Constants — To inquire whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — To specify the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_. Note that [`M_TYPE_AIL_ID`](../../Reference/agm/MagmInquire.md) should only be used with [`M_OWNER_SYSTEM`](../../Reference/agm/MagmInquire.md).

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
