---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_Geometric_Matcher
section: Retrieving_and_analyzing_training_results
module_tag: agm
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-geometric-matcher / Retrieving and analyzing training results"
---

# Retrieving and analyzing training results

After training your model using [`MagmTrain`](../../Reference/agm/MagmTrain.md), you can extract the required results from your train AGM result buffer using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md). For information on training, see[Training a model](S07_Training_a_model.md).

Note, for information on search results, see [Retrieving and analyzing search results](S06_Retrieving_and_analyzing_search_results.md).

## Possible results

You can retrieve several types of training results with the AGM module. These results provide information on the nature of the trained composite-definition model and training labels. In addition to the model's training status discussed previously, you can retrieve results for:

- Status of the [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation ([`M_STATUS`](../../Reference/agm/MagmGetResult.md)).
- Number of trained composite-definition models ([`M_NUMBER`](../../Reference/agm/MagmGetResult.md)). Note that training results in either 0 or 1 trained composite-definition models.
- Number of positive (blue) and negative (red) rectangles across all training images ([`M_NUMBER_POS_RECTANGLES`](../../Reference/agm/MagmGetResult.md) and [`M_NUMBER_NEG_RECTANGLES`](../../Reference/agm/MagmGetResult.md)).
- Training confidence ([`M_TRAINING_CONFIDENCE`](../../Reference/agm/MagmGetResult.md)).
- Training error ([`M_TRAINING_ERROR`](../../Reference/agm/MagmGetResult.md)).

See [Using a trained composite-definition model](S07_Training_a_model.md) for more information about the training confidence and training error.

You can also retrieve the following rectangle-specific results. These are useful, for example, to retrieve information about the automatically established negative (red) rectangles. Use the [`Index`](../../Reference/agm/MagmGetResult.md) parameter of [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) to specify the rectangle for which to retrieve information.

- Rectangle's angle ([`M_ANGLE`](../../Reference/agm/MagmGetResult.md)).
- Rectangle's center coordinates ([`M_CENTER_X`](../../Reference/agm/MagmGetResult.md) and [`M_CENTER_Y`](../../Reference/agm/MagmGetResult.md)).
- Rectangle's top-left corner coordinates ([`M_CORNER_TOP_LEFT_X`](../../Reference/agm/MagmGetResult.md) and [`M_CORNER_TOP_LEFT_Y`](../../Reference/agm/MagmGetResult.md)).
- Rectangle's height and width ([`M_RECTANGLE_HEIGHT`](../../Reference/agm/MagmGetResult.md) and [`M_RECTANGLE_WIDTH`](../../Reference/agm/MagmGetResult.md)).
- Rectangle's training image index ([`M_IMAGE_INDEX`](../../Reference/agm/MagmGetResult.md)).

Rectangles are indexed with positive integers starting from 0.

## Drawing results

The [`MagmDraw`](../../Reference/agm/MagmDraw.md) function provides several operations for drawing results in any specified image buffer or 2D graphics list. You can also choose to draw in the display's overlay buffer. By drawing into the display's overlay buffer, you can annotate an image non-destructively (see [Annotating the displayed image non-destructively](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md)).

You can use a previously allocated 2D graphics context (see [Generating graphics](../C26_Generating_graphics/ChapterInformation.md)) to control the drawing color, or use the default 2D graphics context ([`M_DEFAULT`](../../Reference/agm/MagmDraw.md)).

Supported drawing operations include drawing the rectangles that were used to label the negative model locations or the rectangles that were used to label the positive model locations.
