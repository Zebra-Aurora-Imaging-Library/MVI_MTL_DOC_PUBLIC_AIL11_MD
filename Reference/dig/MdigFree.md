---
doctype: Reference
module: dig
function: MdigFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dig / MdigFree"
---

# MdigFree

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

> Free a digitizer.

## Syntax

```c
void MdigFree(
    AIL_ID DigId  //in
)
```

## Description

This function deallocates a digitizer previously allocated with [`MdigAlloc`](../../Reference/dig/MdigAlloc.md).

All digitizers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/dig/MdigAlloc.md) was specified during allocation.

## Parameters

### `DigId` *(in, AIL_ID)*

Specifies the identifier of the digitizer.
