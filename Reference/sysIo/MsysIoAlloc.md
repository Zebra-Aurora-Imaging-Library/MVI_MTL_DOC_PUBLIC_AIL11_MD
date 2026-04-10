---
doctype: Reference
module: sysIo
function: MsysIoAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sysIo / MsysIoAlloc"
---

# MsysIoAlloc

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
| Iris GTX | Partial |
| Radient eV-CL | No |
| Rapixo CL | No |
| Rapixo CoF | No |
| Rapixo CXP | No |
| USB3 Vision | No |

> Allocate an Aurora Imaging Library I/O command list.

## Syntax

```c
AIL_ID MsysIoAlloc(
    AIL_ID    SysId,             //in
    AIL_INT64 IoCmdListNum,      //in
    AIL_INT64 Type,              //in
    AIL_INT64 CounterSrc,        //in
    AIL_ID *  IoCmdListSysIdPtr  //out
)
```

## Description

This function allocates an Aurora Imaging Library I/O command list and initializes it. An I/O command list allows you to schedule commands to change the state of a bit of an I/O command register at a specified time or counter value. You can route the state of the bit, for example, to an auxiliary output signal to control a connected device at a required moment.

To schedule commands, the I/O command list uses an internal counter to count clock ticks or transitions that occur on a signal specified as the counter source. When allocating an I/O command list, you must specify the counter source that will be used to schedule commands. The counter source can be either a clock, which allows you to schedule commands in time, or a specified signal, which allows you to schedule commands based on the number of transitions that occur on the signal. The latter case allows you to schedule commands, for example, based on the positional information provided by the output signal of a rotary decoder. To schedule commands with the I/O command list, use [`MsysIoCommandRegister`](../../Reference/sysIo/MsysIoCommandRegister.md).

To modify or inquire I/O command list settings, use [`MsysIoControl`](../../Reference/sysIo/MsysIoControl.md) and [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md).

After allocating the Aurora Imaging Library I/O command list, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the Aurora Imaging Library I/O command list identifier returned is not [`M_NULL`](../../Reference/sysIo/MsysIoAlloc.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/sysIo/MsysIoAlloc.md) was specified).

When the Aurora Imaging Library I/O command list is no longer required, release it using[`MsysIoFree`](../../Reference/sysIo/MsysIoFree.md)unless [`M_UNIQUE_ID`](../../Reference/sysIo/MsysIoAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/sysIo/MsysIoAlloc.md) was specified, the smart identifier manages the Aurora Imaging Library I/O command list's lifetime and you must not manually free it.

### System specific

| Board(s) | Note |
|---|---|
| Concord PoE, Host System, Indio | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library system on which to allocate the I/O command list.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `IoCmdListNum` *(in, AIL_INT64)*

Specifies the index of the I/O command list to allocate. Note that you cannot allocate a command list with the same index as one that has already been allocated. This parameter can be set to one of the following values:

*For specifying the I/O command list's index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_IO_COMMAND_LIST1` *(default)* | Allocates the first I/O command list. |
| `M_IO_COMMAND_LIST2` | Allocates the second I/O command list. |

### `Type` *(in, AIL_INT64)*

Specifies the type of I/O command list to allocate. This parameter can be set to the following value:

*For specifying the type of I/O command list*

| Value | Description |
| --- | --- |
| `M_IO_COMMAND_LIST` | Specifies a general I/O command list. |

### `CounterSrc` *(in, AIL_INT64)*

Specifies the signal to use as the counter source. The counter source will affect the units that you should use to specify, for example, when a registered command should be executed ([`MsysIoCommandRegister`](../../Reference/sysIo/MsysIoCommandRegister.md)). This parameter can be set to one of the following values:

*For specifying the counter source*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the counter source, where _n_ is the number for one of the auxiliary input signals. When specifying this counter source, schedule commands in terms of number of transitions that occur on the input signal. To change which signal transition will increment the counter, use [`MsysIoControl`](../../Reference/sysIo/MsysIoControl.md) with [`M_IO_COMMAND_COUNTER_ACTIVATION`](../../Reference/sysIo/MsysIoControl.md) (by default, it is set to low-to-high transitions). |
| `M_CLOCK` *(default)* | Specifies to use your hardware's clock signal as a counter source. When specifying this counter source, schedule commands in terms of seconds.

The specified number of seconds will be internally converted to the same units as the clock. To inquire the frequency of the clock signal, use [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md) with [`M_CLOCK_FREQUENCY`](../../Reference/sysIo/MsysIoInquire.md). |
| `M_ROTARY_ENCODERn` | Specifies to use the output of rotary decoder _n_ as the counter source, where _n_ is the number of the rotary decoder. When specifying this counter source, schedule commands in terms of number of transitions that occur on the rotary decoder's output signal ([`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/sys/MsysControl.md)). |

### `IoCmdListSysIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the I/O command list identifier or specifies the data type that the function should use to return the I/O command list identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated I/O command list; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated I/O command list; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_SYSIO_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the I/O command list (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated I/O command list.

If allocation fails, [`M_NULL`](../../Reference/sysIo/MsysIoAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the I/O command list identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_SYSIO_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/sysIo/MsysIoAlloc.md) was specified).

On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7.

For Zebra 4Sight EV6/EV7, _n_ can be a value from 8 to 15.

For Zebra 4Sight EV6/EV7, _n_ can be either 1 or 2.
