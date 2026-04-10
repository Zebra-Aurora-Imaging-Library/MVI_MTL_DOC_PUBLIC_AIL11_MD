---
doctype: Reference
module: col
function: McolFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolFree"
---

# McolFree

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

> Free a color matching context, a relative color calibration context, or a color matching result buffer.

## Syntax

```c
void McolFree(
    AIL_ID ContextIdOrResultId  //in
)
```

## Description

This function deletes the specified context or result buffer, and releases any memory associated with it. If specifying a context, this function deletes all its color-samples. To only delete color-samples in a context, use [`McolDefine`](../../Reference/col/McolDefine.md) with [`M_DELETE`](../../Reference/col/McolDefine.md).

All contexts and all result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/col/McolAlloc.md) was specified during allocation.

## Parameters

### `ContextIdOrResultId` *(in, AIL_ID)*

Specifies the identifier of the context or result buffer to free. These must have been successfully allocated (with [`McolAlloc`](../../Reference/col/McolAlloc.md) or [`McolAllocResult`](../../Reference/col/McolAllocResult.md)) prior to calling this function.
