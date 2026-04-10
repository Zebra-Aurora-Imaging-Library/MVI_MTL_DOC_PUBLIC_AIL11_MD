---
doctype: Reference
module: reg
function: MregFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregFree"
---

# MregFree

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

> Free a registration context or a registration result buffer.

## Syntax

```c
void MregFree(
    AIL_ID ContextOrResultId  //in
)
```

## Description

This function deletes the specified registration context or registration result buffer, and releases any memory associated with it.

All registration contexts and registration result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/reg/MregAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResultId` *(in, AIL_ID)*

Specifies the identifier of the registration context or registration result buffer to free. These must have been successfully allocated (with [`MregAlloc`](../../Reference/reg/MregAlloc.md) or [`MregAllocResult`](../../Reference/reg/MregAllocResult.md)) prior to calling this function.
