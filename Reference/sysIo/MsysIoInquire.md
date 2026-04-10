---
doctype: Reference
module: sysIo
function: MsysIoInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sysIo / MsysIoInquire"
---

# MsysIoInquire

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

> Inquire about an I/O command list setting.

## Syntax

```c
AIL_INT MsysIoInquire(
    AIL_ID    IoCmdListSysId,  //in
    AIL_INT64 InquireType,     //in
    void *    UserVarPtr       //out
)
```

## Description

This function inquires about the specified I/O command list setting.

Note that you can use [`MsysIoControl`](../../Reference/sysIo/MsysIoControl.md) to control specific I/O command list settings.

### System specific

| Board(s) | Note |
|---|---|
| Concord PoE, Host System, Indio | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

## Parameters

### `IoCmdListSysId` *(in, AIL_ID)*

Specifies the identifier of the I/O command list.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of I/O command list setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring general I/O command list settings

The following inquire types allow you to inquire general I/O command list settings.

---

### `M_CLOCK_FREQUENCY`

Inquires the frequency of the I/O command list's clock signal.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the frequency of the I/O command list's clock, in Hz. |

---

### `M_IO_COMMAND_COUNTER_ACTIVATION`

Inquires which edge of the source signal is used to increment the I/O command list's internal counter.

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

---

### `M_IO_COMMAND_COUNTER_SOURCE`

Inquires the signal to use as the counter source.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the counter source, where _n_ is the number for one of the auxiliary input signals. |
| `M_CLOCK` *(default)* | Specifies to use your hardware's clock signal as a counter source. |
| `M_ROTARY_ENCODERn` | Specifies to use the output of rotary decoder _n_ as the counter source, where _n_ is the number of the rotary decoder. |

---

### `M_IO_COMMAND_LIST_NUMBER`

Inquires which I/O command list is allocated.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_IO_COMMAND_LIST1` *(default)* | Allocates the first I/O command list. |
| `M_IO_COMMAND_LIST2` | Allocates the second I/O command list. |

---

### `M_IO_OBJECT_TYPE`

Inquires the type of I/O command list that is allocated.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_IO_COMMAND_LIST` | Specifies a general I/O command list. |

---

### `M_OWNER_SYSTEM`

Inquires the Aurora Imaging Library identifier (_AIL_ID_) of the system on which the I/O command list has been allocated.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_REFERENCE_VALUE`

Inquires the I/O command list's internal counter value at the current moment (that is, the moment [`MsysIoInquire`](../../Reference/sysIo/MsysIoInquire.md) is called).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the I/O command list's internal counter value. |

### For inquiring reference latch settings

The following inquire types allow you to inquire settings for one of the I/O command list's reference latches.

---

### `M_REFERENCE_LATCH_ACTIVATION`

Inquires the signal transition upon which to store the time or counter value to the specified reference latch.

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

Inquires the state of the specified reference latch.

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

Inquires which input signal will trigger storing the time or counter value to the specified reference latch.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal_n_, where _n_ is a number of the auxiliary input signal. |
| `M_EXPOSURE` | Specifies to route the exposure signal of the camera. |
| `M_GRAB_TRIGGER_READY` | Specifies to route the internal grab trigger ready signal. |
| `M_TIMER_STROBE` | Specifies to route the internal timer strobe signal. |
| `M_TIMERn` | Specifies to use timer_n_, where _n_ is a number of the timer. |

---

### `M_REFERENCE_LATCH_VALUE`

Inquires the last time or counter value stored by the specified reference latch.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the last timestamp or counter value stored by the specified reference latch. |

### Combination Constants — For specifying the reference latch to inquire

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which reference latch to inquire about.

| Value | Description |
| --- | --- |
| `M_LATCHn` | Specifies to inquire about reference latch _n_, where _n_ is the latch number. |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

On Zebra 4Sight EV6/EV7, this function is only available if an Aurora Imaging Library Host system was previously allocated. It is not available on Zebra 4Sight XV6/XV7.
