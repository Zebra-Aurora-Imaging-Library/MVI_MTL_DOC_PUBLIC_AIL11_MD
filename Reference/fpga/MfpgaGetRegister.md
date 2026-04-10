---
doctype: Reference
module: fpga
function: MfpgaGetRegister
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaGetRegister"
---

# MfpgaGetRegister

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

> Set up a request to read registers of a PU.

## Syntax

```c
void MfpgaGetRegister(
    AIL_FPGA_CONTEXT FpgaCommandContext,  //in
    AIL_INT64        RegisterSection,     //in
    AIL_INT          Offset,              //in
    AIL_INT          Length,              //in
    void *           ValuePtr,            //out
    AIL_INT64        ReadAccessFlag       //in
)
```

## Description

This function sets up a request to read registers of the PU associated with the specified command context. A maximum of 16 [`MfpgaGetRegister`](../../Reference/fpga/MfpgaGetRegister.md) calls can be made with any FPGA command context, although you can read multiple registers in each call. For a PU from the Zebra PU library, you should consult the FDK documentation or relevant examples for details of the register file. You can specify whether read accesses will be collected and made before processing or after the PU finishes processing (according to [`CompletionMode`](../../Reference/fpga/MfpgaCommandQueue.md)).

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

## Parameters

### `FpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the FPGA command context associated with the PU. The command context must have been previously allocated on the system using [`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md).

### `RegisterSection` *(in, AIL_INT64)*

Specifies the section of the PU's register space to access. You must set this parameter to the following value:

*For register space access*

| Value | Description |
| --- | --- |
| `M_PU_BASE` | Specifies to access the base address of the PU's register space. |

### `Offset` *(in, AIL_INT)*

Specifies the offset relative to the PU's base address, from which to begin reading, in bytes. The offset must be a multiple of 4 bytes.

### `Length` *(in, AIL_INT)*

Specifies how many bytes of the register to read. The length must be a multiple of 4 bytes.

### `ValuePtr` *(out, *void)*

Specifies the address of the variable in which to write the value read from the register. This address must remain valid for the duration of the operation, otherwise a memory corruption will occur.

### `ReadAccessFlag` *(in, AIL_INT64)*

Specifies when the register read access must take place. This parameter must be set to one of the following values:

*For specifying when to read the register*

| Value | Description |
| --- | --- |
| `M_WHEN_COMPLETED` | Accesses the registers after the PU finishes processing (according to [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) completion mode). |
| `M_WHEN_DISPATCHED` | Accesses the registers during PU setup, prior to the start of processing. |
