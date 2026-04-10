---
doctype: Reference
module: agm
function: MagmDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmDraw"
---

# MagmDraw

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

> Draw specific features of the model, result occurrence, or training image in an image buffer or 2D graphics list.

## Syntax

```c
void MagmDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ContextOrResultAgmId,    //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT64 Index,                   //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific model, result occurrence, or training image features in the destination image buffer or 2D graphics list.

Drawing the trained composite-definition model will help you determine the success of the [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation. If the trained composite-definition model does not look like the positively labeled objects in your training images, you should modify the images and repeat the training process.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResultAgmId` *(in, AIL_ID)*

Specifies the find AGM context or AGM result buffer from which to extract the features to draw. The find AGM context must have been previously allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md). The find or train AGM result buffer must have been previously allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_FIND_RESULT`](../../Reference/agm/MagmAllocResult.md) or [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md) and must contain the results of a call to [`MagmFind`](../../Reference/agm/MagmFind.md) or [`MagmTrain`](../../Reference/agm/MagmTrain.md), respectively.

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. Operations can be added together to draw multiple features at a time. For example, to draw both the result occurrence's position and edges, you would set the [`Operation`](../../Reference/agm/MagmDraw.md) parameter to [`M_DRAW_POSITION`](../../Reference/agm/MagmDraw.md)+[`M_DRAW_EDGES`](../../Reference/agm/MagmDraw.md). The [`Operation`](../../Reference/agm/MagmDraw.md) parameter can be set to a combination of the following values:

*For a find AGM context only*

| Value | Description |
| --- | --- |
| `M_DRAW_IMAGE` | Draws the model image of a single-definition or a composite-definition model. For a composite-definition model, if the image does not exist, an Aurora Imaging Library error is thrown. To inquire if the model image exists, you can use [`MagmInquire`](../../Reference/agm/MagmInquire.md) with [`M_HAS_USER_MODEL`](../../Reference/agm/MagmInquire.md).

You can use [`MagmInquire`](../../Reference/agm/MagmInquire.md) with [`M_SIZE_X`](../../Reference/agm/MagmInquire.md) and [`M_SIZE_Y`](../../Reference/agm/MagmInquire.md) to inquire the size of the model. |
| `M_DRAW_TRAINED_MODEL` | Draws the trained composite-definition model.

You can use [`MagmInquire`](../../Reference/agm/MagmInquire.md) with [`M_SIZE_X`](../../Reference/agm/MagmInquire.md) and [`M_SIZE_Y`](../../Reference/agm/MagmInquire.md) to inquire the size of the model. |

*For a train AGM result buffer only*

| Value | Description |
| --- | --- |
| `M_DRAW_NEG_RECTANGLES` | Draws all the rectangles that were used to label the negative model locations in the training image.

You can use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) set to [`M_COLOR_RED`](../../Reference/gra/MgraControl.md) to reuse these rectangles for another [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation. |
| `M_DRAW_POS_RECTANGLES` | Draws all the rectangles that were used to label the positive model locations in the training image.

Note that if the rectangles were realigned, using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_REALIGN_STRENGTH`](../../Reference/agm/MagmControl.md), the realigned rectangles are drawn.

You can use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) set to [`M_COLOR_BLUE`](../../Reference/gra/MgraControl.md) to reuse these rectangles for another [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation. |

*For specifying an operation that can extract features to draw from either a find AGM context or a find AGM result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_BOX` | Draws the model box, or a bounding box around the occurrence. Note that when drawing an occurrence, the bounding box is drawn according to the occurrence's angle and scale. Since a find AGM result buffer only contains occurrences that are similar in size to the model, the occurrence's scale is close to 1.0. You can get the exact angle and scale of the occurrence, using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_ANGLE`](../../Reference/agm/MagmGetResult.md) and [`M_SCALE`](../../Reference/agm/MagmGetResult.md). |
| `M_DRAW_EDGES` | Draws the edges related to the model or result.

When working with result occurrences, the drawing depends on whether you are adding [`M_MODEL`](../../Reference/agm/MagmDraw.md) or [`M_TARGET`](../../Reference/agm/MagmDraw.md) to [`M_DRAW_EDGES`](../../Reference/agm/MagmDraw.md).

Note that the AGM context must be preprocessed. If it is not preprocessed, an Aurora Imaging Library error is thrown. |
| `M_DRAW_POSITION` | Draws a cross-like symbol at the model's reference axis origin or occurrence position. The cross is drawn maintaining the angle of the model's reference axis or the occurrence. |

*For*

| Value | Description |
| --- | --- |
| `M_MODEL` *(default)* | Draws the used edges of the model transformed at the occurrence position.

For a composite-definition model, if [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md), the model image is used for the fit and coverage calculations and the edges of the model image are drawn. If [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_FROM_TRAIN`](../../Reference/agm/MagmControl.md), the trained composite-definition model is used for the detection, fit, and coverage calculations and the edges of the trained composite-definition model are drawn. |
| `M_TARGET` | Draws the edges of the target in the region of the occurrence.

This value is only available when [`M_USE_MAGNITUDE_TARGET`](../../Reference/agm/MagmControl.md) is set to [`M_DISABLE`](../../Reference/agm/MagmControl.md). |

### `Index` *(in, AIL_INT64)*

Specifies the index of the model in the specified find AGM context, the result occurrence in the specified find AGM result buffer, or the image in the specified train AGM result buffer.

*For specifying the index of the model, result occurrence, or image*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ALL`](../../Reference/agm/MagmDraw.md).

Note that this value is only available if a find AGM result buffer is specified. |
| `M_AGM_IMAGE_INDEX` | Specifies that the drawing operation is for a specific image in the train AGM result buffer, if one is specified. |
| `M_AGM_MODEL_INDEX` | Specifies that the drawing operation is for a specific model in the find AGM context, if one is specified. |
| `M_ALL` | Draws the specified feature of all occurrences, if a find AGM result buffer is specified. |
| `Value >= 0` | Specifies the index of the occurrence in the find AGM result buffer, if one is specified. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. this parameter must be set to `M_DEFAULT`.
