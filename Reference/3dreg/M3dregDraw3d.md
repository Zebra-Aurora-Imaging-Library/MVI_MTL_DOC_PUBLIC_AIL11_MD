---
doctype: Reference
module: 3dreg
function: M3dregDraw3d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregDraw3d"
---

# M3dregDraw3d

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

> Draw the result of a pairwise 3D registration operation into a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dregDraw3d(
    AIL_ID    OperationDraw3dContext3dregId,  //in
    AIL_ID    SrcResult3dregId,               //in
    AIL_INT   Index,                          //in
    AIL_INT   IterationIndex,                 //in
    AIL_INT   PairsRank,                      //in
    AIL_ID    DstList3dgraId,                 //out
    AIL_INT64 DstParentLabel,                 //in
    AIL_INT64 ControlFlag                     //in
)
```

## Description

This function draws the result of a pairwise 3D registration operation, stored in the specified pairwise 3D registration result buffer, into a 3D graphics list. Set the draw operations and options for the draw using [`M3dregControlDraw`](../../Reference/3dreg/M3dregControlDraw.md).

> **Note:** To ensure that the result buffer will hold the required information for the draw, prior to calling [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) you must specify to save pairs information at each iteration, using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_SAVE_PAIRS_INFO`](../../Reference/3dreg/M3dregControl.md) set to [`M_TRUE`](../../Reference/3dreg/M3dregControl.md).

[`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) must be called before calling this function.

The specified registration result element's point cloud is drawn relative to the coordinate system specified using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_MERGE_LOCATION`](../../Reference/3dreg/M3dregControl.md).

## Parameters

### `OperationDraw3dContext3dregId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D registration context that specifies the features to draw and how to draw them.

*For specifying the draw 3D registration context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default draw 3D registration context with all draw context control types ([`M3dregControlDraw`](../../Reference/3dreg/M3dregControlDraw.md)) set to their default. |
| `Draw 3D registration context identifier` | Specifies a valid draw 3D registration context identifier, previously allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dreg/M3dregAlloc.md). |

### `SrcResult3dregId` *(in, AIL_ID)*

Specifies the identifier of the pairwise 3D registration result buffer from which to retrieve calculated results. The result buffer must have been previously allocated using [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md) with [`M_PAIRWISE_REGISTRATION_RESULT`](../../Reference/3dreg/M3dregAllocResult.md).

### `Index` *(in, AIL_INT)*

Specifies the registration result element that is associated with the point cloud whose points and/or calculated features you want to draw.

*For specifying the registration result element*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies all registration result elements. |
| `0 <= Value < M3dregInquire(SrcResult3dregId, M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the registration result element. |

### `IterationIndex` *(in, AIL_INT)*

Specifies the index of the iteration whose registration results to draw. This parameter must be set to one of the following values:

*For specifying the iteration index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INTERMEDIATE_ITERATION` | Specifies to draw the result of an intermediate iteration of the pairwise 3D registration operation.

You can specify from which intermediate iteration to draw results using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_ITERATION_INDEX`](../../Reference/3dreg/M3dregControl.md). |
| `M_LAST_ITERATION` *(default)* | Specifies to draw the result of the final iteration of the registration process. |
| `Value >= 0` | Specifies the index of the iteration.

> **Note:** If the specified index is greater than the number of elements in the pairwise 3D registration result buffer ([`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) with [`M_NB_ITERATIONS`](../../Reference/3dreg/M3dregGetResult.md)), the result drawn will be of the final iteration ([`M_LAST_ITERATION`](../../Reference/3dreg/M3dregDraw3d.md)) for that registration result element. |

### `PairsRank` *(in, AIL_INT)*

Specifies the rank of the point pairs to draw. When a reference point is paired to multiple points in the registration result element's point cloud, [`PairsRank`](../../Reference/3dreg/M3dregDraw3d.md) sets from which pairing to draw results. Note that pairings are ranked from nearest (rank 0) to farthest.

*For specifying the pairs rank*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies to draw results from all pairings in the registration result element that were calculated at the specified iteration. |
| `Value >= 0` *(default)* | Specifies the rank of the point pair from which to draw results. |

### `DstList3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to draw. You can specify a 3D graphics list that you have previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `DstParentLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in the 3D graphics list to use as the annotation's parent.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dreg/M3dregDraw3d.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the parent label of all 3D graphics that the function added to the 3D graphics list.
