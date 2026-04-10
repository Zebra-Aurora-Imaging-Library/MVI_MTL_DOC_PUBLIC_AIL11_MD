---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Bead
section: Drawing
module_tag: bead
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / bead / Drawing"
---

# Drawing

By calling [`MbeadDraw`](../../Reference/bead/MbeadDraw.md), you can perform drawing operations with a bead template and its points (when passing a bead context to the function), or with a measured bead and its points (when passing a bead result to the function). For example, when using a bead context, you can use [`M_DRAW_POSITION`](../../Reference/bead/MbeadDraw.md) with [`M_TRAINED_PASS`](../../Reference/bead/MbeadDraw.md) to draw a cross at the position of each trained point that has successfully passed the training phase. Similarly, when using a bead result, you can use [`M_DRAW_POSITION`](../../Reference/bead/MbeadDraw.md) with [`M_PASS`](../../Reference/bead/MbeadDraw.md) to draw a cross at the position of each trained point's corresponding measured point that has successfully passed the verification phase.

You can perform drawings in either an image buffer or 2D graphics list, unless otherwise specified. To control the drawing color, you can use [`MbeadDraw`](../../Reference/bead/MbeadDraw.md) with a previously allocated 2D graphics context or with the default 2D graphics context. You can also choose to draw in the display's overlay buffer, which allows you to annotate an image non-destructively. For more information, see [Generating graphics](../C26_Generating_graphics/ChapterInformation.md) and [Annotating the displayed image non-destructively](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md).

When calling [`MbeadDraw`](../../Reference/bead/MbeadDraw.md) with a bead context (and using a calibrated training image) or with a bead result (and using a calibrated target image), the destination drawing buffer need not be calibrated to the same world. Aurora Imaging Library draws according to the camera calibration context associated to either the training or verification image.

## Specifying the points

When calling [`MbeadDraw`](../../Reference/bead/MbeadDraw.md), you must specify the points with which to draw, by setting the [`DrawOption`](../../Reference/bead/MbeadDraw.md) parameter. The settings available depend on whether you are calling [`MbeadDraw`](../../Reference/bead/MbeadDraw.md) with a bead context or result.

### When drawing with a bead context

When drawing with a bead context, you can specify that Aurora Imaging Library performs the drawing operation ([`M_DRAW_...`](../../Reference/bead/MbeadDraw.md)) using one of the following:

- [`M_USER`](../../Reference/bead/MbeadDraw.md), which performs the drawing operation using the template's vertices.
  If the template's path ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md)) follows a polyline, [`M_USER`](../../Reference/bead/MbeadDraw.md) refers to the template's polyline points ([`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md)). If the template's path follows a circle or segment, [`M_USER`](../../Reference/bead/MbeadDraw.md) refers to the minimum critical points that Aurora Imaging Library internally calculates based on the settings of the path. For a path that follows a segment, Aurora Imaging Library calculates three critical points, according to the specified start and end of the segment ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TEMPLATE_SEGMENT_...`](../../Reference/bead/MbeadControl.md)). For a path that follows a circle, Aurora Imaging Library calculates five critical points, according to the specified center and radius of the circle ([`M_TEMPLATE_CIRCLE_...`](../../Reference/bead/MbeadControl.md)).
- [`M_TRAINED_FAIL`](../../Reference/bead/MbeadDraw.md), which performs the drawing operation using all the trained points that were expected but not established (found) during training ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)). In this case, the drawing operation uses the expected position of the trained point. Such drawings can be particularly useful when deciphering why a template isn't completely trained ([`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_STATUS`](../../Reference/bead/MbeadInquire.md)), especially if drawn on top of the training image ([`M_DRAW_TRAINING_IMAGE`](../../Reference/bead/MbeadDraw.md)).
- [`M_TRAINED_PASS`](../../Reference/bead/MbeadDraw.md), which performs the drawing operation using all the trained points that were established (found) during training.
- [`M_TRAINED`](../../Reference/bead/MbeadDraw.md), which performs the drawing operation using all the trained points that were found ([`M_TRAINED_PASS`](../../Reference/bead/MbeadDraw.md)) and all the trained points that were expected but not found ([`M_TRAINED_FAIL`](../../Reference/bead/MbeadDraw.md)) during training.

### When drawing with a bead result

When drawing with a bead result, you can specify that Aurora Imaging Library performs the drawing operation ([`M_DRAW_...`](../../Reference/bead/MbeadDraw.md)) using one of the following:

- [`M_FAIL`](../../Reference/bead/MbeadDraw.md), which performs the drawing operation using all the measured points that were expected but did not pass verification. In this case, the drawing operation uses the expected position of the measured point (based on its associated trained point).
  You can also draw using the measured points that failed for a more specific reason. For example, you can draw: the expected position of a measured point that was not found ([`M_FAIL_NOT_FOUND`](../../Reference/bead/MbeadDraw.md)), the position of a measured point that was found but has failed because its position is not within the specified offset ([`M_FAIL_OFFSET`](../../Reference/bead/MbeadDraw.md)), or the position of a measured point that was found but has failed because the width of the measured bead at that point is less than the specified minimum width ([`M_FAIL_WIDTH_MIN`](../../Reference/bead/MbeadDraw.md)). Such drawings can be particularly useful when deciphering why [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) is returning a failed status ([`M_STATUS`](../../Reference/bead/MbeadGetResult.md)), especially when drawn on top of a target image.
- [`M_PASS`](../../Reference/bead/MbeadDraw.md), which performs the drawing operation using all the measured points that have passed verification.
- [`M_ALL`](../../Reference/bead/MbeadDraw.md), which performs the drawing operation using all the measured points ([`M_PASS`](../../Reference/bead/MbeadDraw.md) and [`M_FAIL`](../../Reference/bead/MbeadDraw.md)).

## Specifying the drawing operation

Once you have selected the set of points, you should specify the type of drawing operation to perform on them. Using the selected set of points, Aurora Imaging Library can draw:

- The index value of each point ([`M_DRAW_POSITION_INDEX`](../../Reference/bead/MbeadDraw.md)). If you specify [`M_USER`](../../Reference/bead/MbeadDraw.md) as your set of points, you can only perform this drawing operation for templates with a path that follows a polyline, otherwise you will get an error.
  You can also draw the index or label of the template that contains the selected set of points ([`M_DRAW_INDEX`](../../Reference/bead/MbeadDraw.md) or [`M_DRAW_LABEL`](../../Reference/bead/MbeadDraw.md)).
- A cross at the position of each point ([`M_DRAW_POSITION`](../../Reference/bead/MbeadDraw.md)) or a line that connects all the points ([`M_DRAW_POSITION_POLYLINE`](../../Reference/bead/MbeadDraw.md)).
- The area within which each point was established ([`M_DRAW_SEARCH_BOX`](../../Reference/bead/MbeadDraw.md)) or an H-type line (|-|) indicating the width of the bead at that point ([`M_DRAW_WIDTH`](../../Reference/bead/MbeadDraw.md)).

In the following example, Aurora Imaging Library has drawn: the width of the bead (in white) at each measured point that has a passing status ([`M_DRAW_WIDTH`](../../Reference/bead/MbeadDraw.md) and [`M_PASS`](../../Reference/bead/MbeadDraw.md)), the search box (in white) of the measured points that were expected but not found ([`M_DRAW_SEARCH_BOX`](../../Reference/bead/MbeadDraw.md) and [`M_FAIL_NOT_FOUND`](../../Reference/bead/MbeadDraw.md)), and the search box (in gray) of the measured points that were expected but failed because the corresponding bead width was too small ([`M_DRAW_SEARCH_BOX`](../../Reference/bead/MbeadDraw.md) and [`M_FAIL_WIDTH_MIN`](../../Reference/bead/MbeadDraw.md)).

*[Image: BeadDrawExample.png]*

You can also call [`MbeadDraw`](../../Reference/bead/MbeadDraw.md) to draw a training image, using [`M_DRAW_TRAINING_IMAGE`](../../Reference/bead/MbeadDraw.md). Unlike other drawing operations, drawing a training image is not based on a specific set of points and it cannot be drawn in a 2D graphics list. Also, when drawing a training image, you must use [`MbeadDraw`](../../Reference/bead/MbeadDraw.md) with a bead context and you must have internally saved the training image with [`MbeadTrain`](../../Reference/bead/MbeadTrain.md).

## Adjusting the templates and trained points with which to draw

As previously discussed, Aurora Imaging Library performs the drawing operation ([`DrawOperation`](../../Reference/bead/MbeadDraw.md)) using the type of points selected ([`DrawOption`](../../Reference/bead/MbeadDraw.md)), for either the bead context or result. Typically, you will want to perform the drawing operation on all points in all beads that meet the specified requirements. In this case, you must set the [`LabelOrIndex`](../../Reference/bead/MbeadDraw.md) and [`Index`](../../Reference/bead/MbeadDraw.md) parameters to [`M_ALL`](../../Reference/bead/MbeadDraw.md). You can however adjust these two parameter settings and specify that Aurora Imaging Library should perform the drawing using a specific point in a specific template, or using one or all points in one or all templates. For example, you can draw search boxes for only one particular template in the context, or for only one particular point.

You can also specify that Aurora Imaging Library should base the drawing on the specified templates as a whole, by setting the [`Index`](../../Reference/bead/MbeadDraw.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadDraw.md) and the [`LabelOrIndex`](../../Reference/bead/MbeadDraw.md) parameter to one or all templates. In this case, Aurora Imaging Library performs the drawing for the entire template if it contains one or more points that meet the requirements of the [`DrawOption`](../../Reference/bead/MbeadDraw.md) parameter setting. For example, if you are using bead results and specify [`M_DRAW_POSITION`](../../Reference/bead/MbeadDraw.md) with [`M_PASS`](../../Reference/bead/MbeadDraw.md), and you set the [`Index`](../../Reference/bead/MbeadDraw.md) parameter to [`M_GENERAL`](../../Reference/bead/MbeadDraw.md) and the [`LabelOrIndex`](../../Reference/bead/MbeadDraw.md) parameter to a specific template, Aurora Imaging Library will draw the position of all the points related to that template as long as at least one of its trained point's corresponding measured point has passed verification. Given this high level of flexibility, you should be especially mindful of each parameter setting when calling [`MbeadDraw`](../../Reference/bead/MbeadDraw.md).
