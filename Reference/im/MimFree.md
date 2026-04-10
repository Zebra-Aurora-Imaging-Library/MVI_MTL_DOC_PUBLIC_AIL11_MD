---
doctype: Reference
module: im
function: MimFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimFree"
---

# MimFree

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

> Free an image processing context or result buffer.

## Syntax

```c
void MimFree(
    AIL_ID ContextOrResultImId  //in
)
```

## Description

This function frees the specified image processing context or result buffer, previously allocated with [`MimAlloc`](../../Reference/im/MimAlloc.md) or [`MimAllocResult`](../../Reference/im/MimAllocResult.md), respectively. It should also be used if an image processing context was restored using [`MimRestore`](../../Reference/im/MimRestore.md) or [`MimStream`](../../Reference/im/MimStream.md) (The control flag in [`MimStream`](../../Reference/im/MimStream.md) should be set to [`M_RESTORE`](../../Reference/im/MimStream.md) in this case).

All image processing contexts and result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/im/MimAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResultImId` *(in, AIL_ID)*

Specifies the identifier of the image processing context or result buffer to free. These must have been successfully allocated (with [`MimAlloc`](../../Reference/im/MimAlloc.md) or [`MimAllocResult`](../../Reference/im/MimAllocResult.md)) prior to calling this function.
