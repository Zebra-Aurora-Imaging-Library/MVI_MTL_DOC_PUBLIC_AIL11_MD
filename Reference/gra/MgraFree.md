---
doctype: Reference
module: gra
function: MgraFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraFree"
---

# MgraFree

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

> Free a 2D graphics context or a 2D graphics list.

## Syntax

```c
void MgraFree(
    AIL_ID ObjectId  //in
)
```

## Description

This function deallocates a 2D graphics context or a 2D graphics list previously allocated with [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md).

If you free a 2D graphics list that is annotating the display non-destructively (specified using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md)), the 2D graphics list is automatically disassociated from the display.

All 2D graphics contexts or 2D graphics lists allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/gra/MgraAlloc.md) was specified during allocation.

## Parameters

### `ObjectId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context or 2D graphics list to deallocate. If `M_DEFAULT` is specified, an error will occur.
