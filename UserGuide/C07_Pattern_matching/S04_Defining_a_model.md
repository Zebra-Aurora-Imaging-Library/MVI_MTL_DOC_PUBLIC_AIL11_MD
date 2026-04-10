---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Defining_a_model
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Defining a model"
---

# Defining and adding models to your Pattern Matching context

Once you have allocated a Pattern Matching context, using [`MpatAlloc`](../../Reference/pat/MpatAlloc.md), you can begin to add models to your Pattern Matching context, using [`MpatDefine`](../../Reference/pat/MpatDefine.md).

There are two types of models: models defined from a specified location in the model's source image, and models that Aurora Imaging Library automatically defines from the model's source image. These models can be mixed in the same Pattern Matching context, using [`MpatDefine`](../../Reference/pat/MpatDefine.md).

Models of both types should be distinct and not defined from a flat region (consistent pixel values with no edges present). Also, the model's source image should be of the best quality possible. The model can be defined when it is at an angle. When defining multiple models, they must be the same size and at the same resolution. If noisy, try to clean the image, using the image processing techniques discussed elsewhere in this user guide.

## Automatic models

To have Aurora Imaging Library define one or more distinct models from a model's source image, use [`MpatDefine`](../../Reference/pat/MpatDefine.md) with[`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md). Specify the width and height of the model(s), as well as the maximum displacement expected between the position of the model in the source image and the position of the model when found in the target image. These four measurements define an area in which Aurora Imaging Library expects to find a model in the source image. The reference position (used to identify the start of the area) is placed at half the width and height of the extracted model. To change the reference position, use [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_REFERENCE_X`](../../Reference/pat/MpatControl.md) and [`M_REFERENCE_Y`](../../Reference/pat/MpatControl.md).

*[Image: mpatwafer.png]*

You can only define [`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md) models that respect the following condition: `(_MaxPixelValue_ <sup>2</sup> *`   _ModelWidth_ x _ModelHeight_)`&lt; 2<sup>63</sup>`, where _MaxPixelValue_ is the maximum pixel value (typically, 255) in the target image and the model. This restriction is imposed to avoid overflows in the internal 64-bit accumulators.

The total area of the model must be greater or equal to 4 pixels.

## Regular models

To define a model from a specified location in a model's source image, use [`MpatDefine`](../../Reference/pat/MpatDefine.md) with[`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md); this is referred to as a regular model. To create a regular model, pass [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md), the model's required offset from the top-left corner of the model's source image, width, and height. A regular model is typically used in cases where the target image contains an inconsistent surrounding (such as an image of loose nuts and bolts lying on a metal sheet). If the eventual model can appear at an angle, enable angular search, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SEARCH_ANGLE_MODE`](../../Reference/pat/MpatControl.md) and set the range of angles at which it can appear using[`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md). If the model has a background that will be consistently found in the target image (such as when searching for a chip on an integrated circuit board), combine [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md) with [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md)when calling [`MpatDefine`](../../Reference/pat/MpatDefine.md).

To define a model from a rotated region of the model's source image, you can specify a source image that is associated with a rotated rectangular ROI (that is, a rectangular ROI oriented at a non-zero angle). To define the ROI, add a rectangle at the required angle to a 2D graphics list, using [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md). To set that rectangle as the ROI of an image buffer, call [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with [`M_RASTERIZE`](../../Reference/buf/MbufSetRegion.md) or [`M_NO_RASTERIZE`](../../Reference/buf/MbufSetRegion.md) and with[`M_FILL_REGION`](../../Reference/buf/MbufSetRegion.md).

## Drawing models

Upon definition, the model is extracted from the selected region in the model's source image buffer and stored in a non-displayable model buffer. The model's source image buffer is then no longer needed. To view the portion of the image from which the model was extracted (referred to as the model image), use [`MpatDraw`](../../Reference/pat/MpatDraw.md) with [`M_DRAW_IMAGE`](../../Reference/pat/MpatDraw.md). [`MpatDraw`](../../Reference/pat/MpatDraw.md) can also draw the various model features (such as [`M_DRAW_POSITION`](../../Reference/pat/MpatDraw.md)). Use the [`ControlFlag`](../../Reference/pat/MpatDraw.md) parameter to specify where in the destination buffer to draw the model information. You can start drawing at the top-left pixel of the destination buffer ([`M_DEFAULT`](../../Reference/pat/MpatDraw.md)) or at the same offset as was used to extract the model from the model's source image buffer ([`M_ORIGINAL`](../../Reference/pat/MpatDraw.md)).

You can use a previously allocated 2D graphics context (see [Generating graphics](../C26_Generating_graphics/ChapterInformation.md)) to control the drawing color, or use the default 2D graphics context ([`M_DEFAULT`](../../Reference/pat/MpatDraw.md)). You can also choose to draw in the display's overlay buffer. By drawing into the display's overlay buffer, you can annotate an image non-destructively. For more information, see [Displaying an image](../C25_Displaying_an_image/ChapterInformation.md).
