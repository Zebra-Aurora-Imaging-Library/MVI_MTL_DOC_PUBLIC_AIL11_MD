---
doctype: Reference
module: fpga
function: MfpgaCommandAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaCommandAlloc"
---

# MfpgaCommandAlloc

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

> Allocate an FPGA command context for a PU in the FPGA configuration loaded in a Processing FPGA on a target system.

## Syntax

```c
AIL_INT MfpgaCommandAlloc(
    AIL_ID             AilSysId,              //in
    AIL_INT            DeviceNumber,          //in
    AIL_INT            FunctionId,            //in
    AIL_INT            SubFunctionId,         //in
    AIL_INT64          FunctionNumber,        //in
    AIL_INT            ExecutionMode,         //in
    AIL_INT64          ControlFlag,           //in
    AIL_FPGA_CONTEXT * FpgaCommandContextPtr  //out
)
```

## Description

This function allocates an FPGA command context. An FPGA command context is used to contain the necessary command information to perform the required operation using the specified PU in a Processing FPGA on a target system, without writing it immediately to the target hardware. The FPGA command context is valid only for the thread on which the current command context is allocated. It cannot be referenced by any other thread.

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

After calling this function, you should ensure that the context was successfully allocated by verifying that the context handle is not **M_NULL**. For example, the context will not be allocated if the specified Processing FPGA does not contain the specified PU.

## Parameters

### `AilSysId` *(in, AIL_ID)*

Specifies the identifier of the system that has the required Processing FPGA.

### `DeviceNumber` *(in, AIL_INT)*

Specifies the Processing FPGA for which to allocate the command context. This parameter must be set to the following value:

*For specifying the rank of the Processing FPGA*

| Value | Description |
| --- | --- |
| `M_DEVn` | Specifies the rank of the Processing FPGA on the board, where _n_ can be a value between 0 and the total number of Processing FPGAs-1. |

### `FunctionId` *(in, AIL_INT)*

Specifies the function identifier of the required PU. Typically, you know the name of the PU, but not the function identifier. In this case, you will need to convert the name to the function identifier using [`MfpgaInquire`](../../Reference/fpga/MfpgaInquire.md). For more information, refer to the FDK documentation. Note that the range of custom PU function identifiers is between 0xFC00 and 0xFFFF, inclusive.

### `SubFunctionId` *(in, AIL_INT)*

Reserved for future expansion. Set this parameter to `M_DEFAULT`.

### `FunctionNumber` *(in, AIL_INT64)*

Specifies the PU to use when two or more instances, with the same function identifier and subfunction identifier, are present in a FPGA configuration loaded in a Processing FPGA. This parameter can be set to the following value:

*For specifying the PU to use*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ANY`](../../Reference/fpga/MfpgaCommandAlloc.md). |
| `M_ANY` | Specifies that any of the PUs with the specified function identifier and subfunction identifier can be used. |
| `M_DEVn` | Specifies the rank of the PU in the FPGA configuration loaded in the Processing FPGA, where _n_ represents the specific PU instance starting from 0. The higher the instance's register base address, the higher the rank. |

### `ExecutionMode` *(in, AIL_INT)*

Specifies how the processing operation should be issued on the system command queue. This parameter should be set to one of the following values:

*For specifying whether the processing operation is asynchronous or synchronous*

| Value | Description |
| --- | --- |
| `M_ASYNCHRONOUS` | Specifies that, after the command is queued, the thread continues executing without waiting for the operation to complete. |
| `M_SYNCHRONOUS` | Specifies that, after the command is queued, the thread waits for the processing operation to complete before continuing. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. Set this parameter to `M_DEFAULT`.

### `FpgaCommandContextPtr` *(out, *AIL_FPGA_CONTEXT)*

Specifies the address of the variable in which to write the handle of the FPGA command context. The command context is valid only for the thread on which the command context is allocated. It cannot be referenced by any other thread.

## Return Value

**Type:** `AIL_INT`

The returned value is `M_VALID` if allocation is successful. If allocation fails, `M_NULL` is returned.
