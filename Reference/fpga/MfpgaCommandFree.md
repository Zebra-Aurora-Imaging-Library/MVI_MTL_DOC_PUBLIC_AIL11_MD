---
doctype: Reference
module: fpga
function: MfpgaCommandFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaCommandFree"
---

# MfpgaCommandFree

| Board | Supported |
| --- | --- |
| Host System | No |
| V4L2 | No |
| Clarity UHD | No |
| Concord PoE | No |
| GenTL | No |
| GevIQ | No |
| GigE Vision | No |
| Indio | No |
| Iris GTX | No |
| Radient eV-CL | No |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | No |

> Free an FPGA command context.

## Syntax

```c
AIL_INT MfpgaCommandFree(
    AIL_FPGA_CONTEXT FpgaCommandContext,  //in
    AIL_INT64        ControlFlag          //in
)
```

## Description

This function deallocates a previously allocated FPGA command context. The FPGA command context should be freed after an [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) is issued. The processing operation that is associated with the command context, will complete its operation even if [`MfpgaCommandFree`](../../Reference/fpga/MfpgaCommandFree.md) is executed before processing is finished.

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

## Parameters

### `FpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the FPGA command context to deallocate. The command context must have been previously allocated on the system using [`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. Set this parameter to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT`

The returned value is `M_VALID` if deallocation is successful. If deallocation fails, `M_NULL` is returned.
