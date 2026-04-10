---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Measurement
section: Extracting_profiles
module_tag: 3dmeas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Measurement / Extracting profiles"
---

# Extracting profiles

The 3D Measurement module analyzes profiles to find markers. If you are finding markers using a profile 3D measurement context, each row of the depth map is interpreted as a single profile. If you are finding markers using a path or template 3D measurement context, the profiles must be extracted along a path or perpendicular to a template before the search. This section describes the various settings that you can use to customize how the profiles are generated.

After allocating a path or template 3D measurement context using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md), you must add a path or template to it, respectively, using [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md). Most of the settings available when defining the path or template can be adjusted later using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md). Note that you cannot add a path or template to a profile 3D measurement context. The profiles in a profile 3D measurement context have a corresponding default template that cannot be defined nor deleted.

## Number of profiles

When using a path 3D measurement context, a single profile is extracted along the path. When using a template 3D measurement context, you can customize how many profiles to extract perpendicular to the template.

For a template 3D measurement context, you can specify the mode with which to set the number of profiles, using the [`NumberMode`](../../Reference/3dmeas/M3dmeasDefine.md) parameter in [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md). To explicitly define the number of profiles, you can set this parameter to [`M_USER_DEFINED`](../../Reference/3dmeas/M3dmeasDefine.md) and specify the number of profiles to extract using the [`NumberValue`](../../Reference/3dmeas/M3dmeasDefine.md) parameter. The specified number of profiles are evenly distributed perpendicular to the template. If you do not know how many profiles are needed, the number of profiles can be based on how many profiles fit with a required spacing between them. You must set the [`NumberMode`](../../Reference/3dmeas/M3dmeasDefine.md) parameter to [`M_SPACING`](../../Reference/3dmeas/M3dmeasDefine.md) and then specify the required spacing between the profiles using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TEMPLATE_PROFILE_SPACING`](../../Reference/3dmeas/M3dmeasControl.md). Note that you must also set whether to interpret the specified spacing value as an absolute value or relative to a pixel size, using [`M_TEMPLATE_PROFILE_SPACING_MODE`](../../Reference/3dmeas/M3dmeasControl.md). The [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md) modes allow the spacing to be dependent on the depth map calibration.

Note that the mode used to determine the number of profiles and the precise number of profiles can be later adjusted using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) and [`M_TEMPLATE_PROFILE_NUMBER_VALUE`](../../Reference/3dmeas/M3dmeasControl.md), respectively.

## Profile length

When using a template 3D measurement context, you can customize the profile length. The length of a profile in a template 3D measurement context is defined by its nominal length plus its margin. Note that you cannot customize the length of the profile when using a path 3D measurement context because the length of the profile is equivalent to the length of the path.

For a template 3D measurement context, you should set the nominal length large enough to include the expected position variation of the marker.

Typically, the nominal length should be the same for all of the template's perpendicular profiles. In this case, you should set the [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md) [`LengthSource`](../../Reference/3dmeas/M3dmeasDefine.md) parameter to [`M_TEMPLATE`](../../Reference/3dmeas/M3dmeasDefine.md) and specify a global nominal profile length by passing a single value to the [`ParamArrayPtr`](../../Reference/3dmeas/M3dmeasDefine.md) parameter.

If the same nominal length does not work for all of the template's perpendicular profiles (for example, there are areas where another object could interfere, and some profiles need to be shorter than others to exclude it), you can set the [`LengthSource`](../../Reference/3dmeas/M3dmeasDefine.md) parameter to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasDefine.md) and specify the nominal length of each individual profile by passing an array of values to the [`ParamArrayPtr`](../../Reference/3dmeas/M3dmeasDefine.md) parameter. Note that you must know the exact number of profiles to specify the length of each profile ([`NumberMode`](../../Reference/3dmeas/M3dmeasDefine.md) must be set to [`M_USER_DEFINED`](../../Reference/3dmeas/M3dmeasDefine.md)). Alternatively, you can set a global nominal profile length that works for most profiles, using the method described in the preceding paragraph, or using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasControl.md), and then set custom nominal lengths for specific profiles using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TEMPLATE_PROFILE_LENGTH`](../../Reference/3dmeas/M3dmeasControl.md). In this case, for each profile that you specify a custom nominal length, you must set [`M_TEMPLATE_PROFILE_LENGTH_SOURCE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasControl.md) to use it.

In the example below, the global nominal profile length value is 10 and the nominal length value of the third profile is 20.

*[Image: 3dmeas_profile_lengths.png]*

Note that when you specify a nominal length, you must also specify the mode with which to interpret it, using the [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md) [`LengthMode`](../../Reference/3dmeas/M3dmeasDefine.md) parameter or [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TEMPLATE_PROFILE_LENGTH_MODE`](../../Reference/3dmeas/M3dmeasControl.md). For example, you can specify to interpret the specified nominal length value as an absolute value or relative to a pixel size. The [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md) modes allow the profile length to be dependent on the depth map calibration. In the example above, [`M_TEMPLATE_PROFILE_LENGTH_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_RELATIVE_TO_PIXEL_SIZE_MIN`](../../Reference/3dmeas/M3dmeasControl.md) (default), so the specified nominal profile length values are multiplied by the minimum pixel size.

If you want to return a transition even when it is outside the expected range of positions, you can add a margin to the length, using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TEMPLATE_PROFILE_LENGTH_MARGIN`](../../Reference/3dmeas/M3dmeasControl.md). Specify the margin as a percentage of the nominal length. You can set a global margin for the template's perpendicular profiles and, if necessary, you can set custom margins for specific profiles. For each profile in which you specify a custom margin, you must set [`M_TEMPLATE_PROFILE_LENGTH_MARGIN_SOURCE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasControl.md) to use it.

*[Image: 3dmeas_profile_length_margin.png]*

## Relative profile position

When using a template 3D measurement context, you can also customize the position of the profiles, relative to the template. For example, if there is another object to the left of the template that could interfere, you can move the profiles toward the right of the template to exclude it.

You can set the relative position of a profile using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TEMPLATE_PROFILE_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md). Specify the relative position as a percentage of the profile's nominal length. When the relative position is 0% (default), the center of the profile touches the template. If you specify a value of -50%, the profile is moved forwards, such that the end of its nominal length touches the template, and if you specify a value of 50%, the profile is moved backwards, such that the start of its nominal length touches the template. You can set a global relative position for the template's perpendicular profiles and, if necessary, you can set custom relative positions for specific profiles. For each profile in which you specify a custom relative position, you must set [`M_TEMPLATE_PROFILE_POSITION_SOURCE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_PROFILE`](../../Reference/3dmeas/M3dmeasControl.md) to use it.

*[Image: 3dmeas_relative_position_0.png]*

*[Image: 3dmeas_relative_position_-40.png]*

The example above illustrates the profile positions when [`M_TEMPLATE_PROFILE_POSITION_RELATIVE`](../../Reference/3dmeas/M3dmeasControl.md) is set to 0% (top) and -40% (bottom).

## Extended search

When using a template 3D measurement context, you can set whether to extend the search outside of the template's defined area, using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_TEMPLATE_EXTENDED_SEARCH`](../../Reference/3dmeas/M3dmeasControl.md). By default, extended search is enabled. You can set [`M_TEMPLATE_EXTENDED_SEARCH`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_DISABLE`](../../Reference/3dmeas/M3dmeasControl.md) to constrain the profile extraction so that it does not use data beyond the limits of the template. The profiles will be distributed such that no profile region extends past the start or end point of the template. If the number of profiles is set with an explicit value, the number of profiles remains the same, but the spacing between them decreases. If the number of profiles is dependent on the spacing, the number of profiles can decrease depending on the maximum number of profiles whose region fits within the template while respecting the spacing between them.

*[Image: 3dmeas_extended_search_enabled.png]*

*[Image: 3dmeas_extended_search_disabled.png]*

The example above illustrates the profile regions perpendicular to a template when [`M_TEMPLATE_PROFILE_NUMBER_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_SPACING`](../../Reference/3dmeas/M3dmeasControl.md) and [`M_TEMPLATE_EXTENDED_SEARCH`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_ENABLE`](../../Reference/3dmeas/M3dmeasControl.md) (top) and [`M_DISABLE`](../../Reference/3dmeas/M3dmeasControl.md) (bottom).

## Thickness

The thickness of the profile line determines the size of the profile region. Use [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md) to set the thickness value. You can use [`M_PROFILE_THICKNESS_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to specify whether to interpret the specified thickness as an absolute value or whether to multiply it by a pixel size or by the spacing between profiles. For example, if you want don't want gaps between the profile regions, you can set [`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md) to 1 and set [`M_PROFILE_THICKNESS_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_RELATIVE_TO_SPACING`](../../Reference/3dmeas/M3dmeasControl.md). The [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md) modes allow the thickness to be dependent on the depth map calibration. Pixels along the profile line, within the specified thickness, are sampled and corresponding points are considered for the profile. See [Sampling distance, minimum valid percentage, and accumulation type](S05_Extracting_profiles.md) for more information.

*[Image: 3dmeas_profile_thickness_1_relative_to_spacing.png]*

*[Image: 3dmeas_profile_thickness_25_relative_to_pixel_size_min.png]*

The example above illustrates the resulting profile regions when [`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md) is set to 1 and [`M_PROFILE_THICKNESS_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_RELATIVE_TO_SPACING`](../../Reference/3dmeas/M3dmeasControl.md) (top), and when [`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md) is set to 25 and [`M_PROFILE_THICKNESS_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_RELATIVE_TO_PIXEL_SIZE_MIN`](../../Reference/3dmeas/M3dmeasControl.md) (bottom).

## Sampling distance, interpolation mode, minimum valid percentage, and accumulation type

When extracting the profile along the path or the profiles at regular intervals perpendicular to the template, you can use [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasControl.md) and [`M_PROFILE_SAMPLE_SIZE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to set the sampling distance used to extract consecutive profile points along the profile line. The sampling distance determines where each profile point will be extracted from the depth map. The points within the thickness of the profile line at each of these positions are considered a sample, and a single profile point is calculated from the points in each sample. You can use [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to specify the interpolation mode used to calculate the value for each point in the sample. Use [`M_PROFILE_MIN_VALID_PERCENTAGE`](../../Reference/3dmeas/M3dmeasControl.md) to set the minimum percentage of points in a sample that must be valid to set the corresponding profile point to a valid value, and use [`M_PROFILE_ACCUMULATE_TYPE`](../../Reference/3dmeas/M3dmeasControl.md) to specify whether to take the minimum, maximum, or mean value of the points in the sample to create the profile point when the minimum valid percentage is met. These concepts are shown in the image below.

*[Image: 3dmeas_profile_samples.png]*

The image above illustrates the extracted profile when [`M_PROFILE_SAMPLE_SIZE`](../../Reference/3dmeas/M3dmeasControl.md) is set to 2, [`M_PROFILE_SAMPLE_SIZE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_RELATIVE_TO_PIXEL_SIZE_X`](../../Reference/3dmeas/M3dmeasControl.md), [`M_PROFILE_THICKNESS`](../../Reference/3dmeas/M3dmeasControl.md) is set to 5, [`M_PROFILE_THICKNESS_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_RELATIVE_TO_PIXEL_SIZE_Y`](../../Reference/3dmeas/M3dmeasControl.md), [`M_PROFILE_INTERPOLATION_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_NEAREST_NEIGHBOR`](../../Reference/3dmeas/M3dmeasControl.md), [`M_PROFILE_MIN_VALID_PERCENTAGE`](../../Reference/3dmeas/M3dmeasControl.md) is set to 50%, and [`M_PROFILE_ACCUMULATE_TYPE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_MEAN`](../../Reference/3dmeas/M3dmeasControl.md).

## Projection angle

You can use [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasControl.md) to specify the angle, relative to the profile line, from which to take samples. By default, samples are taken perpendicular to the profile line. The projection angle determines the angle of the markers that are found.

*[Image: 3dmeas_profile_projection_angle.png]*

The image above illustrates samples taken when [`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasControl.md) is set to 45°.

If you want to determine the best projection angle, you can allow Aurora Imaging Library to rotate the angle of projection by enabling a multiple-angle projection, using [`M_PROFILE_PROJECTION_ANGLE_MODE`](../../Reference/3dmeas/M3dmeasControl.md). In this case, you must set the range for the projection angle ([`M_PROFILE_PROJECTION_ANGLE_DELTA_NEG`](../../Reference/3dmeas/M3dmeasControl.md) and [`M_PROFILE_PROJECTION_ANGLE_DELTA_POS`](../../Reference/3dmeas/M3dmeasControl.md)), the step angle ([`M_PROFILE_PROJECTION_ANGLE_TOLERANCE`](../../Reference/3dmeas/M3dmeasControl.md)), and the required degree of accuracy for the best angle ([`M_PROFILE_PROJECTION_ANGLE_ACCURACY`](../../Reference/3dmeas/M3dmeasControl.md)). Note that the minimum and maximum projection angles considered are 10 degrees and 170 degrees, respectively.

To perform a multiple-angle projection for a profile, Aurora Imaging Library first finds the approximate angle by projecting within the angular range at every _n_ (rotation tolerance) degrees. Once Aurora Imaging Library establishes the approximate angle with the best markers, Aurora Imaging Library performs a number of fine-tuned projections, according to the degree of accuracy. To be effective, the degree of accuracy must be less than the rotation tolerance.

Aurora Imaging Library determines the projection angle based on the number of markers found and the average strength of those markers.

You can use [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md) with [`M_PROFILE_PROJECTION_ANGLE`](../../Reference/3dmeas/M3dmeasGetResult.md) to retrieve the projection angle used to create the profile.
