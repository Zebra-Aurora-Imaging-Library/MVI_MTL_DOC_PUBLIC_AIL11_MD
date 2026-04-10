---
doctype: Reference
module: obj
function: MobjFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / obj / MobjFree"
---

# MobjFree

> Free an object.

## Syntax

```c
void MobjFree(
    AIL_ID ObjId  //in
)
```

## Description

This function frees an object previously allocated using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md).

All objects allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/obj/MobjAlloc.md) was specified during allocation.

## Parameters

### `ObjId` *(in, AIL_ID)*

Specifies the identifier of the object.
