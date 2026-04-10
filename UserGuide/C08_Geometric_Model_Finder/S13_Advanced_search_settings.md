---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Advanced_search_settings
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Advanced search settings"
---

# Advanced search settings

In addition to the fundamental search settings, the Model Finder module also provides advanced settings that allow you to specify whether the polarity of the edges in a model differs from those in occurrences, how much or little separation between occurrences is permitted, as well as other information.

## Polarity

The polarity of an edge indicates whether edges occur as transitions from light to dark or vice versa. When the polarity of edges in the target might be different from those of the model (for example, due to shadows), you should specify the expected polarity using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_POLARITY`](../../Reference/mod/MmodControl.md). This control type allows you to specify whether edges present in the model and in the occurrence have the same, the reverse, either of these, or a mixture of both polarities.

*[Image: Polarities.png]*

The default polarity setting for a model in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context is [`M_SAME`](../../Reference/mod/MmodControl.md). For a model in an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md), [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md), or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the default polarity setting is [`M_SAME_OR_REVERSE`](../../Reference/mod/MmodControl.md). Note that for a model in an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, [`M_POLARITY`](../../Reference/mod/MmodControl.md) is not supported.

The polarity setting can be useful when dealing with adverse lighting conditions where dark shadows can cause edges to vary in polarity.

*[Image: ModPolarity.png]*

## Separation

You can specify the minimum amount of separation from other occurrences (of the same model) for an occurrence to be considered distinct (a match). In essence, this determines the amount of overlap by different model occurrences that is possible.

You can set the minimum separation for five criteria, which are: the X-position, Y-position, angle, scale, and the aspect ratio. These can be set for each individual model in your Model Finder context. For an occurrence to be considered distinct from another, only one of the minimum separation conditions needs to be met. For example, if the minimum separation in terms of angle is met, then the occurrence is considered distinct, regardless of the separation in position or scale. However, each of these separation criteria can be disabled ([`M_DISABLE`](../../Reference/mod/MmodControl.md)) so that it is not considered when determining a valid occurrence.

`DISTINCT OCCURRENCE = (Separation in X) OR (Separation in Y) OR (Separation in Angle) OR (Separation in Scale) OR (Separation in Aspect Ratio)`

The minimum positional separation ([`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_MIN_SEPARATION_X`](../../Reference/mod/MmodControl.md) and [`M_MIN_SEPARATION_Y`](../../Reference/mod/MmodControl.md)) determines the minimum distance between the found positions of two occurrences of the same model. This separation is specified as a percentage of the model size at the nominal scale ([`M_SCALE`](../../Reference/mod/MmodControl.md)). The minimum value that you can set for [`M_MIN_SEPARATION_X`](../../Reference/mod/MmodControl.md) and [`M_MIN_SEPARATION_Y`](../../Reference/mod/MmodControl.md) is 0, which means that there is no minimum distance needed for occurrences to be distinct; all occurrences will be considered distinct regardless of other separation conditions. You can set very large values for [`M_MIN_SEPARATION_X`](../../Reference/mod/MmodControl.md) and [`M_MIN_SEPARATION_Y`](../../Reference/mod/MmodControl.md); if they are large enough, for example, if the values are larger than the size of the image, it is equivalent to setting this separation condition to [`M_DISABLE`](../../Reference/mod/MmodControl.md) since this condition will never be met. In this case, whether an occurrence is distinct or not will depend on the other separation conditions.

The minimum angular separation ([`M_MIN_SEPARATION_ANGLE`](../../Reference/mod/MmodControl.md)) determines the minimum difference in angle between occurrences. This value is specified as an absolute angle value. The default value is `10.0<sup>o</sup>`. The minimum value that you can set for [`M_MIN_SEPARATION_ANGLE`](../../Reference/mod/MmodControl.md) is 0, which means that there is no minimum difference in angle needed for occurrences to be distinct; all occurrences will be considered distinct regardless of other separation conditions. The maximum absolute angle difference is 180 degrees. For a diagram of the delta convention used in Aurora Imaging Library, see [Angle and angular range](S10_Position_angle_scale.md). If the angle is larger than 180, an error will be returned. At 180 degrees, it is equivalent to setting this separation condition to [`M_DISABLE`](../../Reference/mod/MmodControl.md). In this case, whether an occurrence is distinct or not will depend on the other separation conditions. It is to be noted that [`M_MIN_SEPARATION_ANGLE`](../../Reference/mod/MmodControl.md) is not supported for models defined with an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

The minimum scale separation ([`M_MIN_SEPARATION_SCALE`](../../Reference/mod/MmodControl.md)) determines the minimum difference in scale between occurrences, as a scale factor. The default value is 1.1. The minimum value that you can set for [`M_MIN_SEPARATION_SCALE`](../../Reference/mod/MmodControl.md) is 1, which means that there is no minimum difference in scale needed for occurrences to be distinct; all occurrences will be considered distinct regardless of other separation conditions. The maximum value that you can set for [`M_MIN_SEPARATION_SCALE`](../../Reference/mod/MmodControl.md) is 4, which is equivalent to setting this separation condition to [`M_DISABLE`](../../Reference/mod/MmodControl.md) for an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or an [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. In this case, whether an occurrence is distinct or not will depend on the other separation conditions.

The minimum aspect ratio separation ([`M_MIN_SEPARATION_ASPECT_RATIO`](../../Reference/mod/MmodControl.md)) determines the minimum difference in aspect ratio between occurrences. The default value is 1.1. The minimum value that you can set for [`M_MIN_SEPARATION_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) is 1, which means that there is no minimum difference in the aspect ratio needed for occurrences to be distinct; all occurrences will be considered distinct regardless of other separation conditions. The maximum you can set for [`M_MIN_SEPARATION_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) is 4. It is to be noted that [`M_MIN_SEPARATION_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) is only supported for a model defined in an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) or [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

The five criteria are summarized below in equation form.

*[Image: SeparationEquations.png]*

The following example illustrates the five separation criteria; an arrow represents two occurrences of the same model and the various types of separation possible.

*[Image: Separation.png]*

It should be noted that for a model to be found, the number of visible edges in the occurrence must be sufficient to provide a match according to your acceptance levels.

In addition, to find occurrences of a model in an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, at least a portion of each side of the rectangle must be visible; that is, the model coverage on any side cannot be 0. You can control the minimum coverage on each side using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_MIN_SIDE_COVERAGE`](../../Reference/mod/MmodControl.md).

## Shared edges

You can choose to allow occurrences to share edges, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SHARED_EDGES`](../../Reference/mod/MmodControl.md) set to [`M_ENABLE`](../../Reference/mod/MmodControl.md). Otherwise, edges that can be part of more than one occurrence are considered part of the occurrence with the greatest score. For example, in the illustration below, two occurrences of two simple models share a common edge. With shared edges enabled, these occurrences would have perfect scores.

*[Image: M_SHARED_EDGES.png]*

However, with shared edges disabled (default), the shared edge would be considered part of occurrence 1, since it has the greater score; the score of occurrence 2 would be subsequently reduced by the loss of the shared edge in the score calculation. For models in an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md), [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md), or [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, [`M_SHARED_EDGES`](../../Reference/mod/MmodControl.md) is not supported.

## Fit error weighting factor

The fit error weighting factor, [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_FIT_ERROR_WEIGHTING_FACTOR`](../../Reference/mod/MmodControl.md), determines the relative importance of the fit error when calculating the match score and the target score. The higher the factor, the greater the influence that the fit error has on the resulting score and target score for an occurrence (see [Determining what is a match](S09_Determining_what_is_a_match.md)). Setting this factor to 0.0 means that the fit error is not considered in the score calculation, so the score is equal to the coverage score. Setting this factor to 100.0 means that the fit error has a maximum contribution in the score. The default value for [`M_FIT_ERROR_WEIGHTING_FACTOR`](../../Reference/mod/MmodControl.md) is 25.0%.
