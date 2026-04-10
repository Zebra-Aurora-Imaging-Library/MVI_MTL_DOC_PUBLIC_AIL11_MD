---
doctype: Reference
module: bead
function: MbeadFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadFree"
---

# MbeadFree

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

> Free a bead context or a bead result buffer.

## Syntax

```c
void MbeadFree(
    AIL_ID ContextOrResultBeadId  //in
)
```

## Description

This function deletes the specified bead context or result buffer identifier, and releases any memory associated with it.

All bead contexts and all bead result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/bead/MbeadAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResultBeadId` *(in, AIL_ID)*

Specifies the identifier of the bead context or bead result buffer to free. These must have been successfully allocated (with [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md) or [`MbeadAllocResult`](../../Reference/bead/MbeadAllocResult.md)) prior to calling this function.
