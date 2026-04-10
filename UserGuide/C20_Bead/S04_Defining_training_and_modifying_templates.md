---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Bead
section: Defining_training_and_modifying_templates
module_tag: bead
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / bead / Defining training and modifying templates"
---

# Defining, training, and modifying bead templates

After allocating a bead context using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md), you can add bead templates to it using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md).

A bead template is a reusable model of a bead. To define a template, you can specify a variety of settings, such as its type, path, and width. For example, you can define a template that represents a bead stripe, follows a polyline path (according to specified vertices), and has a consistent nominal width. To establish a closed bead template, the first and last points in the path must be at the same position.

*[Image: BeadBasicTrainingPath.png]*

Once you have specified the settings of your templates, you must train them using [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). During the training phase, Aurora Imaging Library typically uses a training image to refine the template's path (that you initially specified) and establish the template's trained points.

*[Image: BeadTrainingPath.png]*

Once trained, a bead template contains all the information required to verify beads, such as the trained points.

## Template type

A template's type refers to whether the template represents a bead that is an edge (edge-bead) or a stripe (stripe-bead). Templates that represent an edge indicate that the bead is essentially an outline, which has been delineated by a boundary established from intensity transitions in an image. In general, you should use an edge-bead to represent the skeletal border of an object. Templates that represent a stripe indicate that the bead has two edges separated by a width. In general, you should use a stripe-bead to represent a strip of some malleable material, such as glue or lead. By default, Aurora Imaging Library assumes that you are using stripe-beads.

*[Image: BeadEdgeType.png]*

*[Image: BeadStripeType.png]*

To specify that the template represents a bead that is a stripe or an edge, use [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_BEAD_STRIPE`](../../Reference/bead/MbeadTemplate.md) or [`M_BEAD_EDGE`](../../Reference/bead/MbeadTemplate.md), respectively, when adding the template. Deciding whether your template should represent a stripe or an edge depends on the bead that you will be working with and the requirements of your application.

## Foreground

By default, Aurora Imaging Library assumes that the foreground of the measured bead is lighter than the background. For stripe-beads, this implies a light stripe-bead (for example, a white bead) on a dark background (for example, a black surface).

To establish the foreground of an edge-bead, Aurora Imaging Library considers the bead's direction, which is implied by the order of its vertices. Based on the direction, the foreground is on the left side of the edge-bead, when following the path of the bead in that direction; the background is on the right side.

*[Image: BeadForegroundEdge.png]*

To set whether Aurora Imaging Library considers the foreground to be lighter or darker than the background, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_FOREGROUND_VALUE`](../../Reference/bead/MbeadControl.md).

## Template path

A template's path represents the continuous sequence of positions that the bead follows. To establish the path of a template, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md). You can specify that the template's path follows a:

- Polyline that you must refine with a training image ([`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md)).
- Fixed polyline ([`M_POLYLINE`](../../Reference/bead/MbeadControl.md)).
- Fixed circle or segment ([`M_CIRCLE`](../../Reference/bead/MbeadControl.md) or [`M_SEGMENT`](../../Reference/bead/MbeadControl.md)).

All paths are available with templates representing a bead that is either a stripe ([`M_BEAD_STRIPE`](../../Reference/bead/MbeadTemplate.md)) or an edge ([`M_BEAD_EDGE`](../../Reference/bead/MbeadTemplate.md)).

### Polyline that will be refined by a training image

By default ([`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md)), the template's path follows a polyline that you must specify with vertices, and that Aurora Imaging Library must refine with a training image. If you use [`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md) and do not specify a training image when calling [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), you will get an error.

To define the polyline, you must specify the X- and Y-coordinates of its vertices when adding the template using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_ADD`](../../Reference/bead/MbeadTemplate.md). The order of the vertices establishes the polyline's shape and direction. Typically, you will derive the positions of the vertices from the same image that you will use as your training image (which is usually a good version of a typical target image that you will use in the verification phase). A polyline path must have at least two vertices.

*[Image: BeadPolylineRefinedWithTrainingImage.png]*

Ideally, each vertex that you add with [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) should be at a position that corresponds to (or, is close to) a point along the corresponding bead in the training image. If there is an acute angle (0° to 90°), you should add a vertex at the angle's corner and on either side of the corner, to ensure that Aurora Imaging Library properly establishes the path.

*[Image: BeadAddInitialPoints.png]*

Several operations are available for templates whose path is [`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md), such as inserting, removing, and modifying its vertices. For more information, see [General operations with a template](S04_Defining_training_and_modifying_templates.md).

All templates in a bead context use the same training image. If you require multiple training images to refine the paths your templates must follow, use multiple bead contexts.

### Fixed polyline

To specify a polyline path that is fixed, use [`M_POLYLINE`](../../Reference/bead/MbeadControl.md). In this case, the trained position of the path will always follow the specified vertices (the path's position is not refined by a training image, even if you specify one). Typically, you should only use [`M_POLYLINE`](../../Reference/bead/MbeadControl.md) when your vertices define the exact path required to train the bead. Such points can, for example, come from a CAD file. To define a fixed polyline, you must specify its vertices using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_ADD`](../../Reference/bead/MbeadTemplate.md), as explained in [Polyline that will be refined by a training image](S04_Defining_training_and_modifying_templates.md).

Even when specifying a fixed polyline path, Aurora Imaging Library creates trained points along the path by default. In this case, although the trained path will be the same as the path specified by the vertices, the number of trained points in the template will be greater than the number of vertices you specified (which can improve accuracy for the verification phase). If you want the trained points to be exactly the same as the vertices specified, you must disable the spacing control Aurora Imaging Library uses to distribute the trained points by setting [`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadControl.md) to [`M_DISABLE`](../../Reference/bead/MbeadControl.md). For more information, see [The training box of the template](S04_Defining_training_and_modifying_templates.md).

*[Image: BeadPolylineFixed.png]*

### Fixed circle or segment

To specify a path that follows a fixed circle or segment, use [`M_CIRCLE`](../../Reference/bead/MbeadControl.md) or [`M_SEGMENT`](../../Reference/bead/MbeadControl.md). In this case, the trained position of the circle or segment path will always follow the specified path (the path's position is not refined by a training image, even if you specify one).

Unlike paths that follow a polyline, you do not specify vertices to set paths that follow a circle or segment. Instead, once you have added the template with [`M_ADD`](../../Reference/bead/MbeadTemplate.md), you must define the circle or segment path using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TEMPLATE_CIRCLE_CENTER_X`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_CIRCLE_CENTER_Y`](../../Reference/bead/MbeadControl.md), and [`M_TEMPLATE_CIRCLE_RADIUS`](../../Reference/bead/MbeadControl.md) for circles, or [`M_TEMPLATE_SEGMENT_START_X`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_SEGMENT_START_Y`](../../Reference/bead/MbeadControl.md), [`M_TEMPLATE_SEGMENT_END_X`](../../Reference/bead/MbeadControl.md), and [`M_TEMPLATE_SEGMENT_END_Y`](../../Reference/bead/MbeadControl.md) for segments.

*[Image: BeadTemplatePathCircleAndSegment.png]*

You can consider this as a quick way to create a circle or segment path, without having to provide multiple vertices to the template.

The direction of fixed segments is from their start point to their end point, while the direction of fixed circles is counter-clockwise. Aurora Imaging Library uses the direction to establish the bead's foreground. For more information, see [Foreground](S04_Defining_training_and_modifying_templates.md).

## Template from a 2D graphics list

You can add a bead template based on the vertices of a polyline (or polygon) from a 2D graphics list, by calling [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_ADD_FROM_GRAPHIC_LIST`](../../Reference/bead/MbeadTemplate.md). The 2D graphics list must contain one polyline (or polygon) graphic, added with [`MgraLines`](../../Reference/gra/MgraLines.md) or [`MgraInteractive`](../../Reference/gra/MgraInteractive.md). For more information about interactively adding graphics to a 2D graphics list, see [Creating graphics interactively](../C26_Generating_graphics/S07_Creating_and_modifying_graphics_interactively.md).

Aurora Imaging Library considers the specified vertices ([`MgraLines`](../../Reference/gra/MgraLines.md) or [`MgraInteractive`](../../Reference/gra/MgraInteractive.md)) as the template's vertices. Templates added with [`M_ADD_FROM_GRAPHIC_LIST`](../../Reference/bead/MbeadTemplate.md) behave the same as those added with [`M_ADD`](../../Reference/bead/MbeadTemplate.md), and whose path is set to [`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md) (default) or [`M_POLYLINE`](../../Reference/bead/MbeadControl.md). For more information, see [Template path](S04_Defining_training_and_modifying_templates.md).

Note that a polygon is simply a polyline that Aurora Imaging Library automatically closes by connecting the final vertex to the first with a straight line. Any operation or setting in the Aurora Imaging Library Bead module that applies to polylines also applies to polygons.

## Template width

A template's width refers to how thick its bead is, from side to side, along its path. If a template represents a bead that is a stripe ([`M_BEAD_STRIPE`](../../Reference/bead/MbeadTemplate.md)), the width refers to the thickness between the stripe's two edges. If the template represents a bead that is an edge ([`M_BEAD_EDGE`](../../Reference/bead/MbeadTemplate.md)), the width refers to the thickness of the edge's intensity transition.

To specify how to establish the width of the template's path, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md). You can either establish the width automatically according to a training image, or you can explicitly set it.

During training, Aurora Imaging Library attempts to establish the best possible width (the trained width) using your width settings, and all other relevant template settings. For example, to establish the best trained width, Aurora Imaging Library also considers the actual width of the bead in the training image (if one is used). To inquire the actual trained width of the bead template corresponding to each trained point along the bead template's path, use [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_TRAINED_INDIVIDUAL_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md). If the template's path follows a polyline, you can also inquire the width corresponding to each originally-specified vertex (before training), using [`M_INDIVIDUAL_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md). To inquire the trained nominal width of the bead template (that is, the average width of the bead template), use [`M_TRAINED_WIDTH_NOMINAL`](../../Reference/bead/MbeadInquire.md).

### Automatically establishing the width

By default, Aurora Imaging Library uses a training image to automatically establish an average width along the specified path ([`M_AUTO_UNIFORM`](../../Reference/bead/MbeadControl.md)); in this case, the trained bead width at each trained point is the same. To use a training image to establish a unique width at the position of each trained point, specify [`M_AUTO_CONTINUOUS`](../../Reference/bead/MbeadControl.md). If you use [`M_AUTO_...`](../../Reference/bead/MbeadControl.md) and do not specify a training image, you will get an error.

*[Image: BeadWidthAutoUniformContinuous.png]*

When using [`M_AUTO_...`](../../Reference/bead/MbeadControl.md) with a stripe-bead template, you can specify a range of valid bead widths. If the width established by the training phase is bigger or smaller than the specified range, the template will not be trained. To set the range of valid bead widths, you must set a nominal width using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md), and an upper and lower factor by which the nominal width can vary, using [`M_TRAINING_WIDTH_SCALE_MAX`](../../Reference/bead/MbeadControl.md) and [`M_TRAINING_WIDTH_SCALE_MIN`](../../Reference/bead/MbeadControl.md).

### Explicitly setting the width

To define an explicit nominal width for the path, you must set [`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md) to [`M_USER_DEFINED`](../../Reference/bead/MbeadControl.md), and then use [`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) to specify the value of the nominal width.

The width set for [`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md) applies to the entire path. If the template's path follows a polyline, you can also specify a unique width value for one or more of its vertices, using [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) with [`M_SET_WIDTH_NOMINAL`](../../Reference/bead/MbeadTemplate.md). You should only set the width of a vertex when you know that it differs from the nominal width. Aurora Imaging Library linearly interpolates the width between consecutive vertices.

*[Image: beadwidthnominal.png]*

## Training box of the template

The training box specifies the area along the path in which to establish a trained point when you call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). To define the training box, you must specify its height ([`M_TRAINING_BOX_HEIGHT`](../../Reference/bead/MbeadControl.md)) and width ([`M_TRAINING_BOX_WIDTH`](../../Reference/bead/MbeadControl.md)); you must also define the amount of space between the center of each training box along the path ([`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadControl.md)). The spacing between the boxes affects the total number of trained points (one per box) along the path.

*[Image: BeadTrainingBoxIntro.png]*

Aurora Imaging Library establishes trained points at regular intervals along the template's path and ensures that a trained point is at the start and end of paths that are not closed. The specified spacing is therefore adjusted accordingly, when you call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). Similarly, Aurora Imaging Library also modifies the specified height of the training box to achieve the best value based on all settings and the training image, if you are using one (note that Aurora Imaging Library does not typically alter the specified training box width). To inquire the actual (trained) spacing, height, and width that Aurora Imaging Library establishes, use [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with[`M_TRAINED_BOX_SPACING`](../../Reference/bead/MbeadInquire.md), [`M_TRAINED_BOX_HEIGHT`](../../Reference/bead/MbeadInquire.md) and [`M_TRAINED_BOX_WIDTH`](../../Reference/bead/MbeadInquire.md). To inquire the position and number of trained points, use [`M_TRAINED_POSITION_X`](../../Reference/bead/MbeadInquire.md), [`M_TRAINED_POSITION_Y`](../../Reference/bead/MbeadInquire.md), and [`M_TRAINED_SIZE`](../../Reference/bead/MbeadInquire.md). To inquire the training values exactly as you specified them, use [`M_TRAINING_...`](../../Reference/bead/MbeadInquire.md).

If your template follows a polyline path that Aurora Imaging Library must refine with a training image ([`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md)), the template can only be completely trained if the trained point expected for each training box is located in the training image.

*[Image: BeadTrainingBoxForPolylinePathWithTrainingImage.png]*

When using a polyline path that Aurora Imaging Library refines with a training image, you should ensure that the training box is large enough to enclose the bead's width, otherwise Aurora Imaging Library might not be able to completely train the template. For more information, see [Template width](S04_Defining_training_and_modifying_templates.md) and [Training status](S04_Defining_training_and_modifying_templates.md).

### Smoothness and threshold

When you are using a training image with questionable qualities, such as specular highlights and low contrast, and some expected trained points are not found, or you are finding unwanted points, you might consider modifying the default smoothness ([`M_SMOOTHNESS`](../../Reference/bead/MbeadControl.md)) and threshold ([`M_THRESHOLD_VALUE`](../../Reference/bead/MbeadControl.md)) settings that Aurora Imaging Library uses to extract the bead. In general, the training image should be a good representation of the target images that you will use with [`MbeadVerify`](../../Reference/bead/MbeadVerify.md).

Smoothness is the noise reduction that Aurora Imaging Library applies when extracting beads from an image. You can set the smoothness to a value between 0.0 (almost no noise reduction) and 100.0 (a very strong noise reduction). The default is 50.0, which is typically sufficient. If you experience shiny specular highlights on the bead causing unwanted points to be extracted, adjusting the smoothness can be useful.

The threshold ranges from 0.0 to 100.0 and refers to the limit beneath which a grayscale variation within the image is not considered a part of the bead. 0.0 indicates that Aurora Imaging Library considers all grayscale variations, while 100.0 indicates that Aurora Imaging Library only considers the most extreme grayscale variations. The default is 10.0, which is typically sufficient. If your image has very low contrast, adjusting the threshold can be useful.

Note that Aurora Imaging Library extracts beads from images using processes based on the Aurora Imaging Library Measurement module. For more information about this process, and how to affect it using smoothness and threshold settings, see [Search algorithm](../C19_Measurements/S05_Search_Algorithm.md).

## General operations with a template

[`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) allows you to perform general operations with a template, such as deleting any template in the context ([`M_DELETE`](../../Reference/bead/MbeadTemplate.md)). For templates whose path follows a polyline ([`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md) or [`M_POLYLINE`](../../Reference/bead/MbeadControl.md)), there are also several other general operations available, allowing you to:

- Move a template ([`M_TRANSLATE_POINTS`](../../Reference/bead/MbeadTemplate.md)).
- Rotate a template around its origin ([`M_ROTATE_POINTS`](../../Reference/bead/MbeadTemplate.md)).
- Resize a template according to a scale factor ([`M_SCALE_POINTS`](../../Reference/bead/MbeadTemplate.md)).
- Remove a set of vertices from a template ([`M_DELETE`](../../Reference/bead/MbeadTemplate.md)).
- Add a set of vertices to a template ([`M_INSERT`](../../Reference/bead/MbeadTemplate.md)).
- Replace a set of vertices in a template with another set of vertices ([`M_REPLACE`](../../Reference/bead/MbeadTemplate.md)).

After performing one of these operations, you must re-train the template before calling [`MbeadVerify`](../../Reference/bead/MbeadVerify.md).

### Obtaining the closest template and vertex

[`MbeadGetNeighbors`](../../Reference/bead/MbeadGetNeighbors.md) allows you to specify a source position, and to retrieve the label of the closest template, and the closest vertex within that template, to that position. You can also use this function to retrieve the closest vertex within a specified template, to the specified position.

To set the maximum distance within which Aurora Imaging Library considers a template or vertex to be the closest, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_CLOSEST_POINT_MAX_DISTANCE`](../../Reference/bead/MbeadControl.md). By default, there is no maximum distance.

You can only use [`MbeadGetNeighbors`](../../Reference/bead/MbeadGetNeighbors.md) with templates whose path follows a polyline.

## Training status

Once you have specified the required training settings and used [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) to train the templates, you should inquire their training status using [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_STATUS`](../../Reference/bead/MbeadInquire.md). The status accounts for all the templates in the context and is based on the difference between the number of trained points established and the number of trained points expected, after the last call to [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) (using the most current settings at that time). As previously discussed, a trained point is expected within each training box along the template's path.

The possible values that Aurora Imaging Library returns for [`M_STATUS`](../../Reference/bead/MbeadInquire.md) are:

- [`M_COMPLETE`](../../Reference/bead/MbeadInquire.md).
  For a template to be completely trained, every trained point that was expected must have been established (found). Aurora Imaging Library only returns [`M_COMPLETE`](../../Reference/bead/MbeadInquire.md) when all templates in the context meet this requirement.
- [`M_PARTIAL`](../../Reference/bead/MbeadInquire.md).
  For a template to be partially trained, at least two trained points (but less than all) that were expected must have been established (found). Aurora Imaging Library only returns [`M_PARTIAL`](../../Reference/bead/MbeadInquire.md) when one or more templates in the context meet this requirement.
- [`M_NOT_TRAINED`](../../Reference/bead/MbeadInquire.md).
  For a template to be not-trained, no trained point (or only one) has been established (found). Aurora Imaging Library only returns [`M_NOT_TRAINED`](../../Reference/bead/MbeadInquire.md) when one or more templates in the context meet this requirement. [`M_NOT_TRAINED`](../../Reference/bead/MbeadInquire.md) can indicate that you have not yet called [`MbeadTrain`](../../Reference/bead/MbeadTrain.md).

The following is a list of possible values that Aurora Imaging Library returns for [`M_STATUS`](../../Reference/bead/MbeadInquire.md), given the specified condition of the templates in the context:

| A context with ten templates | Possible values returned by [`M_STATUS`](../../Reference/bead/MbeadInquire.md) |
| --- | --- |
| In every template, Aurora Imaging Library established a trained point for each that was expected. | [`M_COMPLETE`](../../Reference/bead/MbeadInquire.md) |
| In nine of the templates, Aurora Imaging Library established a trained point for all that were expected. In one of the templates, Aurora Imaging Library established a trained point for all that were expected, except one (for example, it was not found in the training image). | [`M_PARTIAL`](../../Reference/bead/MbeadInquire.md) |
| In eight of the templates, Aurora Imaging Library established a trained point for each that was expected. In one of the templates, Aurora Imaging Library established a trained point for each that was expected, except one. In one template, Aurora Imaging Library was only able to establish one trained point that was expected. | [`M_NOT_TRAINED`](../../Reference/bead/MbeadInquire.md) |

### When templates are not completely trained

For the most part, templates that are not completely trained use a training image. Typically, such templates follow a polyline path that Aurora Imaging Library must refine ([`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md)). When such templates fail, it usually indicates that one or more points that Aurora Imaging Library expects to be trained (given the template's settings) are not found in the training image. To help decipher why a template isn't completely trained, you can perform drawing operations on the template. For example, you can draw a cross at the position of all the trained points in the template that were expected but not found during the training phase. For more information, see [Drawing](S06_Drawing.md).

In general, you should only verify beads with templates that are completely trained. When they are not, you should re-call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) after modifying the relevant training settings discussed in this subsection. For example, you can try:

- Modifying the template's vertices with [`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md) to ensure that the template's path follows a polyline that replicates, as accurately as possible, the bead in the training image. To do so, you can try increasing the number or positional accuracy of your vertices.
- Adjusting the width of the template's path to encompass the width along the entire length of the bead in the training image. Ensure that the width of the path includes the largest possible width of the bead in the training image.
- Resizing the training box in which Aurora Imaging Library finds the trained points. Ensure that the training box includes the largest possible width of the bead. You can also modify the spacing between the training boxes, which essentially alters the number of trained points expected.
- Improving the quality of the bead in the training image by adjusting the smoothness and threshold settings that Aurora Imaging Library uses to extract the bead from the image.

Templates that you do not train with a training image, such as those that follow a fixed path ([`M_POLYLINE`](../../Reference/bead/MbeadControl.md), [`M_CIRCLE`](../../Reference/bead/MbeadControl.md), [`M_SEGMENT`](../../Reference/bead/MbeadControl.md)) and whose width is explicitly set ([`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md)), are less likely to fail training because their settings do not require [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) to make significant preprocessing adjustments. For example, the first call to [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) will always completely train a template with two or more vertices, whose path follows a fixed polyline, and whose width is user-defined.

## Input units

Certain bead values relating to position and dimension (such as [`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadControl.md)) can be interpreted in either pixel or world units. To set the units, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with either [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md) or [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadControl.md). This essentially establishes the input coordinate system to use.

If you set the input units to [`M_PIXEL`](../../Reference/bead/MbeadControl.md), related values are interpreted in pixel units (pixel coordinate system). If you use calibrated images and specify [`M_PIXEL`](../../Reference/bead/MbeadControl.md), the specified pixel values will be internally converted to world values.

If you set the input units to [`M_WORLD`](../../Reference/bead/MbeadControl.md), related values are interpreted in world units (relative coordinate system). If you do not use calibrated images and specify [`M_WORLD`](../../Reference/bead/MbeadControl.md), an error will be generated.

To use a calibrated training image, you can either pass one to [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), or you can specify the camera calibration context to associate to the training image using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/bead/MbeadControl.md). If both camera calibrations are available, Aurora Imaging Library ignores the camera calibration set with [`M_ASSOCIATED_CALIBRATION`](../../Reference/bead/MbeadControl.md).

When you use [`MbeadSave`](../../Reference/bead/MbeadSave.md),[`MbeadStream`](../../Reference/bead/MbeadStream.md), or [`MbeadRestore`](../../Reference/bead/MbeadRestore.md), the camera calibration is saved to or restored from the same file as the bead context and cannot be managed independently from the bead context. When the bead context is freed, the camera calibration is automatically freed as well.

For more information, see [Working with real-world units](../C28_Calibration/S09_Working_with_realworld_units.md).

## Saving the training image

A copy of the training image is stored with the bead context upon calling [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). This allows you to draw the training image ([`MbeadDraw`](../../Reference/bead/MbeadDraw.md)), or to save a training image with the bead context ([`MbeadSave`](../../Reference/bead/MbeadSave.md) or [`MbeadStream`](../../Reference/bead/MbeadStream.md)), and then use it when you restore the context ([`MbeadRestore`](../../Reference/bead/MbeadRestore.md) or [`MbeadStream`](../../Reference/bead/MbeadStream.md)).

## Summary of the main training settings

As described above, there are numerous settings for training your template. The following represents a global summary of the main settings, and how they interrelate.

1. Set the bead's type ([`M_BEAD_STRIPE`](../../Reference/bead/MbeadTemplate.md) or [`M_BEAD_EDGE`](../../Reference/bead/MbeadTemplate.md)).
2. Set the bead's path ([`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md)).
   1. A polyline that must be refined with a training image ([`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md)).
   2. A fixed polyline ([`M_POLYLINE`](../../Reference/bead/MbeadControl.md)).
   3. A fixed circle or segment ([`M_CIRCLE`](../../Reference/bead/MbeadControl.md) or [`M_SEGMENT`](../../Reference/bead/MbeadControl.md)).
3. Set the width of the bead's path ([`M_WIDTH_NOMINAL_MODE`](../../Reference/bead/MbeadControl.md)).
   1. Automatically establish the path's width from a training image ([`M_AUTO_UNIFORM`](../../Reference/bead/MbeadControl.md) or [`M_AUTO_CONTINUOUS`](../../Reference/bead/MbeadControl.md)).
      1. For stripe beads, set the range of valid widths ([`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md), [`M_TRAINING_WIDTH_SCALE_MAX`](../../Reference/bead/MbeadControl.md), and [`M_TRAINING_WIDTH_SCALE_MIN`](../../Reference/bead/MbeadControl.md)).
   2. Explicitly set the path's width value ([`M_USER_DEFINED`](../../Reference/bead/MbeadControl.md)).
      1. Set the width value ([`M_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md)).
      2. For polyline beads with varying widths, set the width to associate to the specified vertices ([`M_SET_WIDTH_NOMINAL`](../../Reference/bead/MbeadTemplate.md)).
4. Set the training box ([`M_TRAINING_BOX_HEIGHT`](../../Reference/bead/MbeadControl.md), [`M_TRAINING_BOX_WIDTH`](../../Reference/bead/MbeadControl.md), and [`M_TRAINING_BOX_SPACING`](../../Reference/bead/MbeadControl.md)).
5. Train the template ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)).
