---
doctype: Reference
module: bead
function: MbeadDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadDraw"
---

# MbeadDraw

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

> Draw specific features of a bead context or of a bead result.

## Syntax

```c
void MbeadDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ContextOrResultBeadId,   //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 DrawOperation,           //in
    AIL_INT64 DrawOption,              //in
    AIL_INT   LabelOrIndex,            //in
    AIL_INT   Index,                   //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features of a selected bead context or bead result, in the specified destination image buffer or 2D graphics list. Drawings are typically performed using either the template's vertices or trained points (when specifying a bead context), or using the trained points' corresponding measured point (when specifying a bead result). Drawing operations for trained points are only available after calling [`MbeadTrain`](../../Reference/bead/MbeadTrain.md), while drawing operations for bead results are only available after calling [`MbeadVerify`](../../Reference/bead/MbeadVerify.md).

You can draw results and settings, obtained relative to an offset, at the top-left corner of the destination image, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md) and zoom them using [`MgraControl`](../../Reference/gra/MgraControl.md) with[`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md). For example, you can draw a zoomed section of the bead found in the target image at the top-left corner of the destination image. For more information, see [Drawing with an offset and scale](../../UserGuide/C26_Generating_graphics/S04_Drawing_graphics.md).

All operations are available for drawing in either the specified destination image buffer or 2D graphics list, unless otherwise specified.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResultBeadId` *(in, AIL_ID)*

Specifies the identifier of the bead context or bead result buffer that this function will use to perform the drawing operation. The context or result buffer must have been previously allocated on the system using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md) or [`MbeadAllocResult`](../../Reference/bead/MbeadAllocResult.md), respectively.

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `DrawOperation` *(in, AIL_INT64)*

Specifies the drawing operation to perform.

*Drawing operations for a context or result*

| Value | Description |
| --- | --- |
| `M_DRAW_INDEX` | Draws the index of the templates containing the points that fulfill the conditions of the [`DrawOption`](../../Reference/bead/MbeadDraw.md) parameter. For example, if you set the [`DrawOption`](../../Reference/bead/MbeadDraw.md) parameter to [`M_TRAINED`](../../Reference/bead/MbeadDraw.md), the index of all the successfully trained templates will be drawn. |
| `M_DRAW_LABEL` | Draws the label of the templates containing the points that fulfill the conditions of the [`DrawOption`](../../Reference/bead/MbeadDraw.md) parameter. For example, if you set the [`DrawOption`](../../Reference/bead/MbeadDraw.md) parameter to [`M_FAIL`](../../Reference/bead/MbeadDraw.md), the label of all the templates with at least one failed point will be drawn. |
| `M_DRAW_POSITION` | Draws a cross at the position of the point. |
| `M_DRAW_POSITION_INDEX` | Draws the index value of the point. If you specify [`M_USER`](../../Reference/bead/MbeadDraw.md), you can only perform this drawing operation for templates with a path that follows a polyline ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md)), otherwise you will get an error. |
| `M_DRAW_POSITION_POLYLINE` | Draws the polyline that connects the points. If the template's path follows a circle, Aurora Imaging Library draws an actual circle (smooth curve). |
| `M_DRAW_SEARCH_BOX` | Draws the search box of the point. |
| `M_DRAW_WIDTH` | Draws an H-type line (|-|) indicating the width of the bead at that point. |

*Drawing the training image*

| Value | Description |
| --- | --- |
| `M_DRAW_TRAINING_IMAGE` | Draws the training image. This operation cannot draw in a 2D graphics list. |

### `DrawOption` *(in, AIL_INT64)*

Specifies the type of points that this function will use to perform the drawing operation. If this information is not required for the specified drawing operation, set this parameter to [`M_DEFAULT`](../../Reference/bead/MbeadDraw.md).

*For performing a drawing operation with a context or a result*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

For a bead context, the default value is the same as [`M_TRAINED`](../../Reference/bead/MbeadDraw.md). For a bead result, the default value is the same as [`M_ALL`](../../Reference/bead/MbeadDraw.md). For an [`M_DRAW_TRAINING_IMAGE`](../../Reference/bead/MbeadDraw.md) operation, the default indicates that this parameter's information is not required. |

*For performing a drawing operation with a context*

| Value | Description |
| --- | --- |
| `M_TRAINED` | Specifies that Aurora Imaging Library performs the drawing operation using the trained points indicated by both [`M_TRAINED_FAIL`](../../Reference/bead/MbeadDraw.md) and [`M_TRAINED_PASS`](../../Reference/bead/MbeadDraw.md). |
| `M_TRAINED_FAIL` | Specifies that Aurora Imaging Library performs the drawing operation using all the trained points that were expected but not found, during the training phase ([`MbeadTrain`](../../Reference/bead/MbeadTrain.md)). In this case, the drawing operation uses the expected position of the trained point. |
| `M_TRAINED_PASS` | Specifies that Aurora Imaging Library performs the drawing operation using all the trained points that were established (found), during the training phase [`MbeadTrain`](../../Reference/bead/MbeadTrain.md). |
| `M_USER` | Specifies that Aurora Imaging Library performs the drawing operation using the vertices of the template.

If the template's path follows a polyline ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md)), [`M_USER`](../../Reference/bead/MbeadDraw.md) refers to the specified polyline points ([`MbeadTemplate`](../../Reference/bead/MbeadTemplate.md)). If the template's path follows a circle or segment, [`M_USER`](../../Reference/bead/MbeadDraw.md) refers to the minimum critical points that Aurora Imaging Library internally calculates based on the specified settings of the path. For a path that follows a segment, Aurora Imaging Library calculates three critical points, according to the specified start and end of the segment ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TEMPLATE_SEGMENT_...`](../../Reference/bead/MbeadControl.md)). For a path that follows a circle, Aurora Imaging Library calculates five critical points, according to the specified center and radius of the circle ([`M_TEMPLATE_CIRCLE_...`](../../Reference/bead/MbeadControl.md)).

You cannot specify [`M_USER`](../../Reference/bead/MbeadDraw.md) for an [`M_DRAW_WIDTH`](../../Reference/bead/MbeadDraw.md) operation; Aurora Imaging Library considers width a trained aspect of the bead (as is the case, for example, when you are using a training image to establish the width). |

*For performing a drawing operation with a result*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that the drawing operation will be performed using all measured points, regardless of whether they have passed ([`M_PASS`](../../Reference/bead/MbeadDraw.md)) or failed ([`M_FAIL`](../../Reference/bead/MbeadDraw.md)). For measured points that were not found, the drawing operation uses their expected position. |
| `M_FAIL` | Specifies that the drawing operation will be performed using all measured points that were expected but did not pass verification. In this case, the drawing operation uses the expected position of the measured point. |
| `M_FAIL_GAP_MAX` | Specifies that the drawing operation will be performed using all measured points that have failed verification, and whose expected position falls within the maximum gap in the bead, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_GAP_MAX_LENGTH`](../../Reference/bead/MbeadControl.md). |
| `M_FAIL_INTENSITY_MAX` | Specifies that the drawing operation will be performed using all measured points that have failed verification because their intensity is greater than the highest intensity allowed, as specified using: ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md)) + ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_INTENSITY_DELTA_POS`](../../Reference/bead/MbeadControl.md)). |
| `M_FAIL_INTENSITY_MIN` | Specifies that the drawing operation will be performed using all measured points that have failed verification because their intensity is less than the lowest intensity allowed, as specified using: ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_INTENSITY_NOMINAL`](../../Reference/bead/MbeadControl.md)) - ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_INTENSITY_DELTA_NEG`](../../Reference/bead/MbeadControl.md)). |
| `M_FAIL_NOT_FOUND` | Specifies that the drawing operation will be performed using all measured points that have failed verification because they were not found in the target image. |
| `M_FAIL_OFFSET` | Specifies that the drawing operation will be performed using all measured points that have failed verification because their position has surpassed the offset criterion, as specified using [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_OFFSET_MAX`](../../Reference/bead/MbeadControl.md). |
| `M_FAIL_WIDTH_MAX` | Specifies that the drawing operation will be performed using all measured points that have failed verification because the width of the measured bead at their position is greater than the maximum width criterion, as specified using: ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md)) + ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_WIDTH_DELTA_POS`](../../Reference/bead/MbeadControl.md)). |
| `M_FAIL_WIDTH_MIN` | Specifies that the drawing operation will be performed using all measured points that have failed verification because the width of the measured bead at their position is less than the minimum width criterion, as specified using: ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_WIDTH_NOMINAL`](../../Reference/bead/MbeadControl.md)) - ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_WIDTH_DELTA_NEG`](../../Reference/bead/MbeadControl.md)). |
| `M_PASS` | Specifies that the drawing operation will be performed using all the measured points that have passed verification. |

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the templates (one or all) that this function will use to perform the drawing operation, or specifies that you are performing a general type of drawing operation. The specified template is either in a bead context or result buffer, depending on the [`ContextOrResultBeadId`](../../Reference/bead/MbeadDraw.md) parameter setting. All [`LabelOrIndex`](../../Reference/bead/MbeadDraw.md) parameter values can be used for any draw operation, unless otherwise specified.

*For specifying the template*

| Value | Description |
| --- | --- |
| `M_TEMPLATE_INDEX` | Specifies the index of a template for which to perform the drawing operation. |
| `M_TEMPLATE_LABEL` | Specifies the label of a template, for which to perform the drawing operation. |
| `M_ALL` | Specifies to perform the drawing operation for all templates. |
| `M_GENERAL` | Specifies that you are performing a general type of drawing operation. You can only use [`M_GENERAL`](../../Reference/bead/MbeadDraw.md) with [`M_DRAW_TRAINING_IMAGE`](../../Reference/bead/MbeadDraw.md). |

### `Index` *(in, AIL_INT)*

Specifies the set of template points (one or all) for which to perform the drawing operation, or specifies that you are performing a general type of drawing operation. All [`Index`](../../Reference/bead/MbeadDraw.md) parameter values can be used for any draw operation (except [`M_DRAW_TRAINING_IMAGE`](../../Reference/bead/MbeadDraw.md)).

*For specifying the index of the trained point*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to perform the drawing operation for all points in the specified templates. |
| `M_GENERAL` | Specifies that you are performing a general type of drawing operation. In this case, the entire template is either drawn or not, depending on whether it contains one or more points that meet the conditions of the drawing operation. You must specify [`M_GENERAL`](../../Reference/bead/MbeadDraw.md) when using [`M_DRAW_TRAINING_IMAGE`](../../Reference/bead/MbeadDraw.md). |
| `Value >= 0` | Specifies the index of the point in the specified templates for which to perform the drawing operation. The index is from 0 (inclusive) to the total number of points in the template minus 1. To get the number of trained points in the template, use [`MbeadGetResult`](../../Reference/bead/MbeadGetResult.md) with [`M_NUMBER`](../../Reference/bead/MbeadGetResult.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether Aurora Imaging Library performs drawings with fail-type points differently than drawings with other types of points.

*For specifying whether Aurora Imaging Library performs drawings with fail-type points differently*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library does not perform drawings with fail-type points differently. |
| `M_ENHANCED` | Specifies that Aurora Imaging Library performs drawings with fail-type points differently than drawings with other types of points. That is, when possible, drawing operations ([`M_DRAW_...`](../../Reference/bead/MbeadDraw.md)) with [`M_..._FAIL`](../../Reference/bead/MbeadDraw.md) or [`M_FAIL_...`](../../Reference/bead/MbeadDraw.md) points, such as [`M_TRAINED_FAIL`](../../Reference/bead/MbeadDraw.md) and [`M_FAIL_NOT_FOUND`](../../Reference/bead/MbeadDraw.md), will be visually distinguishable from drawing operations with other types of points, such as [`M_USER`](../../Reference/bead/MbeadDraw.md), [`M_TRAINED_PASS`](../../Reference/bead/MbeadDraw.md), and [`M_PASS`](../../Reference/bead/MbeadDraw.md).

This can be useful when drawing both fail-type and pass-type points, either by calling [`MbeadDraw`](../../Reference/bead/MbeadDraw.md) multiple times, or by performing the drawing operation on all points ([`M_TRAINED`](../../Reference/bead/MbeadDraw.md) and [`M_ALL`](../../Reference/bead/MbeadDraw.md)). For example, if you specify [`M_DRAW_POSITION_INDEX`](../../Reference/bead/MbeadDraw.md) and [`M_ALL`](../../Reference/bead/MbeadDraw.md), the index of each trained point is drawn, if its associated measured point has been found (pass); for measured points that were expected but not found (fail), the index of their associated trained point is drawn with a horizontal line above it.

You cannot use [`M_ENHANCED`](../../Reference/bead/MbeadDraw.md) when drawing the training image ([`M_DRAW_TRAINING_IMAGE`](../../Reference/bead/MbeadDraw.md)). |

This value is only supported for stripe-beads.
