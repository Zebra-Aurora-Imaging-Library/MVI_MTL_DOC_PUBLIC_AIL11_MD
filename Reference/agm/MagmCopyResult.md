---
doctype: Reference
module: agm
function: MagmCopyResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmCopyResult"
---

# MagmCopyResult

> Copy a trained composite-definition model from a train AGM result buffer to a find AGM context.

## Syntax

```c
void MagmCopyResult(
    AIL_ID    SrcTrainResultId,  //in
    AIL_INT64 SrcIndex,          //in
    AIL_ID    DstContextId,      //out
    AIL_INT64 DstIndex,          //in
    AIL_INT64 CopyType,          //in
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function copies a model from a train AGM result buffer to a find AGM context. You can only copy trained results after calling [`MagmTrain`](../../Reference/agm/MagmTrain.md).

You must use [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md) to add a composite-definition model to a find AGM context. Once added, you must preprocess the context before calling [`MagmFind`](../../Reference/agm/MagmFind.md).

## Parameters

### `SrcTrainResultId` *(in, AIL_ID)*

Specifies the identifier of the source result buffer from which to copy. The result buffer must have been allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md), and must contain the results of a call to [`MagmTrain`](../../Reference/agm/MagmTrain.md).

### `SrcIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DstContextId` *(out, AIL_ID)*

Specifies the identifier of the destination context in which to store the copied results. The context must have been allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md).

### `DstIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

*Specifies the type of copy operation*

| Value | Description |
| --- | --- |
| `M_TRAINED_MODEL` | Specifies to copy the trained composite-definition model from the train AGM result buffer to the find AGM context. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
