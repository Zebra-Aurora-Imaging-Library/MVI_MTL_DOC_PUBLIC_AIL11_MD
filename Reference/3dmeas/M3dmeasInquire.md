---
doctype: Reference
module: 3dmeas
function: M3dmeasInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasInquire"
---

# M3dmeasInquire

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

> Inquire information about a 3D measurement context.

## Syntax

```c
AIL_INT64 M3dmeasInquire(
    AIL_ID    Context3dmeasId,      //in
    AIL_INT64 PathOrTemplateIndex,  //in
    AIL_INT64 ProfileIndex,         //in
    AIL_INT64 InquireType,          //in
    void *    UserVarPtr            //out
)
```

## Description

This function inquires information about a 3D measurement context.

To retrieve results from a 3D measurement result buffer, use [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md).

To inquire about draw 3D measurement settings for [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md), use [`M3dmeasInquireDraw`](../../Reference/3dmeas/M3dmeasInquireDraw.md) instead.

## Parameters

### `Context3dmeasId` *(in, AIL_ID)*

Specifies the identifier of the 3D measurement context to inquire. The 3D measurement context must have been previously allocated on the required system using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md).

### `PathOrTemplateIndex` *(in, AIL_INT64)*

Specifies that information will be inquired about a 3D measurement context, a path, a template, or a default template. Set this parameter to one of the following values:

*For specifying what to inquire*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PATH_INDEX` | Specifies to inquire about an individual path in a path 3D measurement context, if one is specified. |
| `M_TEMPLATE_INDEX` | Specifies to inquire about an individual template in a template 3D measurement context, if one is specified. |
| `M_CONTEXT` *(default)* | Specifies to inquire about a global setting of a 3D measurement context. |
| `M_DEFAULT_TEMPLATE` | Specifies to inquire about the default template in a profile 3D measurement context, if one is specified.

Note that the default template in a profile 3D measurement context is different from an explicitly defined template in a template 3D measurement context. The default template is inherent to the supplied profiles, such that it is the theoretical template that would result in these profiles. Note, unlike for the profiles perpendicular to the template in a template 3D measurement context, multiple markers can be found along the profiles perpendicular to the default template in a profile 3D measurement context. |

### `ProfileIndex` *(in, AIL_INT64)*

Specifies the profile to inquire about, if required. Set this parameter to one of the following values:

*For specifying a profile*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the index of a profile is not required. |
| `M_PROFILE_INDEX` | Specifies to inquire about an individual profile in a template 3D measurement context, if one is specified. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of information about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address of the variable in which to write the requested information. Since the [`M3dmeasInquire`](../../Reference/3dmeas/M3dmeasInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a global setting of a profile, path, or template 3D measurement context

For a profile, path, or template 3D measurement context, the [`InquireType`](../../Reference/3dmeas/M3dmeasInquire.md) parameter can be set to one of the following to inquire about a global setting. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md) or [`M_CONTEXT`](../../Reference/3dmeas/M3dmeasInquire.md), and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md).

---

### `M_TIMEOUT`

Inquires the maximum amount of time for [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) to complete the find marker operation before generating a time-out error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no timeout value. |
| `Value > 0.0` | Specifies the timeout value, in msec. |

### For inquiring about the default template in a profile 3D measurement context, path in a path 3D measurement context, or template in a template 3D measurement context

For the default template in a profile 3D measurement context, a path in a path 3D measurement context, or a template in a template 3D measurement context, the [`InquireType`](../../Reference/3dmeas/M3dmeasInquire.md) parameter can be set to one of the following. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT_TEMPLATE`](../../Reference/3dmeas/M3dmeasInquire.md), the index of a path, or the index of a template, respectively, and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md).

---

### `M_MARKER_DEPTH_MIN`

Inquires the minimum depth required for a peak or valley in the profile response, for it to be considered a valid transition signifying a marker.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum depth. |

---

### `M_MARKER_NUMBER`

Inquires the number of markers to search for in the profile.  Note that this inquire type is not available for a template 3D measurement context. In this case, the find marker operation always looks for 1 marker in each perpendicular profile of the template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to search for all markers. |
| `Value > 0` *(default)* | Specifies the number of markers for which to search. |

---

### `M_MARKER_NUMBER_TRANSITION_INSIDE`

Inquires the number of transitions to find inside a marker.  > **Note:** This inquire type is only available when [`M_MARKER_TYPE`](../../Reference/3dmeas/M3dmeasInquire.md) is set to [`M_PAIR`](../../Reference/3dmeas/M3dmeasInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to find markers with any number of transitions. |
| `Value > 0` | Specifies the number of transitions. |

---

### `M_MARKER_POLARITY`

Inquires the polarity of the transition to find. This is the polarity of the sole transition for an [`M_SINGLE`](../../Reference/3dmeas/M3dmeasInquire.md) marker and the first transition for an [`M_PAIR`](../../Reference/3dmeas/M3dmeasInquire.md) marker.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies to find a transition with any polarity. |
| `M_NEGATIVE` | Specifies to find a transition with negative polarity. |
| `M_POSITIVE` *(default)* | Specifies to find a transition with positive polarity. |

---

### `M_MARKER_POLARITY_SECOND`

Inquires the polarity of the second transition to find. This is the polarity of the second transition for an [`M_PAIR`](../../Reference/3dmeas/M3dmeasInquire.md) marker.  > **Note:** This inquire type is only available when [`M_MARKER_TYPE`](../../Reference/3dmeas/M3dmeasInquire.md) is set to [`M_PAIR`](../../Reference/3dmeas/M3dmeasInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies to find a transition with any polarity. |
| `M_NEGATIVE` | Specifies to find a transition with negative polarity. |
| `M_OPPOSITE` *(default)* | Specifies to find a transition with the opposite polarity of the first transition. |
| `M_POSITIVE` | Specifies to find a transition with positive polarity. |
| `M_SAME` | Specifies to find a transition with the same polarity as the first transition. |

---

### `M_MARKER_POSITION_INVALID`

Inquires where to identify a transition when an invalid region is encountered in the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AFTER` | Specifies that the transition is immediately after the gap. |
| `M_BEFORE` | Specifies that the transition is immediately before the gap. |
| `M_BOTH` *(default)* | Specifies that there is one transition immediately before and one transition immediately after the gap. |
| `M_MIDDLE` | Specifies that the transition is in the middle of the gap. |

---

### `M_MARKER_POSITION_RELATIVE`

Inquires the position that the markers should be closest to when [`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasInquire.md) is set to [`M_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasInquire.md)and the number of candidates is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasInquire.md).  Note that this also inquires the relative position used when calculating the [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md) [`M_OFFSET`](../../Reference/3dmeas/M3dmeasGetResult.md) result type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the position, as a percentage of the nominal profile length. |

---

### `M_MARKER_SELECTION`

Inquires the policy used to select found markers when the number of candidates is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BEST_FIT` | Specifies to use a different selection criteria depending on the operation.When performing an [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) operation, [`M_BEST_FIT`](../../Reference/3dmeas/M3dmeasInquire.md) specifies to select markers according to their proximity to the profile's origin; the closest marker is selected first. When performing an [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) operation with a template 3D measurement context, [`M_BEST_FIT`](../../Reference/3dmeas/M3dmeasInquire.md) specifies to select markers according to possible resulting fit scores; the marker that would result in the best fit possible is selected first. |
| `M_CLOSEST_TO_SPACING` | Specifies to select markers according to their similarity to the spacing set using [`M_MARKER_SPACING_RELATIVE`](../../Reference/3dmeas/M3dmeasInquire.md); the marker with the closest spacing is selected first. |
| `M_CLOSEST_TO_WIDTH` | Specifies to select markers according to their similarity to the width set using [`M_MARKER_WIDTH_RELATIVE`](../../Reference/3dmeas/M3dmeasInquire.md); the marker with the closest width is selected first. |
| `M_NARROWEST` | Specifies to select markers according to their width; the narrowest marker is selected first. |
| `M_POSITION_FIRST` | Specifies to select markers according to their proximity to the profile's origin; the closest marker is selected first. |
| `M_POSITION_LAST` | Specifies to select markers according to their proximity to the end of the profile; the closest marker is selected first. |
| `M_POSITION_RELATIVE` | Specifies to select markers according to their proximity to the position set using [`M_MARKER_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasInquire.md); the closest marker is selected first. |
| `M_STRONGEST` *(default)* | Specifies to select markers according to the strength of the transition; the marker with the strongest transition is selected first. |
| `M_TALLEST` | Specifies to select markers according to their measured depth; the marker with the greatest depth is selected first. |
| `M_WIDEST` | Specifies to select markers according to their width; the widest marker is selected first. |

---

### `M_MARKER_SPACING_RELATIVE`

Inquires the spacing that the marker's spacing should be closest to when [`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasInquire.md) is set to [`M_CLOSEST_TO_SPACING`](../../Reference/3dmeas/M3dmeasInquire.md)and the number of candidates is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the spacing, as a percentage of the nominal profile length. |

---

### `M_MARKER_STRENGTH_MIN`

Inquires the minimum strength required for a peak or valley in the profile response, for it to be considered a valid transition signifying a marker.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum strength. |

---

### `M_MARKER_STRENGTH_MIN_VAR`

Inquires the minimum prominence required for a peak or valley in the profile response, for it to be considered a valid transition signifying a marker.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum prominence. |

---

### `M_MARKER_TRANSITION`

Inquires the type of transition to find.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE` *(default)* | Specifies to find edge transitions. |
| `M_EDGE_OR_INVALID` | Specifies to find edge transitions and invalid transitions. |
| `M_INVALID` | Specifies to find invalid transitions. |
| `M_RIDGE` | Specifies to find ridge transitions. |

---

### `M_MARKER_TYPE`

Inquires the type of marker to extract from the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PAIR` | Specifies to extract markers represented by a pair of transitions. |
| `M_SINGLE` *(default)* | Specifies to extract markers represented by a single transition. |

---

### `M_MARKER_WIDTH_RELATIVE`

Inquires the width that the marker's width should be closest to when [`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasInquire.md) is set to [`M_CLOSEST_TO_WIDTH`](../../Reference/3dmeas/M3dmeasInquire.md)and the number of candidates is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the width, as a percentage of the nominal profile length. |

---

### `M_PROFILE_FILTER_SMOOTHNESS`

Inquires the normalized degree of smoothness (strength of denoising) applied to the profile to create the profile response before finding the markers.  > **Note:** This inquire type is only used when [`M_PROFILE_FILTER_SMOOTHNESS_TYPE`](../../Reference/3dmeas/M3dmeasInquire.md) is set to [`M_NORMALIZED`](../../Reference/3dmeas/M3dmeasInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value. |

---

### `M_PROFILE_FILTER_SMOOTHNESS_SIZE`

Inquires the smoothness size applied to the profile to create the profile response before finding the markers.  > **Note:** This inquire type is only used when [`M_PROFILE_FILTER_SMOOTHNESS_TYPE`](../../Reference/3dmeas/M3dmeasInquire.md) is set to [`M_SIZE`](../../Reference/3dmeas/M3dmeasInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 2.0` *(default)* | Specifies the smoothness size. |

---

### `M_PROFILE_FILTER_SMOOTHNESS_TYPE`

Inquires the filter smoothness type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NORMALIZED` *(default)* | Specifies to apply a normalized filter smoothness, set with [`M_PROFILE_FILTER_SMOOTHNESS`](../../Reference/3dmeas/M3dmeasInquire.md). |
| `M_SIZE` | Specifies to apply a filter smoothness size, set with [`M_PROFILE_FILTER_SMOOTHNESS_SIZE`](../../Reference/3dmeas/M3dmeasInquire.md). |

---

### `M_PROFILE_GAP_FILL_SHARP_ELEVATION`

Inquires how to fill sharp elevation gaps.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AFTER` | Specifies to fill the gap with the value of the boundary after the gap. |
| `M_BEFORE` | Specifies to fill the gap with the value of the boundary before the gap. |
| `M_DISABLE` *(default)* | Specifies not to fill sharp elevation gaps. |
| `M_MAX` | Specifies to fill the gap with the maximum gap boundary value. |
| `M_MIDDLE` | Specifies to fill the first half with the value of the boundary before the gap and the second half with the value of the boundary after the gap. |
| `M_MIN` | Specifies to fill the gap with the minimum gap boundary value. |

---

### `M_PROFILE_GAP_FILL_SHARP_ELEVATION_DEPTH`

Inquires the threshold used to differentiate between gradual elevation gaps and sharp elevation gaps.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the threshold is infinite. |
| `Value >= 0.0` | Specifies the threshold, as a percentage of the depth map's Z-range or in world units, depending on the units specified with [`M_PROFILE_GAP_FILL_SHARP_ELEVATION_DEPTH_UNITS`](../../Reference/3dmeas/M3dmeasInquire.md). |

---

### `M_PROFILE_GAP_FILL_SHARP_ELEVATION_DEPTH_UNITS`

Inquires the units with which to interpret the [`M_PROFILE_GAP_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dmeas/M3dmeasInquire.md) inquire type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PERCENTAGE` *(default)* | Specifies to interpret the value as a percentage of the range of Z-values (Z-range) visible in the depth map. |
| `M_WORLD` | Specifies to interpret the value in world units. |

---

### `M_PROFILE_GAP_FILL_THRESHOLD`

Inquires the maximum size of gaps that will be filled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies no maximum gap size. |
| `Value >= 0.0` *(default)* | Specifies the maximum gap size, in sample or world units, depending on the units specified with [`M_PROFILE_GAP_FILL_THRESHOLD_UNITS`](../../Reference/3dmeas/M3dmeasInquire.md). |

---

### `M_PROFILE_GAP_FILL_THRESHOLD_UNITS`

Inquires the units with which to interpret the [`M_PROFILE_GAP_FILL_THRESHOLD`](../../Reference/3dmeas/M3dmeasInquire.md) inquire type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SAMPLE` *(default)* | Specifies to interpret the value relative to the sampling distance ([`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasInquire.md)). |
| `M_WORLD` | Specifies to interpret the value in world units. |

### For inquiring about the profile extraction along a path in a path 3D measurement context or perpendicular to the template in a template 3D measurement context

For a path in a path 3D measurement context or template in a template 3D measurement context, the [`InquireType`](../../Reference/3dmeas/M3dmeasInquire.md) parameter can be set to one of the following. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to the index of a path or template, respectively, and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md). These inquire types are used to inquire about how to extract the profile along the path or the profiles at regular intervals perpendicular to the template.

---

### `M_PROFILE_ACCUMULATE_TYPE`

Inquires how points in a given sample are accumulated to create a single profile point when there are multiple points within the thickness of the profile line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX` | Specifies to use the largest value of all the points in the sample. |
| `M_MEAN` *(default)* | Specifies to use the mean value of all the points in the sample. |
| `M_MIN` | Specifies to use the smallest value of all the points in the sample. |

---

### `M_PROFILE_INTERPOLATION_AVERAGE_FRACTION`

Inquires the fraction of the sampling distance to use when taking a weighted average of the area surrounding the profile point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 1.0` *(default)* | Specifies the fraction. |

---

### `M_PROFILE_INTERPOLATION_MODE`

Inquires the interpolation mode used to calculate the value for each point in the sample that will be used to create the profile point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` | Specifies averaging interpolation. |
| `M_BILINEAR` *(default)* | Specifies bilinear interpolation. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. |

---

### `M_PROFILE_MIN_VALID_PERCENTAGE`

Inquires the minimum percentage of points in a given sample that must be valid to set the corresponding profile point to a valid value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the percentage of points in the sample. |

---

### `M_PROFILE_PROJECTION_ANGLE`

Inquires the nominal angle at which the projection is done for a given profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `10.0 <= Value <= 170.0` *(default)* | Specifies the nominal projection angle, in degrees. |

---

### `M_PROFILE_PROJECTION_ANGLE_ACCURACY`

Inquires the accuracy of the projection angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `10.0 <= Value <= 170.0` *(default)* | Specifies the nominal projection angle, in degrees. |

---

### `M_PROFILE_PROJECTION_ANGLE_DELTA_NEG`

Inquires the lower limit of the projection angle range, relative to the nominal projection angle ([`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasInquire.md)). That is, _LowerLimit_ = [`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasInquire.md)  - [`M_PROFILE_PROJECTION_ANGLE_DELTA_NEG`](../../Reference/3dmeas/M3dmeasInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 170.0` *(default)* | Specifies the lower limit of the projection angle range, in degrees. |

---

### `M_PROFILE_PROJECTION_ANGLE_DELTA_POS`

Inquires the upper limit of the projection angle range, relative to the nominal projection angle ([`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasInquire.md)). That is, _UpperLimit_ = [`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasInquire.md)  + [`M_PROFILE_PROJECTION_ANGLE_DELTA_POS`](../../Reference/3dmeas/M3dmeasInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 170.0` *(default)* | Specifies the upper limit of the projection angle range, in degrees. |

---

### `M_PROFILE_PROJECTION_ANGLE_MODE`

Inquires whether to consider projection angles within an angular range when extracting the profile to search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to use multiple projection angles. |
| `M_ENABLE` | Specifies to use multiple projection angles. |

---

### `M_PROFILE_PROJECTION_ANGLE_TOLERANCE`

Inquires the tolerance used to determine the projection angles.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 170.0` *(default)* | Specifies the tolerance, in degrees. |

---

### `M_PROFILE_SAMPLE_SIZE`

Inquires the sampling distance between two consecutive points along the profile line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the distance. |

---

### `M_PROFILE_SAMPLE_SIZE_MODE`

Inquires how to interpret the specified sampling distance ([`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the sampling distance directly, without applying a multiplying factor. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the sampling distance by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` *(default)* | Specifies to multiply the sampling distance by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the sampling distance by the pixel size in X. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the sampling distance by the pixel size in Y. |

---

### `M_PROFILE_THICKNESS`

Inquires the thickness of the profile line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the thickness. |

---

### `M_PROFILE_THICKNESS_MODE`

Inquires how to interpret the specified thickness value ([`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the thickness value directly, without applying a multiplying factor. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the thickness value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` *(default)* | Specifies to multiply the thickness value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the thickness value by the pixel size in X. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the thickness value by the pixel size in Y. |
| `M_RELATIVE_TO_SPACING` | Specifies to multiply the thickness value by the profile spacing. |

### For inquiring about a template in a template 3D measurement context

For a template in a template 3D measurement context, the [`InquireType`](../../Reference/3dmeas/M3dmeasInquire.md) parameter can be set to one of the following. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to the index of a template, and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md).

---

### `M_TEMPLATE_EXTENDED_SEARCH`

Inquires whether to extend the search outside of the template's defined area.  *[Image: 3dmeas_extended_search.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to extend the search beyond the template. |
| `M_ENABLE` *(default)* | Specifies to extend the search beyond the template. |

---

### `M_TEMPLATE_PROFILE_COUNT`

Inquires the number of profiles used to find the markers of a template.

| Value | Description |
| --- | --- |
| `M_UNKNOWN` | Specifies that the number of profiles used is unknown. This occurs when the number of profiles is dependent on the depth map calibration. For example, when [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_SPACING`](../../Reference/3dmeas/M3dmeasControl.md) and [`M_TEMPLATE_PROFILE_SPACING_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md). |
| `Value > 0` | Specifies the number profiles used. |

---

### `M_TEMPLATE_PROFILE_NUMBER_ELEMENTS`

Inquires the number of profile elements in the template 3D measurement context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= M_TEMPLATE_PROFILE_NUMBER_VALUE` *(default)* | Specifies the number of profile elements. |

---

### `M_TEMPLATE_PROFILE_NUMBER_MODE`

Inquires the mode with which to determine the number of profiles to use to find the markers of a template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SPACING` | Specifies that the number of profiles is determined based on the spacing between the profiles. |
| `M_USER_DEFINED` *(default)* | Specifies to use the value set with [`M_TEMPLATE_PROFILE_NUMBER_VALUE`](../../Reference/3dmeas/M3dmeasInquire.md). |

---

### `M_TEMPLATE_PROFILE_NUMBER_VALUE`

Inquires the number of profiles perpendicular to the template.  Note that this inquire type is only valid if [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 < Value <= M_TEMPLATE_PROFILE_NUMBER_ELEMENTS` *(default)* | Specifies the number of profiles. |

---

### `M_TEMPLATE_PROFILE_SPACING`

Inquires the spacing between the profiles along the template.  Note that this inquire type is only valid if [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_SPACING`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the spacing. |

---

### `M_TEMPLATE_PROFILE_SPACING_MODE`

Inquires how to interpret the specified spacing value ([`M_TEMPLATE_PROFILE_SPACING`](../../Reference/3dmeas/M3dmeasInquire.md)).  Note that this inquire type is only valid if [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_SPACING`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the spacing value directly, without applying a multiplying factor. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the spacing value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` | Specifies to multiply the spacing value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the spacing value by the pixel size in X. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the spacing value by the pixel size in Y. |
| `M_RELATIVE_TO_THICKNESS` *(default)* | Specifies to multiply the spacing value by the profile thickness. |

### For inquiring about a template or profile in a template 3D measurement context

For a template or profile in a template 3D measurement context, the [`InquireType`](../../Reference/3dmeas/M3dmeasInquire.md) parameter can be set to one of the following. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to the index of a template. If [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) is set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md), these inquire types allow you to retrieve global settings for the template's perpendicular profiles. If [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) is set to a specific index, they allow you to retrieve custom settings for the specified profile. Note that any custom setting for the specified profile will only be used if its corresponding [`M_TEMPLATE_PROFILE_..._SOURCE`](../../Reference/3dmeas/M3dmeasInquire.md) control is set to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasInquire.md).

---

### `M_TEMPLATE_PROFILE_LENGTH`

Inquires the nominal length of the profile to extract (perpendicular to the template).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the length. |

---

### `M_TEMPLATE_PROFILE_LENGTH_MARGIN`

Inquires the margin on the profile length. The nominal length ([`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasInquire.md)), plus the margin define the length of the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the margin, as a percentage of the nominal length. |

---

### `M_TEMPLATE_PROFILE_LENGTH_MODE`

Inquires how to interpret the specified length value ([`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the length value directly, without applying a multiplying factor. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the length value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` *(default)* | Specifies to multiply the length value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the length value by the pixel size in X. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the length value by the pixel size in Y. |

---

### `M_TEMPLATE_PROFILE_POSITION_RELATIVE`

Inquires the position of the profile, relative to the template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `-50.0 <= Value <= 50.0` *(default)* | Specifies the relative position, as a percentage of the nominal length. |

### For inquiring about a profile in a template 3D measurement context

For a profile perpendicular to a template in a template 3D measurement context, the [`InquireType`](../../Reference/3dmeas/M3dmeasInquire.md) parameter can be set to one of the following. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to the index of a template, and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to the index of one or all profiles. These inquire types allow you to retrieve whether the global or custom settings will be used for the specified profile, when extracting the profile perpendicular to the template.

---

### `M_TEMPLATE_PROFILE_LENGTH_MARGIN_SOURCE`

Inquires whether to use the global margin value or a custom margin for the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PROFILE` | Specifies to use the custom margin value for the specified profile. |
| `M_TEMPLATE` *(default)* | Specifies to use the template's global margin value for the specified profile. |

---

### `M_TEMPLATE_PROFILE_LENGTH_SOURCE`

Inquires whether to use the global nominal length value and length mode or a custom nominal length and length mode for the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PROFILE` | Specifies to use the custom profile length for the specified profile. |
| `M_TEMPLATE` *(default)* | Specifies to use the template's global profile length for the specified profile. |

---

### `M_TEMPLATE_PROFILE_POSITION_SOURCE`

Inquires whether to use the global relative position value or a custom relative position for the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PROFILE` | Specifies to use the custom relative position value for the specified profile. |
| `M_TEMPLATE` *(default)* | Specifies to use the template's global relative position value. |

### For inquiring about a fit 3D measurement context

For a fit 3D measurement context, the [`InquireType`](../../Reference/3dmeas/M3dmeasInquire.md) parameter can be set to one of the following. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md) or [`M_CONTEXT`](../../Reference/3dmeas/M3dmeasInquire.md), and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md).

---

### `M_EXPECTED_OUTLIER_PERCENTAGE`

Inquires the expected percentage of outlier markers among the markers that are extracted.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 100.0` *(default)* | Specifies the expected outliers, as a percentage. |

---

### `M_FIT_COVERAGE_MIN`

Inquires the minimum percentage of profiles that must have a marker covered by a fitted point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the minimum coverage, as a percentage. |

---

### `M_FIT_DISTANCE`

Inquires the distance value that Aurora Imaging Library uses to establish outlier markers, which are excluded from the robust best fit.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the fit distance is infinite. |
| `Value > 0.0` | Specifies the fit distance. |

---

### `M_FIT_DISTANCE_MODE`

Inquires how to interpret the specified fit distance value ([`M_FIT_DISTANCE`](../../Reference/3dmeas/M3dmeasInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the fit distance value directly, without applying a multiplying factor. |
| `M_AUTO` *(default)* | Specifies to automatically calculate the fit distance; the distance is estimated from all the extracted markers. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the fit distance value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` | Specifies to multiply the fit distance value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the fit distance value by the pixel size in X. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the fit distance value by the pixel size in Y. |
| `M_RELATIVE_TO_SAMPLING` | Specifies to multiply the fit distance value by the sampling distance along each profile. |

---

### `M_REMOVE_OUTLIERS`

Inquires whether to remove outlier markers from the template 3D measurement result buffer after the fit.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to remove outliers. |
| `M_ENABLE` | Specifies to remove outliers. |

### For inquiring about a path 3D measurement context

For a path 3D measurement context, the [`InquireType`](../../Reference/3dmeas/M3dmeasInquire.md) parameter can be set to one of the following. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md) or [`M_CONTEXT`](../../Reference/3dmeas/M3dmeasInquire.md), and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md).

---

### `M_NUMBER_OF_PATHS`

Inquires the number of paths in the path 3D measurement context.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of paths. |

### For inquiring about a template 3D measurement context

For a template 3D measurement context, the [`InquireType`](../../Reference/3dmeas/M3dmeasInquire.md) parameter can be set to one of the following. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md) or [`M_CONTEXT`](../../Reference/3dmeas/M3dmeasInquire.md), and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasInquire.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasInquire.md).

---

### `M_NUMBER_OF_TEMPLATES`

Inquires the number of templates in the template 3D measurement context.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of templates. |

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

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
