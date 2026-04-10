---
doctype: Reference
module: func
function: MfuncBufMaxValue
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncBufMaxValue"
---

# MfuncBufMaxValue

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

> Return the maximum pixel value possible in the buffer.

## Syntax

```c
AIL_DOUBLE MfuncBufMaxValue(
    AIL_BUFFER_INFO BufferInfoHandle  //in
)
```

## Description

This function returns the maximum pixel value possible in the specified Aurora Imaging Library buffer.

## Parameters

### `BufferInfoHandle` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the Aurora Imaging Library buffer. The buffer handle must be obtained using [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md).

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the maximum pixel value possible in the specified Aurora Imaging Library buffer.

## Remarks

> This function is equivalent to using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_MAX`](../../Reference/buf/MbufInquire.md), but provides no parameter checking or error reporting.
