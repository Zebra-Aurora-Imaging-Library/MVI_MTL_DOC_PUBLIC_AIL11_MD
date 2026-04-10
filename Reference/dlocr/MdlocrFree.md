---
doctype: Reference
module: dlocr
function: MdlocrFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrFree"
---

# MdlocrFree

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

> Free a Deep Learning OCR object.

## Syntax

```c
void MdlocrFree(
    AIL_ID DlocrId  //in
)
```

## Description

This function deletes the specified Deep Learning OCR context, Deep Learning OCR result buffer, or Deep Learning OCR dataset identifier, and releases any memory associated with it.

All Deep Learning OCR objects allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrAlloc.md) was specified during allocation.

## Parameters

### `DlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR object to free. These must have been successfully allocated (with [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md), [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md), or [`MdlocrAllocDataset`](../../Reference/dlocr/MdlocrAllocDataset.md)) prior to calling this function.
