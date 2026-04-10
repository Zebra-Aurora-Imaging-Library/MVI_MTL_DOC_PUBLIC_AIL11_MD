---
doctype: Reference
module: edge
function: MedgeDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeDraw"
---

# MedgeDraw

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Draw specific edge results in the destination image buffer or 2D graphics list.

## Syntax

```c
void MedgeDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ResultEdgeId,            //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   IndexOrLabel,            //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific edge results in the destination image buffer or 2D graphics list.

To draw edge results, the required information must be available for the specified edge or group of edges, in the Edge Finder result buffer. To verify availability, call [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with the required edge result combined with [`M_AVAILABLE`](../../Reference/edge/MedgeGetResult.md).

You can draw results and settings, obtained relative to an offset, at the top-left corner of the destination image, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md) and zoom them using [`MgraControl`](../../Reference/gra/MgraControl.md) with[`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md). For example, you can draw a zoomed section of the edge map found in the target image at the top-left corner of the destination image. For more information, see [Drawing with an offset and scale](../../UserGuide/C26_Generating_graphics/S04_Drawing_graphics.md).

*[Image: medgezooming.png]*

Note that if the edges are calculated using a calibrated source image, Edge Finder takes the camera calibration into account; that is, drawings might be distorted, according to the camera calibration. For example, a straight line (in the world) might be drawn as a curve.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ResultEdgeId` *(in, AIL_ID)*

Specifies the identifier of the Edge Finder result buffer from which to extract the results to draw. The Edge Finder result buffer must have been previously allocated on the required system using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md).

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. The possible [`Operation`](../../Reference/edge/MedgeDraw.md) parameter values in the table below can be added together to draw multiple feature results at once.

*For specifying the type of operation to perform*

| Value | Description |
| --- | --- |
| `M_DRAW_BOX` | Draws a bounding box around each edge.

Note that to perform this operation, the [`M_BOX`](../../Reference/edge/MedgeControl.md) feature in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_CENTER_OF_GRAVITY` | Draws a cross at the center of gravity of each edge.

Note that to perform this operation, the [`M_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeControl.md) feature in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_CIRCLE_FIT` | Draws the circle fit of each edge.

Note that to perform this operation, the [`M_CIRCLE_FIT`](../../Reference/edge/MedgeControl.md) feature in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_EDGE` | Draws each edge. |
| `M_DRAW_EDGELS` | Draws a cross at each edgel. |
| `M_DRAW_ELLIPSE_FIT` | Draws the ellipse fit of each edge.

Note that to perform this operation, the [`M_ELLIPSE_FIT`](../../Reference/edge/MedgeControl.md) feature in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_FERET_GENERAL` | Draws the edges' general Feret, using an H-type line (|-|). This line is drawn on contact with the edge's two extrema edgels, at the general Feret diameter's angle.

Note that to perform this operation, the [`M_FERET_GENERAL`](../../Reference/edge/MedgeControl.md) feature in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_FERET_MAX` | Draws the edges' maximum Feret, using an H-type line (|-|). This line is drawn on contact with the edge's two extrema edgels, at the maximum Feret diameter's angle.

Note that to perform this operation, the [`M_FERET_MAX_DIAMETER`](../../Reference/edge/MedgeControl.md) and [`M_FERET_MAX_ANGLE`](../../Reference/edge/MedgeControl.md) features in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_FERET_MIN` | Draws the edges' minimum Feret, using an H-type line (|-|). This line is drawn on contact with the edge's two extrema edgels, at the minimum Feret diameter's angle.

Note that to perform this operation, the [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md) and [`M_FERET_MIN_ANGLE`](../../Reference/edge/MedgeControl.md) features in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_INDEX` | Draws each edge's index value.

Note that this operation will only work for included edges. Therefore the [`IndexOrLabel`](../../Reference/edge/MedgeDraw.md) parameter cannot be set to [`M_EXCLUDED_EDGES`](../../Reference/edge/MedgeDraw.md) and can only be set to [`M_ALL_EDGES`](../../Reference/edge/MedgeDraw.md) if all edges are included. |
| `M_DRAW_LABEL` | Draws each edge's label value.

Note that to perform this operation, the [`M_LABEL_VALUE`](../../Reference/edge/MedgeControl.md) feature in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_LINE_FIT` | Draws the line fit of each edge.

Note that to perform this operation, the [`M_LINE_FIT`](../../Reference/edge/MedgeControl.md) feature in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_POSITION` | Draws a cross at the center of each edge.

Note that to perform this operation, the [`M_POSITION`](../../Reference/edge/MedgeControl.md) feature in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be calculated. |
| `M_DRAW_SEGMENTS` | Draws the segments of the edge approximation.

Note that to perform this operation, edge approximations must have been previously calculated by setting [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_CHAIN_APPROXIMATION`](../../Reference/edge/MedgeControl.md) to [`M_LINE`](../../Reference/edge/MedgeControl.md) or [`M_WORLD_LINE`](../../Reference/edge/MedgeControl.md). |
| `M_DRAW_VERTICES` | Draws the segment intersections (or vertices) of the edge approximation.

Note that to perform this operation, edge approximations must have been previously calculated by setting [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_CHAIN_APPROXIMATION`](../../Reference/edge/MedgeControl.md) to [`M_LINE`](../../Reference/edge/MedgeControl.md) or [`M_WORLD_LINE`](../../Reference/edge/MedgeControl.md). |

*For*

| Value | Description |
| --- | --- |
| `M_DRAW_VALUE` | Specifies that the operation's numerical value is drawn. For example, if you add [`M_DRAW_VALUE`](../../Reference/edge/MedgeDraw.md) to [`M_DRAW_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeDraw.md), the center of gravity's coordinates are drawn within parenthesis, and centered above the drawing cross. |

*For specifying operations that cannot use a 2D graphics list as a destination*

| Value | Description |
| --- | --- |
| `M_DRAW_ANGLE` | Draws the internal angle buffer.

Note that to perform this operation, the angle buffer must have been previously saved in the result buffer by enabling [`M_SAVE_ANGLE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md). |
| `M_DRAW_CROSS_DERIVATIVE` | Draws the internal cross-derivative buffer of the source image.

[`M_DRAW_CROSS_DERIVATIVE`](../../Reference/edge/MedgeDraw.md) is only relevant for [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. |
| `M_DRAW_FIRST_DERIVATIVE_X` | Draws the internal first derivative buffer of the source image, in the X-direction.

[`M_DRAW_FIRST_DERIVATIVE_X`](../../Reference/edge/MedgeDraw.md) is only relevant for [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. |
| `M_DRAW_FIRST_DERIVATIVE_Y` | Draws the internal first derivative buffer of the source image, in the Y-direction.

[`M_DRAW_FIRST_DERIVATIVE_Y`](../../Reference/edge/MedgeDraw.md) is only relevant for [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. |
| `M_DRAW_IMAGE` | Draws the source image.

Note that to perform this operation, the source image must have been previously saved in the result buffer by enabling [`M_SAVE_IMAGE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md). |
| `M_DRAW_MAGNITUDE` | Draws the internal magnitude buffer.

Note that to perform this operation, the magnitude buffer must have been previously saved in the result buffer by enabling [`M_SAVE_MAGNITUDE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md). |
| `M_DRAW_MASK` | Draws the mask buffer.

Note that to perform this operation, the mask buffer must have been previously saved in the result buffer by enabling [`M_SAVE_MASK`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md). |
| `M_DRAW_SECOND_DERIVATIVE_X` | Draws the internal second derivative buffer of the source image, in the X-direction.

[`M_DRAW_SECOND_DERIVATIVE_X`](../../Reference/edge/MedgeDraw.md) can only be used with [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. |
| `M_DRAW_SECOND_DERIVATIVE_Y` | Draws the internal second derivative buffer of the source image, in the Y-direction.

[`M_DRAW_SECOND_DERIVATIVE_Y`](../../Reference/edge/MedgeDraw.md) can only be used with [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. |

### `IndexOrLabel` *(in, AIL_INT)*

Specifies the edge(s) to draw. This parameter must be set to one of the values below.

*For specifying the edge(s) to draw*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_INCLUDED_EDGES`](../../Reference/edge/MedgeDraw.md). |
| `M_ALL_EDGES` | Specifies all edges. |
| `M_EXCLUDED_EDGES` | Specifies all currently excluded edges. |
| `M_INCLUDED_EDGES` | Specifies all currently included edges. |
| `Value` | Specifies either the edge's index or label. For more information, see combination values below.

Valid index values must fall within the following range: 0 to the number of included edges in the Edge Finder result buffer - 1. You can retrieve valid label values using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_LABEL_VALUE`](../../Reference/edge/MedgeGetResult.md). |

*For an index or a label value*

| Value | Description |
| --- | --- |
| `M_TYPE_INDEX` *(default)* | Specifies an edge using its index value.

Note that only included edges ([`M_INCLUDED_EDGES`](../../Reference/edge/MedgeDraw.md)) can be drawn. |
| `M_TYPE_LABEL` | Specifies an edge using its label value. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

Note that to perform this operation, the derivatives must have been previously saved in the result buffer by enabling [`M_SAVE_DERIVATIVES`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md).

You can modify the cross size by setting [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_DRAW_CROSS_SIZE`](../../Reference/edge/MedgeControl.md) to an appropriate value.
