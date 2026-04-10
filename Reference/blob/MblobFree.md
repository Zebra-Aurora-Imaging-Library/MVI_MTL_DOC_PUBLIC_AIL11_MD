---
doctype: Reference
module: blob
function: MblobFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / blob / MblobFree"
---

# MblobFree

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

> Free the blob analysis context or result buffer.

## Syntax

```c
void MblobFree(
    AIL_ID ContextOrResultBlobId  //in
)
```

## Description

This function deletes the specified blob analysis context or result buffer and releases any memory associated with it.

All blob contexts and all blob result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/blob/MblobAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResultBlobId` *(in, AIL_ID)*

Specifies the identifier of the blob analysis context or result buffer to free. The buffer must have been successfully allocated, using [`MblobAllocResult`](../../Reference/blob/MblobAllocResult.md) or [`MblobAlloc`](../../Reference/blob/MblobAlloc.md), prior to calling this function.
