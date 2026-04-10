---
doctype: Reference
module: fpga
function: MfpgaCommandInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaCommandInquire"
---

# MfpgaCommandInquire

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

> Inquire about a specified FPGA command context setting.

## Syntax

```c
void MfpgaCommandInquire(
    AIL_FPGA_CONTEXT FpgaCommandContext,  //in
    AIL_INT64        InquireType,         //in
    void *           UserVarPtr           //out
)
```

## Description

This function inquires information about a specified FPGA command context setting.

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

## Parameters

### `FpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the FPGA command context associated with the PU.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire. This parameter can be set to one of the following values:

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For specifying the type of setting

---

### `M_COMPLETION_MODE`

Inquires how the processing operation is issued on the system command queue.

| Value | Description |
| --- | --- |
| `M_ASYNCHRONOUS` | Specifies that, after the command is queued, the thread continues executing without waiting for the operation to complete. |
| `M_SYNCHRONOUS` | Specifies that, after the command is queued, the thread waits for the processing operation to complete before continuing. |

---

### `M_FUNCTION_ID`

Inquires the PU's function identifier.

| Value | Description |
| --- | --- |
| `Value` | Specifies the PU's function identifier. |

---

### `M_NUMBER_OF_EVENTS`

Inquires the total number of interrupts that the PU can generate.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of interrupts the PU can generate. |

---

### `M_NUMBER_OF_INPUTS`

Inquires the total number of stream input ports.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of stream input ports. |

---

### `M_NUMBER_OF_OUTPUTS`

Inquires the total number of stream output ports.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of stream output ports. |

---

### `M_PORT_NAME`

Inquires the name of the nth stream input or output port.

| Value | Description |
| --- | --- |
| `Value` | Specifies the name of the nth stream input or output port. |

### Combination Constants — For specifying which stream input or output port to inquire

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the index of the stream input or output port to inquire.

| Value | Description |
| --- | --- |
| `M_INPUT + n` | Specifies the stream input port at index _n_. This is useful if you want to loop through all the stream input ports. |
| `M_INPUTn` | Specifies the stream input port at index _n_, where _n_ is between 0 and 9. If you have more than 10 inputs, use [`M_INPUT + n`](../../Reference/fpga/MfpgaCommandInquire.md). |
| `M_OUTPUT + n` | Specifies the stream output port at index _n_. This is useful if you want to loop through all the stream output ports. |
| `M_OUTPUTn` | Specifies the stream output port at index _n_, where _n_ is between 0 and 9. If you have more than 10 outputs, use [`M_OUTPUT + n`](../../Reference/fpga/MfpgaCommandInquire.md). |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

To retrieve the string size of a port name, you must also specify the required port index. For example, [`M_PORT_NAME`](../../Reference/fpga/MfpgaCommandInquire.md) + [`M_INPUTn`](../../Reference/fpga/MfpgaCommandInquire.md) + [`M_STRING_SIZE`](../../Reference/fpga/MfpgaCommandInquire.md).

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").
