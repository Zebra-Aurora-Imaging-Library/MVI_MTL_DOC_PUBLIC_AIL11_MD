---
doctype: Reference
module: 3dreg
function: M3dregFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregFree"
---

# M3dregFree

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

> Free a 3D registration context or a 3D registration result buffer.

## Syntax

```c
void M3dregFree(
    AIL_ID ContextOrResult3dregId  //in
)
```

## Description

This function deletes the specified 3D registration context or 3D registration result buffer, and releases any memory associated with it.

All 3D registration contexts and 3D registration result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3dreg/M3dregAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResult3dregId` *(in, AIL_ID)*

Specifies the identifier of the 3D registration context or 3D registration result buffer to free. These must have been successfully allocated (using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) or [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md)) prior to calling this function.
