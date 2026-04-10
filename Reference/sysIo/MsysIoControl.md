---
doctype: Reference
module: sysIo
function: MsysIoControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sysIo / MsysIoControl"
---

# MsysIoControl

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

> Control an I/O command list setting.

## Syntax

```c
void MsysIoControl(
    AIL_ID     IoCmdListSysId,  //out
    AIL_INT64  ControlType,     //in
    AIL_DOUBLE ControlValue     //in
)
```

## Description

This function allows you to control an I/O command list setting.

To inquire the current value of a particular I/O command list setting, use [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md).

### System specific

| Board(s) | Note |
|---|---|
| Concord PoE, Host System, Indio | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

## Parameters

### `IoCmdListSysId` *(out, AIL_ID)*

Specifies the identifier of the I/O command list.

### `ControlType` *(in, AIL_INT64)*

Specifies the type of I/O command list setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the new value to assign to the I/O command list setting specified by the [`ControlType`](../../Reference/sysIo/MsysIoControl.md) parameter.

## Parameter Associations

### For controlling I/O command list settings

The following control type allows you to control an I/O command list setting.

---

### `M_IO_COMMAND_CANCEL`

Clears the specified I/O command list of any command that affects the specified I/O command register bits.

#### System specific

| Board(s) | Note |
|---|---|
| System specific | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_IO_COMMAND_BIT_MASK` | Specifies I/O command register bits using a bit-encoded value, in which each bit represents one I/O command register bit. |
| `M_IO_COMMAND_BITn` | Specifies the I/O command register bit _n_, where _n_ is a number from 0 to 7. |

---

### `M_IO_COMMAND_COUNTER_ACTIVATION`

Sets which edge of the source signal to use to increment the I/O command list's internal counter. This setting should be changed only when the I/O command list is allocated using [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md) with [`M_AUX_IOn`](../../Reference/sysIo/MsysIoAlloc.md).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies to use both a low-to-high and a high-to-low signal transition to increment the I/O command list's internal counter. |
| `M_EDGE_FALLING` | Specifies to use a high-to-low signal transition to increment the I/O command list's internal counter. |
| `M_EDGE_RISING` *(default)* | Specifies to use a low-to-high signal transition to increment the I/O command list's internal counter. |

### For controlling an I/O command list reference latch

The following control types allow you to control settings for one of the I/O command list's reference latches. Reference latches are used to store a time or counter value when the required signal transition occurs on a specified input signal. You can then inquire the time or counter value saved in the latch using [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md) with [`M_REFERENCE_LATCH_VALUE`](../../Reference/sysIo/MsysIoInquire.md).

---

### `M_REFERENCE_LATCH_ACTIVATION`

Sets the signal transition upon which to store the time or counter value to the specified reference latch.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies to store the time or counter value to the latch upon both a low-to-high and a high-to-low signal transition. |
| `M_EDGE_FALLING` | Specifies to store the time or counter value to the latch upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies to store the time or counter value to the latch upon a low-to-high signal transition. |

---

### `M_REFERENCE_LATCH_STATE`

Sets the state of the specified reference latch.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies the reference latch is disabled. |
| `M_ENABLE` | Specifies the reference latch is enabled. |

---

### `M_REFERENCE_LATCH_TRIGGER_SOURCE`

Sets which input signal will trigger storing the timestamp or counter value to the specified reference latch.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal_n_, where _n_ is a number of the auxiliary input signal. |
| `M_EXPOSURE` | Specifies to route the exposure signal of the camera. **[Iris GTX]** |
| `M_GRAB_TRIGGER_READY` | Specifies to route the internal grab trigger ready signal. **[Iris GTX]** |
| `M_TIMER_STROBE` | Specifies to route the internal timer strobe signal. **[Iris GTX]** |
| `M_TIMERn` | Specifies to use timer_n_, where _n_ is a number of the timer. |

### Combination Constants — For specifying the reference latch to affect

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which reference latch to affect.

| Value | Description |
| --- | --- |
| `M_LATCHn` | Specifies to affect reference latch _n_, where _n_ is the latch number. |

On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7.

For Zebra 4Sight EV6/EV7, _n_ can be a value from 8 to 15.

For Zebra 4Sight EV6/EV7, _n_ can be either 1 or 2.

For Zebra 4Sight EV6/EV7, _n_ is a number from 1 to 4.
