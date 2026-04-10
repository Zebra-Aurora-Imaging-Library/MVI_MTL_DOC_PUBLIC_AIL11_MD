---
doctype: Reference
module: func
function: MfuncBufId
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncBufId"
---

# MfuncBufId

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

> Return the Aurora Imaging Library identifier of the buffer.

## Syntax

```c
AIL_ID MfuncBufId(
    AIL_BUFFER_INFO BufferInfoHandle  //in
)
```

## Description

This function retrieves the Aurora Imaging Library identifier of the specified buffer.

## Parameters

### `BufferInfoHandle` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the Aurora Imaging Library buffer. The buffer handle must be obtained using [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md).

## Return Value

**Type:** `AIL_ID`

The returned value is the Aurora Imaging Library identifier of the specified Aurora Imaging Library buffer.

## Remarks

> This function provides no parameter checking or error reporting.
