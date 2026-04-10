---
doctype: Reference
module: edge
function: MedgeFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeFree"
---

# MedgeFree

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

> Free an Edge Finder context or an Edge Finder result buffer.

## Syntax

```c
void MedgeFree(
    AIL_ID ObjectId  //in
)
```

## Description

This function deletes the specified Edge Finder context or Edge Finder result buffer identifier, and releases any memory associated with it.

All Edge Finder contexts and all Edge Finder result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/edge/MedgeAlloc.md) was specified during allocation.

## Parameters

### `ObjectId` *(in, AIL_ID)*

Specifies the identifier of the Edge Finder context or Edge Finder result buffer to free. These must have been successfully allocated (with [`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md) or [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md)) prior to calling this function.
