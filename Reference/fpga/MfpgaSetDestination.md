---
doctype: Reference
module: fpga
function: MfpgaSetDestination
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaSetDestination"
---

# MfpgaSetDestination

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

> Specify a destination buffer for the FPGA processing operation.

## Syntax

```c
AIL_INT MfpgaSetDestination(
    AIL_FPGA_CONTEXT FpgaCommandContext,  //in
    AIL_BUFFER_INFO  DstBuf,              //in
    AIL_INT          StreamOutputNumber,  //in
    AIL_INT64        ControlFlag          //in
)
```

## Description

This function specifies a destination buffer for the FPGA processing operation. This buffer is used to save the data from the specified stream output port of the PU associated with the specified FPGA command context. To route the data to another PU, refer to [`MfpgaSetLink`](../../Reference/fpga/MfpgaSetLink.md).

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

Depending on your FPGA configuration, there can be limitations on the types of buffers to which PUs can output data using their stream output port(s). You must keep these limitations in mind when allocating your destination Aurora Imaging Library buffer(s). This function will not automatically convert the buffers so that they are appropriate for the operation. For more information, refer to the FDK documentation.

## Parameters

### `FpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the FPGA command context associated with the PU.

### `DstBuf` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the destination buffer. Use [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md) to get the handle. The destination buffer can reside in Host memory or in on-board memory, if supported by the FPGA configuration. If allocated in Host memory, the destination buffer must be in non-paged, Processing FPGA accessible memory (using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_FPGA_ACCESSIBLE`](../../Reference/buf/MbufAllocColor.md)+[`M_HOST_MEMORY`](../../Reference/buf/MbufAllocColor.md)). If allocated on-board, the destination buffer must be allocated in on-board, Processing FPGA accessible memory (using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_FPGA_ACCESSIBLE`](../../Reference/buf/MbufAllocColor.md)).

### `StreamOutputNumber` *(in, AIL_INT)*

Specifies the stream output port whose data will be routed to the destination buffer. You must set this parameter to the following value:

*For specifying the rank of the stream output port*

| Value | Description |
| --- | --- |
| `M_OUTPUT + n` | Specifies the rank of the stream output port. |
| `M_OUTPUTn` | Specifies the rank of the stream output port, where _n_ is between 0 and 9. If you have more than 10 outputs, use [`M_OUTPUT + n`](../../Reference/fpga/MfpgaSetDestination.md).

> **Note:** If a PU has one output, use [`M_OUTPUT0`](../../Reference/fpga/MfpgaSetDestination.md). Otherwise, you cannot assume the order of the outputs and need to convert the output stream name to a number using calls to [`MfpgaCommandInquire`](../../Reference/fpga/MfpgaCommandInquire.md). For more information, refer to the FDK documentation. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter can be set to one of the following values:

*For specifying the size of the destination buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the size of the image that is processed will be the intersection (minimum size) of all source and destination buffers. This is used for most applications. |
| `M_FPGA_DONT_INTERSECT` | Specifies that the size of the destination buffer will be a different size than the other source and destination buffers. For example, use this when scaling an image buffer. |

## Return Value

**Type:** `AIL_INT`

Returns the route number to program in to the processing unit (PU) for the stream to be routed to its destination. This return value is needed if the stream is routed to more than one destination. In this case, you will need to program the route number into the appropriate control register of the PU to ensure that the stream is routed properly. For more information, refer to the FDK documentation and examples.
