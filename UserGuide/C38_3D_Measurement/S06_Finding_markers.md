---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Measurement
section: Finding_markers
module_tag: 3dmeas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Measurement / Finding markers"
---

# Finding markers

You can search for markers in a depth map using [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md). Markers denote the location of transitions along a profile.

With successive calls to [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md), you can specify the characteristics of the markers you want to find. Aurora Imaging Library can only find a marker if it meets all the specified characteristics.

## Marker type

There are two types of markers in the 3D Measurement module: single markers and pair markers. Single markers consist of a single transition, allowing you to detect individual height changes or thin gaps. Pair markers group these transitions to detect a pattern in the object, such as bumps or larger gaps. Using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md), you can set [`M_MARKER_TYPE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_SINGLE`](../../Reference/3dmeas/M3dmeasControl.md) or [`M_PAIR`](../../Reference/3dmeas/M3dmeasControl.md) to search for single markers or pair markers, respectively.

## Transition type

You can use [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_TRANSITION`](../../Reference/3dmeas/M3dmeasControl.md) to specify whether to search for edge transitions, invalid transitions, both edge and invalid transitions, or ridge transitions. You should set [`M_MARKER_TRANSITION`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_EDGE`](../../Reference/3dmeas/M3dmeasControl.md) to identify object contours along the profile. An edge transition is a sudden, sustained change from a lower Z-value to a higher Z-value, or vice versa, depending on the specified polarity ([`M_MARKER_POLARITY`](../../Reference/3dmeas/M3dmeasControl.md)). See [Polarity](S06_Finding_markers.md) for more information. You should set [`M_MARKER_TRANSITION`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_INVALID`](../../Reference/3dmeas/M3dmeasControl.md) to identify invalid regions along the profile. Invalid regions in a profile result from missing data in the source depth map due to occlusion, reflections, or areas out of the camera's field of view. You can specify where to identify an invalid transition relative to the invalid region, using [`M_MARKER_POSITION_INVALID`](../../Reference/3dmeas/M3dmeasControl.md). See [Invalid position](S06_Finding_markers.md) for more information. Note that you can set [`M_MARKER_TRANSITION`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_EDGE_OR_INVALID`](../../Reference/3dmeas/M3dmeasControl.md) to find both edge transitions and invalid transitions. You should set [`M_MARKER_TRANSITION`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_RIDGE`](../../Reference/3dmeas/M3dmeasControl.md) to identify the center of thin, crest-like objects along the profile. A ridge transition is a sudden, temporary change in Z-values, where the depth first increases (or decreases), then returns to the starting depth.

## Minimum strength

The edge profile response is established by taking the normalized first derivative of the profile, whereas the ridge profile response is established by taking the normalized second derivative of the profile.

In the profile response, transitions appear as peaks or valleys. The minimum strength is a type of threshold characteristic that you can set to represent the strength above which a Z-value variation can be considered a valid transition signifying a marker. Any Z-value variation in the profile response that does not meet the specified minimum strength is therefore discarded when Aurora Imaging Library is searching for the marker. Although the default minimum strength is typically sufficient, you can change it using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md).

*[Image: 3dmeas_min_strength_peak.png]*

Aurora Imaging Library finds the highest peak among those for which the profile response rises above and then falls below the minimum strength on both sides, or the lowest valley among those for which the profile response falls below and then rises above the negative of the minimum strength on both sides. By default, only one possible transition is established from each interval above the specified minimum strength (for peaks) or below the negative of the specific minimum strength (for valleys).

*[Image: 3dmeas_min_strength.png]*

## Minimum strength variation

When searching for multiple transitions, some advanced applications might also need you to specify the minimum prominence required for a peak or valley for it to be considered a transition signifying a marker, using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_STRENGTH_MIN_VAR`](../../Reference/3dmeas/M3dmeasControl.md). For example, in the image above, you can see that [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md) does not give you enough control to establish 3 transitions.

A peak's prominence is the strength difference between the summit of the peak and the dip that connects it to a higher peak. A valley's prominence is the strength difference between the bottom of the valley and the rise that connects it to a lower valley. To determine if the minimum prominence is met, Aurora Imaging Library establishes a local threshold for each peak or valley in the profile response. A peak's threshold is its value minus [`M_MARKER_STRENGTH_MIN_VAR`](../../Reference/3dmeas/M3dmeasControl.md). A valley's threshold is its value plus [`M_MARKER_STRENGTH_MIN_VAR`](../../Reference/3dmeas/M3dmeasControl.md).

*[Image: 3dmeas_min_strength_var.png]*

The portion of the profile response above the local threshold that contains the peak, or below the local threshold that contains the valley, can be referred to as the interval of the peak/valley, provided that the profile response eventually returns to the other side of the local threshold on both sides. For Aurora Imaging Library to consider a peak or valley to be a valid transition, there must be no strength greater than it within its interval. For peaks, if the local threshold calculated with [`M_MARKER_STRENGTH_MIN_VAR`](../../Reference/3dmeas/M3dmeasControl.md) is less than [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md), the local threshold equals [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md). For valleys, the local threshold equals the negative of [`M_MARKER_STRENGTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md) if it is greater.

## Minimum depth

The depth of an edge transition is measured as the difference between the Z-coordinates at the start and end of the transition. The depth of a ridge transition is measured as the difference between the Z-coordinate at the summit of the peak/bottom of the valley and the closest Z-coordinate at the start or end of the transition.

To specify the minimum depth required for a peak or valley for it to be considered a transition signifying a marker, you can use [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_DEPTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md).

[`M_MARKER_DEPTH_MIN`](../../Reference/3dmeas/M3dmeasControl.md) is particularly useful for ridge transitions. There are typically 2 false ridge transitions, with a depth close to 0, on each side of a true ridge transition. Increasing the minimum depth causes these false transitions to be filtered out.

## Polarity

The polarity of a transition describes whether it is rising or falling, relative to the direction of the search along the profile. A rising transition denotes an increase in Z-value and a positive polarity, while a falling transition denotes a decrease in Z-value and a negative polarity. Use [`M_MARKER_POLARITY`](../../Reference/3dmeas/M3dmeasControl.md) to specify that the polarity of the transition must be positive ([`M_POSITIVE`](../../Reference/3dmeas/M3dmeasControl.md)) or negative ([`M_NEGATIVE`](../../Reference/3dmeas/M3dmeasControl.md)). You can also specify that the transition can have any polarity ([`M_ANY`](../../Reference/3dmeas/M3dmeasControl.md)), in which case Aurora Imaging Library does not consider polarity when trying to find the marker.

*[Image: 3dmeas_positive_polarity.png]*

*[Image: 3dmeas_negative_polarity.png]*

The example above illustrates the different markers found in each row (profile) of a depth map when [`M_MARKER_POLARITY`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_POSITIVE`](../../Reference/3dmeas/M3dmeasControl.md) (top) and [`M_NEGATIVE`](../../Reference/3dmeas/M3dmeasControl.md) (bottom).

When searching for pair markers, you can use [`M_MARKER_POLARITY_SECOND`](../../Reference/3dmeas/M3dmeasControl.md) to specify the required polarity of the second transition. In addition to [`M_POSITIVE`](../../Reference/3dmeas/M3dmeasControl.md), [`M_NEGATIVE`](../../Reference/3dmeas/M3dmeasControl.md), and [`M_ANY`](../../Reference/3dmeas/M3dmeasControl.md), you can also set [`M_MARKER_POLARITY_SECOND`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_SAME`](../../Reference/3dmeas/M3dmeasControl.md) or [`M_OPPOSITE`](../../Reference/3dmeas/M3dmeasControl.md) to find transitions with the same or opposite polarity as [`M_MARKER_POLARITY`](../../Reference/3dmeas/M3dmeasControl.md).

## Invalid position

When searching for invalid transitions, you should use [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_POSITION_INVALID`](../../Reference/3dmeas/M3dmeasControl.md) to specify where to identify a transition when an invalid region is encountered in the profile. You can specify to identify a transition before and/or after the invalid region, or in the middle of the invalid region. When the transition is identified in the middle of the invalid region, its Z-value is the mid-value between the first valid values on either side of the invalid region.

## Expected number of markers

You can set the expected number of markers to find in the profile, using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md). The default value is 1. To find all markers, set [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_ALL`](../../Reference/3dmeas/M3dmeasControl.md). Note that this control type is only available when using a profile or path 3D measurement context. When using a template 3D measurement context, the find marker operation always looks for 1 marker in each perpendicular profile of the template.

If the number of candidates is greater than the expected number of markers, the candidates are filtered out based on their location, strength, depth, width, or spacing, depending on the selection policy chosen ([`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasControl.md)). See [Selection criteria](S06_Finding_markers.md) for more information.

## Selection policy

Aurora Imaging Library can only find a maximum of [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md) markers in a profile. If the number of candidates that meet the specified characteristics is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md), the candidates are ranked according to a selection policy. Only the top candidates are returned as found markers. You can use [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasControl.md) to set the selection policy. You can choose to return only the specified number of markers that are the strongest, tallest, widest, or narrowest, or those closest to the profile's origin, the end of the profile, the specified relative position ([`M_MARKER_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md)), the specified relative spacing ([`M_MARKER_SPACING_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md)), or the specified relative width ([`M_MARKER_WIDTH_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md)). Note that when using [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) to find markers and perform the fit simultaneously, you can choose to return the marker in each profile that would result in the best fit possible, by setting [`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_BEST_FIT`](../../Reference/3dmeas/M3dmeasControl.md).

*[Image: 3dmeas_marker_selection_position_first.png]*

*[Image: 3dmeas_marker_selection_position_last.png]*

The example above illustrates the different markers found in each row (profile) of a depth map when [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md) is set to 1 and [`M_MARKER_SELECTION`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_POSITION_FIRST`](../../Reference/3dmeas/M3dmeasControl.md) (top) and [`M_POSITION_LAST`](../../Reference/3dmeas/M3dmeasControl.md) (bottom).

## Relative position, relative spacing, and relative width

When the number of candidates is greater than [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md) and the specified selection policy is [`M_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md), [`M_CLOSEST_TO_SPACING`](../../Reference/3dmeas/M3dmeasControl.md), or [`M_CLOSEST_TO_WIDTH`](../../Reference/3dmeas/M3dmeasControl.md), markers are returned based on their proximity to a specified relative position, similarity to a specified relative spacing, or similarity to a specified relative width, respectively. You can set the relative position, relative spacing, and relative width, using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_MARKER_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md), [`M_MARKER_SPACING_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md), and [`M_MARKER_WIDTH_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md), respectively. Specify the position, spacing, or width as a percentage of the profile length. For example, if the profile is 10 mm in length and you set [`M_MARKER_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md) to 60%, the relative position is 6 mm from the start of the profile. In this case, if [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md), [`M_MARKER_NUMBER`](../../Reference/3dmeas/M3dmeasControl.md) is set to 1, and there are marker candidates 2 mm from the start of the profile and 4 mm from the start of the profile, only the latter candidate is returned as a found marker since it is closer to the relative position. Note that the specified relative position is also used when calculating the [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md) [`M_OFFSET`](../../Reference/3dmeas/M3dmeasGetResult.md) result type.
