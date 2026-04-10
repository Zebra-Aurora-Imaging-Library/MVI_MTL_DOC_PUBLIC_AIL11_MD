---
doctype: Reference
module: sys
function: MsysControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sys / MsysControl"
---

# MsysControl

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
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Control a system setting.

## Syntax

```c
void MsysControl(
    AIL_ID     SysId,        //out
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function allows you to control the specified system setting.

> **Note:** Note that, when a control type has only one supported control value on a given system, it is not documented in this function because it cannot be changed to another value. Instead, it can only be inquired.

To inquire the current value of a particular system setting, use [`MsysInquire`](../../Reference/sys/MsysInquire.md).

You can also interactively control and test most of the system settings in real-time, using Aurora Imaging Intellicam's Feature Browser.

## Parameters

### `SysId` *(out, AIL_ID)*

Specifies the system identifier. This parameter should be set to one of the following values:

*For the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the new value to assign to the system setting specified by the [`ControlType`](../../Reference/sys/MsysControl.md) parameter.

## Parameter Associations

### For general system settings

The following control types allow you to control the general system settings.

---

### `M_ALLOCATION_OVERSCAN`

Sets whether image buffers, allocated on the system, are allocated by default with an overscan region. Specify the size of the overscan region using [`M_ALLOCATION_OVERSCAN_SIZE`](../../Reference/sys/MsysControl.md).  Note that you can override this setting when allocating a buffer using the [`M_ALLOCATION_OVERSCAN`](../../Reference/buf/MbufAlloc2d.md) attribute.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that image buffers allocated on the system will have no overscan region. |
| `M_ENABLE` *(default)* | Specifies that image buffers are allocated on the system with an overscan region. |

---

### `M_ALLOCATION_OVERSCAN_SIZE`

Sets the size of the overscan region, added around all subsequently allocated image buffers ([`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md)). The overscan settings previously allocated image buffers are not changed. For more information, see [Buffer overscan region](../../UserGuide/C23_Data_buffers/S17_Buffer_overscan.md).  To enable or disable the allocation of an overscan region, change the setting of [`M_ALLOCATION_OVERSCAN`](../../Reference/sys/MsysControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default size of the overscan region. |
| `Value` | Specifies the size of the overscan region, in pixels. For example, if you specify a size of 2, a 2-pixel overscan border is added around subsequently allocated image buffers. |

---

### `M_DEBUG_LOG_INFO`

**Board availability:** GevIQ, GigE Vision, Host System, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Logs a user-defined event in [Aurora Imaging Gecho Viewer](../../UserGuide/C01_Tools_and_utilities/S06_Aurora_Imaging_Gecho_Viewer.md). The event will appear in its thread's timeline.  This control can be useful for debugging or monitoring specific sections of code or internal data for your application.

| Value | Description |
| --- | --- |
| `Value` | Specifies the value that will be logged for the user-defined event. |

---

### `M_DEFAULT_PITCH_BYTE_MULTIPLE`

Sets the pitch (or stride) multiple (in bytes) for the buffers allocated on the system. The pitch is the number of bytes between the beginnings of any two adjacent rows of the buffer's data. The pitch multiple is the factor by which the pitch of a buffer must be divisible. When a buffer is allocated, if the specified width is not a multiple of this value, padding is added to the buffer so that the buffer's pitch meets this constraint. You would typically change this value when you need to reduce the padding of your buffer.  Note that, allocating a buffer with an overscan region can also be a cause of buffer padding. For more information, refer to [`M_ALLOCATION_OVERSCAN`](../../Reference/buf/MbufAllocColor.md) and [Accessing an Aurora Imaging Library buffer directly](../../UserGuide/C23_Data_buffers/S13_Accessing_a_buffer_directly.md).  When dealing with an on-board operation, changing the pitch multiple might force on-board operations to be performed off-board due to hardware limitations, thereby increasing the amount of time the operation might take.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value for the pitch multiple. The default value is highly Aurora Imaging Library system-dependent. Always inquire the current pitch multiple before setting it, using [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_DEFAULT_PITCH_BYTE_MULTIPLE`](../../Reference/sys/MsysInquire.md). |
| `Value` | Specifies the pitch multiple, in bytes. |

---

### `M_DEVICE_NAME`

**Board availability:** Concord PoE

Sets a user-defined name for the board. The user-defined name is written to the board, making it persistent, and allows the board to be allocated using this user-defined name in the future. The name is typically assigned and written to the board from a separate executable that allocates a system for the board with [`M_DEVn`](../../Reference/sys/MsysAlloc.md) and then calls [`MsysControl`](../../Reference/sys/MsysControl.md) with the control type ([`M_DEVICE_NAME`](../../Reference/sys/MsysControl.md)).  > **Note:** This control type is also available for the Zebra Concord PoE base model.

| Value | Description |
| --- | --- |
| `"DeviceName"` | Specifies the name of the board. Note that this must be a unique name with a string that can be up to [`M_DEVICE_NAME_MAX_SIZE`](../../Reference/sys/MsysInquire.md) characters. |

---

### `M_GC_FEATURE_BROWSER`

**Board availability:** GenTL

Sets whether to open or close a dialog box that allows you to view and edit the GenTL SFNC-compliant system and interface configuration information interactively. This window is referred to as the Feature Browser.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLOSE` | Closes Feature Browser. |
| `M_OPEN` *(default)* | Opens Feature Browser. |

---

### `M_GC_FEATURE_EXECUTE_POLLING_MODE`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF

Sets whether the executable feature is executed synchronously or asynchronously.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTOMATIC` | Specifies that the specific executable camera feature is executed synchronously.  When in automatic mode, the executable feature is executed synchronously. Aurora Imaging Library polls the feature to establish if the executable feature has completed, returning control only when the operation is complete. Get the polling interval using [`M...InquireFeature`](../../Reference/dig/MdigInquireFeature.md) with [`M_FEATURE_POLLING_INTERVAL`](../../Reference/dig/MdigInquireFeature.md). |
| `M_MANUAL` *(default)* | Specifies that the executable camera feature is executed asynchronously.  When in manual mode, the executable feature is executed asynchronously, and control is returned once the executable operation begins. To determine whether the executable feature completed, use [`M...InquireFeature`](../../Reference/dig/MdigInquireFeature.md) with [`M_FEATURE_EXECUTE_COMPLETED`](../../Reference/dig/MdigInquireFeature.md). |

---

### `M_LED_USER`

**Board availability:** Iris GTX

Sets the color of the user LED on your Zebra Iris GTX.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GREEN` | Specifies to turn the user LED green. |
| `M_OFF` *(default)* | Specifies to turn the user LED off. |
| `M_ORANGE` | Specifies to turn the user LED orange. |
| `M_RED` | Specifies to turn the user LED red. |

---

### `M_MODIFIED_BUFFER_HOOK_MODE`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets whether to run user-defined functions hooked to a buffer modification on separate threads, up to the number of CPU cores present in the computer. This is particularly useful when functions are hooked using [`MdigProcess`](../../Reference/dig/MdigProcess.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MULTI_THREAD` | Specifies to run user-defined functions hooked to a buffer modification on separate threads. The hooked functions are executed by any available CPU core. Aurora Imaging Library cannot guarantee the processing order of any image on such a computer, because they are executed concurrently. If required, use Microsoft Windows functions to synchronize the threads.  > **Note:** Note that by default, this control value will allocate, as necessary, up to 16 threads, or the total number of CPU cores in the computer, whichever is least. |
| `M_SINGLE_THREAD` *(default)* | Specifies that only one thread should be created and that all user-defined functions hooked to buffer modifications are run on the same thread. |

---

### `M_POWER_OVER_CABLE`

**Board availability:** Concord PoE, Host System, Rapixo CXP

Sets whether the board provides power to connected devices.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | For a Host system, this sets whether PoE (Power over Ethernet) is enabled on the specified port.  This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |
| Concord PoE | For Zebra Concord PoE, this sets whether PoE (Power over Ethernet) is enabled on all ports.  > **Note:** This control type is also available for the Zebra Concord PoE base model. |
| Rapixo CXP | For Zebra Rapixo CXP, this sets whether PoCXP (Power over CXP) is automatically enabled when a PoCXP-compliant camera is connected to the specified connector. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_AUTOMATIC` | Specifies to automatically enable or disable PoCXP on this CXP connector, depending on detected device support. **[Rapixo CXP]** |
| `M_OFF` | Specifies not to provide power to connected devices. |
| `M_ON` | Specifies to provide power to connected devices. |
| `M_RESET` | Specifies to reset an over- or under-current condition. To learn whether there is an over- or under-current condition, use [`MsysInquire`](../../Reference/sys/MsysInquire.md)with [`M_POWER_OVER_CABLE_STATUS`](../../Reference/sys/MsysInquire.md).  > **Note:** Typically, to prevent the over- or under-current condition from immediately recurring, you should first restart PoCXP for this connector by setting this control type to [`M_OFF`](../../Reference/sys/MsysControl.md) and then returning it to its previous setting ([`M_AUTOMATIC`](../../Reference/sys/MsysControl.md) is recommended). If the condition persists, discontinue using PoCXP with your camera by leaving this control type set to [`M_OFF`](../../Reference/sys/MsysControl.md). **[Rapixo CXP]** |

---

### `M_THREAD_MODE`

Sets whether threads allocated on the system can execute in asynchronous mode.  If you specify that threads on the system can execute in asynchronous mode, the [`MthrControl`](../../Reference/thr/MthrControl.md) [`M_THREAD_MODE`](../../Reference/thr/MthrControl.md) control type setting of each of its threads is taken into account; otherwise, their [`M_THREAD_MODE`](../../Reference/thr/MthrControl.md) control type setting is ignored.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_ASYNCHRONOUS` | Specifies that threads allocated on the system can execute in asynchronous mode. In asynchronous mode, control is returned to the Host immediately after an Aurora Imaging Library function is sent to the processor of the system (when the system and function allow an immediate return). **[Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |
| `M_SYNCHRONOUS` | Specifies that threads allocated on the system can only execute in synchronous mode. In synchronous mode, the execution of an Aurora Imaging Library function sent to the processor of a system must be completed (execution terminated) before returning control to the Host. |

---

### `M_TIMEOUT`

**Board availability:** Clarity UHD, Concord PoE, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets the maximum amount of time for the Host to wait for a synchronous function to return before generating a time-out error, in sec.  Note that this value only works with functions that are performed on-board, such as [`MbufCopy`](../../Reference/buf/MbufCopy.md) when copying from (or to) on-board memory or functions using an on-board processing FPGA.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_INFINITE` | Waits indefinitely. **[Iris GTX]** |
| `Value` | Specifies the time to wait, in sec. |

### Combination Constants — For specifying which connector or port to control

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which connector or port to control.

> **Board availability:** Host System, Rapixo CXP

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to control all relevant connectors or ports on your board or industrial computer. |
| `M_CONNECTIONn` | Specifies to control connector_n_, where _n_ corresponds to a physical connector or port on your board or industrial computer. |

### Combination Constants — For specifying which configuration information file to access

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify which instance of the GenTL interface configuration file (XML file) is associated with the feature.

> **Board availability:** GenTL

| Value | Description |
| --- | --- |
| `M_GENTL_INTERFACE_NUMBER` | Specifies which instance of the GenTL interface configuration file is associated with the feature. |
| `M_GENTL_SYSTEM` | Specifies to display the GenTL system configuration information. |

### Combination Constants — For specifying whether Feature Browser should be synchronous or asynchronous

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set whether the Feature Browser should be synchronous or asynchronous.

> **Board availability:** GenTL

| Value | Description |
| --- | --- |
| `M_ASYNCHRONOUS` | Specifies that this function returns immediately once a Feature Browser window opens. |
| `M_SYNCHRONOUS` *(default)* | Specifies that this function is blocked until a Feature Browser window closes. |

### Combination Constants — For specifying the number of hook threads

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the number of hook threads to allocate.

> **Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of hook threads to allocate. Note that this value cannot exceed the number of CPU cores available. |

### For discovering connected devices

The following control type allows you to refresh the list of discovered devices.

> **Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

---

### `M_DISCOVER_DEVICE`

Refresh the list of discovered devices that can be accessed by the specified system (for example, all cameras connected to a frame grabber, or all GigE Vision devices accessible on the local subnet).  After you have refreshed the list, you can inquire information about connected devices using [`MsysInquire`](../../Reference/sys/MsysInquire.md)with settings from[`DiscoverySettings`](../../Reference/sys/MsysInquire.md).  > **Note:** Note that this does not affect which devices are available for you to allocate using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md); you do not need to discover a device before allocating it.

#### System specific

| Board(s) | Note |
|---|---|
| GevIQ, GigE Vision, USB3 Vision | This can trigger an[`M_CAMERA_PRESENT`](../../Reference/sys/MsysHookFunction.md) event. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default mode of operation for [`M_DISCOVER_DEVICE`](../../Reference/sys/MsysControl.md). |

### For routing I/O signals and setting their mode

The following control types allow you to set the mode and the routing of your Zebra imaging board's I/O signals (such as, auxiliary signal). Once the routing and mode are determined for an I/O signal, the Aurora Imaging Library function that you should use to act upon an input signal or setup the source of an output signal depends on the functionality. For example, you can use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_USER_BIT...`](../../Reference/sys/MsysControl.md) control types, [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_TRIGGER...`](../../Reference/dig/MdigControl.md) control types, or [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER...`](../../Reference/dig/MdigControl.md) control types.  Note that for other Zebra imaging boards that have auxiliary I/O signals, but are not supported with the constants below, see [`MdigControl`](../../Reference/dig/MdigControl.md).  Each Zebra imaging board and Aurora Imaging Library driver has its own list of limitations regarding the signals you can control with this function. While general limitations are listed in the table below, for a complete list of the available signals and their limitations, see the _Connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board or Aurora Imaging Library driver.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_IO_DEBOUNCE_TIME`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Sets the amount of time that the specified auxiliary input signal is debounced.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | When a signal is debounced on Zebra 4Sight EV6/EV7, after detecting a valid input signal edge any other transitions are considered noise and are suppressed for the specified time, to ignore any additional transitions caused by the contact bounce of mechanical switches. |
| Iris GTX | When a signal is debounced on Zebra Iris GTX, after detecting a valid input signal edge any other transitions are considered noise and are suppressed for the specified time, to ignore any additional transitions caused by the contact bounce of mechanical switches. To remove problems occurring at the edge of a signal, use [`M_IO_GLITCH_FILTER_STATE`](../../Reference/sys/MsysControl.md). |

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the minimum amount of time to ignore any additional signal transitions after accepting a signal transition, in nsec. |

---

### `M_IO_GLITCH_FILTER_STATE`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Sets whether to enable a glitch filter on input signals. A glitch is an unexpected signal transition of a short duration. Enabling the glitch filter will eliminate glitches of less than 500 nsec on all input signals.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to use a glitch filter. |
| `M_ENABLE` | Specifies to use a glitch filter. |

---

### `M_IO_INTERRUPT_ACTIVATION`

Sets the signal transition upon which to generate an interrupt, if interrupt generation has been enabled for the specified I/O signal. Use [`M_IO_INTERRUPT_STATE`](../../Reference/sys/MsysControl.md) to enable interrupt generation.  Note that this only applies to input signals.  Note that this control type only has an effect when [`M_IO_INTERRUPT_STATE`](../../Reference/sys/MsysControl.md) is enabled.

#### System specific

| Board(s) | Note |
|---|---|
| Iris GTX | This control type is only available with auxiliary I/O signals 3-6. |
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies to generate an interrupt upon both a low-to-high and a high-to-low signal transition. **[Host System, Indio, Iris GTX]** |
| `M_EDGE_FALLING` | Specifies that an interrupt will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that an interrupt will be generated upon a low-to-high signal transition. |

---

### `M_IO_INTERRUPT_STATE`

Sets whether to generate an interrupt upon the specified transition of the I/O signal. Use [`M_IO_INTERRUPT_ACTIVATION`](../../Reference/sys/MsysControl.md) to specify the transition.

#### System specific

| Board(s) | Note |
|---|---|
| Iris GTX | This control type is only available with auxiliary I/O signals 3-6. |
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to generate an interrupt. |
| `M_ENABLE` | Specifies to generate an interrupt. |

---

### `M_IO_INVERTER`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Sets whether the specified I/O signal should be inverted. This causes the low portion of the signal (the delay period) to be high and the high portion of the signal (the active portion) to be low.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to invert the specified I/O signal. |
| `M_ENABLE` | Specifies to invert the specified I/O signal. |

---

### `M_IO_SOURCE`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Sets the type of signal to route to an output signal, or a bidirectional signal set to output mode.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EXPOSURE` | Specifies to route the exposure signal of the camera. **[Iris GTX]** |
| `M_GRAB_TRIGGER_READY` | Specifies to route the internal grab trigger ready signal. **[Iris GTX]** |
| `M_IO_COMMAND_LISTn` | Specifies to route a bit of the I/O command register of I/O command list _n_, where _n_ is the number of the I/O command list. **[Concord PoE, Host System, Indio, Iris GTX]** |
| `M_ROTARY_ENCODERn` | Specifies to route the output of rotary encoder _n_, where _n_ is the number of rotary encoders available. |
| `M_TIMER_STROBE` | Specifies to route the internal timer strobe signal. **[Iris GTX]** |
| `M_TIMERn` | Specifies to route the output of timer _n_, where _n_ is the number of timers available. |
| `M_USER_BITn` *(default)* | Specifies to route the state of bit n of the main static-user-output register, where _n_ is the bit number. |

### Combination Constants — For specifying the type and number of the I/O signal to affect

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to set the type and number of the I/O signal to affect.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to affect auxiliary signal n, where _n_ is the signal number.  For a list of the available auxiliary I/O signals, see the _Connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board. |

### For resetting CXP connection error counts

The following is used to reset CXP errors.

> **Board availability:** Rapixo CXP, Rapixo CoF

---

### `M_TL_ERROR_CORRECTED_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Resets the count of corrected duplicate character errors in the CXP control words.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to reset the count of corrected duplicate character errors in the CXP control words.  > **Note:** Note that if a non-zero value is specified, an error is generated. |

---

### `M_TL_ERROR_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Resets the count of all errors encountered.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to reset the count of all errors encountered.  > **Note:** Note that if a non-zero value is specified, an error is generated. |

---

### `M_TL_ERROR_CTRL_CRC_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Resets the count of CRC errors detected in a control packet.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to reset the count of CRC errors detected in a control packet.  > **Note:** Note that if a non-zero value is specified, an error is generated. |

---

### `M_TL_ERROR_DATA_CRC_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Resets the count of CRC errors detected in a data packet.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to reset the count of CRC errors detected in a data packet.  > **Note:** Note that if a non-zero value is specified, an error is generated. |

---

### `M_TL_ERROR_ENCODING_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Resets the count of protocol encoding errors detected.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to reset the count of protocol encoding errors detected.  > **Note:** Note that if a non-zero value is specified, an error is generated. |

---

### `M_TL_ERROR_EVENT_CRC_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Resets the count of CRC errors detected in an event packet.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to reset the count of CRC errors detected in an event packet.  > **Note:** Note that if a non-zero value is specified, an error is generated. |

---

### `M_TL_ERROR_LOCK_LOSS_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Resets the count of lock losses encountered.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to reset the count of lock losses encountered.  > **Note:** Note that if a non-zero value is specified, an error is generated. |

---

### `M_TL_ERROR_UNCORRECTED_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Resets the count of uncorrected duplicate character errors in the CXP control words.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to reset the count of uncorrected duplicate character errors in the CXP control words.  > **Note:** Note that if a non-zero value is specified, an error is generated. |

### Combination Constants — For specifying the CXP input connector

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the CXP input connector.

> **Board availability:** Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies all CXP input connectors. |
| `M_CONNECTIONn` | Specifies the connection made at CXP input connector _n_, where _n_ is the CXP input connector number from 0 to 3. |

### For setting the state of specified user-bits in a static-user-output register

The following control types and control values allow you to set the bits in a static-user-output register. You can route the bits to output signals or I/O signals set to output; to do so use [`M_IO_SOURCE`](../../Reference/sys/MsysControl.md) with [`M_USER_BIT...`](../../Reference/sys/MsysControl.md)). To establish which user-bits can be routed to a specific signal, see the _connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board.  > **Note:** Note that for other Zebra imaging boards that have user-bits, but are not supported with the constants below, see [`MdigControl`](../../Reference/dig/MdigControl.md).

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_USER_BIT_STATE`

Sets the state of the specified bit of a static-user-output register.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_OFF` | Specifies that the specified bit is set to off. |
| `M_ON` | Specifies that the specified bit is set to on. |

---

### `M_USER_BIT_STATE_ALL`

Sets the state of all the bits in the main static-user-output register.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies a bit-encoded value that establishes the value of all the bits of the specified static-user-output register. It is recommended to specify the value in hexadecimal notation (0x), so that it is more legible to what you are setting each bit of the register. |

### Combination Constants — For specifying the bit in a static-user-output register to affect

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the bit in a static-user-output register to affect.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_USER_BITn` | Specifies to affect bit n of the main static-user-output register. |

### For controlling the settings of a timer

The following control types and control values specify the settings for controlling timers and the signals generated from a timer (timer output signals). For more information, see [Timers and coordinating events](../../UserGuide/C56_IO_signals/S06_Timers_and_coordinating_events.md).

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_TIMER_ARM`

Sets whether to enable the timer arming mechanism. If timer arming is enabled, then the timer will ignore its trigger signal ([`M_TIMER_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md)) until a signal transition specified using [`M_TIMER_ARM_ACTIVATION`](../../Reference/sys/MsysControl.md) occurs on the signal specified using [`M_TIMER_ARM_SOURCE`](../../Reference/sys/MsysControl.md).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that timer arming is disabled. |
| `M_ENABLE` | Specifies that timer arming is enabled. |

---

### `M_TIMER_ARM_ACTIVATION`

Sets the signal transition upon which to arm the timer, if timer arming is enabled. Use [`M_TIMER_ARM`](../../Reference/sys/MsysControl.md) to enable timer arming. Use [`M_TIMER_ARM_SOURCE`](../../Reference/sys/MsysControl.md) to set which input signal will arm the timer.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that the timer will be armed by both a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that the timer will be armed upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the timer will be armed upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a timer is continuously armed during a high signal polarity. |
| `M_LEVEL_LOW` | Specifies that a timer is continuously armed during a low signal polarity. |

---

### `M_TIMER_ARM_SOFTWARE`

Issues a software trigger to arm the timer.  To use this setting, the timer's arm source must be set to software ([`M_TIMER_ARM_SOURCE`](../../Reference/sys/MsysControl.md) set to [`M_SOFTWARE`](../../Reference/sys/MsysControl.md)).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_ACTIVATE` | Specifies the default behavior. |

---

### `M_TIMER_ARM_SOURCE`

Sets which input signal will arm the timer, if timer arming is enabled. Use [`M_TIMER_ARM`](../../Reference/sys/MsysControl.md) to enable timer arming.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the source signal used to arm the specified timer, where _n_ is the number of the auxiliary signal. |
| `M_EXPOSURE` | Specifies to use the exposure signal as the trigger source. **[Iris GTX]** |
| `M_GRAB_TRIGGER_READY` | Specifies to route the internal grab trigger ready signal to the specified signal. **[Iris GTX]** |
| `M_SOFTWARE` | Specifies to use software to arm the specified timer. |
| `M_TIMER_STROBE` | Specifies to route the strobe's timer signal to the specified signal. **[Iris GTX]** |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the source signal to arm the timer, where _n_ is the timer number. |

---

### `M_TIMER_CLOCK_ACTIVATION`

Sets the edge of the signal that will increment the clock used to control the active portion of the timer's output signal.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that the clock will be incremented by both a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that the clock will be incremented by a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the clock will be incremented by a low-to-high signal transition. |

---

### `M_TIMER_CLOCK_FREQUENCY`

Sets the frequency of the clock source signal for the active portion of the timer's output signal ([`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md)).  Note that if [`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md) is set to [`M_AUX_IOn`](../../Reference/sys/MsysControl.md) or [`M_ROTARY_ENCODERn`](../../Reference/sys/MsysControl.md), and no clock source frequency is specified, the values that are normally specified or returned in units of time will be in number of ticks (clock signal transitions on the clock source).  > **Note:** If [`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md) is set to [`M_SYSCLK`](../../Reference/sys/MsysControl.md), changing the frequency to any value different than the system clock's frequency will cause an error.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_UNKNOWN` | Specifies that the signal is not periodic or the frequency is unknown. |
| `0 < Value <= Frequency of M_SYSCLK` | Specifies the frequency, in Hz. |

---

### `M_TIMER_CLOCK_SOURCE`

Sets the source of the clock that drives the active portion of the specified timer's output signal.  > **Note:** The clock source used for the delay can be different from the clock source used for the active portion of the timer's output signal. This is useful, for example, if you want to specify the delay in signal transitions and the active portion in nsec.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the clock source, where _n_ is the number of the auxiliary signal. If the signal is periodic, specify its frequency using [`M_TIMER_CLOCK_FREQUENCY`](../../Reference/sys/MsysControl.md). |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the clock source, where _n_ is the number of the rotary decoder. If the signal is periodic, specify its frequency using [`M_TIMER_CLOCK_FREQUENCY`](../../Reference/sys/MsysControl.md). |
| `M_SYSCLK` *(default)* | Specifies to use the allocated system's clock source. |

---

### `M_TIMER_DELAY`

Sets the delay between the timer trigger and the active portion of the timer's output signal.  Note, an error is generated if the specified delay cannot be respected.  > **Note:** The clock source used for the delay can be different from the clock source used for the active portion of the timer's output signal. This is useful, for example, if you want to specify the delay in signal transitions and the active portion in nsec.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `0 <= Value <= Max. value` | Specifies the delay. If [`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md) is set to [`M_SYSCLK`](../../Reference/sys/MsysControl.md) or a frequency is specified using [`M_TIMER_DELAY_CLOCK_FREQUENCY`](../../Reference/sys/MsysControl.md), this value is specified in nsec. Otherwise, it is specified in number of clock ticks (signal transitions on the clock source).  Use [`MsysInquire`](../../Reference/sys/MsysInquire.md) to determine the maximum possible value. |

---

### `M_TIMER_DELAY_CLOCK_ACTIVATION`

Sets the signal transition that will increment the clock used to control the delay between the timer's trigger and the active portion of the timer's output signal.  > **Note:** If [`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md) is set to [`M_FOLLOW_TIMER_CLOCK`](../../Reference/sys/MsysControl.md), changing this setting to a value different than the value of [`M_TIMER_CLOCK_ACTIVATION`](../../Reference/sys/MsysControl.md) will cause an error.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that the clock will be incremented by both a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that the clock will be incremented by a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the clock will be incremented by a low-to-high signal transition. |

---

### `M_TIMER_DELAY_CLOCK_FREQUENCY`

Sets the frequency of the clock source signal for the delay between the timer's trigger and the active portion of the timer's output signal ([`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md)).  Note that if [`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md) is set to [`M_AUX_IOn`](../../Reference/sys/MsysControl.md) or [`M_ROTARY_ENCODERn`](../../Reference/sys/MsysControl.md), and no clock source frequency is specified, the values that are normally specified or returned in units of time will be in number of ticks.  > **Note:** If [`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md) is set to [`M_SYSCLK`](../../Reference/sys/MsysControl.md), changing the frequency to any value different than system clock's frequency will cause an error. If [`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md) is set to [`M_FOLLOW_TIMER_CLOCK`](../../Reference/sys/MsysControl.md), changing the frequency to a value different than the value of [`M_TIMER_CLOCK_FREQUENCY`](../../Reference/sys/MsysControl.md) will cause an error.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_UNKNOWN` | Specifies that the signal is not periodic or the frequency is unknown. |
| `0 < Value <= Frequency of M_SYSCLK` | Specifies the frequency, in Hz. |

---

### `M_TIMER_DELAY_CLOCK_SOURCE`

Sets the source of the clock that drives the delay between the timer's trigger and the active portion of the specified timer's output signal.  > **Note:** The clock source used for the delay can be different from the clock source used for the active portion of the timer's output signal. This is useful, for example, if you want to specify the delay in signal transitions and the active portion in nsec.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the clock source, where _n_ is the number of the auxiliary signal. |
| `M_FOLLOW_TIMER_CLOCK` *(default)* | Specifies to use the clock source specified by [`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md). If the clock source is set to this value and [`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md) is changed, this clock source changes as well. Additionally, any changes to [`M_TIMER_CLOCK_ACTIVATION`](../../Reference/sys/MsysControl.md) and [`M_TIMER_CLOCK_FREQUENCY`](../../Reference/sys/MsysControl.md) will also be reflected in [`M_TIMER_DELAY_CLOCK_ACTIVATION`](../../Reference/sys/MsysControl.md) and [`M_TIMER_DELAY_CLOCK_FREQUENCY`](../../Reference/sys/MsysControl.md). |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the clock source, where _n_ is the number of the rotary decoder. |
| `M_SYSCLK` | Specifies to use the allocated system's clock source. |

---

### `M_TIMER_DURATION`

Sets the duration for the active portion of the timer's output signal.  Note, an error is generated if the specified duration cannot be respected.  > **Note:** The clock source used for the delay can be different from the clock source used for the active portion of the timer's output signal. This is useful, for example, if you want to specify the delay in signal transitions and the active portion in nsec.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `0 <= Value <= Max. value` | Specifies the duration of the active portion of the timer output signal. If [`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md) is set to [`M_SYSCLK`](../../Reference/sys/MsysControl.md) or a frequency is specified using [`M_TIMER_CLOCK_FREQUENCY`](../../Reference/sys/MsysControl.md), this value is specified in nsec. Otherwise, it is specified in number of clock ticks (signal transitions on the clock source).  Use [`MsysInquire`](../../Reference/sys/MsysInquire.md) to determine the maximum possible value. |

---

### `M_TIMER_OUTPUT_INVERTER`

Sets whether the output of the timer should be inverted. This causes the low portion of the signal (the delay period) to be high and the high portion of the signal (the active portion) to be low.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to invert the output of the timer. |
| `M_ENABLE` | Specifies to invert the output of the timer. |

---

### `M_TIMER_STATE`

Sets the state of the specified timer.  When a timer is enabled, the timer waits for a trigger to be received. To set the source of the trigger, use [`M_TIMER_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md). Once the trigger is received, the timer starts by outputting a low signal. This lasts for the duration of the delay period (set using [`M_TIMER_DELAY`](../../Reference/sys/MsysControl.md)). The timer then changes to output a high signal for the duration of the active period (set using [`M_TIMER_DURATION`](../../Reference/sys/MsysControl.md)). To invert this signal, use [`M_TIMER_OUTPUT_INVERTER`](../../Reference/sys/MsysControl.md).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the timer is disabled. |
| `M_ENABLE` | Specifies that the timer is enabled. |

---

### `M_TIMER_TRIGGER_ACTIVATION`

Sets the signal variation upon which to generate a timer trigger. The timer will be triggered when the specified signal transition occurs on the source signal specified by [`M_TIMER_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that a timer trigger will be generated both upon a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that a timer trigger will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that a timer trigger will be generated upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a timer trigger is continuously issued during a high signal polarity. |
| `M_LEVEL_LOW` | Specifies that a timer trigger is continuously issued during a low signal polarity. |

---

### `M_TIMER_TRIGGER_OVERLAP`

Sets how to deal with a new trigger that occurs while the associated timer has not yet expired (both its delay and duration).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LATCH` | Specifies that a trigger received, while the associated timer has not expired, will be latched (stored). As soon as the current timer expires, a new trigger is issued by software. |
| `M_OFF` *(default)* | Specifies that a new trigger is ignored. |
| `M_RESET` | Specifies that a new trigger automatically resets the timer (regardless of whether it is in its delay or active period) and then restarts the timer. This process will repeat for each new trigger received. |

---

### `M_TIMER_TRIGGER_SOFTWARE`

Issues a software trigger for the specified timer.  To use this setting, the timer's trigger source must be set to software ([`M_TIMER_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md) set to [`M_SOFTWARE`](../../Reference/sys/MsysControl.md)).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_ACTIVATE` | Specifies the default behavior. |

---

### `M_TIMER_TRIGGER_SOURCE`

Sets the trigger source for the specified timer. The timer will be triggered when the signal transition specified using [`M_TIMER_TRIGGER_ACTIVATION`](../../Reference/sys/MsysControl.md) occurs on the selected source.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source for the specified timer, where _n_ is the number of the auxiliary signal. |
| `M_CONTINUOUS` | Specifies to run the specified timer in periodic mode; no actual trigger signal is used. The timer is automatically reset after the timer's duration expires. The timer loops between a delay and an active period. |
| `M_EXPOSURE` | Specifies to use the exposure signal as the trigger source. **[Iris GTX]** |
| `M_GRAB_TRIGGER_READY` | Specifies to route the internal grab trigger ready signal to the specified signal. **[Iris GTX]** |
| `M_IO_COMMAND_LISTn` | Specifies to use a bit of the I/O command register of I/O command list _n_, where _n_ is the number of the I/O command list. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the trigger source, where _n_ is the number of the rotary decoder. |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. Use [`M_TIMER_TRIGGER_SOFTWARE`](../../Reference/sys/MsysControl.md) to issue the trigger. |
| `M_TIMER_STROBE` | Specifies to route the strobe's timer signal to the specified signal. **[Iris GTX]** |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the timer number. |

---

### `M_TIMER_USAGE`

Sets the purpose of the timer.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PULSE_GENERATION` *(default)* | Specifies the normal use of the timer, as described in [Timers and coordinating events](../../UserGuide/C56_IO_signals/S06_Timers_and_coordinating_events.md). |
| `M_PULSE_MEASUREMENT` | Specifies to use the timer to measure the duration of the pulse that occurs on the timer's trigger source. Note that when in this mode, the values set using [`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md), [`M_TIMER_DELAY`](../../Reference/sys/MsysControl.md), and [`M_TIMER_DURATION`](../../Reference/sys/MsysControl.md) are ignored.  When a timer is set in this mode, the duration of pulses that occur on the timer's trigger source ([`M_TIMER_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md)) are measured with respect to the timer's clock source ([`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md)). To measure active-high pulses, set [`M_TIMER_TRIGGER_ACTIVATION`](../../Reference/sys/MsysControl.md) to [`M_LEVEL_HIGH`](../../Reference/sys/MsysControl.md); to measure active-low pulses, set [`M_TIMER_TRIGGER_ACTIVATION`](../../Reference/sys/MsysControl.md) to [`M_LEVEL_LOW`](../../Reference/sys/MsysControl.md). The duration of the pulse can be inquired using [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_TIMER_VALUE`](../../Reference/sys/MsysInquire.md).  Between pulses, you can reset the timer value using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_TIMER_VALUE`](../../Reference/sys/MsysControl.md). |

---

### `M_TIMER_VALUE`

Resets the timer value.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `0` | Specifies to reset the timer value to 0. |

### Combination Constants — For specifying which on-board timer to control

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which on-board timer to control.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_TIMERn` | Specifies on-board timer _n_, where _n_ is the number of the timer.  > **Note:** To set the exposure and strobe timers, use [`MdigControl`](../../Reference/dig/MdigControl.md)with [`M_EXPOSURE_TIME`](../../Reference/dig/MdigControl.md)or [`M_TIMER_STROBE`](../../Reference/dig/MdigControl.md), respectively. |

### For controlling the settings of a rotary decoder

The following control types allow you to control the settings of a quadrature decoder with inputs from a rotary or linear encoder.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_ROTARY_ENCODER_BIT0_SOURCE`

Sets the auxiliary input signal on which to receive bit 0 of the 2-bit Gray code.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the signal on which to receive bit 0 of the 2-bit Gray code, where _n_ is the number of the auxiliary signal. |

---

### `M_ROTARY_ENCODER_BIT1_SOURCE`

Sets the auxiliary input signal on which to receive bit 1 of the 2-bit Gray code.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE, Host System, Indio | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the signal on which to receive bit 1 of the 2-bit Gray code, where _n_ is the number of the auxiliary signal. |

---

### `M_ROTARY_ENCODER_OUTPUT_MODE`

Sets the rotary decoder's counter value and/or the direction of movement upon which the rotary decoder should output a pulse. The pulse can be used to trigger a timer. To trigger a timer, set [`M_TIMER_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md) to [`M_ROTARY_ENCODERn`](../../Reference/sys/MsysControl.md).  To decimate (subsample) the rotary decoder output signal before sending it to a timer or a grab controller, set this control type to [`M_POSITION_TRIGGER`](../../Reference/sys/MsysControl.md) and set [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/sys/MsysControl.md) to the required decimation value. For more information, refer to [Pixel aspect ratio](../../UserGuide/C56_IO_signals/S07_Using_quadrature_input_from_a_rotary_encoder.md).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_POSITION_TRIGGER` | Specifies to output a pulse upon the trigger generated by [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/sys/MsysControl.md). |
| `M_STEP_ANY` | Specifies to output a pulse upon any change in the rotary decoder's counter value (position change in any direction). |
| `M_STEP_BACKWARD` | Specifies to output a pulse upon a rotary decoder counter decrement only. |
| `M_STEP_FORWARD` | Specifies to output a pulse upon a rotary decoder counter increment only. |
| `M_STEP_FORWARD_NEW_POSITIVE` | Specifies to output a pulse upon a rotary decoder counter increment of a new value that has not been reached before. For example, if the counter value is at 10, and is decremented down to 7, the rotary decoder will only output a pulse when the counter is incremented up to 11. |

---

### `M_ROTARY_ENCODER_POSITION`

Resets the rotary decoder's counter to 0 immediately.  To reset the counter to 0 upon a signal, use [`M_ROTARY_ENCODER_RESET_SOURCE`](../../Reference/sys/MsysControl.md).  Note that a call to [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_ROTARY_ENCODER_POSITION`](../../Reference/sys/MsysInquire.md) inquires the current value of the rotary decoder counter.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value = 0` | Implements the default behavior. Note that, if a non-zero value is specified, an error is generated. |

---

### `M_ROTARY_ENCODER_POSITION_TRIGGER`

Sets the value of the rotary decoder's counter upon which a trigger is generated.  You can output this trigger to a timer or a grab controller using [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/sys/MsysControl.md) set to [`M_POSITION_TRIGGER`](../../Reference/sys/MsysControl.md).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `0 <= Value <= 0xFFFFFFFF` | Specifies the value of the counter upon which a trigger is generated. If a value beyond the supported range is specified, an error is generated.  If you are treating the counter values as a signed range of values (for example, forcing the counter to reset to 0 at 0x80000000) and you want to generate a trigger upon a negative value, specify the equivalent value in the range of 0x80000000 to 0xFFFFFFFF. |

---

### `M_ROTARY_ENCODER_RESET_ACTIVATION`

Sets the signal transition upon which to reset the rotary decoder's counter to 0. The rotary decoder will reset the counter to 0 when the signal transition occurs on the source signal specified by [`M_ROTARY_ENCODER_RESET_SOURCE`](../../Reference/sys/MsysControl.md).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE_FALLING` | Specifies to reset the rotary decoder upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies to reset the rotary decoder upon a low-to-high signal transition. |

---

### `M_ROTARY_ENCODER_RESET_SOURCE`

Sets the source signal to use to reset the rotary decoder's counter to 0. The rotary decoder will reset the counter to 0 when the signal transition specified using [`M_ROTARY_ENCODER_RESET_ACTIVATION`](../../Reference/sys/MsysControl.md) occurs on the selected source.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies not to reset using a hardware signal source. |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source for the specified timer, where _n_ is the number of the auxiliary signal. |
| `M_POSITION_TRIGGER` | Specifies to use the trigger signal generated by the rotary decoder when the counter reaches the value specified with [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/sys/MsysControl.md). |

---

### `M_ROTARY_ENCODER_STATE`

Sets whether to enable the rotary decoder.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the rotary decoder is disabled. |
| `M_ENABLE` | Specifies that the rotary decoder is enabled. |

### Combination Constants — For specifying which rotary decoder to set

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which rotary decoder to set.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_ROTARY_ENCODERn` | Specifies rotary decoder _n_, where _n_ is the number of the rotary decoder. |

### For controlling the settings of a data latch associated with rotary encoders

The following control types allow you to control the settings of a data latch associated with the rotary decoders of your system.

> **Board availability:** Host System

---

### `M_SYS_DATA_LATCH_STATE`

Sets the state of the specified data latch.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the data latch is disabled. |
| `M_ENABLE` | Specifies that the data latch is enabled.  The data latch will store the position couner value of the rotary encoder when its associated event (specified using [`M_SYS_DATA_LATCH_TRIGGER_ACTIVATION`](../../Reference/sys/MsysControl.md)) occurs. |

---

### `M_SYS_DATA_LATCH_TRIGGER_ACTIVATION`

Sets the trigger signal transition upon which to store the specified information to the specified data latch.  To set the signal with which to trigger the data latch, use [`M_SYS_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that the specified information is stored in the data latch both upon a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that the specified information is stored in the data latch upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the specified information is stored in the data latch upon a low-to-high signal transition. |

---

### `M_SYS_DATA_LATCH_TRIGGER_SOURCE`

Sets what triggers storing the specified information to the specified data latch.

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal.  > **Note:** In this case, _n_ can be a value from 8 to 15.  To specify the signal transition, use [`M_SYS_DATA_LATCH_TRIGGER_ACTIVATION`](../../Reference/sys/MsysControl.md). |
| `M_TIMERn` | Specifies to use the output signal of timer _n_ as the trigger source, where _n_ is the number of the timer.  > **Note:** In this case, _n_ can be a value from 1 to 16. |

---

### `M_SYS_DATA_LATCH_TYPE`

Sets which rotary decoder the specified data latch will store the position counter value of when the data latch is triggered.  > **Note:** You can access the stored rotary decoder position within a hook function that was triggered by an auxiliary input or a timer, using [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md)with[`M_SYS_DATA_LATCH_VALUE`](../../Reference/sys/MsysGetHookInfo.md).

| Value | Description |
| --- | --- |
| `M_ROTARY_ENCODERn` | Specifies to store the value of the counter of rotary decoder _n_, where _n_ is a number between 1 and 2. To configure the rotary decoder, use the [`M_ROTARY_ENCODER...`](../../Reference/sys/MsysControl.md) control types. |

### Combination Constants — For specifying which data latch to set

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which data latch to affect.

> **Board availability:** Host System

| Value | Description |
| --- | --- |
| `M_LATCHn` | Specifies which data latch to affect, where_n_ is 1 or 2. |

### For UART settings

The following control types and control values specify the settings for UARTs. To specify a particular UART, see the [`Uart`](../../Reference/Uart.md) combination value below.  > **Note:** Note that for other Zebra imaging boards that have a UART, but are not supported with the constants below, see [`MdigControl`](../../Reference/dig/MdigControl.md).

> **Board availability:** Radient eV-CL, Rapixo CL

---

### `M_UART_DATA_SIZE`

Sets the number of data bits per character that are sent or received by the UART.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `7` | Specifies that the data length is 7 bits. |
| `8` *(default)* | Specifies that the data length is 8 bits. |

---

### `M_UART_FREE`

Stops all Aurora Imaging Library UART operations and frees all UART resources used by Aurora Imaging Library.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_UART_PARITY`

Sets whether character data is sent or received with a parity bit and how the parity bit is set. The parity bit is an extra data bit (0 or 1) that is added to each character for error checking purposes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that no extra bit is added (no parity). |
| `M_EVEN` | Specifies that the number of 1's will be even. |
| `M_ODD` | Specifies that the number of 1's will be odd. |

---

### `M_UART_READ_CHAR`

Reads one character from the UART input buffer. If the input buffer is empty, the function will wait for the amount of time specified by [`M_UART_TIMEOUT`](../../Reference/sys/MsysControl.md). If a time-out occurs, the '?' character will be returned.

| Value | Description |
| --- | --- |
| `Value` | Specifies the address of the variable in which to save the character read from the UART. |

---

### `M_UART_READ_STRING`

Reads a string of incoming data from the UART. The number of characters to read can be specified with [`M_UART_READ_STRING_SIZE`](../../Reference/sys/MsysControl.md) or [`M_UART_STRING_DELIMITER`](../../Reference/sys/MsysControl.md). [`M_UART_TIMEOUT`](../../Reference/sys/MsysControl.md) specifies the maximum time to wait between each byte when reading incoming data. Use [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_UART_BYTES_READ`](../../Reference/sys/MsysInquire.md) to wait for the read operation to complete and retrieve the actual number of bytes read.

| Value | Description |
| --- | --- |
| `Value` | Specifies the address of the character array in which to save the string read from the UART. The size of this array must be set to the same value as the [`M_UART_READ_STRING_MAXIMUM_SIZE`](../../Reference/sys/MsysControl.md) control type. |

---

### `M_UART_READ_STRING_MAXIMUM_SIZE`

Sets the maximum length of the string to read. This prevents global protection faults from happening when using the [`M_UART_READ_STRING`](../../Reference/sys/MsysControl.md) control type.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum length of the string, in bytes. |

---

### `M_UART_READ_STRING_SIZE`

Sets the length of the string to read using [`M_UART_READ_STRING`](../../Reference/sys/MsysControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the use of [`M_UART_STRING_DELIMITER`](../../Reference/sys/MsysControl.md) to delineate the end of the string to read. |
| `Value` | Specifies the string length, in bytes. |

---

### `M_UART_SPEED`

Sets the baud rate of the UART.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `300;600;1200;1800;2400;4800;7200;9600;14400;19200;38400;57600;115200` *(default)* | Specifies the baud rate of the UART. Note that the maximum baud rate is highly dependent on the amount of computer resources available. |

---

### `M_UART_STOP_BITS`

Sets the number of extra data bit(s) (1 or 2) that are added to each character to indicate the end of the character.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1` *(default)* | Specifies that there is 1 stop bit. |
| `2` | Specifies that there are 2 stop bits. |

---

### `M_UART_STRING_DELIMITER`

Sets the character used to terminate strings of incoming or outgoing data. The delimiter is used but not sent when writing data with [`M_UART_WRITE_STRING`](../../Reference/sys/MsysControl.md); it is read for incoming data with [`M_UART_READ_STRING`](../../Reference/sys/MsysControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the character used to terminate strings. |

---

### `M_UART_TIMEOUT`

Sets the maximum time to wait between each byte when reading incoming data.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Waits indefinitely. |
| `Value` | Specifies the time to wait, in msec. |

---

### `M_UART_WRITE_CHAR`

Sends one character to the UART for transmission.

| Value | Description |
| --- | --- |
| `Value` | Specifies the character to send. |

---

### `M_UART_WRITE_STRING`

Sends a string of data through the UART for transmission. The number of characters to send can be specified with [`M_UART_WRITE_STRING_SIZE`](../../Reference/sys/MsysControl.md) or [`M_UART_STRING_DELIMITER`](../../Reference/sys/MsysControl.md). Use [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_UART_BYTES_WRITTEN`](../../Reference/sys/MsysInquire.md) to wait for the write operation to complete and retrieve the actual number of bytes written.

| Value | Description |
| --- | --- |
| `Value` | Specifies the character array. |

---

### `M_UART_WRITE_STRING_SIZE`

Sets the length of the string to be sent to the UART for transmission.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the use of [`M_UART_STRING_DELIMITER`](../../Reference/sys/MsysControl.md) to end the string. The delimiter will not be sent through the UART. |
| `Value` | Specifies the string length, in bytes. |

### Combination Constants — For specifying which UART to control

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set which UART to control.

> **Board availability:** Radient eV-CL, Rapixo CL

| Value | Description |
| --- | --- |
| `M_UART_NB` | Specifies which UART to control.  Use [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_UART_PRESENT`](../../Reference/sys/MsysInquire.md) to determine the number of UARTS on the system. |

### For configuring and sending an action command to a GigE Vision camera

The following control types and control values allow you to configure an action command and send it to one or more GigE Vision cameras. Action commands require both an Aurora Imaging Library-side and a camera-side configuration. The following control types and control values configure the Aurora Imaging Library-side of the action command. To configure the camera-side, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the appropriate feature values. For more information, refer to Triggering simultaneous actions in multiple GigE Vision cameras.  > **Note:** To configure features of the camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md)with a digitizer allocated for the camera on an Aurora Imaging Library GigE system.

> **Board availability:** Concord PoE, GevIQ, GigE Vision

---

### `M_GC_ACTION_ACKNOWLEDGE_NUMBER`

**Board availability:** GevIQ, GigE Vision

Sets the number of acknowledgments that Aurora Imaging Library should expect to receive when the action command is issued. There should be one per target camera performing the action. By default, this number is equal to the number of Aurora Imaging Library digitizers added to the action command. If the number of Aurora Imaging Library digitizers added to the action command does not equal the number of Aurora Imaging Library digitizers that will perform the action, Aurora Imaging Library will return an error (for example, if you set [`M_GC_ACTION_GROUP_MASK`](../../Reference/sys/MsysControl.md) to mask out one or more cameras). To avoid this error, use this control type to set the number of acknowledgments that Aurora Imaging Library should expect.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the number of cameras added to the action command, using [`M_GC_ACTION_ADD_DEVICE`](../../Reference/sys/MsysControl.md). |
| `Value` | Specifies the number of acknowledgments expected. |

---

### `M_GC_ACTION_ADD_DEVICE`

**Board availability:** GevIQ, GigE Vision

Adds a GigE Vision camera to the list of cameras associated with the action command.

| Value | Description |
| --- | --- |
| `Digitizer identifier` | Specifies the digitizer identifier allocated for the GigE Vision camera to add. Note that the digitizer should be allocated on an Aurora Imaging Library GigE Vision system using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). |

---

### `M_GC_ACTION_CLEAR_DEVICES`

**Board availability:** GevIQ, GigE Vision

Removes all cameras from the list of cameras associated with the action command.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_GC_ACTION_DEVICE_KEY`

Sets the action device key for the action command. The action device key identifies the cameras on which the action should be performed. For the camera to accept and perform the action, the device key on your camera and the device key of the action command must match. If they do not match, the camera ignores the action command. To configure the device key on your camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the appropriate GenICam SFNC feature (for example, `ActionDeviceKey`).

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** To configure features of the camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md)with a digitizer allocated for the camera on an Aurora Imaging Library GigE system.  > **Note:** If using an Aurora Imaging Library Concord PoE system, this control type is only available for Zebra Concord PoE with ToE. To configure an action command for the Zebra Concord PoE base model, use an Aurora Imaging Library GigE system with this control type. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the action device key. |

---

### `M_GC_ACTION_EXECUTE`

**Board availability:** GevIQ, GigE Vision

Issues the action command.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_GC_ACTION_GROUP_KEY`

Sets the action group key for the action command. The action group key identifies the action you want to perform on the camera. For the camera to know which action signal to generate, the group key of the action command must match the group key on your camera. If they do not match, the camera ignores the action command. To configure the group key of the action command on your camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the appropriate GenICam SFNC feature (for example, `ActionGroupKey`).

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** To configure features of the camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md)with a digitizer allocated for the camera on an Aurora Imaging Library GigE system.  > **Note:** If using an Aurora Imaging Library Concord PoE system, this control type is only available for Zebra Concord PoE with ToE. To configure an action command for the Zebra Concord PoE base model, use an Aurora Imaging Library GigE system with this control type. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the action group key. |

---

### `M_GC_ACTION_GROUP_MASK`

Sets the action group mask for the action command. In the case where you need one (or more) cameras to temporarily ignore an action command, you can mask out the action command for the camera by changing the action group mask.  For the camera to know if it generates an action signal upon receiving the command, the action group mask of the action command, and the action group mask of the camera, when combined in a bitwise AND operation, must result in a non-zero value. If the result is a zero, the camera ignores the action command. Note that, if you are masking out the action command for the camera, you must change the number of action acknowledgment packets Aurora Imaging Library should expect; otherwise, Aurora Imaging Library will generate an error. To set the number of action acknowledgment packets, use [`M_GC_ACTION_ACKNOWLEDGE_NUMBER`](../../Reference/sys/MsysControl.md).

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** To configure features of the camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md)with a digitizer allocated for the camera on an Aurora Imaging Library GigE system.  > **Note:** If using an Aurora Imaging Library Concord PoE system, this control type is only available for Zebra Concord PoE with ToE. To configure an action command for the Zebra Concord PoE base model, use an Aurora Imaging Library GigE system with this control type. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the action group mask. |

---

### `M_GC_ACTION_REMOVE_DEVICE`

**Board availability:** GevIQ, GigE Vision

Removes an Aurora Imaging Library GigE Vision camera from the list of cameras associated with the action command.

| Value | Description |
| --- | --- |
| `Digitizer identifier` | Specifies the digitizer identifier for the GigE Vision camera to remove.  > **Note:** Note that the digitizer should be allocated on an Aurora Imaging Library GigE Vision system using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). |

---

### `M_GC_ACTION_TIME`

**Board availability:** GevIQ, GigE Vision

Sets the time at which the action command should execute.  Note that, for scheduled action commands to function, your camera must be capable of using IEEE 1588, and the appropriate feature must be enabled (for example, `GevIEEE1588`). To verify the availability of these features on your camera, use[`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_CONTROL_PROTOCOL_CAPABILITY`](../../Reference/dig/MdigInquire.md). It should return [`M_GC_SCHEDULED_ACTION_SUPPORT`](../../Reference/dig/MdigInquire.md) or [`M_GC_IEEE_1588_SUPPORT`](../../Reference/dig/MdigInquire.md). Note that, you must enable 1588 on each GigE Vision camera that will receive this action command, using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md), or Feature Browser (accessible through [`MsysControl`](../../Reference/sys/MsysControl.md)with [`M_GC_FEATURE_BROWSER`](../../Reference/sys/MsysControl.md)) and the appropriate SFNC feature (for example,`GevIEEE1588`).

| Value | Description |
| --- | --- |
| `Value` | Specifies the time at which to execute the action command on your camera, relative to the time at which the action command was sent, in sec. |

### For specifying a Trigger-over-Ethernet packet (action command or GigE Vision software trigger) for transmission using a ToE module

The following control types and control values allow you to configure a Trigger-over-Ethernet packet for transmission using a ToE module. The Trigger-over-Ethernet packet can be sent as an action command or a GigE Vision software trigger. Action commands and GigE Vision software triggers require both an Aurora Imaging Library-side and a camera-side configuration. The following control types and control values configure the Aurora Imaging Library-side of the action command or GigE Vision software trigger. To configure the camera-side, you need to use a digitizer allocated for the camera on an Aurora Imaging Library GigE system ([`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md)) and use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the appropriate feature values. For more information, refer to Triggering simultaneous actions in multiple GigE Vision cameras.

> **Board availability:** Concord PoE

---

### `M_ADD_DESTINATION`

Adds a GigE Vision camera to the list of cameras associated with the specified action command or GigE Vision software trigger.

| Value | Description |
| --- | --- |
| `Digitizer identifier` | Specifies the digitizer identifier allocated for the GigE Vision camera to add. Note that the digitizer should be allocated on a GigE Vision system using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). |

---

### `M_CLEAR_DESTINATIONS`

Removes all cameras from the list of cameras associated with the specified action command or GigE Vision software trigger.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_GC_TRIGGER_SELECTOR`

Sets the type of GigE Vision software trigger that should take place on the camera upon receiving the ToE packet (for example, FrameStart). For the camera to know what should be triggered upon receiving the packet, this control type must match the trigger selector on your camera. To configure the trigger selector on your camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the appropriate GenICam SFNC feature (for example, TriggerSelector).

| Value | Description |
| --- | --- |
| `"FeatureName"` | Specifies the type of trigger; see your GigE Vision camera's documentation for a list of available types. |

---

### `M_REMOVE_DESTINATION`

Removes a GigE Vision camera from the list of cameras associated with the specifies action command or GigE Vision software trigger.

| Value | Description |
| --- | --- |
| `Digitizer identifier` | Specifies the digitizer identifier allocated for the GigE Vision camera to remove. Note that the digitizer should be allocated on a GigE Vision system using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). |

---

### `M_TRIGGER_COMMAND`

Issues a software trigger event to prompt the transmission of the specified action command or GigE Vision software trigger from the ToE module. To use this control type, [`M_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md) must be set to [`M_SOFTWAREn`](../../Reference/sys/MsysControl.md).  > **Note:** Note that this will trigger any action command or GigE Vision software trigger that uses the same software signal ([`M_SOFTWAREn`](../../Reference/sys/MsysControl.md)) as the specified action command/GigE Vision software trigger. For example, if you specify [`M_TRIGGER_COMMAND`](../../Reference/sys/MsysControl.md) + [`M_GC_ACTION1`](../../Reference/sys/MsysControl.md) and the trigger source of [`M_GC_ACTION1`](../../Reference/sys/MsysControl.md) is [`M_SOFTWARE1`](../../Reference/sys/MsysControl.md), all ToE packets that use [`M_SOFTWARE1`](../../Reference/sys/MsysControl.md) are triggered.

| Value | Description |
| --- | --- |
| `M_ACTIVATE` | Specifies the default behavior. |

---

### `M_TRIGGER_SOURCE`

Sets the event that will cause the ToE module to send the specified action command or GigE Vision software trigger as a ToE packet.

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal.  > **Note:** In this case, _n_ can be a value from 0 to 5. |
| `M_IO_COMMAND_LISTn` | Specifies to use the I/O command list _n_, where _n_ is the number of the I/O command list.  > **Note:** In this case, _n_ can be a value from 0 to 1. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder n as the trigger source, where _n_ is the number of the rotary decoder.  > **Note:** In this case, _n_ can be a value from 1 to 2. |
| `M_SOFTWAREn` | Specifies to use software as a trigger source to trigger the ToE module, where _n_ is the number of the software trigger; _n_ can be a value between 1 and 4. Use [`M_TRIGGER_COMMAND`](../../Reference/sys/MsysControl.md) to issue the software trigger event.  > **Note:** In this case, _n_ can be a value from 1 to 4. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the number of the timer.  > **Note:** In this case, _n_ can be a value from 1 to 16. |

---

### `M_TRIGGER_STATE`

Sets the state of the specified action command or GigE Vision software trigger in the ToE module.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies the ToE packet is disabled. |
| `M_ENABLE` | Specifies the ToE packet is enabled, and will be transmitted when its associated event (specified using [`M_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md)) occurs. |

### Combination Constants — For specifying which action command to control

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set which action command to control.

> **Board availability:** Concord PoE, GevIQ, GigE Vision

| Value | Description |
| --- | --- |
| `M_GC_ACTIONn` | Specifies to control action command _n_, where _n_ is a value from 0 to 31. |

### Combination Constants — For specifying which GigE Vision software trigger to control

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set which GigE Vision software trigger to control.

> **Board availability:** Concord PoE

| Value | Description |
| --- | --- |
| `M_GC_TRIGGER_SOFTWAREn` | Specifies to control a GigE Vision software trigger n, where n is a value from 0 to 31. This combination value cannot be used with[`M_GC_ACTION_DEVICE_KEY`](../../Reference/sys/MsysControl.md), [`M_GC_ACTION_GROUP_KEY`](../../Reference/sys/MsysControl.md), or [`M_GC_ACTION_GROUP_MASK`](../../Reference/sys/MsysControl.md). |

### Combination Constants — For specifying which I/O command register bit to use

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which I/O command register bit to use.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_IO_COMMAND_BITn` | Specifies I/O command register bit _n_, where _n_ represents the bit number. |

This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7.

For Zebra 4Sight EV6/EV7, _n_ can be a value from 8 to 15.

For Zebra 4Sight EV6/EV7, _n_ can be either 1 or 2.

For Zebra 4Sight EV6/EV7, _n_ is a number from 1 to 4.

For Zebra 4Sight EV6/EV7, _n_ can be a value from 1 to 16.

For Zebra 4Sight EV6/EV7, _n_ can be a value from 0 to 7.
