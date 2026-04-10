---
doctype: Reference
module: 3dblob
function: M3dblobFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobFree"
---

# M3dblobFree

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

> Free a 3D blob analysis context or a 3D blob analysis result buffer.

## Syntax

```c
void M3dblobFree(
    AIL_ID ContextOrResult3dblobId  //in
)
```

## Description

This function deletes the specified 3D blob analysis context or 3D blob analysis result buffer, and releases any memory associated with it.

All 3D blob analysis contexts and 3D blob analysis result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResult3dblobId` *(in, AIL_ID)*

Specifies the identifier of the 3D blob analysis context or 3D blob analysis result buffer to free. These must have been successfully allocated (using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) or [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md)) prior to calling this function.
