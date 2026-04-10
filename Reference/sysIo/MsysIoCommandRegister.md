---
doctype: Reference
module: sysIo
function: MsysIoCommandRegister
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sysIo / MsysIoCommandRegister"
---

# MsysIoCommandRegister

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

> Add a command to change the state of a bit of the I/O command register at a specified time or counter value.

## Syntax

```c
AIL_INT MsysIoCommandRegister(
    AIL_ID     IoCmdListSysId,      //out
    AIL_INT64  Operation,           //in
    AIL_INT64  Reference,           //in
    AIL_DOUBLE DelayFromReference,  //in
    AIL_DOUBLE Duration,            //in
    AIL_INT64  BitToOperate,        //in
    void *     CommandStatusPtr     //out
)
```

## Description

This function adds (registers) a command to the I/O command list to change the state of a specified bit of the I/O command register at a specified moment (time or counter value). To route the state of an I/O command register bit to an output signal, use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_SOURCE`](../../Reference/sys/MsysControl.md) before you change the state of the bit.

When adding a command, you must specify when it must be executed. If the command list was allocated using [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md) with [`M_CLOCK`](../../Reference/sysIo/MsysIoAlloc.md), specify the time at which to execute the command in seconds after a specified reference time; otherwise, specify it in number of counter increments after a specified reference counter value. For example, if you want to execute a command 50 msecs after the moment [`MsysIoCommandRegister`](../../Reference/sysIo/MsysIoCommandRegister.md) is called, you would specify [`M_REFERENCE_VALUE_CURRENT`](../../Reference/sysIo/MsysIoCommandRegister.md) as the reference time and 0.05 seconds as the amount of time after this reference time to execute the command.

When adding a command to change an I/O command register bit such that a pulse is generated on the associated signal, two commands are actually added; one command to transition the signal at the specified moment and one command to transition the signal back to its original state after the specified pulse duration. Note that if you want to specify the duration of the pulse in seconds, but [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md) with [`M_CLOCK`](../../Reference/sysIo/MsysIoAlloc.md) is not set as the counter source, use a timer to generate the pulse and the I/O command list to trigger the timer (generation of the pulse); schedule an [`M_IMPULSE`](../../Reference/sysIo/MsysIoCommandRegister.md) command instead and use the affected I/O command register bit as the trigger source of the timer.

Multiple commands can be scheduled to execute at the same time or counter value, as long as the commands are affecting different I/O command register bits. If a command has been registered with the I/O command list to affect a specific I/O command register bit, and you attempt to add a different command to affect the same bit at the same specified time or counter value, the original command will be overwritten.

### System specific

| Board(s) | Note |
|---|---|
| Concord PoE, Host System, Indio | On an Aurora Imaging Library Host system, this function is only available when using Zebra 4Sight EV6/EV7. |

## Parameters

### `IoCmdListSysId` *(out, AIL_ID)*

Specifies the identifier of the I/O command list in which to add a command.

### `Operation` *(in, AIL_INT64)*

Specifies the command to add to the I/O command list to change the state of the specified I/O command register bit. This parameter can be set to one of the following values:

*For specifying the command to register*

| Value | Description |
| --- | --- |
| `M_EDGE_FALLING` | Specifies that the command will change the specified bit such that the associated signal will transition from high to low, if it is high. |
| `M_EDGE_RISING` | Specifies that the command will change the specified bit such that the associated signal will transition from low to high, if it is low. |
| `M_IMPULSE` | Specifies the command will change the specified bit such that the associated signal will produce the shortest possible active-high pulse on the specified I/O command list bit. An active-high pulse is a low-to-high signal transition followed by a high-to-low signal transition. Unlike [`M_PULSE_HIGH`](../../Reference/sysIo/MsysIoCommandRegister.md), the [`Duration`](../../Reference/sysIo/MsysIoCommandRegister.md) parameter must be set to [`M_DEFAULT`](../../Reference/sysIo/MsysIoCommandRegister.md) and a single command producing the shortest possible pulse is generated. This operation should not be output to external devices since it might be filtered out as noise. It can, however, be used to trigger internal hardware devices, such as timers. |
| `M_NONE` | Specifies no operation is performed. This operation can be used to cancel a command previously added to the I/O command list. |
| `M_PULSE_HIGH` | Specifies to add two commands to the I/O command list, an [`M_EDGE_RISING`](../../Reference/sysIo/MsysIoCommandRegister.md) and an [`M_EDGE_FALLING`](../../Reference/sysIo/MsysIoCommandRegister.md) command, to change the specified bit such that the associated signal will produce an active-high pulse. An active-high pulse is a low-to-high signal transition followed by a high-to-low signal transition; specify the length of the pulse using the [`Duration`](../../Reference/sysIo/MsysIoCommandRegister.md) parameter. Note that if the signal is already high, it will remain high for the specified duration and then transition to low (unless a command is added to transition it to low earlier). |
| `M_PULSE_LOW` | Specifies to add two commands to the I/O command list, an [`M_EDGE_FALLING`](../../Reference/sysIo/MsysIoCommandRegister.md) and an [`M_EDGE_RISING`](../../Reference/sysIo/MsysIoCommandRegister.md) command, to change the specified bit such that the associated signal will produce an active-low pulse. An active-low pulse is a high-to-low signal transition followed by a low-to-high signal transition; specify the length of the pulse using the [`Duration`](../../Reference/sysIo/MsysIoCommandRegister.md) parameter. Note that if the signal is already low, it will remain low for the specified duration and then transition to high (unless a command is added to transition it to high earlier). |

*For automatically adding operations to the I/O command list*

| Value | Description |
| --- | --- |
| `M_AUTO_REGISTER` | Specifies to automatically add the specified command to the I/O command list every time the latch used as a reference ( [`M_LATCHn`](../../Reference/sysIo/MsysIoCommandRegister.md)) is triggered. |
| `M_AUTO_REGISTER_CANCEL` | Specifies to stop automatically adding the specified command to the I/O command list every time the latch used as a reference ( [`M_LATCHn`](../../Reference/sysIo/MsysIoCommandRegister.md)) is triggered. |

### `Reference` *(in, AIL_INT64)*

Specifies the reference time or counter value to which the delay is added. This parameter can be set to one of the following values:

*For specifying the reference timestamp*

| Value | Description |
| --- | --- |
| `M_LATCHn` | Specifies to schedule the operation relative to the current value of reference latch _n_, where _n_ is a number from 1 to 4.

Note that [`M_LATCHn`](../../Reference/sysIo/MsysIoCommandRegister.md) must be used with either [`M_AUTO_REGISTER`](../../Reference/sysIo/MsysIoCommandRegister.md) or[`M_AUTO_REGISTER_CANCEL`](../../Reference/sysIo/MsysIoCommandRegister.md). |
| `M_REFERENCE_VALUE_CURRENT` | Specifies to schedule the command relative to the current moment (that is, the moment [`MsysIoCommandRegister`](../../Reference/sysIo/MsysIoCommandRegister.md) is called). |
| `Value` | Specifies the time or counter value relative to which to schedule the command. This is typically a previously inquired time or counter value. You can inquire the time or counter value at a specific moment during the execution of your program using [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md) with [`M_REFERENCE_VALUE`](../../Reference/sysIo/MsysIoInquire.md). Additionally, if using a latch that has previously saved the time or counter value upon a hardware event, you can inquire this using [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md) with [`M_REFERENCE_LATCH_VALUE`](../../Reference/sysIo/MsysIoInquire.md) or within the scope of a system hook-handler function using [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) with [`M_REFERENCE_LATCH_VALUE`](../../Reference/sys/MsysGetHookInfo.md). |

### `DelayFromReference` *(in, AIL_DOUBLE)*

Specifies the delay to add to the reference time or counter value; the sum of the two establishes the specific moment that the command is executed. If the I/O command list was allocated using [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md) with [`M_CLOCK`](../../Reference/sysIo/MsysIoAlloc.md) as the counter source, specify the delay in seconds. Otherwise, specify it in number of counter increments.

### `Duration` *(in, AIL_DOUBLE)*

Specifies the duration of the pulse if the command specified with [`Operation`](../../Reference/sysIo/MsysIoCommandRegister.md) is [`M_PULSE_HIGH`](../../Reference/sysIo/MsysIoCommandRegister.md) or [`M_PULSE_LOW`](../../Reference/sysIo/MsysIoCommandRegister.md). If the I/O command list was allocated with [`M_CLOCK`](../../Reference/sysIo/MsysIoAlloc.md) as the counter source, specify the delay in seconds. Otherwise, specify it in number of counter increments. When adding other types of commands, set this parameter to `M_DEFAULT`.

### `BitToOperate` *(in, AIL_INT64)*

Specifies the bit of the I/O command register to affect with the command. This parameter can be set to the following value:

*For specifying the bit of the I/O command register to operate*

| Value | Description |
| --- | --- |
| `M_IO_COMMAND_BITn` | Specifies that the command must affect bit _n_ of the I/O command register, where _n_ can be a value of 0 to 7.

Note that, if you need to route the state of the bit to an auxiliary signal, use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_SOURCE`](../../Reference/sys/MsysControl.md) before you change the state of the bit.

When performing multiple automatically registered operations (using [`M_AUTO_REGISTER`](../../Reference/sysIo/MsysIoCommandRegister.md)), each automatically registered operation must affect a unique I/O command register bit; otherwise, the last automatically registered operation will override the previous that affects the same register bit. |

### `CommandStatusPtr` *(out, *void)*

Specifies the address in which to write whether the command was successfully added to the list. You can set this parameter to **M_NULL** if this information is not required.

*For returning the status of the command registration*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the function successfully scheduled the command. |
| `M_INVALID` | Specifies that the moment at which to execute the command had elapsed when the function was executed. |
| `M_UNKNOWN` | Specifies that the function encountered an error and could not schedule the command. |

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if the function successfully scheduled the command, `M_INVALID` if the moment at which to execute the command had elapsed when the function was executed, and `M_UNKNOWN` if the function encountered an error or if the [`CommandStatusPtr`](../../Reference/sysIo/MsysIoCommandRegister.md) parameter was set to `M_NULL`.

On an Aurora Imaging Library Host system, this function is only available when using Zebra 4Sight EV6/EV7.

For Zebra 4Sight EV6/EV7, _n_ is a number from 1 to 4.
