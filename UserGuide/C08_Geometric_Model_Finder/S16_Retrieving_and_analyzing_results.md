---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Retrieving_and_analyzing_results
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Retrieving and analyzing results"
---

# Retrieving and analyzing results

After having successfully located your model occurrences in your target using [`MmodFind`](../../Reference/mod/MmodFind.md), you can extract the required results from your result buffer using [`MmodGetResult`](../../Reference/mod/MmodGetResult.md).

## Possible results

You can retrieve several types of results with the Model Finder module. These results provide considerable information on the nature of the occurrence found. In addition to the score, target score, model coverage, target coverage, fit error, and forward/reverse transformation coefficients discussed previously in this chapter, results can be returned for:

- Number of occurrences.
- Index or user label of occurrences.
- Position X and position Y.
- Scale.
- Polarity.
- Angle.
- Whether the timeout limit has been reached.

You can specify whether to retrieve results for all edges of the target in the region of the occurrence, or only the edges in the occurrence that correspond to the model's active edges, using [`M_TARGET`](../../Reference/mod/MmodGetResult.md) and [`M_MODEL`](../../Reference/mod/MmodGetResult.md), respectively.

*[Image: ModelvsTarget.png]*

Results are returned in descending order of match score, such that the result with the highest score is returned first. Generally, you should first retrieve the total number of occurrences found for all the models in the Model Finder context, to ascertain the size of the result array needed. Note that results can be returned for the entire context; the model index ([`M_INDEX`](../../Reference/mod/MmodGetResult.md) result type) is used to differentiate results between models. For a complete description of all possible results, refer to the description of [`MmodGetResult`](../../Reference/mod/MmodGetResult.md) in the Aurora Imaging Library Reference.

Note that certain results types are only supported with certain types of models, or result buffers. For instance, [`M_RADIUS`](../../Reference/mod/MmodGetResult.md) is only supported for [`M_CIRCLE`](../../Reference/mod/MmodDefine.md) type models, while the [`M_ASPECT_RATIO`](../../Reference/mod/MmodGetResult.md) result type is not supported for an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAllocResult.md) type of Model Finder result buffer.

Occurrences are indexed with positive integers starting from 0.

## Drawing results

The [`MmodDraw`](../../Reference/mod/MmodDraw.md) function provides several operations for drawing results in any specified image buffer or 2D graphics list. You can also choose to draw in the display's overlay buffer. By drawing into the display's overlay buffer, you can annotate an image non-destructively (see [Annotating the displayed image non-destructively](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md)). You can also draw zoomed results. You can draw a model box, or a bounding box around the occurrence, the active edges of the model, the edges of the result occurrence, or draw a cross-like symbol at the model's reference axis origin or occurrence position.

Typically, all drawing operations in [`MmodDraw`](../../Reference/mod/MmodDraw.md) can be combined; you can therefore draw multiple results simultaneously. For example, to draw the edges of the result occurrence and its position, you would specify [`M_DRAW_EDGES`](../../Reference/mod/MmodDraw.md) + [`M_DRAW_POSITION`](../../Reference/mod/MmodDraw.md).

The following code snippet shows how to combine drawing operations using Aurora Imaging Library constant combination:

> **Code example:** [userguide.geometric_model_finder.retrieving_and_analyzing_results01](userguide.geometric_model_finder.retrieving_and_analyzing_results01)

When drawing results, you can draw the active edges of the model transformed at the occurrence position, or the edges of the target in the region of the occurrence.

You can also draw a zoomed version of results that were obtained from a specified region in the target. To do so, specify the appropriate values for the [`MgraControl`](../../Reference/gra/MgraControl.md)   [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md), [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md), [`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md), and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md) control types. The relative origin values must be specified in pixels, and are relative to the coordinates of the top-left corner of the region in the model source, while the scale values specify the X- and Y-scaling factors used to fill the destination buffer. For more information on zooming, see [Defining and adding models to your Model Finder context](S05_Defining_and_adding_models_to_your_model_finder_context.md).

You can use a previously allocated 2D graphics context (see [Generating graphics](../C26_Generating_graphics/ChapterInformation.md)) to control the drawing color, or use the default 2D graphics context ([`M_DEFAULT`](../../Reference/mod/MmodDraw.md)).
