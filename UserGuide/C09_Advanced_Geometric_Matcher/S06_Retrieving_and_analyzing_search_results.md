---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_Geometric_Matcher
section: Retrieving_and_analyzing_search_results
module_tag: agm
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-geometric-matcher / Retrieving and analyzing search results"
---

# Retrieving and analyzing search results

After successfully locating your model occurrences in your target using [`MagmFind`](../../Reference/agm/MagmFind.md), you can extract the required results from your find AGM result buffer using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md). For more information on locating model occurrences, see[Finding model occurrences](S05_Finding_model_occurrences.md).

## Possible results

Generally, you should first retrieve the total number of occurrences of the model (using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_NUMBER`](../../Reference/agm/MagmGetResult.md)), to ascertain the size of the result array needed. You can then retrieve several types of results with the AGM module. These results provide information on the nature of the occurrence(s) found. In addition to the detection, fit, overall fit, and coverage scores discussed previously, you can retrieve results for:

- Number of occurrences ([`M_NUMBER`](../../Reference/agm/MagmGetResult.md)).
- Angle ([`M_ANGLE`](../../Reference/agm/MagmGetResult.md)).
- Polarity ([`M_POLARITY`](../../Reference/agm/MagmGetResult.md)).
- Position X ([`M_POSITION_X`](../../Reference/agm/MagmGetResult.md)) and position Y ([`M_POSITION_Y`](../../Reference/agm/MagmGetResult.md)).
- Scale ([`M_SCALE`](../../Reference/agm/MagmGetResult.md)).
- Status of the [`MagmFind`](../../Reference/agm/MagmFind.md) operation ([`M_STATUS`](../../Reference/agm/MagmGetResult.md)).

Note that only occurrences with small scale differences can be found.

Results are returned in descending score order using either the detection, fit, or coverage score. Set the score by which to sort using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_SORT_CANDIDATES_SCORE`](../../Reference/agm/MagmControl.md). The result with the highest score is returned first. For a complete description of all possible results, refer to [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) in the Aurora Imaging Library Reference.

Occurrences are indexed with positive integers starting from 0.

## Drawing results

The [`MagmDraw`](../../Reference/agm/MagmDraw.md) function provides several operations for drawing results in any specified image buffer or 2D graphics list. You can also choose to draw in the display's overlay buffer. By drawing into the display's overlay buffer, you can annotate an image non-destructively (see [Annotating the displayed image non-destructively](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md)).

You can use a previously allocated 2D graphics context (see [Generating graphics](../C26_Generating_graphics/ChapterInformation.md)) to control the drawing color, or use the default 2D graphics context ([`M_DEFAULT`](../../Reference/agm/MagmDraw.md)).

Supported drawing operations include drawing a bounding box around the model or occurrence, the edges of the model or occurrence, or a cross at the model's reference axis origin or occurrence position.

## Improving results

When false occurrences are found, you should re-call [`MagmFind`](../../Reference/agm/MagmFind.md) after modifying the relevant search settings discussed in [Finding model occurrences](S05_Finding_model_occurrences.md). For example, you can try:

- Increasing the minimum detection score ([`M_ACCEPTANCE_DETECTION`](../../Reference/agm/MagmControl.md)) until you begin to miss model occurrences.
- Increasing the minimum fit score ([`M_ACCEPTANCE_FIT`](../../Reference/agm/MagmControl.md)) when model appearances are not deformed.
- Increasing the minimum coverage score ([`M_ACCEPTANCE_COVERAGE`](../../Reference/agm/MagmControl.md)) when no model appearance is occluded.

If false occurrences are still found, you should use/re-train a composite-definition model. Make sure to label all the false occurrences as negative model locations. For more information, see [Composite-definition models](S04_Defining_and_adding_a_model_to_your_AGM_context.md) and [Training a model](S07_Training_a_model.md).

When the find operation does not find all occurrences, you should experiment with different values for controls, such as [`M_EDGEL_ANGLE_MODE`](../../Reference/agm/MagmControl.md) and [`M_EMPTY_REGIONS_MODE`](../../Reference/agm/MagmControl.md). See the [`MagmControl`](../../Reference/agm/MagmControl.md) function for other possible controls to modify.
