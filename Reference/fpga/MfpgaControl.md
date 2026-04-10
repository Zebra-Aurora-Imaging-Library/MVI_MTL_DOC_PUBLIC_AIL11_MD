---
doctype: Reference
module: fpga
function: MfpgaControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaControl"
---

# MfpgaControl

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

> Controls a global setting of a specified Processing FPGA.

## Syntax

```c
AIL_INT MfpgaControl(
    AIL_ID       AilSystemId,       //in
    AIL_INT      FpgaDeviceNumber,  //in
    AIL_INT64    ControlType,       //in
    const void * ControlValuePtr    //in
)
```

## Description

This function controls global settings of a specified Processing FPGA. See [`MfpgaCommandControl`](../../Reference/fpga/MfpgaCommandControl.md) to retrieve information about a specific command context.

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

## Parameters

### `AilSystemId` *(in, AIL_ID)*

Specifies the identifier of the system that has the required Processing FPGA.

### `FpgaDeviceNumber` *(in, AIL_INT)*

Specifies the Processing FPGA on the system to control. This parameter must be set to the following value:

*For specifying the rank of the Processing FPGA*

| Value | Description |
| --- | --- |
| `M_DEVn` | Specifies the rank of the Processing FPGA to control, where _n_ can be a value between 0 and the total number of Processing FPGAs-1. |

### `ControlType` *(in, AIL_INT64)*

Specifies the Processing FPGA setting to control.

### `ControlValuePtr` *(in, *void)*

Specifies the value to assign to the Processing FPGA setting.

## Parameter Associations

### For controlling Processing FPGA settings

---

### `M_ERROR`

Sets whether basic parameter checking occurs. Note that, if enabled, this will also report errors when attempting to associate a command context to a PU not in the FPGA configuration, and when attempting to use an invalid interrupt.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PRINT_DISABLE` | Disables printing of error messages. |
| `M_PRINT_ENABLE` *(default)* | Enables printing of error messages. |

## Return Value

**Type:** `AIL_INT`

The returned value is `M_VALID` if successful. If the operation fails, `M_NULL` is returned.
