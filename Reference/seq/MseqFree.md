---
doctype: Reference
module: seq
function: MseqFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / seq / MseqFree"
---

# MseqFree

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

> Free a sequence context.

## Syntax

```c
void MseqFree(
    AIL_ID ContextSeqId  //in
)
```

## Description

This function deallocates a previously allocated sequence context. The memory reserved for the specified sequence context is released.

All sequence contexts allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/seq/MseqAlloc.md) was specified during allocation.

## Parameters

### `ContextSeqId` *(in, AIL_ID)*

Specifies the identifier of the sequence context to deallocate. The sequence context must have been previously allocated using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md).
