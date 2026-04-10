---
doctype: Reference
module: disp
function: MdispFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / disp / MdispFree"
---

# MdispFree

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

> Free a display.

## Syntax

```c
void MdispFree(
    AIL_ID DisplayId  //in
)
```

## Description

This function deallocates a display previously allocated with [`MdispAlloc`](../../Reference/disp/MdispAlloc.md).

If an image buffer was selected to the display using [`MdispSelect`](../../Reference/disp/MdispSelect.md), [`MdispFree`](../../Reference/disp/MdispFree.md) also closes the associated window for windowed displays; if an image buffer was selected to the display using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md), the associated window is left open but it is left blank. For exclusive displays, this causes the display to disappear, allowing the desktop to reappear on the screen.

All displays allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/disp/MdispAlloc.md) was specified during allocation.

## Parameters

### `DisplayId` *(in, AIL_ID)*

Specifies the identifier of the display.
