---
doctype: Reference
module: fpga
function: MfpgaSetRegister
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaSetRegister"
---

# MfpgaSetRegister

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

> Set the contents of one or more FPGA registers.

## Syntax

```c
void MfpgaSetRegister(
    AIL_FPGA_CONTEXT FpgaCommandContext,  //in
    AIL_INT64        RegisterSection,     //in
    AIL_INT          Offset,              //in
    AIL_INT          Length,              //in
    void *           ValuePtr,            //out
    AIL_INT64        WriteAccessFlag      //in
)
```

## Description

This function writes values to FPGA registers of the PU associated with the specified command context. A maximum of 16 [`MfpgaSetRegister`](../../Reference/fpga/MfpgaSetRegister.md) calls can be made with any FPGA command context, although you can set the contents of multiple registers in each call. For a PU from the Zebra PU library, you should consult the FDK documentation or relevant examples for details of the register file. You can specify whether write accesses will be collected and made before processing or after the PU finishes processing (according to [`CompletionMode`](../../Reference/fpga/MfpgaCommandQueue.md)).

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

## Parameters

### `FpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the FPGA command context associated with the PU.

### `RegisterSection` *(in, AIL_INT64)*

Specifies the section of the PU's register space to access. You must set this parameter to the following value:

*For register space access*

| Value | Description |
| --- | --- |
| `M_PU_BASE` | Specifies to access the base address of the PU's register space. |

### `Offset` *(in, AIL_INT)*

Specifies the offset relative to the PU's base address, from which to begin writing, in bytes. The offset must be a multiple of 4 bytes.

### `Length` *(in, AIL_INT)*

Specifies the number of bytes to write in the register. The length must be a multiple of 4 bytes.

### `ValuePtr` *(out, *void)*

Specifies the address of the variable in which to write the value to be sent to the register.

### `WriteAccessFlag` *(in, AIL_INT64)*

Specifies when the register write access must take place. This parameter must be set to one of the following values:

*For specifying when to access the register*

| Value | Description |
| --- | --- |
| `M_WHEN_COMPLETED` | Accesses the registers after the PU issues its end-of-processing interrupt. |
| `M_WHEN_DISPATCHED` | Accesses the registers during PU setup, prior to the start of processing. |
