---
doctype: Reference
module: agm
function: MagmFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmFree"
---

# MagmFree

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

> Free an AGM context or result buffer.

## Syntax

```c
void MagmFree(
    AIL_ID ContextOrResultAgmId  //in
)
```

## Description

This function deletes the specified AGM context (and its model) or result buffer identifier, and releases any memory associated with it. Note that to only delete individual models in the context, use [`MagmDefine`](../../Reference/agm/MagmDefine.md).

All AGM contexts and result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/agm/MagmAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResultAgmId` *(in, AIL_ID)*

Specifies the identifier of the AGM context or result buffer to free. These must have been successfully allocated (with [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) or [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md)) prior to calling this function.
