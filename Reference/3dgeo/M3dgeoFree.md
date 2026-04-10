---
doctype: Reference
module: 3dgeo
function: M3dgeoFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoFree"
---

# M3dgeoFree

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

> Free a 3D geometry object or transformation matrix object.

## Syntax

```c
void M3dgeoFree(
    AIL_ID GeometryOrMatrix3dgeoId  //in
)
```

## Description

This function deletes the specified 3D geometry object or transformation matrix object and releases any memory associated with it.

All 3D geometry objects or transformation matrix objects allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3dgeo/M3dgeoAlloc.md) was specified during allocation.

## Parameters

### `GeometryOrMatrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the 3D geometry object or transformation matrix object to free. These must have been successfully allocated with [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) prior to calling this function.
