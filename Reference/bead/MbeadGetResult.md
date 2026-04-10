---
doctype: Reference
module: bead
function: MbeadGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadGetResult"
---

# MbeadGetResult

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

> Get results from a bead result buffer.

## Syntax

```c
void MbeadGetResult(
    AIL_ID    ResultBeadId,  //in
    AIL_INT   LabelOrIndex,  //in
    AIL_INT   ResultIndex,   //in
    AIL_INT64 ResultType,    //in
    void *    UserVarPtr     //out
)
```

## Description

This function retrieves results of the specified type from a bead result buffer. Results are only available after calling [`MbeadVerify`](../../Reference/bead/MbeadVerify.md).

Results for the specified bead template or trained point are based on how they compare with their corresponding measured bead or measured point in the verification's target image. The returned results are ordered by the label value of their template.

If your target image was associated with a camera calibration context, positional and dimensional results are returned with respect to the relative coordinate system of the target image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

## Parameters

### `ResultBeadId` *(in, AIL_ID)*

Specifies the identifier of the bead result buffer from which to get results.

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the label or index of the template for which to get results, or specifies that you are getting general results related to all templates. This parameter must be set to one of the following values:

*For specifying where you are getting results*

| Value | Description |
| --- | --- |
| `M_TEMPLATE_INDEX` | Specifies the index of the template for which to get results. |
| `M_TEMPLATE_LABEL` | Specifies the label of the template for which to get results. |
| `M_GENERAL` | Specifies to get general results related to all templates. |

### `ResultIndex` *(in, AIL_INT)*

Specifies the trained points (one or all) for which to get results or specifies that you are getting general results. This parameter must be set to one of the following values:

*For specifying the trained points for which to get results or for specifying general results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md). |
| `M_ALL` | Specifies to get results for each trained point in the template.

In this case, the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter must be set to **M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**. |
| `M_GENERAL` | Specifies to get general results.

To get general results related to all templates, the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter must be set to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md).

To get general results related to a specific template, the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter must be set to **M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**. |
| `Value` | Specifies the index of the trained point for which to get results. Valid indices range from 0 (the first point) to the total number of trained points in the template minus 1. To get the number of trained points in the template, use [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_NUMBER`](../../Reference/bead/MbeadGetResult.md).

When specifying the index of the trained point, the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter must be set to **M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**. |

### `ResultType` *(in, AIL_INT64)*

Specifies the result to get.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For retrieving the status of results

To get the status of results, the [`ResultType`](../../Reference/bead/MbeadGetResult.md) parameter can be set to the following value.

---

### `M_STATUS`

Retrieves whether results have passed or failed verification.  To retrieve the status of results related to all templates, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) and the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md). In this case, you will get a passing status only if all status results ([`M_STATUS_...`](../../Reference/bead/MbeadGetResult.md)) of all templates have passed verification.  To retrieve the status of results related to a specific template, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter to the label or index of the template and the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md). In this case, you will get a passing status only if the template's corresponding measured bead has passed all of the following: [`M_STATUS_GAP_TOLERANCE`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_GAP_MAX`](../../Reference/bead/MbeadGetResult.md), and [`M_STATUS_SCORE`](../../Reference/bead/MbeadGetResult.md).  To retrieve the status of results related to one or all trained points, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter to the label or index of a template and the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific value or [`M_ALL`](../../Reference/bead/MbeadGetResult.md). In this case, you will get a passing status only if the trained point's corresponding measured point has passed all of the following: [`M_STATUS_SEARCH`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_FOUND`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_OFFSET`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_WIDTH_MAX`](../../Reference/bead/MbeadGetResult.md), [`M_STATUS_WIDTH_MIN`](../../Reference/bead/MbeadGetResult.md), and [`M_STATUS_GAP_MAX`](../../Reference/bead/MbeadGetResult.md).

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies that the results have not passed verification. |
| `M_PASS` | Specifies that the results have passed verification. |

### For retrieving the number of templates or the number of trained points in a template

To retrieve the number of templates or the number of trained points in a template, the [`ResultType`](../../Reference/bead/MbeadGetResult.md) parameter can be set to the following value.

---

### `M_NUMBER`

Retrieves the number of templates or the number of trained points in a template. To retrieve the number of templates, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) and the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameters to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md). To retrieve the number of trained points in a template, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter to the index or label of the template and the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md).

### For retrieving results related to a template (measured bead)

To retrieve results related to a template's corresponding measured bead, the [`ResultType`](../../Reference/bead/MbeadGetResult.md) parameter should be set to one of the following values. In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter to the index or label of the template for which to get results (**M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**) and the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md).

---

### `M_CLOSURE`

Retrieves whether the measured bead is closed.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the measured bead is not closed. |
| `M_TRUE` | Specifies that the measured bead is closed. |

---

### `M_GAP_COVERAGE`

Retrieves the percentage of the total gap in the measured bead, relative to the length of the expected measured bead.

---

### `M_GAP_MAX_LENGTH`

Retrieves the length of the longest uninterrupted chain of trained points that were not found in the bead.

---

### `M_INTENSITY_MAX`

Retrieves the maximum pixel intensity of the measured bead.

---

### `M_INTENSITY_MAX_INDEX`

Retrieves the index of the trained point that corresponds to the maximum intensity of the measured bead.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that Aurora Imaging Library could not establish a maximum pixel intensity. This can occur if, for example, no bead was found in the target image. |
| `Value` | Specifies the index of the trained point that corresponds to the maximum intensity. |

---

### `M_INTENSITY_MIN`

Retrieves the minimum pixel intensity of the measured bead.

---

### `M_INTENSITY_MIN_INDEX`

Retrieves the index of the trained point that corresponds to the minimum intensity of the measured bead.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that Aurora Imaging Library could not establish a minimum pixel intensity. This can occur if, for example, no bead was found in the target image. |
| `Value` | Specifies the index of the trained point that corresponds to the minimum intensity. |

---

### `M_LABEL_VALUE`

Retrieves the label of the template corresponding to the index specified with **M_TEMPLATE_INDEX()**.

---

### `M_NUMBER_FOUND`

Retrieves the number of trained points that have a corresponding measured point in the bead.

---

### `M_OFFSET_MAX`

Retrieves the maximum linear distance between the position of a trained point and the position of its corresponding measured point.

---

### `M_OFFSET_MAX_INDEX`

Retrieves the index of the trained point that represents the maximum offset of the measured bead.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that Aurora Imaging Library could not establish a maximum offset. This can occur if, for example, no bead was found in the target image. |
| `Value > 0` | Specifies the index of the trained point that corresponds to the maximum offset. |

---

### `M_SCORE`

Retrieves the percentage of trained points that have a corresponding measured point with a passing status, relative to the total number of trained points in the template.

---

### `M_STATUS_GAP_TOLERANCE`

Retrieves whether the [`M_GAP_COVERAGE`](../../Reference/bead/MbeadGetResult.md) result has passed or failed the gap tolerance criterion, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_GAP_TOLERANCE`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies that the result is greater than the gap tolerance criterion. |
| `M_PASS` | Specifies that the result is less than or equal to the gap tolerance criterion. |

---

### `M_STATUS_SCORE`

Retrieves whether the [`M_SCORE`](../../Reference/bead/MbeadGetResult.md) result has passed or failed the acceptance criterion, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_ACCEPTANCE`](../../Reference/bead/MbeadControl.md).

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies that the result is greater than the acceptance criterion. |
| `M_PASS` | Specifies that the result is less than or equal to the acceptance criterion. |

---

### `M_WIDTH_AVERAGE`

Retrieves the average width of the measured bead.

---

### `M_WIDTH_MAX`

Retrieves the maximum width of the measured bead.

---

### `M_WIDTH_MAX_INDEX`

Retrieves the index of the trained point that corresponds to the maximum width of the measured bead.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that Aurora Imaging Library could not establish a maximum width. This can occur if, for example, no bead was found in the target image. |
| `Value` | Specifies the index of the trained point that corresponds to the maximum width. |

---

### `M_WIDTH_MIN`

Retrieves the minimum width of the measured bead.

---

### `M_WIDTH_MIN_INDEX`

Retrieves the index of the trained point that corresponds to the minimum width of the measured bead.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that Aurora Imaging Library could not establish a minimum width. This can occur if, for example, no bead was found in the target image. |
| `Value` | Specifies the index of the trained point that corresponds to the minimum width. |

### For retrieving results related to trained points (measured points)

To retrieve results related to a trained point's (one or all) corresponding measured point, the [`ResultType`](../../Reference/bead/MbeadGetResult.md) parameter should be set to one of the following values. In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter to the index or label of the template for which to get results (**M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**) and the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific value or [`M_ALL`](../../Reference/bead/MbeadGetResult.md).

---

### `M_ANGLE`

Retrieves the angle of the measured bead at the position of the measured point.

---

### `M_END_POS_X`

Retrieves the X-coordinate of the right edge of the bead, at a point that is collinear with the measured point and a corresponding point on the left edge. Note that left and right are established from the bead's direction, which follows the sequence of measured points in increasing order. This value is only supported for stripe-beads.

---

### `M_END_POS_Y`

Retrieves the Y-coordinate of the right edge of the bead, at a point that is collinear with the measured point and a corresponding point on the left edge. Note that left and right are established from the bead's direction, which follows the sequence of measured points in increasing order. This value is only supported for stripe-beads.

---

### `M_INTENSITY`

Retrieves the intensity of the measured bead at the position of the measured point. This value is only supported for stripe-beads.

---

### `M_OFFSET`

Retrieves the linear distance between the trained point and its corresponding measured point.

---

### `M_POSITION_X`

Retrieves the X-coordinate of the measured point. The position is taken at the center of the bead width, at that point.

---

### `M_POSITION_Y`

Retrieves the Y-coordinate of the measured point. The position is taken at the center of the bead width, at that point.

---

### `M_START_POS_X`

Retrieves the X-coordinate of the left edge of the bead, at a point that is collinear with the measured point and a corresponding point on the right edge. Note that left and right are established from the bead's direction, which follows the sequence of measured points in increasing order. This value is only supported for stripe-beads.

---

### `M_START_POS_Y`

Retrieves the Y-coordinate of the left edge of the bead, at a point that is collinear with the measured point and a corresponding point on the right edge. Note that left and right are established from the bead's direction, which follows the sequence of measured points in increasing order. This value is only supported for stripe-beads.

---

### `M_STATUS_SEARCH`

Retrieves whether Aurora Imaging Library considers it possible to establish the measured point, given the current settings. For example, if the search box corresponding to a measured point falls outside the target image, Aurora Imaging Library returns [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) since you can never establish a measured point that is beyond the boundaries of the image.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies that Aurora Imaging Library considers it impossible to establish the measured point. |
| `M_PASS` | Specifies that Aurora Imaging Library considers it possible to establish the measured point. |

---

### `M_TRAINED_INDEX`

Retrieves the index of the trained point that corresponds to the measured point. This can be useful to get the indices of all trained points ([`M_ALL`](../../Reference/bead/MbeadGetResult.md)).

---

### `M_TRAINED_POSITION_X`

Retrieves the X-position of the trained point.

---

### `M_TRAINED_POSITION_Y`

Retrieves the Y-position of the trained point.

---

### `M_WIDTH_VALUE`

Retrieves the width of the measured bead at the position of the measured point. This value is only supported for stripe-beads.

### For retrieving results related to a template (measured bead) or to trained points (measured points)

To retrieve results related to a template's corresponding measured bead, or to a trained point's (one or all) corresponding measured point, the [`ResultType`](../../Reference/bead/MbeadGetResult.md) parameter can be set to one of the following values. In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadGetResult.md) parameter to the index or label of the template for which to get results (**M_TEMPLATE_INDEX()** or **M_TEMPLATE_LABEL()**). When retrieving results related to a template, you must set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md). When retrieving results related to the trained points, you must set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific value or [`M_ALL`](../../Reference/bead/MbeadGetResult.md).

---

### `M_STATUS_FOUND`

Retrieves the found status of the related template or trained point.  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md), [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned if all trained points have a corresponding measured point (all were found). Otherwise, [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) is returned.  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_ALL`](../../Reference/bead/MbeadGetResult.md), [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) or [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned for each trained point, depending on whether the corresponding measured point was found. If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific trained point, only the status of that point is returned.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies a failing status. |
| `M_PASS` | Specifies a passing status. |

---

### `M_STATUS_GAP_MAX`

Retrieves whether the [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadGetResult.md) result has passed or failed the maximum gap criterion, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadControl.md).  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md), [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned if the longest uninterrupted chain of trained points that were not found in the measured bead is less than [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadGetResult.md). Otherwise, [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) is returned.  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_ALL`](../../Reference/bead/MbeadGetResult.md), [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) or [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned for each trained point, depending on whether its associated measured point was not found, and is part of a gap that has failed the maximum gap criterion. If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific trained point, only the status of that point is returned.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies a failing status. |
| `M_PASS` | Specifies a passing status. |

---

### `M_STATUS_INTENSITY_MAX`

Retrieves whether the [`M_INTENSITY`](../../Reference/bead/MbeadGetResult.md) result has passed or failed the criterion for the highest pixel intensity allowed, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with _NominalIntensity_ + [`M_INTENSITY_DELTA_POS`](../../Reference/bead/MbeadControl.md). This value is only supported for stripe-beads. The nominal intensity can either be established by the training phase ([`M_AUTO`](../../Reference/bead/MbeadControl.md)) or with an explicit value ([`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md)).  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md), [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned if all measured points have an intensity that is lower than the highest intensity allowed. Otherwise, [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) is returned.  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_ALL`](../../Reference/bead/MbeadGetResult.md), [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) or [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned for each trained point, depending on whether its corresponding measured point has failed or passed the maximum color criterion. If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific trained point, only the status of that point is returned.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies a failing status. |
| `M_PASS` | Specifies a passing status. |

---

### `M_STATUS_INTENSITY_MIN`

Retrieves whether the [`M_INTENSITY`](../../Reference/bead/MbeadGetResult.md) result has passed or failed the criterion for the lowest intensity allowed, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with _NominalIntensity_ - [`M_INTENSITY_DELTA_NEG`](../../Reference/bead/MbeadControl.md). This value is only supported for stripe-beads. The nominal intensity can either be established by the training phase ([`M_AUTO`](../../Reference/bead/MbeadControl.md)) or with an explicit value ([`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md)).  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md), [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned if all measured points have an intensity that is higher than the lowest intensity allowed. Otherwise, [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) is returned.  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_ALL`](../../Reference/bead/MbeadGetResult.md), [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) or [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned for each trained point, depending on whether its measured point has failed or passed the minimum color criterion. If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific trained point, only the status of that point is returned.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies a failing status. |
| `M_PASS` | Specifies a passing status. |

---

### `M_STATUS_OFFSET`

Retrieves whether the [`M_OFFSET`](../../Reference/bead/MbeadGetResult.md) result has passed or failed the maximum offset criterion, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_OFFSET_MAX`](../../Reference/bead/MbeadControl.md).  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md), [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned if all measured points pass the maximum offset criterion. Otherwise, [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) is returned.  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_ALL`](../../Reference/bead/MbeadGetResult.md), [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) or [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned for each trained point, depending on whether its measured point has failed or passed the maximum offset criterion. If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific trained point, only the status of that point is returned.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies a failing status. |
| `M_PASS` | Specifies a passing status. |

---

### `M_STATUS_WIDTH_MAX`

Retrieves whether the [`M_WIDTH_VALUE`](../../Reference/bead/MbeadGetResult.md) result has passed or failed the maximum width criterion, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) + [`M_WIDTH_DELTA_POS`](../../Reference/bead/MbeadControl.md).  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md), [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned if all measured points pass the maximum width criterion. Otherwise, [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) is returned.  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_ALL`](../../Reference/bead/MbeadGetResult.md), [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) or [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned for each trained point, depending on whether its measured point has failed or passed the maximum width criterion. If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific trained point, only the status of that point is returned.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies a failing status. |
| `M_PASS` | Specifies a passing status. |

---

### `M_STATUS_WIDTH_MIN`

Retrieves whether the [`M_WIDTH_VALUE`](../../Reference/bead/MbeadGetResult.md) result has passed or failed the minimum width criterion, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) - [`M_WIDTH_DELTA_NEG`](../../Reference/bead/MbeadControl.md).  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadGetResult.md), [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned if all measured points pass the minimum width criterion. Otherwise, [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) is returned.  If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to [`M_ALL`](../../Reference/bead/MbeadGetResult.md), [`M_FAIL`](../../Reference/bead/MbeadGetResult.md) or [`M_PASS`](../../Reference/bead/MbeadGetResult.md) is returned for each trained point, depending on whether its measured point has failed or passed the minimum width criterion. If you set the [`ResultIndex`](../../Reference/bead/MbeadGetResult.md) parameter to a specific trained point, only the status of that point is returned.

| Value | Description |
| --- | --- |
| `M_FAIL` | Specifies a failing status. |
| `M_PASS` | Specifies a passing status. |

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
