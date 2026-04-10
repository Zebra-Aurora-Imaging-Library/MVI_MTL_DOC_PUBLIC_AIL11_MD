---
doctype: Reference
module: sysIo
function: MsysIoFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sysIo / MsysIoFree"
---

# MsysIoFree

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | No |
| Clarity UHD | No |
| Concord PoE | Partial |
| GenTL | No |
| GevIQ | No |
| GigE Vision | No |
| Indio | Partial |
| Iris GTX | Yes |
| Radient eV-CL | No |
| Rapixo CL | No |
| Rapixo CoF | No |
| Rapixo CXP | No |
| USB3 Vision | No |

> Free the specified I/O command list.

## Syntax

```c
void MsysIoFree(
    AIL_ID IoCmdListSysId  //in
)
```

## Description

This function deallocates the specified I/O command list previously allocated with [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md).

All specified I/O command lists allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/sysIo/MsysIoAlloc.md) was specified during allocation.

### System specific

| Board(s) | Note |
|---|---|
| Concord PoE, Host System, Indio | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

## Parameters

### `IoCmdListSysId` *(in, AIL_ID)*

Specifies the identifier of the I/O command list to free.

On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7.
