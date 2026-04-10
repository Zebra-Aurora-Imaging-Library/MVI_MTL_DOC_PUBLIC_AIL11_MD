---
doctype: Reference
module: fpga
function: MfpgaSetSource
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaSetSource"
---

# MfpgaSetSource

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

> Specify a source buffer for the FPGA processing operation.

## Syntax

```c
void MfpgaSetSource(
    AIL_FPGA_CONTEXT FpgaCommandContext,  //in
    AIL_BUFFER_INFO  SrcBuf,              //in
    AIL_INT          StreamInputNumber,   //in
    AIL_INT64        ControlFlag          //in
)
```

## Description

This function specifies a source buffer for the FPGA processing operation. Data from the specified buffer is routed to the specified stream input port of the PU associated with the specified FPGA command context. To obtain the data from another PU, refer to [`MfpgaSetLink`](../../Reference/fpga/MfpgaSetLink.md).

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

Depending on your FPGA configuration, there can be limitations on the types of buffers from which PUs can receive data using their stream input port(s). You must keep these limitations in mind when allocating your source Aurora Imaging Library buffer(s). This function will not automatically convert the buffers so that they are appropriate for the operation. For more information, refer to the FDK documentation.

## Parameters

### `FpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the FPGA command context associated with the PU.

### `SrcBuf` *(in, AIL_BUFFER_INFO)*

Specifies the handle of the source buffer. Use [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md) to get the handle. The source buffer can only reside in on-board, FPGA accessible memory (using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_FPGA_ACCESSIBLE`](../../Reference/buf/MbufAllocColor.md)).

### `StreamInputNumber` *(in, AIL_INT)*

Specifies the stream input port to which to route data from the source buffer. You must set this parameter to the following value:

*For specifying the rank of the stream input port*

| Value | Description |
| --- | --- |
| `M_INPUT + n` | Specifies the rank of the stream input port. |
| `M_INPUTn` | Specifies the rank of the stream input port, where _n_ is between 0 and 9. If you have more than 10 inputs, use [`M_INPUT + n`](../../Reference/fpga/MfpgaSetSource.md).

> **Note:** If a PU has one input, use [`M_INPUT0`](../../Reference/fpga/MfpgaSetSource.md). Otherwise, you cannot assume the order of the inputs and need to convert the input stream name to a number using calls to [`MfpgaCommandInquire`](../../Reference/fpga/MfpgaCommandInquire.md). For more information, refer to the FDK documentation. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter can be set to one of the following values:

*For specifying the size of the destination buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the size of the image that is processed will be the intersection (minimum size) of all source and destination buffers. This is used for most applications. |
| `M_FPGA_DONT_INTERSECT` | Specifies that the size of the destination buffer will be a different size than the other source and destination buffers. For example, use this when scaling an image buffer. |
