---
doctype: Reference
module: str
function: MstrFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrFree"
---

# MstrFree

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

> Free a String Reader context or a String Reader result buffer.

## Syntax

```c
void MstrFree(
    AIL_ID ObjectId  //in
)
```

## Description

This function deletes the specified String Reader context or String Reader result buffer identifier, and releases any memory associated with it.

All String Reader contexts and all String Reader result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/str/MstrAlloc.md) was specified during allocation.

## Parameters

### `ObjectId` *(in, AIL_ID)*

Specifies the identifier of the String Reader context or String Reader result buffer to free. These must have been successfully allocated (with [`MstrAlloc`](../../Reference/str/MstrAlloc.md) or [`MstrAllocResult`](../../Reference/str/MstrAllocResult.md)) prior to calling this function.
