---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: Additional_GigE_Vision_notes_command_reference
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / Additional GigE Vision notes command reference"
---

# Miscellaneous reference information for Zebra GigE Vision driver

This section includes additional command reference information for Zebra GigE Vision driver. The information found in this section might be a reiteration of content previously documented.

The following modules are mentioned in this section:

- [Mbuf](S13_Additional_GigE_Vision_notes_command_reference.md).
- [Mdig](S13_Additional_GigE_Vision_notes_command_reference.md).
- [Msys](S13_Additional_GigE_Vision_notes_command_reference.md).

## Mbuf

This section presents information about Zebra GigE Vision driver and the buffer module.

- Support for grab containers. See container support in the Mbuf module of the Help for details.

### Additions to MbufAlloc...

The following lists additional GigE Vision support when using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md).

- Support for **M_IMAGE** + **M_GRAB** + **M_DYNAMIC** buffer attribute.

### Additions to MbufInquire

The following lists additional GigE Vision support when using [`MbufInquire`](../../Reference/buf/MbufInquire.md).

- Support for **M_GRAB_TIME_STAMP**.
  Inquires the time when Aurora Imaging Library finished grabbing into the buffer, expressed in seconds. Inquire returns an **AIL_DOUBLE**.
- Support for **M_GRAB_TIME_STAMP_NS**.
  Inquires the time when Aurora Imaging Library finished grabbing into the buffer, expressed in nanoseconds. Inquire returns an **AIL_INT64**.
- Support for **M_CAMERA_TIME_STAMP**.
  Inquires the time at which the device generated the frame of data, expressed in seconds. Inquire returns an **AIL_DOUBLE**.
- Support for **M_CAMERA_TIME_STAMP_NS**.
  Inquires the time at which the device generated the frame of data expressed in nanoseconds. Inquire returns an **AIL_INT64**.
- Support for **M_COMPONENT_TIME_STAMP**.
  **BufId** must be that of a component. When grabbing into a container, from a device transmitting data in GenDC format, this inquire gets the time at which the device generated the data component expressed in seconds. Zero is returned if there is no component time stamp available. Inquire returns an **AIL_DOUBLE**. Note: components belonging to the same container can have different component time stamps.
- Support for **M_COMPONENT_TIME_STAMP_NS**.
  **BufId** must be that of a component. When grabbing into a container, from a device transmitting data in GenDC format, this inquire gets the time at which the device generated the data component expressed in nanoseconds. Zero is returned if there is no component time stamp available. Inquire returns an **AIL_INT64**. Note: components belonging to the same container can have different component time stamps.
- Support for **M_COMPONENT_TIME_STAMP_IS_PTP**.
  **BufId** must be that of a component. If **M_COMPONENT_TIME_STAMP_IS_PTP** returns **M_TRUE**, then **M_COMPONENT_TIME_STAMP_NS** is relative to the epoch January 1st 1970 0:00:00.
- Support for **M_LAYOUT_MODIFICATION_COUNT**.
  **BufId** must be that of a container. This value is modified every time a component is added or removed from the container. Inquire returns an **AIL_INT**.
- Support for **M_PFNC_VALUE**.
  Returns the buffer's format as defined in the GenICam Pixel Format Naming Convention (PFNC).
- Support for **M_PFNC_SUPPORT**.
  Returns **M_YES** if the PFNC format is a native Aurora Imaging Library format.
  Returns **M_WITH_COMPENSATION** if the PFNC format is not a native Aurora Imaging Library format but can be converted to an Aurora Imaging Library native format. A buffer of this type can only be used as a source buffer.
  Returns **M_NO** if Aurora Imaging Library cannot handle this type of format. In this case the user has to access the buffer's memory directly. **MbufInquire** with **M_HOST_ADDRESS** can be used to get a pointer to the buffer's data.

### Additions to MbufInquireContainer

The following lists additional GigE Vision support when using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md).

- Support for **M_COMPONENT_TIME_STAMP**.
  **BufId** must be that of a container and the **Component** parameter must not be set to **M_CONTAINER**. When grabbing into a container, from a device transmitting data in GenDC format, this inquire gets the time at which the device generated the data component expressed in seconds. Zero is returned if there is no component time stamp available. Inquire returns an **AIL_DOUBLE**. Note: components belonging to the same container can have different component time stamps.
- Support for **M_COMPONENT_TIME_STAMP_NS**.
  **BufId** must be that of a container and the **Component** parameter must not be set to **M_CONTAINER**. When grabbing into a container, from a device transmitting data in GenDC format, this inquire gets the time at which the device generated the data component expressed in nanoseconds. Zero is returned if there is no component time stamp available. Inquire returns an **AIL_INT64**. Note: components belonging to the same container can have different component time stamps.
- Support for **M_COMPONENT_TIME_STAMP_IS_PTP**.
  the **Component** parameter must not be set to **M_CONTAINER**. If **M_COMPONENT_TIME_STAMP_IS_PTP** returns **M_TRUE**, then **M_COMPONENT_TIME_STAMP_NS** is relative to the epoch January 1st 1970 0:00:00.
- Support for **M_LAYOUT_MODIFICATION_COUNT**.
  **BufId** must be that of a container and the **Component** parameter must be set to **M_CONTAINER**. This value is modified every time a component is added or removed from the container. Inquire returns an **AIL_INT**.

## Mdig

This section presents information about Zebra GigE Vision driver and the digitizer module.

### Additions to MdigAlloc

The following lists additional GigE Vision support when using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md).

- The **InitFlag** parameter now supports the following settings:
  - Support for **M_GC_XML_FORCE_DOWNLOAD**.
    Forces the Zebra driver to download the GigE Vision device's XML description file at [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). If this setting is omitted, the cached version present in the Aurora Imaging Library XML repository is used instead.
  - Support for **M_GC_XML_DOWNLOAD_SKIP**.
    Prevents the Zebra driver from downloading the GigE Vision device's XML description file at [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). The cached version present in the Aurora Imaging Library XML repository is used instead. The **M_GC_XML_DOWNLOAD_SKIP** flag is ignored if:
    - The XML description file is not present in the Aurora Imaging Library XML repository.
    - The XML description file name and/or file size, as reported by the GigE Vision device, has changed.
  - Support for **M_GC_MULTICAST_CONTROLLER**.
    Specifies that the allocated device will be the controller digitizer in a multicasting controller-worker relationship. When this setting is used, the GigE Vision device's stream and message channel (if present) will be programmed to use a destination IPv4 address in the multicast address range (from 224.0.0.0 to 239.255.255.255). By default, Aurora Imaging Library chooses an administratively scoped IP multicast address in the range 239.0.0.0 to 239.255.255.255. IP multicast also requires a network designed to deliver a multicast service. See the IP multicast section of the Aurora Imaging Capture Works help for more details.
  - Support for **M_GC_MULTICAST_WORKER**.
    Specifies that the allocated device will be a worker digitizer in a multicasting controller-worker relationship. See the IP multicast section of the Aurora Imaging Capture Works help for more details.
  - Support for **M_GC_MULTICAST_MONITOR**.
    Specifies that the allocated device will be a monitor digitizer in a multicasting controller-worker relationship.
  - Support for **M_GC_PACKET_SIZE_NEGOTIATION_SKIP**.
    Prevents the Zebra driver from negotiating a packet size with the GigE Vision device at [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). Instead, the GigE Vision device's packet size register is read and this value is used by the Zebra driver. If the read packet size is too small, acquisition reliability will suffer. Using this scheme, the packet size can be forced by storing it in the DCF or by using [`MdigControl`](../../Reference/dig/MdigControl.md) with **M_GC_PACKET_SIZE**. In either case, acquisition reliability will be affected if the forced packet size is too small or if the packet size is not supported by the network infrastructure.
  - Support for **M_DEV_NUMBER**.
    Supports customary allocation using device numbers.
- **M_GC_CAMERA_ID(MT(""))** can now be used with the **DigNum** parameter ([`MdigAlloc`](../../Reference/dig/MdigAlloc.md)), in conjunction with the following **InitFlag** parameter settings: **M_GC_DEVICE_IP_ADDRESS** or **M_GC_DEVICE_USER_NAME**. For example, the following 2 calls to [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) will allocate a camera whose IP address is 169.254.23.237 and whose user-defined name is MyCameraName, respectively.
  - **MdigAlloc(..., M_GC_CAMERA_ID(MT("169.254.23.237")), ..., M_GC_DEVICE_IP_ADDRESS, ...)**.
  - **MdigAlloc(..., M_GC_CAMERA_ID(MT("MyCameraName")), ..., M_GC_DEVICE_USER_NAME, ...)**.

### Additions to MdigControl

The following lists additional GigE Vision support when using [`MdigControl`](../../Reference/dig/MdigControl.md).

- Support for **M_GC_NETWORK_INTERFACE_CONFIGURATION**.
  Controls the GigE Vision device's persistent network interface configuration. The configuration bits that can be used can be any of the following:
  - **M_GC_PAUSE_RECEPTION_SUPPORT**.
  - **M_GC_PAUSE_GENERATION_SUPPORT**.
  - **M_GC_LINK_LOCAL_ADDRESS_SUPPORT**.
  - **M_GC_DHCP_SUPPORT**.
  - **M_GC_PERSISTENT_IP_SUPPORT**.
- Support for **M_GC_STREAM_CHANNEL_MULTICAST_ADDRESS_STRING**.
  Controls the multicast address used by the device to stream image data. The device must have been allocated with the **M_GC_MULTICAST_CONTROLLER** **InitFlag**. Supported control values are any valid IPv4 multicast address in the range 224.0.0.0 to 239.255.255.255.
- Support for **M_GC_MESSAGE_CHANNEL_MULTICAST_ADDRESS_STRING**.
  Controls the multicast address used by the device to send message channel event data. The device must have been allocated with the **M_GC_MULTICAST_CONTROLLER** **InitFlag**. Supported control values are any valid IPv4 multicast address in the range 224.0.0.0 to 239.255.255.255.
- Support for **M_GC_STREAM_PORT**.
  Controls the multicast UDP port number used for the stream channel. The device must have been allocated with the **M_GC_MULTICAST_CONTROLLER** **InitFlag**.
- Support for **M_GC_MESSAGE_PORT**.
  Controls the multicast UDP port number used for the message channel. The device must have been allocated with the **M_GC_MULTICAST_CONTROLLER** **InitFlag**.
- Support for **M_GC_NUMBER_OF_STREAM_CHANNELS**.
  Controls the number of stream channels on the GigE Vision device. This control can only be used when the digitizer is allocated with the **M_GC_MULTICAST_MONITOR** **InitFlag**.
- Support for **M_GC_STREAMING_MODE**.
  Controls the camera's image stream activation mechanism. Supported control values are:
  - **M_DEFAULT**: Lets the Zebra driver be in control when the camera's image stream is started/stopped.
  - **M_AUTOMATIC**: Same as **M_DEFAULT**.
  - **M_MANUAL**: Lets the user be in control when the camera's image stream is started/stopped.
- Support for **M_GC_STREAMING_START**.
  Starts the camera's image stream. **M_GC_STREAMING_MODE** must be set to **M_MANUAL**. Supported control values are:
  - **M_DEFAULT**.
- Support for **M_GC_STREAMING_STOP**.
  Stops the camera's image stream. Supported control values are:
  - **M_DEFAULT**.
- Support for **M_GC_STREAMING_STOP_CHECK_PERIOD**.
  Control value: **AIL_INT**. Represents the time period in milliseconds (ms), or **M_DEFAULT**. Default is 1000 ms. This is the period at which the driver checks if the camera's image stream should be stopped.
- Support for **M_GC_STREAMING_STOP_DELAY**.
  Control value: **AIL_INT**. Represents the delay, in ms, before stopping the camera's image stream. Default is 0 ms. When the **M_GC_STREAMING_STOP_CHECK_PERIOD** time elapses, this delay is applied before a check is made to stop the stream.
- Support for **M_GC_PIXEL_FORMAT**.
  Controls the camera's pixel format. The pixel format value must be a valid GigE Vision pixel format. See GVSP_Pixel_Formats.h for a list of supported values.
- Support for **M_GC_FEATURE_POLLING**.
  Controls the feature browser's polling thread. Your GigE Vision device can define polling values for features that can change without the user's input. An example of this is the "DeviceTemperature" feature. The polling thread can be used when the device's feature browser is displayed. Its role is to poll features that specify a polling period. Supported control values are:
  - **M_DEFAULT**: Same as **M_ENABLE**.
  - **M_ENABLE**: Enables the polling thread. Camera features that report polling support will be read periodically.
  - **M_DISABLE**: Disables the polling thread.
- Support for **M_DIGITIZER_INTERNAL_BUFFERS_NUM**.
  Controls the number of internal grab buffers allocated and used when the Zebra driver cannot directly grab into the user's buffer. Internal grab buffers are needed when the GenICam chunk mode is active. They are also needed when the grab buffer's format is not compatible with the camera's current pixel format. Supported control values are:
  - **M_DEFAULT**: Five internal grab buffers will be allocated.
  - A positive integer between 2 and 1024 representing the number of internal grab buffers to use. An error will get generated if the amount of non paged memory available is insufficient to allocate the buffers.
- Support for **M_GC_ACQUISITION_MODE**.
  Controls the camera's **AcquisitionMode** feature. Can be set to **M_CONTINUOUS**/**M_DEFAULT **or **M_SINGLE_FRAME**.
- Support for **M_GC_HEARTBEAT_STATE** (if supported by the camera).
  Controls if the heartbeat mechanism is used to keep the GigE Vision Control channel open. Supported values: **M_ENABLE**, **M_DISABLE**, **M_DEFAULT** (same as **M_ENABLE**). Can only be used with GigE Vision cameras that support the heartbeat disable feature.
- Support for **M_GC_PACKET_TIMEOUT**.
  Value is in ms. Default is 10 ms. This is the maximum amount of time to wait before flagging a packet as dropped.
- Support for **M_GC_FRAME_TIMEOUT**.
  Value is in ms. Default is 100 ms. This is the maximum amount of time to wait for the remaining packets of a frame after the trailer packet is received. If packets are missing, the frame is flagged as corrupted.
- Support for **M_GC_PACKET_MAX_RETRIES**.
  Default is 3. This is the maximum number of retries for a packet. If reached, the frame is corrupted.
- Support for **M_GC_FRAME_MAX_RETRIES**.
  Default is 30. This is the maximum number of packet retries per frame. If reached, the frame is corrupted.
- Support for **M_GC_STATISTICS_RESET**.
  Control value must be **M_DEFAULT**. This resets the statistics.
- Support for **M_GC_MAX_NBR_PACKETS_OUT_OF_ORDER**.
  Default value is 0. Controls the number of packets to wait before asking for a retransmission. This is useful when using link aggregated cameras where packets are received out of order.
- Support for **M_USER_BIT_FORMAT**+Number.
  Controls the current electrical format of the specified physical input or output Line. Number can range from 0 or 1 up to the maximum number of Lines in the camera. Supported control values are:
  - **M_NO_CONNECT**: The line is not connected.
  - **M_TRI_STATE**: The Line is currently in Tri-State mode (Not driven).
  - **M_TTL**: The Line is currently accepting or sending TTL level signals.
  - **M_RS422**: The Line is currently accepting or sending RS422 level signals.
  - **M_LVDS**: The Line is currently accepting or sending LVDS level signals.
  - **M_OPTO**: The Line is Opto-Coupled.
  An Aurora Imaging Library error will be generated for this control type if the specified Line number does not exist in the camera or if the Line format cannot be changed by the camera.
  An Aurora Imaging Library error will be generated for any of the previous control values if the specified format does not exist for the specified Line number.
- Support for **M_USER_BIT_MODE**+Number.
  Controls if the physical Line is used to input or output a signal. Number can range from 0 or 1 up to the maximum number of Lines in the camera. Supported control values are:
  - **M_INPUT**: The selected physical line is used to input an electrical signal.
  - **M_OUTPUT**: The selected physical line is used to output an electrical signal.
  An Aurora Imaging Library error will be generated for this control type if the specified Line number does not exist in the camera or if the Line mode cannot be changed by the camera.
  An Aurora Imaging Library error will be generated for any of the previous control values if the specified mode does not exist for the specified Line number.
- Support for **M_AUX_SIGNAL_SOURCE**+Number.
  Selects which internal signal to output on the selected Line. The corresponding **M_USER_BIT_MODE** must be **M_OUTPUT**. Number can range from 0 or 1 up to the maximum number of Lines in the camera. Supported control values are:
  - **M_USER_BIT**: The line will output a user-bit signal.
  - **M_TIMER1**: The line will output if the chosen timer is in an active state.
  - **M_TIMER2**: The line will output if the chosen timer is in an active state.
  An Aurora Imaging Library error will be generated for this control type if the specified Line number does not exist in the camera or if the Line mode is set to **M_INPUT**.
  An Aurora Imaging Library error will be generated for any of the previous control values if the specified signal source does not exist for the specified Line number.
- Support for **M_USER_BIT_VALUE**+Number.
  Sets the value of the specified user bit. Number can range from 0 or 1 up to the maximum number of user-defined signals in the camera. Supported control values are:
  - **M_ON**: The selected physical line is used to input an electrical signal.
  - **M_OFF**: The selected physical line is used to output an electrical signal.
  An Aurora Imaging Library error will be generated for this control type if the specified Line number does not exist in the camera.
- Support for **M_GC_FEATURE_BROWSER**.
  Creates the device feature browser dialog from an Aurora Imaging Library console application. Supported control values are:
  - **M_DEFAULT**: Opens the feature browser dialog. The calling thread is blocked.
  - **M_OPEN**+**M_SYNCHRONOUS**: Same as **M_DEFAULT**.
  - **M_OPEN**+**M_ASYNCHRONOUS**: Opens the feature browser dialog. The calling thread returns as soon as the feature browser is created.
  - **M_CLOSE**: Closes the feature browser dialog.

### Additions to MdigControlFeature

The following lists additional GigE Vision support when using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md).

- The **FeatureType** parameter has been changed to **UserVarType**. This was done to simplify writing code with [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md)/[`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md). **UserVarType** must always reflect the type of the pointer passed to the **UserVarPtr** parameter. Legacy code is transparently supported, but we recommend you update your code. Note that **M_TYPE_REGISTER** now becomes **M_TYPE_UINT8**, **M_TYPE_ENUMERATION** now becomes **M_TYPE_INT64** or **M_TYPE_STRING**, and **M_TYPE_COMMAND** now becomes **M_DEFAULT**. Data type conversions are made, whenever possible, in cases where the feature's "native" data type is different than the **UserVarType** supplied. Regardless of a feature's "native" data type it can always be read as a string.
  The following is a list of example calls using the new **UserVarType**:
  - **MdigControlFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("Width"), M_TYPE_INT64, &Int64Var)**.
  - **MdigControlFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("Gain"), M_TYPE_DOUBLE, &DoubleVar)**.
  - **MdigControlFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("ReverseX"), M_TYPE_BOOLEAN, &BoolVar)**.
  - **MdigControlFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("PixelFormat"), M_TYPE_STRING, AIL_TEXT("Mono8"))**.
  - **MdigControlFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("LUTValueAll"), M_TYPE_UINT8, Uint8Array)**.
  - **MdigControlFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("AcquisitionStart"), M_DEFAULT, M_NULL)**.
- Support for **M_FEATURE_CHANGE_HOOK**.
  Identifies the specified** FeatureName** to trigger the **M_FEATURE_CHANGE** hook callback. You must be hooked to the **M_FEATURE_CHANGE** hook type using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md).

### Additions to MdigGetHookInfo

The following lists additional GigE Vision support when using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md).

- Hooking to a GenICam feature change event.
  **M_GC_FEATURE_CHANGE_NAME** can be used from an **M_FEATURE_CHANGE** hook function. The function returns the name of the GenICam feature that triggered the hook. **UserVarPtr** must point to a user allocated array of type **AIL_TEXT_CHAR**.
  **M_GC_FEATURE_CHANGE_NAME_SIZE** can be used from an **M_FEATURE_CHANGE** hook function. The function returns the number of characters in the string returned by **M_GC_FEATURE_CHANGE_NAME**. **UserVarPtr** must point to an **AIL_INT**.
- Support for **M_GC_FRAME_STATUS_CODE**.
  Returns an **AIL_INT**. Returns the status code from the last image acquired. The status code is extracted from the GigE Vision device's stream packets.
- Support for **M_GC_PACKETS_MISSED**.
  Returns an **AIL_INT**. Returns the number of missing packets in the corrupted frame. If 0, the frame is not corrupted.
- Support for **M_GC_PACKETS_RECEIVED**.
  Returns an **AIL_INT**. This is the number of packets in the frame.
- Support for **M_GC_PACKETS_RESENDS_NUM**.
  Returns an **AIL_INT**. This is the number of resend commands sent to the camera for a frame. One resend command can contain multiple consecutive packets.
- Support for **M_GC_PACKETS_RECOVERED**.
  Returns an **AIL_INT**. This is the number of packets recovered in the frame.
- Support for **M_GC_FRAME_ERROR_CODE**.
  Returns an **AIL_INT**. This is the error code returned by the camera for a frame.
- Support for **M_GC_FRAME_LINE_COUNT**.
  Returns an **AIL_INT**. This is the number of lines in the frame.
- Support for **M_GC_FRAME_BLOCK_ID**.
  Returns an **AIL_INT**. This is the frame BlockID value from the camera. The BlockID in the camera is implemented as a 16-bit running counter.
- Support for **M_GC_FRAME_TIMESTAMP**.
  Returns an **AIL_DOUBLE** in seconds. This is the frame timestamp from the camera.
- Support for **M_COUNTER_INDEX**.
  Returns an **AIL_INT**. This is the index of the counter that generated the event.
- Support for **M_TIMER_INDEX**.
  Returns an **AIL_INT**. This is the index of the timer that generated the event.
- Support for **M_LINE_INDEX**.
  Returns an **AIL_INT**. This is the index of the line that generated the event.
- Support for **M_GC_EVENT_TYPE**.
  Returns an **AIL_INT**. This is the raw GigE Vision event sent by the camera.

### Additions to MdigHookFunction

The following lists additional GigE Vision support when using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md).

- Hooking to a GenICam feature change event.
  **M_GC_FEATURE_CHANGE** can be used as a hook type. The hook is triggered when a GenICam feature gets invalidated. This usually occurs when a feature or a dependent feature is written.
- Support for **M_GC_EVENT**.
  Hooks an Aurora Imaging Library function callback to a generic GigE Vision camera event. Note that the underlying event must be enabled in the camera with [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md).
- Support for **M_GC_EVENT**+**M_ACQUISITION_TRIGGER**.
  Hooks an Aurora Imaging Library function callback to the GenICam "AcquisitionTrigger" camera event.
- Support for **M_GC_EVENT**+**M_ACQUISITION_START**.
  Hooks an Aurora Imaging Library function callback to the GenICam "AcquisitionStart" camera event.
- Support for **M_GC_EVENT**+**M_ACQUISITION_END**.
  Hooks an Aurora Imaging Library function callback to the GenICam "AcquisitionEnd" camera event.
- Support for **M_GC_EVENT**+**M_ACQUISITION_TRANSFER_START**.
  Hooks an Aurora Imaging Library function callback to the GenICam "AcquisitionTransferStart" camera event.
- Support for **M_GC_EVENT**+**M_ACQUISITION_TRANSFER_END**.
  Hooks an Aurora Imaging Library function callback to the GenICam "AcquisitionTransferEnd" camera event.
- Support for **M_GC_EVENT**+**M_ACQUISITION_ERROR**.
  Hooks an Aurora Imaging Library function callback to the GenICam "AcquisitionError" camera event.
- Support for **M_GC_EVENT**+**M_FRAME_TRIGGER**.
  Hooks an Aurora Imaging Library function callback to the GenICam "FrameTrigger" camera event.
- Support for **M_GC_EVENT**+**M_FRAME_START**.
  Hooks an Aurora Imaging Library function callback to the GenICam "FrameStart" camera event.
- Support for **M_GC_EVENT**+**M_FRAME_END**.
  Hooks an Aurora Imaging Library function callback to the GenICam "FrameEnd" camera event.
- Support for **M_GC_EVENT**+**M_FRAME_TRANSFER_START**.
  Hooks an Aurora Imaging Library function callback to the GenICam "FrameTransferStart" camera event.
- Support for **M_GC_EVENT**+**M_FRAME_TRANSFER_END**.
  Hooks an Aurora Imaging Library function callback to the GenICam "FrameTransferEnd" camera event.
- Support for **M_GC_EVENT**+**M_EXPOSURE_START**.
  Hooks an Aurora Imaging Library function callback to the GenICam "ExposureStart" camera event.
- Support for **M_GC_EVENT**+**M_EXPOSURE_END**.
  Hooks an Aurora Imaging Library function callback to the GenICam "ExposureEnd" camera event.
- Support for **M_GC_EVENT**+**M_COUNTER_START**+Number.
  Hooks an Aurora Imaging Library function callback to the GenICam "CounterXStart" camera event. Number determines the index of the Counter used to generate the event. Number can range from 0 to 8.
- Support for **M_GC_EVENT**+**M_COUNTER_END**+Number.
  Hooks an Aurora Imaging Library function callback to the GenICam "CounterXEnd" camera event. Number determines the index of the Counter used to generate the event. Number can range from 0 to 8.
- Support for **M_GC_EVENT**+**M_TIMER_START**+Number.
  Hooks an Aurora Imaging Library function callback to the GenICam "TimerXStart" camera event. Number determines the index of the Timer used to generate the event. Number can range from 0 to 8.
- Support for **M_GC_EVENT**+**M_TIMER_END**+Number.
  Hooks an Aurora Imaging Library function callback to the GenICam "TimerXEnd" camera event. Number determines the index of the Timer used to generate the event. Number can range from 0 to 8.
- Support for **M_GC_EVENT**+**M_LINE_RISING_EDGE**+Number.
  Hooks an Aurora Imaging Library function callback to the GenICam "LineXRisingEdge" camera event. Number determines the index of the Line used to generate the event. Number can range from 0 to 32.
- Support for **M_GC_EVENT**+**M_LINE_FALLING_EDGE**+Number.
  Hooks an Aurora Imaging Library function callback to the GenICam "LineXFallingEdge" camera event. Number determines the index of the Line used to generate the event. Number can range from 0 to 32.
- Support for **M_GC_EVENT**+**M_LINE_ANY_EDGE**+Number.
  Hooks an Aurora Imaging Library function callback to the GenICam "LineXRisingEdge" and "LineXFallingEdge camera events. Number determines the index of the Line used to generate the event. Number can range from 0 to 32.
- Notes:
  **M_UNHOOK** can be used with all of the above **M_GC_EVENT** events to disable the underlying event in the camera.
  The above message channel events must be implemented in the GigE Vision camera, otherwise calling [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with **M_GC_EVENT**+... will generate an Aurora Imaging Library error message.

### Additions to MdigInquire

The following lists additional GigE Vision support when using [`MdigInquire`](../../Reference/dig/MdigInquire.md).

- Support for **M_GC_PIXEL_FORMAT_STRING**.
  Returns the camera's **PixelFormat** as a string.
- Support for **M_GC_LOCAL_CONTROL_PORT**.
  Returns the host UDP port number for the control channel.
- Support for **M_GC_LOCAL_MESSAGE_PORT**.
  Returns the host UDP port number for the control channel.
- Support for **M_GC_LOCAL_STREAM_PORT**.
  Returns the host UDP port number for the control channel.
- Support for **M_GC_REMOTE_CONTROL_PORT**.
  Returns the camera's UDP port number for the control channel.
- Support for **M_GC_REMOTE_MESSAGE_PORT**.
  Returns the camera's UDP port number for the control channel.
- Support for **M_GC_REMOTE_STREAM_PORT**.
  Returns the camera's UDP port number for the control channel.
- Support for **M_GC_LOCAL_IP_ADDRESS**.
  Returns the IP address of the host NIC as an **AIL_UINT64**.
- Support for **M_GC_LOCAL_IP_ADDRESS_STRING**.
  Returns the IP address of the host NIC as a string.
- Support for **M_GC_LOCAL_MAC_ADDRESS**.
  Returns the MAC address of the host NIC as an **AIL_UINT64**.
- Support for **M_GC_LOCAL_MAC_ADDRESS_STRING**.
  Returns the MAC address of the host NIC as a string.
- Support for **M_GC_REMOTE_IP_ADDRESS**.
  Returns the IP address of the camera as an **AIL_UINT64**.
- Support for **M_GC_REMOTE_IP_ADDRESS_STRING**.
  Returns the IP address of the camera as a string.
- Support for **M_GC_ REMOTE _MAC_ADDRESS**.
  Returns the MAC address of the camera as an **AIL_UINT64**.
- Support for **M_GC_ REMOTE _MAC_ADDRESS_STRING**.
  Returns the MAC address of the camera as a string.
- Support for **M_GC_CONTROL_PROTOCOL_CAPABILITY**.
  Returns an **AIL_INT** that represents the control channel capabilities of the GigE Vision device. Capability bits returned can be any combination of the following:
  - **M_GC_USER_DEFINED_NAME_SUPPORT**.
  - **M_GC_SERIAL_NUMBER_SUPPORT**.
  - **M_GC_HEARTBEAT_DISABLE_SUPPORT**.
  - **M_GC_LINK_SPEED_REGISTER_SUPPORT**.
  - **M_GC_PORT_AND_IP_REGISTER_SUPPORT**.
  - **M_GC_MANIFEST_TABLE_SUPPORT**.
  - **M_GC_TEST_DATA_SUPPORT**.
  - **M_GC_DISCOVERY_ACK_DELAY_SUPPORT**.
  - **M_GC_WRITABLE_DISCOVERY_ACK_DELAY_SUPPORT**.
  - **M_GC_EXTENDED_STATUS_CODES_1_SUPPORT**.
  - **M_GC_PRIMARY_APP_SWITCHOVER_SUPPORT**.
  - **M_GC_UNCONDITIONAL_ACTION_SUPPORT**.
  - **M_GC_IEEE_1588_SUPPORT**.
  - **M_GC_EXTENDED_STATUS_CODES_2_SUPPORT**.
  - **M_GC_SCHEDULED_ACTION_SUPPORT**.
  - **M_GC_ACTION_SUPPORT**.
  - **M_GC_PENDING_ACK_SUPPORT**.
  - **M_GC_EVENT_DATA_SUPPORT**.
  - **M_GC_EVENT_SUPPORT**.
  - **M_GC_PACKET_RESEND_SUPPORT**.
  - **M_GC_WRITE_MEM_SUPPORT**.
  - **M_GC_CONCATENATION_SUPPORT**.
- Support for **M_GC_STREAM_PROTOCOL_CAPABILITY**.
  Returns an **AIL_INT** that represents the stream protocol capabilities of the GigE Vision device. Capability bits returned can be any combination of the following:
  - **M_GC_FIREWALL_TRAVERSAL_SUPPORT**.
  - **M_GC_LEGACY_16BIT_BLOCK_SUPPORT**.
- Support for **M_GC_MESSAGE_PROTOCOL_CAPABILITY**.
  Returns an **AIL_INT** that represents the message protocol capabilities of the GigE Vision device. Capability bits returned can be any combination of the following:
  - **M_GC_FIREWALL_TRAVERSAL_SUPPORT**.
- Support for **M_GC_NETWORK_INTERFACE_CAPABILITY**.
  Returns an **AIL_INT** that represents the GigE Vision device networking interface capabilities. Capability bits returned are can be any combination of the following:
  - **M_GC_PAUSE_RECEPTION_SUPPORT**.
  - **M_GC_PAUSE_GENERATION_SUPPORT**.
  - **M_GC_LINK_LOCAL_ADDRESS_SUPPORT**.
  - **M_GC_DHCP_SUPPORT**.
  - **M_GC_PERSISTENT_IP_SUPPORT**.
- Support for **M_GC_NETWORK_INTERFACE_CONFIGURATION**.
  Controls the GigE Vision device's persistent network interface configuration. The inquire returns an **AIL_INT** which represents the currently enabled configuration. The configuration bits that can be used can be any of the following:
  - **M_GC_PAUSE_RECEPTION_SUPPORT**.
  - **M_GC_PAUSE_GENERATION_SUPPORT**.
  - **M_GC_LINK_LOCAL_ADDRESS_SUPPORT**.
  - **M_GC_DHCP_SUPPORT**.
  - **M_GC_PERSISTENT_IP_SUPPORT**.
- Support for **M_GC_PHYSICAL_LINK_CONFIGURATION_CAPABILITY**.
  Returns an **AIL_INT** that represents the GigE Vision device physical link configuration capabilities. Capability bits returned can be any combination of the following:
  - **M_GC_SINGLE_LINK_SUPPORT**.
  - **M_GC_MULTIPLE_LINK_SUPPORT**.
  - **M_GC_STATIC_LINK_AGGREGATION_SUPPORT**.
  - **M_GC_DYNAMIC_LINK_AGGREGATION_SUPPORT**.
- Support for **M_GC_STREAM_CHANNEL_CAPABILITY**.
  Returns an **AIL_INT** that represents the stream channel capabilities of the GigE Vision device. Capability bits returned can be any combination of the following:
  - **M_GC_BIG_AND_LITTLE_ENDIAN_SUPPORT**.
  - **M_GC_IP_REASSEMBLY_SUPPORT**.
  - **M_GC_MULTI_ZONE_SUPPORT**.
  - **M_GC_PACKET_RESEND_OPTION_SUPPORT**.
  - **M_GC_ALL_IN_SUPPORT**.
  - **M_GC_UNCONDITIONAL_STREAMING_SUPPORT**.
  - **M_GC_EXTENDED_CHUNK_DATA_SUPPORT**.
- Support for **M_GC_STREAM_CHANNEL_MULTICAST_ADDRESS_STRING**.
  Returns the multicast address used by the device to stream image data. The inquire returns an **AIL_TEXT_CHAR** string.
- Support for **M_GC_MESSAGE_CHANNEL_MULTICAST_ADDRESS_STRING**.
  Returns the multicast address used by the device to send message channel event data. The inquire returns an **AIL_TEXT_CHAR** string.
- Support for **M_GC_MULTICAST_CONTROLLER_CONNECTED**.
  Returns an **AIL_INT**. Returns **M_TRUE** if a controller digitizer is connected to the device.
- Support for **M_GC_STREAM_PORT**.
  Returns the multicast UDP port number used for the stream channel. The device must have been allocated with the **M_GC_MULTICAST_CONTROLLER** **InitFlag**. The inquire returns an **AIL_INT**.
- Support for **M_GC_MESSAGE_PORT**.
  Returns the multicast UDP port number used for the message channel. The device must have been allocated with the **M_GC_MULTICAST_CONTROLLER** **InitFlag**. The inquire returns an **AIL_INT**.
- Support for **M_GC_NUMBER_OF_STREAM_CHANNELS**.
  Returns the number of stream channels supported by the GigE Vision device. The inquire can always be used. The inquire returns an **AIL_INT**.
- Support for **M_GC_STREAMING_MODE**.
  Returns the camera's image stream activation mechanism. Inquire returns an **AIL_INT**.
- Support for **M_GC_PIXEL_FORMAT**.
  Returns the camera's pixel format. The pixel format value must be a valid GigE Vision pixel format. See GVSP_Pixel_Formats.h for a list of supported values. The inquire returns an **AIL_INT**.
- Support for **M_GC_FEATURE_POLLING**.
  Inquire returns an **AIL_INT**. Returns the feature browser's polling thread. Your GigE Vision device can define polling values for features that can change without the user's input. An example of this is the "DeviceTemperature" feature. The polling thread can be used when the device's feature browser is displayed. Its role is to poll features that specify a polling period.
- Support for **M_DIGITIZER_INTERNAL_BUFFERS_NUM**.
  Inquire returns an **AIL_INT**. Returns the number of internal grab buffers allocated and used when the Zebra driver cannot directly grab into the user's buffer. Internal grab buffers are needed when the GenICam chunk mode is active. They are also needed when the grab buffer's format is not compatible with the camera's current pixel format.
- Support for **M_SOURCE_DATA_FORMAT**.
  Inquire returns an **AIL_INT64**. Returns a value representing the Aurora Imaging Library buffer format that is compatible with the camera's current **PixelFormat**. Useful for allocating grab buffers and avoiding costly color space conversions, Ex:
  - **Mono8** **PixelFormat** will return **M_MONO8**+**M_PLANAR**.
  - **Mono10** **PixelFormat** will return **M_MONO16**+**M_PLANAR**.
  - **BayerBG8** **PixelFormat** will return **M_YUV16_YUYV**+**M_PACKED**.
  - **BayerBG12Packed** **PixelFormat** will return** M_RGB48**+**M_PACKED**.
  - **YUYVPacked** **PixelFormat** will return **M_YUV16_YUYV**+**M_PACKED**.
- Support for **M_GC_TOTAL_PACKET_CACHE_HITS**.
  Inquire returns an **AIL_INT**. Represents the total number of packets that passed through the packet cache.
- Support for **M_GC_TOTAL_FRAME_CACHE_HITS**.
  Inquire returns an **AIL_INT**. Represents the total number of partial or complete frames that passed through the packet cache.
- Support for **M_GC_NIC_FRIENDLY_NAME**.
  Inquire returns an **AIL_TEXT_CHAR** string. Returns the network adapter's friendly name connected to your GigE Vision device.
- Support for **M_GC_FRAME_STATUS_CODE**.
  Inquire returns an **AIL_INT**. Returns the status code from the last image acquired. The status code is extracted from the GigE Vision device's stream packets.
- Support for **M_GC_ACQUISITION_MODE**.
  Inquire returns an **AIL_INT**. Returns the camera's **AcquisitionMode** feature.
- Support for **M_GC_FRAME_TIMESTAMP**.
  Inquire returns an **AIL_DOUBLE**. Inquires the timestamp, in seconds, of the last frame grabbed.
- Support for **M_GC_COUNTER_TICK_FREQUENCY**.
  Inquire returns an **AIL_INT64**. Inquires the camera's tick frequency register. A value of 0 Hz indicates the camera does not support time stamps.
- Support for **M_GC_HEARTBEAT_STATE** (if supported by the camera).
  Inquire returns an **AIL_INT**. Supported values: **M_ENABLE**, **M_DISABLE**, **M_DEFAULT** (same as **M_ENABLE**). Inquires if the heartbeat mechanism is used to keep the GigE Vision Control channel open. Can only be used with GigE Vision cameras that support the heartbeat disable feature.
- Support for **M_GC_NIC_IP_ADDRESS**.
  Inquire returns an **AIL_INT64**. Returns the host NIC's IP address used with the camera.
- Support for **M_GC_NIC_MAC_ADDRESS**.
  Inquire returns an **AIL_INT64**. Returns the host NIC's MAC address used with the camera.
- Support for **M_GC_PACKET_TIMEOUT**.
  Inquire returns an **AIL_INT**. Value is in ms. Default is 10 ms. This is the maximum amount of time to wait before flagging a packet as dropped.
- Support for **M_GC_FRAME_TIMEOUT**.
  Inquire returns an **AIL_INT**. Value is in ms. Default is 100 ms. This is the maximum amount of time to wait for the remaining packets of a frame after the trailer packet is received. If packets are missing, the frame is flagged as corrupted.
- Support for **M_GC_PACKET_MAX_RETRIES**.
  Inquire returns an **AIL_INT**. Default is 3. This is the maximum number of retries for a packet. If reached, the frame is corrupted.
- Support for **M_GC_FRAME_MAX_RETRIES**.
  Inquire returns an **AIL_INT**. Default is 30. This is the maximum number of packet retries per frame. If reached, the frame is corrupted.
- Support for **M_GC_TOTAL_PACKETS_MISSED**.
  Inquire returns an **AIL_INT**. This is the total number of packets that are missing. Does not include recovered packets.
- Support for **M_GC_TOTAL_PACKETS_RECEIVED**.
  Inquire returns an **AIL_INT**. This is the total number of packets received.
- Support for **M_GC_TOTAL_PACKETS_RESENDS_NUM**.
  Inquire returns an **AIL_INT**. This is the total number of emitted PACKETRESEND commands. One resend command can contain multiple consecutive packets.
- Support for **M_GC_TOTAL_PACKETS_RECOVERED**.
  Inquire returns an **AIL_INT**. This is the total number of packets recovered.
- Support for **M_GC_TOTAL_FRAMES_GRABBED**.
  Inquire returns an **AIL_INT**. This is the total number of frames grabbed.
- Support for **M_GC_TOTAL_FRAMES_CORRUPTED**.
  Inquire returns an **AIL_INT**. Returns the number of corrupted frames.
- Support for **M_GC_TOTAL_PACKETS_RECEIVED_OUT_OF_ORDER**.
  Inquire returns an **AIL_INT**. This is the total number of packets that were received out of order, which usually happens with link aggregated cameras. If this number is high, increase the value of **M_GC_MAX_NBR_PACKETS_OUT_OF_ORDER**.
- Support for **M_GC_MAX_NBR_PACKETS_OUT_OF_ORDER**.
  Inquire returns an **AIL_INT**. Default value is 0. Controls the number of packets to wait before asking for a retransmission. This is useful when using link aggregated cameras where packets are received out of order.
- Support for **M_PROCESS_PENDING_GRAB_NUM**.
  Inquire returns an **AIL_INT**. Returns the number of buffers remaining in the list of buffers used by [`MdigProcess`](../../Reference/dig/MdigProcess.md). Note that this number includes the buffer used by the grab in progress. When dealing with round-robin grabbing, this number will typically be relatively close to the total number of allocated grab buffers.
- Support for **M_USER_BIT_FORMAT**+Number.
  Inquire returns an **AIL_INT**. Inquires the current electrical format of the specified physical input or output Line. Number can range from 0 or 1 up to the maximum number of Lines in the camera.
- Support for **M_USER_BIT_MODE**+Number.
  Inquire returns an **AIL_INT**. Inquires if the physical Line is used to input or output a signal. Number can range from 0 or 1 up to the maximum number of Lines in the camera.
- Support for **M_AUX_SIGNAL_SOURCE**+Number.
  Inquire returns an **AIL_INT**. Inquires which internal signal to output on the selected Line. The corresponding **M_USER_BIT_MODE** must be **M_OUTPUT**. Number can range from 0 or 1 up to the maximum number of Lines in the camera.
- Support for **M_USER_BIT_VALUE**+Number.
  Inquire returns an **AIL_INT**. Returns the current status of the selected input or output Line. Number can range from 0 or 1 up to the maximum number of user-defined signals in the camera.

### Additions to MdigInquireFeature

The following lists additional GigE Vision support when using [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md).

- Support for **M_FEATURE_ENUM_ENTRY_TOOLTIP** + N.
  Returns the nth enum entry's tooltip string. The string size can be inquired by adding the **M_STRING_SIZE** combination constant.
- Support for **M_FEATURE_ENUM_ENTRY_DESCRIPTION** + N.
  Returns the nth enum entry's description string. The string size can be inquired by adding the **M_STRING_SIZE** combination constant.
- Support for **M_FEATURE_ENUM_ENTRY_ACCESS_MODE ** + N.
  Returns the nth enum entry's access mode. See **M_FEATURE_ACCESS_MODE ** for details.
- Support for **M_FEATURE_ENUM_ENTRY_VISIBILITY** + N.
  Returns the nth enum entry's visibility value. See **M_FEATURE_VISIBILITY ** for details.
- Support for **M_FEATURE_ENUM_ENTRY_CACHING_MODE** + N.
  Returns the nth enum entry's caching mode. See **M_FEATURE_CACHING_MODE** for details.
- Support for **M_FEATURE_ENUM_ENTRY_STREAMABLE** + N.
  Returns the nth enum entry's streamable value. See **M_FEATURE_STREAMABLE** for details.
- Support for **M_FEATURE_SELECTOR_COUNT**.
  Returns the number of features selected by the specified **FeatureName** parameter. Note that the **FeatureName** parameter must specify a selector type feature.
- Support for **M_FEATURE_SELECTOR_NAME** + N.
  Returns the nth feature name, as a string, selected by the specified feature. Note that the **FeatureName** parameter must specify a selector type feature.
- Support for **M_FEATURE_VALID_VALUE_COUNT**.
  Integer and floating-point type features can support a fixed list of valid values instead of the traditional minimum, maximum, and increment values. **M_FEATURE_VALID_VALUE_COUNT** returns the number of valid values supported. A value of 0 returned indicates the feature does not support a list of valid values; minimum, maximum and increment values must be used instead.
- Support for **M_FEATURE_VALID_VALUE** + N.
  Returns the nth valid value for an integer of floating-point type feature.
- The **FeatureType** parameter has been changed to **UserVarType**. This was done to simplify writing code with [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md)/[`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md). **UserVarType** must always reflect the type of the pointer passed to the **UserVarPtr** parameter. Legacy code is transparently supported, but we recommend you update your code. Note that **M_TYPE_REGISTER** now becomes **M_TYPE_UINT8**, **M_TYPE_ENUMERATION** now becomes **M_TYPE_INT64** or **M_TYPE_STRING**, and **M_TYPE_COMMAND** now becomes **M_DEFAULT**. Data type conversions are made, whenever possible, in cases where the feature's "native" data type is different than the **UserVarType** supplied. Regardless of a feature's "native" data type it can always be read as a string. See Board-specific examples for details.
  The following is a list of example calls using the new **UserVarType**:
  - **MdigInquireFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("Width"), M_TYPE_INT64, &Int64Var)**.
  - **MdigInquireFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("Gain"), M_TYPE_DOUBLE, &DoubleVar)**.
  - **MdigInquireFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("ReverseX"), M_TYPE_BOOLEAN, &BoolVar)**.
  - **MdigInquireFeature(Digitizer, M_FEATURE_VALUE + M_STRING_SIZE, AIL_TEXT("PixelFormat"), M_TYPE_AIL_INT, &IntVar)**.
  - **MdigInquireFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("PixelFormat"), M_TYPE_STRING, TextCharArray)**.
  - **MdigInquireFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("LUTValueAll"), M_TYPE_UINT8, Uint8Array)**.
- **M_FEATURE_USER_ARRAY_SIZE** can now be used with [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) when the data type returned is a string or an array of bytes (register). The **M_FEATURE_USER_ARRAY_SIZE** macro is used to pass the size of the user-allocated buffer passed to the **UserVarPtr ** parameter of [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md). **M_FEATURE_USER_ARRAY_SIZE** is passed using the **UserVarType** parameter.
  The following is a list of example calls using **M_FEATURE_USER_ARRAY_SIZE**:
  - **MdigInquireFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("PixelFormat"), M_TYPE_STRING + M_FEATURE_USER_ARRAY_SIZE(N), TextCharArray);** N being equal to the number of **AIL_TEXT_CHAR** in the **TextCharArray**.
  - **MdigInquireFeature(Digitizer, M_FEATURE_VALUE, AIL_TEXT("LUTValueAll"), M_TYPE_UINT8 + M_FEATURE_USER_ARRAY_SIZE(N), Uint8Array); N being equal to the number of Uint8 in the Uint8Array**.
- **M_FEATURE_ENUM_ENTRY_DISPLAY_NAME** can now be used to inquire possible enumeration string entry to use for display purposes. See **M_FEATURE_ENUM_ENTRY_NAME**.
- Support for **M_FEATURE_CHANGE_HOOK**.
  Identifies the specified** FeatureName** to trigger the **M_FEATURE_CHANGE** hook callback. You must be hooked to the **M_FEATURE_CHANGE** hook type using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md).
- Support for **M_INCREMENT** when using **M_TYPE_AIL_INT64**.
  Returns an **AIL_INT64**, representing the feature's increment value.
- Support for **M_INCREMENT** when using **M_TYPE_AIL_INT32**.
  Returns an **AIL_INT32**, representing the feature's increment value.
- Support for **M_INCREMENT** when using **M_TYPE_DOUBLE**.
  Returns an **AIL_DOUBLE**, representing the feature's increment value.

## Msys

This section presents information about Zebra GigE Vision driver and the system module.

### Additions to MsysControl

The following lists additional GigE Vision support when using [`MsysControl`](../../Reference/sys/MsysControl.md).

- Support for **M_DISCOVER_DEVICE**.
  Performs a device discovery. **ControlValue** must be **M_DEFAULT**. Note: executing this control can trigger an **M_CAMERA_PRESENT** hook.
- Support for **M_GC_ACTION_DEVICE_KEY** + N.
  Adds an action device key to action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**.
- Support for **M_GC_ACTION_GROUP_KEY** + N.
  Adds an action group key to action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**.
- Support for **M_GC_ACTION_GROUP_MASK** + N.
  Adds an action group mask to action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**.
- Support for **M_GC_ACTION_ACKNOWLEDGE_NUMBER** + N.
  Lets the GigE Vision driver know how many action command acknowledge packets will be received for action context N. If not specified, the number of calls to **M_GC_ACTION_ADD_DEVICE** for this action context will be used as the number of acknowledge packets expected. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**.
- Support for **M_GC_ACTION_EXECUTE** + N.
  Broadcast a GigE Vision Action command packet or scheduled action command packet containing the device key, group key, and group mask associated to action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. Control value must be **M_DEFAULT**. Note that the device key, group key, and group mask must have previously been programmed in each GigE Vision device associated to this action context using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with "ActionDeviceKey", "ActionSelector", "ActionGroupKey", and "ActionGroupMask". See SFNC (Standard Feature Naming Convention) for details.
- Support for **M_GC_ACTION_ADD_DEVICE** + N.
  Associates a GigE Vision device with action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. The control value must be a valid Digitizer ID.
- Support for **M_GC_ACTION_REMOVE_DEVICE** + N.
  Removes a GigE Vision device from action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. The control value must be a valid Digitizer ID.
- Support for **M_GC_ACTION_CLEAR_DEVICES** + N.
  Removes all GigE Vision devices associated to action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. The control value must be **M_DEFAULT**.
- Support for **M_GC_ACTION_TIME** + N.
  Associates an action time to action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. This control must be used only if it is intended to send a scheduled action command. The time specified must be a time in the future. The GigE Vision devices associated to this action context must have IEEE 1588 PTP enabled using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with "GevIEEE1588", "GevIEEE1588ClockAccuracy", and "GevIEEE1588Status". See SFNC (Standard Feature Naming Convention) for details. The current time can be inquired with **MdigInquire(M_GC_CAMERA_TIME_STAMP)** or **MdigGetHookInfo(M_GC_CAMERA_TIME_STAMP)**. The current time is valid only if IEEE 1588 PTP is enabled on all devices when the inquire is made.

### Additions to MsysGetHookInfo

The following lists additional GigE Vision support when using [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md).

- Support for **M_CAMERA_PRESENT**.
  Returns an **AIL_INT**. Returns **M_TRUE** if a camera has been added or re-connected to the system. Returns **M_FALSE** if a camera has been removed from the system.
- Support for **M_NUMBER**.
  Returns an **AIL_INT**. Returns the system assigned digitizer device number associated with the camera that generated an **M_CAMERA_PRESENT** event.
- Support for **M_TIME_STAMP**.
  Returns an **AIL_DOUBLE**. Returns the operating system's time stamp of the event that generates a service request, in sec. The time stamp is generated by the operating system's performance counter.
- Support for **M_GC_USER_NAME_LENGTH**.
  Returns an **AIL_INT**. Returns the maximum number of characters in the camera's user-defined name string.
- Support for **M_GC_USER_NAME**.
  Returns an **AIL_INT**. Returns the user-defined name string for the camera.
- Support for **M_GC_IP_ADDRESS**.
  Returns an **AIL_INT64**. Returns the 32-bit internet protocol (IPv4) address of the GigE Vision device. Note that these addresses are returned in a 64-bit variable.
- Support for **M_GC_MAC_ADDRESS**.
  Returns an **AIL_INT**. Returns the system assigned digitizer device number associated with the camera that generated an **M_CAMERA_PRESENT** event.

### Additions to MsysHookFunction

The following lists additional GigE Vision support when using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md).

- Can now hook a callback to **M_CAMERA_PRESENT**. Note that **M_CAMERA_PRESENT** hook support requires the use of the AILGigEService process. If the service is not started, an Aurora Imaging Library error will be generated when calling [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with **M_CAMERA_PRESENT**.

### Additions to MsysInquire

The following lists additional GigE Vision support when using [`MsysInquire`](../../Reference/sys/MsysInquire.md).

- Support for **M_DISCOVER_DEVICE_COUNT**.
  Inquires the number of devices present. Inquire returns an **AIL_INT**.
- Support for **M_DISCOVER_DEVICE_DIGITIZER_NUMBER** + N.
  Inquires the digitizer device number associated to the nth device. N must be between 0 and **M_DISCOVER_DEVICE_COUNT** – 1. Inquire returns an **AIL_INT**.
- Support for **M_DISCOVER_DEVICE_MANUFACTURER_NAME** + N.
  Inquires the device's manufacturer name, for the nth device. N must be between 0 and **M_DISCOVER_DEVICE_COUNT** – 1. Inquire returns an **AIL_STRING**.
- Support for **M_DISCOVER_DEVICE_MODEL_NAME ** + N.
  Inquires the device's model name, for the nth device. N must be between 0 and **M_DISCOVER_DEVICE_COUNT** – 1. Inquire returns an **AIL_STRING**.
- Support for **M_DISCOVER_DEVICE_MANUFACTURER_INFO** + N.
  Inquires the device's manufacturer-specific information, for the nth device. N must be between 0 and **M_DISCOVER_DEVICE_COUNT** – 1. Inquire returns an **AIL_STRING**.
- Support for **M_DISCOVER_DEVICE_USER_NAME** + N.
  Inquires the device's user programmable device name. N must be between 0 and **M_DISCOVER_DEVICE_COUNT** – 1. Inquire returns an **AIL_STRING**.
- Support for **M_DISCOVER_DEVICE_VERSION** + N.
  Inquires the device version of the nth device. N must be between 0 and **M_DISCOVER_DEVICE_COUNT** – 1. Inquire returns an **AIL_STRING**.
- Support for **M_DISCOVER_DEVICE_SERIAL_NUMBER** + N.
  Inquires the device serial number of the nth device. N must be between 0 and **M_DISCOVER_DEVICE_COUNT** – 1. Inquire returns an **AIL_STRING**.
- Support for **M_DISCOVER_DEVICE_INTERFACE_NAME ** + N.
  Inquires the name of the host interface associated to the nth device. N must be between 0 and **M_DISCOVER_DEVICE_COUNT** – 1. Inquire returns an **AIL_STRING**.
- Support for **M_DISCOVER_DEVICE_ADDRESS ** + N.
  Inquires the address of the nth device. N must be between 0 and **M_DISCOVER_DEVICE_COUNT** – 1. Inquire returns an **AIL_STRING**.
- Support for **M_GC_ACTION_DEVICE_KEY** + N.
  Inquires the action device key of action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. Inquire returns an **AIL_INT**.
- Support for **M_GC_ACTION_GROUP_KEY** + N.
  Inquires the action group key of action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. Inquire returns an **AIL_INT**.
- Support for **M_GC_ACTION_GROUP_MASK** + N.
  Inquires the action group mask of action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. Inquire returns an **AIL_INT**.
- Support for **M_GC_ACTION_ACKNOWLEDGE_NUMBER** + N.
  Inquires how many action command acknowledge packets will be received for action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. Inquire returns an **AIL_INT**.
- Support for **M_GC_ACTION_ADD_DEVICE** + N.
  Inquires the GigE Vision device added to action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. Inquire returns an **AIL_ID**.
- Support for **M_GC_ACTION_REMOVE_DEVICE** + N.
  Inquires the GigE Vision device removed from action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. Inquire returns an **AIL_ID**.
- Support for **M_GC_ACTION_TIME** + N.
  Inquires the action time of action context N. N can range from **M_GC_ACTION0** to **M_GC_ACTION31**. Inquire returns an **AIL_DOUBLE**. The current time can be inquired with **MdigInquire(M_GC_CAMERA_TIME_STAMP)** or **MdigGetHookInfo(M_GC_CAMERA_TIME_STAMP)**. The current time is valid only if IEEE 1588 PTP is enabled on all devices when the inquire is made.
