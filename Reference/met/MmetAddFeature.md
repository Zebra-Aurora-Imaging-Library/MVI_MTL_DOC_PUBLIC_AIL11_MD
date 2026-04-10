---
doctype: Reference
module: met
function: MmetAddFeature
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / met / MmetAddFeature"
---

# MmetAddFeature

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

> Add a feature to, or modify a feature of, the metrology template of a metrology context.

## Syntax

```c
void MmetAddFeature(
    AIL_ID          ContextId,                //out
    AIL_INT64       FeatureType,              //in
    AIL_INT64       Geometry,                 //in
    AIL_INT         FeatureLabel,             //in
    AIL_INT64       BuildOperation,           //in
    const AIL_INT * FeatureLabelArrayPtr,     //in
    const AIL_INT * SubFeatureIndexArrayPtr,  //in
    AIL_INT         SizeOfArray,              //in
    AIL_INT64       ControlFlag               //in
)
```

## Description

This function adds a feature to the metrology template of the specified metrology context. This function can also modify an existing feature of a metrology template. To delete features, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_DELETE`](../../Reference/met/MmetControl.md).

To add several features to the template, you must call this function for each feature to add.

Features can either be physically measured from the target image or constructed from a set of other features, which can be referred to as base features. When a feature is composed of several instances of the same feature type, it is considered a multiple feature. Every instance of the feature type is a subfeature of the multiple feature. A single feature is a feature that has only one instance of the feature type. All supported features are single features, except for the edgel feature ([`M_MEASURED`](../../Reference/met/MmetAddFeature.md) or [`M_CONSTRUCTED`](../../Reference/met/MmetAddFeature.md) with [`M_EDGEL`](../../Reference/met/MmetAddFeature.md)) and the measured multiple point feature ([`M_MEASURED`](../../Reference/met/MmetAddFeature.md) with [`M_POINT`](../../Reference/met/MmetAddFeature.md)). When adding a constructed feature that requires a point, it is possible to isolate and use a specific point subfeature of a measured multiple point feature. This is not true for the edgel feature since it is always used as a group.

Every feature has a position in the metrology template. For a physically measured feature, its position can be considered to be set according to the metrology region in which it is expected to be established ([`MmetSetRegion`](../../Reference/met/MmetSetRegion.md)). By default, the metrology region is infinite (the whole target image). For a constructed feature, its position in the template is defined by the position of the features used to construct it (its base features). The position of a feature is relative to a reference coordinate system, referred to as the reference frame. The global frame is the default. If the target image is not calibrated, the global frame's default origin (0,0) is aligned with the center of the top-left corner pixel of the target image. If the target image is calibrated, the global frame's default origin is aligned with the origin of the relative (world) coordinate system. To change the reference frame to a local frame, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_REFERENCE_FRAME`](../../Reference/met/MmetControl.md). Local frames are coordinate systems located anywhere within the metrology template and defined as metrology features.

Once a feature is added to the template, it is possible to change its position using [`MmetSetPosition`](../../Reference/met/MmetSetPosition.md). In addition, you can modify the feature so that it is constructed from a new set of base features and/or a new build operation; to do so, call [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_MODIFY`](../../Reference/met/MmetAddFeature.md). Note that you cannot modify a feature's geometry.

To define the acceptable geometric relationship between several features in the template or the acceptable deviation from the definition of a feature, add a geometric tolerance, using [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md). To calculate the added features and validate the geometric tolerances in the target image, use [`MmetCalculate`](../../Reference/met/MmetCalculate.md). To change the settings of a feature in the template, use [`MmetControl`](../../Reference/met/MmetControl.md).

When adding a constructed feature with a fit operation, or when adding a constructed edgel feature with the clone operation, you can specify base features that have a delimited metrology region ([`MmetSetRegion`](../../Reference/met/MmetSetRegion.md)). By default, Aurora Imaging Library uses an infinite metrology region ([`M_INFINITE`](../../Reference/met/MmetSetRegion.md)).

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the identifier of the metrology context containing the template. The metrology context must have been previously allocated on the required system using [`MmetAlloc`](../../Reference/met/MmetAlloc.md).

### `FeatureType` *(in, AIL_INT64)*

Specifies the type of feature to add ([`M_CONSTRUCTED`](../../Reference/met/MmetAddFeature.md) or [`M_MEASURED`](../../Reference/met/MmetAddFeature.md)) or specifies to modify a feature ([`M_MODIFY`](../../Reference/met/MmetAddFeature.md)).

### `Geometry` *(in, AIL_INT64)*

Specifies the geometric shape of the feature to add. If modifying a feature ([`M_MODIFY`](../../Reference/met/MmetAddFeature.md)), this parameter must be set to `M_DEFAULT`.

### `FeatureLabel` *(in, AIL_INT)*

Specifies the label of the feature.

*For specifying label values*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library automatically selects the feature's label value. Once a feature is added, you can inquire its label using [`MmetInquire`](../../Reference/met/MmetInquire.md) with the feature's index and [`M_LABEL_VALUE`](../../Reference/met/MmetInquire.md). |
| `Value > 0` | Specifies the label value. Each feature must have a unique label otherwise you will get an error. You can have an extremely high number of labels (up to **M_MET_FEATURE_ID_MAX**). |

### `BuildOperation` *(in, AIL_INT64)*

Specifies the operation to use to build the feature.

### `FeatureLabelArrayPtr` *(in, *AIL_INT)*

Specifies the array that holds the label of every feature to use to construct the feature. Features used to construct a feature are considered to be its base features.

### `SubFeatureIndexArrayPtr` *(in, *AIL_INT)*

Specifies the array that holds the index of every subfeature to use to construct the feature. Features used to construct a feature are considered to be its base features.

### `SizeOfArray` *(in, AIL_INT)*

Specifies the size of the arrays that hold the information that will be used to construct the feature ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddFeature.md) and [`SubFeatureIndexArrayPtr`](../../Reference/met/MmetAddFeature.md)). When setting the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddFeature.md) parameter to `M_NULL`, set this parameter to 0.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For adding a constructed feature

To add a constructed feature, the [`FeatureType`](../../Reference/met/MmetAddFeature.md), [`Geometry`](../../Reference/met/MmetAddFeature.md), and [`BuildOperation`](../../Reference/met/MmetAddFeature.md) parameters can be set to the following values. Note that certain build operations for constructed features require base features, which you can specify using the [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddFeature.md) parameter and, if necessary, the [`SubFeatureIndexArrayPtr`](../../Reference/met/MmetAddFeature.md) parameter.

---

### `M_CONSTRUCTED`

Adds a constructed feature.  Constructed features are geometrically built using base features (other features added using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md)), or they are defined by their parametric controls (set using [`MmetControl`](../../Reference/met/MmetControl.md)). In the latter case, they are referred to as parametrically constructed features ([`M_PARAMETRIC`](../../Reference/met/MmetAddFeature.md)).  Constructed features built using an [`M_CLONE_FEATURE`](../../Reference/met/MmetAddFeature.md) operation are cloned from the specified base feature ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddFeature.md)). For example, an arc can be cloned from an arc feature. Cloned features can be scaled, rotated, and/or moved using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_CLONE_...`](../../Reference/met/MmetControl.md).  The base features of constructed features built using a fit operation ([`M_FIT`](../../Reference/met/MmetAddFeature.md), [`M_INNER_FIT`](../../Reference/met/MmetAddFeature.md), or [`M_OUTER_FIT`](../../Reference/met/MmetAddFeature.md)) can be delimited by a metrology region. You must call [`MmetSetRegion`](../../Reference/met/MmetSetRegion.md) to specify the metrology region for each base feature. The base feature of a constructed edgel feature built using [`M_CLONE_FEATURE`](../../Reference/met/MmetAddFeature.md) can also be delimited by a metrology region. Aurora Imaging Library only uses the edgels or points within the delimited region of the base feature to build the constructed feature; edgels or points outside the region are clipped.  For constructed features that require several points or edgels, the actual number of point and edgel features can vary. For example, if 3 points are needed to construct a feature, it is possible to provide one multiple point feature containing three points, three separate point features, or one multiple point feature containing two points and one point subfeature. Only the total number of elements is important, not the number of features. If you do not specify the subfeature indices, then all subfeatures are used.  Constructed features built with [`M_FIT`](../../Reference/met/MmetAddFeature.md) depend on [`M_FIT_TYPE`](../../Reference/met/MmetControl.md) and the settings related to the type of fit selected. All fit operations will fail if their coverage is less than [`M_FIT_COVERAGE_MIN`](../../Reference/met/MmetControl.md).  If the constructed feature uses edgels as a source, then the fit operations are also affected by the gradient angle settings. You can adjust these using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md) and [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md). When performing a standard best fit ([`M_STANDARD_FIT`](../../Reference/met/MmetControl.md)), more importance is typically given to the edgels whose gradient orientation follows the region's orientation.

#### `M_ARC`

Adds a constructed arc feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLONE_FEATURE` | Specifies that the arc feature is cloned from the specified base feature. |
| `M_CONSTRUCTION` | Specifies that the arc feature is built from a geometric construction using the specified base features. You can specify any three of the following as base features: points, circles, arcs, or local frames. For example, you can specify three point features, or one point, one circle, and one local frame feature. In all cases, Aurora Imaging Library requires three points to construct the arc. If you specify a circle or arc feature, Aurora Imaging Library takes its center point. If you specify a local frame feature, Aurora Imaging Library takes its origin point.  To build the arc, Aurora Imaging Library uses the first, second, and third point as the center, the start, and the end of the arc, respectively. If the radius formed by the first and second points is not equal to the radius formed by the first and third points, the constructed arc will pass as close as possible to both the second and third points. This occurs because arc features are circular arcs, which are portions of a circle. Therefore, every point on the arc must have the same radius. Note that arcs follow the counter-clockwise convention.  *[Image: MetArcConstruction.png]* |
| `M_FIT` | Specifies that the arc feature is built from the arc that best fits the specified base features. You can specify three or more edgels or points as base features. It is possible to have a combination of edgels and points. The best fit arc passes as close to as much data as possible while not necessarily touching it. Note that arcs follow the counter-clockwise convention.  The angular constraints are ignored for the base features which do not have angle information.  *[Image: MetArcFit.png]* |
| `M_PARAMETRIC` *(default)* | Specifies that the arc feature is built from the following [`MmetControl`](../../Reference/met/MmetControl.md) parametric controls: [`M_POSITION_X`](../../Reference/met/MmetControl.md), [`M_POSITION_Y`](../../Reference/met/MmetControl.md), [`M_ANGLE_START`](../../Reference/met/MmetControl.md), [`M_ANGLE_END`](../../Reference/met/MmetControl.md), and [`M_RADIUS`](../../Reference/met/MmetControl.md). |

#### `M_CIRCLE`

Adds a constructed circle feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLONE_FEATURE` | Specifies that the circle feature is cloned from the specified base feature. |
| `M_CONSTRUCTION` | Specifies that the circle feature is built from a geometric construction using the specified base features. You can specify any two of the following as base features: points, circles, arcs, or local frames. For example, you can specify two point features, or one point and one circle. In all cases, Aurora Imaging Library requires two points to construct the circle. If you specify a circle or arc feature, Aurora Imaging Library takes its center point. If you specify a local frame feature, Aurora Imaging Library takes its origin point.  To build the circle, Aurora Imaging Library uses the first point as the center of the circle, while the second point is considered to be on the circle's contour. The distance between the two points is the new circle's radius. |
| `M_FIT` | Specifies that the circle feature is built from the circle that best fits the specified base features. You can specify three or more edgels or points as base features. It is possible to have a combination of edgels and points. The best fit circle passes as close to as much data as possible while not necessarily touching it.  The angular constraints are ignored for the base features which do not have angle information.  *[Image: MetCircleFit.png]* |
| `M_INNER_FIT` | Specifies that the circle feature is built from the best inner fit of a circle around the specified base features. You can specify three or more edgels or points as base features. It is possible to have a combination of edgels and points. The inner fit for a circle feature is the largest possible circle that does not contain any edgels or points. The circle feature is fit to the edgels or points using [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md) mode. Therefore, all settings related to [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md) will affect the [`M_INNER_FIT`](../../Reference/met/MmetAddFeature.md) circle.  The angular constraints are ignored for the base features which do not have angle information.  *[Image: MetCircleInnerFit.png]* |
| `M_OUTER_FIT` | Specifies that the circle feature is built from the best outer fit of a circle around the specified base features. You can specify three or more edgels or points as base features. It is possible to have a combination of edgels and points. The outer fit for a circle feature is the smallest possible circle that contains all the edgels or points. The circle feature is fit to the edgels or points using [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md) mode. Therefore, all settings related to [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md) will affect the [`M_OUTER_FIT`](../../Reference/met/MmetAddFeature.md) circle.  The angular constraints are ignored for the base features which do not have angle information.  *[Image: MetCircleOuterFit.png]* |
| `M_PARAMETRIC` *(default)* | Specifies that the circle feature is built from the following [`MmetControl`](../../Reference/met/MmetControl.md) parametric controls: [`M_POSITION_X`](../../Reference/met/MmetControl.md), [`M_POSITION_Y`](../../Reference/met/MmetControl.md), and [`M_RADIUS`](../../Reference/met/MmetControl.md). |

#### `M_EDGEL`

Adds a constructed edgel feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLONE_FEATURE` | Specifies that the edgel feature is cloned from the specified base feature. |
| `M_COPY_FEATURE_EDGELS` *(default)* | Specifies that the edgel feature is constructed by copying the edgels of the specified base features. You can specify physically measured features or constructed edgel features as base features.  For physically measured base features, you can restrict the operation to copy only the active edgels or only the fitted edgels of the base features, using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_EDGEL_TYPE`](../../Reference/met/MmetControl.md). For constructed edgel base features, Aurora Imaging Library considers the edgels active (not fitted). |
| `M_EXTERNAL_FEATURE` | Specifies that the edgel feature is constructed from edgels or points established using other Aurora Imaging Library modules or third party software. Use [`MmetPut`](../../Reference/met/MmetPut.md)to add the edgels/points to the feature. |
| `M_GROUP_EDGELS` | Specifies that the edgel feature is constructed by grouping the edgels of the specified base features. By default, Aurora Imaging Library uses all edgels. To change this (for example, to group edgels according to the distance to a point), call [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_GROUP_SELECTION_MODE`](../../Reference/met/MmetControl.md). |

#### `M_LINE`

Adds a constructed line feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE` | Specifies that the line feature is built at a specified angle from the specified linear base feature (segment or line), and passing through the specified point base feature. You can specify one or two base features.  If one base feature is specified, it must be a point. In this case, the reference line from which the angle is measured is the X-axis of the reference frame.  If two base features are specified, one must be a point and the other must be a line or a segment. The order in which you specify the base features does not matter.  To specify the angle, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_ANGLE`](../../Reference/met/MmetControl.md) and set a value between 0° and 360°. The angle is measured counter-clockwise, relative to the linear feature (from start to end).  *[Image: MetLineAngle.png]* |
| `M_BISECTOR` | Specifies that the line feature is built by dividing an angle formed by the specified base features. You can specify three points or two lines as base features. By default, Aurora Imaging Library builds the feature by dividing the angle, formed by the base features, in two equal parts.  If three points are specified, the first point is used as the vertex of the angle.  *[Image: MetLineBisector.png]*  If two lines are specified, the bisector line is built where it divides the angle formed by the first line and the second line in the counter-clockwise direction.  *[Image: MetLineBisectorTwoLines.png]*  If specifying two lines as base features, you can use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_LINE_BISECTOR_TYPE`](../../Reference/met/MmetControl.md) to alter how the new line is built. For example, you can build the bisector line at the middle of the smallest or biggest angle formed by the two line base features. |
| `M_CLONE_FEATURE` | Specifies that the line feature is cloned from the specified base feature. |
| `M_CONSTRUCTION` | Specifies that the line feature is built from a geometric construction using the specified base features. You can specify any two of the following as base features: points, circles, arcs, or local frames. For example, you can specify two point features, or one point and one circle. In all cases, Aurora Imaging Library requires two points to construct the line. If you specify a circle or arc feature, Aurora Imaging Library takes its center point. If you specify a local frame feature, Aurora Imaging Library takes its origin point.  To build the line, Aurora Imaging Library ensures that it passes through the two points specified. |
| `M_FIT` | Specifies that the line feature is built from the resulting best fit of the specified base feature. You can specify a segment as the base feature. |
| `M_PARALLEL` | Specifies that the line feature is built parallel to the specified linear base feature (segment or line), and passing through the specified point base feature. You can specify one or two base features.  If one base feature is specified, it must be a point. In this case, the line feature is built parallel to the X-axis of the reference frame.  If two base features are specified, one must be a point and the other must be a line or a segment. The order in which you specify the base features does not matter.  *[Image: MetLineParallel.png]* |
| `M_PARAMETRIC` *(default)* | Specifies that the line feature is built from the following [`MmetControl`](../../Reference/met/MmetControl.md) parametric controls: [`M_LINE_A`](../../Reference/met/MmetControl.md), [`M_LINE_B`](../../Reference/met/MmetControl.md), and [`M_LINE_C`](../../Reference/met/MmetControl.md). |
| `M_PERPENDICULAR` | Specifies that the line feature is built perpendicular to the specified linear base feature (segment or line), and passing through the specified point base feature. You can specify one or two base features.  If one base feature is specified, it must be a point. In this case, the line feature is built perpendicular to the X-axis of the reference frame.  If two base features are specified, one must be a point and the other must be a line or a segment. The order in which you specify the base features does not matter.  *[Image: MetLinePerpendicular.png]* |
| `M_SECTOR_RELATIVE` | Specifies that the line feature is built where it divides, in two parts, an angle formed by the specified base features. You can specify three points as base features. The first point is used as the vertex of the angle.  Specify the size of the angle between the line and the first pair of points (first and second points) using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_ANGLE`](../../Reference/met/MmetControl.md). Express the angle as a percentage of the total angle of all three points.  *[Image: MetLineSectorRelative.png]* |

#### `M_LOCAL_FRAME`

Adds a constructed local frame feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLONE_FEATURE` | Specifies that the local frame feature is cloned from the specified base feature. |
| `M_CONSTRUCTION` | Specifies that the local frame feature is built from a geometric construction using one or two specified base features. You can specify one or two of the following as base features: points, circles, arcs, or local frames. You also specify a segment if it is the only base feature.  When you specify two base features, the local frame's origin is set at the center of the first base feature, and the local frame's X-axis is set to pass through and in the direction of the center of the second base feature. If you specify a local frame as a base feature, Aurora Imaging Library uses its origin to construct the local frame.  *[Image: MetLocalFrameConstruction.png]*  You can also build a local frame by specifying only one base feature. Aurora Imaging Library uses the center of the specified feature as the origin of the local frame. Aurora Imaging Library sets the local frame's X-axis at a specified angle from the X-axis of the reference frame; use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_ANGLE`](../../Reference/met/MmetControl.md) to specify the local frame's angle. The angle is applied in the counter-clockwise direction. When the base feature is a segment, the origin of the local frame is set at the segment's center; the local frame's X-axis is aligned with the segment, pointing in the direction from the segment's first point to its second point.  *[Image: MetLocalFrameConstructionPoint.png]* |
| `M_PARAMETRIC` *(default)* | Specifies that the local frame feature is built from the following [`MmetControl`](../../Reference/met/MmetControl.md) parametric controls: [`M_POSITION_X`](../../Reference/met/MmetControl.md), [`M_POSITION_Y`](../../Reference/met/MmetControl.md), and [`M_ANGLE`](../../Reference/met/MmetControl.md). Note that the angle is measured counter-clockwise, and is relative to the global frame. |

#### `M_POINT`

Adds a constructed point feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE_ABSOLUTE` | Specifies that the point feature is built at the specified absolute angle, on the contour of the specified base feature. You can specify either an arc or a circle as the base feature. The angle, in degrees, is relative to the start of the contour of the specified feature and is set using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_ANGLE`](../../Reference/met/MmetControl.md). The angle is measured counter-clockwise.  *[Image: MetPointAngleAbsolute.png]* |
| `M_ANGLE_RELATIVE` | Specifies that the point feature is built at the specified relative angle, on the contour of the specified base feature. You can specify either an arc or a circle as the base feature. Specify the angle as a percentage, relative to the start of the contour using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_ANGLE`](../../Reference/met/MmetControl.md).  *[Image: MetPointAngleRelative.png]* |
| `M_CENTER` | Specifies that the point feature is built at the center of the specified base feature(s). You can specify one, or a combination, of the following as base features: arc, circle, edgel, local frame, point, or segment. If multiple features are provided, the point feature is built at the center of gravity of the centers of all the features.  *[Image: MetPointCenter.png]*  [`M_CENTER`](../../Reference/met/MmetAddFeature.md) does not necessarily build a point on the feature's edge. Use [`M_MIDDLE`](../../Reference/met/MmetAddFeature.md) if you want to build a point at the edge's midpoint. |
| `M_CLONE_FEATURE` | Specifies that the point feature is cloned from the specified base feature. |
| `M_CLOSEST` | Specifies that the point feature is built at the point on the first specified base feature that is, by default, closest to the other (one or more) specified base features.  *[Image: MetPointClosest.png]*  You can specify any features as the base features.  You can explicitly specify an angle at which to build the closest point between the features, by calling [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_DISTANCE_MODE`](../../Reference/met/MmetControl.md). This can be seen as a type of caliper feature. Aurora Imaging Library considers the specified distance mode angle at which to measure the closest distance to be relative to the first base feature. Aurora Imaging Library applies the angle counter-clockwise. |
| `M_CLOSEST_TO_INFINITE_POINT` | Specifies that the point feature is built, at the position on the base feature, that is the closest to an infinite point. This is a virtual point that Aurora Imaging Library establishes, at infinity, along the direction (X-axis) of the local frame. To change the angle along which to establish the infinite point, call [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_ANGLE`](../../Reference/met/MmetControl.md). Aurora Imaging Library measures this angle counter-clockwise, relative to the X-axis of the local frame.  *[Image: M_CIRCLE_ARC_CLOSEST_TO_INFINITY.png]*  The base features can be any feature except line, which is itself infinite (unbounded).  You can, for example, use [`M_CLOSEST_TO_INFINITE_POINT`](../../Reference/met/MmetAddFeature.md) to establish the boundaries of an edgel feature by fetching the coordinates of the constructed closest infinite points at 0°, 90, 180°, and 270° angles. |
| `M_EXTENDED_INTERSECTION` | Specifies that the point feature is built at the point of intersection between the extension of the first and second specified base features. Note that the following can be extended: a segment, which is extended into a line, and an arc, which is extended into a circle.  You can specify the following as base features:  - A segment feature and a segment, arc, circle, line, segment, or edgel feature. - An arc feature and a segment, arc, circle, line, or edgel feature. - A circle feature and a segment or arc feature. - A line feature and a segment or arc feature. - An edgel feature and a segment or arc feature.  If there is more than one intersection candidate, specify the index of the intersection at which to build the feature, using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_OCCURRENCE`](../../Reference/met/MmetControl.md); the intersection is relative to the first feature that has a direction. The default intersection is the one indexed as 0.  *[Image: MetPointExtendedIntersection.png]* |
| `M_INTERSECTION` | Specifies that the point feature is built at the point of intersection between the first and second specified base features.  You can specify the following as base features:  - A segment feature and a segment, arc, circle, line, or edgel feature. - An arc feature and a segment, arc, circle, line, or edgel feature. - A circle feature and a segment, arc, circle, or edgel feature. - A line feature and a segment, arc, line, or edgel feature. - An edgel feature and a segment, arc, circle, line, or edgel feature.  If there is more than one intersection candidate, specify the index of the intersection at which to build the feature, using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_OCCURRENCE`](../../Reference/met/MmetControl.md); the intersection is relative to the first feature that has a direction. The default intersection is the one indexed as 0.  *[Image: MetPointIntersection.png]*  For two circle intersections, specify the intersections relative to the segment that can be created by joining the circles' centers. The first intersection obtained by scanning the first circle counter-clockwise from the segment is indexed as 0; the other is indexed at 1.  *[Image: MetPointIntersection2.png]* |
| `M_MAX_DISTANCE_POINT` | Specifies that the point feature is built at the point on the first specified base feature that is, by default, farthest from any one point on the other (one or more) specified base features.  *[Image: MetPointMaxDistancePoint.png]*  You can specify any feature except line as the first base feature, and any feature as the other base features. |
| `M_MAX_OF_MIN_DISTANCE_POINT` | Specifies that the point feature is built at the point on the first specified base feature that is the farthest of all the closest points on the second base feature. This point can be considered the maximin (the greatest of all the smallest).  *[Image: MetPointMaxDistancePointFromAll.png]*  You can specify any feature except a line as the first base feature, and any feature except an edgel as the second base feature. |
| `M_MIDDLE` | Specifies that the point feature is built at the midpoint of the contour (edge) of the specified base feature. You can specify a segment, arc, circle, or edgel as the base feature. The middle of the contour of a circle is where the circle intersects the negative X-axis at 180°.  When using an edgel as the base feature, the point feature is built at the position of the edgel occupying the center position in the group (rounded down). For example, for an edgel feature containing 5 edgels, [Edgel0, Edgel1, Edgel2, Edgel3, Edgel4], the point is built at Edgel2's position.  [`M_MIDDLE`](../../Reference/met/MmetAddFeature.md) builds a point, located on the feature's edge, that splits the edge at the halfway mark. Use [`M_CENTER`](../../Reference/met/MmetAddFeature.md) if you want to build the point feature at the center point of the specified features and not necessarily on the feature's edge. |
| `M_PARAMETRIC` *(default)* | Specifies that the point feature is built from the following [`MmetControl`](../../Reference/met/MmetControl.md) parametric controls: [`M_POSITION_X`](../../Reference/met/MmetControl.md)and [`M_POSITION_Y`](../../Reference/met/MmetControl.md). |
| `M_POSITION_ABSOLUTE` | Specifies that the point feature is built at the specified position on the contour of the specified base feature. You can specify a segment, arc, circle, or edgel feature as the base feature. The position is relative to the start of the contour of the specified feature and is set using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_POSITION`](../../Reference/met/MmetControl.md). In the example below, [`M_POSITION`](../../Reference/met/MmetControl.md) has a value of 40 and the provided segment feature has a length of 120.  *[Image: MetPointPositionAbsolute.png]* |
| `M_POSITION_END` | Specifies that the point feature is built at the end of the contour of the specified base feature.  You can specify a segment, arc, circle, or edgel feature as the base feature. Note that the end of a circle's contour is at its rightmost side, or 360°. This is the point where the circle's positive X-axis meets the circle, given a local origin at the circle's center, and is the same point as its start of contour. The end of the contour of an edgel feature is the last edgel subfeature of the edgel feature. |
| `M_POSITION_RELATIVE` | Specifies that the point feature is built at the specified relative position on the contour of the specified base feature. You can specify a segment, arc, circle, or edgel feature as the base feature. Specify the position as a percentage of the total length of the specified feature's contour, relative to the start of the contour, using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_POSITION`](../../Reference/met/MmetControl.md). In the example below, [`M_POSITION`](../../Reference/met/MmetControl.md) is set to 50%.  *[Image: MetPointPositionRelative.png]* |
| `M_POSITION_START` | Specifies that the point feature is built at the start position of the contour of the specified base feature.  You can specify a segment, arc, circle, or edgel feature as the base feature. Note that the start of a circle's contour is at its rightmost side, or 0°. This is the point where the circle's positive X-axis meets the circle, given a local origin at the circle's center. The start of the contour of an edgel feature is the first edgel subfeature of the edgel feature. |

#### `M_SEGMENT`

Adds a constructed segment feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLONE_FEATURE` | Specifies that the segment feature is cloned from the specified base feature. |
| `M_CONSTRUCTION` | Specifies that the segment feature is built from a geometric construction using the specified base features. You can specify any two of the following as base features: points, circles, arcs, or local frames. For example, you can specify two point features, or one point and one circle. In all cases, Aurora Imaging Library requires two points to construct the segment. If you specify a circle or arc feature, Aurora Imaging Library takes its center point. If you specify a local frame feature, Aurora Imaging Library takes its origin point.  To build the segment, Aurora Imaging Library uses the first point as the start of the segment and the second as the end of the segment. |
| `M_FIT` | Specifies that the segment feature is built from the line that best fits the specified base features. You can specify two or more edgels or points as base features. The best fit segment passes as close to as much data as possible while not necessarily touching it. The start of the segment is the extremity closest to the origin of the reference frame.  The angular constraints are ignored for the base features which do not have angle information.  *[Image: MetSegmentFit.png]* |
| `M_INNER_FIT` | Specifies that the segment feature is built from the segment that has the best inner fit in the specified base features. You can specify two or more edgels or points as base features.  The edgels used to find the inner fit segment are the active edgels on the counter-clockwise side of the best fit segment (from start to end). These edgels are considered active edgels since they satisfy the edgel constraints set using [`MmetControl`](../../Reference/met/MmetControl.md). The best fit segment, whose starting point is the extremity closest to the origin of the global frame, separates the inner and outer fit regions. The inner fit segment passes through the two farthest edgels of the inner fit region such that all the edgels of the inner fit region are located on the same side of the inner fit segment. All the active edgels can be projected on the segment, so the segment's extremities are determined by the two farthest active edgels.  The angular constraints are ignored for the base features which do not have angle information.  *[Image: MetSegmentInnerFit.png]* |
| `M_OUTER_FIT` | Specifies that the segment feature is built from the outer fit of the specified base features. You can specify two or more edgels or points as base features.  The edgels used to find the outer fit segment are the active edgels on the clockwise side of the best fit segment, relative to its direction. These edgels are considered active edgels since they satisfy the edgel constraints set using [`MmetControl`](../../Reference/met/MmetControl.md). The best fit segment, whose starting point is the extremity closest to the origin of the global frame, separates the inner and outer fit regions. The outer fit segment passes through the two farthest edgels of the outer fit region such that all the edgels of the outer fit region are located on the same side of the outer fit segment. All the active edgels can be projected on the segment, so the segment's extremities are determined by the two farthest active edgels.  The angular constraints are ignored for the base features which do not have angle information.  *[Image: MetSegmentOuterFit.png]* |
| `M_PARAMETRIC` *(default)* | Specifies that the segment feature is built from the following [`MmetControl`](../../Reference/met/MmetControl.md) parametric controls: [`M_POSITION_START_X`](../../Reference/met/MmetControl.md), [`M_POSITION_START_Y`](../../Reference/met/MmetControl.md), [`M_POSITION_END_X`](../../Reference/met/MmetControl.md), and [`M_POSITION_END_Y`](../../Reference/met/MmetControl.md). |

### For adding a measured feature

To add a measured feature, the [`FeatureType`](../../Reference/met/MmetAddFeature.md), [`Geometry`](../../Reference/met/MmetAddFeature.md), and [`BuildOperation`](../../Reference/met/MmetAddFeature.md) parameters can be set to the following values.

---

### `M_MEASURED`

Adds a physically measured feature.  Measured features are built using data (contour edgels) extracted from the target image within the metrology region.  Measured features built with [`M_FIT`](../../Reference/met/MmetAddFeature.md) depend on [`M_FIT_TYPE`](../../Reference/met/MmetControl.md) and the settings related to the type of fit selected. All fit operations will fail if their coverage is less than [`M_FIT_COVERAGE_MIN`](../../Reference/met/MmetControl.md).  Fit operations for measured features are also affected by the gradient angle settings, which you can control using [`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md) and [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md). When performing a standard best fit ([`M_STANDARD_FIT`](../../Reference/met/MmetControl.md)), more importance is typically given to the edgels whose gradient orientation follows the region's orientation.

#### `M_ARC`

Adds a physically measured arc feature. In this case, you must use an infinite or a ring-sector metrology region ([`MmetSetRegion`](../../Reference/met/MmetSetRegion.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIT` *(default)* | Specifies that the measured arc feature is built using a best fit operation applied to the data (edgels) extracted from the metrology region. The best fit arc passes as close to as much data as possible while not necessarily touching it.  *[Image: MetArcFitForMeasured.png]* |

#### `M_CIRCLE`

Adds a physically measured circle feature. In this case, you must use an infinite or a ring metrology region ([`MmetSetRegion`](../../Reference/met/MmetSetRegion.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIT` *(default)* | Specifies that the measured circle feature is built using a best fit operation applied to the data (edgels) extracted from the metrology region. The best fit circle passes as close to as much data as possible while not necessarily touching it.  *[Image: MetCircleFit.png]* |
| `M_INNER_FIT` | Specifies that the measured circle feature is built using the best inner fit operation applied to the data (edgels) extracted from the metrology region. The inner fit for a circle feature is the largest possible circle that does not contain any edgels. The circle feature is fit to the edgels using [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md) mode. Therefore, all settings related to [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md) will affect the [`M_INNER_FIT`](../../Reference/met/MmetAddFeature.md) circle.  *[Image: MetCircleInnerFit.png]* |
| `M_OUTER_FIT` | Specifies that the measured circle feature is built using the best outer fit operation applied to the data (edgels) extracted from the metrology region. The outer fit for a circle feature is the smallest possible circle that contains all the edgels. The circle feature is fit to the edgels using [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md) mode. Therefore, all settings related to [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md) will affect the [`M_OUTER_FIT`](../../Reference/met/MmetAddFeature.md) circle.  *[Image: MetCircleOuterFit.png]* |

#### `M_EDGEL`

Adds a physically measured edgel feature. In this case, you must use an infinite, rectangle, ring, or ring-sector metrology region ([`MmetSetRegion`](../../Reference/met/MmetSetRegion.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the measured edgel feature is built by extracting all the edgels in the metrology region. |

#### `M_POINT`

Adds a physically measured point feature. In this case, you must use an arc, infinite, or segment metrology region ([`MmetSetRegion`](../../Reference/met/MmetSetRegion.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIT` *(default)* | Specifies that the measured point feature is built using a best fit operation applied to the data (edgels) extracted from the metrology region. |

#### `M_SEGMENT`

Adds a physically measured segment feature. In this case, you must use an infinite, rectangle, or ring-sector metrology region ([`MmetSetRegion`](../../Reference/met/MmetSetRegion.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIT` *(default)* | Specifies that the measured segment feature is built from the line that best fits the data (edgels) extracted from the metrology region. The best fit segment passes as close to as much data as possible while not necessarily touching it. The start of the segment is the extremity closest to the origin of the global frame.  *[Image: MetSegmentFitForMeasured.png]* |
| `M_INNER_FIT` | Specifies that the measured segment feature is built from the segment that has the best inner fit applied to the data (edgels) extracted from the metrology region. For more information on how an inner fit segment differs from a best fit segment, see [`M_CONSTRUCTED`](../../Reference/met/MmetAddFeature.md) with [`M_SEGMENT`](../../Reference/met/MmetAddFeature.md) and [`M_INNER_FIT`](../../Reference/met/MmetAddFeature.md). |
| `M_OUTER_FIT` | Specifies that the measured segment feature is built from the segment that has the best outer fit applied to the data (edgels) extracted from the metrology region. For more information on how an outer fit segment differs from a best fit segment, see [`M_CONSTRUCTED`](../../Reference/met/MmetAddFeature.md) with [`M_SEGMENT`](../../Reference/met/MmetAddFeature.md) and [`M_OUTER_FIT`](../../Reference/met/MmetAddFeature.md). |

### For modifying a feature

To modify a feature, the [`FeatureType`](../../Reference/met/MmetAddFeature.md), [`Geometry`](../../Reference/met/MmetAddFeature.md), and [`BuildOperation`](../../Reference/met/MmetAddFeature.md) parameters can be set to the following values.

---

### `M_MODIFY`

Modifies the build operation of the specified feature, and/or for a constructed feature, modifies its base features, which is the set of features used to build the constructed feature.  To specify the new features from which to construct the specified feature, use [`FeatureLabelArrayPtr`](../../Reference/met/MmetAddFeature.md) and [`SubFeatureIndexArrayPtr`](../../Reference/met/MmetAddFeature.md).

#### `M_DEFAULT`

Specifies to use the current geometry of the feature. The selected feature's geometry cannot be changed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to change only the features from which the selected feature is built (its base features) and that the selected feature's build operation should not be changed. |
| `BuildOperation` | Specifies a new build operation for the selected feature. The new build operation must be appropriate for the feature's geometry. See the [`AddConstructedFeature`](../../Reference/AddConstructedFeature.md) or [`AddMeasuredFeature`](../../Reference/AddMeasuredFeature.md) table for possible build operations given the feature's geometry.  > **Note:** When modifying a constructed feature, you must pass the list of base features from which to construct the specified feature ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddFeature.md) and [`SubFeatureIndexArrayPtr`](../../Reference/met/MmetAddFeature.md)), even if you only need to modify the build operation. |

Note that the start of a circle's contour is at its rightmost side, or 0°. This is the point where the circle's positive X-axis meets the circle, given a local origin at the circle's center.

Note that the start of a circle's contour is at its rightmost side, or 0°. This is the point where the circle's positive X-axis meets the circle, given a local origin at the circle's center. The start of the contour of an edgel feature is the first edgel subfeature of the edgel feature.
