---
doctype: Reference
module: func
function: MfuncBufParentOffsetX
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncBufParentOffsetX"
---

# MfuncBufParentOffsetX

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

> Return the X-offset relative to the parent buffer.

## Syntax

```c
AIL_INT MfuncBufParentOffsetX(
    AIL_BUFFER_INFO BufferInfoHandle  //in
)
```

## Description

This function returns the X-offset of the specified Aurora Imaging Library buffer relative its parent buffer.

## Parameters

### `BufferInfoHandle` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the Aurora Imaging Library buffer. The buffer handle must be obtained using [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md).

## Return Value

**Type:** `AIL_INT`

The returned value is the X-offset of the specified Aurora Imaging Library buffer relative its parent buffer.

## Remarks

> This function is equivalent to using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_PARENT_OFFSET_X`](../../Reference/buf/MbufInquire.md), but provides no parameter checking or error reporting.
