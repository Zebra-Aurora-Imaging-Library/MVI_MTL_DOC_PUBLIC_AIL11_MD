---
doctype: Reference
module: func
function: MfuncBufPitchByte
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncBufPitchByte"
---

# MfuncBufPitchByte

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

> Return the number of bytes between the beginnings of any two adjacent lines of the buffer.

## Syntax

```c
AIL_INT MfuncBufPitchByte(
    AIL_BUFFER_INFO BufferInfoHandle  //in
)
```

## Description

This function retrieves the number of bytes between the beginnings of any two adjacent lines of the specified Aurora Imaging Library buffer.

## Parameters

### `BufferInfoHandle` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the Aurora Imaging Library buffer. The buffer handle must be obtained using [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md).

## Return Value

**Type:** `AIL_INT`

The returned value is the number of bytes between the beginnings of any two adjacent lines of the specified Aurora Imaging Library buffer.

## Remarks

> This function is equivalent to using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_PITCH_BYTE`](../../Reference/buf/MbufInquire.md), but provides no parameter checking or error reporting.
