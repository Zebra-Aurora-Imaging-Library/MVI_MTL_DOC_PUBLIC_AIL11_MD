---
doctype: Reference
module: 3dmeas
function: M3dmeasFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasFree"
---

# M3dmeasFree

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

> Free a 3D measurement context or result buffer.

## Syntax

```c
void M3dmeasFree(
    AIL_ID ContextOrResult3dmeasId  //in
)
```

## Description

This function deletes the specified 3D measurement context or result buffer identifier, and releases any memory associated with it.

All 3D measurement contexts and result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3dmeas/M3dmeasAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResult3dmeasId` *(in, AIL_ID)*

Specifies the identifier of the 3D measurement context or result buffer to free. These must have been successfully allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) or [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md) prior to calling this function.
