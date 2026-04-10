---
doctype: Reference
module: 3dim
function: M3dimFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimFree"
---

# M3dimFree

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

> Free a 3D image processing context or result buffer.

## Syntax

```c
void M3dimFree(
    AIL_ID ContextOrResult3dimId  //in
)
```

## Description

This function deletes the specified 3D image processing context or result buffer identifier, and releases any memory associated with it.

All 3D image processing contexts or result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3dim/M3dimAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResult3dimId` *(in, AIL_ID)*

Specifies the identifier of the 3D image processing context or result buffer to free. These must have been successfully allocated (using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) or [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md)) prior to calling this function.
