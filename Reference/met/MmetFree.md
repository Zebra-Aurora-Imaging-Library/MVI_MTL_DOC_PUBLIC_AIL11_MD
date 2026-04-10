---
doctype: Reference
module: met
function: MmetFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetFree"
---

# MmetFree

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

> Free a metrology context, a derived metrology region object, or a metrology result buffer.

## Syntax

```c
void MmetFree(
    AIL_ID MetId  //in
)
```

## Description

This function deletes the specified metrology context, derived metrology region object, or metrology result buffer and releases any memory associated with it.

All metrology contexts, derived metrology region objects, and metrology result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/met/MmetAlloc.md) was specified during allocation.

## Parameters

### `MetId` *(in, AIL_ID)*

Specifies the identifier of the metrology context, derived metrology region object, or metrology result buffer to free. These must have been successfully allocated (with [`MmetAlloc`](../../Reference/met/MmetAlloc.md) or [`MmetAllocResult`](../../Reference/met/MmetAllocResult.md)) prior to calling this function.
