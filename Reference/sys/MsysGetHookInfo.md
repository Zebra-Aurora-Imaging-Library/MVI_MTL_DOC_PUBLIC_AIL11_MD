---
doctype: Reference
module: sys
function: MsysGetHookInfo
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sys / MsysGetHookInfo"
---

# MsysGetHookInfo

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | Partial |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | Partial |
| Iris GTX | Partial |
| Radient eV-CL | No |
| Rapixo CL | No |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Get information about a hook event.

## Syntax

```c
AIL_INT MsysGetHookInfo(
    AIL_ID    SysId,        //in
    AIL_ID    EventId,      //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function allows you to get information about the event that caused the hook function to be called. [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) should only be called within the scope of a system hook-handler function (see [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md)).

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system.

*For the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `EventId` *(in, AIL_ID)*

Specifies the system event identifier received by the hook-handler function (see [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md)).

### `InquireType` *(in, AIL_INT64)*

Specifies the type of information about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For retrieving information about a camera-specific event

The following allows you to retrieve information about a camera-specific event. The following information types are only available if [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) was called from a function hooked to a system event using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with[`M_CAMERA_PRESENT`](../../Reference/sys/MsysHookFunction.md).

---

### `M_CAMERA_PRESENT`

**Board availability:** Clarity UHD, GevIQ, GigE Vision, Host System, Iris GTX, Rapixo CXP, Rapixo CoF, USB3 Vision

Retrieves whether a camera has been added or re-connected to the system.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that a camera has been removed from the system. |
| `M_TRUE` | Specifies that a camera has been added or re-connected to the system. |

---

### `M_GC_REMOTE_IP_ADDRESS_STRING`

**Board availability:** GevIQ, GigE Vision

Retrieves the IP address (IPv4) of the camera that generated the [`M_CAMERA_PRESENT`](../../Reference/sys/MsysHookFunction.md) event.

| Value | Description |
| --- | --- |
| `"nnn.nnn.nnn.nnn"` | Specifies the IP address of the camera as a string. The address string is expressed in dotted decimal notation, where each dotted decimal value (_nnn_) is a number between 0 and 255. |

---

### `M_GC_REMOTE_MAC_ADDRESS_STRING`

**Board availability:** GevIQ, GigE Vision

Retrieves the MAC address of the camera that generated the [`M_CAMERA_PRESENT`](../../Reference/sys/MsysHookFunction.md) event.

| Value | Description |
| --- | --- |
| `"nn-nn-nn-nn-nn-nn"` | Specifies the MAC address of the camera, as a string. The address string is expressed in hexadecimal pairs, where each pair (_nn_) is a hexadecimal number between 00 and FF. |

---

### `M_GC_UNIQUE_ID_STRING`

**Board availability:** GevIQ, GigE Vision, Host System, Iris GTX, USB3 Vision

Retrieves the unique identifier for your camera.

#### System specific

| Board(s) | Note |
|---|---|
| GevIQ, GigE Vision | This is the camera's MAC address. |
| USB3 Vision | This is the camera's global unique identifier (GUID). |

| Value | Description |
| --- | --- |
| `"nnnnnnnnnnnn"` | Specifies the unique identifier, as a string. The string is expressed in hexadecimal. |

---

### `M_GC_USER_NAME`

**Board availability:** GevIQ, GigE Vision, Host System, Iris GTX, USB3 Vision

Retrieves the user-defined name of the camera that generated the [`M_CAMERA_PRESENT`](../../Reference/dig/MdigInquire.md) event.

| Value | Description |
| --- | --- |
| `"CameraName"` | Specifies the name of the camera. |

---

### `M_NUMBER`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Retrieves the device number of the digitizer to which a camera has been connected or disconnected from, causing an [`M_CAMERA_PRESENT`](../../Reference/dig/MdigHookFunction.md) event.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default board. |
| `M_DEVn` | Specifies the device number (rank) of the board. |
| `"BoardIdentifierString"` | Specifies the user-defined name of the board. |
| `M_GENTL_PRODUCER` | Specifies the GenTL Producer (library) for which to allocate this Aurora Imaging Library GenTL system. |
| `0 <= Value <= 127` | Specifies the index. |

---

### `M_TIME_STAMP`

**Board availability:** Concord PoE, GenTL, GevIQ, GigE Vision, Host System, Indio, Iris GTX, USB3 Vision, V4L2

Retrieves the operating system's time stamp at the point which the event generated a service request. The time stamp is generated by the operating system's performance counter. Time stamps can be used to see if a frame has been missed since the last time stamp.

| Value | Description |
| --- | --- |
| `Value` | Specifies operating system's time stamp, in sec. |

### Combination Constants — For identifying the instance of the GenTL library to use

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to identify the GenTL library for which an Aurora Imaging Library GenTL system was allocated.

All third-party proprietary GenTL libraries installed on your computer are sorted and indexed by Aurora Imaging Library. Use the GenTL Producer index number to identify the library that your Aurora Imaging Library system should use to communicate with the hardware devices on the specified transportation layer. These libraries are indexed and sorted for reference by Aurora Imaging Library (the GenTL Consumer). In all cases, the same number and type of libraries should be installed on every computer that will run your application; the order in which they are installed, however, is not important.  To determine the number of GenTL libraries installed on your computer, use [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_GENTL_PRODUCER_COUNT`](../../Reference/app/MappInquire.md) or use the **General Default Values** page of the Aurora Imaging Configurator utility.

> **Board availability:** GenTL, V4L2

| Value | Description |
| --- | --- |
| `M_GENTL_PRODUCER` | Specifies the GenTL Producer (library) for which this Aurora Imaging Library GenTL system was allocated. |

### For retrieving information about a timer start or timer end event

The following allows you to retrieve information about a timer event. The following information types are only available if [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) was called from a function hooked to an event using [`M_TIMER_START`](../../Reference/sys/MsysHookFunction.md) or [`M_TIMER_END`](../../Reference/sys/MsysHookFunction.md).

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_TIMER_INDEX`

Retrieves the index value of the timer that generated an [`M_TIMER_END`](../../Reference/sys/MsysHookFunction.md) or an[`M_TIMER_START`](../../Reference/sys/MsysHookFunction.md) event.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the value of the index of the timer. |

---

### `M_TIMER_VALUE`

Retrieves the value of the specified timer.  This is typically used with a timer end event (set using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_TIMER_END`](../../Reference/sys/MsysHookFunction.md)). Note that, if used with a timer start event ([`M_TIMER_START`](../../Reference/sys/MsysHookFunction.md)), it will return 0.  To inquire the value of the timer that caused the event, use [`M_TIMER_INDEX`](../../Reference/sys/MsysGetHookInfo.md) to learn which timer to inquire.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the timer, in nsec. |

### Combination Constants — For specifying the timer to inquire about

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the timer to inquire about.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_TIMERn` | Specifies the timer to inquire about. |

### For retrieving information about an I/O change event

The following allows you to retrieve information about an I/O change event. The following information types are only available if [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) was called from a function hooked to an event using [`M_IO_CHANGE`](../../Reference/sys/MsysHookFunction.md).  If more than one input signal triggers an interrupt at the same time, the hook-handler function (or chain of hook-handler functions) is called for each input signal that generated an interrupt. As a result, any and all user-specified function(s) hooked using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_IO_CHANGE`](../../Reference/sys/MsysHookFunction.md) will be executed for each interrupt.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_IO_INTERRUPT_SOURCE`

Retrieves the input signal that triggered the hook function.  See the _Aurora Imaging Library Hardware-specific Notes_ for a listing of available auxiliary signals and their corresponding numbers in Aurora Imaging Library.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies that auxiliary signal _n_ triggered the hook function, where _n_ is the number of the auxiliary signal. |

---

### `M_IO_STATUS`

Retrieves the state of the input signal that generated the interrupt. The returned value reflects the state of the input signal at the time of the interrupt.

| Value | Description |
| --- | --- |
| `M_OFF` | Specifies that the I/O signal is off. |
| `M_ON` | Specifies that the I/O signal is on. |

### For retrieving information about a data latch value

The following allows you to retrieve information about a data latch event. The following information types are only available if using a latch that has previously saved the time, I/O command list counter, or rotary decoder position counter value upon a hardware event.

---

### `M_REFERENCE_LATCH_VALUE`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Retrieves the last timestamp or counter value stored by the specified latch, associated with the specified I/O command list.  This setting is available in functions hooked to any type of event.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the last timestamp or counter value stored by the specified latch, associated with the specified command list. |

---

### `M_SYS_DATA_LATCH_VALUE`

**Board availability:** Host System

Retrieves the last rotary decoder position counter value stored by the specified latch, associated with the rotary decoders of the system.  > **Note:** This setting is only available if [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) was called from a function hooked to an event using [`M_TIMER_START`](../../Reference/sys/MsysHookFunction.md), [`M_TIMER_END`](../../Reference/sys/MsysHookFunction.md), or [`M_IO_CHANGE`](../../Reference/sys/MsysHookFunction.md).

#### System specific

| Board(s) | Note |
|---|---|
| System specific | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the last rotary decoder position counter value stored by the specified latch, associated with the rotary decoders of the system. |

### Combination Constants — For specifying the I/O command list to use

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which of the I/O command lists to use.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_IO_COMMAND_LISTn` | Specifies to inquire about I/O command list _n_. |

### Combination Constants — For specifying the latch to inquire

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which latch to inquire about.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_LATCHn` | Specifies to inquire about latch _n_. |

### For retrieving information from a GenTL feature change event

The following allows you to retrieve information from a GenTL feature change event. These information types are only available if [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) was called from a function hooked to GenICam feature change event using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_FEATURE_CHANGE`](../../Reference/sys/MsysHookFunction.md).

> **Board availability:** GenTL, V4L2

---

### `M_GC_FEATURE_CHANGE_NAME`

Retrieves the name of the GenTL feature that changed. This information type is only available if[`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) is used from a function hooked to GenTL feature change events using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_FEATURE_CHANGE`](../../Reference/sys/MsysHookFunction.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the feature name. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

> **Board availability:** GenTL, GigE Vision, Host System, Iris GTX, USB3 Vision, V4L2

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (!`M_NULL`) value is returned.

This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7.

For Zebra 4Sight EV6/EV7, _n_ can be either 1 or 2.

For Zebra 4Sight EV6/EV7, _n_ is a number from 1 to 4, unless used with [`M_SYS_DATA_LATCH_VALUE`](../../Reference/sys/MsysGetHookInfo.md) (in which case,_n_ is a number from 1 to 2).
