---
doctype: Reference
module: met
function: MmetAddTolerance
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / met / MmetAddTolerance"
---

# MmetAddTolerance

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

> Add a geometric tolerance to or modify a geometric tolerance of the metrology template of the metrology context.

## Syntax

```c
void MmetAddTolerance(
    AIL_ID          ContextId,                //out
    AIL_INT64       ToleranceType,            //in
    AIL_INT         ToleranceLabel,           //in
    AIL_DOUBLE      ValueMin,                 //in
    AIL_DOUBLE      ValueMax,                 //in
    const AIL_INT * FeatureLabelArrayPtr,     //in
    const AIL_INT * SubFeatureIndexArrayPtr,  //in
    AIL_INT         SizeOfArray,              //in
    AIL_INT64       ControlFlag               //in
)
```

## Description

This function adds a geometric tolerance to the metrology template of the specified metrology context. This function can also modify an existing tolerance of a metrology template. To delete geometric tolerances, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_DELETE`](../../Reference/met/MmetControl.md).

To add several geometric tolerances, you must call this function for each tolerance to add.

A geometric tolerance defines the acceptable deviation from the definition of a feature, or the acceptable relationship between multiple features. For example, you can add a geometric tolerance to the metrology template to specify that one feature must be a certain length, or that two features must be perpendicular. To add a feature, use [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md).

When adding a geometric tolerance that requires a point, you can use a specific point subfeature of a measured multiple point feature by specifying its index with the [`SubFeatureIndexArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter. Note that you cannot do this for other features, including the measured multiple edgel feature, as it is always used as a group.

When adding a geometric tolerance, you must also set its minimum and maximum limit values with the [`ValueMin`](../../Reference/met/MmetAddTolerance.md) and [`ValueMax`](../../Reference/met/MmetAddTolerance.md) parameters. When defining the tolerance of one feature, these limits represent the acceptable deviation from the definition of the feature. For example, a segment's length tolerance can be between 90 and 100 pixels. For multiple features, these limits represent the valid range of acceptable values between the features. For example, the perpendicular tolerance between two segments can be +/- 0.05° (that is, 89.95° to 90.05°). You can also set warning values to alert you when the tolerance is close to its minimum and maximum limits, using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md) and [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md).

When you call [`MmetCalculate`](../../Reference/met/MmetCalculate.md), the tolerance is calculated (for example, 95 pixels) and a status is assigned (for example, [`M_PASS`](../../Reference/met/MmetGetResult.md)). To retrieve the tolerance value or the status, use [`MmetGetResult`](../../Reference/met/MmetGetResult.md) with [`M_TOLERANCE_VALUE`](../../Reference/met/MmetGetResult.md) or [`M_STATUS`](../../Reference/met/MmetGetResult.md), respectively.

You can modify a geometric tolerance (using [`M_MODIFY`](../../Reference/met/MmetAddTolerance.md)) so that it uses a new set of features or subfeatures. In this case, you can also modify the minimum and maximum limit values of a tolerance.

If a camera calibration context is associated with the target image, set values in real-world units (for example, tolerance values and warning values, and positional values). Otherwise use pixel units. For more information, see [Camera calibration - overview](../../UserGuide/C28_Calibration/S01_Calibration_overview.md).

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the identifier of the metrology context. The metrology context must have been previously allocated on the required system using [`MmetAlloc`](../../Reference/met/MmetAlloc.md).

### `ToleranceType` *(in, AIL_INT64)*

Specifies the type of geometric tolerance to add. The icon (image symbol) of every tolerance type is in its description. You can draw this icon using [`MmetDraw`](../../Reference/met/MmetDraw.md) with [`M_DRAW_TOLERANCE`](../../Reference/met/MmetDraw.md).

*For adding geometric tolerances*

| Value | Description |
| --- | --- |
| `M_ANGULARITY` | Adds an angularity tolerance. The icon for this tolerance is:

*[Image: Metro_Icon_Angularity.png]*

An angularity tolerance validates that the angle between two features falls within the specified angular range (for example, that two lines intersect at an angle between 25° and 35°). An angularity tolerance can be applied between the following features.

- Two segment features.
  To validate the angularity between two segment features, Aurora Imaging Library internally connects each segment at their start point and considers the angle between them to be, counter-clockwise, from the first segment to the second segment. This angle is valid if it falls within the angular range set by the minimum and maximum tolerance limits. To designate the valid angular range, the minimum and maximum tolerance limits are angles applied counter-clockwise around the start point of the first segment. Note that Aurora Imaging Library does not physically rearrange the specified segments, it only orients them internally to measure the required angles.
  *[Image: Metro_Tolerance_Angularity_Segments_Two.png]*
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

Note that an angularity tolerance between a segment and a line produces too many ambiguities and therefore cannot be calculated. |
| `M_AREA_BETWEEN_CURVES` | Adds an area tolerance for the area between two curves of edgels. The icon for this tolerance is:

*[Image: Metro_Icon_AreaBetwixtCurves.png]*

To use [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md), you must specify two edgel features ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md)). Aurora Imaging Library internally joins the edgels of each edgel feature to form two curves, between which the area is validated.

*[Image: Metro_AreaBetwixtCurves_Typical.png]*

When specifying an edgel feature with this tolerance, Aurora Imaging Library internally sorts and filters the edgels, and uses a maximum gap between them, to determine whether they are part of the same curve. To specify an explicit gap size, call [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_CURVE_EDGEL_GAP_SIZE`](../../Reference/met/MmetControl.md).

By default, [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) assumes each edgel (curve) feature is unbounded (open), and does not expect the edgels to follow a specific direction. To modify this behavior and potentially speed up calculations and accuracy, call [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_CURVE_INFO`](../../Reference/met/MmetControl.md).

If the two curves cross over each other, there will be areas on both sides of the curves.

*[Image: Metro_AreaBetwixtCurves_Default.png]*

By default, Aurora Imaging Library adds all the areas. To modify this behavior (for example, to calculate the area on only one side of a curve), call [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md) or [`M_AREA_BETWEEN_CURVES_OPPOSITES_SUBTRACT`](../../Reference/met/MmetControl.md).

If you maintain the default behavior of [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md), the order in which you specify the curves is inconsequential. |
| `M_AREA_CONVEX_HULL` | Adds an area tolerance, based on the convex hull, for the surface of an edgel, point, or circle feature. The icon for this tolerance is:

*[Image: Metro_Icon_SurfaceAreaConvexHull.png]*

[`M_AREA_CONVEX_HULL`](../../Reference/met/MmetAddTolerance.md) validates that the surface area (convex hull) of the specified feature ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md)) falls within the minimum and maximum limit values. |
| `M_AREA_SIMPLE` | Adds a simple area tolerance for the surface of an edgel, point, or circle feature. The icon for this tolerance is:

*[Image: Metro_Icon_SurfaceArea.png]*

[`M_AREA_SIMPLE`](../../Reference/met/MmetAddTolerance.md) validates that the surface area of the specified feature ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md)) falls within the minimum and maximum limit values. |
| `M_AREA_UNDER_CURVE_MAX` | Adds an area tolerance for the maximum area under a curve of edgels or points. The icon for this tolerance is:

*[Image: Metro_Icon_AreaUnderCurveMax.png]*

[`M_AREA_UNDER_CURVE_MAX`](../../Reference/met/MmetAddTolerance.md) validates the maximum area between a curve (edgel or point) feature and a line feature ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md)). Aurora Imaging Library considers the maximum area to be between an internally established curve under the furthest set of edgels or points and the line. The order of the features specified with the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter is inconsequential, since Aurora Imaging Library always validates the curve (edgel or point) feature against the line feature. [`M_AREA_UNDER_CURVE_MAX`](../../Reference/met/MmetAddTolerance.md) is typically used to validate the entire area under a curve (edgel or point) feature and a line, regardless of any interfering edges or noise.

*[Image: Metro_AreaUnderCurveMax_Typical.png]*

When specifying an edgel feature with this tolerance, Aurora Imaging Library internally sorts and filters the edgels, and uses a maximum gap between them, to determine whether they are part of the same curve. To specify an explicit gap size, call [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_CURVE_EDGEL_GAP_SIZE`](../../Reference/met/MmetControl.md).

When specifying a point feature with this tolerance, Aurora Imaging Library does not manipulate the points and chains all of them, assuming the resulting curve is well defined; for example, Aurora Imaging Library assumes the chained points do not create a degenerate polygonal figure, where curves cross over each other.

If the specified curve (edgel or point) feature that you are validating crosses over the specified line feature, you are considered to have opposing areas (that is, there are areas on both sides of the line), and the tolerance fails, by default. You can allow Aurora Imaging Library to consider opposing (negative) areas when establishing the area tolerance, by calling [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_AREA_UNDER_CURVE_ALLOW_NEGATIVE`](../../Reference/met/MmetControl.md). In general, the curve (edgel or point) feature should remain on one side of the line. |
| `M_AREA_UNDER_CURVE_MIN` | Adds an area tolerance for the minimum area under a curve of edgels. The icon for this tolerance is:

*[Image: Metro_Icon_AreaUnderCurveMin.png]*

[`M_AREA_UNDER_CURVE_MIN`](../../Reference/met/MmetAddTolerance.md) validates the minimum area between a curve (edgel or point) feature and a line feature ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md)). Aurora Imaging Library considers the minimum area to be between an internally established curve under the furthest set of edgels or points and the line. The order of the features specified with the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter is inconsequential, since Aurora Imaging Library always validates the curve (edgel or point) feature against the line feature. [`M_AREA_UNDER_CURVE_MIN`](../../Reference/met/MmetAddTolerance.md) is typically used to validate the exclusive area under a curve (edgel or point) feature and a line, which excludes the area under the curve and any interfering edges or noise.

*[Image: Metro_AreaUnderCurveMin_Typical.png]*

When specifying an edgel feature with this tolerance, Aurora Imaging Library performs an internal sort and filtering of the edgels, and uses a maximum gap between edgels to determine whether they are part of the same curve. To specify an explicit gap size, call [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_CURVE_EDGEL_GAP_SIZE`](../../Reference/met/MmetControl.md).

When specifying a point feature with this tolerance, Aurora Imaging Library does not manipulate the points and chains all of them, assuming the resulting curve is well defined; for example, Aurora Imaging Library assumes the chained points do not create a degenerate polygonal figure, where curves cross over each other.

If the specified curve (edgel or point) feature that the tolerance validates crosses over the specified line feature, you are considered to have opposing areas (that is, there are areas on both sides of the line), and the tolerance fails, by default. You can allow Aurora Imaging Library to consider opposing (negative) areas when establishing the area tolerance, by calling [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_AREA_UNDER_CURVE_ALLOW_NEGATIVE`](../../Reference/met/MmetControl.md). In general, the curve (edgel or point) feature should remain on one side of the line. |
| `M_CONCENTRICITY` | Adds a concentricity tolerance. The icon for this tolerance is:

*[Image: Metro_Icon_Concentricity.png]*

[`M_CONCENTRICITY`](../../Reference/met/MmetAddTolerance.md) validates the concentricity between a combination of two of the following features: arc or circle. This tolerance validates the distance between the centers of the features. Ideally, concentric features have the same center. The order of the features specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter is inconsequential.

*[Image: Metro_Tolerance_Concentricity.png]* |
| `M_DISTANCE_MAX` | Adds a maximum distance tolerance. The icon for this tolerance is:

*[Image: Metro_Icon_Distance.png]*

[`M_DISTANCE_MAX`](../../Reference/met/MmetAddTolerance.md) validates that a specified maximum distance is separating any two features except lines. By default, Aurora Imaging Library uses the distance at which the features are the farthest. In this case, the order of the features in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter is inconsequential.

*[Image: Metro_Tolerance_Distance_Maximum.png]*

You can explicitly specify an angle at which to apply the maximum distance tolerance between two features, by calling [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_DISTANCE_MODE`](../../Reference/met/MmetControl.md). This can be seen as a type of caliper tolerance between the features. In this case, the order of the features in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter can affect your results, since Aurora Imaging Library considers the specified angle at which to measure the maximum distance to be relative to the first feature. Aurora Imaging Library applies the angle counter-clockwise. |
| `M_DISTANCE_MIN` | Adds a minimum distance tolerance. The icon for this tolerance is:

*[Image: Metro_Icon_Distance.png]*

[`M_DISTANCE_MIN`](../../Reference/met/MmetAddTolerance.md) validates that a specified minimum distance is separating any two features. By default, Aurora Imaging Library uses the distance at which the features are the closest. In this case, the order of the features in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter is inconsequential.

*[Image: Metro_Tolerance_Distance_Minimum.png]*

You can explicitly specify an angle at which to apply the minimum distance tolerance between two features, by calling [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_DISTANCE_MODE`](../../Reference/met/MmetControl.md). This can be seen as a type of caliper tolerance between the features. In this case, the order of the features in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter can affect your results, since Aurora Imaging Library considers the specified angle at which to measure the minimum distance to be relative to the first feature. Aurora Imaging Library applies the angle counter-clockwise. |
| `M_LENGTH` | Adds a length tolerance.

When used with an arc or circle, the icon for this tolerance is:

*[Image: Metro_Icon_Length_Circle.png]*

When used with a segment or edgel, the icon for this tolerance is:

*[Image: Metro_Icon_Length_Segment.png]*

[`M_LENGTH`](../../Reference/met/MmetAddTolerance.md) validates the length of one of the following features: segment, edgel, circle, or arc. The length of a circle is its circumference.

*[Image: Metro_Tolerance_Length.png]*

You should only use [`M_LENGTH`](../../Reference/met/MmetAddTolerance.md) with an edgel feature if it represents one continuous curve (path) that does not cross over itself. |
| `M_PARALLELISM` | Adds a parallelism tolerance. The icon for this tolerance is:

*[Image: Metro_Icon_Parallelism.png]*

A parallelism tolerance validates the degree to which two features are parallel. A parallelism tolerance can be applied between the following features:

- Two linear features (segments and lines).
- A linear feature (segment or line) and an edgel feature.

For any combination of 2 linear features (segments and lines), [`M_PARALLELISM`](../../Reference/met/MmetAddTolerance.md) validates that the angle of the two features, relative to the global reference frame, is the same. The maximum tolerance parameter sets the valid angular range, from the nominal angle (0°) (that is, 0° +/- [`ValueMax`](../../Reference/met/MmetAddTolerance.md)). In this case, you should only set the maximum tolerance parameter, and set the minimum tolerance parameter to 0. The order of the features specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter, as well as the start/end of the segments, is inconsequential.

*[Image: Metro_Tolerance_Parallelism_Two_Linear.png]*

Note that, for parallelism tolerances between two linear features, angles are remapped to 0°; that is, the angle that is measured, and the angular range that you must provide, is the angle that is closest to 0°.

*[Image: Metro_Tolerance_Parallelism_Angle_Measured.png]*

For a linear feature (segment or line) and an edgel feature, [`M_PARALLELISM`](../../Reference/met/MmetAddTolerance.md) validates the width of the projection of the edgel feature on a theoretical line that is perpendicular to the segment or line feature. The minimum and maximum tolerance values set the valid projection width. The smaller the width, the more the edgel feature is parallel to the given segment or line feature. Typically, the minimum width value is set to 0. The width of the projection is in pixel or world units. The second feature specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter must be edgel.

*[Image: Metro_Tolerance_Parallelism_Linear_Edgel.png]* |
| `M_PERIMETER_CONVEX_HULL` | Adds a perimeter tolerance, based on the convex hull, for the surface of an edgel, point, or circle feature. The icon for this tolerance is:

*[Image: Metro_Icon_SurfacePerimeterConvexHull.png]*

[`M_PERIMETER_CONVEX_HULL`](../../Reference/met/MmetAddTolerance.md) validates that the surface perimeter (convex hull) of the specified feature ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md)) falls within the minimum and maximum limit values. |
| `M_PERIMETER_SIMPLE` | Adds a simple perimeter tolerance for the surface of an edgel, point, or circle feature. The icon for this tolerance is:

*[Image: Metro_Icon_SurfacePerimeter.png]*

[`M_PERIMETER_SIMPLE`](../../Reference/met/MmetAddTolerance.md) validates that the surface perimeter of the specified feature ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md)) falls within the minimum and maximum limit values. |
| `M_PERPENDICULARITY` | Adds a perpendicularity tolerance. The icon for this tolerance is:

*[Image: Metro_Icon_Perpendicularity.png]*

A perpendicularity tolerance validates the degree to which two features are perpendicular. A perpendicularity tolerance can be applied between the following features:

- Two linear features (segments and lines).
- A linear feature (segment or line) and an edgel feature.

For any combination of 2 linear features (segments and lines), [`M_PERPENDICULARITY`](../../Reference/met/MmetAddTolerance.md) validates that the angle between the two features is 90°. The maximum tolerance parameter sets the valid angular range, from the nominal angle (90°) (that is, 90° +/- [`ValueMax`](../../Reference/met/MmetAddTolerance.md)). In this case, you should only set the maximum tolerance parameter, and set the minimum tolerance parameter to 0. The order of the features specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter, as well as the start/end of the segments, is inconsequential.

*[Image: Metro_Tolerance_Perpendicularity_Two_Linear.png]*

Note that, for perpendicularity tolerances between two linear features, angles are remapped to 90°; that is, the angle that is measured, and the angular range that you must provide, is the angle that is closest to 90°.

*[Image: Metro_Tolerance_Perpendicularity_Angle_Measured.png]*

For a linear feature (segment or line) and an edgel feature, [`M_PERPENDICULARITY`](../../Reference/met/MmetAddTolerance.md) validates the width of the projection of the edgel feature on a theoretical line that is parallel to the segment or line feature. The minimum and maximum tolerance values set the valid projection width. The smaller the width, the more the edgel feature is perpendicular to the given segment or line feature. Typically, the minimum width value is set to 0. The width of the projection is in pixel or world units. The second feature specified in the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter must be edgel.

*[Image: Metro_Tolerance_Perpendicularity_Linear_Edgel.png]* |
| `M_POSITION_X` | Adds a positioning tolerance, along the X-direction of the specified reference frame. The icon for this tolerance is:

*[Image: Metro_Icon_PositionX.png]*

A positioning tolerance can be used to validate the geometric relationship between a reference frame and one of the following features: local frame, point, or circle. For a circle feature, only the position of its center is validated, not its contour. If you only specify one feature, Aurora Imaging Library validates the geometric relationship between that feature and its reference frame.

When using a positioning tolerance, the first element provided to the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter must be the label value of the reference frame, while the second element must be the label value of the feature to validate. If you only specify one feature, Aurora Imaging Library validates the geometric relationship between that feature and its reference frame (every feature is associated to a reference frame).

*[Image: Metro_Tolerance_Position_X.png]* |
| `M_POSITION_Y` | Adds a positioning tolerance, along the Y-direction of the specified reference frame. The icon for this tolerance is:

*[Image: Metro_Icon_PositionY.png]*

A positioning tolerance can be used to validate the geometric relationship between a reference frame and one of the following features: local frame, point, or circle. For a circle feature, only the position of its center is validated, not its contour.

When using a positioning tolerance, the first element provided to the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md) parameter must be the label value of the reference frame, while the second element must be the label value of the feature to validate. If you only specify one feature, Aurora Imaging Library validates the geometric relationship between that feature and its reference frame (every feature is associated to a reference frame).

*[Image: Metro_Tolerance_Position_Y.png]* |
| `M_RADIUS` | Adds a radius tolerance. The icon for this tolerance is:

*[Image: Metro_Icon_Radius.png]*

[`M_RADIUS`](../../Reference/met/MmetAddTolerance.md) validates the radius of the specified arc or circle feature. The radius is the linear length between the circle or arc's center and perimeter.

*[Image: metro_tolerance_radius.png]* |
| `M_ROUNDNESS` | Adds a roundness tolerance. The icon for this tolerance is:

*[Image: Metro_Icon_Roundness.png]*

[`M_ROUNDNESS`](../../Reference/met/MmetAddTolerance.md) validates the roundness of one of the following features: arc, circle, or edgel.

Aurora Imaging Library establishes a feature's roundness by calculating the distance between the inner and outer circular curves formed by the given feature. Note that the inner and outer curves are concentric.

*[Image: Metro_Tolerance_Roundness.png]* |
| `M_STRAIGHTNESS` | Adds a straightness tolerance. The icon for this tolerance is:

*[Image: Metro_Icon_Straightness.png]*

[`M_STRAIGHTNESS`](../../Reference/met/MmetAddTolerance.md) validates the straightness of one of the following features: segment or edgel.

This is done by calculating the distance between the two parallel lines that are formed by the inner and outer boundaries of the feature. The angle of the two parallel lines is chosen such that the distance between them is the smallest.

*[Image: Metro_Tolerance_Straightness.png]* |

*For modifying a geometric tolerance*

| Value | Description |
| --- | --- |
| `M_MODIFY` | Modifies the features that the tolerance uses, and the tolerance's minimum and maximum limit values. You cannot modify only the features or only the limits. Use [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddTolerance.md), [`SubFeatureIndexArrayPtr`](../../Reference/met/MmetAddTolerance.md), [`ValueMin`](../../Reference/met/MmetAddTolerance.md), and [`ValueMax`](../../Reference/met/MmetAddTolerance.md) to specify the new features and limits. |

### `ToleranceLabel` *(in, AIL_INT)*

Specifies the label of the target tolerance.

*For labeling the tolerance*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library automatically selects the tolerance's label value. Once a tolerance is added, you can inquire its label using [`MmetInquire`](../../Reference/met/MmetInquire.md) with the tolerance's index and [`M_LABEL_VALUE`](../../Reference/met/MmetInquire.md). |
| `Value > 0` | Specifies the label value. Each tolerance must have a unique label otherwise you will get an error. You can have an extremely high number of labels (up to **M_MET_TOLERANCE_ID_MAX**). |

### `ValueMin` *(in, AIL_DOUBLE)*

Specifies the minimum value of the geometric tolerance.

*For specifying the minimum geometric tolerance*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the minimum value. |

### `ValueMax` *(in, AIL_DOUBLE)*

Specifies the maximum value of the geometric tolerance.

*For specifying the maximum geometric tolerance*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the maximum value. |

### `FeatureLabelArrayPtr` *(in, *AIL_INT)*

Specifies the array that holds the label values of the features whose relationship to validate. The features must have already been added to the metrology template of the metrology context. To add a feature, use [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md). The specified features, whose relationship you want to validate, can be considered the base features of the tolerance.

### `SubFeatureIndexArrayPtr` *(in, *AIL_INT)*

Specifies the array that holds the index values of the multiple features' subfeatures whose relationship the added tolerance will validate. For non-multiple features (single features), specify 0 as the index. If the tolerance validates the relationship between only single features (there are no multiple features), set [`SubFeatureIndexArrayPtr`](../../Reference/met/MmetAddTolerance.md) to `M_NULL`.

### `SizeOfArray` *(in, AIL_INT)*

Specifies the size of the arrays that hold the information that will be used to add the geometric tolerance ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddFeature.md)and [`SubFeatureIndexArrayPtr`](../../Reference/met/MmetAddFeature.md)).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

To draw the actual area that Aurora Imaging Library uses to define this tolerance, call [`MmetDraw`](../../Reference/met/MmetDraw.md) with [`M_DRAW_TOLERANCE_AREA...`](../../Reference/met/MmetDraw.md).
