---
doctype: Reference
module: func
function: MfuncBufHostAddressBand
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncBufHostAddressBand"
---

# MfuncBufHostAddressBand

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

> Returns the Host address of a band of the buffer.

## Syntax

```c
void * MfuncBufHostAddressBand(
    AIL_BUFFER_INFO BufferInfoHandle,  //in
    AIL_INT         Band               //in
)
```

## Description

This function returns the Host address of a band of the specified Aurora Imaging Library buffer, if the buffer is a planar 3-band buffer.

## Parameters

### `BufferInfoHandle` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the Aurora Imaging Library buffer. The buffer handle must be obtained using [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md).

### `Band` *(in, AIL_INT)*

Specifies which band's address to return.

## Return Value

**Type:** `void`

The returned value is the Host address of a band of the specified Aurora Imaging Library buffer.

## Remarks

> This function provides no parameter checking or error reporting.
