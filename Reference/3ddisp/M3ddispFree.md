---
doctype: Reference
module: 3ddisp
function: M3ddispFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3ddisp / M3ddispFree"
---

# M3ddispFree

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

> Free a 3D display, picking context, or picking result buffer.

## Syntax

```c
void M3ddispFree(
    AIL_ID Disp3dId  //in
)
```

## Description

This function deallocates a 3D display, picking context, or picking result buffer previously allocated with [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md) or [`M3ddispAllocResult`](../../Reference/3ddisp/M3ddispAllocResult.md).

If the 3D display was shown in a window using [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md), [`M3ddispFree`](../../Reference/3ddisp/M3ddispFree.md) also closes the associated window; if the 3D display was shown in a window using [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md), the associated window is left open but it is left blank.

All 3D displays, picking contexts, and picking result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/3ddisp/M3ddispAlloc.md) was specified during allocation.

> **Note:** An error will be generated if you attempt to free the internal 3D graphics list associated with a 3D display.

## Parameters

### `Disp3dId` *(in, AIL_ID)*

Specifies the identifier of a 3D display, picking context, or picking result buffer previously allocated with [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md) or [`M3ddispAllocResult`](../../Reference/3ddisp/M3ddispAllocResult.md).
