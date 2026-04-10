---
doctype: Reference
module: fpga
function: MfpgaInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaInquire"
---

# MfpgaInquire

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

> Inquire global information about a specified Processing FPGA.

## Syntax

```c
AIL_INT MfpgaInquire(
    AIL_ID    AilSystemId,       //in
    AIL_INT   FpgaDeviceNumber,  //in
    AIL_INT64 InquireType,       //in
    void *    UserVarPtr         //out
)
```

## Description

This function inquires global information about a Processing FPGA, including all loaded processing units (PUs) and transfer units (TUs). See [`MfpgaCommandInquire`](../../Reference/fpga/MfpgaCommandInquire.md) to retrieve information about a specific command context.

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

## Parameters

### `AilSystemId` *(in, AIL_ID)*

Specifies the identifier of the system that has the required Processing FPGA.

### `FpgaDeviceNumber` *(in, AIL_INT)*

Specifies the Processing FPGA to inquire. This parameter must be set to the following value:

*For specifying the rank of the Processing FPGA*

| Value | Description |
| --- | --- |
| `M_DEVn` | Specifies the rank of the Processing FPGA about which to inquire, where _n_ can be a value between 0 and the total number of Processing FPGAs-1. |

### `InquireType` *(in, AIL_INT64)*

Specifies the information about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address at which to write the requested information. Since the [`MfpgaInquire`](../../Reference/fpga/MfpgaInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For FPGAs

---

### `M_FPGA_CONFIGURATION_FILENAME`

Inquires the string containing identifying information about the file name of the Processing FPGA configuration. The information consists of the location of the file (its path), followed by the name of the file (for example, "C:\mydirectory\myfile").  For a file on the remote computer (under Distributed Aurora Imaging Library), the path and file name will be preceded by "remote:///" (for example, "remote:///C:\mydirectory\myfile").

| Value | Description |
| --- | --- |
| `"String"` | Specifies the string containing information about the file name. |

---

### `M_FPGA_PACKAGE_NAME`

Inquires the string containing identifying information about the Processing FPGA on your board. The information consists of the specific package name, followed by an underscore, followed by the speed grade.

| Value | Description |
| --- | --- |
| `"String"` | Specifies the string containing information about the Processing FPGA. |

---

### `M_NUMBER_OF_PU`

Inquires the number of processing units (PUs) currently loaded in the Processing FPGA.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of PUs. |

---

### `M_NUMBER_OF_TU`

**Board availability:** Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the number of transfer units (TUs) currently loaded in the Processing FPGA.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of TUs. |

---

### `M_PU_FID + n`

**Board availability:** Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the function identifier of the PU at index _n_, where _n_ is a number between 0 and the value returned by [`M_NUMBER_OF_PU`](../../Reference/fpga/MfpgaInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the function identifier. |

---

### `M_PU_LIST`

Inquires the list of function identifiers of all PUs currently loaded in the Processing FPGA.

| Value | Description |
| --- | --- |
| `Value` | Specifies the function identifier. |

---

### `M_PU_NAME + n`

**Board availability:** Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the name of the PU at index _n_, where _n_ is a number between 0 and the value returned by [`M_NUMBER_OF_PU`](../../Reference/fpga/MfpgaInquire.md).

| Value | Description |
| --- | --- |
| `String` | Specifies the name of the PU at index _n_. |

---

### `M_TU_FID + n`

**Board availability:** Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the function identifier of the TU at index _n_, where _n_ is a number between 0 and the value returned by [`M_NUMBER_OF_TU`](../../Reference/fpga/MfpgaInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the function identifier. |

---

### `M_TU_LIST`

**Board availability:** Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the list of function identifiers of all TUs currently loaded in the Processing FPGA.

| Value | Description |
| --- | --- |
| `Value` | Specifies the function identifier. |

---

### `M_TU_NAME + n`

**Board availability:** Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the name of the TU at index _n_, where _n_ is a number between 0 and the value returned by [`M_NUMBER_OF_TU`](../../Reference/fpga/MfpgaInquire.md).

| Value | Description |
| --- | --- |
| `String` | Specifies the name of the TU at index _n_. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

## Return Value

**Type:** `AIL_INT`

The returned value is `M_VALID` if successful. If the operation fails, `M_NULL` is returned.
