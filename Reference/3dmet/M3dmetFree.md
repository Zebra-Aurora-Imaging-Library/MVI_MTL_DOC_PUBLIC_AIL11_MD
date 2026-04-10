---
doctype: Reference
module: 3dmet
function: M3dmetFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetFree"
---

# M3dmetFree

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

> Free a 3D metrology context or result buffer.

## Syntax

```c
void M3dmetFree(
    AIL_ID ContextOrResult3dmetId  //in
)
```

## Description

This function deletes the specified 3D metrology context or result buffer identifier, and release any memory associated with it.

All 3D metrology contexts or 3D metrology result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3dmet/M3dmetAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResult3dmetId` *(in, AIL_ID)*

Specifies the identifier of the 3D metrology context or result buffer to free. These must have been successfully allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) or [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) prior to calling this function.
