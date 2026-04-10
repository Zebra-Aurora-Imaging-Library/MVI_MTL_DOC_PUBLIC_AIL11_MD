---
doctype: Reference
module: ocr
function: MocrFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrFree"
---

# MocrFree

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

> Free an OCR font or result buffer.

## Syntax

```c
void MocrFree(
    AIL_ID FontContextOrResultOcrId  //in
)
```

## Description

This function deletes the specified OCR font context or result buffer identifier, and releases any memory associated with it.

All OCR font or result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/ocr/MocrAllocFont.md) was specified during allocation.

## Parameters

### `FontContextOrResultOcrId` *(in, AIL_ID)*

Specifies the identifier of the OCR font context, or result buffer to free. The OCR font context, or result buffer, must have been successfully allocated with [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md) or [`MocrAllocResult`](../../Reference/ocr/MocrAllocResult.md), respectively, prior to calling this function.
