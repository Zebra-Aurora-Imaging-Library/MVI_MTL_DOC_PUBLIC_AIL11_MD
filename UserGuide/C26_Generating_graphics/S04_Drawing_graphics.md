---
doctype: UserGuide
part: "2D related information"
chapter: Generating_graphics
section: Drawing_graphics
module_tag: gra
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / graphics / Drawing graphics"
---

# Drawing graphics

There are multiple ways to annotate an image. The functions provided by the Aurora Imaging Library Graphics module allow you to draw graphics of geometric shapes or text. The [`M...Draw`](../../Reference/3dmap/M3dmapDraw.md) functions of processing and analysis modules allow you to draw module-specific features into your image.

With the functions provided by the Aurora Imaging Library Graphics module, you can draw the outline of most shapes. The width of a line forming a graphics outline (including lines and dots) is one pixel by default, and can be modified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_LINE_THICKNESS`](../../Reference/gra/MgraControl.md); however, when zooming graphics drawn in an image, the outline size can appear larger than its set value. Graphics can be rotated, filled, and drawn multiple times.

| Examples of Aurora Imaging Library graphics |
| --- |
| *[Image: graphicLines.png]* [`MgraLine`](../../Reference/gra/MgraLine.md) and [`MgraLines`](../../Reference/gra/MgraLines.md) | *[Image: graphicPolyline.png]* [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYLINE`](../../Reference/gra/MgraLines.md) | *[Image: graphicPolygon.png]* [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYGON`](../../Reference/gra/MgraLines.md) | *[Image: graphicPolygonFilled.png]* [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYGON`](../../Reference/gra/MgraLines.md) + [`M_FILLED`](../../Reference/gra/MgraLines.md) |
| *[Image: graphicVectors.png]* [`MgraVectors`](../../Reference/gra/MgraVectors.md) and [`MgraVectorsGrid`](../../Reference/gra/MgraVectorsGrid.md) | *[Image: graphicArc.png]* [`MgraArc`](../../Reference/gra/MgraArc.md) | *[Image: graphicArcFill.png]* [`MgraArcFill`](../../Reference/gra/MgraArcFill.md) | *[Image: graphicArcAngle.png]* [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md) with [`M_CONTOUR`](../../Reference/gra/MgraArcAngle.md) |
| *[Image: graphicArcAngleSector.png]* [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md) with [`M_SECTOR`](../../Reference/gra/MgraArcAngle.md) | *[Image: graphicRect.png]* [`MgraRect`](../../Reference/gra/MgraRect.md) | *[Image: graphicRectFill.png]* [`MgraRectFill`](../../Reference/gra/MgraRectFill.md) | *[Image: graphicRectAngle.png]* [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md) |
| *[Image: graphicRectAngleFill.png]* [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md) with [`M_FILLED`](../../Reference/gra/MgraRectAngle.md) | *[Image: graphicDot.png]* [`MgraDot`](../../Reference/gra/MgraDot.md) | *[Image: graphicDots.png]* [`MgraDots`](../../Reference/gra/MgraDots.md) | *[Image: graphicText.png]* [`MgraText`](../../Reference/gra/MgraText.md) |

If you need complex filled-in shapes, you can draw a graphic's outline (using one of the [`Mgra...`](../../Reference/gra/MgraArc.md) functions) and use [`MgraFill`](../../Reference/gra/MgraFill.md) to fill it. [`MgraFill`](../../Reference/gra/MgraFill.md) performs a boundary-type seed fill. It fills an area of the target buffer with the current foreground color, starting from the specified seed position. Filling occurs on adjacent pixels of the same value as the original seed pixel.

*[Image: FILL.png]*

Note that text is a unique type of graphic. For more information, see [Writing text](S05_Writing_text.md).

Processing and analysis modules (such as Bead or Edge Finder) have a function that can draw specific settings or results into an image buffer or 2D graphics list. Each draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)) can draw into an image buffer without restriction, but some restrictions exist when drawing in a 2D graphics list.

> **Note:** When drawing a graphic in an image buffer, it is drawn destructively (raster-based), changing the image's pixel values along the specified graphic. Instead of destructively drawing the graphic in an image, you can also use the same function to add the graphic to the 2D graphics list and annotate the display non-destructively. For more information, see [2D graphics list](S06_Graphics_list.md).

## Drawing vectors

You can draw vectors in an image (or add them to a 2D graphics list) by calling [`MgraVectors`](../../Reference/gra/MgraVectors.md) (to draw vectors at arbitrary positions) or [`MgraVectorsGrid`](../../Reference/gra/MgraVectorsGrid.md) (to draw vectors at equally spaced positions).

When calling [`MgraVectors`](../../Reference/gra/MgraVectors.md), you must explicitly specify the starting positions and displacements of the vectors. The following example shows how Aurora Imaging Library uses this information to draw two vectors in the destination image.

*[Image: MgraVectorsExample.png]*

When calling [`MgraVectorsGrid`](../../Reference/gra/MgraVectorsGrid.md), you must specify two image buffers: one containing pixel values corresponding to the vectors' X-displacements, and the other containing pixel values corresponding to the vectors' Y-displacements. You must also specify a stride value, to establish the pixels for which to draw a vector. If you set the stride to 1, a vector is drawn in the destination image for every position, according to that position's corresponding X- and Y-displacements.

*[Image: MgraVectorsGridExampleStride1.png]*

By default, vectors are not drawn for pixels whose displacements are both 0. Vectors that extend past the size of the destination image are clipped.

You can consider [`MgraVectorsGrid`](../../Reference/gra/MgraVectorsGrid.md) a grid type drawing because it is based on every position (pixel) in the image. Although you can draw a vector for every position (by setting the stride to 1), the result is often unintelligible (unless the displacement buffers have a significant number of regularly distributed positions with no displacement). Increasing the stride helps regulate the vectors drawn.

In the following example, with the stride set to 2, a vector is drawn in the destination image for every second position, in the X-direction and in the Y-direction, according to the corresponding displacements (highlighted by a gray box). Aurora Imaging Library always draws a vector for the first position (0,0), and applies the stride from there.

*[Image: MgraVectorsGridExampleStride2.png]*

Typically, the displacement buffers come from the result of a previous image processing operation. For example, the vectors in the following image show the gradient of an edge detection done with [`MimConvolve`](../../Reference/im/MimConvolve.md).

*[Image: MgraVectorsGridExampleFromEdgeDetect.png]*

For more information about drawing vectors from an image processing result, refer to _MgraVectors.cpp_ in Aurora Imaging Example Launcher.

## Drawing direction

You can draw an arrow pointing in the direction with which graphics were defined. This only affects graphics that can have such a direction; for example, arc, ellipse, line, rectangle, and ring. Aurora Imaging Library ignores this setting for other graphics. Aurora Imaging Library does not draw the direction arrow if the graphic is filled. This setting does not affect drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)). This affects graphics drawn destructively (raster-based), as well as graphics added to a [2D graphics list](S06_Graphics_list.md) (vector-based version), which can then be used to non-destructively annotate a display.

The direction arrow can be drawn in the primary and secondary direction of the graphic. For example, if you draw the primary direction for a rectangle defined with [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md), the arrow will point in the angular direction set with the [`Angle`](../../Reference/gra/MgraRectAngle.md) parameter. The following table lists the primary and secondary direction for the graphics.

| Graphic | Primary direction | Secondary direction |
| --- | --- | --- |
| Rectangle | For [`MgraRect`](../../Reference/gra/MgraRect.md), the primary direction is established by the angle that results from the rectangle's definition. For [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md), the primary direction is established by the rotation parameter setting ([`Angle`](../../Reference/gra/MgraRectAngle.md)). | For [`MgraRect`](../../Reference/gra/MgraRect.md) and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md), the secondary direction is equal to the `_PrimaryDirection_ - 90°`. |
| Arc | For [`MgraArc`](../../Reference/gra/MgraArc.md) and [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md), the primary direction is established by the arc's definition, and is taken counter-clockwise along its tangent (tangential). For [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md), the arc's definition includes a rotation parameter setting ([`XAxisAngle`](../../Reference/gra/MgraArcAngle.md)). | For [`MgraArc`](../../Reference/gra/MgraArc.md) and [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md), the secondary direction is from the arc's center to its radius (radial). |
| Line, polyline, and polygon | For [`MgraLine`](../../Reference/gra/MgraLine.md) and [`MgraLines`](../../Reference/gra/MgraLines.md) (for lines and polylines), the primary direction is from the line or polyline's start point to its end point. For [`MgraLines`](../../Reference/gra/MgraLines.md) (when defining polygons), the primary direction is from the start point to the end point of the polygon's last segment. | For [`MgraLine`](../../Reference/gra/MgraLine.md) and [`MgraLines`](../../Reference/gra/MgraLines.md) (for lines, polylines, and polygons), the secondary direction is equal to - 90° from the `_PrimaryDirection_.` |

## Drawing graphics in a calibrated image

When a graphic is drawn, position, dimension, and angle information is interpreted according to the unit of measurement set using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). If part of the graphic falls outside of the specified image buffer, that part is clipped off.

A graphic defined in world units does not necessarily respect the graphic shape when viewed in pixel units, if there is distortion. For example, a straight-edge rectangle in world units might have arced edges when converted to pixel units due to spatial distortion. There are three different ways to manage graphic conversion from world to pixel units when distortion is present. To draw the graphic following distortion, the graphic can be converted respecting the camera calibration information exactly using [`MgraControl`](../../Reference/gra/MgraControl.md) (or [`MgraControlList`](../../Reference/gra/MgraControlList.md)) with [`M_GRAPHIC_CONVERSION_MODE`](../../Reference/gra/MgraControl.md) set to [`M_RESHAPE_FOLLOWING_DISTORTION`](../../Reference/gra/MgraControl.md). Alternatively, Aurora Imaging Library can convert only the graphic's key points or features (for example, intersection-points, center-point or radius) using the camera calibration information, with [`M_GRAPHIC_CONVERSION_MODE`](../../Reference/gra/MgraControl.md) set to [`M_RESHAPE_FROM_POINTS`](../../Reference/gra/MgraControl.md); the remainder of the graphic is drawn from these points, ignoring any non-linear distortion. This is the default mode. It is the fastest and is accurate as long as there is only linear distortion. If you require a dot or straight-edge rectangle with 90 degree angles drawn, ignoring all distortion, use [`M_GRAPHIC_CONVERSION_MODE`](../../Reference/gra/MgraControl.md) set to [`M_PRESERVE_SHAPE_AVERAGE`](../../Reference/gra/MgraControl.md).

*[Image: ConversionMode_ReshapeFollowingDistortion.png]*

*[Image: ConversionMode_ReshapeFromPoints.png]*

*[Image: ConversionMode_PreserveShapeAverage.png]*

## Drawing graphics with offset and zoom

Using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md), you can offset the position of graphics in the destination image by a specified amount in pixels. The offset values are subtracted from the specified position of the graphic when it is rendered (because you are specifying where the old location is with respect to the new location). In addition, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md), you can have a graphic appear zoomed when rendered. The specified zoom factors will enlarge or shrink the apparent dimensions of a graphic in the X- and Y-direction.

Any portion of a graphic that falls outside of the destination image when offset or zoomed is not drawn. This is particularly useful when drawing the results of a processing or analysis operation, using most of the [`M...Draw`](../../Reference/3dmap/M3dmapDraw.md) functions. This allows you to draw a subset of positional results and settings (for example, the edge map of a region of an image) at a required magnification and starting from a required location in a destination image. For example, the following image shows the edge map of a region of a source image, beginning at the point (X draw-offset, Y draw-offset) in the source image's coordinate system, and being drawn and zoomed by a factor of 3 in both X- and Y- directions, in a destination image buffer using [`MedgeDraw`](../../Reference/edge/MedgeDraw.md).

*[Image: medgezooming.png]*

[`M_DRAW_OFFSET_...`](../../Reference/gra/MgraControl.md) and [`M_DRAW_ZOOM_...`](../../Reference/gra/MgraControl.md) are also useful to draw results, obtained from a child buffer, as if they were obtained from an operation on its parent buffer. For example, the following image shows results obtained from a child buffer drawn in a parent buffer, with and without offset.

*[Image: medgezooming_child.png]*

> **Note:** Since the offsets are subtracted from the position at which to draw in the destination, this example requires [`M_DRAW_OFFSET_...`](../../Reference/gra/MgraControl.md) be set to negative values to draw the edges at the right location in the destination image.
