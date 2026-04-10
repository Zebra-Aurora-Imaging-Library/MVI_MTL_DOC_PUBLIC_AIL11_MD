---
doctype: Reference
module: func
function: MfuncBufParentId
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncBufParentId"
---

# MfuncBufParentId

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

> Return the Aurora Imaging Library identifier of the parent buffer.

## Syntax

```c
AIL_ID MfuncBufParentId(
    AIL_BUFFER_INFO BufferInfoHandle  //in
)
```

## Description

This function retrieves the Aurora Imaging Library identifier of the specified Aurora Imaging Library buffers' parent buffer.

Only child buffers have a parent buffer. The parent buffer is the buffer from which the specified buffer was defined. The parent buffer can itself have a parent buffer.

## Parameters

### `BufferInfoHandle` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the Aurora Imaging Library buffer. The buffer handle must be obtained using [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md).

## Return Value

**Type:** `AIL_ID`

The returned value is the Aurora Imaging Library identifier of the specified Aurora Imaging Library buffers' parent buffer.

## Remarks

> This function is equivalent to using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_PARENT_ID`](../../Reference/buf/MbufInquire.md), but provides no parameter checking or error reporting.
