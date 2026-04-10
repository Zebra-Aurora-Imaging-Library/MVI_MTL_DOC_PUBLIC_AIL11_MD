---
doctype: Reference
module: mod
function: MmodDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodDraw"
---

# MmodDraw

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

> Draw specific features of models or result occurrences in an image buffer or 2D graphics list.

## Syntax

```c
void MmodDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ContextOrResultModId,    //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   Index,                   //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features of models or result occurrences in the destination image buffer or 2D graphics list.

You can draw results and settings, obtained relative to an offset, at the top-left corner of the destination image, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md) and zoom them using [`MgraControl`](../../Reference/gra/MgraControl.md) with[`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md). For more information, see [Drawing with an offset and scale](../../UserGuide/C26_Generating_graphics/S04_Drawing_graphics.md).

*[Image: MmodDrawZooming.png]*

When zooming, [`MmodDraw`](../../Reference/mod/MmodDraw.md) will draw into the destination image buffer even if the buffer is not large enough to contain all of the zoomed image. The image will be truncated.

Note that if you try to draw a model's edges and its Model Finder context has not been preprocessed, a preliminary edge extraction operation will be performed for the model.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResultModId` *(in, AIL_ID)*

Specifies the Model Finder context or result buffer from which to extract the features to draw. The Model Finder context or result buffer must have been previously allocated using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) or [`MmodAllocResult`](../../Reference/mod/MmodAllocResult.md), respectively.

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. Operations can be added together to draw multiple features at a time. For example, to draw both the result occurrence's position and active edges, you would set the [`Operation`](../../Reference/mod/MmodDraw.md) parameter to [`M_DRAW_POSITION`](../../Reference/mod/MmodDraw.md)+[`M_DRAW_EDGES`](../../Reference/mod/MmodDraw.md). The [`Operation`](../../Reference/mod/MmodDraw.md) parameter can be set to a combination of the following values:

*For specifying an operation that can extract features from a model context only*

| Value | Description |
| --- | --- |
| `M_DRAW_DONT_CARE` | Draws the model's "don't care" pixels. |
| `M_DRAW_FLAT_REGIONS` | Draws the model's "flat regions". |
| `M_DRAW_IMAGE` | Draws the model image. |
| `M_DRAW_WEIGHT_REGIONS` | Draws the model's weighted region mask. |

*For specifying an operation that can extract features from a result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_ACTIVE_EDGELS` | Draws the active edgels of the target. To set the degree by which Aurora Imaging Library considers edgels in the target active, use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ACTIVE_EDGELS`](../../Reference/mod/MmodControl.md).

When using [`M_DRAW_ACTIVE_EDGELS`](../../Reference/mod/MmodDraw.md), you must set the [`Index`](../../Reference/mod/MmodDraw.md) parameter to [`M_GENERAL`](../../Reference/mod/MmodDraw.md); also, the find operation must have been performed with [`M_SAVE_TARGET_EDGES`](../../Reference/mod/MmodControl.md) set to [`M_ENABLE`](../../Reference/mod/MmodControl.md).

For each model, Aurora Imaging Library establishes one set of active edgels in the target. For multi-model contexts, [`M_DRAW_ACTIVE_EDGELS`](../../Reference/mod/MmodDraw.md) draws all the edgels that have contributed to at least one model.

You can only use [`M_DRAW_ACTIVE_EDGELS`](../../Reference/mod/MmodDraw.md) with a Model Finder result buffer whose results were generated using a geometric Model Finder context ([`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md)). |

*For specifying an operation that can extract features to draw from both a model context and a result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_BOX` | Draws the model box, or a bounding box around the occurrence. Note that when drawing an occurrence, the bounding box is drawn maintaining the angle and scale of the occurrence. |
| `M_DRAW_EDGES` | Draws the edges related to the model or result.

When working with a model context, Aurora Imaging Library draws the active edges of the model (not masked out).

When working with result occurrences, the drawing depends on whether you are adding [`M_MODEL`](../../Reference/mod/MmodDraw.md) or [`M_TARGET`](../../Reference/mod/MmodDraw.md) to [`M_DRAW_EDGES`](../../Reference/mod/MmodDraw.md). |
| `M_DRAW_POSITION` | Draws a cross-like symbol at the model's reference axis origin or occurrence position. The cross is drawn maintaining the angle of the model's reference axis or the occurrence. |

*For*

| Value | Description |
| --- | --- |
| `M_MODEL` *(default)* | Draws the active edges of the model transformed at the occurrence position. |
| `M_TARGET` | Draws the edges of the target in the region of the occurrence. If the [`Index`](../../Reference/mod/MmodDraw.md) parameter is set to [`M_GENERAL`](../../Reference/mod/MmodDraw.md), then all the edges of the target are drawn. To draw the edges of the target, the find operation must have been performed with [`M_SAVE_TARGET_EDGES`](../../Reference/mod/MmodControl.md) set to [`M_ENABLE`](../../Reference/mod/MmodControl.md). |

### `Index` *(in, AIL_INT)*

Specifies the index of the model in the specified Model Finder context, or of the result occurrence. User labels cannot be used as indices; to retrieve the index of a model from its user label, use [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_INDEX_FROM_LABEL`](../../Reference/mod/MmodInquire.md).

*For specifying the index of the model or of the result occurrence*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Refers to index 0 for models and [`M_ALL`](../../Reference/mod/MmodDraw.md) for results. |
| `M_ALL` | Draws the specified feature of all models or all occurrences. |
| `M_GENERAL` | Draws the specified feature of the entire target. [`M_GENERAL`](../../Reference/mod/MmodDraw.md) can only be specified when using [`M_DRAW_EDGES`](../../Reference/mod/MmodDraw.md) + [`M_TARGET`](../../Reference/mod/MmodDraw.md) or [`M_DRAW_ACTIVE_EDGELS`](../../Reference/mod/MmodDraw.md). |
| `Value >= 0` | Specifies the model index or the result occurrence. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies where in the destination image buffer to draw.

*For specifying where to draw*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Draws at the center of the top-left pixel of the destination image buffer; or, if drawing in a 2D graphics list, from the center of the top-left pixel of the image used at the time of annotation.

When drawing features of a result occurrence, this parameter must be set to [`M_DEFAULT`](../../Reference/mod/MmodDraw.md). |
| `M_ORIGINAL` | Draws at the offsets used to define the model region in the model source. If [`M_ORIGINAL`](../../Reference/mod/MmodDraw.md) is selected, the drawing is done at the original position of the model.

[`M_ORIGINAL`](../../Reference/mod/MmodDraw.md) is only supported for image-type or Edge Finder-type models. |

This operation is not supported for a model defined in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.
