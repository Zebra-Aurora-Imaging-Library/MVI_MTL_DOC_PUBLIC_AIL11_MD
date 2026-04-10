---
doctype: Reference
module: 3dmeas
function: M3dmeasGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasGetResult"
---

# M3dmeasGetResult

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

> Get the specified type of result(s) from a 3D measurement result buffer.

## Syntax

```c
AIL_DOUBLE M3dmeasGetResult(
    AIL_ID    Result3dmeasId,       //in
    AIL_INT64 PathOrTemplateIndex,  //in
    AIL_INT64 ProfileIndex,         //in
    AIL_INT64 MarkerIndex,          //in
    AIL_INT64 ResultType,           //in
    void *    ResultArrayPtr        //out
)
```

## Description

This function retrieves the result(s) of the specified type from a 3D measurement result buffer.

## Parameters

### `Result3dmeasId` *(in, AIL_ID)*

Specifies the identifier of the 3D measurement result buffer from which to retrieve results.

### `PathOrTemplateIndex` *(in, AIL_INT64)*

Specifies the index of the path or template, or the default template in a profile 3D measurement result buffer. Set this parameter to one of the following values:

*For specifying the index of the template or path*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PATH_INDEX` | Specifies to retrieve results for a path in a path 3D measurement result buffer, if one is specified. |
| `M_TEMPLATE_INDEX` | Specifies to retrieve results for a template in a template 3D measurement result buffer, if one is specified. |
| `M_DEFAULT_TEMPLATE` | Specifies to retrieve results for the default template in a profile 3D measurement result buffer, if one is specified.

Note that the default template in a profile 3D measurement context is different from an explicitly defined template in a template 3D measurement context. The default template is inherent to the supplied profiles, such that it is the theoretical template that would result in these profiles. Note, unlike for the profiles perpendicular to the template in a template 3D measurement context, multiple markers can be found along the profiles perpendicular to the default template in a profile 3D measurement context. |
| `M_GENERAL` *(default)* | Specifies to retrieve general results relating to the entire 3D measurement result buffer. |

### `ProfileIndex` *(in, AIL_INT64)*

Specifies the index of the profile. Set this parameter to one of the following values:

*For specifying the index of the profile*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If the [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter is set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasGetResult.md) or [`M_GENERAL`](../../Reference/3dmeas/M3dmeasGetResult.md), specifies that this parameter is unused; otherwise, same as [`M_GENERAL`](../../Reference/3dmeas/M3dmeasGetResult.md). |
| `M_PROFILE_INDEX` | Specifies to retrieve results for a profile. |
| `M_GENERAL` | Specifies to retrieve general results for the default template, a path, or a template. |

### `MarkerIndex` *(in, AIL_INT64)*

Specifies the index of the marker. Set this parameter to one of the following values:

*For specifying the index of the marker*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If the [`ProfileIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter is set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasGetResult.md) or [`M_GENERAL`](../../Reference/3dmeas/M3dmeasGetResult.md), specifies that this parameter is unused; otherwise, same as [`M_GENERAL`](../../Reference/3dmeas/M3dmeasGetResult.md). |
| `M_TRANSITION_FIRST` | Specifies to retrieve results for the first transition of a marker. Note that the first transition is the sole transition of an [`M_SINGLE`](../../Reference/3dmeas/M3dmeasControl.md) marker. |
| `M_TRANSITION_SECOND` | Specifies to retrieve results for the second transition of an [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md) marker. |
| `M_ALL` | Specifies to retrieve results for all markers. |
| `M_GENERAL` | Specifies to retrieve general results for a profile. |
| `0 <= Value < M_NUMBER_MARKER` | Specifies the index of the individual marker for which to retrieve results. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address in which to write the results. Since the [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md) function also returns the results, you can set this parameter to `M_NULL`.

## Parameter Associations

### For retrieving general results from a profile, path, or template 3D measurement result buffer

When retrieving general results from a profile, path, or template 3D measurement result buffer, the [`ResultType`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter can be set to the following value. The [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to [`M_GENERAL`](../../Reference/3dmeas/M3dmeasGetResult.md), and the [`ProfileIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) and [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameters must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasGetResult.md).

---

### `M_STATUS_FIND`

Retrieves the status of the find marker operation.  This result is always available.  For the status of a fit operation, use [`M_STATUS_FIT`](../../Reference/3dmeas/M3dmeasGetResult.md).

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the find marker operation completed. |
| `M_CURRENTLY_CALCULATING` | Specifies that the find marker operation is ongoing. You can only get this status if you are retrieving it from another thread. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected internal error occurred during the find marker operation. |
| `M_NOT_INITIALIZED` | Specifies that the 3D measurement result buffer was not used in a call to [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md), and contains no results. |
| `M_STOPPED_BY_REQUEST` | Specifies that the find marker operation was stopped from another thread using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_STOP_FIND`](../../Reference/3dmeas/M3dmeasControl.md). |
| `M_TIMEOUT_REACHED` | Specifies that the find marker operation took longer than the allowed value, specified using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TIMEOUT`](../../Reference/3dmeas/M3dmeasControl.md), and has stopped before completion. |

### For retrieving default template, path, or template results from a profile, path, or template 3D measurement result buffer

When retrieving results for a default template, path, or template from a profile, path, or template 3D measurement result buffer, the [`ResultType`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter can be set to one of the following values. The [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to [`M_DEFAULT_TEMPLATE`](../../Reference/3dmeas/M3dmeasGetResult.md), **M_PATH_INDEX()**, or **M_TEMPLATE_INDEX()**, the [`ProfileIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to [`M_GENERAL`](../../Reference/3dmeas/M3dmeasGetResult.md), and the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasGetResult.md).

---

### `M_NUMBER_PROFILE`

Retrieves the number of profiles that were analyzed. Note that along a path, only one profile is extracted and analyzed.

---

### `M_PROFILE_PROJECTION_ANGLE`

Retrieves the projection angle used to create the profile. This is also the angle, relative to the profile line, at which markers are found.  Note that this result type is not available for a profile 3D measurement result buffer.

---

### `M_PROFILE_SAMPLE_SIZE`

Retrieves the sampling distance between profile points, in world units. Note that this is also the pixel size in X of the profiles.  For a path or template 3D measurement result buffer, this is equivalent to the sampling distance specified using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasControl.md) when [`M_PROFILE_SAMPLE_SIZE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_ABSOLUTE`](../../Reference/3dmeas/M3dmeasControl.md), or equivalent to [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasControl.md) multiplied by the source pixel size when [`M_PROFILE_SAMPLE_SIZE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md).

### For retrieving profile results from a profile, path, or template 3D measurement result buffer

When retrieving results for a profile from a profile, path, or template 3D measurement result buffer, the [`ResultType`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter can be set to one of the following values. The [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to [`M_DEFAULT_TEMPLATE`](../../Reference/3dmeas/M3dmeasGetResult.md), **M_PATH_INDEX()**, or **M_TEMPLATE_INDEX()**, the [`ProfileIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the index of one or all profiles, and the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to [`M_GENERAL`](../../Reference/3dmeas/M3dmeasGetResult.md).

---

### `M_NUMBER_MARKER`

Retrieves the number of markers found. Note that for a template 3D measurement result buffer, a maximum of 1 marker is found for each profile taken perpendicular to a template.

---

### `M_PROFILE_RESPONSE`

Retrieves the response of the profile after applying a filter (that is, the normalized first derivative edge profile response or normalized second derivative ridge profile response).  Note that this result type is not available if the [`M_MARKER_TRANSITION`](../../Reference/3dmeas/M3dmeasControl.md) control type is set to [`M_INVALID`](../../Reference/3dmeas/M3dmeasControl.md).

---

### `M_RESULT_SIZE_X`

Retrieves the size in X of the profile response, in sample units. Note that this is equivalent to the number of profile points.

### For retrieving marker or transition results from a profile, path, or template 3D measurement result buffer

When retrieving results for a marker or marker transition from a profile, path, or template 3D measurement result buffer, the [`ResultType`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter can be set to one of the following values. The [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to [`M_DEFAULT_TEMPLATE`](../../Reference/3dmeas/M3dmeasGetResult.md), **M_PATH_INDEX()**, or **M_TEMPLATE_INDEX()**, the [`ProfileIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the index of one or all profiles, and the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the index of one or all markers or the first or second transition of one or all markers.

---

### `M_DEPTH`

Retrieves the depth of the marker or transition. This is equivalent to [`M_MAX_Z`](../../Reference/3dmeas/M3dmeasGetResult.md) - [`M_MIN_Z`](../../Reference/3dmeas/M3dmeasGetResult.md).

---

### `M_FIT_INLIER_STATUS`

Retrieves whether the marker was inside or outside the fit.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the index of one or all markers.

| Value | Description |
| --- | --- |
| `M_INSIDE` | Specifies that the marker was inside. |
| `M_OUTSIDE` | Specifies that the marker was outside. |

---

### `M_MARKER_POSITION_END`

Retrieves the position of the end of the marker relative to the start of the profile, in world units. The end position of an [`M_SINGLE`](../../Reference/3dmeas/M3dmeasControl.md) marker is the end of the transition. The end position of an [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md) marker is the center of the second transition.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the index of one or all markers.

---

### `M_MARKER_POSITION_START`

Retrieves the position of the start of the marker relative to the start of the profile, in world units. The start position of an [`M_SINGLE`](../../Reference/3dmeas/M3dmeasControl.md) marker is the start of the transition. The start position of an [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md) marker is the center of the first transition.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the index of one or all markers.

---

### `M_MAX_Z`

Retrieves the maximum Z-value that makes up the marker or transition.

---

### `M_MIN_Z`

Retrieves the minimum Z-value that makes up the marker or transition.

---

### `M_OFFSET`

Retrieves the offset of the marker or transition from the position specified using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md).  The offset is calculated using the following formula: `([`M_POSITION`](../../Reference/3dmeas/M3dmeasGetResult.md) / _nominal profile length_ * 100) - [`M_MARKER_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasInquire.md)`. For example, if the marker/transition was found at 3 mm from the start of the profile, the nominal profile length is 10 mm, and [`M_MARKER_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasInquire.md) is set to 50%, [`M_OFFSET`](../../Reference/3dmeas/M3dmeasGetResult.md) will return -20%.  Use [`M_POSITION`](../../Reference/3dmeas/M3dmeasGetResult.md) to retrieve the position relative to the start of the profile.

---

### `M_POLARITY`

Retrieves the polarity of the transition.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the first or second transition of one or all markers.

| Value | Description |
| --- | --- |
| `M_NEGATIVE` | Specifies a negative polarity. |
| `M_POSITIVE` | Specifies a positive polarity. |
| `M_UNDEFINED` | Specifies an undefined polarity because the transition is an [`M_INVALID`](../../Reference/3dmeas/M3dmeasGetResult.md) transition with no depth. |

---

### `M_POLARITY_PAIR`

Retrieves the polarity of the [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md) marker.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the index of one or all markers.

| Value | Description |
| --- | --- |
| `M_OPPOSITE` | Specifies that the polarity of the second transition is the opposite of that of the first. |
| `M_SAME` | Specifies that the polarity of the two transitions are the same. |
| `M_UNDEFINED` | Specifies an undefined polarity because one or both of the transitions is an [`M_INVALID`](../../Reference/3dmeas/M3dmeasGetResult.md) transition with no depth. |

---

### `M_POSITION`

Retrieves the position of the marker or transition relative to the start of the profile, in world units. For example, if the marker/transition was found at 3 mm from the start of the profile, [`M_POSITION`](../../Reference/3dmeas/M3dmeasGetResult.md) will return 3 mm.  Use [`M_OFFSET`](../../Reference/3dmeas/M3dmeasGetResult.md) to retrieve the position relative to another profile position.

---

### `M_POSITION_EXTRACTED`

Retrieves the position of the marker or transition relative to the start of the profile, in sample units. Note that this considers the entire profile, including areas that are clipped due to falling outside of the depth map.  The extracted position is calculated using the following formula: `[`M_POSITION`](../../Reference/3dmeas/M3dmeasGetResult.md) / ([`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasGetResult.md) / sample)`. For example, if the marker/transition was found at 3 mm from the start of the profile and the sampling distance is 0.25 mm, [`M_POSITION_EXTRACTED`](../../Reference/3dmeas/M3dmeasGetResult.md) will return 12 sample units.

---

### `M_POSITION_EXTRACTED_CLIPPED`

Retrieves the position of the marker or transition relative to the start of the clipped profile, in sample units. For example, if the marker or transition is 12 sample units from the start of whole profile, but the first 2 sample units of the profile fall outside of the depth map, [`M_POSITION_EXTRACTED_CLIPPED`](../../Reference/3dmeas/M3dmeasGetResult.md) will return 10 sample units.

---

### `M_POSITION_INVALID`

Retrieves the position of the invalid transition relative to an invalid region (gap) in the profile. You can specify where to identify a transition when an invalid region is encountered in the profile, using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_POSITION_INVALID`](../../Reference/3dmeas/M3dmeasControl.md).  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the first or second transition of one or all markers. This result type is only meaningful if the [`M_TRANSITION`](../../Reference/3dmeas/M3dmeasGetResult.md) result type is [`M_INVALID`](../../Reference/3dmeas/M3dmeasGetResult.md).

| Value | Description |
| --- | --- |
| `M_AFTER` | Specifies that the transition is located after the gap. |
| `M_BEFORE` | Specifies that the transition is located before the gap. |
| `M_MIDDLE` | Specifies that the transition is located in the middle of the gap. |

---

### `M_POSITION_RELATIVE`

Retrieves the position of the marker or transition relative to the start of the profile, as a percentage of the nominal length of the profile.  The relative position is calculated using the following formula: `[`M_POSITION`](../../Reference/3dmeas/M3dmeasGetResult.md) / _nominal profile length_ * 100`. For example, if the marker/transition was found at 3 mm from the start of the profile and the nominal profile length is 10 mm, [`M_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasGetResult.md) will return 30%.

---

### `M_POSITION_X`

Retrieves the X-coordinate of the marker or transition in the source depth map, in world units.

---

### `M_POSITION_Y`

Retrieves the Y-coordinate of the marker or transition in the source depth map, in world units.

---

### `M_POSITION_Z`

Retrieves the Z-coordinate of the marker or transition in the source depth map, in world units.

---

### `M_SPACING`

Retrieves the spacing between the marker and the next marker. The spacing is equivalent to [`M_POSITION`](../../Reference/3dmeas/M3dmeasGetResult.md) of the next marker minus [`M_POSITION`](../../Reference/3dmeas/M3dmeasGetResult.md) of the current marker. Note that the spacing of the last marker is always 0.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the index of one or all markers.

---

### `M_SPACING_RELATIVE`

Retrieves the spacing between the marker and the next marker, as a percentage of the nominal length of the profile.  The relative spacing is calculated using the following formula: `[`M_SPACING`](../../Reference/3dmeas/M3dmeasGetResult.md) / _nominal profile length_ * 100`. For example, if the spacing is 4mm and the nominal profile length is 10 mm, [`M_SPACING_RELATIVE`](../../Reference/3dmeas/M3dmeasGetResult.md) will return 40%.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the index of one or all markers.

---

### `M_STRENGTH`

Retrieves the strength of the marker or transition in the profile response.

---

### `M_TRANSITION`

Retrieves the type of transition.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the first or second transition of one or all markers.

| Value | Description |
| --- | --- |
| `M_EDGE` | Specifies an edge transition. |
| `M_INVALID` | Specifies an invalid transition. If [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_TRANSITION`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_EDGE_OR_INVALID`](../../Reference/3dmeas/M3dmeasControl.md) or [`M_INVALID`](../../Reference/3dmeas/M3dmeasControl.md), invalid transitions can be identified before, after, or in the middle of gaps encountered in the profile. |
| `M_RIDGE` | Specifies a ridge transition. |

---

### `M_TRANSITION_POSITION_END`

Retrieves the position of the end of the transition relative to the start of the profile, in world units.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the first or second transition of one or all markers.

---

### `M_TRANSITION_POSITION_START`

Retrieves the position of the start of the transition relative to the start of the profile, in world units.  Note that the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to the first or second transition of one or all markers.

---

### `M_WIDTH`

Retrieves the width of the marker or transition. The width of a transition or an [`M_SINGLE`](../../Reference/3dmeas/M3dmeasControl.md) marker is the distance between the start and end of the transition. The width of an [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md) marker is the distance between the center of the first transition and center of the second transition.  Note that for a transition, the width is equivalent to [`M_TRANSITION_POSITION_START`](../../Reference/3dmeas/M3dmeasGetResult.md) - [`M_TRANSITION_POSITION_END`](../../Reference/3dmeas/M3dmeasGetResult.md). For a marker, the width is equivalent to [`M_MARKER_POSITION_START`](../../Reference/3dmeas/M3dmeasGetResult.md) - [`M_MARKER_POSITION_END`](../../Reference/3dmeas/M3dmeasGetResult.md).

---

### `M_WIDTH_RELATIVE`

Retrieves the width of the marker or transition, as a percentage of the nominal length of the profile.  The relative width is calculated using the following formula: `[`M_WIDTH`](../../Reference/3dmeas/M3dmeasGetResult.md) / _nominal profile length_ * 100`. For example, if the marker/transition is 2 mm wide and the nominal profile length is 10 mm, [`M_WIDTH_RELATIVE`](../../Reference/3dmeas/M3dmeasGetResult.md) will return 20%.

### For retrieving fit results from a template 3D measurement result buffer

When retrieving fit results for a template from a template 3D measurement result buffer, the [`ResultType`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter can be set to one of the following values. The [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to **M_TEMPLATE_INDEX()**, the [`ProfileIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to [`M_GENERAL`](../../Reference/3dmeas/M3dmeasGetResult.md), and the [`MarkerIndex`](../../Reference/3dmeas/M3dmeasGetResult.md) parameter must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasGetResult.md). These results are only available after a call to [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md), unless otherwise specified.

---

### `M_ANGULARITY`

Retrieves the angle between the fitted 3D geometry and the template, in degrees.

---

### `M_AXIS_X`

Retrieves the X-component of the fitted 3D geometry's unit vector.  For a 3D line geometry, the X-component of the line's direction unit vector is retrieved. This vector does not reflect the line's length.

---

### `M_AXIS_Y`

Retrieves the Y-component of the fitted 3D geometry's unit vector.  For a 3D line geometry, the Y-component of the line's direction unit vector is retrieved. This vector does not reflect the line's length.

---

### `M_AXIS_Z`

Retrieves the Z-component of the fitted 3D geometry's unit vector.  For a 3D line geometry, the Z-component of the line's direction unit vector is retrieved. This vector does not reflect the line's length.

---

### `M_CENTER_DISTANCE`

Retrieves the distance between the center of the fitted 3D geometry and the template.

---

### `M_CENTER_X`

Retrieves the X-coordinate of the center of the fitted 3D geometry.

---

### `M_CENTER_Y`

Retrieves the Y-coordinate of the center of the fitted 3D geometry.

---

### `M_CENTER_Z`

Retrieves the Z-coordinate of the center of the fitted 3D geometry.

---

### `M_DISTANCE_MAX`

Retrieves the maximum distance between the fitted 3D geometry and the template.

---

### `M_END_POINT_X`

Retrieves the X-coordinate of the fitted 3D geometry's end point.

---

### `M_END_POINT_Y`

Retrieves the Y-coordinate of the fitted 3D geometry's end point.

---

### `M_END_POINT_Z`

Retrieves the Z-coordinate of the fitted 3D geometry's end point.

---

### `M_FIT_RMS_ERROR`

Retrieves the root-mean-squared (RMS) error of the distance between the markers and the fitted 3D geometry. Only inliers are considered when calculating the RMS error.

---

### `M_NUMBER_MARKER_FITTED`

Retrieves the number of markers that were used for the fit operation.

---

### `M_NUMBER_MARKER_OUTLIERS`

Retrieves the number of markers that were not used for the fit operation.

---

### `M_START_POINT_X`

Retrieves the X-coordinate of the fitted 3D geometry's start point.

---

### `M_START_POINT_Y`

Retrieves the Y-coordinate of the fitted 3D geometry's start point.

---

### `M_START_POINT_Z`

Retrieves the Z-coordinate of the fitted 3D geometry's start point.

---

### `M_STATUS_FIT`

Retrieves the status of the fit operation.  This result is always available.  For the status of a find marker operation, use [`M_STATUS_FIND`](../../Reference/3dmeas/M3dmeasGetResult.md).

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the fit operation completed. |
| `M_CURRENTLY_CALCULATING` | Specifies that the fit operation is ongoing. You can only get this status if you are retrieving it from another thread. |
| `M_INSUFFICIENT_COVERAGE` | Specifies that the fit operation failed because an insufficient number of markers were covered by the fitted points. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected error occurred during the fit operation. |
| `M_NOT_ENOUGH_VALID_DATA` | Specifies that the fit operation failed because there were not enough valid points to fit the specified 3D geometry. |
| `M_NOT_INITIALIZED` | Specifies that the 3D measurement result buffer was not used in a call to [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md), and contains no results. |

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

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the requested information, cast to an _AIL_DOUBLE_. If the requested information does not fit into an _AIL_DOUBLE_, this function will return `M_NULL`or truncate the information.
