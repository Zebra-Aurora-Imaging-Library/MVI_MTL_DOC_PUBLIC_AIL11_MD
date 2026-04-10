---
doctype: Reference
module: 3dgra
function: M3dgraFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraFree"
---

# M3dgraFree

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

> Free a 3D graphics list.

## Syntax

```c
void M3dgraFree(
    AIL_ID List3dgraId  //in
)
```

## Description

This function deletes the specified 3D graphics list, and releases any memory associated with it.

All 3D graphics lists allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3dgra/M3dgraAlloc.md) was specified during allocation.

## Parameters

### `List3dgraId` *(in, AIL_ID)*

Specifies the identifier of the 3D graphics list to free. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).
