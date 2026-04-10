---
doctype: Reference
module: cal
function: McalFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalFree"
---

# McalFree

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

> Free a camera calibration context, a fixturing offset object, or a calibration result buffer.

## Syntax

```c
void McalFree(
    AIL_ID CalibrationId  //in
)
```

## Description

This function frees a camera calibration context, a fixturing offset object, or a calibration result buffer.

All camera calibration contexts allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/cal/McalAlloc.md) was specified during allocation.

## Parameters

### `CalibrationId` *(in, AIL_ID)*

Specifies the identifier of the camera calibration context, fixturing offset object or calibration result buffer.
