---
doctype: Reference
module: meas
function: MmeasDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasDraw"
---

# MmeasDraw

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

> Draw expected marker characteristics or the characteristics of marker occurrences, or draw calculation results, in the destination image buffer or 2D graphics list.

## Syntax

```c
void MmeasDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    MarkerOrResultMeasId,    //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   Index,                   //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function performs the specified drawing operation on the expected marker characteristics (as set with [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md)) or the actual characteristics of the marker occurrences (as found with [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md)). This function can also perform the specified drawing operation on the result of a measurement calculation between two markers ([`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)).

This function can execute multiple drawing operations simultaneously (for example, [`M_DRAW_SEARCH_REGION`](../../Reference/meas/MmeasDraw.md) + [`M_DRAW_SEARCH_DIRECTION`](../../Reference/meas/MmeasDraw.md)). Aurora Imaging Library performs the drawing operations in the specified destination image buffer or 2D graphics list.

You can draw settings and results, obtained relative to an offset, at the top-left corner of the destination image, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md), and zoom them using [`MgraControl`](../../Reference/gra/MgraControl.md) with[`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md). For more information, see [Drawing with an offset and scale](../../UserGuide/C26_Generating_graphics/S04_Drawing_graphics.md).

Note that you can draw characteristics specified or returned in world units ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_INCLUSION_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md), [`M_MARKER_REFERENCE_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md), [`M_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md), [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md), and/or [`M_RESULT_OUTPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) set to [`M_WORLD`](../../Reference/meas/MmeasSetMarker.md)). However, if you set any of these constants to [`M_WORLD`](../../Reference/meas/MmeasSetMarker.md) and you specify to draw expected marker characteristics from a measurement marker, you must pass [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) a calibrated target image. Otherwise, the function will generate an error.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the 2D graphics context to use. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `MarkerOrResultMeasId` *(in, AIL_ID)*

Specifies the identifier of a measurement marker buffer or the identifier of a measurement result buffer. A measurement marker buffer contains both the expected marker characteristics that you specified with [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md), and the actual marker characteristics that Aurora Imaging Library found in the target image with [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md). To specify which characteristics to draw (expected or found), use the [`ControlFlag`](../../Reference/meas/MmeasDraw.md) parameter ([`M_MARKER`](../../Reference/meas/MmeasDraw.md) or [`M_RESULT`](../../Reference/meas/MmeasDraw.md)).

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. You can add the possible [`Operation`](../../Reference/meas/MmeasDraw.md) parameter values in the tables below to draw multiple characteristics at once.

*For measurement marker buffers with either*

| Value | Description |
| --- | --- |
| `M_DRAW_INCLUSION_POINT` | Draws a cross (+) at the position of the marker's inclusion point. This applies to stripe markers. To specify an inclusion point, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_INCLUSION_POINT`](../../Reference/meas/MmeasSetMarker.md). [`M_DRAW_INCLUSION_POINT`](../../Reference/meas/MmeasDraw.md) is only available with [`M_MARKER`](../../Reference/meas/MmeasDraw.md). |
| `M_DRAW_POSITION` | Draws a cross (+) at the position of the marker, for each marker occurrence. This applies to edge, stripe, circle, and point markers. For edge, stripe, and circle markers, the found position is drawn ([`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md)); you must specify [`M_RESULT`](../../Reference/meas/MmeasDraw.md). For point markers, the defined position is drawn ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md)), although you can specify either [`M_MARKER`](../../Reference/meas/MmeasDraw.md) or [`M_RESULT`](../../Reference/meas/MmeasDraw.md). |
| `M_DRAW_SEARCH_DIRECTION` | Draws an arrow starting from the middle of the marker's box search region and pointing towards the direction of the edge transition. This applies to edge and stripe markers; in this case, the search direction is based on the marker's orientation ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_ORIENTATION`](../../Reference/meas/MmeasSetMarker.md)). |
| `M_DRAW_SEARCH_REGION` | Draws the marker's search region. This applies to edge, stripe, and circle markers. |
| `M_DRAW_SEARCH_REGION_CENTER` | Draws the center of the marker's search region. This applies to edge, stripe, and circle markers. |

*For measurement marker buffers and*

| Value | Description |
| --- | --- |
| `M_DRAW_EDGES` | Draws a line along the found edges of the marker. This applies to edge, stripe, and circle markers.

For circle markers, the fitted circle is drawn, along with the found radius and position. |
| `M_DRAW_EDGES_PROFILE` | Draws the first derivative of the intensity profile (edgevalue) from which the marker was established. This applies to edge and stripe markers. |
| `M_DRAW_EDGEVALUE_MIN_IN_PROFILE` | Draws the threshold above which a grayscale variation (edgevalue) can be considered an edge. This is a graphical representation of [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md). This applies to edge and stripe markers. |
| `M_DRAW_EDGEVALUE_PEAK_WIDTH_IN_PROFILE` | Draws a horizontal line, beginning and ending with arrows (for example, &lt;--->), to indicate the width of the edge peak of an edge or stripe marker. This is a graphical representation of [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_EDGEVALUE_PEAK_WIDTH`](../../Reference/meas/MmeasGetResult.md). This applies to edge and stripe markers. For stripe markers, the width of both its outermost edges is drawn, unless you specify [`M_EDGE_FIRST`](../../Reference/meas/MmeasDraw.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasDraw.md). |
| `M_DRAW_POSITION_IN_PROFILE` | Draws a vertical bar at the position of the marker within its profile. This applies to edge and stripe markers. |
| `M_DRAW_SPACING` | Draws an H-type line (|-|) indicating the found spacing. This applies to multiple-occurrence markers (edge and stripe). |
| `M_DRAW_SUB_POSITIONS` | Draws a cross (+) at the found positions of the subedges. This applies to edge, stripe, and circle markers. |
| `M_DRAW_WIDTH` | Draws an H-type line (|-|) to indicate the width of the edge peak of an edge or stripe marker. This is a graphical representation of [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_EDGE_WIDTH`](../../Reference/meas/MmeasGetResult.md). This applies to stripe markers only. |

*For measurement result buffers and*

| Value | Description |
| --- | --- |
| `M_DRAW_LINE` | Draws the line that joins the two markers specified with [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md). |

*For scaling down the specified drawing (edge and stripe markers only)*

| Value | Description |
| --- | --- |
| `M_DRAW_IN_BOX` | Performs the specified drawing operation (for example, [`M_DRAW_EDGES_PROFILE`](../../Reference/meas/MmeasDraw.md)), except scaled down (resized) to fit within the dimensions of the box search region. If you do not use this combination value, the same drawing operation is performed (for example, the edge profile is drawn), but sized to fit the dimensions of the destination image buffer. |

*For specifying the edge for which to perform the drawing (stripe markers only)*

| Value | Description |
| --- | --- |
| `M_EDGE_FIRST` | Draws the specified characteristic for the first outermost edge of a stripe. The first edge of a stripe is the edge closest to the search region's origin. |
| `M_EDGE_SECOND` | Draws the specified characteristic for the second outermost edge of a stripe. |

### `Index` *(in, AIL_INT)*

Specifies the index of the result to draw from a measurement marker buffer or from a measurement result buffer. When drawing the expected characteristics of a marker ([`M_MARKER`](../../Reference/meas/MmeasDraw.md)), Aurora Imaging Library does not need any index information and you must set this parameter to [`M_DEFAULT`](../../Reference/meas/MmeasDraw.md).

*For specifying the index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_RESULT_ALL_OCCURRENCES`](../../Reference/meas/MmeasDraw.md), when drawing the results of a measurement marker buffer or a measurement result buffer.

When drawing the expected characteristics of a marker, this parameter must be set to [`M_DEFAULT`](../../Reference/meas/MmeasDraw.md), which specifies that the index information is not needed. Note that when drawing the expected characteristics of a marker that is actually a multiple-occurrence marker, only the first instance of the marker is drawn. |
| `M_RESULT_PER_SUBREGION` | Specifies that the operation will be performed for a measurement result buffer, according to the specified subregions (one or all) of the specified occurrences (one or all).

You can only use **M_RESULT_PER_SUBREGION()** with edge and stripe markers for the following operations: [`M_DRAW_SEARCH_DIRECTION`](../../Reference/meas/MmeasDraw.md), [`M_DRAW_SEARCH_REGION`](../../Reference/meas/MmeasDraw.md), [`M_DRAW_SEARCH_REGION_CENTER`](../../Reference/meas/MmeasDraw.md), [`M_DRAW_EDGES_PROFILE`](../../Reference/meas/MmeasDraw.md), and [`M_DRAW_EDGEVALUE_MIN_IN_PROFILE`](../../Reference/meas/MmeasDraw.md). |
| `M_RESULT_ALL_OCCURRENCES` | Specifies that the operation will be performed for all results. |
| `Value` | Specifies the index of the result to draw. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to perform the drawing operation on the expected marker characteristics, the actual marker characteristics found in the target image, or the results of the measurement calculation. This parameter must be set to one of the values below.

*For specifying whether to use the expected characteristics, the found characteristics, or the calculation results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_RESULT`](../../Reference/meas/MmeasDraw.md). |
| `M_MARKER` | Specifies to use the expected characteristics of a marker (as set with [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md)). In this case, the [`MarkerOrResultMeasId`](../../Reference/meas/MmeasDraw.md) parameter must be set to the identifier of a measurement marker buffer and the [`Index`](../../Reference/meas/MmeasDraw.md) parameter must be set to [`M_DEFAULT`](../../Reference/meas/MmeasDraw.md). |
| `M_RESULT` | Specifies to use the actual marker characteristics located in the target image ([`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md)), or the result of the measurement calculation ([`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)). In this case, the [`MarkerOrResultMeasId`](../../Reference/meas/MmeasDraw.md) parameter must be set to the identifier of either a measurement marker buffer or a measurement result buffer. |

If you are drawing in a 2D graphics list, you must add [`M_DRAW_IN_BOX`](../../Reference/meas/MmeasDraw.md) to this operation.
