---
doctype: Reference
module: 3dmeas
function: M3dmeasControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasControl"
---

# M3dmeasControl

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

> Control a setting for a 3D measurement context or result buffer.

## Syntax

```c
void M3dmeasControl(
    AIL_ID     ContextOrResult3dmeasId,  //out
    AIL_INT64  PathOrTemplateIndex,      //in
    AIL_INT64  ProfileIndex,             //in
    AIL_INT64  ControlType,              //in
    AIL_DOUBLE ControlValue              //in
)
```

## Description

This function allows you to control a setting of a 3D measurement context or result buffer. You can typically inquire these settings using [`M3dmeasInquire`](../../Reference/3dmeas/M3dmeasInquire.md).

To control draw 3D measurement settings for [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md), use [`M3dmeasControlDraw`](../../Reference/3dmeas/M3dmeasControlDraw.md) instead.

## Parameters

### `ContextOrResult3dmeasId` *(out, AIL_ID)*

Specifies the identifier of the 3D measurement context or result buffer to control. The 3D measurement context or result buffer must have been previously allocated on the required system using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) or [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md).

### `PathOrTemplateIndex` *(in, AIL_INT64)*

Specifies to control a 3D measurement context, a path, a template, a default template, or a 3D measurement result buffer. Set this parameter to one of the following values:

*For specifying what to control*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If a 3D measurement context is specified, same as [`M_CONTEXT`](../../Reference/3dmeas/M3dmeasControl.md).

If a 3D measurement result buffer is specified, same as [`M_GENERAL`](../../Reference/3dmeas/M3dmeasControl.md). |
| `M_PATH_INDEX` | Specifies to control an individual path in a path 3D measurement context, if one is specified. |
| `M_TEMPLATE_INDEX` | Specifies to control an individual template in a template 3D measurement context, if one is specified. |
| `M_CONTEXT` | Specifies to control a global setting of a 3D measurement context, if one is specified. |
| `M_DEFAULT_TEMPLATE` | Specifies to control the default template in a profile 3D measurement context, if one is specified.

Note that the default template in a profile 3D measurement context is different from an explicitly defined template in a template 3D measurement context. The default template is inherent to the supplied profiles, such that it is the theoretical template that would result in these profiles. Note, unlike for the profiles perpendicular to the template in a template 3D measurement context, multiple markers can be found along the profiles perpendicular to the default template in a profile 3D measurement context. |
| `M_GENERAL` | Specifies to control a general setting of a 3D measurement result buffer, if one is specified. |

### `ProfileIndex` *(in, AIL_INT64)*

Specifies the profile to control, if required. Set this parameter to one of the following values:

*For specifying a profile*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the index of a profile is not required. |
| `M_PROFILE_INDEX` | Specifies to control an individual profile in a template 3D measurement context, if one is specified. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### To control a global setting of a profile, path, or template 3D measurement context

The following [`ControlType`](../../Reference/3dmeas/M3dmeasControl.md) and [`ControlValue`](../../Reference/3dmeas/M3dmeasControl.md) parameter setting can be specified to control a global setting of a profile, path, or template 3D measurement context. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md) or [`M_CONTEXT`](../../Reference/3dmeas/M3dmeasControl.md), and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md).

---

### `M_TIMEOUT`

Sets the maximum amount of time for [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) to complete the find marker operation before generating a time-out error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no timeout value. |
| `Value > 0.0` | Specifies the timeout value, in msec. |

### To control the default template in a profile 3D measurement context, path in a path 3D measurement context, or template in a template 3D measurement context

The following [`ControlType`](../../Reference/3dmeas/M3dmeasControl.md) and [`ControlValue`](../../Reference/3dmeas/M3dmeasControl.md) parameter settings can be specified for the default template in a profile 3D measurement context, a path in a path 3D measurement context, or a template in a template 3D measurement context. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT_TEMPLATE`](../../Reference/3dmeas/M3dmeasControl.md), the index of a path, or the index of a template, respectively, and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md).

---

### `M_MARKER_DEPTH_MIN`

Sets the minimum depth required for a peak or valley in the profile response, for it to be considered a valid transition signifying a marker.  The edge profile response is established by taking the normalized first derivative of the profile, which generally resembles a peak when a valid edge transition is present. The depth of an edge transition is measured as the difference between the Z-coordinates at the start and end of the transition.  The ridge profile response is established by taking the normalized second derivative of the profile, which generally resembles a peak or valley when a valid ridge transition is present. The depth of a ridge transition is measured as the difference between the Z-coordinate at the summit of the peak/bottom of the valley and the closest Z-coordinate at the start or end of the transition.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum depth.  A setting of 0.0 indicates that Aurora Imaging Library considers all depths. |

---

### `M_MARKER_NUMBER`

Sets the number of markers to search for in the profile.  Note that this control type is not available for a template 3D measurement context. In this case, the find marker operation always looks for 1 marker in each perpendicular profile of the template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to search for all markers. |
| `Value > 0` *(default)* | Specifies the number of markers for which to search. |

---

### `M_MARKER_NUMBER_TRANSITION_INSIDE`

Sets the number of transitions to find inside a marker. Note that border transitions are not included.  > **Note:** This control type is only available when [`M_MARKER_TYPE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to find markers with any number of transitions. |
| `Value > 0` | Specifies the number of transitions. |

---

### `M_MARKER_POLARITY`

Sets the polarity of the transition to find. This is the polarity of the sole transition for an [`M_SINGLE`](../../Reference/3dmeas/M3dmeasControl.md) marker and the first transition for an [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md) marker. Transitions from a lower Z-value to a higher Z-value, according to the direction of the search along the profile, have a positive polarity. Transitions from a higher Z-value to a lower one have a negative polarity. If the polarity does not matter, you can set this control to [`M_ANY`](../../Reference/3dmeas/M3dmeasControl.md) to find transitions with either a positive or negative polarity.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies to find a transition with any polarity. |
| `M_NEGATIVE` | Specifies to find a transition with negative polarity. |
| `M_POSITIVE` *(default)* | Specifies to find a transition with positive polarity. |

---

### `M_MARKER_POLARITY_SECOND`

Sets the polarity of the second transition to find. This is the polarity of the second transition for an [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md) marker. Transitions from a lower Z-value to a higher Z-value, according to the direction of the search along the profile, have a positive polarity. Transitions from a higher Z-value to a lower one have a negative polarity. If the polarity does not matter, you can set this control to [`M_ANY`](../../Reference/3dmeas/M3dmeasControl.md) to find transitions with either a positive or negative polarity. If you want to find transitions with the same or opposite polarity as [`M_MARKER_POLARITY`](../../Reference/3dmeas/M3dmeasControl.md), you can set this control to [`M_SAME`](../../Reference/3dmeas/M3dmeasControl.md) or [`M_OPPOSITE`](../../Reference/3dmeas/M3dmeasControl.md), respectively.  > **Note:** This control type is only available when [`M_MARKER_TYPE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md).

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

Sets where to identify a transition when an invalid region is encountered in the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AFTER` | Specifies that the transition is immediately after the gap. |
| `M_BEFORE` | Specifies that the transition is immediately before the gap. |
| `M_BOTH` *(default)* | Specifies that there is one transition immediately before and one transition immediately after the gap. |
| `M_MIDDLE` | Specifies that the transition is in the middle of the gap. |

---

### `M_MARKER_POSITION_RELATIVE`

Sets the position that the markers should be closest to when [`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md) and the number of candidates is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md).  Note that this also sets the relative position to use when calculating the [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md) [`M_OFFSET`](../../Reference/3dmeas/M3dmeasGetResult.md) result type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the position, as a percentage of the nominal profile length. |

---

### `M_MARKER_SELECTION`

Sets the policy used to select found markers when the number of candidates is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BEST_FIT` | Specifies to use a different selection criteria depending on the operation.  When performing an [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) operation, [`M_BEST_FIT`](../../Reference/3dmeas/M3dmeasControl.md) specifies to select markers according to their proximity to the profile's origin; the closest marker is selected first.  When performing an [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) operation with a template 3D measurement context, [`M_BEST_FIT`](../../Reference/3dmeas/M3dmeasControl.md) specifies to select markers according to possible resulting fit scores; the marker that would result in the best fit possible is selected first. |
| `M_CLOSEST_TO_SPACING` | Specifies to select markers according to their similarity to the spacing set using [`M_MARKER_SPACING_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md); the marker with the closest spacing is selected first. |
| `M_CLOSEST_TO_WIDTH` | Specifies to select markers according to their similarity to the width set using [`M_MARKER_WIDTH_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md); the marker with the closest width is selected first. |
| `M_NARROWEST` | Specifies to select markers according to their width; the narrowest marker is selected first. |
| `M_POSITION_FIRST` | Specifies to select markers according to their proximity to the profile's origin; the closest marker is selected first. |
| `M_POSITION_LAST` | Specifies to select markers according to their proximity to the end of the profile; the closest marker is selected first. |
| `M_POSITION_RELATIVE` | Specifies to select markers according to their proximity to the position set using [`M_MARKER_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md); the closest marker is selected first. |
| `M_STRONGEST` *(default)* | Specifies to select markers according to the strength of the transition; the marker with the strongest transition is selected first. |
| `M_TALLEST` | Specifies to select markers according to their measured depth; the marker with the greatest depth is selected first. |
| `M_WIDEST` | Specifies to select markers according to their width; the widest marker is selected first. |

---

### `M_MARKER_SPACING_RELATIVE`

Sets the spacing that the marker's spacing should be closest to when [`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_CLOSEST_TO_SPACING`](../../Reference/3dmeas/M3dmeasControl.md) and the number of candidates is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md). The spacing of a marker is the distance between the center of the previous marker and the center of the next marker.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the spacing, as a percentage of the nominal profile length. |

---

### `M_MARKER_STRENGTH_MIN`

Sets the minimum strength required for a peak or valley in the profile response, for it to be considered a valid transition signifying a marker.  The edge profile response is established by taking the normalized first derivative of the profile, which generally resembles a peak when a valid edge transition is present.  The ridge profile response is established by taking the normalized second derivative of the profile, which can resemble a peak or valley when a valid ridge transition is present.  For peaks, the summit of the highest peak that rises above and then falls below the minimum strength on both sides of the peak represents the transition's position. For valleys, the transition's position is at the bottom of the lowest valley that falls below and then rises above the negative of the minimum strength on both sides of the valley. The strength is the absolute value at these positions.  *[Image: 3dmeas_min_strength.png]*  When searching for multiple transitions, you might need to further constrain the strengths that Aurora Imaging Library considers, using [`M_MARKER_STRENGTH_MIN_VAR`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum strength.  A setting of 0.0 indicates that Aurora Imaging Library considers all strengths, while a setting of 100.0 indicates that Aurora Imaging Library only considers the greatest possible strength. The greatest possible strength results from a perfectly straight transition from the lowest Z-value to the highest Z-value (or vice versa) over one pixel in the profile response image. If you set the minimum strength to a very low value, [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) can take longer to execute. |

---

### `M_MARKER_STRENGTH_MIN_VAR`

Sets the minimum prominence required for a peak or valley in the profile response, for it to be considered a valid transition signifying a marker. This control is useful for advanced applications where you are trying to find multiple neighboring transitions in the profile, and [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md) does not give you enough control over identifying the required transitions.  To determine if the minimum prominence is met, Aurora Imaging Library establishes a local threshold for each peak or valley in the profile response. A peak's threshold is its value minus [`M_MARKER_STRENGTH_MIN_VAR`](../../Reference/3dmeas/M3dmeasControl.md). A valley's threshold is its value plus [`M_MARKER_STRENGTH_MIN_VAR`](../../Reference/3dmeas/M3dmeasControl.md).  *[Image: 3dmeas_min_strength_var.png]*  The portion of the profile response above the local threshold that contains the peak, or below the local threshold that contains the valley, can be referred to as the interval of the peak/valley, provided that the profile response eventually returns to the other side of the local threshold on both sides. For Aurora Imaging Library to consider a peak or valley to be a valid transition, there must be no strength greater than it within its interval. For peaks, if the local threshold calculated with [`M_MARKER_STRENGTH_MIN_VAR`](../../Reference/3dmeas/M3dmeasControl.md) is less than [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md), the local threshold equals [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md). For valleys, the local threshold equals the negative of [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md) if it is greater.  Note that this control type is most useful for edge transitions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum prominence.  A setting of 0.0 indicates no prominence; that is, Aurora Imaging Library only considers the maximum strength of the peak/valley's interval. A setting of 100.0 (default) indicates the greatest possible prominence; that is, Aurora Imaging Library considers all strengths of the interval that exceed [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md). In this case, [`M_MARKER_STRENGTH_MIN_VAR`](../../Reference/3dmeas/M3dmeasControl.md) has no effect. |

---

### `M_MARKER_TRANSITION`

Sets the type of transition to find.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE` *(default)* | Specifies to find edge transitions. |
| `M_EDGE_OR_INVALID` | Specifies to find edge transitions and invalid transitions. Use [`M_MARKER_POSITION_INVALID`](../../Reference/3dmeas/M3dmeasControl.md) to specify whether invalid transitions are identified before, after, or in the middle of gaps encountered in the profile. |
| `M_INVALID` | Specifies to find invalid transitions. Use [`M_MARKER_POSITION_INVALID`](../../Reference/3dmeas/M3dmeasControl.md) to specify whether invalid transitions are identified before, after, or in the middle of gaps encountered in the profile. |
| `M_RIDGE` | Specifies to find ridge transitions. |

---

### `M_MARKER_TYPE`

Sets the type of marker to extract from the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PAIR` | Specifies to extract markers represented by a pair of transitions. |
| `M_SINGLE` *(default)* | Specifies to extract markers represented by a single transition. |

---

### `M_MARKER_WIDTH_RELATIVE`

Sets the width that the marker's width should be closest to when [`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_CLOSEST_TO_WIDTH`](../../Reference/3dmeas/M3dmeasControl.md) and the number of candidates is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md). The width of an [`M_SINGLE`](../../Reference/3dmeas/M3dmeasControl.md) marker is the distance between the start and end of the transition. The width of an [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md) marker is the distance between the center of the first transition and center of the second transition.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the width, as a percentage of the nominal profile length. |

---

### `M_PROFILE_FILTER_SMOOTHNESS`

Sets the normalized degree of smoothness (strength of denoising) applied to the profile to create the profile response before finding the markers. Increasing the smoothness value reduces transitions caused by noise, lowering the risk of finding false markers. It can also increase the strength of gradual transitions, allowing some previously undetected markers to be found.  > **Note:** This control type is only used when [`M_PROFILE_FILTER_SMOOTHNESS_TYPE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_NORMALIZED`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value.  A value of 100.0 results in a strong noise reduction effect, while a value of 0.0 has almost no noise reduction effect. |

---

### `M_PROFILE_FILTER_SMOOTHNESS_SIZE`

Sets the smoothness size applied to the profile to create the profile response before finding the markers.  A size close to 3 times the transition's width ([`M_WIDTH`](../../Reference/3dmeas/M3dmeasGetResult.md) * 3) is recommended, particularly for ridge transitions. If the size is too large, transition strengths will decrease.  For edge transitions, increasing the smoothness size reduces transitions caused by noise, lowering the risk of finding false markers. It can also increase the strength of gradual transitions, allowing some previously undetected markers to be found.  > **Note:** This control type is only used when [`M_PROFILE_FILTER_SMOOTHNESS_TYPE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_SIZE`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 2.0` *(default)* | Specifies the smoothness size. |

---

### `M_PROFILE_FILTER_SMOOTHNESS_TYPE`

Sets the filter smoothness type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NORMALIZED` *(default)* | Specifies to apply a normalized filter smoothness, set with [`M_PROFILE_FILTER_SMOOTHNESS`](../../Reference/3dmeas/M3dmeasControl.md). |
| `M_SIZE` | Specifies to apply a filter smoothness size, set with [`M_PROFILE_FILTER_SMOOTHNESS_SIZE`](../../Reference/3dmeas/M3dmeasControl.md). |

---

### `M_PROFILE_GAP_FILL_SHARP_ELEVATION`

Sets how to fill sharp elevation gaps. A gap is considered a sharp elevation gap if the difference between the values on either side of the gap is at least [`M_PROFILE_GAP_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dmeas/M3dmeasControl.md). You can specify to propagate the boundary value before or after the gap, propagate the minimum or maximum gap boundary value, propagate both boundary values up to the middle of the gap, or not to fill sharp elevation gaps.

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

Sets the threshold used to differentiate between gradual elevation gaps and sharp elevation gaps. A gap is an invalid region in the profile resulting from missing data in the source depth map. If the difference between a gap's boundary values is less than the specified threshold, the gap is considered a gradual elevation gap, otherwise it is considered a sharp elevation gap.  Gradual elevation gaps are filled using linear interpolation; sharp elevation gaps are either filled by propagating the boundary values or are left unfilled, depending on the [`M_PROFILE_GAP_FILL_SHARP_ELEVATION`](../../Reference/3dmeas/M3dmeasControl.md) control type. It is often better to fill gradual elevation gaps using linear interpolation since the underlying surface of such a gap is considered to be part of the same surface as its boundaries. Whereas for sharp elevation gaps, the underlying surface is considered to be discontinuous at least at one of its boundaries, so propagating the value of one of the boundaries is recommended.  If a gap is greater than [`M_PROFILE_GAP_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dmeas/M3dmeasControl.md), it is replaced using the secondary algorithm described in [`M_PROFILE_GAP_FILL_SHARP_ELEVATION`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the threshold is infinite.  When [`M_INFINITE`](../../Reference/3dmeas/M3dmeasControl.md) is specified, all gaps are considered gradual elevation gaps, so linear interpolation is always used and [`M_PROFILE_GAP_FILL_SHARP_ELEVATION`](../../Reference/3dmeas/M3dmeasControl.md) is ignored. |
| `Value >= 0.0` | Specifies the threshold, as a percentage of the depth map's Z-range or in world units, depending on the units specified with [`M_PROFILE_GAP_FILL_SHARP_ELEVATION_DEPTH_UNITS`](../../Reference/3dmeas/M3dmeasControl.md).  When the threshold is set to 0.0, all gaps are considered sharp elevation gaps, so linear interpolation is never used and all gaps are filled according to [`M_PROFILE_GAP_FILL_SHARP_ELEVATION`](../../Reference/3dmeas/M3dmeasControl.md). |

---

### `M_PROFILE_GAP_FILL_SHARP_ELEVATION_DEPTH_UNITS`

Sets the units with which to interpret the [`M_PROFILE_GAP_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dmeas/M3dmeasControl.md) control type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PERCENTAGE` *(default)* | Specifies to interpret the value as a percentage of the range of Z-values (Z-range) visible in the depth map. |
| `M_WORLD` | Specifies to interpret the value in world units. |

---

### `M_PROFILE_GAP_FILL_THRESHOLD`

Sets the maximum size of gaps that will be filled. Gaps are invalid regions in the profile where the 3D sensor could not pick up information, due to occlusion, reflections, or areas out of the camera's field of view.  If a gap in the profile is larger than the fill threshold, it is not replaced.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies no maximum gap size. |
| `Value >= 0.0` *(default)* | Specifies the maximum gap size, in sample or world units, depending on the units specified with [`M_PROFILE_GAP_FILL_THRESHOLD_UNITS`](../../Reference/3dmeas/M3dmeasControl.md). |

---

### `M_PROFILE_GAP_FILL_THRESHOLD_UNITS`

Sets the units with which to interpret the [`M_PROFILE_GAP_FILL_THRESHOLD`](../../Reference/3dmeas/M3dmeasControl.md) control type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SAMPLE` *(default)* | Specifies to interpret the value relative to the sampling distance ([`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasControl.md)). |
| `M_WORLD` | Specifies to interpret the value in world units. |

### To control the profile extraction along a path in a path 3D measurement context or perpendicular to the template in a template 3D measurement context

The following [`ControlType`](../../Reference/3dmeas/M3dmeasControl.md) and [`ControlValue`](../../Reference/3dmeas/M3dmeasControl.md) parameter settings can be specified for a path in a path 3D measurement context or a template in a template 3D measurement context. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to the index of a path or template, respectively, and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md). These control types are used to specify how to extract the profile along the path or the profiles at regular intervals perpendicular to the template. Note that the sampling distance determines where each profile point will be extracted. The points within the thickness of the profile line at each of these positions are considered a sample, and a single profile point is calculated from the points in each sample.

---

### `M_PROFILE_ACCUMULATE_TYPE`

Sets how points in a given sample are accumulated to create a single profile point when there are multiple points within the thickness of the profile line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX` | Specifies to use the largest value of all the points in the sample.  > **Note:** This option is not supported when [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_AVERAGE`](../../Reference/3dmeas/M3dmeasControl.md). |
| `M_MEAN` *(default)* | Specifies to use the mean value of all the points in the sample. |
| `M_MIN` | Specifies to use the smallest value of all the points in the sample.  > **Note:** This option is not supported when [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_AVERAGE`](../../Reference/3dmeas/M3dmeasControl.md). |

---

### `M_PROFILE_INTERPOLATION_AVERAGE_FRACTION`

Sets the fraction of the sampling distance to use when taking a weighted average of the area surrounding the profile point. For example, a value of 0.5 indicates that 50 percent of the pixel area that surrounds the profile point (defined by 50% of the sampling distance along the line) is used to calculate a weighted average for the profile point.  > **Note:** This control type is only available when [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_AVERAGE`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 1.0` *(default)* | Specifies the fraction. |

---

### `M_PROFILE_INTERPOLATION_MODE`

Sets the interpolation mode used to calculate the value for each point in the sample that will be used to create the profile point. Note that when averaging interpolation is used ([`M_AVERAGE`](../../Reference/3dmeas/M3dmeasControl.md)), the profile point is calculated without calculating the value for each point in the sample.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` | Specifies averaging interpolation. The value of the profile point is determined by taking a weighted average of the pixel area that surrounds the profile point. You can use [`M_PROFILE_INTERPOLATION_AVERAGE_FRACTION`](../../Reference/3dmeas/M3dmeasControl.md) to specify how much of the pixel area to consider. |
| `M_BILINEAR` *(default)* | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the point in the sample. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the point in the sample. |

---

### `M_PROFILE_MIN_VALID_PERCENTAGE`

Sets the minimum percentage of points in a given sample that must be valid to set the corresponding profile point to a valid value. When there are multiple points within the thickness for the sample that will be used to create the profile point, the profile point will be invalid if the minimum valid percentage is not met.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the percentage of points in the sample. Note that a value of 0.0% means there are no constraints on the number of valid points, but if all points contributing to the calculation of a profile point are invalid, the profile point will be invalid in the destination. |

---

### `M_PROFILE_PROJECTION_ANGLE`

Sets the nominal angle at which the projection is done for a given profile. This is the angle, relative to the profile line, from which samples are taken for a profile point. The projection angle determines the angle of the markers that are found.  You should set this control type to the angle at which you expect to find the markers, relative to the profile line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `10.0 <= Value <= 170.0` *(default)* | Specifies the nominal projection angle, in degrees. |

---

### `M_PROFILE_PROJECTION_ANGLE_ACCURACY`

Sets the accuracy of the projection angle. The accuracy defines an angular range in which to fine-tune the projection after the approximate angle with the best markers is found. To be effective, you must set the degree of accuracy to a value smaller than that of the rotation tolerance.  When [`M_PROFILE_PROJECTION_ANGLE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is disabled, this setting has no effect.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 170.0` *(default)* | Specifies the accuracy, in degrees. |

---

### `M_PROFILE_PROJECTION_ANGLE_DELTA_NEG`

Sets the lower limit of the projection angle range, relative to the nominal projection angle ([`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasControl.md)). That is, _LowerLimit_ = [`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasControl.md)  - [`M_PROFILE_PROJECTION_ANGLE_DELTA_NEG`](../../Reference/3dmeas/M3dmeasControl.md).  Note that the minimum projection angle considered is 10.0°, even if [`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasControl.md) - [`M_PROFILE_PROJECTION_ANGLE_DELTA_NEG`](../../Reference/3dmeas/M3dmeasControl.md) is less than 10.0°.  When [`M_PROFILE_PROJECTION_ANGLE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is disabled, this setting has no effect.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 170.0` *(default)* | Specifies the lower limit of the projection angle range, in degrees. |

---

### `M_PROFILE_PROJECTION_ANGLE_DELTA_POS`

Sets the upper limit of the projection angle range, relative to the nominal projection angle ([`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasControl.md)). That is, _UpperLimit_ = [`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasControl.md)  + [`M_PROFILE_PROJECTION_ANGLE_DELTA_POS`](../../Reference/3dmeas/M3dmeasControl.md).  Note that the maximum projection angle considered is 170.0°, even if [`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasControl.md) + [`M_PROFILE_PROJECTION_ANGLE_DELTA_POS`](../../Reference/3dmeas/M3dmeasControl.md) is greater than 170.0°.  When [`M_PROFILE_PROJECTION_ANGLE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is disabled, this setting has no effect.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 170.0` *(default)* | Specifies the upper limit of the projection angle range, in degrees. |

---

### `M_PROFILE_PROJECTION_ANGLE_MODE`

Sets whether to consider projection angles within an angular range when extracting the profile to search. The chosen projection angle is determined based on the number of markers found and the average strength of those markers.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to use multiple projection angles. |
| `M_ENABLE` | Specifies to use multiple projection angles. |

---

### `M_PROFILE_PROJECTION_ANGLE_TOLERANCE`

Sets the tolerance used to determine the projection angles. The tolerance determines the spacing of the first phase of the angle refinement. The angle refinement is performed at the nominal projection angle and at every +/- [`M_PROFILE_PROJECTION_ANGLE_TOLERANCE`](../../Reference/3dmeas/M3dmeasControl.md) degrees within the projection angle range.  When [`M_PROFILE_PROJECTION_ANGLE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is disabled, this setting has no effect.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 170.0` *(default)* | Specifies the tolerance, in degrees. |

---

### `M_PROFILE_SAMPLE_SIZE`

Sets the sampling distance between two consecutive points along the profile line. When the pixel size in X is not equal to the pixel size in Y in the source depth map, this ensures that the extracted points are at a uniform distance.  Note that this will be the pixel size of the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the distance. Use [`M_PROFILE_SAMPLE_SIZE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to specify how to interpret the specified sampling distance. |

---

### `M_PROFILE_SAMPLE_SIZE_MODE`

Sets how to interpret the specified sampling distance ([`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasControl.md)).  The [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md) modes allow the sampling distance to be dependent on the depth map calibration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the sampling distance directly, without applying a multiplying factor. For example, if you set [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0, the sampling distance will be 2.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the sampling distance by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` *(default)* | Specifies to multiply the sampling distance by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the sampling distance by the pixel size in X. For example, if you set [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in X is 0.5 mm, the sampling distance will be 1.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the sampling distance by the pixel size in Y. For example, if you set [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in Y is 0.6 mm, the sampling distance will be 1.2 mm. |

---

### `M_PROFILE_THICKNESS`

Sets the thickness of the profile line. Pixels along the line, within the specified thickness, are sampled and corresponding points are considered for the profile.  Note that a thick profile line typically results in multiple points contributing to the same profile point. You can use [`M_PROFILE_ACCUMULATE_TYPE`](../../Reference/3dmeas/M3dmeasControl.md) to determine how multiple point values are combined to set the value of the corresponding profile point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the thickness. Use [`M_PROFILE_THICKNESS_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to specify how to interpret the specified thickness. |

---

### `M_PROFILE_THICKNESS_MODE`

Sets how to interpret the specified thickness value ([`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md)).  The [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md) modes allow the thickness to be dependent on the depth map calibration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the thickness value directly, without applying a multiplying factor. For example, if you set [`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0, the profiles will be 2.0 mm thick. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the thickness value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` *(default)* | Specifies to multiply the thickness value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the thickness value by the pixel size in X. For example, if you set [`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in X is 0.5 mm, the profiles will be 1.0 mm thick. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the thickness value by the pixel size in Y. For example, if you set [`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in Y is 0.6 mm, the profiles will be 1.2 mm thick. |
| `M_RELATIVE_TO_SPACING` | Specifies to multiply the thickness value by the profile spacing. For example, if you set [`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the spacing between profiles is 2.0 mm, the profiles will be 4.0 mm thick.  Note that this value is only available if a template 3D measurement context is specified and [`M_TEMPLATE_PROFILE_SPACING_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is not set to [`M_RELATIVE_TO_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md). |

### To control a template in a template 3D measurement context

The following [`ControlType`](../../Reference/3dmeas/M3dmeasControl.md) and [`ControlValue`](../../Reference/3dmeas/M3dmeasControl.md) parameter settings can be specified for a template in a template 3D measurement context. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to the index of a template, and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md).

---

### `M_TEMPLATE_EXTENDED_SEARCH`

Sets whether to extend the search outside of the template's defined area. When this control type is enabled, the profiles are positioned normally and all data within the profile's thickness, including data outside of the template, is considered. When this control type is disabled, the first profile will be offset towards the center of the template such that its thickness does not extend outside the template, meaning data outside of the template's defined area is never considered.  *[Image: 3dmeas_extended_search.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to extend the search beyond the template. |
| `M_ENABLE` *(default)* | Specifies to extend the search beyond the template. |

---

### `M_TEMPLATE_PROFILE_NUMBER_ELEMENTS`

Sets the number of profile elements in the template 3D measurement context. Each profile element contains the settings for a single profile. If you reduce the number of profile elements, the settings of the ones that remain are kept unchanged. If you increase the number of profile elements, the new ones are initialized with the default settings.  You should have at least as many profile elements as the number of profiles to extract. You configure profile elements by setting the [`M_TEMPLATE_PROFILE_..._SOURCE`](../../Reference/3dmeas/M3dmeasControl.md) controls to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasControl.md) and setting the corresponding control in the [`TemplateOrProfileInTemplateContext`](../../Reference/TemplateOrProfileInTemplateContext.md) table.  Note this value is not available when [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_SPACING`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= M_TEMPLATE_PROFILE_NUMBER_VALUE` *(default)* | Specifies the number of profile elements. |

---

### `M_TEMPLATE_PROFILE_NUMBER_MODE`

Sets the mode with which to determine the number of profiles to use to find the markers of a template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SPACING` | Specifies that the number of profiles is determined based on the spacing between the profiles. |
| `M_USER_DEFINED` *(default)* | Specifies to use the value set with [`M_TEMPLATE_PROFILE_NUMBER_VALUE`](../../Reference/3dmeas/M3dmeasControl.md). The distance between profiles is established by distributing the specified number of profiles evenly perpendicular to the template. |

---

### `M_TEMPLATE_PROFILE_NUMBER_VALUE`

Sets the number of profiles perpendicular to the template. The distance between profiles is established by evenly distributing the specified number of profiles.  Note this value is only available when [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 < Value <= M_TEMPLATE_PROFILE_NUMBER_ELEMENTS` *(default)* | Specifies the number of profiles. |

---

### `M_TEMPLATE_PROFILE_SPACING`

Sets the spacing between the profiles along the template.  Note that this control type is only used if [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_SPACING`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the spacing. Use [`M_TEMPLATE_PROFILE_SPACING_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to specify how to interpret the specified spacing. |

---

### `M_TEMPLATE_PROFILE_SPACING_MODE`

Sets how to interpret the specified spacing value ([`M_TEMPLATE_PROFILE_SPACING`](../../Reference/3dmeas/M3dmeasControl.md)).  The [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md) modes allow the spacing to be dependent on the depth map calibration.  Note that this control type is only used if [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_SPACING`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the spacing value directly, without applying a multiplying factor. For example, if you set [`M_TEMPLATE_PROFILE_SPACING`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0, the spacing between profiles will be 2.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the spacing value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` | Specifies to multiply the spacing value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the spacing value by the pixel size in X. For example, if you set [`M_TEMPLATE_PROFILE_SPACING`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in X is 0.5 mm, the spacing between profiles will be 1.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the spacing value by the pixel size in Y. For example, if you set [`M_TEMPLATE_PROFILE_SPACING`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in Y is 0.6 mm, the spacing between profiles will be 1.2 mm. |
| `M_RELATIVE_TO_THICKNESS` *(default)* | Specifies to multiply the spacing value by the profile thickness. For example, if you set [`M_TEMPLATE_PROFILE_SPACING`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the profile thickness is 2.0 mm, the spacing between profiles will be 4.0 mm. |

### To control a template or profile in a template 3D measurement context

The following [`ControlType`](../../Reference/3dmeas/M3dmeasControl.md) and [`ControlValue`](../../Reference/3dmeas/M3dmeasControl.md) parameter settings can be specified for a template or profile in a template 3D measurement context. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to the index of a template. If [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md), these control types allow you to specify global settings for the template's perpendicular profiles. If [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) is set to a specific index, they allow you to specify custom settings for the specified profile. Note that you must set the corresponding [`M_TEMPLATE_PROFILE_..._SOURCE`](../../Reference/3dmeas/M3dmeasControl.md) control to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasControl.md) for the custom settings of the profile to be used.

---

### `M_TEMPLATE_PROFILE_LENGTH`

Sets the nominal length of the profile to extract (perpendicular to the template). You should set the nominal length large enough to include the expected position variation of the marker. If you want to return a transition even when it is outside the expected range of positions, you can add a margin to the length, using [`M_TEMPLATE_PROFILE_LENGTH_MARGIN`](../../Reference/3dmeas/M3dmeasControl.md), and Aurora Imaging Library will continue the search within the margin.  *[Image: 3dmeas_profile_length_margin.png]*  Note that if you are setting the nominal length for a specific profile, you must set [`M_TEMPLATE_PROFILE_LENGTH_SOURCE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasControl.md) to use the custom length value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the length. Use [`M_TEMPLATE_PROFILE_LENGTH_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to specify how to interpret the specified length. |

---

### `M_TEMPLATE_PROFILE_LENGTH_MARGIN`

Sets a margin on the profile length. You should include a margin when you still want to find a transition when it is located outside of the positions that you want to verify. The nominal length, specified with [`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasControl.md), plus the specified margin define the length of the profile.  *[Image: 3dmeas_profile_length_margin.png]*  Note that if you are setting the margin for a specific profile, you must set [`M_TEMPLATE_PROFILE_LENGTH_MARGIN_SOURCE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasControl.md) to use the custom margin value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the margin, as a percentage of the nominal length. |

---

### `M_TEMPLATE_PROFILE_LENGTH_MODE`

Sets how to interpret the specified length value ([`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasControl.md)).  The [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md) modes allow the profile length to be dependent on the depth map calibration.  Note that if you are setting the length mode for a specific profile, you must set [`M_TEMPLATE_PROFILE_LENGTH_SOURCE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasControl.md) to use the custom length mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the length value directly, without applying a multiplying factor. For example, if you set [`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0, the profile length will be 2.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the length value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` *(default)* | Specifies to multiply the length value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the length value by the pixel size in X. For example, if you set [`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in X is 0.5 mm, the profile length will be 1.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the length value by the pixel size in Y. For example, if you set [`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in Y is 0.6 mm, the profile length will be 1.2 mm. |

---

### `M_TEMPLATE_PROFILE_POSITION_RELATIVE`

Sets the position of the profile, relative to the template. By default, the profiles are centered on the template. You must specify the position as a percentage of the nominal length of the profile (not including the margin). For example, if a profile has a nominal length of 10 mm and you specify a value of 50.0%, the profile is moved 5 mm backwards, such that the end of its nominal length touches the template; if a margin was specified, it will continue across the template. If you specify a value of -50.0%, the profile is moved 5 mm forwards, such that the start of its nominal length touches the template.  *[Image: 3dmeas_position_relative.png]*  Note that if you are setting the relative position for a specific profile, you must set [`M_TEMPLATE_PROFILE_POSITION_SOURCE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasControl.md) to use the custom relative position value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `-50.0 <= Value <= 50.0` *(default)* | Specifies the relative position, as a percentage of the nominal length. |

### To control a profile in a template 3D measurement context

The following [`ControlType`](../../Reference/3dmeas/M3dmeasControl.md) and [`ControlValue`](../../Reference/3dmeas/M3dmeasControl.md) parameter settings can be specified for a profile perpendicular to a template in a template 3D measurement context. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to the index of a template, and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to the index of one or all profiles. These control types allow you to specify whether to use the global or custom settings for the specified profile, when extracting the profile perpendicular to the template.

---

### `M_TEMPLATE_PROFILE_LENGTH_MARGIN_SOURCE`

Sets whether to use the global margin value or a custom margin for the profile. The margin is added to the nominal length to define the length of the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PROFILE` | Specifies to use the custom margin value for the specified profile. You must set the custom margin using [`M_TEMPLATE_PROFILE_LENGTH_MARGIN`](../../Reference/3dmeas/M3dmeasControl.md) with [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) set to the index of the profile. |
| `M_TEMPLATE` *(default)* | Specifies to use the template's global margin value for the specified profile. You must set the template's global margin using [`M_TEMPLATE_PROFILE_LENGTH_MARGIN`](../../Reference/3dmeas/M3dmeasControl.md) with [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md). |

---

### `M_TEMPLATE_PROFILE_LENGTH_SOURCE`

Sets whether to use the global nominal length value and length mode or a custom nominal length and length mode for the profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PROFILE` | Specifies to use the custom profile length for the specified profile. You must set the custom length using [`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasControl.md) and [`M_TEMPLATE_PROFILE_LENGTH_MODE`](../../Reference/3dmeas/M3dmeasControl.md) with [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) set to the index of the profile. |
| `M_TEMPLATE` *(default)* | Specifies to use the template's global profile length for the specified profile. You must set the template's global length using [`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasControl.md) and [`M_TEMPLATE_PROFILE_LENGTH_MODE`](../../Reference/3dmeas/M3dmeasControl.md) with [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md). |

---

### `M_TEMPLATE_PROFILE_POSITION_SOURCE`

Sets whether to use the global relative position value or a custom relative position for the profile. The relative position is the position of the profile relative to the template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PROFILE` | Specifies to use the custom relative position value for the specified profile. You must set the custom relative position using [`M_TEMPLATE_PROFILE_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md) with [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) set to the index of the profile. |
| `M_TEMPLATE` *(default)* | Specifies to use the template's global relative position value. You must set the template's global relative position using [`M_TEMPLATE_PROFILE_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md) with [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md). |

### To control a fit 3D measurement context

The following [`ControlType`](../../Reference/3dmeas/M3dmeasControl.md) and [`ControlValue`](../../Reference/3dmeas/M3dmeasControl.md) parameter settings can be specified for a fit 3D measurement context. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md) or [`M_GENERAL`](../../Reference/3dmeas/M3dmeasControl.md), and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md).

---

### `M_EXPECTED_OUTLIER_PERCENTAGE`

Sets the expected percentage of outlier markers among the markers that are extracted.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value < 100.0` *(default)* | Specifies the expected outliers, as a percentage. |

---

### `M_FIT_COVERAGE_MIN`

Sets the minimum percentage of profiles that must have a marker covered by a fitted point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the minimum coverage, as a percentage. |

---

### `M_FIT_DISTANCE`

Sets the distance value that Aurora Imaging Library uses to establish outlier markers, which are excluded from the robust best fit.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that the fit distance is infinite.  When [`M_INFINITE`](../../Reference/3dmeas/M3dmeasControl.md) is specified, no markers are considered outliers, so no markers are excluded from the robust best fit. |
| `Value > 0.0` | Specifies the fit distance. |

---

### `M_FIT_DISTANCE_MODE`

Sets how to interpret the specified fit distance value ([`M_FIT_DISTANCE`](../../Reference/3dmeas/M3dmeasControl.md)).  The [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md) modes allow the fit distance to be dependent on the depth map calibration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the fit distance value directly, without applying a multiplying factor. For example, if you set [`M_FIT_DISTANCE`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0, the fit distance will be 2.0 mm. |
| `M_AUTO` *(default)* | Specifies to automatically calculate the fit distance; the distance is estimated from all the extracted markers.  Note that when the find and fit are performed together (by calling [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) with a template 3D measurement context and a depth map), and there are multiple valid transitions within a single profile, the automatic fit distance can be overestimated. In this case, you should use a different mode. |
| `M_RELATIVE_TO_PIXEL_SIZE_MAX` | Specifies to multiply the fit distance value by the pixel size in X or in Y, depending on which is bigger. |
| `M_RELATIVE_TO_PIXEL_SIZE_MIN` | Specifies to multiply the fit distance value by the pixel size in X or in Y, depending on which is smaller. |
| `M_RELATIVE_TO_PIXEL_SIZE_X` | Specifies to multiply the fit distance value by the pixel size in X. For example, if you set [`M_FIT_DISTANCE`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in X is 0.5 mm, the fit distance will be 1.0 mm. |
| `M_RELATIVE_TO_PIXEL_SIZE_Y` | Specifies to multiply the fit distance value by the pixel size in Y. For example, if you set [`M_FIT_DISTANCE`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the pixel size in Y is 0.6 mm, the fit distance will be 1.2 mm. |
| `M_RELATIVE_TO_SAMPLING` | Specifies to multiply the fit distance value by the sampling distance along each profile. For example, if you set [`M_FIT_DISTANCE`](../../Reference/3dmeas/M3dmeasControl.md) to 2.0 and the sampling distance along each profile is 2.0 mm, the fit distance will be 4.0 mm. |

---

### `M_REMOVE_OUTLIERS`

Sets whether to remove outlier markers from the template 3D measurement result buffer after the fit. Note that when the find and fit are performed simultaneously, using [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) with a template 3D measurement context and a depth map, the outliers are always removed, even if this control type is set to [`M_DISABLE`](../../Reference/3dmeas/M3dmeasControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to remove outliers. |
| `M_ENABLE` | Specifies to remove outliers. |

### To control a profile, path, or template 3D measurement result buffer

The following [`ControlType`](../../Reference/3dmeas/M3dmeasControl.md) and [`ControlValue`](../../Reference/3dmeas/M3dmeasControl.md) parameter setting can be specified for a profile, path, or template 3D measurement result buffer. [`PathOrTemplateIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md) or [`M_GENERAL`](../../Reference/3dmeas/M3dmeasControl.md), and [`ProfileIndex`](../../Reference/3dmeas/M3dmeasControl.md) must be set to [`M_DEFAULT`](../../Reference/3dmeas/M3dmeasControl.md).

---

### `M_STOP_FIND`

Stops the current execution of [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md). This can only be done from another thread of higher priority. Results already available in the result buffer become invalid and will be discarded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |
