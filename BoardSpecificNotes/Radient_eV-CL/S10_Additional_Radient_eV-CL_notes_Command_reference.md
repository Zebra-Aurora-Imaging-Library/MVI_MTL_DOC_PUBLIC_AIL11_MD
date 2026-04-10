---
doctype: BoardSpecificNotes
part: ""
chapter: Radient_eV-CL
section: Additional_Radient_eV-CL_notes_Command_reference
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / radient_ev-cl / Additional Radient eV-CL notes Command reference"
---

# Miscellaneous reference information for Zebra Radient eV-CL

This section includes additional command reference information for Zebra Radient eV-CL. The information found in this section might be a reiteration of content previously documented.

The following modules are mentioned in this section:

- [Mdig](S10_Additional_Radient_eV-CL_notes_Command_reference.md).

## Mdig

This section presents information about Zebra Radient eV-CL and the digitizer module.

### MdigControl/MdigInquire

The following lists additional Radient eV-CL support when using [`MdigControl`](../../Reference/dig/MdigControl.md)/[`MdigInquire`](../../Reference/dig/MdigInquire.md).

- Support for **M_IO_DEBOUNCE_TIME** + **(M_AUX_IOn)**.
  Sets the amount of time that the specified auxiliary input signal is debounced. A maximum of 4 inputs can have a debounce set. You must specify a combination value to set the I/O signal to affect. Possible control/return values are:
  - **0 &lt;= Value &lt;= 8300000**.
    Specifies the minimum amount of time to ignore any additional signal transitions after accepting a signal transition, in nsec.
- Support for **M_SHADING_CORRECTION**.
  Sets whether to perform a gain and offset correction per pixel. When grabbing to a buffer in Host memory, your digitizer performs the shading correction operation using following equation: (`Grab Image` x** M_SHADING_CORRECTION_GAIN_ID**) + **M_SHADING_CORRECTION_OFFSET_ID**. Each row of gain and offset values is applied to the equivalent row of the grabbed image.
  To set the gain and provide an offset for shading correction, use **M_SHADING_CORRECTION_OFFSET_ID** and **M_SHADING_CORRECTION_GAIN_ID**, respectively. Possible control/return values are:
  - **M_ENABLE**. Enables shading correction.
  - **M_DISABLE**. Disables shading correction.
- Support for **M_SHADING_CORRECTION_GAIN_ID**.
  Sets the buffer containing the gain values that your digitizer should use when it performs a shading correction.
  To enable on-board shading correction and provide an offset, use **M_SHADING_CORRECTION** and **M_SHADING_CORRECTION_OFFSET_ID**, respectively. Possible control/return values are:
  - **M_NULL**. Specifies to not apply a gain when performing a shading correction.
  - **ShadingCorrectionGainID**. Specifies the identifier of an **M_IMAGE** buffer containing the gain values. The buffer must be allocated as a single-band unsigned 8-bit or unsigned 16-bit buffer. It is interpreted as a fixed point buffer with 2-bits for the numerical part. Use **M_SHADING_CORRECTION_GAIN_FIXED_POINT** to set the number of bits for the fractional part. It can have an X-size of up to 16384 pixels and a Y-size equal to the size of the DCF.
- Support for **M_SHADING_CORRECTION_GAIN_FIXED_POINT**.
  Sets the number of bits for the fraction part of the fixed point gain value that your digitizer should use when it performs a shading correction.
  To enable on-board shading correction and provide an offset, use **M_SHADING_CORRECTION**, **M_SHADING_CORRECTION_GAIN_ID**, and **M_SHADING_CORRECTION_OFFSET_ID**, respectively. Possible control/return values are:
  - **0&lt;= Value &lt;16**. Specifies the fractional part of the fixed point gain value.
- Support for **M_SHADING_CORRECTION_OFFSET_ID**.
  Sets the buffer containing the offset values that your digitizer should use when it performs a shading correction.
  To enable on-board shading correction and provide a gain, use **M_SHADING_CORRECTION** and **M_SHADING_CORRECTION_GAIN_ID**, respectively. Possible control/return values are:
  - **M_NULL**. Specifies to not apply an offset when performing a shading correction.
  - **ShadingCorrectionOffsetID**. Specifies the identifier of an **M_IMAGE** buffer containing the offset values. The buffer must be allocated as a signed 8-bit or signed 16-bit buffer.
- Support for **M_GRAB_TRIGGER_OVERLAP**.
  Sets how to deal with a new trigger that occurs while a grab is in progress. Possible control/return values are:
  - **M_DEFAULT**. Same as **M_RESET** or value specified in DCF.
  - **M_PREVIOUS_FRAME**. Specifies that a trigger received, while the grab is in progress, will be latched (stored). As soon as the current grab finishes, a new grab will be started.
  - **M_OFF**. Specifies that a new trigger is ignored.
  - **M_RESET**. Specifies that the current grab will be immediately stopped and a new grab will be started.
- Support for **M_ROTARY_ENCODER_POSITION_START_TRIGGER (+ M_ROTARY_ENCODERn)**.
  Sets the rotary decoder's counter value upon which to start generating triggers, when **M_ROTARY_ENCODER_OUTPUT_MODE** is set to **M_POSITION_TRIGGER_MULTIPLE**. If it is not set to this value, then **M_ROTARY_ENCODER_POSITION_START_TRIGGER** is ignored. Possible control/return values are:
  - **0 &lt; Value &lt;= 0xFFFFFFFF**. Specifies the value of the counter upon which to start generating triggers.
- Support for **M_ROTARY_ENCODER_OUTPUT_MODE (+ M_ROTARY_ENCODERn)**.
  Sets the rotary decoder's counter value and/or the direction of movement upon which the rotary decoder should output a pulse. Possible control/return values are:
  - **M_POSITION_TRIGGER_MULTIPLE**. Specifies to start triggering only when the counter value set with **M_ROTARY_ENCODER_POSITION_START_TRIGGER** is reached and then to generate a trigger at a set interval (change) in counter value. Set the interval with **M_ROTARY_ENCODER_POSITION_TRIGGER**.
- Support for **M_ROTARY_ENCODER_POSITION_TRIGGER (+ M_ROTARY_ENCODERn)**.
  Sets the rotary decoder's counter value at which a trigger is generated. Possible control/return values are:
  - **0 &lt; Value &lt;= 0xFFFFFFFF**. Specifies the counter value or change in counter value at which to generate a trigger.
    If **M_ROTARY_ENCODER_OUTPUT_MODE** is set to **M_POSITION_TRIGGER_MULTIPLE**, this will specify the change in counter value at which to generate a trigger, beginning at the counter value specified by **M_ROTARY_ENCODER_POSITION_START_TRIGGER**.
    If **M_ROTARY_ENCODER_OUTPUT_MODE** is set to **M_POSITION_TRIGGER**, this will specify the counter value at which to generate a trigger each time this value is reached.
    If **M_POSITION_TRIGGER** is set to any other control value, this value will be ignored.
- Support for **M_TIMER_TRIGGER_OVERLAP** + **M_TIMERn**.
  Sets how to deal with a new trigger that occurs while the associated timer has not yet expired (both its delay and duration). Possible control/return values are:
  - **M_DEFAULT**. Same as **M_RESET** or value specified in DCF.
  - **M_LATCH**. Specifies that a trigger received, while the associated timer has not expired, will be latched (stored). As soon as the current timer expires, a new trigger is issued.
  - **M_OFF**. Specifies that a new trigger is ignored.
  - **M_RESET**. Specifies that a new trigger automatically resets the timer (regardless of whether it is in its delay or active period) and then restarts the timer. This process will repeat for each new trigger received.
- Support for **M_TIMER_TRIGGER_RATE_DIVIDER**.
  Sets the frequency to accept trigger pulses (for example, if set to 2, the first trigger pulse is ignored and the second is accepted). Possible control/return values are:
  - **M_DEFAULT**. Specifies the default value; the default value is 1.
  - **1 &lt;= Value &lt;= 255**. Specifies the frequency with which to accept a trigger out of a series of trigger pulses. Note that if the value is set to 1, all trigger pulses are accepted.
- Support for **M_TIMER_DELAY2** + **M_TIMERn**.
  Sets the delay between the end of the first active portion of the timer output signal and the start of the second pulse. Possible control/return values are:
  - **M_DEFAULT**. Specifies the default value. This is the same as specified in the DFC; if no value is specified, then 0.
  - **Value > 0**. Specifies the delay, in nsec.
- Support for **M_TIMER_DURATION2** + **M_TIMERn**.
  Sets the duration for the active portion of the second pulse of the timer output signal. Possible control/return values are:
  - **M_DEFAULT**. Specifies the default value. This is the same as specified in the DFC; if no value is specified, then 0.
  - **Value > 0**. Specifies the duration of the active portion of the second pulse of the timer output signal, in nsec.
- Support for **M_GRAB_FRAME_BURST_SIZE**.
  Specifies the number of frames grabbed into the same buffer at each grab command. The size Y of the grab buffer must be equal to (`height of the frame` * **M_GRAB_FRAME_BURST_SIZE**). Possible control/return values are:
  - **M_DEFAULT**. Same as 1.
  - **1 &lt;= Value &lt;= 1023**. Sets the number of frames grabbed.
- Support for **M_GRAB_FRAME_BURST_MAX_TIME**.
  Specifies the maximum amount of time to wait for all the frames to be grabbed in the multi-frame buffer.
  The timer starts when the first frame is grabbed. The number of frames in the buffer can be inquired using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with **M_GRAB_FRAME_BURST_COUNT**. This is useful when the camera stops sending frames and the multi-frame buffer is only partially full. Possible control/return values are:
  - **M_DEFAULT**. Same as **M_INFINITE**.
  - **0.000008 &lt;= Value &lt;= 1.000000**. Specifies the maximum amount of time to wait, in seconds.
  - **M_INFINITE**. Specifies to wait indefinitely.
- Support for **M_GRAB_FRAME_BURST_END_TRIGGER_SOURCE**.
  Specifies the signal from which a rising edge signals the end of a multi-frame sequence. This is useful to force a partially completed multi-frame buffer to complete. Possible control/return values are:
  - **M_DEFAULT**. Same as **M_AUX_IO0**.
  - **M_AUX_IOn**. Specifies to use auxiliary input signal n as the trigger source, where n is the number of the auxiliary signal. Note that the specified auxiliary signal can also be a bidirectional signal set to input (using **M_IO_MODE** set to **M_INPUT**).
- Support for **M_GRAB_FRAME_BURST_END_TRIGGER_STATE**.
  Enables the **M_GRAB_FRAME_BURST_END_TRIGGER_SOURCE** source. Possible control/return values are:
  - **M_DEFAULT**. Same as **M_DISABLE**.
  - **M_DISABLE**. Disables the grab frame burst end trigger source.
  - **M_ENABLE**. Enables the grab frame burst end trigger source.

### MdigControlFeature/MdigInquireFeature

The following lists additional Radient eV-CL support when using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md)/[`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md).

- Support for **M_FEATURE_ENUM_ENTRY_DISPLAY_NAME** + `n`.
  Inquires the display name of the specified enumeration entry of the specified feature, where `n` is the index of the enumerated list. For more information, see **M_FEATURE_ENUM_ENTRY_NAME** in the command reference.
- Support for **M_STRING_ARRAY_SIZE(** **)**.
  Specifies that the feature value is expressed as a string of a specified size. The **M_STRING_ARRAY_SIZE(** **)** macro passes the size of the user-allocated buffer (first passed to the [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)'s UserVarPtr parameter). See **M_STRING_ARRAY_SIZE(** **)** in the command reference for more information.

### MdigGetHookInfo

The following lists additional Radient eV-CL support when using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md).

The following information types allows you to retrieve information about a GenICam SFNC-compliant event. The information types are only available if [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) was called from a function hooked to a GenICam event using **M_GC_EVENT** + **M_FEATURE_CHANGE**. In addition, the GenICam event must be enabled using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md), and the message channel must be supported by your camera.

- Support for **M_GC_FEATURE_CHANGE_NAME**.
  Retrieves the name of the GenICam feature that changed. The UserVarPtr must point to a user allocated array of type **AIL_TEXT_CHAR**.
- Support for **M_GC_FEATURE_CHANGE_NAME_SIZE**.
  Retrieves the size of the name of the GenICam feature that changed. The UserVarPtr must point to an **AIL_INT** that is the required string length +1.

The following allows you to retrieve information about grab frame burst events. Unless otherwise specified, you can retrieve these information types if you call this function from within a function hooked to any digitizer event using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) or [`MdigProcess`](../../Reference/dig/MdigProcess.md).

- Support for **M_GRAB_FRAME_BURST_COUNT**.
  Retrieves the number of frames grabbed in the multi-frame buffer. Possible return values are:
  - **Value > 0**. Specifies the number of frames grabbed.
- Support for **M_GRAB_FRAME_BURST_END_SOURCE**.
  Retrieves the type of event that generated the end of the frame burst. Multiple events can be set at the same time. Bitwise operators must be used to verify the presence of a specific returned value. Possible return values are:
  - **M_BURST_TRIGGER**. Specifies that trigger signal generates the end of the burst sequence. To specify the source signal, use [`MdigControl`](../../Reference/dig/MdigControl.md) with **M_GRAB_FRAME_BURST_END_TRIGGER_SOURCE**.
  - **M_BURST_MAX_TIME**. Specifies that the frame burst has taken as much time to complete as the maximum frame burst time. To specify the maximum time for a frame burst to complete, use [`MdigControl`](../../Reference/dig/MdigControl.md) with **M_GRAB_FRAME_BURST_MAX_TIME**.
  - **M_BURST_COUNT**. Specifies the number of frames that have been grabbed. To specify the number of frames in a frame burst, use [`MdigControl`](../../Reference/dig/MdigControl.md) with **M_GRAB_FRAME_BURST_SIZE**.

The following allows you to retrieve information from a data latch. These information types are only available if [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) was called from a function hooked to an end of grabbed frame event using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with **M_GRAB_FRAME_END** or from the hook-handler function (callback function) of [`MdigProcess`](../../Reference/dig/MdigProcess.md). In addition, you must have enabled the data latch to store this information, using [`MdigProcess`](../../Reference/dig/MdigProcess.md) with **M_DATA_LATCH_STATE**.

Note that you can only use data latches in association with the Aurora Imaging Library digitizer allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with **M_DEV0**. The following values require that you pass the UserVarPtr parameter the address of an AIL_INT64.

- Support for **M_DATA_LATCH_VALUE** + **M_LATCHn** + **M_VALUE_INDEX(i)**.
  Retrieves the stored data of the specified data latch. If there are multiple instances of this data (for example, your data latch is triggered at the end of each grabbed line), use + **M_VALUE_INDEX(i)**. Possible return values are:
  - **Value**. Specifies the stored data.
- Support for **M_DATA_LATCH_VALUE_ALL**.
  Retrieves all the stored data of the specified data latch. Possible return values are:
  - **Value**. Specifies all the stored data.
- Support for **M_DATA_LATCH_VALUE_COUNT**.
  Counts the number of items stored inside a data latch. Note that a data latch is reset at every frame event. Possible return values are:
  - **Value**. Specifies the number of instances. Note that a data latch is reset at every frame event.

The following is a combination constant for **M_DATA_LATCH_VALUE**. You can add the following value to **M_DATA_LATCH_VALUE** to specify the instance of the stored information to retrieve.

- + **M_VALUE_INDEX(AIL_INT64 IndexValue)**. Specifies the instance of stored information to retrieve. **IndexValue** is used to set the instance of stored information. For example, to read the information latched when the last line finishes being grabbed for a frame with 1024 lines, the instance would be 1023.
