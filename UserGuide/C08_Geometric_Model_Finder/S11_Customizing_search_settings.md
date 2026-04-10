---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Customizing_search_settings
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Customizing search settings"
---

# Customizing search settings

With successive calls to [`MmodControl`](../../Reference/mod/MmodControl.md), you can customize the global search settings of a Model Finder context, the search settings of each individual model in the context, and the general settings of a Model Finder result buffer; specify which settings to modify using the [`Index`](../../Reference/mod/MmodControl.md) parameter ([`M_CONTEXT`](../../Reference/mod/MmodControl.md), the model's index, or [`M_GENERAL`](../../Reference/mod/MmodControl.md), respectively). You can also customize a search setting for all the models in a context by setting the [`Index`](../../Reference/mod/MmodControl.md) parameter to [`M_ALL`](../../Reference/mod/MmodControl.md). However, when using [`M_ALL`](../../Reference/mod/MmodControl.md), ensure that the specified setting is appropriate for all models. You can inquire about any Model Finder context setting, individual model setting, or Model Finder result buffer setting using [`MmodInquire`](../../Reference/mod/MmodInquire.md).

This section discusses the fundamental model search settings that are usually necessary to adjust (and not previously discussed) and their inter-related global context settings. Other settings are discussed later.

Fundamental model search settings include the acceptance and certainty levels which determine what is considered a match, the expected number of occurrences to find, the reference axis which determines the position returned for a match. Most of the model search settings can affect the speed and robustness of your application. It is recommended that you begin with the default settings, and then, as your application demands, adjust the individual settings as required.

## Acceptance levels

For each model in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, acceptance levels can be set for both the score and target score; whereas for a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, you cannot set the acceptance levels for the target score. The acceptance levels determine the minimum scores required for an occurrence to be considered a match.

Aurora Imaging Library will search the target for the required number of occurrences, returning the occurrences with the best scores greater than or equal to the acceptance levels. If the score or target score of an occurrence is less than the acceptance levels, it is not considered a match and no result will be returned.

You can set the acceptance level for the score of a specified model, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ACCEPTANCE`](../../Reference/mod/MmodControl.md). For example, a setting of 100% means that you will only accept occurrences that contain every active edge in the model with a perfect fit. That is, for every active edge in the model, an equivalent edge must be found in the occurrence with a perfect fit. However, perfect matches are generally unobtainable in real images because of noise and distortion introduced when grabbing images. You should use a reasonable acceptance level that is high enough to avoid false matches, but not so high that occurrences are missed. If your images have considerable noise and/or distortion, or if occlusion of occurrences is expected, you might have to set the level below the default value of 60%.

You can set the target score required using [`M_ACCEPTANCE_TARGET`](../../Reference/mod/MmodControl.md). A setting of 100% for the target score means that you will not tolerate any extra edges, and common edges will have a perfect fit. A setting of 0% (default) allows for any number of extra edges. Essentially, the target score acceptance level allows you to control how tolerant your search is to details not present in the original model.

Note that the contribution of the fit can be controlled using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_FIT_ERROR_WEIGHTING_FACTOR`](../../Reference/mod/MmodControl.md), and that the default value for the weighting factor is 25.0%. For more information, see [Fit error weighting factor](S13_Advanced_search_settings.md).

Note that the [`M_ACCEPTANCE_TARGET`](../../Reference/mod/MmodControl.md) control type is not supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

## Certainty levels

The certainty levels are used to speed up the search when very good matches are expected often. Any occurrence that is greater than or equal to the certainty levels is considered a certain match.

The certainty levels determine the score and target score above (or equal to) which the algorithm can assume that it has found a certain match. Both of these scores must be greater than or equal to their respective certainty levels for an occurrence to be considered a certain match. If the required number of occurrences has been found above (or equal to) these certainty levels, Aurora Imaging Library stops searching the target for better ones, avoiding exhaustive checking of all possible candidates. If not, Aurora Imaging Library will continue to search for the required number of matches greater than or equal to the acceptance level and with the best score.

You can set the certainty level for the score of a specified model using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_CERTAINTY`](../../Reference/mod/MmodControl.md) (the default setting is 90%). You can set the certainty level for the target score of a specified model using [`M_CERTAINTY_TARGET`](../../Reference/mod/MmodControl.md) (the default value is 0%). Note that the [`M_CERTAINTY_TARGET`](../../Reference/mod/MmodControl.md) control type is not supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

If necessary, when searching for a fixed number of occurrences in a target, you can ensure that Aurora Imaging Library always returns those occurrences with the highest match score among those found, by setting the certainty levels of each individual model to 100%. Aurora Imaging Library will return occurrences with the best score(s) greater than or equal to the acceptance or those with scores of 100%. Note that doing so will probably increase the search time.

## Expected number of occurrences

You can set the expected number of occurrences for the Model Finder context, and for each individual model in the Model Finder context.

For each individual model in the context, you can set the expected number of occurrences of the model in the target. To do so, use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_NUMBER`](../../Reference/mod/MmodControl.md), specifying the index of the particular model. The default value is 1. To find all occurrences of a model, set [`M_NUMBER`](../../Reference/mod/MmodControl.md) to [`M_ALL`](../../Reference/mod/MmodControl.md).

The number set for the context specifies the maximum total of all model occurrences (for all models within the context together). To set the number for a Model Finder context, use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_NUMBER`](../../Reference/mod/MmodControl.md), specifying [`M_CONTEXT`](../../Reference/mod/MmodControl.md) as the index. The default value is [`M_ALL`](../../Reference/mod/MmodControl.md), which finds the expected number of occurrences specified for each model.

Note that setting the number of occurrences for the context to [`M_ALL`](../../Reference/mod/MmodControl.md) and the number of occurrences for one of the models to [`M_ALL`](../../Reference/mod/MmodControl.md) can significantly slow the [`MmodFind`](../../Reference/mod/MmodFind.md) operation, depending on the complexity of your model and the target (operation speed will only be slightly affected for simple models on a uniform background). It is recommended that you specify the exact number of expected occurrences whenever possible.

Aurora Imaging Library searches for all models within a context in parallel, therefore the model index does not have any bearing in terms of a search order. Once the number of occurrences for the context has been found, with scores greater than or equal to the certainty levels, the search will stop.

For example, in an application involving a number of different objects being examined one at a time on a conveyor belt, the actual object present in the target at any time is unknown. Since only one occurrence is expected at any one time, the Model Finder context's number would be set to one, as would the number for all the models within the context.

*[Image: M_NUMBER.png]*

Aurora Imaging Library will search the target for all three models; the first occurrence of any of these models, found above (or equal to) the certainty level, will stop the search.

To further illustrate the relationship between the [`M_NUMBER`](../../Reference/mod/MmodControl.md) setting for the context and that of the individual models, the tables below show the maximum possible results which can be returned for different context [`M_NUMBER`](../../Reference/mod/MmodControl.md) settings.

| Model Finder Context Element | Setting for [`M_NUMBER`](../../Reference/mod/MmodControl.md) | Maximum possible results |
| --- | --- | --- |
| Context | [`M_ALL`](../../Reference/mod/MmodControl.md) | All |
| Model 0 | 1 | 1 |
| Model 1 | [`M_ALL`](../../Reference/mod/MmodControl.md) | All |
| Model 2 | 2 | 2 |

The following table shows that if certain [`M_NUMBER`](../../Reference/mod/MmodControl.md) settings are specified, different combinations of maximum number of results are possible.

| Model Finder Context Element | Setting for [`M_NUMBER`](../../Reference/mod/MmodControl.md) | Ex. 1 | Ex. 2 | Ex. 3 | Ex. 4 | Ex. 5 | Ex. 6 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Context | 6 | 6 | 6 | 6 | 6 | 6 | 6 |
| Model 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 |
| Model 1 | [`M_ALL`](../../Reference/mod/MmodControl.md) | 6 | 5 | 5 | 4 | 4 | 3 |
| Model 2 | 2 | 0 | 0 | 1 | 1 | 2 | 2 |

## Reference axis

When an occurrence of a model is found, the model's reference axis determines the coordinates and angle returned as the actual occurrence's found position and angle. Position results return the coordinates of the model's reference axis origin transformed at the model occurrence. Angle results are also returned relative to the reference axis, rather than the model source image axis.

### Reference axis origin

By default, the reference axis origin, for an image-type or result-type model, is at the center of the model box. For a synthetic model, the reference axis origin is, by default, at the model origin. For models defined with an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the reference axis origin is, by default, at the center of the conic shape (whether that be a circle or an ellipse).

If the default reference axis is not at a practical position for your application, you can change the position of the reference axis origin using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_REFERENCE_X`](../../Reference/mod/MmodControl.md) and [`M_REFERENCE_Y`](../../Reference/mod/MmodControl.md). Specify coordinates relative to the model origin; position results are returned relative to the origin of the target.

Modifying the reference axis origin can be useful if you cannot define a unique model for an object of interest. In this case, define a model that is unique and at a fixed location relative to your object of interest and move your reference axis.

In the diagram below, the easily found chip is used as the model, and the model's reference axis origin is shifted to the location of the diode.

*[Image: ReferencePosition.png]*

The reference axis origin does not have to reside within the model, meaning that you can place the reference axis origin outside of the model boundaries.

Note that the found position of an occurrence respects differences in angle and scale, meaning that the found position is relative to the angle and scale of the occurrence.

*[Image: RefPosScale.png]*

When changing the reference axis origin, make sure to update the position range if necessary, particularly when searching through a range of angles and/or scales. The position range limits the region in which the position of a model occurrence can be found; position coordinates which fall outside this region cannot be returned as results (see [Search position and position range](S10_Position_angle_scale.md)). In the diode example presented above, the position range must include, and can be limited to, a small area including the diode for the chip to be found.

*[Image: LimitedSearchRegion.png]*

### Reference axis angle

Often, when allocating models, the angle of the object in your model source image is not aligned with the horizontal image axis.

*[Image: ReferenceAngle.png]*

When occurrences in your target are successfully located, the angle of the occurrence returned is with respect to the model's reference axis. The actual angle of your object of interest in the model is not taken into consideration. You can align the reference axis with the object of interest using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodControl.md). Angle results will then reflect the actual angle of your object of interest.

In the example above, setting the reference axis angle to 4.0 degrees will ensure that the angle returned for the occurrence will represent the actual angle of the object.

### Forward and reverse transformation coefficients

Forward and reverse transformation coefficients are available (as results) for mapping additional points of interest other than the reference axis origin. These coefficients allow you to convert coordinates in the model coordinate system to the corresponding coordinates in the target coordinate system for that occurrence (or vice versa). These coefficients handle variations in scale, translation, and angle, allowing for easier mapping of critical points between model and occurrence.

*[Image: ModCoeff.png]*

Use the following equations:

`_x<sub>d</sub> _ = A_x<sub>s</sub> _ + B_y<sub>s</sub> _ + C`

`_y<sub>d</sub> _ = E_x<sub>s</sub> _ + F_y<sub>s</sub> _ + D`

where:

- _A, B, C, D, E, F_: Transformation coefficients (forward or reverse).
- Note that `_E_ = _-B_` and `_F_ = _A_` for models that do not have an [`M_ASPECT_RATIO`](../../Reference/mod/MmodGetResult.md) result type.
- `x<sub>s</sub> and y<sub>s</sub>`: Source coordinates (with respect to the origin of the model coordinate system for a forward transformation or target coordinate system for a reverse transformation).
- `x<sub>d</sub> and y<sub>d</sub>`: Destination coordinates (with respect to the origin of the target coordinate system for a forward transformation or model coordinate system for a reverse transformation).

If the model is calibrated, these coefficients are given for the real world. For more information, see [Using transformation coefficients with calibrated images](S17_Calibration.md).
