---
doctype: Reference
module: fpga
function: MfpgaSetLink
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaSetLink"
---

# MfpgaSetLink

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

> Cascade two PUs, such that a stream output of one PU is routed to a stream input port of the other PU.

## Syntax

```c
AIL_INT MfpgaSetLink(
    AIL_FPGA_CONTEXT SrcFpgaCommandContext,  //in
    AIL_INT          StreamOutputNumber,     //in
    AIL_FPGA_CONTEXT DstFpgaCommandContext,  //in
    AIL_INT          StreamInputNumber,      //in
    AIL_INT64        ControlFlag             //in
)
```

## Description

This function cascades (links) the ports of two PUs that are interconnected in the FPGA configuration and loaded in a Processing FPGA. This function allows you to route data from one PU's stream output port to the specified stream input port of another PU.

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

## Parameters

### `SrcFpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the FPGA command context to use in the link.

### `StreamOutputNumber` *(in, AIL_INT)*

Specifies the stream output port of the PU associated with the source command context. You must set this parameter to the following value:

*For specifying the rank of the stream output port*

| Value | Description |
| --- | --- |
| `M_OUTPUT + n` | Specifies the rank of the PU's stream output port to use in the link. |
| `M_OUTPUTn` | Specifies the rank of the PU's stream output port to use in the link, where _n_ is between 0 and 9. If you have more than 10 outputs, use [`M_OUTPUT + n`](../../Reference/fpga/MfpgaSetLink.md).

> **Note:** If the source PU has one stream output port, use [`M_OUTPUT0`](../../Reference/fpga/MfpgaSetLink.md). Otherwise, you cannot assume the order of the outputs and need to convert the output stream name to a number using calls to [`MfpgaCommandInquire`](../../Reference/fpga/MfpgaCommandInquire.md). For more information, refer to the FDK documentation. |

### `DstFpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the destination command context to use in the link.

### `StreamInputNumber` *(in, AIL_INT)*

Specifies the stream input port of the PU associated with the destination command context. You must set this parameter to the following value:

*For specifying the rank of the stream input port*

| Value | Description |
| --- | --- |
| `M_INPUT + n` | Specifies the rank of the PU's stream input port to use as the destination stream port. |
| `M_INPUTn` | Specifies the rank of the PU's stream input port to use as the destination stream port, where _n_ is between 0 and 9. If you have more than 10 inputs, use [`M_INPUT + n`](../../Reference/fpga/MfpgaSetLink.md).

> **Note:** If the destination PU has one stream input port, use [`M_INPUT0`](../../Reference/fpga/MfpgaSetLink.md). Otherwise, you cannot assume the order of the inputs and need to convert the input stream name to a number using calls to [`MfpgaCommandInquire`](../../Reference/fpga/MfpgaCommandInquire.md). For more information, refer to the FDK documentation. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. Set this parameter to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT`

Returns the route number to program in to the source processing unit (PU) for the stream to be routed to its destination. This return value is needed if the stream output port ([`StreamOutputNumber`](../../Reference/fpga/MfpgaSetLink.md)) is routed to more than one destination. In this case, you will need to program the route number into the appropriate control register of the source PU to ensure that the stream is routed properly. For more information, refer to the FDK documentation and examples.
