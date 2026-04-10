---
doctype: Reference
module: blob
function: MblobDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / blob / MblobDraw"
---

# MblobDraw

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

> Draw specific blob features in the destination image buffer or 2D graphics list.

## Syntax

```c
void MblobDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ResultBlobId,            //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   LabelOrIndex,            //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific blob features, which were calculated using [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), in the destination image buffer or 2D graphics list. This function can draw multiple features at a time.

You can draw results obtained relative to an offset, at the top-left corner of the destination image, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md) and zoom them using [`MgraControl`](../../Reference/gra/MgraControl.md) with[`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md). For example, you can draw a zoomed section of the blobs found in the target image at the top-left corner of the destination image. For more information, see [Drawing with an offset and scale](../../UserGuide/C26_Generating_graphics/S04_Drawing_graphics.md).

*[Image: DrawBlob.png]*

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ResultBlobId` *(in, AIL_ID)*

Specifies the identifier of the blob analysis result buffer from which to retrieve the features to draw.

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

*For specifying the type of operation*

| Value | Description |
| --- | --- |
| `M_DRAW_AXIS` | Draws a cross at the blobs' center of gravity, respecting the principal and secondary axes. Note that to perform this operation, one feature among [`M_AXIS_PRINCIPAL_ANGLE`](../../Reference/blob/MblobGetResult.md) or [`M_AXIS_SECONDARY_ANGLE`](../../Reference/blob/MblobGetResult.md) must have been calculated using the binary definition. |
| `M_DRAW_BLOBS` | Draws the blobs. Note that to perform this operation, runs must have been previously saved using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md). |
| `M_DRAW_BLOBS_CONTOUR` | Draws the external outline of the blobs.

Note that to perform this operation, runs must have been previously saved using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md).

Also note that when using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md) set to either [`M_WHOLE_IMAGE`](../../Reference/blob/MblobControl.md) or [`M_LABELED`](../../Reference/blob/MblobControl.md), both the blob and hole contours will be drawn when performing this operation. If [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md) is set to [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md), you cannot specify [`M_DRAW_BLOBS_CONTOUR`](../../Reference/blob/MblobDraw.md). |
| `M_DRAW_BOX` | Draws the image-axis aligned bounding box of each blob. The image-axis-aligned bounding box is the bounding box that is aligned with the pixel coordinate system's axes. |
| `M_DRAW_BOX_CENTER` | Draws a cross at the center of the blobs' bounding box. |
| `M_DRAW_CENTER_OF_GRAVITY` | Same as [`M_DRAW_POSITION`](../../Reference/blob/MblobDraw.md). |
| `M_DRAW_CONVEX_HULL` | Draws the convex hull of the blobs. |
| `M_DRAW_CONVEX_HULL_CONTOUR` | Draws the convex perimeter of the blobs. |
| `M_DRAW_FERET_MAX` | Draws the blobs' maximum Feret diameter, using an H-type line (|-|) that passes through the center of the blob's bounding box at the maximum Feret diameter's angle. The line's end points are tangent to the Feret diameter's contact points.

*[Image: Blob_feret_maximum.png]*

Note that to perform this operation, the following features must have been calculated: [`M_BOX`](../../Reference/blob/MblobControl.md), [`M_FERET_MAX_DIAMETER`](../../Reference/blob/MblobGetResult.md), and [`M_FERET_MAX_ANGLE`](../../Reference/blob/MblobGetResult.md). |
| `M_DRAW_FERET_MIN` | Draws the blobs' minimum Feret diameter, using an H-type line (|-|) that passes through the center of the blob's bounding box at the minimum Feret diameter's angle. The line's end points are tangent to the Feret diameter's contact points.

*[Image: Blob_feret_minimum.png]*

Note that to perform this operation, the following features must have been calculated: [`M_BOX`](../../Reference/blob/MblobControl.md), [`M_FERET_MIN_DIAMETER`](../../Reference/blob/MblobGetResult.md), and [`M_FERET_MIN_ANGLE`](../../Reference/blob/MblobGetResult.md). |
| `M_DRAW_HOLES` | Draws the holes of the blobs.

Note that to perform this operation, runs must have been previously saved using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md) and one feature among [`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md), [`M_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md), or [`M_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md) must have been calculated. |
| `M_DRAW_HOLES_CONTOUR` | Draws the outline of the blobs' holes.

Note that to perform this operation, runs must have been previously saved using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md).

Also note that when using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md) set to either [`M_WHOLE_IMAGE`](../../Reference/blob/MblobControl.md) or [`M_LABELED`](../../Reference/blob/MblobControl.md), both the blob and hole contours will be drawn when performing this operation. If [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md) is set to [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md), you cannot draw the blobs' holes with [`M_DRAW_HOLES_CONTOUR`](../../Reference/blob/MblobDraw.md). |
| `M_DRAW_MIN_AREA_BOX` | Draws the blobs' minimum-area bounding box.

Note that to perform this operation, at least one of the [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md) features must have been calculated. |
| `M_DRAW_MIN_PERIMETER_BOX` | Draws the blobs' minimum-perimeter bounding box.

Note that to perform this operation, at least one of the [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md) features must have been calculated. |
| `M_DRAW_POSITION` | Draws a cross at the center of gravity of the blobs. Note that to perform this operation, [`M_CENTER_OF_GRAVITY`](../../Reference/blob/MblobControl.md) must have been calculated using the binary definition. |
| `M_DRAW_WORLD_BOX` | Draws the world-axis aligned bounding box of each blob. The world-axis-aligned bounding box is the bounding box that is aligned with the relative coordinate system's axes.

Note that to perform this operation, the [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md) features must have been calculated. |
| `M_DRAW_WORLD_BOX_CENTER` | Draws a cross at the center of the blobs' bounding box, calculated in the relative coordinate system.

Note that to perform this operation, the [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md) features must have been calculated. |
| `M_DRAW_WORLD_FERET_X` | Draws the blobs' Feret diameter, using an H-type line (|-|), centered at the blobs' position, parallel to the X-axis of the relative coordinate system.

Note that to perform this operation, the [`M_WORLD_FERET_X`](../../Reference/blob/MblobGetResult.md) feature must have been calculated. |
| `M_DRAW_WORLD_FERET_Y` | Draws the blobs' Feret diameter, using an H-type line (|-|), centered at the blobs' position, parallel to the Y-axis of the relative coordinate system.

Note that to perform this operation, the [`M_WORLD_FERET_Y`](../../Reference/blob/MblobGetResult.md) feature must have been calculated. |

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the label or index of the blob or blobs whose information to draw.

*For specifying the label of the blob or blobs to draw*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ALL_BLOBS`](../../Reference/blob/MblobDraw.md). |
| `M_BLOB_INDEX` | Specifies the index of the blob. |
| `M_BLOB_LABEL` | Specifies the label of the blob for which to get results. You can get a list of valid blob label values with [`M_LABEL_VALUE`](../../Reference/blob/MblobGetResult.md). |
| `M_ALL_BLOBS` | Specifies all blobs. |
| `M_EXCLUDED_BLOBS` | Specifies all currently excluded blobs. |
| `M_INCLUDED_BLOBS` | Specifies all currently included blobs. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

Note that to perform this operation, the [`M_BOX`](../../Reference/blob/MblobControl.md) feature must have been calculated.

Note that to perform this operation, the [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md) feature must have been calculated, and runs must have been previously saved using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md).

When drawing in either an image buffer or a 2D graphics list, [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) cannot be set to [`M_WORLD`](../../Reference/gra/MgraControl.md) for this operation.
