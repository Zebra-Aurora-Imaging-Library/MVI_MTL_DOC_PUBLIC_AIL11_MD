---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Metrology
section: Eliminating_unwanted_edgels
module_tag: met
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / metrology / Eliminating unwanted edgels"
---

# Eliminating unwanted edgels

The Metrology module has been configured, by default, to conduct a fast and robust calculation of physically measured features in the target image. This is done, in part, by typically using all edgels in the feature's metrology region. However, for unusually complex target images, or to fit your application's needs, you might want to restrict which edgels are used. Eliminating unwanted edgels can result in a more precise feature extraction, a more accurate fit, and a more robust calculation. Edgels within the metrology region that don't pass certain constraints are eliminated; the remainder can be considered active edgels. The following animation shows how edgels are selected within a region of interest.

*[Image: EdgelExtraction]*

## Customizing the edge extraction algorithm

Before determining which edgels apply to a fit operation, you can modify the processing settings of the metrology context that affect how Aurora Imaging Library extracts edges. For example, you can specify a smoothing operation to reduce image noise. To adjust the processing settings, use [`MmetControl`](../../Reference/met/MmetControl.md), and alter the:

- Edge extraction filter ([`M_CHAIN_ALL_NEIGHBORS`](../../Reference/met/MmetControl.md), [`M_EXTRACTION_SCALE`](../../Reference/met/MmetControl.md), [`M_FILTER_SMOOTHNESS`](../../Reference/met/MmetControl.md), and [`M_FILTER_TYPE`](../../Reference/met/MmetControl.md)).
- Calculation precision ([`M_FLOAT_MODE`](../../Reference/met/MmetControl.md)).
- Magnitude type ([`M_MAGNITUDE_TYPE`](../../Reference/met/MmetControl.md)).
- Edge extraction threshold ([`M_THRESHOLD_MODE`](../../Reference/met/MmetControl.md), [`M_THRESHOLD_TYPE`](../../Reference/met/MmetControl.md), [`M_THRESHOLD_VALUE_HIGH`](../../Reference/met/MmetControl.md), and [`M_THRESHOLD_VALUE_LOW`](../../Reference/met/MmetControl.md)).

You can adjust these settings for each physically measured feature, or for the entire metrology context, which will affect all physically measured features. For more information on customizing the edge extraction algorithm, see [Customizing the edge extraction settings](../C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

## Edgel and fit constraints

To control which edgels apply to a fit operation, you can adjust several criteria. You can specify which edgels are considered active, using gradient angle and edgel selection rank settings. You can further specify fit constraints that determine edgel selection for fitting a feature, and therefore establish which of the active edgels are considered fitted edgels.

### Active edgels and fitted edgels

Active edgels are those edgels, in the metrology region, that satisfy the edgel constraints. Typically, they are used to build the feature. The edgel constraints include:

- The [gradient angle restrictions](S06_Eliminating_unwanted_edgels.md) ([`MmetControl`](../../Reference/met/MmetControl.md) with [`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md) and [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md)).
- The [edgel selection rank restrictions](S06_Eliminating_unwanted_edgels.md) ([`MmetControl`](../../Reference/met/MmetControl.md) with [`M_EDGEL_SELECTION_RANK`](../../Reference/met/MmetControl.md)).

Fitted edgels are those active edgels, in the metrology region, used by the fit operation to define a fitted feature. They satisfy both the edgel constraints mentioned above, and by default, the following:

- The fit operation ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_FIT`](../../Reference/met/MmetAddFeature.md), [`M_INNER_FIT`](../../Reference/met/MmetAddFeature.md), or [`M_OUTER_FIT`](../../Reference/met/MmetAddFeature.md)). For information on fit operations, see [Best fit, inner fit, and outer fit operations](S04_Features.md).
- The [common fit constraints](S06_Eliminating_unwanted_edgels.md) ([`MmetControl`](../../Reference/met/MmetControl.md) with [`M_FIT_COVERAGE_MIN`](../../Reference/met/MmetControl.md) and [`M_FIT_ITERATIONS_MAX`](../../Reference/met/MmetControl.md)).
- The [standard fit constraints](S06_Eliminating_unwanted_edgels.md) ([`MmetControl`](../../Reference/met/MmetControl.md) with [`M_FIT_VARIATION_MAX`](../../Reference/met/MmetControl.md) and [`M_FIT_DISTANCE_MAX`](../../Reference/met/MmetControl.md)) or [robust fit constraints](S06_Eliminating_unwanted_edgels.md) ([`MmetControl`](../../Reference/met/MmetControl.md) with [`M_FIT_DISTANCE_OUTLIERS`](../../Reference/met/MmetControl.md), [`M_FIT_DISTANCE_OUTLIERS_VALUE`](../../Reference/met/MmetControl.md), and [`M_FIT_INTERNAL_NUMBER_OF_POINTS`](../../Reference/met/MmetControl.md)).

For fit operations, it is possible to have active edgels that are not used to construct a feature. This occurs when there are edgels that satisfy the edgel constraints (active edgels), but do not satisfy the fit constraints (fitted edgels). Therefore, the fitted edgels are the subset of active edgels used to construct the feature. For example, an inner fit and outer fit can produce two different features, and ultimately have two different sets of fitted edgels despite one common set of active edgels.

Note that some active edgels are considered outliers because they are too far away from the expected feature fit. Outliers should not participate in the final fit.

The standard fit constraints (applied when [`M_FIT_TYPE`](../../Reference/met/MmetControl.md) is set to [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md)) can alter the precision of the edge that is ultimately fit; however, they might not reliably discard outliers (outlier edgels/points) when they significantly impact the initial fit (the first iteration of the fit). When dealing with a lot of outliers, use a robust best fit ([`M_FIT_TYPE`](../../Reference/met/MmetControl.md) set to [`M_ROBUST_FIT`](../../Reference/met/MmetControl.md)).

Note that fit constraints apply when building either physically measured features or constructed features with a fit operation.

### Gradient angle

The gradient angle (edgel direction) is the counter-clockwise angle between the target image's horizontal axis and the edge's gradient (the direction of the edgel's most sudden change from black to white) at each edgel location. The following example illustrates various edgels and their direction.

*[Image: Metro_Gradient_Angle.png]*

You can filter out unwanted edgels by restricting the degrees of freedom applied to the edgel's gradient angle. To do so, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md). Only those edgels that have a gradient angle that falls within the angular range can be used to build the feature. You can set the angular range to any value between 0.0° and 360.0° (the default is 180.0°).

By default, the angular range is set relative to the metrology region's orientation. To change the relative angle, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md). If the relative angle is set to [`M_SAME`](../../Reference/met/MmetControl.md) (the default), only those edgels that have the same gradient angle (from black to white) as the metrology region's orientation, and whose gradient angle falls within the angular range, will be used. If the relative angle is set to [`M_REVERSE`](../../Reference/met/MmetControl.md), only those edgels that have the opposite gradient angle (from white to black) of the metrology region's orientation, and whose gradient angle falls within the angular range, will be used (+180°). You can also set the relative angle to same or reverse ([`M_SAME_OR_REVERSE`](../../Reference/met/MmetControl.md)). Note that for circular metrology regions (arc, ring, ring-sector), the metrology region's orientation is radial. For more information on metrology regions, see [Metrology regions](S04_Features.md).

The following example shows how the gradient angle range (approximately 60°) and the relative angle (same and reverse) settings can be used with a rectangular metrology region and a ring-sector metrology region to select active edgels.

*[Image: Metro_Gradient_Angle_Rectangle_And_Ring.png]*

The following example shows how the relative angle settings can be used with an arc-shaped metrology region to select different active edgel features in an object. Note that to get these results, a slight angular range must also be applied.

*[Image: Metro_Gradient_Angle_Example.png]*

Note that gradient angle restrictions apply when building either physically measured features, or constructed features that use physically measured edgels as a source.

### Edgel selection rank

The edgel selection rank refers to the rank of the edgels to select when scanning the metrology region column by column from bottom to top. For example, if there are several possible edgels in a given column of the metrology region, the rank will determine whether the first edgel found, as opposed to the second or any subsequent edgel, is selected. To specify the rank, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_EDGEL_SELECTION_RANK`](../../Reference/met/MmetControl.md). If the edgel selection rank is set to [`M_LAST`](../../Reference/met/MmetControl.md), the last edgel found in the column will be used. Note that this control type takes effect after [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md) and[`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md) are satisfied.

In the following example, there are multiple edges in the metrology region. The solid gray line illustrates the edgels selected when[`M_EDGEL_SELECTION_RANK`](../../Reference/met/MmetControl.md) is set to 1; the dotted gray line illustrates the edgels selected when [`M_EDGEL_SELECTION_RANK`](../../Reference/met/MmetControl.md) is set to 2.

*[Image: EdgeSelectionRank.png]*

### Common fit constraints

Metrology calculates the fit of a feature based on an iterative process, where each iteration results in a more accurate fit than its predecessor. This process results in a robust fit where unwanted edgels, such as noise, can be eliminated. To control the fit calculation, you can set specific constraints, depending on the type of fit. For both[`M_STANDARD_FIT`](../../Reference/met/MmetControl.md) and [`M_ROBUST_FIT`](../../Reference/met/MmetControl.md), you can specify [`M_FIT_COVERAGE_MIN`](../../Reference/met/MmetControl.md) and [`M_FIT_ITERATIONS_MAX`](../../Reference/met/MmetControl.md).

The iteration process takes into account the minimum fit coverage ([`M_FIT_COVERAGE_MIN`](../../Reference/met/MmetControl.md)), which represents an estimation of how much the fitted feature must be covered by fitted edgels. The actual coverage that results from this setting depends on the scale of the image at which the feature edges were extracted, as specified with [`M_EXTRACTION_SCALE`](../../Reference/met/MmetControl.md); a scale of 1 provides the best estimation. To set [`M_FIT_COVERAGE_MIN`](../../Reference/met/MmetControl.md), you must specify a percentage based on the feature as a whole. For example, by specifying 80, you are indicating that approximately 80% of the entire fitted feature must be covered by fitted edgels, which also implies that the remaining parts of the feature can be gaps. As expected, the greater the minimum fit coverage, the more fitted edgels are needed to cover the feature. The default is 0%.

For constructed features using a fit operation performed on multiple physically measured edgel features, the accuracy of the minimum fit coverage is based on the average of each feature's extraction scale. For physically measured or constructed arcs, circles, and segments, you can retrieve the actual coverage of the feature using [`MmetGetResult`](../../Reference/met/MmetGetResult.md) with [`M_COVERAGE`](../../Reference/met/MmetGetResult.md).

The maximum fit iterations setting ([`M_FIT_ITERATIONS_MAX`](../../Reference/met/MmetControl.md)) allows you to set the maximum number of iterations used to compute a fitted feature. When this value is reached, the iteration process ends. A setting of one will consider all edgels/points in the fit. Settings higher than one will progressively eliminate outlying edgels (outlier edgels/points) in the fit. The more iterations, the better the fit, but the longer the calculation. By default, the number of iterations is determined automatically.

*[Image: Metro_Fit_Iterations_Max.png]*

### Standard fit constraints

When the number of outliers (outlier edgels/points) is not too significant, you should typically use standard fit mode ([`M_FIT_TYPE`](../../Reference/met/MmetControl.md) set to [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md)). To control the fit calculation in this mode, you can specify [`M_FIT_VARIATION_MAX`](../../Reference/met/MmetControl.md) and [`M_FIT_DISTANCE_MAX`](../../Reference/met/MmetControl.md), besides the [common fit constraints](S06_Eliminating_unwanted_edgels.md) discussed above.

If the maximum fit variation setting ([`M_FIT_VARIATION_MAX`](../../Reference/met/MmetControl.md)) is reached, the iteration process can end. The maximum fit variation setting represents the maximum allowable difference in the value of the feature's underlying coefficients, from one fit iteration to the next. For example, a segment feature has 4 coefficients: the X-coordinate of its start point, the Y-coordinate of its start point, the X-coordinate of its end point, and the Y-coordinate of its end point. During each iteration, the segment's fit becomes more accurate and therefore, the values of its coefficients change and stabilize. If, for instance, the maximum fit variation setting is set to 0.05, then as soon as the difference of each coefficients' value between iterations is less than 0.05, the iteration process ends.

During the iteration process with [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md), the maximum fit distance setting ([`M_FIT_DISTANCE_MAX`](../../Reference/met/MmetControl.md)) is applied. This setting limits the distance an active edgel can be from the current iterative fit for it to be considered in the next iterative fit. For example, if the first iterative fit of the iterative process includes all active edgels, one of which is farther away from the fit than the distance specified by [`M_FIT_DISTANCE_MAX`](../../Reference/met/MmetControl.md), this edgel will not be taken into account during the next fit iteration. The higher the value of [`M_FIT_DISTANCE_MAX`](../../Reference/met/MmetControl.md), the farther away an active edgel can be for it to be considered in the next iteration of the fit calculation. You must set this value in pixel units. The default is no maximum distance. If all the remaining edgels fall within the maximum allowable distance, they are considered as fitted edgels, provided they pass all other edgel constraints that have been set (for example, the gradient angle restrictions).

### Robust fit constraints

When the number of outliers (outlier edgels/points) is significant and affecting the accuracy of the standard fit, you should typically use robust fit mode ([`M_FIT_TYPE`](../../Reference/met/MmetControl.md) set to [`M_ROBUST_FIT`](../../Reference/met/MmetControl.md)). To control the fit calculation in this mode, you can specify [`M_FIT_DISTANCE_OUTLIERS`](../../Reference/met/MmetControl.md), [`M_FIT_DISTANCE_OUTLIERS_VALUE`](../../Reference/met/MmetControl.md), and [`M_FIT_INTERNAL_NUMBER_OF_POINTS`](../../Reference/met/MmetControl.md), besides the [common fit constraints](S06_Eliminating_unwanted_edgels.md) discussed above.

When working in [`M_ROBUST_FIT`](../../Reference/met/MmetControl.md) mode, Aurora Imaging Library uses the random sample consensus (RANSAC) algorithm during the fit operation. This approach repeats a two-step iteration to produce multiple candidates for the fit, from which the best is chosen. The algorithm proceeds as follows:

1. A random subset of active edgels are selected for an initial fit.
   Set the number of elements in this subset with [`M_FIT_INTERNAL_NUMBER_OF_POINTS`](../../Reference/met/MmetControl.md). The minimum number is 2 when fitting a segment and 3 when fitting a circle or an arc. These points are considered inliers (for this iteration) from the start.
2. The remaining active edgels located within a certain distance from the fitted feature are also considered inliers and those located further away are considered outliers.
   To set how Aurora Imaging Library determines the distance, use [`M_FIT_DISTANCE_OUTLIERS`](../../Reference/met/MmetControl.md). If set to [`M_AUTO`](../../Reference/met/MmetControl.md), the distance is automatically determined for you. If set to [`M_USER_DEFINED`](../../Reference/met/MmetControl.md), use [`M_FIT_DISTANCE_OUTLIERS_VALUE`](../../Reference/met/MmetControl.md) to specify the distance in pixel units.
3. Aurora Imaging Library performs a second fit using all the inliers to improve the result.
4. The RANSAC algorithm repeats, each time choosing a new subset for the initial fit and again performing a second fit based on inliers. Repeated random selection of initial points serves to eliminate outlier points because fits made with those points have larger errors than other fits that successfully use true inlier edgels.
   > **Note:** Note that a point can be an inlier for one iteration and an outlier for another.
5. The robust fit process ends when the number of repeated two-step fits reaches the value set with [`M_FIT_ITERATIONS_MAX`](../../Reference/met/MmetControl.md). The operation returns the best-fitted feature. The best-fit feature is the one with the smallest RMS error, which could be any of the fits obtained over all iterations.

Note that, if the edge on which a fit is performed is somewhat indistinct or fuzzy, the default number of initial points and the default outlier distance value might not be high enough to provide a stable fit. For example, in applications where a static scene is grabbed multiple times, the result of the fit on an indistinct edge could vary slightly between each frame. Slightly increasing the [`M_FIT_INTERNAL_NUMBER_OF_POINTS`](../../Reference/met/MmetControl.md) and/or [`M_FIT_DISTANCE_OUTLIERS_VALUE`](../../Reference/met/MmetControl.md) and/or [`M_FIT_ITERATIONS_MAX`](../../Reference/met/MmetControl.md) settings could correct this instability.

## Copy feature edgels

You can add a constructed edgel feature with the copy feature edgels operation ([`M_COPY_FEATURE_EDGELS`](../../Reference/met/MmetAddFeature.md)) using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md). When the base features are physically measured, you can build the new edgel feature using the active edgels (default), the fitted edgels, or all the edgels in the base feature's metrology region, regardless of any edgel constraints. For example, if you select the active edgels and use [`M_COPY_FEATURE_EDGELS`](../../Reference/met/MmetAddFeature.md) with a base feature that was built using the best-fit operation for a segment, edgels that were not considered active when calculating the segment's best fit are not included in the new feature. Note that if the base features are constructed features, Aurora Imaging Library copies all of the base feature's edgels.

To select the active edgels, fitted edgels, or all edgels, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_EDGEL_TYPE`](../../Reference/met/MmetControl.md) set to [`M_ACTIVE_EDGELS`](../../Reference/met/MmetControl.md), [`M_FITTED_EDGELS`](../../Reference/met/MmetControl.md), or [`M_ALL_EDGELS`](../../Reference/met/MmetControl.md), respectively. Note that these settings only affect the copy feature edgels build operation ([`M_COPY_FEATURE_EDGELS`](../../Reference/met/MmetAddFeature.md)). For other build operations, each edgel constraint control must be adjusted according to your active edgel requirements.

*[Image: Metro_constructed_edgel_example.png]*

## Denoising edgels

To adjust edgels from rough edges and noise, you can specify a denoising mode with [`M_EDGEL_DENOISING_MODE`](../../Reference/met/MmetControl.md). This will apply to all physically measured features and constructed edgel features. You must define a radius of neighborhood points in which the denoising is applied, with [`M_EDGEL_DENOISING_RADIUS`](../../Reference/met/MmetControl.md). Larger radii provide stronger denoising, but can result in lost precision.

The new coordinates of noisy edgels are calculated based on the specified denoising mode. There are three types of modes to select from. [`M_GAUSSIAN`](../../Reference/met/MmetControl.md) specifies to take the average point, with a weighting of all points based on the Gaussian function. The standard deviation of the Gaussian function is taken as half the neighborhood radius. [`M_MEAN`](../../Reference/met/MmetControl.md) specifies to take the mean of all provided points in the neighborhood. Finally,[`M_MEDIAN`](../../Reference/met/MmetControl.md) takes the median X and median Y of the neighborhood points. The found point is not necessarily an extracted point in the provided radius.

You can draw edgels before the denoising operation was applied using [`M_DRAW_NOISY_EDGELS`](../../Reference/met/MmetDraw.md).
