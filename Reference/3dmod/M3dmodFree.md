---
doctype: Reference
module: 3dmod
function: M3dmodFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodFree"
---

# M3dmodFree

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

> Free a 3D model finder context or a 3D model finder result buffer.

## Syntax

```c
void M3dmodFree(
    AIL_ID ContextOrResult3dmodId  //in
)
```

## Description

This function deletes the specified 3D model finder context or 3D model finder result buffer, and releases any memory associated with it.

All 3D model finder contexts and 3D model finder result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3dgra/M3dgraAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResult3dmodId` *(in, AIL_ID)*

Specifies the identifier of the 3D model finder context or 3D model finder result buffer to free. These must have been successfully allocated (using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) or [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md)) prior to calling this function.
