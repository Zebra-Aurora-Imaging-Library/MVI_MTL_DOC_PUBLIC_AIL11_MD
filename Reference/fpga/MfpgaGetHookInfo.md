---
doctype: Reference
module: fpga
function: MfpgaGetHookInfo
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaGetHookInfo"
---

# MfpgaGetHookInfo

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

> Get information about a PU hook event.

## Syntax

```c
AIL_INT MfpgaGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function allows you to get information about the event that caused the hook-handler function to be called. The [`MfpgaGetHookInfo`](../../Reference/fpga/MfpgaGetHookInfo.md) function should only be called within the scope of a PU hook-handler function (see[`MfpgaHookFunction`](../../Reference/fpga/MfpgaHookFunction.md)).

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the PU event identifier received by the hook-handler function (see [`MfpgaHookFunction`](../../Reference/fpga/MfpgaHookFunction.md)).

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information to get.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For inquiring information

---

### `M_FPGA_DEVICE_NUMBER`

Retrieves the device number of the Processing FPGA that contains the PU that triggered the event. This value corresponds to the rank of the Processing FPGA on the board, starting from 0. If there is only one Processing FPGA on-board, then this value will be set to 0 by default.

---

### `M_FUNCTION_ID`

Retrieves the function identifier of the PU that triggered the event.

---

### `M_INSTANCE_ID`

Retrieves the rank of the instance of the PU that triggered the event, when two or more instances, with the same function and subfunction identifier, are present in the loaded FPGA configuration. This value starts at 0.

---

### `M_SUB_FUNCTION_ID`

Retrieves the subfunction identifier of the PU that triggered the event.

---

### `M_TIME_STAMP`

Retrieves the time stamp of the event.

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (\![`M_NULL`](../../Reference/fpga/MfpgaGetHookInfo.md)) value is returned.
