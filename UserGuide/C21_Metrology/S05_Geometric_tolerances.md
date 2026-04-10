---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Metrology
section: Geometric_tolerances
module_tag: met
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / metrology / Geometric tolerances"
---

# Geometric tolerances

After adding features to the metrology template, you will typically add geometric tolerances, using [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md). A geometric tolerance defines the acceptable deviation from the definition of a feature, or the acceptable geometric relationship between multiple features.

*[Image: Metro_GD_T01_Tolerance.png]*

When adding a geometric tolerance, you must set its minimum and maximum limit values. When defining the tolerance of one feature, these limits represent the acceptable deviation from the definition of the feature. For example, a segment's length tolerance can be between 90 and 100 pixels. For multiple features, these limits represent the valid range of acceptable values between the features. For example, the perpendicular tolerance between two segments can be +/- 0.05° (that is, 89.95° to 90.05°). To change the minimum and maximum limit values, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_VALUE_MIN`](../../Reference/met/MmetControl.md) and [`M_VALUE_MAX`](../../Reference/met/MmetControl.md). You can also use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md) and [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md) to set warning values to alert you when the tolerance is close to its minimum and maximum limits.

When you call [`MmetCalculate`](../../Reference/met/MmetCalculate.md), the tolerance is calculated (for example, 95 pixels) and a status is assigned (for example, [`M_PASS`](../../Reference/met/MmetGetResult.md)). To retrieve the tolerance value and status, use [`MmetGetResult`](../../Reference/met/MmetGetResult.md) with [`M_TOLERANCE_VALUE`](../../Reference/met/MmetGetResult.md) and [`M_STATUS`](../../Reference/met/MmetGetResult.md).

The status of the geometric tolerance returns [`M_PASS`](../../Reference/met/MmetGetResult.md) or [`M_WARNING`](../../Reference/met/MmetGetResult.md) if the tolerance meets the specified requirements, and [`M_FAIL`](../../Reference/met/MmetGetResult.md) if it does not. [`M_FAIL`](../../Reference/met/MmetGetResult.md) is returned when a tolerance value falls outside the range defined by the minimum and maximum tolerance values and/or the minimum and maximum warning values. [`M_WARNING`](../../Reference/met/MmetGetResult.md) is returned when a tolerance value is less than or equal to the minimum tolerance value, but greater than the minimum warning value. A warning also occurs when a tolerance value is greater or equal to the maximum tolerance value, and less than the maximum warning value. [`M_PASS`](../../Reference/met/MmetGetResult.md) is returned when the value is between the minimum and maximum tolerance values. By default, the minimum and maximum tolerance and warning values are 0. The following is an example of (non-zero) minimum and maximum tolerance and warning values:

*[Image: Metro_Tolerance_Pass_Warning_Fail.png]*

Note that a warning will never be returned if the warning range is within the pass range.

If a calibration context is associated with the target image, set values in real-world units. Otherwise use pixel units (for example, tolerance values and warning values, and positional values). For more information, see [Camera calibration - overview](../C28_Calibration/S01_Calibration_overview.md).

## Adding a geometric tolerance

With [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md), you can add one of the following geometric tolerances:

|   |   |   |
| --- | --- | --- |
| [Angularity](S05_Geometric_tolerances.md) *[Image: metro_icon_angularity.png]* | [Area - simple](S05_Geometric_tolerances.md) *[Image: Metro_Icon_SurfaceArea.png]* | [Area - convex hull](S05_Geometric_tolerances.md) *[Image: Metro_Icon_SurfaceAreaConvexHull.png]* |
| [Area between curves](S05_Geometric_tolerances.md) *[Image: Metro_Icon_AreaBetwixtCurves.png]* | [Area under curve max](S05_Geometric_tolerances.md) *[Image: Metro_Icon_AreaUnderCurveMax.png]* | [Area under curve min](S05_Geometric_tolerances.md) *[Image: Metro_Icon_AreaUnderCurveMin.png]* |
| [Concentricity](S05_Geometric_tolerances.md) *[Image: Metro_Icon_Concentricity.png]* | [Distance](S05_Geometric_tolerances.md) *[Image: Metro_Icon_Distance.png]* | [Length (arc or circle)](S05_Geometric_tolerances.md) *[Image: Metro_Icon_Length_Circle.png]* |
| [Length (segment or edgel)](S05_Geometric_tolerances.md) *[Image: Metro_Icon_Length_Segment.png]* | [Parallelism](S05_Geometric_tolerances.md) *[Image: Metro_Icon_Parallelism.png]* | [Perimeter - simple](S05_Geometric_tolerances.md) *[Image: Metro_Icon_SurfacePerimeter.png]* |
| [Perimeter - convex hull](S05_Geometric_tolerances.md) *[Image: Metro_Icon_SurfacePerimeterConvexHull.png]* | [Perpendicularity](S05_Geometric_tolerances.md) *[Image: Metro_Icon_Perpendicularity.png]* | [Position-X and Position-Y](S05_Geometric_tolerances.md) *[Image: Metro_Icon_PositionX.png]* *[Image: Metro_Icon_PositionY.png]* |
| [Radius](S05_Geometric_tolerances.md) *[Image: Metro_Icon_Radius.png]* | [Roundness](S05_Geometric_tolerances.md) *[Image: Metro_Icon_Roundness.png]* | [Straightness](S05_Geometric_tolerances.md) *[Image: Metro_Icon_Straightness.png]* |

This table illustrates the tolerance's icon, which you can draw by calling [`MmetDraw`](../../Reference/met/MmetDraw.md) with [`M_DRAW_TOLERANCE`](../../Reference/met/MmetDraw.md). For more information about drawing, see [Annotating the results](S07_Calculating_and_retrieving_results.md).

### Angularity

An angularity tolerance ([`M_ANGULARITY`](../../Reference/met/MmetAddTolerance.md)) validates that the angle between two features falls within the specified angular range (for example, that two lines intersect at an angle between 25° and 35°). An angularity tolerance can be applied between the following features.

- Two segment features.
  To validate the angularity between two segment features, Aurora Imaging Library internally connects each segment at their start point and considers the angle between them to be, counter-clockwise, from the first segment to the second segment. This angle is valid if it falls within the angular range set by the minimum and maximum tolerance limits. To designate the valid angular range, the minimum and maximum tolerance limits are angles applied counter-clockwise around the start point of the first segment. Note that Aurora Imaging Library does not physically rearrange the specified segments, it only orients them internally to measure the required angles. The following animation illustrates angularity.
  *[Image: Metrology_Angularity]*
  Each segment's start point, as well as the order you list the segments in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter, can affect your results. For example, the angle between segment 1 and segment 2 is different than the angle between segment 2 and segment 1.
  *[Image: Metro_Tolerance_Angularity_Segments_TwoB.png]*
- Two line features.
  To validate the angularity between two line features, Aurora Imaging Library considers the angle between them to be, counter-clockwise, from the first line to the second line. This angle is valid if it falls within the angular range set by the minimum and maximum tolerance limits. To designate the valid angular range, the minimum and maximum tolerance limits are angles applied counter-clockwise, from the first line to the second line, around the intersection point of the two lines.
  *[Image: Metro_Tolerance_Angularity_Lines_Two.png]*
  The order you list the lines in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter can affect your results. For example, the angle between line 1 and line 2 is different than the angle between line 2 and line 1.
  *[Image: Metro_Tolerance_Angularity_Lines_TwoC.png]*
  Since lines are infinite, there are actually two ways to measure the angle between line 1 and line 2 (indicated as angle (a) in the following diagram) and between line 2 and line 1 (indicated as angle (b) in the following diagram). Note that this is inconsequential since each pair of angles is equivalent.
  *[Image: Metro_Tolerance_Angularity_Lines_TwoB.png]*
- A linear feature (segment or line) and an edgel feature.
  For one linear feature (segment or line) and one edgel feature, [`M_ANGULARITY`](../../Reference/met/MmetAddTolerance.md) validates the width of the projection of the edgel feature, along the nominal angle specified using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_ANGLE`](../../Reference/met/MmetControl.md). The angle is in the counter-clockwise direction relative to the linear feature. The minimum and maximum tolerance parameters set the valid projection width. Typically, you should set the minimum width value to 0. The width of the projection is in pixel or world units. The second feature specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter must be edgel.
  *[Image: Metro_Tolerance_Angularity.png]*
- Three point features.
  To validate the angularity between three point features, Aurora Imaging Library considers the first point as the center of the angularity tolerance, the second point as the _StartAngle_ (in the counter-clockwise direction relative to the first point), and the third point as the _EndAngle_ (in the counter-clockwise direction relative to the first point). [`M_ANGULARITY`](../../Reference/met/MmetAddTolerance.md) validates the angle that results from _EndAngle_ - _StartAngle_.*[Image: Metro_Tolerance_Angularity_Three_Points.png]*

Note that an angularity tolerance between a segment and a line produces too many ambiguities and therefore cannot be calculated.

### Area and perimeter

You can add the following tolerances, based on area or perimeter.

- A tolerance for the typical area or perimeter of a feature's surface ([`M_AREA_SIMPLE`](../../Reference/met/MmetAddTolerance.md) or [`M_PERIMETER_SIMPLE`](../../Reference/met/MmetAddTolerance.md)).
- A tolerance, based on the convex hull, for the area or perimeter of a feature's surface ([`M_AREA_CONVEX_HULL`](../../Reference/met/MmetAddTolerance.md) or [`M_PERIMETER_CONVEX_HULL`](../../Reference/met/MmetAddTolerance.md)). Abstractly, you can consider the convex hull like placing a rubber band around the feature, and then calculating the area or perimeter.
  *[Image: Boltarea.png]*
- A tolerance for the area between two curves of edgels ([`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md)).
  *[Image: M_AREA_BETWEEN_CURVES.png]*
  You can control how Aurora Imaging Library establishes curves from the edgel features using [`M_CURVE_INFO`](../../Reference/met/MmetControl.md). Aurora Imaging Library can automatically establish unbounded (open) or bounded (closed) curves. Alternatively, for unbounded curves, you can specify a reference frame to indicate the direction of the curve, which can make the tolerance faster to calculate and less sensitive to gaps between edgels.
  *[Image: M_CURVE_INFO-CurveDirection.png]*
  If the two specified curves intersect, then Aurora Imaging Library will validate the areas on both sides of the curves by default. If you only want to validate areas on one side of the curves, enable [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md) using [`MmetControl`](../../Reference/met/MmetControl.md). Aurora Imaging Library must establish the side on which the areas are considered positive, and add them. Areas on the other side (which is considered the negative side) are ignored. The positive and negative sides are determined by the direction of the global reference frame's Y-axis, and whether it intersects with the first or second specified curve first. You can specify a different reference frame with [`M_AREA_BETWEEN_CURVES_DEFINE_SIDE`](../../Reference/met/MmetControl.md).
  If the Y-axis initially intersects with the first specified curve, all the areas on the side of the first curve facing the direction of the Y-axis are considered positive and Aurora Imaging Library adds them; all the areas on the other side of the first curve are considered negative and Aurora Imaging Library ignores them. If the Y-axis initially intersects with the second specified curve, all the areas on the side of the second curve facing the direction of the Y-axis are considered negative and Aurora Imaging Library ignores them; all the areas on the other side of the second curve are considered positive and Aurora Imaging Library adds them.
  *[Image: Metro_AreaBetwixtCurves_Side_Matters.png]*
  If neither curve intersects the Y-axis, or if only one curve intersects the Y-axis, the sign of the areas between the curves is computed based on the order in which the two curves are defined. If the points delimiting an area on the first specified curve appear in a clockwise order, then this area will be negative. If they appear in a counter-clockwise order, then this area will be positive.
  *[Image: metro_areabetwixtcurves_side_matters_no_intersect.png]*
- A tolerance for the maximum or minimum area under a curve of edgels or points ([`M_AREA_UNDER_CURVE_MAX`](../../Reference/met/MmetAddTolerance.md) or [`M_AREA_UNDER_CURVE_MIN`](../../Reference/met/MmetAddTolerance.md)) and a line.
  *[Image: M_AREA_UNDER_CURVE_MAXMIN.png]*
  When a curve intersects with a segment or line feature, areas(s) can exist on both sides of the feature. The positive area(s) are those located on the side of the feature where the sum of the area(s) is the largest, and the negative area(s) are those located on the opposing side. Use [`M_DRAW_TOLERANCE_AREA...`](../../Reference/met/MmetDraw.md) to draw the positive or negative area.
  *[Image: AreaAllowNegative.png]*

Note that for an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md), [`M_AREA_UNDER_CURVE_MAX`](../../Reference/met/MmetAddTolerance.md), or [`M_AREA_UNDER_CURVE_MIN`](../../Reference/met/MmetAddTolerance.md) tolerance, you can set the maximum gap between edgels to consider them part of the same curve with [`M_CURVE_EDGEL_GAP_SIZE`](../../Reference/met/MmetControl.md)for an. [`M_CURVE_EDGEL_GAP_SIZE`](../../Reference/met/MmetControl.md) sets the look ahead distance used to identify which points to consider as the next point along the curve. For a small gap size, fewer points will be considered. For a large gap size, noisy and outlier points might be considered. The actual points chosen will vary depending both on their distances from the last chosen point and the type of tolerance that is being evaluated.

### Concentricity

A concentricity tolerance ([`M_CONCENTRICITY`](../../Reference/met/MmetAddTolerance.md)) validates that two features have a common center. A concentricity tolerance can be applied to circle and arc features. The minimum and maximum tolerance parameters set the valid minimum and maximum distance allowed between the center of each feature. Typically, you should set the minimum distance value to 0.

*[Image: Metro_Tolerance_Concentricity.png]*

### Distance

A distance tolerance ([`M_DISTANCE_MAX`](../../Reference/met/MmetAddTolerance.md) and [`M_DISTANCE_MIN`](../../Reference/met/MmetAddTolerance.md)) validates that the distance between two features meets either the specified maximum or minimum distance values. You can use a maximum distance tolerance with any two features except lines (as lines are infinitely long); you can use a minimum distance tolerance with any two features. The maximum and minimum tolerance parameters set the valid maximum and minimum distances between features for either the maximum or minimum distance tolerance.

*[Image: Metro_Tolerance_Distance_Maximum.png]*

*[Image: Metro_Tolerance_Distance_Minimum.png]*

By default, to validate the tolerance, Aurora Imaging Library uses the distance at which the features are either the farthest (for [`M_DISTANCE_MAX`](../../Reference/met/MmetAddTolerance.md)) or the closest (for [`M_DISTANCE_MIN`](../../Reference/met/MmetAddTolerance.md)). You can however explicitly specify an angle at which to apply the distance tolerance between two features, by calling [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_DISTANCE_MODE`](../../Reference/met/MmetControl.md). This can be seen as a type of caliper tolerance between the features. The following example shows how Aurora Imaging Library applies the minimum distance tolerance between a circle and an edgel feature at a specific angle.

*[Image: Metro_Tolerance_Distance_Minimum_At_An_Angle.png]*

### Length

A length tolerance ([`M_LENGTH`](../../Reference/met/MmetAddTolerance.md)) validates that the linear dimension of one feature meets the specified value. A length tolerance can be applied to one of the following features: segment, edgel, circle, or arc. For an edgel feature, you should only use [`M_LENGTH`](../../Reference/met/MmetAddTolerance.md) if the edgel represents one continuous curve (or path) that does not cross over itself. For circle and arc, length refers to the circumference. The minimum and maximum tolerance parameters set the valid minimum and maximum lengths of the feature.

*[Image: Metro_Tolerance_Length.png]*

### Parallelism

A parallelism tolerance ([`M_PARALLELISM`](../../Reference/met/MmetAddTolerance.md)) validates the degree to which two features are parallel. A parallelism tolerance can be applied between the following:

- Two linear features (segments and lines).
- A linear feature (segment or line) and an edgel feature.

For any combination of 2 linear features (segments and lines), [`M_PARALLELISM`](../../Reference/met/MmetAddTolerance.md) validates that the angle of the two features, relative to the global reference frame, is the same. The maximum tolerance parameter sets the valid angular range, from the nominal angle (0°). In this case, you should only set the maximum tolerance parameter, and set the minimum tolerance parameter to 0. The order of the features specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter, as well as the start/end of the segments, is inconsequential.

*[Image: Metro_Tolerance_Parallelism_Two_Linear.png]*

For parallelism tolerances between two linear features, angles are remapped to 0°; that is, the angle that is measured, and the angular range that you must provide, is the angle that is closest to 0°.

*[Image: Metro_Tolerance_Parallelism_Angle_Measured.png]*

For a linear feature and an edgel feature, [`M_PARALLELISM`](../../Reference/met/MmetAddTolerance.md) validates the width of the projection of the edgel feature on a theoretical line perpendicular to the segment or line feature. The minimum and maximum tolerance values set the valid projection width. The smaller the width, the more the edgel feature is parallel to the given segment or line feature. Typically, the minimum width value is 0. The width of the projection is in pixel or world units. The second feature specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter must be edgel.

*[Image: Metro_Tolerance_Parallelism_Linear_Edgel.png]*

### Perpendicularity

A perpendicularity tolerance ([`M_PERPENDICULARITY`](../../Reference/met/MmetAddTolerance.md)) validates the degree to which two features are perpendicular. A perpendicularity tolerance can be applied between the following features:

- Two linear features (segments and lines).
- A linear feature (segment or line) and an edgel feature.

For any combination of 2 linear features (segments and lines), [`M_PERPENDICULARITY`](../../Reference/met/MmetAddTolerance.md) validates that the angle between the two features is 90°. The maximum tolerance parameter sets the valid angular range, from the nominal angle (90°). In this case, you should only set the maximum tolerance parameter, and set the minimum tolerance parameter to 0. The order of the features specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter, as well as the start/end of the segments, is inconsequential.

*[Image: Metro_Tolerance_Perpendicularity_Two_Linear.png]*

For perpendicularity tolerances between two linear features, angles are remapped to 90°; that is, the angle measured and the angular range you provide is the angle closest to 90°.

*[Image: Metro_Tolerance_Perpendicularity_Angle_Measured.png]*

For a linear feature and an edgel feature, [`M_PERPENDICULARITY`](../../Reference/met/MmetAddTolerance.md) validates the width of the projection of the edgel feature on a theoretical line parallel to the segment or line feature. The minimum and maximum tolerance values set the valid projection width. The smaller the width, the more the edgel feature is perpendicular to the given segment or line feature. Typically, the minimum width value is 0. The width of the projection is in pixel or world units. The second feature specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter must be edgel.

*[Image: Metro_Tolerance_Perpendicularity_Linear_Edgel.png]*

### Position

A position tolerance ([`M_POSITION_X`](../../Reference/met/MmetAddTolerance.md) and [`M_POSITION_Y`](../../Reference/met/MmetAddTolerance.md)) validates that a feature must be located at the specified position, along the X- or Y-direction of the specified reference frame. A position tolerance can be applied to a local frame feature, and one of the following features: local frame, point, circle. The minimum and maximum tolerance parameters set the valid minimum and maximum positions for either the X- or Y-position tolerance. If you only specify one feature, Aurora Imaging Library validates the geometric relationship between that feature and its reference frame (every feature is associated to a reference frame).

*[Image: Metro_Tolerance_Position_X.png]*

*[Image: Metro_Tolerance_Position_Y.png]*

### Radius

A radius tolerance ([`M_RADIUS`](../../Reference/met/MmetAddTolerance.md)) validates that the linear length between the feature's center and perimeter falls within the specified values. A radius tolerance can be applied to one of the following features: arc or circle. The minimum and maximum tolerance parameters set the valid minimum and maximum radius of the feature.

*[Image: Metro_Tolerance_Radius.png]*

### Roundness

A roundness tolerance ([`M_ROUNDNESS`](../../Reference/met/MmetAddTolerance.md)) validates that a feature is round. This is done by calculating the distance between the inner and outer circular curves formed by the given feature, such as the circle feature shown below. Note that the inner and outer circular curves are concentric.

A roundness tolerance can be applied to circle, arc, and edgel features. The minimum and maximum tolerance parameters set the valid minimum and maximum width between the two concentric curves (for non circle features, this can be seen as portions of a circle). Typically, you should set the minimum width value to 0.

*[Image: Metro_Tolerance_Roundness.png]*

### Straightness

A straightness tolerance ([`M_STRAIGHTNESS`](../../Reference/met/MmetAddTolerance.md)) validates that a feature is straight. This is done by calculating the distance between the two parallel lines that are formed by the inner and outer boundaries of the feature. The angle of the two parallel lines is chosen such that the distance between them is the smallest.

A straightness tolerance can be applied to segment and edgel features. The minimum and maximum tolerance parameters set the valid minimum and maximum width between the two parallel lines. Typically, you should set the minimum width value to 0.

*[Image: Metro_Tolerance_Straightness.png]*

## Using multiple geometric tolerances

To accurately represent a relationship between features, the features might have to be validated by multiple geometric tolerances. For example, to verify the following result, you might set a 30° angularity tolerance between two segments.

*[Image: Metro_Tolerance_Angularity_Result_Required.png]*

However, using only the angularity tolerance, each of the following results has a valid tolerance status.

*[Image: Metro_Tolerance_Angularity_Results.png]*

In this case, you need an additional tolerance to check the location of the segments. To do so, you would add two constructed point features at the start of each segment, using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_POINT`](../../Reference/met/MmetAddFeature.md) and [`M_POSITION_START`](../../Reference/met/MmetAddFeature.md). You would then add a distance tolerance for these two points, ensuring they are at the same location. To satisfy the required feature relationship, both the angularity status of the segments and the distance status of the points must be valid.

*[Image: Metro_Tolerance_Angularity_Result_Valid.png]*
