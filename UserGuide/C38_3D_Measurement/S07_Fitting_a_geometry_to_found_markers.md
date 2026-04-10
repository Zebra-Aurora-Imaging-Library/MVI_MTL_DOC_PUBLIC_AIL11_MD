---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Measurement
section: Fitting_a_geometry_to_found_markers
module_tag: 3dmeas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Measurement / Fitting a geometry to found markers"
---

# Fitting a geometry to found markers

The 3D Measurement module can perform a fit operation. The fit operation tries to fit a 3D geometry to markers found in a template's perpendicular profiles. Note that the type of 3D geometry used in the fit operation is equivalent to the type of 3D geometry used to define the template in the template 3D measurement context. Currently, only 3D geometries of type [`M_LINE`](../../Reference/3dgeo/M3dgeoInquire.md) are supported.

Once you have found the positions of your markers using [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md), you can pass the template 3D measurement result buffer containing the markers to [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) to fit a 3D line geometry to them. [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) can also perform the find marker operation and the fit operation simultaneously, in which case, all possible markers are considered and the ones that provide the best fit are kept.

The fit operation uses a random sampling consensus (RANSAC) algorithm. This approach tries to fit a 3D line geometry to several random samplings of the markers, and returns the best-fitted geometry (the one with the lowest root-mean-squared (RMS) error).

With successive calls to [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md), you can customize the settings of the fit 3D measurement context.

## Fit distance

Once an initial fit estimate is calculated for a random sampling of markers, the remaining markers located within a certain distance from the fitted geometry are considered inliers and those located further away are considered outliers. The RMS error between the geometry and inlier markers is calculated, and the geometry is then repositioned to minimize the RMS error. Note that outlier markers are excluded from the robust best fit. You can set the fit distance value using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_FIT_DISTANCE`](../../Reference/3dmeas/M3dmeasControl.md). Use [`M_FIT_DISTANCE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to specify whether to interpret the specified fit distance value as an absolute value or relative to a pixel size or the sampling distance. The [`M_RELATIVE_TO_PIXEL_SIZE_...`](../../Reference/3dmeas/M3dmeasControl.md) modes allow the fit distance to be dependent on the depth map calibration. Note that you can set [`M_FIT_DISTANCE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) to [`M_AUTO`](../../Reference/3dmeas/M3dmeasControl.md) to automatically calculate the fit distance, which estimates it from all the extracted markers.

*[Image: 3dmeas_fit_defined_fit_distance.png]*

*[Image: 3dmeas_fit_auto_fit_distance.png]*

The example above illustrates the fitted geometry when [`M_FIT_DISTANCE`](../../Reference/3dmeas/M3dmeasControl.md) is set to 3 and [`M_FIT_DISTANCE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_RELATIVE_TO_SAMPLING`](../../Reference/3dmeas/M3dmeasControl.md) (top), and when [`M_FIT_DISTANCE_MODE`](../../Reference/3dmeas/M3dmeasControl.md) is set to [`M_AUTO`](../../Reference/3dmeas/M3dmeasControl.md) (bottom). In this case, using the automatic fit distance results in more inlier markers but a higher RMS error.

Note that when the find and fit are performed together (by calling [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) with a template 3D measurement context and a depth map), and there are multiple valid transitions within a single profile, the automatic fit distance can be overestimated. In this case, you should use a different mode.

## Expected percentage of outlier markers

To optimize the likelihood of a good initial fit estimate, you can specify the expected percentage of outlier markers, using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_EXPECTED_OUTLIER_PERCENTAGE`](../../Reference/3dmeas/M3dmeasControl.md). The default value is 20%. If you expect there to be a lot of outlier markers (for example, because the actual edge or ridge does not span the entire template), you should increase the expected percentage of outlier markers.

## Minimum fit coverage

The minimum fit coverage represents the percentage of profiles that must have a marker covered by the fitted geometry. Set the minimum fit coverage using [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_FIT_COVERAGE_MIN`](../../Reference/3dmeas/M3dmeasControl.md). The default value is 0%. If the fit operation cannot fit the geometry to the specified amount of markers, [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md) with [`M_STATUS_FIT`](../../Reference/3dmeas/M3dmeasGetResult.md) will return [`M_INSUFFICIENT_COVERAGE`](../../Reference/3dmeas/M3dmeasGetResult.md).

## Outlier removal

When the find marker and fit operations are performed separately (using [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) followed by [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md)), you can choose whether to remove the outlier markers from the template 3D measurement result buffer after the fit. This is useful if you only want to retrieve results for inlier markers. To do so, use [`M3dmeasControl`](../../Reference/3dmeas/M3dmeasControl.md) with [`M_REMOVE_OUTLIERS`](../../Reference/3dmeas/M3dmeasControl.md). Outlier removal is disabled by default. Note that when the find marker and fit operations are performed simultaneously (using [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md)), the outlier markers are always removed.

## Comparing the fitted geometry to the template

[`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) also calculates some results that allow you to compare the resulting fitted geometry to the defined template. These can be useful if the template follows an expected edge or ridge of your object, and you want to verify the location or shape of your object. Using [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md), you can retrieve the angle between the fitted geometry and the template ([`M_ANGULARITY`](../../Reference/3dmeas/M3dmeasGetResult.md)), the distance between their centers ([`M_CENTER_DISTANCE`](../../Reference/3dmeas/M3dmeasGetResult.md)), and the maximum distance between them ([`M_DISTANCE_MAX`](../../Reference/3dmeas/M3dmeasGetResult.md)). Note that these comparison results are also available when the markers were previously found (because the template 3D measurement result buffer also holds the original template).
