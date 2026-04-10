---
doctype: Reference
module: pat
function: MpatFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / pat / MpatFree"
---

# MpatFree

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

> Free a Pattern Matching context or a result buffer.

## Syntax

```c
void MpatFree(
    AIL_ID ContextOrResultPatId  //in
)
```

## Description

This function deletes the specified Pattern Matching context (and all its models) or result buffer identifier, and releases any memory associated with it. Note that, to delete individual models in the context, use [`MpatDefine`](../../Reference/pat/MpatDefine.md).

All Pattern Matching contexts or result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/pat/MpatAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResultPatId` *(in, AIL_ID)*

Specifies the identifier of the Pattern Matching context or result buffer to free. These must have been successfully allocated (with [`MpatAlloc`](../../Reference/pat/MpatAlloc.md) or [`MpatAllocResult`](../../Reference/pat/MpatAllocResult.md)) prior to calling this function.
