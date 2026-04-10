---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Edge_Finder
section: Edge_features
module_tag: edge
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / edge-finder / Edge features"
---

# Edge features

Edge Finder allows you to calculate many edge features that can provide useful information, such as the edge's length and center of gravity. To enable ([`M_ENABLE`](../../Reference/edge/MedgeControl.md)) or disable ([`M_DISABLE`](../../Reference/edge/MedgeControl.md)) the calculation of a specific edge feature, use [`MedgeControl`](../../Reference/edge/MedgeControl.md); you can also select a group of features for calculation in a single call. To know the calculation state of a feature, use [`MedgeInquire`](../../Reference/edge/MedgeInquire.md). To retrieve the results of the feature calculation, use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md). For more information on retrieving results, see [Retrieving the results](S07_Calculating_and_retrieving_results.md).

Note that the label feature ([`M_LABEL_VALUE`](../../Reference/edge/MedgeControl.md)) is the only edge feature that is enabled by default. The label value is a positive integer greater or equal to one; it is assigned to each edge in an image for identification. Each edge in an image has a unique label value. The label allows you to track or choose particular edges for drawing, edge manipulation, or result retrieval purposes. Typically, you should not disable the edge's label value.

Edge features generally fall into one of the following groups:

- Dimension features (typically very fast to compute).
- Location features (typically fast to compute).
- Advanced features (can take some time to compute, due to their complexity).

To compute features as efficiently as possible, you should calculate a few features first (preferably, the fastest), and eliminate as many unnecessary edges as possible. Then, post-calculate expensive features on the remaining edges. For more information on how to select and post-calculate edges, see [Calculating and retrieving results](S07_Calculating_and_retrieving_results.md).

Note that, occasionally, enabling a feature for calculation will result in another feature being calculated and available for result retrieval. For example, enabling [`M_MOMENT_ELONGATION`](../../Reference/edge/MedgeControl.md) for calculation in [`MedgeControl`](../../Reference/edge/MedgeControl.md) will allow you to retrieve the result of [`M_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeGetResult.md) in [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md), even if you have disabled the calculation of the center of gravity.

The following subsections list all the Edge Finder features that you can enable or disable for calculation using [`MedgeControl`](../../Reference/edge/MedgeControl.md), according to dimension, location, and advanced application. There is also a subsection on selecting a group of features for calculation in a single call. Note that only the more complicated features are explained in some detail. For a description of each feature, see [`MedgeControl`](../../Reference/edge/MedgeControl.md) in the _Aurora Imaging Library Reference_.

## Dimension features

The Edge Finder dimension features are:

|   |   |   |
| --- | --- | --- |
| [`M_CLOSURE`](../../Reference/edge/MedgeControl.md) | [`M_ELLIPSE_FIT_MAJOR_AXIS`](../../Reference/edge/MedgeControl.md) | [`M_FERET_X`](../../Reference/edge/MedgeControl.md) |
| [`M_CIRCLE_FIT_CENTER_X`](../../Reference/edge/MedgeControl.md) | [`M_ELLIPSE_FIT_MINOR_AXIS`](../../Reference/edge/MedgeControl.md) | [`M_FERET_Y`](../../Reference/edge/MedgeControl.md) |
| [`M_CIRCLE_FIT_CENTER_Y`](../../Reference/edge/MedgeControl.md) | [`M_FAST_LENGTH`](../../Reference/edge/MedgeControl.md) | [`M_FERET_GENERAL`](../../Reference/edge/MedgeControl.md) |
| [`M_CIRCLE_FIT_COVERAGE`](../../Reference/edge/MedgeControl.md) | [`M_FERET_BOX`](../../Reference/edge/MedgeControl.md) | [`M_FERET_GENERAL_ANGLE`](../../Reference/edge/MedgeControl.md) |
| [`M_CIRCLE_FIT_ERROR`](../../Reference/edge/MedgeControl.md) | [`M_FERET_ELONGATION`](../../Reference/edge/MedgeControl.md) | [`M_LENGTH`](../../Reference/edge/MedgeControl.md) |
| [`M_CIRCLE_FIT_RADIUS`](../../Reference/edge/MedgeControl.md) | [`M_FERET_MAX_ANGLE`](../../Reference/edge/MedgeControl.md) | [`M_LINE_FIT_A`](../../Reference/edge/MedgeControl.md) |
| [`M_ELLIPSE_FIT_ANGLE`](../../Reference/edge/MedgeControl.md) | [`M_FERET_MAX_DIAMETER`](../../Reference/edge/MedgeControl.md) | [`M_LINE_FIT_B`](../../Reference/edge/MedgeControl.md) |
| [`M_ELLIPSE_FIT_CENTER_X`](../../Reference/edge/MedgeControl.md) | [`M_FERET_MEAN_DIAMETER`](../../Reference/edge/MedgeControl.md) | [`M_LINE_FIT_C`](../../Reference/edge/MedgeControl.md) |
| [`M_ELLIPSE_FIT_CENTER_Y`](../../Reference/edge/MedgeControl.md) | [`M_FERET_MIN_ANGLE`](../../Reference/edge/MedgeControl.md) | [`M_LINE_FIT_ERROR`](../../Reference/edge/MedgeControl.md) |
| [`M_ELLIPSE_FIT_COVERAGE`](../../Reference/edge/MedgeControl.md) | [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md) | [`M_SIZE`](../../Reference/edge/MedgeControl.md) |
| [`M_ELLIPSE_FIT_ERROR`](../../Reference/edge/MedgeControl.md) |

### Feret diameter

A Feret diameter of an edge is its diameter at a specific angle. Several Feret diameters are illustrated in the diagram below. Note that the angle at which the Feret diameter is taken (relative to the horizontal axis) is specified as a subscript to F.

*[Image: FeretDescription.png]*

Some features, such as the edge's maximum Feret diameter, are determined by testing the Feret diameter of the edge at several angles. You can specify the angular range to use to calculate the Feret features, using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FERET_ANGLE_SEARCH_END`](../../Reference/edge/MedgeControl.md) and [`M_FERET_ANGLE_SEARCH_START`](../../Reference/edge/MedgeControl.md). The search is done in the counter-clockwise direction. To set the number of angles to test within the specified angular range, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md). The difference between successive angles is `([`M_FERET_ANGLE_SEARCH_END`](../../Reference/edge/MedgeControl.md) - [`M_FERET_ANGLE_SEARCH_START`](../../Reference/edge/MedgeControl.md)) / [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md)`. The default number of angles tested is 8, which is typically sufficient. Note that increasing the number of Feret angles increases the accuracy of the results; however, it also increases the processing time.

### Circle fit and line fit

The circle fit is calculated as the circle that best fits the edge, while the line fit is calculated as the line that best fits the edge.

*[Image: CircleAndLineFit.png]*

To calculate the coordinates of the center of the circle that is the best fit for each edge, use [`M_CIRCLE_FIT_CENTER_X`](../../Reference/edge/MedgeControl.md) and [`M_CIRCLE_FIT_CENTER_Y`](../../Reference/edge/MedgeControl.md). To calculate the radius of the circle that is the best fit for each edge, use [`M_CIRCLE_FIT_RADIUS`](../../Reference/edge/MedgeControl.md).

To calculate the coefficients _A_, _B_, and _C_ that define the line that is the best fit for each edge, use [`M_LINE_FIT_A`](../../Reference/edge/MedgeControl.md), [`M_LINE_FIT_B`](../../Reference/edge/MedgeControl.md), and [`M_LINE_FIT_C`](../../Reference/edge/MedgeControl.md). Note that these coefficients are based on the linear equation, `_A_ _x_ + _B_ _y_ + _C_ = 0`.

To calculate the coverage of the circle fit, use [`M_CIRCLE_FIT_COVERAGE`](../../Reference/edge/MedgeControl.md). The coverage indicates what angular fraction of the fitted circle is subtended by the radii going to the endpoints of the edge. The value returned is between 0.0 and 1.0, inclusive. For example, if the edge is closed, the coverage is 1.0 (100% coverage), for a quarter circle the coverage is 0.25 (25% coverage).

*[Image: CircleFitCoverage.png]*

Note that since a line is theoretically infinite, it is impractical to calculate its coverage.

You can also calculate the error of either the circle fit or line fit using [`M_CIRCLE_FIT_ERROR`](../../Reference/edge/MedgeControl.md) or [`M_LINE_FIT_ERROR`](../../Reference/edge/MedgeControl.md), respectively. These values are calculated as the average quadratic error of the fit.

### Ellipse fit

The ellipse fit is calculated as the ellipse that best fits the edge.

*[Image: EllipseFit.png]*

To calculate the coordinates of the center of the ellipse that is the best fit for each edge, use [`M_ELLIPSE_FIT_CENTER_X`](../../Reference/edge/MedgeControl.md) and [`M_ELLIPSE_FIT_CENTER_Y`](../../Reference/edge/MedgeControl.md). To calculate the major and minor axes of the ellipse that is the best fit for each edge, use [`M_ELLIPSE_FIT_MAJOR_AXIS`](../../Reference/edge/MedgeControl.md) and [`M_ELLIPSE_FIT_MINOR_AXIS`](../../Reference/edge/MedgeControl.md), respectively.

To calculate the angle of the ellipse fit, use [`M_ELLIPSE_FIT_ANGLE`](../../Reference/edge/MedgeControl.md). To calculate the error of the ellipse fit, use [`M_ELLIPSE_FIT_ERROR`](../../Reference/edge/MedgeControl.md). This value is calculated as the average quadratic error of the fit.

## Location features

The Edge Finder location features are:

|   |   |   |
| --- | --- | --- |
| [`M_BOX`](../../Reference/edge/MedgeControl.md) | [`M_CENTER_OF_GRAVITY_X`](../../Reference/edge/MedgeControl.md) | [`M_POSITION_X`](../../Reference/edge/MedgeControl.md) |
| [`M_BOX_X_MAX`](../../Reference/edge/MedgeControl.md) | [`M_CENTER_OF_GRAVITY_Y`](../../Reference/edge/MedgeControl.md) | [`M_POSITION_Y`](../../Reference/edge/MedgeControl.md) |
| [`M_BOX_X_MIN`](../../Reference/edge/MedgeControl.md) | [`M_CONTACT_POINTS`](../../Reference/edge/MedgeControl.md) | [`M_X_MAX_AT_Y_MAX`](../../Reference/edge/MedgeControl.md) |
| [`M_BOX_Y_MAX`](../../Reference/edge/MedgeControl.md) | [`M_FIRST_POINT`](../../Reference/edge/MedgeControl.md) | [`M_Y_MAX_AT_X_MIN`](../../Reference/edge/MedgeControl.md) |
| [`M_BOX_Y_MIN`](../../Reference/edge/MedgeControl.md) | [`M_FIRST_POINT_X`](../../Reference/edge/MedgeControl.md) | [`M_X_MIN_AT_Y_MIN`](../../Reference/edge/MedgeControl.md) |
| [`M_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeControl.md) | [`M_FIRST_POINT_Y`](../../Reference/edge/MedgeControl.md) | [`M_Y_MIN_AT_X_MAX`](../../Reference/edge/MedgeControl.md) |

The following edge map illustrates where each edge location feature is located:

*[Image: CogDescription.png]*

## Advanced features

The Edge Finder advanced features are:

|   |   |   |
| --- | --- | --- |
| [`M_AVERAGE_STRENGTH`](../../Reference/edge/MedgeControl.md) | [`M_MOMENT_ELONGATION`](../../Reference/edge/MedgeControl.md) | [`M_STRENGTH`](../../Reference/edge/MedgeControl.md) |
| [`M_CONVEX_PERIMETER`](../../Reference/edge/MedgeControl.md) | [`M_MOMENT_ELONGATION_ANGLE`](../../Reference/edge/MedgeControl.md) | [`M_TORTUOSITY`](../../Reference/edge/MedgeControl.md) |

### Convex perimeter

If you enable the convex perimeter feature ([`M_CONVEX_PERIMETER`](../../Reference/edge/MedgeControl.md)), an approximation of the perimeter of each edge's convex hull will be calculated. Abstractly, an edge's convex perimeter is very much like taking a rubber band, and placing it tautly around the edge.

*[Image: ConvexPerimeterSample.png]*

The convex perimeter is derived by taking the diameter of the edge at different angles. The greater the number of Ferets used to calculate the diameter, the more accurate the approximation. Use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md) to adjust the number of Ferets. Note that increasing the number of Ferets increases the accuracy of the results; however, it also increases the processing time.

### Tortuosity

An edge's tortuosity ([`M_TORTUOSITY`](../../Reference/edge/MedgeControl.md)) is the diagonal length of the edge's bounding box ([`M_BOX`](../../Reference/edge/MedgeControl.md)), divided by the length of the edge ([`M_LENGTH`](../../Reference/edge/MedgeControl.md)). Therefore, a non-tortuous edge (a straight line) will have a tortuosity of 1, while a tortuous edge (a curvy line) will have its tortuosity decreasing towards 0. A tortuosity that is close to 1 is considered low while a tortuosity that is close to 0 is considered high.

*[Image: TortuositySample.png]*

### Moment elongation

The moment elongation ([`M_MOMENT_ELONGATION`](../../Reference/edge/MedgeControl.md)) is a geometric elongation measure of the edge. It is defined as the ratio of the principal values of the edge's inertial matrix, which corresponds to the principal directions of the edge shape. This can be approximately defined as the ratio between the edge's minimum and maximum moment. The elongation value varies from 0.0 to 1.0. Values closer to 1.0 are considered to have a low elongation, while values closer to 0.0 are considered to have a high elongation.

*[Image: ElongationSample.png]*

Note that you can also calculate the angle of the principal axis along each edge's moment elongation, using [`M_MOMENT_ELONGATION_ANGLE`](../../Reference/edge/MedgeControl.md).

## Grouped features

The Edge Finder features that allow you to select a group of features for calculation in a single call are:

|   |   |
| --- | --- |
| [`M_ALL_FEATURES`](../../Reference/edge/MedgeControl.md) | [`M_ELLIPSE_FIT`](../../Reference/edge/MedgeControl.md) |
| [`M_BOX`](../../Reference/edge/MedgeControl.md) | [`M_FERET_BOX`](../../Reference/edge/MedgeControl.md) |
| [`M_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeControl.md) | [`M_FIRST_POINT`](../../Reference/edge/MedgeControl.md) |
| [`M_CIRCLE_FIT`](../../Reference/edge/MedgeControl.md) | [`M_LINE_FIT`](../../Reference/edge/MedgeControl.md) |
| [`M_CONTACT_POINTS`](../../Reference/edge/MedgeControl.md) | [`M_POSITION`](../../Reference/edge/MedgeControl.md) |

Each of these features has multiple features associated with it. Therefore, if you enable one of them for calculation, then all the features associated with it will also be enabled. For example, enabling [`M_CIRCLE_FIT`](../../Reference/edge/MedgeControl.md) will result in all the circle fit values of each edge to be calculated. This is equivalent to individually enabling each of the following: [`M_CIRCLE_FIT_CENTER_X`](../../Reference/edge/MedgeControl.md), [`M_CIRCLE_FIT_CENTER_Y`](../../Reference/edge/MedgeControl.md), [`M_CIRCLE_FIT_RADIUS`](../../Reference/edge/MedgeControl.md), [`M_CIRCLE_FIT_ERROR`](../../Reference/edge/MedgeControl.md), and [`M_CIRCLE_FIT_COVERAGE`](../../Reference/edge/MedgeControl.md).

The features that belong to each group is typically self evident. For a description of each, see [`MedgeControl`](../../Reference/edge/MedgeControl.md).
