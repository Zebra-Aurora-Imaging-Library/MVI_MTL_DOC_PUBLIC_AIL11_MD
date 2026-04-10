---
doctype: Reference
module: fpga
function: MfpgaCommandControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaCommandControl"
---

# MfpgaCommandControl

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

> Control a specified FPGA command context setting.

## Syntax

```c
void MfpgaCommandControl(
    AIL_FPGA_CONTEXT FpgaCommandContext,  //in
    AIL_INT64        ControlType,         //in
    const void *     ControlValuePtr      //in
)
```

## Description

This function controls the various settings of the specified FPGA command context. To inquire information about an FPGA command context setting, see [`MfpgaCommandInquire`](../../Reference/fpga/MfpgaCommandInquire.md). To control or inquire about a general Processing FPGA setting, refer to [`MfpgaControl`](../../Reference/fpga/MfpgaControl.md) or [`MfpgaInquire`](../../Reference/fpga/MfpgaInquire.md), respectively.

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

## Parameters

### `FpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the FPGA command context associated with the PU.

### `ControlType` *(in, AIL_INT64)*

Specifies the FPGA command context setting to control.

### `ControlValuePtr` *(in, *void)*

Specifies the address of the variable which contains the value to assign to the command context setting.

## Parameter Associations

### For controlling FPGA Command Settings

---

### `M_COMPLETION_MODE`

Specifies how the processing operation should be issued on the system command queue. Note that this parameter overrides the setting specified using [`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md) with the [`ExecutionMode`](../../Reference/fpga/MfpgaCommandAlloc.md) parameter.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the command is queued according to the thread synchronization mode. See [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_THREAD_MODE`](../../Reference/thr/MthrControl.md). |
| `M_ASYNCHRONOUS` | Specifies that, after the command is queued, the thread continues executing without waiting for the operation to complete. |
| `M_SYNCHRONOUS` | Specifies that, after the command is queued, the thread waits for the processing operation to complete before continuing. |
