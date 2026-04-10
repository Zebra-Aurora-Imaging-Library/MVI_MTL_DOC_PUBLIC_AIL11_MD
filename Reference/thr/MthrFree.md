---
doctype: Reference
module: thr
function: MthrFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / thr / MthrFree"
---

# MthrFree

> Free an Aurora Imaging Library thread context, event, or mutex.

## Syntax

```c
void MthrFree(
    AIL_ID ThreadEventorMutexId  //in
)
```

## Description

This function deallocates an Aurora Imaging Library thread context, event, or mutex previously allocated with [`MthrAlloc`](../../Reference/thr/MthrAlloc.md).

To free an Aurora Imaging Library thread allocated by [`MthrAlloc`](../../Reference/thr/MthrAlloc.md), you must call [`MthrFree`](../../Reference/thr/MthrFree.md) from another thread. [`MthrFree`](../../Reference/thr/MthrFree.md) will wait indefinitely for that thread to terminate it's execution before returning.

All Aurora Imaging Library thread contexts, events, or mutexes allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/thr/MthrAlloc.md) was specified during allocation.

## Parameters

### `ThreadEventorMutexId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library thread context/event/mutex to free.
