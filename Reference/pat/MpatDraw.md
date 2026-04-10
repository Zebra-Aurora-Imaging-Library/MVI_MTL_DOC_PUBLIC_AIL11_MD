---
doctype: Reference
module: pat
function: MpatDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / pat / MpatDraw"
---

# MpatDraw

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

> Draw specific features of the model or result occurrence in an image buffer or 2D graphics list.

## Syntax

```c
void MpatDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ContextOrResultPatId,    //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   Index,                   //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific model or result occurrence features in the destination image buffer or 2D graphics list. [`MpatDraw`](../../Reference/pat/MpatDraw.md) can be used with multiple results.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResultPatId` *(in, AIL_ID)*

Specifies the Pattern Matching context or result buffer identifier from which to extract the features to draw. The Pattern Matching context or result buffer must have been previously allocated using [`MpatAlloc`](../../Reference/pat/MpatAlloc.md) or [`MpatAllocResult`](../../Reference/pat/MpatAllocResult.md), respectively.

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list into which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. Operations can be added together to draw multiple features at a time. For example, to draw both the result occurrence's bounding box and position, you would specify [`M_DRAW_BOX`](../../Reference/pat/MpatDraw.md)+[`M_DRAW_POSITION`](../../Reference/pat/MpatDraw.md) as the [`Operation`](../../Reference/pat/MpatDraw.md) parameter. The [`Operation`](../../Reference/pat/MpatDraw.md) parameter can be set to one or a combination of the values below.

*For specifying an operation that can extract features from both a model and a result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_BOX` | Draws the bounding box of the model or of the result occurrence(s). For a result occurrence, the box is drawn at the coordinates at which its origin was found in the target image (not the reference position), respecting the occurrence's angle. |
| `M_DRAW_POSITION` | Draws a cross at the model's reference position, or at the result occurrence(s) position and angle found in the target image (with respect to the reference position). |

*For specifying an operation that can extract features to draw only from a model*

| Value | Description |
| --- | --- |
| `M_DRAW_DONT_CARE` | Draws the model's don't care pixels. |
| `M_DRAW_IMAGE` | Draws the region of the model source image from which the model was extracted. |

### `Index` *(in, AIL_INT)*

Specifies whether to draw a specific result occurrence or model, or to draw all of them.

*For specifying the index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If a Pattern Matching context is specified, the default is 0.

If a Pattern Matching result buffer is provided, the default is the same as [`M_ALL`](../../Reference/pat/MpatDraw.md). |
| `M_ALL` | Draws the specified feature of all models or all result occurrences. |
| `Value >= 0` | Specifies the model index or the result occurrence. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies where to draw a model. This parameter must be set to one of the values below.

*For specifying where to draw the model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to draw the model starting from the center of the top-left pixel of the destination image; or, if drawing in a 2D graphics list, from the center of the top-left pixel of the image used at the time of annotation. |
| `M_ORIGINAL` | Specifies to draw the model starting from the offsets used to define the model in the model source image. |
