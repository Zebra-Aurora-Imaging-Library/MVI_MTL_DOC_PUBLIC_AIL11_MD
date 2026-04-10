---
doctype: Reference
module: 3dblob
function: M3dblobDraw3d
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobDraw3d"
---

# M3dblobDraw3d

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

> Draw blobs and blob features into a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dblobDraw3d(
    AIL_ID    OperationDraw3dContext3dblobId,  //in
    AIL_ID    ContainerBufId,                  //in
    AIL_ID    Result3dblobId,                  //out
    AIL_INT64 LabelOrIndex,                    //in
    AIL_ID    DstList3dgraId,                  //in
    AIL_INT64 DstParentLabel,                  //in
    AIL_INT64 ControlFlag                      //in
)
```

## Description

This function draws blob points and/or features into the specified 3D graphics list. Set the draw operations and options for the draw using [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md).

Note that the specified draw 3D blob analysis result buffer must contain the result of an [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) operation, and, if you want to draw blob features, results of an [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) operation. Use [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md) to set whether to draw blob points, excluded points, and/or calculated blob features.

The resulting draw has the following structure: Top node --> Blobs --> Features. The subnodes for blobs 0, 1, …N are ordered in the 3D graphics list in the same sequence as the blob indices. If the excluded points are drawn ([`M_DRAW_EXCLUDED_POINTS`](../../Reference/3dblob/M3dblobControlDraw.md) set to [`M_ENABLE`](../../Reference/3dblob/M3dblobControlDraw.md), using [`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md)), they are the last subnode.

## Parameters

### `OperationDraw3dContext3dblobId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D blob analysis context that specifies the features to draw and how to draw them.

*For specifying the draw 3D blob analysis context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies a predefined draw 3D blob analysis context with all draw operations ([`M3dblobControlDraw`](../../Reference/3dblob/M3dblobControlDraw.md)) set to use default settings. |
| `Draw 3D blob analysis context identifier` | Specifies a valid draw 3D blob analysis context identifier, previously allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md). |

### `ContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the 3D-processable point cloud container from which to retrieve the blobs to draw. This must be the same container previously passed to [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md), and its confidence component must not have been modified since then. If the segmentation was performed using a label image instead, this can be any container as long as the size of its range component is the same as the label image.

### `Result3dblobId` *(out, AIL_ID)*

Specifies the identifier of the 3D blob analysis result buffer from which to retrieve calculated results and/or information about the blobs. The result buffer must have been previously allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md).

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies the label or index of the blob whose points and/or calculated features you want to draw, or specifies all blobs. This parameter must be set to one of the following values:

*For specifying the blob label or index*

| Value | Description |
| --- | --- |
| `M_BLOB_INDEX` | Specifies the index of the blob. |
| `M_BLOB_LABEL` | Specifies the label of the blob. |
| `M_ALL_BLOBS` | Specifies to draw the points and/or features of all blobs. |
| `0 <= Value < M_NUMBER` | Specifies the valid index of a blob. |
| `1 <= Value <= M_MAX_LABEL_VALUE` | Specifies the valid label of a blob. |

### `DstList3dgraId` *(in, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to draw. You can specify a 3D graphics list that you have previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `DstParentLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in the 3D graphics list to use as the annotation's parent.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dblob/M3dblobDraw3d.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the parent label of all 3D graphics that the function added to the 3D graphics list. If the specified 3D blob analysis result buffer does not contain a blob segmentation, a node 3D graphic is added to the graphics list and the node's label is returned.
