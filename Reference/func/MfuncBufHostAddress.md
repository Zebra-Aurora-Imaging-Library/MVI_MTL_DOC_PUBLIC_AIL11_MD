---
doctype: Reference
module: func
function: MfuncBufHostAddress
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncBufHostAddress"
---

# MfuncBufHostAddress

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

> Returns the Host address of the buffer.

## Syntax

```c
void * MfuncBufHostAddress(
    AIL_BUFFER_INFO BufferInfoHandle  //in
)
```

## Description

This function returns the Host address of the specified Aurora Imaging Library buffer, if the buffer is visible from the Host address space and is not a planar 3-band buffer.

For a planar 3-band buffer, you can determine its Host address using [`MfuncBufHostAddressBand`](../../Reference/func/MfuncBufHostAddressBand.md).

## Parameters

### `BufferInfoHandle` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the Aurora Imaging Library buffer. The buffer handle must be obtained using [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md).

## Return Value

**Type:** `void`

The returned value is the Host address of the specified Aurora Imaging Library buffer.

## Remarks

> This function is equivalent to using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_HOST_ADDRESS`](../../Reference/buf/MbufInquire.md), but provides no parameter checking or error reporting.
