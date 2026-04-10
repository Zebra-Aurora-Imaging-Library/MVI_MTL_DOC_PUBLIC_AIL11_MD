---
doctype: Reference
module: mod
function: MmodFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodFree"
---

# MmodFree

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

> Free a Model Finder context or a result buffer.

## Syntax

```c
void MmodFree(
    AIL_ID ObjectId  //in
)
```

## Description

This function deletes the specified Model Finder context (and all its models) or result buffer identifier, and releases any memory associated with it. Note that to only delete individual models in the context, use [`MmodDefine`](../../Reference/mod/MmodDefine.md).

All Model Finder contexts and all result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/mod/MmodAlloc.md) was specified during allocation.

## Parameters

### `ObjectId` *(in, AIL_ID)*

Specifies the identifier of the Model Finder context or result buffer to free. These must have been successfully allocated (with [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) or [`MmodAllocResult`](../../Reference/mod/MmodAllocResult.md)) prior to calling this function.
