---
doctype: Reference
module: mod
function: MmodGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / mod / MmodGetResult"
---

# MmodGetResult

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

> Get the specified type of result(s) from a Model Finder result buffer.

## Syntax

```c
void MmodGetResult(
    AIL_ID    ResultId,       //in
    AIL_INT   ResultIndex,    //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the result(s) of the specified type from a Model Finder result buffer. Results are only available after calling [`MmodFind`](../../Reference/mod/MmodFind.md).

The result entries will be ordered by match score, starting with the highest score (regardless of the result type requested). When searching for multiple models, results are not sorted according to which model occurrence was found; results are still sorted according to score. The [`M_INDEX`](../../Reference/mod/MmodGetResult.md) result type returns the index of the model associated with the Model Finder result; whereas the [`M_USER_LABEL`](../../Reference/mod/MmodGetResult.md) result type returns its user label.

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

> **Note:** If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MmodControl`](../../Reference/mod/MmodControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/mod/MmodControl.md) control type set to [`M_PIXEL`](../../Reference/mod/MmodControl.md). However, note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/mod/MmodControl.md) to [`M_WORLD`](../../Reference/mod/MmodControl.md) without specifying a calibrated image in which to calculate the results, [`MmodGetResult`](../../Reference/mod/MmodGetResult.md) will generate an error.

If the target is not calibrated and the nominal search aspect ratio is not set to 1 ([`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ASPECT_RATIO`](../../Reference/mod/MmodControl.md)), results are returned for the corrected target coordinate system (whereby the target pixel width is equal to the pixel height).

## Parameters

### `ResultId` *(in, AIL_ID)*

Specifies the identifier of the Model Finder result buffer from which to retrieve results.

### `ResultIndex` *(in, AIL_INT)*

Specifies where to get results. This parameter can be set to one of the following:

*For specifying where to get results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ALL`](../../Reference/mod/MmodGetResult.md) for result types that apply to one or all occurrences. Same as [`M_GENERAL`](../../Reference/mod/MmodGetResult.md) for general result types. |
| `M_ALL` | Retrieves the results of the specified type for all model occurrences found. |
| `M_GENERAL` | Retrieves a result relating to the entire Model Finder context. |
| `0 <= Value < M_NUMBER` | Specifies the result index. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address in which to write results.

## Parameter Associations

### For a general result

When retrieving a general result, the [`ResultType`](../../Reference/mod/MmodGetResult.md) parameter can be set to one of the following values.

---

### `M_CONTEXT_ID`

Retrieves the identifier of the Model Finder context used by [`MmodFind`](../../Reference/mod/MmodFind.md) to obtain the results in the result buffer. Note that this identifier might not be valid if the Model Finder context has been freed using [`MmodFree`](../../Reference/mod/MmodFree.md).

---

### `M_MOD_DEFINE_COMPATIBLE`

Retrieves whether the result can be used to define a model.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the result cannot be used to define a model. |
| `M_TRUE` | Specifies that the result can be used to define a model. |

---

### `M_NUMBER`

Retrieves the number of occurrences found (all models).

---

### `M_TIMEOUT_END`

Retrieves whether the timeout has been reached. You can set the timeout limit using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_TIMEOUT`](../../Reference/mod/MmodControl.md). By default, there is no limit.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the timeout has not been reached. |
| `M_TRUE` | Specifies that the timeout has been reached. |

### For an individual occurrence, all occurrences, or a general result

When retrieving a general result, a result for an individual occurrence, or a result for all occurrences ([`M_ALL`](../../Reference/mod/MmodGetResult.md)), the [`ResultType`](../../Reference/mod/MmodGetResult.md) parameter can be set to the following value.

---

### `M_NUMBER_OF_CHAINED_EDGELS`

Retrieves the total number of chained edgels.  For a general result, [`M_NUMBER_OF_CHAINED_EDGELS`](../../Reference/mod/MmodGetResult.md) returns the number of chained edgels in the target.  For a specified occurrence, [`M_NUMBER_OF_CHAINED_EDGELS`](../../Reference/mod/MmodGetResult.md) returns the chained edgels in the occurrence. When used with [`M_ALL`](../../Reference/mod/MmodGetResult.md), an array of results is returned, with one result per occurrence.  You can use [`M_NUMBER_OF_CHAINED_EDGELS`](../../Reference/mod/MmodGetResult.md) to determine the required array size for the [`M_CHAIN_...`](../../Reference/mod/MmodGetResult.md) result types.

### For an individual occurrence or a general result

When retrieving a general result or result(s) for a single occurrence, the [`ResultType`](../../Reference/mod/MmodGetResult.md) parameter can be set to one of the following values.

---

### `M_CHAIN_ANGLE`

Retrieves the angle values of each edgel in all chains in the entire target or in the model occurrence.  The angle is always returned in uncalibrated pixel units and is measured counter-clockwise in the image, from the positive X-axis towards the negative Y-axis of the image.  Possible values range from 0 to 360 degrees and mapped in the range of 0 to 255 (that is, an 8-bit range).

---

### `M_CHAIN_INDEX`

Retrieves the chain index of each edgel in all chains in the entire target, or in the model occurrence.  The first chain in the target is identified as index 1, with increasing indices for subsequent chains.

---

### `M_CHAIN_X`

Retrieves the X-coordinate of each edgel in all chains in the entire target or in the model occurrence.

---

### `M_CHAIN_Y`

Retrieves the Y-coordinate of each edgel in all chains in the entire target or in the model occurrence.

### Combination Constants — For specifying which edges in the occurrence to consider

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the edges for which to retrieve results.

| Value | Description |
| --- | --- |
| `M_MODEL` | Retrieves the edge information of the model transformed at the occurrence position. |
| `M_TARGET` | Retrieves the edge information of the target in the region of the occurrence. When using [`M_TARGET`](../../Reference/mod/MmodGetResult.md), you must not set the [`ResultIndex`](../../Reference/mod/MmodGetResult.md) parameter to [`M_GENERAL`](../../Reference/mod/MmodGetResult.md). |

### For a result of a single occurrence or all occurrences

When retrieving a result for a single occurrence or for all occurrences, the [`ResultType`](../../Reference/mod/MmodGetResult.md) parameter can be set to one of the following values.

---

### `M_ANGLE`

Retrieves the angle of the occurrence's reference axis (specified for the model using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodControl.md)), relative to the output coordinate system specified using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/mod/MmodControl.md).  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).  For an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer, the reference axis is the principal axis of the ellipse (its width), unless the reference axis is modified using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodControl.md) prior to calling [`MmodFind`](../../Reference/mod/MmodFind.md).  For an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer, the orientation of the segment is dependent on its gradient, which is set using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SEGMENT_CONSISTENT_GRADIENT`](../../Reference/mod/MmodControl.md). The direction of the segment is orthogonal to its gradient.

---

### `M_ASPECT_RATIO`

Retrieves the found aspect ratio of all occurrences.  For an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder result buffer, the returned aspect ratio is defined as being the ratio of the occurrence's aspect ratio divided by the model's.  This result type is only available for an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAllocResult.md), [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md), or [`M_DEFAULT`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

---

### `M_BOTTOM_LEFT_X`

Retrieves the X-coordinate of the bottom-left corner of the rectangle.

---

### `M_BOTTOM_LEFT_Y`

Retrieves the Y-coordinate of the bottom-left corner of the rectangle.

---

### `M_BOTTOM_RIGHT_X`

Retrieves the X-coordinate of the bottom-right corner of the rectangle.

---

### `M_BOTTOM_RIGHT_Y`

Retrieves the Y-coordinate of the bottom-right corner of the rectangle.

---

### `M_CENTER_X`

Retrieves the X-coordinate of the center of an occurrence of a circle, ellipse, rectangle, or segment model.  This result type is only supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

---

### `M_CENTER_Y`

Retrieves the Y-coordinate of the center of an occurrence of a circle, ellipse, rectangle, or segment model.  This result type is only supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

---

### `M_END_POS_X`

Retrieves the X-coordinate of the end point of an occurrence of a segment.

---

### `M_END_POS_Y`

Retrieves the Y-coordinate of the end point of an occurrence of a segment.

---

### `M_FIT_ERROR`

Retrieves the fit error as the average quadratic distance, in pixels or calibrated units, between the edges in the occurrence and the corresponding edges in the model. A perfect fit is represented as 0.0.

---

### `M_INDEX`

Retrieves the index of the model that was found.

---

### `M_MODEL_COVERAGE`

Retrieves the model coverage of the occurrence. The model coverage is the percentage of the total length of the model's active edges found in the occurrence. 100% indicates that for every active edge in the model, a corresponding edge was found in the occurrence.  Note that using a weighted mask with a model affects the calculation of the model coverage. To view the modified equation, refer to [Determining what is a match](../../UserGuide/C08_Geometric_Model_Finder/S09_Determining_what_is_a_match.md).

---

### `M_POLARITY`

Retrieves the polarity of the occurrence.

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies that the occurrences are a mixture of polarities. For more information, see [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_POLARITY`](../../Reference/mod/MmodControl.md) and [`M_ANY`](../../Reference/mod/MmodControl.md). |
| `M_REVERSE` | Specifies that the polarity of occurrences is the reverse of that of the model. For more information, see [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_POLARITY`](../../Reference/mod/MmodControl.md) and [`M_REVERSE`](../../Reference/mod/MmodControl.md). |
| `M_SAME` | Specifies that the polarity of occurrences is the same as that of the model. For more information, see [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_POLARITY`](../../Reference/mod/MmodControl.md) and [`M_SAME`](../../Reference/mod/MmodControl.md). |

---

### `M_POSITION_X`

Retrieves the X-coordinate of the occurrence. This is the X-position of the model's reference axis transformed at the occurrence ([`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_REFERENCE_X`](../../Reference/mod/MmodInquire.md)).

---

### `M_POSITION_Y`

Retrieves the Y-coordinate of the occurrence. This is the Y-position of the model's reference axis transformed at the occurrence ([`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_REFERENCE_Y`](../../Reference/mod/MmodInquire.md)).

---

### `M_SCALE`

Retrieves the scale of the occurrence. For an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAllocResult.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer, the returned scale is defined as the occurrence's width divided by the model's width. For an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer, the returned scale is defined as the occurrence's length divided by the model's length.

---

### `M_SCORE`

Retrieves the score of the occurrence (as a percentage). The score is calculated as follows:  `Score = Model coverage x (1 - Fit error weighting factor * Normalized fit error)`  > **Note:** Note: the normalized fit error is the fit error ([`M_FIT_ERROR`](../../Reference/mod/MmodGetResult.md)) converted to a number between 0.0-1.0 for an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, while for an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the normalized fit error is influenced by the value of [`M_SAGITTA_TOLERANCE`](../../Reference/mod/MmodControl.md) and is not limited to being between 0.0 and 1.0. For an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the normalized fit error is influenced by the value of [`M_DEVIATION_TOLERANCE`](../../Reference/mod/MmodControl.md) and is not limited to being between 0.0 and 1.0. For all Model Finder context types, the terms within the square brackets result in a number between 0.0 and 1.0.

---

### `M_SCORE_FIT`

Retrieves the fit score of the occurrence (as a percentage). The score is calculated as follows:  `Fit Score = 1 - Normalized fit error`  This result type is only supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. For other types of models see the target score ([`M_SCORE_TARGET`](../../Reference/mod/MmodGetResult.md)).

---

### `M_SCORE_TARGET`

Retrieves the target score of the occurrence. The target score is calculated as follows:  `Target Score = Target coverage x (1 - Fit error weighting factor * Normalized fit error)`  Note that the normalized fit error ([`M_FIT_ERROR`](../../Reference/mod/MmodGetResult.md)) is the fit error converted to a number between 0.0 and 1.0.

---

### `M_SEGMENT_CONSISTENT_GRADIENT`

Retrieves whether the gradients of the segment model occurrences are consistent.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the occurrences do not have consistent gradients. |
| `M_TRUE` | Specifies that the occurrences have consistent gradients. |

---

### `M_START_POS_X`

Retrieves the X-coordinate of the start point of an occurrence of a segment.

---

### `M_START_POS_Y`

Retrieves the Y-coordinate of the start point of an occurrence of a segment.

---

### `M_TARGET_COVERAGE`

Retrieves the target coverage of the model. The target coverage is a percentage of the total length of edges present within the occurrence's bounding box, corresponding to the model's active edges. Lower scores indicate that features or edges found in the target (result occurrence) are not present in the model.  Note that using a weighted mask with a model affects the calculation of the target coverage. To view the modified equation, refer to [Determining what is a match](../../UserGuide/C08_Geometric_Model_Finder/S09_Determining_what_is_a_match.md).

---

### `M_TOP_LEFT_X`

Retrieves the X-coordinate of the top-left corner of the rectangle.

---

### `M_TOP_LEFT_Y`

Retrieves the Y-coordinate of the top-left corner of the rectangle.

---

### `M_TOP_RIGHT_X`

Retrieves the X-coordinate of the top-right corner of the rectangle.

---

### `M_TOP_RIGHT_Y`

Retrieves the Y-coordinate of the top-right corner of the rectangle.

---

### `M_USER_LABEL`

Retrieves the user label of the model that was found.

| Value | Description |
| --- | --- |
| `M_NO_LABEL` *(default)* | Specifies that no user label is associated with the model. |
| `Value` | Specifies the user label of the model. |

### For a result of a single occurrence or all occurrences (with constraints)

When retrieving a result for a single occurrence or for all occurrences, the [`ResultType`](../../Reference/mod/MmodGetResult.md) parameter can be set to one of the following values. The following values are not available for all model types. For one of these values to be available for a result occurrence, the result occurrence must be of the appropriate type. Use [`M_AVAILABLE`](../../Reference/mod/MmodGetResult.md) to ensure that the value can be used with the occurrence. If the [`ResultIndex`](../../Reference/mod/MmodGetResult.md) parameter is set to [`M_ALL`](../../Reference/mod/MmodGetResult.md), all the occurrences must support the result type, otherwise an error is generated.

---

### `M_CORNER_RADIUS`

Retrieves the radius used to round the corners of the models. This control type is only valid for models of type [`M_CROSS`](../../Reference/mod/MmodDefine.md), [`M_DIAMOND`](../../Reference/mod/MmodDefine.md), [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md), [`M_SQUARE`](../../Reference/mod/MmodDefine.md), and [`M_TRIANGLE`](../../Reference/mod/MmodDefine.md).

---

### `M_HEIGHT`

Retrieves the height of the shape. This result type is only available for [`M_CROSS`](../../Reference/mod/MmodDefine.md), [`M_DIAMOND`](../../Reference/mod/MmodDefine.md), [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md), [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md) and [`M_TRIANGLE`](../../Reference/mod/MmodDefine.md).

---

### `M_HORIZONTAL_THICKNESS`

Retrieves the horizontal thickness of the cross. This result type is only available for [`M_CROSS`](../../Reference/mod/MmodDefine.md).

---

### `M_INNER_RADIUS`

Retrieves the inner radius of the ring. This result type is only available for [`M_RING`](../../Reference/mod/MmodDefine.md).

---

### `M_LENGTH`

Retrieves the length of the segment. This result type is only available for [`M_SEGMENT`](../../Reference/mod/MmodDefine.md).

---

### `M_OUTER_RADIUS`

Retrieves the outer radius of the ring. This result type is only available for [`M_RING`](../../Reference/mod/MmodDefine.md).

---

### `M_RADIUS`

Retrieves the radius of the circle. This result type is only available for [`M_CIRCLE`](../../Reference/mod/MmodDefine.md).

---

### `M_VERTICAL_THICKNESS`

Retrieves the vertical thickness of the cross. This result type is only available for [`M_CROSS`](../../Reference/mod/MmodDefine.md).

---

### `M_WIDTH`

Retrieves the width of the shape. This result type is only available for [`M_CROSS`](../../Reference/mod/MmodDefine.md), [`M_DIAMOND`](../../Reference/mod/MmodDefine.md), [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md), [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md), [`M_SQUARE`](../../Reference/mod/MmodDefine.md) and [`M_TRIANGLE`](../../Reference/mod/MmodDefine.md).

### For retrieving a transformation coefficient

To retrieve a transformation coefficient for a single occurrence or for all occurrences, the [`ResultType`](../../Reference/mod/MmodGetResult.md) parameter can be set to one of the following values. These coefficients allow you to convert coordinates in the model coordinate system to the corresponding coordinates in the target coordinate system for that occurrence (or vice versa). These coefficients handle variations in scale, translation, and angle. If the model is calibrated, these coefficients are given for the real world.  Use the following equations:  `_x_ <sub>d</sub> = _A_ _x_ <sub>s</sub> + _B_ _y_ <sub>s</sub> + _C_`  `_y_ <sub>d</sub> = _E_ _x_ <sub>s</sub> + _F_ _y_ <sub>s</sub> + _D_`  For models that do not have an [`M_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) result type, note the following:  `_E_ = -_B_`  `_F_ = _A_`  where `A, B, C, D, E, and F` are the transformation coefficients (forward or reverse); `x <sub>s</sub> and y<sub> s</sub>` specify the source coordinates (with respect to the origin of the model coordinate system for a forward transformation or target coordinate system for a reverse transformation); and, `x <sub>d</sub> and y<sub> d</sub>` are the destination coordinates (with respect to the origin of the target coordinate system for a forward transformation or model coordinate system for a reverse transformation).

---

### `M_A_FORWARD`

Retrieves the forward transformation coefficient A for the occurrence.

---

### `M_A_REVERSE`

Retrieves the reverse transformation coefficient A for the occurrence.

---

### `M_B_FORWARD`

Retrieves the forward transformation coefficient B for the occurrence.

---

### `M_B_REVERSE`

Retrieves the reverse transformation coefficient B for the occurrence.

---

### `M_C_FORWARD`

Retrieves the forward transformation coefficient C for the occurrence.

---

### `M_C_REVERSE`

Retrieves the reverse transformation coefficient C for the occurrence.

---

### `M_D_FORWARD`

Retrieves the forward transformation coefficient D for the occurrence.

---

### `M_D_REVERSE`

Retrieves the reverse transformation coefficient D for the occurrence.

---

### `M_E_FORWARD`

Retrieves the forward transformation coefficient E for the occurrence.

---

### `M_E_REVERSE`

Retrieves the reverse transformation coefficient E for the occurrence.

---

### `M_F_FORWARD`

Retrieves the forward transformation coefficient F for the occurrence.

---

### `M_F_REVERSE`

Retrieves the reverse transformation coefficient F for the occurrence.

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify whether a result is available.

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

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

This result type is not available if the target edges were not saved in the context ([`M_SAVE_TARGET_EDGES`](../../Reference/mod/MmodControl.md) must be set to [`M_ENABLE`](../../Reference/mod/MmodControl.md)).

When retrieving a general result, this result type is not supported if the Model Finder context used was an [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of context.

This result type is not available for an [`M_SHAPE_...`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

This result type is only available for an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

This result type is only available for an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.
