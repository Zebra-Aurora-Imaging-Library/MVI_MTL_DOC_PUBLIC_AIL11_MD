---
doctype: Reference
module: agm
function: MagmGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmGetResult"
---

# MagmGetResult

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

> Get results from a find or train AGM result buffer.

## Syntax

```c
AIL_DOUBLE MagmGetResult(
    AIL_ID    ResultAgmId,    //in
    AIL_INT64 Index,          //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the results of the specified type for the specified occurrence(s) from a find AGM result buffer, or for the specified model or specified rectangle from a train AGM result buffer. For find AGM result buffers, results are available after calling [`MagmFind`](../../Reference/agm/MagmFind.md). For train AGM result buffers, results are available after calling [`MagmTrain`](../../Reference/agm/MagmTrain.md).

## Parameters

### `ResultAgmId` *(in, AIL_ID)*

Specifies the identifier of the AGM result buffer from which to retrieve results. The result buffer must have been previously allocated on the system using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_FIND_RESULT`](../../Reference/agm/MagmAllocResult.md) or [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md).

### `Index` *(in, AIL_INT64)*

Specifies where to get results.

*For specifying where to get results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AGM_MODEL_INDEX` | Specifies to retrieve results for the model in the train AGM result buffer. |
| `M_AGM_RECTANGLE_NEG_INDEX` | Specifies to retrieve results for a specific negative (red) rectangle in the train AGM result buffer. |
| `M_AGM_RECTANGLE_POS_INDEX` | Specifies to retrieve results for a specific positive (blue) rectangle in the train AGM result buffer. |
| `M_ALL` | Specifies to retrieve results for all model occurrences in the find AGM result buffer. |
| `M_GENERAL` *(default)* | Specifies to retrieve results relating to the entire find or train AGM result buffer. |
| `0 <= OccIndex < M_NUMBER` | Specifies the index of the occurrence in the find AGM result buffer for which to retrieve results. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the array in which to write results.

## Parameter Associations

### For retrieving general results from a find or train AGM result buffer

To retrieve general results from a find or train AGM result buffer, set [`ResultType`](../../Reference/agm/MagmGetResult.md) to one of the following values. In this case, set the [`Index`](../../Reference/agm/MagmGetResult.md) parameter to [`M_GENERAL`](../../Reference/agm/MagmGetResult.md).

---

### `Find AGM result buffer ID for general results`

Specifies a find AGM result buffer, allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_FIND_RESULT`](../../Reference/agm/MagmAllocResult.md), and used to store [`MagmFind`](../../Reference/agm/MagmFind.md) results.

#### `M_NUMBER`

Retrieves the number of occurrences found.

#### `M_STATUS`

Retrieves the status of the find operation.

| Value | Description |
| --- | --- |
| `M_CALCULATE_NOT_PERFORMED` | Specifies that a find operation has not been performed. This is the initial status of a find AGM result buffer. |
| `M_COMPLETE` | Specifies that the find operation completed. |
| `M_CURRENTLY_CALCULATING` | Specifies that the find operation is ongoing. You can only get this status if you are retrieving it from another thread. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected error occurred. |
| `M_NOT_ENOUGH_MEMORY` | Specifies that the operation was not completed because of a lack of available memory. |

---

### `Train AGM result buffer ID for general results`

Specifies a train AGM result buffer, allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md), and used to store [`MagmTrain`](../../Reference/agm/MagmTrain.md) results.

#### `M_NUMBER`

Retrieves the number of trained composite-definition models.  Note that training results in only 0 or 1 trained composite-definition models.

#### `M_NUMBER_NEG_RECTANGLES`

Retrieves the number of negative (red) rectangles across all training images.

#### `M_NUMBER_POS_RECTANGLES`

Retrieves the number of positive (blue) rectangles across all training images.

#### `M_STATUS`

Retrieves the status of the train operation.

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the train operation completed.  Note that an [`M_COMPLETE`](../../Reference/agm/MagmGetResult.md) result from a train AGM result buffer does not mean that training attempts on the model were successful. To check the status of an individual model, set the [`Index`](../../Reference/agm/MagmGetResult.md) parameter to the required model's index. |
| `M_CURRENTLY_TRAINING` | Specifies that the train operation is ongoing. You can only get this status if you are retrieving it from another thread. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected error occurred. |
| `M_NOT_ENOUGH_MEMORY` | Specifies that the operation was not completed because of a lack of available memory. |
| `M_STOPPED_BY_REQUEST` | Specifies that the train operation was stopped from another thread using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_STOP_TRAIN`](../../Reference/agm/MagmControl.md). |
| `M_TRAINING_NOT_PERFORMED` | Specifies that a train operation has not been performed. This is the initial status of a train AGM result buffer. |

### For retrieving a result of a single occurrence or all occurrences from a find AGM result buffer

To retrieve results for a single occurrence or for all occurrences from a find AGM result buffer, set [`ResultType`](../../Reference/agm/MagmGetResult.md) to one of the following values. In this case, set the [`Index`](../../Reference/agm/MagmGetResult.md) parameter to [`M_ALL`](../../Reference/agm/MagmGetResult.md) or the index of the occurrence.

---

### `Find AGM result buffer ID for occurrence-specific results`

Specifies a find AGM result buffer, allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_FIND_RESULT`](../../Reference/agm/MagmAllocResult.md), and used to store [`MagmFind`](../../Reference/agm/MagmFind.md) results.

#### `M_ANGLE`

Retrieves the angle of the occurrence's reference axis, relative to the pixel coordinate system.  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise.

#### `M_POLARITY`

Retrieves the polarity of the occurrence.  Note that occurrences are only found if they match the polarity set using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_POLARITY`](../../Reference/agm/MagmControl.md).

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies that [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_POLARITY`](../../Reference/agm/MagmControl.md) is set to [`M_ANY`](../../Reference/agm/MagmControl.md) and the polarity of the occurrence is not determined. It could be the same or reverse of that of the model, a mixture of polarities, or no polarity in the case of an [`M_DXF_FILE`](../../Reference/agm/MagmDefine.md) model occurrence. |
| `M_REVERSE` | Specifies that [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_POLARITY`](../../Reference/agm/MagmControl.md) is set to [`M_REVERSE`](../../Reference/agm/MagmControl.md) and the polarity of the occurrence is the reverse of that of the model. |
| `M_SAME` | Specifies that [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_POLARITY`](../../Reference/agm/MagmControl.md) is set to [`M_SAME`](../../Reference/agm/MagmControl.md) and the polarity of the occurrence is the same as that of the model. |

#### `M_POSITION_X`

Retrieves the X-coordinate of the occurrence. This is the X-position of the model's reference axis transformed at the occurrence. Note, the origin of the model's reference axis is the center of the model.

#### `M_POSITION_Y`

Retrieves the Y-coordinate of the occurrence. This is the Y-position of the model's reference axis transformed at the occurrence. Note, the origin of the model's reference axis is the center of the model.

#### `M_SCALE`

Retrieves the scale of the occurrence. Since the returned scale is uniform, it can be defined as the occurrence's size divided by the model's size.  Note that only occurrences with small scale differences can be found, which means the returned scale will always be close to 1.0.

#### `M_SCORE_COVERAGE`

Retrieves the coverage score of the occurrence, where the coverage score is the percentage of the total length of the model's edges found in the occurrence. 100% indicates that for each of the model's edges, a corresponding edge was found in the occurrence.  Note, a model's edge corresponds to an edge in the occurrence if the distance between them is less than or equal to [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/agm/MagmControl.md).

#### `M_SCORE_DETECTION`

Retrieves the detection score of the occurrence, where the detection score represents the likelihood that the found occurrence is a true occurrence.

#### `M_SCORE_FIT`

Retrieves the fit score of the occurrence, where the fit score is a measure of the correlation of the edges in the model to their corresponding edges in the occurrence. The score is calculated as follows:  `Fit Score = 1 - Normalized fit error`  The fit error is the average distance, in pixels, between the edges in the model and their corresponding edges in the occurrence.  Note, only those model edges that have a corresponding edge in the occurrence are used in the fit score calculation. A model's edge has a corresponding edge in the occurrence if the distance between them is less than or equal to [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/agm/MagmControl.md).

#### `M_SCORE_FIT_OVERALL`

Retrieves the overall fit score of the occurrence, where the overall fit score is a measure of the correlation of all edges in the model to those of the occurrence. The score is calculated as follows:  `Overall fit Score = 1 - Normalized overall fit error`  The overall fit error is the average distance, in pixels, between the edges in the model and edges in the occurrence.  Note, all model edges are used in the overall fit score calculation, including those without a corresponding edge in the occurrence.  For composite-definition models, this result type is only available when [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md).

### For retrieving a transformation coefficient from a find AGM result buffer

To retrieve a transformation coefficient for a single occurrence or for all occurrences from a find AGM result buffer, set [`ResultType`](../../Reference/agm/MagmGetResult.md) to one of the following values. In this case, set the [`Index`](../../Reference/agm/MagmGetResult.md) parameter to [`M_ALL`](../../Reference/agm/MagmGetResult.md) or the index of the occurrence.  These coefficients allow you to convert coordinates in the model coordinate system to the corresponding coordinates in the target coordinate system for that occurrence (or vice versa). These coefficients handle variations in translation and angle.  Use the following equations:  `_x_ <sub>d</sub> = _A_ _x_ <sub>s</sub> + _B_ _y_ <sub>s</sub> + _C_`  `_y_ <sub>d</sub> = _E_ _x_ <sub>s</sub> + _F_ _y_ <sub>s</sub> + _D_`  where `A, B, C, D, E, and F` are the transformation coefficients (forward or reverse); `x <sub>s</sub> and y<sub> s</sub>` specify the source coordinates (with respect to the origin of the model coordinate system for a forward transformation or target coordinate system for a reverse transformation); and, `x <sub>d</sub> and y<sub> d</sub>` are the destination coordinates (with respect to the origin of the target coordinate system for a forward transformation or model coordinate system for a reverse transformation).

---

### `Find AGM result buffer ID for transformation coefficient results`

Specifies a find AGM result buffer, allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_FIND_RESULT`](../../Reference/agm/MagmAllocResult.md), and used to store [`MagmFind`](../../Reference/agm/MagmFind.md) results.

#### `M_A_FORWARD`

Retrieves the forward transformation coefficient A for the occurrence.

#### `M_A_REVERSE`

Retrieves the reverse transformation coefficient A for the occurrence.

#### `M_B_FORWARD`

Retrieves the forward transformation coefficient B for the occurrence.

#### `M_B_REVERSE`

Retrieves the reverse transformation coefficient B for the occurrence.

#### `M_C_FORWARD`

Retrieves the forward transformation coefficient C for the occurrence.

#### `M_C_REVERSE`

Retrieves the reverse transformation coefficient C for the occurrence.

#### `M_D_FORWARD`

Retrieves the forward transformation coefficient D for the occurrence.

#### `M_D_REVERSE`

Retrieves the reverse transformation coefficient D for the occurrence.

#### `M_E_FORWARD`

Retrieves the forward transformation coefficient E for the occurrence.

#### `M_E_REVERSE`

Retrieves the reverse transformation coefficient E for the occurrence.

#### `M_F_FORWARD`

Retrieves the forward transformation coefficient F for the occurrence.

#### `M_F_REVERSE`

Retrieves the reverse transformation coefficient F for the occurrence.

### For retrieving rectangle results from a train AGM result buffer

To retrieve results for a positive (blue) or negative (red) rectangle from a train AGM result buffer, set [`ResultType`](../../Reference/agm/MagmGetResult.md) to one of the following values. In this case, set the [`Index`](../../Reference/agm/MagmGetResult.md) parameter to the index of the positive or negative rectangle.

---

### `Train AGM result buffer ID for rectangle-specific results`

Specifies a train AGM result buffer, allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md), and used to store [`MagmTrain`](../../Reference/agm/MagmTrain.md) results.

#### `M_ANGLE`

Retrieves the angle of the rectangle.

#### `M_CENTER_X`

Retrieves the X-coordinate of the center of the rectangle.

#### `M_CENTER_Y`

Retrieves the Y-coordinate of the center of the rectangle.

#### `M_CORNER_TOP_LEFT_X`

Retrieves the X-coordinate of the top-left corner of the rectangle.

#### `M_CORNER_TOP_LEFT_Y`

Retrieves the Y-coordinate of the top-left corner of the rectangle.

#### `M_IMAGE_INDEX`

Retrieves the index of the training image that contains the specified rectangle.

#### `M_RECTANGLE_HEIGHT`

Retrieves the height of the rectangle.

#### `M_RECTANGLE_WIDTH`

Retrieves the width of the rectangle.

### For retrieving model results from a train AGM result buffer

To retrieve results for a model from a train AGM result buffer, set [`ResultType`](../../Reference/agm/MagmGetResult.md) to one of the following values. In this case, set the [`Index`](../../Reference/agm/MagmGetResult.md) parameter to the index of the model.

---

### `Train AGM result buffer ID for model-specific results`

Specifies a train AGM result buffer, allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md), and used to store [`MagmTrain`](../../Reference/agm/MagmTrain.md) results.

#### `M_STATUS`

Retrieves the status of the model.

| Value | Description |
| --- | --- |
| `M_STATUS_TRAIN_AUTO_LABELING_FAILED` | Specifies that the automatic labeling of negative model locations was unsuccessful. In this case, the model could not be trained and is unusable. You cannot copy the failed model to a find AGM context using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md). |
| `M_STATUS_TRAIN_FAILED` | Specifies that the model could not be trained and is unusable. You cannot copy the failed model to a find AGM context using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md). |
| `M_STATUS_TRAIN_OK` | Specifies that the model was successfully trained and is usable in a find AGM context. |

#### `M_TRAIN_DETECTION_ANGLE`

Retrieves whether using rotated labeled image regions (red and blue rectangles) to train the composite-definition model was enabled.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the use of rotated labeled image regions was disabled. |
| `M_ENABLE` | Specifies that the use of rotated labeled image regions was enabled. |

#### `M_TRAINING_CONFIDENCE`

Retrieves the training confidence of the trained composite-definition model. The training confidence represents an estimation of the difference between the detection scores at the positive positions and the detection scores at the negative positions in the training images. The detection score is the likelihood that there is a true occurrence at the position. Note that a good training confidence is 20.0% or higher. The higher the training confidence, the better the trained composite-definition model will be at discriminating true occurrences from false occurrences.  Note that the training confidence should be used to evaluate the trained composite-definition model. If the training confidence is too low, you should adjust your training settings and re-train the composite-definition model.  This result type is only available when [`M_STATUS`](../../Reference/agm/MagmGetResult.md) is [`M_STATUS_TRAIN_OK`](../../Reference/agm/MagmGetResult.md) and is only meaningful when [`M_TRAINING_ERROR`](../../Reference/agm/MagmGetResult.md) is 0.0%.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the training confidence. |

#### `M_TRAINING_ERROR`

Retrieves the percentage of training image labels that were misclassified by the [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation. For example, assume you train the composite-definition model using images that contain 5 positive (blue) rectangles and 5 negative (red) rectangles. If the detection score at 4/5 of the positive positions is greater than or equal to the trained acceptance level and the detection score at 4/5 of the negative positions is correctly below the trained acceptance level, 2/10 positions are misclassified and the training error will be 20.0%.  A training error of 0.0% means that the trained composite-definition model is able to detect all positively labeled positions without incorrectly detecting any negatively labeled positions.  This result type is only available when [`M_STATUS`](../../Reference/agm/MagmGetResult.md) is [`M_STATUS_TRAIN_OK`](../../Reference/agm/MagmGetResult.md).

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

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the requested information, cast to an _AIL_DOUBLE_. If the requested information does not fit into an _AIL_DOUBLE_, this function will return `M_NULL` or truncate the information.
