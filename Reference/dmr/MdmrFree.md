---
doctype: Reference
module: dmr
function: MdmrFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrFree"
---

# MdmrFree

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

> Free a SureDotOCR context or a SureDotOCR result buffer.

## Syntax

```c
void MdmrFree(
    AIL_ID ContextOrResultDmrId  //in
)
```

## Description

This function deletes the specified SureDotOCR context or SureDotOCR result buffer identifier, and releases any memory associated with it.

All SureDotOCR contexts and all SureDotOCR result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/dmr/MdmrAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResultDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR context or SureDotOCR result buffer to free. These must have been successfully allocated (with [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md) or [`MdmrAllocResult`](../../Reference/dmr/MdmrAllocResult.md)) prior to calling this function.
