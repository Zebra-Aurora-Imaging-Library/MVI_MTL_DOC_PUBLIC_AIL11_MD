---
doctype: Reference
module: 3dmap
function: M3dmapFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapFree"
---

# M3dmapFree

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

> Free a 3D reconstruction context, a 3D alignment context, a 3D reconstruction result buffer, or a 3D alignment result buffer.

## Syntax

```c
void M3dmapFree(
    AIL_ID M3dmapId  //in
)
```

## Description

This function deletes the specified 3D reconstruction context, 3D alignment context, 3D reconstruction result buffer, or 3D alignment result buffer and releases any memory associated with it.

All 3D reconstruction contexts, 3D alignment contexts, 3D reconstruction result buffers, and 3D alignment result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3dmap/M3dmapAlloc.md) was specified during allocation.

## Parameters

### `M3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D reconstruction context, 3D alignement context, 3D reconstruction result buffer, or 3D alignment result buffer to free. These must have been successfully allocated (with [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) or [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md)) prior to calling this function.
