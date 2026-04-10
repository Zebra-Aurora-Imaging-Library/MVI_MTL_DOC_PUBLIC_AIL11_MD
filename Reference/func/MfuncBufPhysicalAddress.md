---
doctype: Reference
module: func
function: MfuncBufPhysicalAddress
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncBufPhysicalAddress"
---

# MfuncBufPhysicalAddress

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

> Return the physical address of the buffer.

## Syntax

```c
AIL_UINT64 MfuncBufPhysicalAddress(
    AIL_BUFFER_INFO BufferInfoHandle  //in
)
```

## Description

This function retrieves the physical address of the specified Aurora Imaging Library buffer, if it is not a planar 3-band buffer.

For a planar 3-band buffer, you can determine its physical address using [`MfuncBufPhysicalAddressBand`](../../Reference/func/MfuncBufPhysicalAddressBand.md).

## Parameters

### `BufferInfoHandle` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the Aurora Imaging Library buffer. The buffer handle must be obtained using [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md).

## Return Value

**Type:** `AIL_UINT64`

The returned value is the physical address of the specified Aurora Imaging Library buffer.

## Remarks

> This function is equivalent to using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_PHYSICAL_ADDRESS`](../../Reference/buf/MbufInquire.md), but provides no parameter checking or error reporting.
