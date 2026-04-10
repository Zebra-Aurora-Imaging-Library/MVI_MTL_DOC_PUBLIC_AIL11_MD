---
doctype: Reference
module: met
function: MmetGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetGetResult"
---

# MmetGetResult

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

> Get the specified type of results from an Aurora Imaging Library metrology result buffer.

## Syntax

```c
void MmetGetResult(
    AIL_ID    ResultId,       //in
    AIL_INT   LabelOrIndex,   //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves all results of the specified type from a metrology result buffer, after an [`MmetCalculate`](../../Reference/met/MmetCalculate.md) call. The result entries are ordered by label value.

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

> **Note:** If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MmetControl`](../../Reference/met/MmetControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/met/MmetControl.md) control type set to [`M_PIXEL`](../../Reference/met/MmetControl.md). However, note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/met/MmetControl.md) to [`M_WORLD`](../../Reference/met/MmetControl.md) without specifying a calibrated image in which to calculate the results, [`MmetGetResult`](../../Reference/met/MmetGetResult.md) will generate an error.

All results are returned relative to the output reference frame that you set using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_OUTPUT_FRAME`](../../Reference/met/MmetControl.md).

## Parameters

### `ResultId` *(in, AIL_ID)*

Specifies the identifier of the metrology result buffer from which to get results.

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the feature or geometric tolerance for which to get results. Labels must be greater than 0 while indices can be equal to 0.

*For specifying a context, feature, tolerance, or result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_INDEX` | Specifies the index value of an existing individual feature for which to get results. |
| `M_FEATURE_LABEL` | Specifies the label of an existing individual feature for which to get results. |
| `M_TOLERANCE_INDEX` | Specifies the index value of an existing individual tolerance for which to get results. |
| `M_TOLERANCE_LABEL` | Specifies the label value of an existing individual tolerance for which to get results. |
| `M_ALL_FEATURES` | Specifies to return results about all features. This should be used only with [`M_STATUS`](../../Reference/met/MmetGetResult.md). |
| `M_ALL_TOLERANCES` | Specifies to return results about all tolerances. This should be used only with [`M_STATUS`](../../Reference/met/MmetGetResult.md). |
| `M_CONSTRUCTED_FEATURES` | Specifies to return results about all constructed features. This should be used only with [`M_STATUS`](../../Reference/met/MmetGetResult.md). |
| `M_GENERAL` *(default)* | Specifies that the results relating to the entire metrology context will be returned. |
| `M_GLOBAL_FRAME` | Specifies that information about the global frame of the context will be returned. |
| `M_MEASURED_FEATURES` | Specifies to return results about all measured features. This should be used only with [`M_STATUS`](../../Reference/met/MmetGetResult.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the first element of the array in which to write the requested information.

## Parameter Associations

### For retrieving general results

To retrieve general results, the[`LabelOrIndex`](../../Reference/met/MmetGetResult.md)parameter must be set to [`M_GENERAL`](../../Reference/met/MmetGetResult.md).

---

### `M_NUMBER_OF_CONSTRUCTED_FEATURES`

Retrieves the number of constructed features.

---

### `M_NUMBER_OF_FEATURES`

Retrieves the total number of features. This total includes both measured features and constructed features.

---

### `M_NUMBER_OF_FEATURES_FAIL`

Retrieves the number of features that are not successfully calculated. The status of those features is [`M_FAIL`](../../Reference/met/MmetGetResult.md).

---

### `M_NUMBER_OF_FEATURES_PASS`

Retrieves the number of features that are successfully calculated. The status of those features is [`M_PASS`](../../Reference/met/MmetGetResult.md).

---

### `M_NUMBER_OF_FEATURES_WARNING`

Retrieves the number of features that are calculated with a warning. The status of those features is [`M_WARNING`](../../Reference/met/MmetGetResult.md).

---

### `M_NUMBER_OF_MEASURED_FEATURES`

Retrieves the number of measured features.

---

### `M_NUMBER_OF_TOLERANCES`

Retrieves the number of tolerances.

---

### `M_NUMBER_OF_TOLERANCES_FAIL`

Retrieves the number of tolerances that have the status [`M_FAIL`](../../Reference/met/MmetGetResult.md), which is the resulting status when tolerance values are smaller than [`M_VALUE_MIN`](../../Reference/met/MmetControl.md) or greater than [`M_VALUE_MAX`](../../Reference/met/MmetControl.md).

---

### `M_NUMBER_OF_TOLERANCES_PASS`

Retrieves the number of tolerances that have the status [`M_PASS`](../../Reference/met/MmetGetResult.md), which is the resulting status when it is not [`M_FAIL`](../../Reference/met/MmetGetResult.md) or [`M_WARNING`](../../Reference/met/MmetGetResult.md).

---

### `M_NUMBER_OF_TOLERANCES_WARNING`

Retrieves the number of tolerances that have the status [`M_WARNING`](../../Reference/met/MmetGetResult.md), which is the resulting status when tolerance values are greater or equal to [`M_VALUE_MIN`](../../Reference/met/MmetControl.md) and lower than [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md). It also occurs when tolerance values are lower or equal to [`M_VALUE_MAX`](../../Reference/met/MmetControl.md) and greater than [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md).

---

### `M_TIMEOUT_END`

Retrieves whether the timeout has been reached. You can set the timeout limit using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_TIMEOUT`](../../Reference/mod/MmodControl.md). By default, there is no limit.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the timeout has not been reached. |
| `M_TRUE` | Specifies that the timeout has been reached. |

### For retrieving results for features

To retrieve results for features, the [`LabelOrIndex`](../../Reference/met/MmetGetResult.md)parameter can be set to [`M_GLOBAL_FRAME`](../../Reference/met/MmetGetResult.md) or an existing feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**.

---

### `M_ANGLE`

Retrieves the aperture of an arc or the angle of a segment, a line, or a local frame. The angle is measured in degrees, relative to the output coordinate system specified using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/met/MmetControl.md).

---

### `M_ANGLE_END`

Retrieves the end angle of an arc. The angle is measured in degrees, relative to the output coordinate system specified using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/met/MmetControl.md).

---

### `M_ANGLE_START`

Retrieves the start angle of an arc. The angle is measured in degrees, relative to the output coordinate system specified using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/met/MmetControl.md).

---

### `M_COVERAGE`

Retrieves the portion of the feature that was covered by the fitted edgels, as a percentage. If the fitted edgels of the feature completely cover all of the possible edgels of the feature (before the fit), [`M_COVERAGE`](../../Reference/met/MmetGetResult.md) returns 100%.  You can retrieve [`M_COVERAGE`](../../Reference/met/MmetGetResult.md) for physically measured or constructed arcs, circles, and segments. For features that have not been fit, the coverage is 100%.

---

### `M_LENGTH`

Retrieves the perimeter of a circle or the length of a segment, edgel, or arc feature.

---

### `M_LINE_A`

Retrieves the coefficient _A_ of the equation for a constructed line.  The line equation is: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

---

### `M_LINE_B`

Retrieves the coefficient _B_ of the equation for a constructed line.  The line equation is: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

---

### `M_LINE_C`

Retrieves the coefficient _C_ of the equation for a constructed line.  The line equation is: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

---

### `M_NUMBER`

Retrieves the number of subfeatures in a multiple feature or the number of edgels in an edgel feature.

---

### `M_NUMBER_OF_GROUPS`

Retrieves the number of groups in the group edgels feature ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_GROUP_EDGELS`](../../Reference/met/MmetAddFeature.md)).

---

### `M_POSITION_END_X`

Retrieves the X-coordinate of the ending point of a segment or arc.

---

### `M_POSITION_END_Y`

Retrieves the Y-coordinate of the ending point of a segment or arc.

---

### `M_POSITION_START_X`

Retrieves the X-coordinate of the starting point of a segment or arc.

---

### `M_POSITION_START_Y`

Retrieves the Y-coordinate of the starting point of a segment or arc.

---

### `M_POSITION_X`

Retrieves the X-coordinate of a point or edgel, the X-coordinate of the center of a circle or arc, or the X-coordinate of the origin of a local frame. For measured points and edgels, you must retrieve [`M_NUMBER`](../../Reference/met/MmetGetResult.md) to allocate the appropriate size of the array.

---

### `M_POSITION_Y`

Retrieves the Y-coordinate of a point or edgel, the Y-coordinate of the center of a circle or arc, or the Y-coordinate of the origin of a local frame. For measured points and edgels, you must retrieve [`M_NUMBER`](../../Reference/met/MmetGetResult.md) to allocate the appropriate size of the array.

---

### `M_RADIUS`

Retrieves the radius of a circle or arc feature.

### For retrieving results for tolerances

To retrieve tolerance results, the [`LabelOrIndex`](../../Reference/met/MmetGetResult.md) parameter can be set to an existing tolerance label or index using **M_TOLERANCE_LABEL()** or using **M_TOLERANCE_INDEX()**.

---

### `M_TOLERANCE_VALUE`

Retrieves the calculated geometric tolerance value.

### For retrieving results for features or tolerances

To retrieve results for features, the [`LabelOrIndex`](../../Reference/met/MmetGetResult.md) parameter can be set to [`M_GLOBAL_FRAME`](../../Reference/met/MmetGetResult.md), or an existing feature label or index, using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**, unless otherwise specified. To retrieve tolerance results, the [`LabelOrIndex`](../../Reference/met/MmetGetResult.md) parameter can be set to an existing tolerance label or index, using **M_TOLERANCE_LABEL()** or **M_TOLERANCE_INDEX()**, unless otherwise specified.

---

### `M_LABEL_VALUE`

Retrieves the label of a specific feature or a specific tolerance.

---

### `M_STATUS`

Retrieves the status of the calculation of features and tolerances.  The feature's status is determined by whether Aurora Imaging Library was able to successfully calculate it. Some features might be mathematically valid, but their unconventional design can prove problematic. For example, you can theoretically build a valid segment feature using two points at the same location; however, you could not create an extended intersection from such a segment. Aurora Imaging Library considers such a feature to be in a degenerated state and returns its status as [`M_WARNING`](../../Reference/met/MmetGetResult.md). Features that are impossible to construct will have an [`M_FAIL`](../../Reference/met/MmetGetResult.md) status; for example, you cannot build a line using two points at the same location (more information is needed to fulfill the requirement that a line extends indefinitely along a directional path).  The geometric tolerance's status is determined by whether it ([`M_TOLERANCE_VALUE`](../../Reference/met/MmetGetResult.md)) adheres to the thresholds set using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_VALUE_MAX`](../../Reference/met/MmetControl.md), [`M_VALUE_MIN`](../../Reference/met/MmetControl.md), [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md), and [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md). The following is an example of (non-zero) minimum and maximum tolerance value and warning value thresholds:  *[Image: metro_tolerance_pass_warning_fail.png]*  To get a global status (the status of multiple statuses), set the [`LabelOrIndex`](../../Reference/met/MmetGetResult.md) parameter to [`M_GENERAL`](../../Reference/met/MmetGetResult.md) (to get the global status of all features and tolerances), [`M_ALL_FEATURES`](../../Reference/met/MmetGetResult.md) (to get the global status of all features), [`M_CONSTRUCTED_FEATURES`](../../Reference/met/MmetGetResult.md) (to get the global status of all constructed features), [`M_MEASURED_FEATURES`](../../Reference/met/MmetGetResult.md) (to get the global status of all measured features), or [`M_ALL_TOLERANCES`](../../Reference/met/MmetGetResult.md) (to get the global status of all tolerances). When retrieving a global status, [`M_STATUS`](../../Reference/met/MmetGetResult.md) only returns [`M_PASS`](../../Reference/met/MmetGetResult.md) when each individual status is [`M_PASS`](../../Reference/met/MmetGetResult.md). If any individual status is [`M_FAIL`](../../Reference/met/MmetGetResult.md), [`M_STATUS`](../../Reference/met/MmetGetResult.md) returns [`M_FAIL`](../../Reference/met/MmetGetResult.md). If there is no individual status with [`M_FAIL`](../../Reference/met/MmetGetResult.md), but one or more with [`M_WARNING`](../../Reference/met/MmetGetResult.md), then [`M_STATUS`](../../Reference/met/MmetGetResult.md) returns [`M_WARNING`](../../Reference/met/MmetGetResult.md).

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies that the feature was not calculated successfully, and/or the tolerance has not passed all restrictions. |
| `M_PASS` | Specifies that the feature was calculated successfully, and/or the geometric tolerance has passed all restrictions. |
| `M_WARNING` | Specifies that the feature was calculated, or the geometric tolerance has passed all restrictions, with a warning. Warnings can be used to advise you that a feature is in a degenerated state, or that a tolerance is at its limit, and is in danger of failing. |

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

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

An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).
