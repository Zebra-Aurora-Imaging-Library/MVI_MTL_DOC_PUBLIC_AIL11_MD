---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_Geometric_Matcher
section: Finding_model_occurrences
module_tag: agm
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-geometric-matcher / Finding model occurrences"
---

# Finding model occurrences

You can search for instances of a model in an image, or you can search for instances of it in an Edge Finder result buffer, using [`MagmFind`](../../Reference/agm/MagmFind.md). In order for an occurrence to be found, it must be at approximately the same scale as the model. [`MagmFind`](../../Reference/agm/MagmFind.md) will write the results of the search in a find AGM result buffer ([`M_GLOBAL_EDGE_BASED_FIND_RESULT`](../../Reference/agm/MagmAllocResult.md)). Note that calling [`MagmFind`](../../Reference/agm/MagmFind.md) successively with targets of different sizes will slow the execution time.

## Finding model occurrences in an image

To search for instances of a model in an image, the target image must be a 1-band, 8-bit unsigned image. The minimum and maximum target image sizes are 6 x 6 and 32768 x 32768 pixels, respectively. It is important to note that the minimum and maximum target image sizes are Aurora Imaging Library limits. To make sure that you have enough memory, you can call [`MagmFind`](../../Reference/agm/MagmFind.md); if you don't have enough memory, you will get an Aurora Imaging Library error.

Note that because AGM does not support calibration, the target image must be uncalibrated.

## Finding model occurrences in an Edge Finder result buffer

When using an Edge Finder result buffer as the target, AGM searches for edges of the model only in the included edges of the result buffer. Therefore, using the Edge Finder result buffer as a target, instead of an image, allows you to be more specific about the edges in which to search. For example, you could select out unwanted edges in the target, which will speed up the search and further calculations, or create a target that is composed of user-defined edges.

Some things have to be done before you can search for instances of a model in an Edge Finder result buffer:

- The Edge Finder result buffer must have been previously allocated using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md).
- The Edge Finder result buffer must be compatible with the AGM context. Enable this using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_AGM_COMPATIBLE`](../../Reference/edge/MedgeControl.md).
- [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must have already been called using this result buffer.

Note that because AGM does not support calibration, the Edge Finder result buffer must be uncalibrated or [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) must be set to [`M_PIXEL`](../../Reference/edge/MedgeControl.md).

## Determining what is a match

Before customizing your search settings, it is necessary to understand how Aurora Imaging Library determines a match between your model and an occurrence in the target. The detection, fit, overall fit, and coverage scores are the primary factors in determining which occurrences are considered matches with the model in your find AGM context.

When you call [`MagmFind`](../../Reference/agm/MagmFind.md), Aurora Imaging Library searches the target for occurrences and returns all occurrences with scores greater than or equal to the specified acceptance levels. If the detection, fit, overall fit, or coverage score of an occurrence is less than the acceptance levels, it is not considered a match and no result is returned.

### Detection score

The detection score is the probability that the occurrence is a match. 100% indicates that Aurora Imaging Library is certain the found occurrence is a true occurrence. Only occurrences with a detection score above a set acceptance level will be considered a possible match. You can set the minimum detection score using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_ACCEPTANCE_DETECTION`](../../Reference/agm/MagmControl.md). You can retrieve the detection score of the occurrence using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_SCORE_DETECTION`](../../Reference/agm/MagmGetResult.md).

In the example below, if you set [`M_ACCEPTANCE_DETECTION`](../../Reference/agm/MagmControl.md) to 60%, the wrong occurrence detected at 52% will not be returned.

*[Image: AGM_detection_score.png]*

### Fit, overall fit, and coverage scores

If an occurrence's detection score is acceptable, it is a viable candidate and Aurora Imaging Library proceeds to calculate its fit and coverage scores. You can set the mode of the fit operation using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_FIT_MODE`](../../Reference/agm/MagmControl.md). Note that for single-definition models and for composite-definition models with [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md), an overall fit score is also calculated.

The (regular) fit score is a measure of the correlation of the edgels in the model to their corresponding edgels in the occurrence. Model edgels without a corresponding occurrence edgel have no effect on this fit score. A fit score of 100% indicates a perfect fit, which means that model edgels conform perfectly to corresponding edgels found in the occurrence. The overall fit score is a measure of the correlation of all edgels in the model to those of the occurrence, including those without a corresponding edgel in the occurrence. An overall fit score of 100% indicates a perfect overall fit, which means that all model edgels conform perfectly to those found in the occurrence.

The coverage score is the percentage of the total length of the model's edgels found in the occurrence. 100% indicates that for each of the model's edgels, a corresponding edgel was found in the occurrence.

Only occurrences with fit, overall fit, and coverage scores above set acceptance levels will be considered a match. You can set the minimum fit score using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_ACCEPTANCE_FIT`](../../Reference/agm/MagmControl.md), the minimum overall fit score using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_ACCEPTANCE_FIT_OVERALL`](../../Reference/agm/MagmControl.md), and the minimum coverage score with [`M_ACCEPTANCE_COVERAGE`](../../Reference/agm/MagmControl.md). You can retrieve the fit, overall fit, and coverage scores of the occurrence using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_SCORE_FIT`](../../Reference/agm/MagmGetResult.md), [`M_SCORE_FIT_OVERALL`](../../Reference/agm/MagmGetResult.md), and [`M_SCORE_COVERAGE`](../../Reference/agm/MagmGetResult.md). See the [Interpreting results](S05_Finding_model_occurrences.md) subsection below for an example.

The AGM module can detect occurrences with some variation in appearance. An occurrence with a high detection score can have a low fit, overall fit, or coverage score, since these scores do not support variation. Setting the acceptance level for the fit, overall fit, and/or coverage score to a high value might remove occurrences with deformities from the list of possible matches. Lowering these levels will allow for more varied occurrences to be returned.

### Maximum association distance

The maximum association distance determines which model edgels are close enough to the occurrence to be included in the regular fit score calculation and increase the coverage score. Set the maximum association distance using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/agm/MagmControl.md). Aurora Imaging Library measures each model edgel's distance from the nearest occurrence edgel. Model edgels that fall beyond this range do not affect the occurrence's regular fit operation. These edgels are also considered uncovered and decrease the coverage score. Note that all model edgels are included in the overall fit score calculation, therefore the maximum association distance does not directly influence the [`M_SCORE_FIT_OVERALL`](../../Reference/agm/MagmGetResult.md) result type.

*[Image: AGM_max_association.png]*

### Interpreting results

The following image will be used as our sample model.

*[Image: AGM_fit_coverage_model.png]*

The following table illustrates how the fit and coverage work together to provide details about the nature of a candidate. Note that values given are for illustrative purposes only, and as such are only qualitative.

| Target image | Fit score ([`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_SCORE_FIT`](../../Reference/agm/MagmGetResult.md)) | Coverage score ([`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_SCORE_COVERAGE`](../../Reference/agm/MagmGetResult.md)) |
| --- | --- | --- |
| *[Image: AGM_fit_coverage_model.png]* | 100% All the model edgels found in the occurrence are a perfect fit. | 100% All the edgels in the model are found in the occurrence. |
| *[Image: AGM_fit_coverage_example2.png]* | 100% All the model edgels found in the occurrence are a perfect fit. | 70% Some of the edgels in the model are missing from the occurrence. |
| *[Image: AGM_fit_coverage_example3.png]* | 100% All the model edgels found in the occurrence are a perfect fit. | 100% Although there are extra edgels in the occurrence, all the edgels in the model have been found in the occurrence. |
| *[Image: AGM_fit_coverage_example4.png]* | 80% Some of the edgels found in the occurrence do not conform perfectly in shape to those in the model. | 100% All the edgels in the model are found in the occurrence. |

## Search settings

With successive calls to [`MagmControl`](../../Reference/agm/MagmControl.md), you can customize the search settings of the model in the find AGM context. Besides the detection, fit, overall fit, and coverage acceptance levels, fundamental model search settings include the source of the model, the detection angle range, the edgel angle mode, the empty regions mode, the fit mode, and the first and last levels. You can inquire about these settings using [`MagmInquire`](../../Reference/agm/MagmInquire.md).

Most of the model search settings can affect the speed and robustness of your application. For example, increasing the search perseverance ([`M_PERSEVERANCE_DETECTION`](../../Reference/agm/MagmControl.md)) increases the number of candidates that Aurora Imaging Library attempts to match to your model. This will take longer to complete, but will increase the possibility of finding all model occurrences. It is recommended that you begin with the default values and then adjust individual settings as required.

### Model source

Training a composite-definition model produces a model that encompasses the features of the labeled appearances from your training images. Consequently, your trained composite-definition model might have many edgels if there is a lot of variation between the model appearances. When fitting the trained composite-definition model to a found occurrence, or when calculating the trained composite-definition model's coverage, this might result in low fit and coverage scores. To perform the fit and coverage calculations, you can choose to use either the trained composite-definition model or an explicitly-specified image of the model, using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md). Note that to perform the overall fit calculation, you must set [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md).

When your trained composite-definition model has more edgels than a typical appearance, you should use an explicitly-specified image of the model for the fit and coverage calculations. You can view the edges of your trained composite-definition model using [`MagmDraw`](../../Reference/agm/MagmDraw.md) with [`M_DRAW_TRAINED_MODEL`](../../Reference/agm/MagmDraw.md). When [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md), you must specify the model image using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_MODEL_IMAGE`](../../Reference/agm/MagmControl.md); note that in this case, the detection uses the trained composite-definition model and the fit and coverage use the model specified with [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md).

### Detection angle

You can search for a model at a specific angle or within a range of angles. Set the nominal search angle using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md). By default, the search angle is 0°.

*[Image: AngleConvention.png]*

The [`MagmFind`](../../Reference/agm/MagmFind.md) operation can detect occurrences within the full angular range of 360° from the nominal angle specified with [`M_DETECTION_REF_ANGLE`](../../Reference/agm/MagmControl.md). Use the [`MagmControl`](../../Reference/agm/MagmControl.md)   [`M_DETECTION_DELTA_POS_ANGLE`](../../Reference/agm/MagmControl.md) and [`M_DETECTION_DELTA_NEG_ANGLE`](../../Reference/agm/MagmControl.md) control types to specify the detection angle range in the counter-clockwise and clockwise direction from the nominal angle, respectively.

Note that for a composite-definition model, these settings are only used when [`M_DETECTION_DELTA_ANGLE_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md). Otherwise, if [`M_TRAIN_DETECTION_ANGLE`](../../Reference/agm/MagmControl.md) was set to [`M_ENABLE`](../../Reference/agm/MagmControl.md) during training, the detection angle range is 360.0°. If [`M_TRAIN_DETECTION_ANGLE`](../../Reference/agm/MagmControl.md) was set to [`M_DISABLE`](../../Reference/agm/MagmControl.md), the detection angle range is 0.0°.

*[Image: DeltaConvention.png]*

Note that the detection angle range only limits the angle of detection. The angle at which an occurrence is detected might not be exact; its true angle can typically be within 10° of the angle of detection, even if that falls outside of the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is disabled, the returned angle of an occurrence will always be the angle at which it was detected and will therefore always be within the detection angle range. If [`M_FIT_MODE`](../../Reference/agm/MagmControl.md) is enabled, a fit operation is performed starting from the angle of detection. In refining the angle so that it is more accurate, the angle of the occurrence's reference axis returned by [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) can end up being outside of the detection angle range.

### Edgel angle

The edgel angle mode determines whether the orientation of edgels affects the detection score. Set the edgel angle mode for a single-definition model using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_EDGEL_ANGLE_MODE`](../../Reference/agm/MagmControl.md). Edgel orientation is defined by the gradient angle (the orientation of the edge's most sudden change in brightness). When you enable [`M_EDGEL_ANGLE_MODE`](../../Reference/agm/MagmControl.md), the find operation calculates the difference between target and model edgel orientations. The greater the difference, the more the detection score will decrease. Note that edgel orientation is indifferent to whether the brightness changes from dark to light or the reverse. Also note that this control is only available for single-definition models.

### Empty regions

Empty regions are featureless regions that do not contain edges. If your model has a consistent region without edges (for example, a disk with a hole in the center), these regions would typically be featureless in the occurrence. You can consider the empty regions of your model when searching for occurrences by enabling empty regions using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_EMPTY_REGIONS_MODE`](../../Reference/agm/MagmControl.md). This ensures that the detection score decreases when edges are found in the corresponding regions of the occurrence. You can also set the empty regions mode when training a composite-definition model. A find operation that uses a composite-definition model will consider empty regions when empty regions were considered during training. If model appearances in your target rest on a non-featureless background, you should disable [`M_EMPTY_REGIONS_MODE`](../../Reference/agm/MagmControl.md). In this case, the detection score will not decrease when edges are found in the corresponding regions.

### Fit mode

A fit operation can be performed to provide better positional accuracy than the position found at detection. You can set whether and how the fit operation is performed, using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_FIT_MODE`](../../Reference/agm/MagmControl.md). The default setting is [`M_PIXEL_PRECISION`](../../Reference/agm/MagmControl.md). When the fit operation is disabled, Aurora Imaging Library uses the features of the model and target to find occurrences but it doesn't do any fitting, resulting in a faster search with a lower accuracy. Positional results are most accurate when the fit operation uses subpixel precision, but this can reduce the speed of the search.

### First and last levels

Model searches use a hierarchical method to reduce the time needed to complete the search. Using this method, AGM produces a series of smaller, lower-resolution versions of both the target and the model, and the search begins on a much-reduced scale. This series of subsampled images is sometimes called a resolution pyramid because of the shape formed when the different resolution levels are stacked on top of each other.

*[Image: Hierarchy.png]*

By default, the resolution levels for the initial and final stages of the search are determined automatically ([`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_AUTO_CONTENT_BASED`](../../Reference/agm/MagmControl.md) by default). This usually provides the best speed and results for the search operation, and should be left in most circumstances. Note that this mode can slow down the [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md) operation.

If required, you can explicitly set the resolution levels for the initial and final stages of the search, using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_FIRST_LEVEL`](../../Reference/agm/MagmControl.md) and [`M_LAST_LEVEL`](../../Reference/agm/MagmControl.md). In this case, you must set [`M_PYRAMID_LEVELS_MODE`](../../Reference/agm/MagmControl.md) to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md). You can set the first and last levels to any integer value from 0 to 7. Level 0 is the original target size and each higher level is half the size (and resolution) of the previous one. The higher the level, the faster the initial search; however, the search will be less reliable at higher levels.

Note that very small models and search regions cannot be subsampled as many times as large ones. If you specify too high a level, an error will occur. For example, when calling [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md), if the subsampled model is too small or does not have enough edgels, an error will occur.

You can inquire the first and last levels used for the search, using [`MagmInquire`](../../Reference/agm/MagmInquire.md) with [`M_FIRST_LEVEL_USED`](../../Reference/agm/MagmInquire.md) and [`M_LAST_LEVEL_USED`](../../Reference/agm/MagmInquire.md).
