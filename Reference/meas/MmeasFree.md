---
doctype: Reference
module: meas
function: MmeasFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasFree"
---

# MmeasFree

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

> Free a measurement context, marker buffer, or result buffer.

## Syntax

```c
void MmeasFree(
    AIL_ID MeasId  //in
)
```

## Description

This function deletes the specified marker buffer, result buffer, or context identifier and releases any memory associated with it, unless [`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocMarker.md) was specified during allocation.

## Parameters

### `MeasId` *(in, AIL_ID)*

Specifies the measurement identifier to free. The identifier must have been successfully allocated with [`MmeasAllocMarker`](../../Reference/meas/MmeasAllocMarker.md), [`MmeasAllocResult`](../../Reference/meas/MmeasAllocResult.md), or [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md) prior to calling this function.
