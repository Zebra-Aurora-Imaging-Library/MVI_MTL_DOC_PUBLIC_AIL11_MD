---
doctype: Reference
module: dig
function: MdigControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dig / MdigControl"
---

# MdigControl

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Control a digitizer setting.

## Syntax

```c
void MdigControl(
    AIL_ID     DigId,        //out
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function allows you to control various digitizer settings.

> **Note:** Note that, when a control type has only one supported control value on a given system, it is not documented in this function because it cannot be changed to another value. Instead it can only be inquired.

To inquire the current value of a particular digitizer setting, use [`MdigInquire`](../../Reference/dig/MdigInquire.md).

If you change the setting of a control type of a digitizer currently being used in a grab, the change does not affect the current grab. It does, however, affect the very next grab.

You can also interactively control and test most of the digitizer settings and obtain results in real-time, using Feature Browser; to access this interface, use Aurora Imaging Intellicam or [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md). If your camera is connected to a CoaXPress (CXP) communication standard-compliant frame grabber (such as Zebra Rapixo CXP), you can save digitizer (and camera) settings selected in Intellicam's Feature Browser to the DCF for your camera.

Note that there are several control types that are available on a Host system, even without any additional Zebra imaging boards; these are to control a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). To see the control types available to control a simulated digitizer, select the Host system as your board type in this Help file. For more information on simulated digitizers, refer to [Simulated digitizer](../../UserGuide/C27_Grabbing_with_your_digitizer/S13_Simulated_Digitizer.md).

### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF | Note that, to change this default behavior and have the change affect the digitizer even if it is currently being used in a grab, use [`M_COMMAND_QUEUE_MODE`](../../Reference/dig/MdigControl.md). |
| GenTL, GigE Vision, USB3 Vision, V4L2 | Typically, the supported control types affect the GenICam-compliant camera (or device) associated with the digitizer. These control types assume your camera supports the associated GenICam standard feature naming convention (SFNC) feature. If the device does not support the associated GenICam SFNC feature, an error is generated. In case of error, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the device-specific feature names and values specified by the device manufacturer. |

## Parameters

### `DigId` *(out, AIL_ID)*

Specifies the identifier of the digitizer.

### `ControlType` *(in, AIL_INT64)*

Specifies the digitizer setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value to assign to the digitizer setting.

## Parameter Associations

### For the general digitizer settings

The following control types allow you to control various digitizer settings.

---

### `M_BAYER_COEFFICIENTS_ID`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Loads the internal white balance coefficients buffer with the values in the specified buffer. To have your digitizer automatically calculate these coefficients and load them into the internal buffer, use the [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) control type with [`M_CALCULATE`](../../Reference/dig/MdigControl.md). Note that the internal buffer only has an effect if the [`M_BAYER_CONVERSION`](../../Reference/dig/MdigControl.md) and [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) control types are enabled.  If you change the coefficient values in the specified buffer, you must call [`MdigControl`](../../Reference/dig/MdigControl.md) again with the updated buffer to load the new values into the internal white balance coefficients buffer.

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF | Refer to your Zebra imaging board's hardware manual for more details. |
| GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2 | Note that this control type is used to perform Bayer conversion if performed by the Host. To control the Bayer conversion on the camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md); refer to your camera's documentation for details regarding the features to set. |
| Iris GTX | Note that this control type is only available when using color versions of Zebra Iris GTX (such as 2000C). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the default coefficients that were automatically set, following a call to [`MdigAlloc`](../../Reference/dig/MdigAlloc.md).  > **Note:** Note this is the same as using [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) with [`M_DISABLE`](../../Reference/dig/MdigControl.md). **[GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2]** |
| `Array buffer identifier` | Specifies the identifier of an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer containing the white balance coefficients. The buffer must be allocated as a single-band 32-bit floating-point buffer with a 3x1 dimension. For more information, see [White balancing your Bayer images](../../UserGuide/C23_Data_buffers/S16_Using_buffers_with_the_Bayer_color_filter.md). |

---

### `M_BAYER_CONVERSION`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets whether to perform a Bayer color conversion, using your digitizer. This control type can only be used when grabbing from a camera that has a Bayer color filter, otherwise an error will be generated.  For a more conventional technique of controlling Bayer conversion, use [`MbufBayer`](../../Reference/buf/MbufBayer.md) and see [White balancing your Bayer images](../../UserGuide/C23_Data_buffers/S16_Using_buffers_with_the_Bayer_color_filter.md).

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF | Refer to your Zebra imaging board's hardware manual for more details. |
| GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2 | Note that this control type is used to perform Bayer conversion if performed by the Host. To control the Bayer conversion on the camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md); refer to your camera's documentation for details regarding the features to set. |
| Iris GTX | Note that this control type is only available when using color versions of Zebra Iris GTX (such as 2000C). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to not perform Bayer conversion on the grabbed image. |
| `M_ENABLE` *(default)* | Specifies to perform Bayer conversion on the grabbed image. Note that to white balance an image, [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) must also be set to [`M_ENABLE`](../../Reference/dig/MdigControl.md). |

---

### `M_CHANNEL`

**Board availability:** Clarity UHD

Sets the channel on which the digitizer is to acquire data, as well as sets the synchronization channel to the default for the selected data channel. Note that, typically, this control type is only available when the specified digitizer uses acquisition paths with several multiplexed data inputs. This means that they have several data input channels but can only grab from one channel at a time.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CH0` *(default)* | Specifies channel 0 as the channel on which the digitizer is to receive input data.  For Y/C input, this corresponds to `VID_IN2` (Green of camera 0) and `VID_IN3` (Blue of camera 0).  For CVBS input, this corresponds to `VID_IN2` (Green of camera 0). |
| `M_CH1` | Specifies channel 1 as the channel on which the digitizer is to receive input data.  For Y/C input, this corresponds to `VID_IN6` (Luminance of camera 1) and `VID_IN5` (Chrominance of camera 1).  For CVBS input, this corresponds to `VID_IN5` (Chrominance of camera 1). |
| `M_CH2` | Specifies channel 2 as the channel on which the digitizer is to receive input data.  For CVBS input, this corresponds to `VID_IN6` (Luminance of camera 2). |

---

### `M_COMMAND_QUEUE_MODE`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets whether changes to digitizer settings affect the digitizer immediately, even if it is currently being used in a grab, or the change will wait until the current grab is complete.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_IMMEDIATE` | Specifies that, if you change the setting of a control type of a digitizer currently being used in a grab, the change affects the current grab. Note that, this can cause the grabbed image to become corrupt. |
| `M_QUEUED` *(default)* | Specifies that, if you change the setting of a control type of a digitizer currently being used in a grab, the change does not affect the current grab. Instead, the changes are queued and applied before the very next grab. |

---

### `M_CORRUPTED_FRAME_ERROR`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2

Sets whether an error is generated when a corrupted or incomplete frame is grabbed.

#### System specific

| Board(s) | Note |
|---|---|
| GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2 | Note that whether the grabbed image is corrupt can always be inquired using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_CORRUPTED_FRAME`](../../Reference/dig/MdigGetHookInfo.md) from the user-defined function hooked to the end of a grab using [`MdigProcess`](../../Reference/dig/MdigProcess.md). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to generate an error when grabbing a corrupted frame. |
| `M_ENABLE` *(default)* | Specifies to generate an error when grabbing a corrupted frame. |

---

### `M_DIGITIZER_INTERNAL_BUFFERS_NUM`

**Board availability:** GenTL, GigE Vision, USB3 Vision, V4L2

Sets the number of internal grab buffers allocated and used when Aurora Imaging Library cannot grab directly into the specified buffers (for example, when the grab buffer's format is not compatible with the camera's current pixel format or when the image sent from a GenICam-compliant camera has additional information that needs to be stripped out to make the grabbed image compatible with Aurora Imaging Library buffers, such as when GenICam "chunk mode" is enabled).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of internal grab buffers. |

---

### `M_FIX_PATTERN_NOISE_CORRECTION`

**Board availability:** Iris GTX

Sets the type of fixed pattern noise correction to apply when acquiring images. Note that, acquisition must be stopped prior to changing this control type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PREPROCESSING` *(default)* | Specifies a correction for dark signal non-uniformity and for photo response non-uniformity, applied to the grabbed images. Aurora Imaging Library calculates the correction factor(s) when the control type is called. |
| `M_SENSOR` | Specifies a correction for dark signal non-uniformity, applied to the grabbed images. Aurora Imaging Library adjusts the correction factor for each image. |

---

### `M_FOCUS`

**Board availability:** Iris GTX

Sets the lens focus capabilities.  > **Note:** Note that this control type requires that your Zebra Iris GTX use an I<sup>2</sup>C (inter-integrated circuit) controlled lens.

| Value | Description |
| --- | --- |
| `Min. focus <= Value <= Max. focus` | Specifies a value between the minimum and maximum values supported by the camera. Use [`MdigInquire`](../../Reference/dig/MdigInquire.md) to determine these values. |

---

### `M_FOCUS_PERSISTENCE`

**Board availability:** Iris GTX

Sets whether to store a focus position for the I<sub>2</sub>C (inter-integrated circuit) controlled lens. This stored focus position is written to the I<sub>2</sub>C (inter-integrated circuit) controlled lens when the smart camera is allocated, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). If there is no I<sub>2</sub>C (inter-integrated circuit) controlled lens attached, the value is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not store the focus position. |
| `M_ENABLE` | Specifies to store the focus position. |

---

### `M_FOCUS_PERSISTENT_VALUE`

**Board availability:** Iris GTX

Sets the focus position to store for the I<sub>2</sub>C (inter-integrated circuit) controlled lens. If there is no I<sub>2</sub>C (inter-integrated circuit) controlled lens attached, the value is ignored.

| Value | Description |
| --- | --- |
| `0 <= Value <= 1023` | Specifies to store the focus position. |

---

### `M_GC_CLPROTOCOL`

**Board availability:** Radient eV-CL, Rapixo CL

Sets whether the GenICam CLProtocol module is enabled. The GenICam CLProtocol module allows Camera Link cameras to be accessed using GenICam-specific constants (including [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md)), as well as using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) and [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md).  Even when the GenICam CLProtocol module is enabled, you should follow the Aurora Imaging Library documentation specific to your board; if a control type requires the CLProtocol module to be enabled, it will be noted.  > **Note:** Note that this control type should be enabled only if using a GenCP-compliant camera or after the required third-party, vendor-supplied, standard-compliant CLProtocol library for the Camera Link camera is installed on your computer and [`M_GC_CLPROTOCOL_DEVICE_ID`](../../Reference/dig/MdigControl.md) is set.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the GenICam CLProtocol module is disabled. |
| `M_ENABLE` | Specifies that the GenICam CLProtocol module is enabled. |

---

### `M_GC_CLPROTOCOL_DEVICE_ID`

**Board availability:** Radient eV-CL, Rapixo CL

Identifies the Camera Link camera connected to your digitizer, as well as the GenICam CLProtocol library for the camera. When allocating a digitizer for a camera, the digitizer identifies that the connected device is a camera; [`M_GC_CLPROTOCOL_DEVICE_ID`](../../Reference/dig/MdigControl.md) provides the digitizer with the details required to identify a specific Camera Link camera and its corresponding GenICam CLProtocol library.  > **Note:** Note that this control type should be used only if using a GenCP-compliant camera or after the required third-party vendor-supplied, standard compliant, CLProtocol library for the Camera Link camera is installed on your computer.  To enumerate the complete list of camera identifiers installed on your computer, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_CLPROTOCOL_DEVICE_ID`](../../Reference/dig/MdigControl.md) + _n_. For an example, see [CLProtocol.cpp](../../Examples/CLProtocol.cpp.md).  Even after you have specified the library to use, you must enable the CLProtocol module (with [`M_GC_CLPROTOCOL`](../../Reference/dig/MdigControl.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md)) before you can use an Aurora Imaging Library GenICam constant (including [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md)), or use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md), or [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md).

| Value | Description |
| --- | --- |
| `"M_DEFAULT"` | Specifies to use the default identifier string of your Camera Link camera and its corresponding GenICam CLProtocol library, specified using the Aurora Imaging Configurator utility.  The specified string must contain sufficient data to uniquely identify the Camera Link camera and its GenICam CLProtocol library. |
| `"CameraLinkCameraIdentifier"` | Specifies the identifier string of your Camera Link camera and its corresponding GenICam CLProtocol library.  The specified string should contain the tokens separated by the hash ("#") sign instead of back slashes (for example, "LibraryDirectory#LibraryFileName#Manufacturer#Family#Model#Version). Each of these tokens must follow the naming convention for C variables.  The specified string must contain sufficient data to uniquely identify the Camera Link camera and its GenICam CLProtocol library. |

---

### `M_GC_FEATURE_BROWSER`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets whether to open or close a window that allows you to view and edit the GenICam-compliant camera configuration information interactively. This window is referred to as the Feature Browser.

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL | > **Note:** Note that, this control type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer. |
| GenTL | The Zebra GenTL Consumer (library) allows you to edit the GenICam-compliant stream, device, or remote device configuration information. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLOSE` | Closes Feature Browser. |
| `M_OPEN` *(default)* | Opens Feature Browser. |

---

### `M_GC_FEATURE_POLLING`

**Board availability:** GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets whether Feature Browser will periodically poll camera features that can automatically change on the camera, for updates (for example, the camera's temperature). The camera's device description file (XML) defines these feature as pollable. To learn whether a camera feature is pollable, refer to your camera's documentation.  If this control type is enabled, these features will be polled periodically and Feature Browser (if open either through Aurora Imaging Intellicam or launched using [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md)) will be updated automatically with this information. The polling period is determined by Aurora Imaging Library and throttled with respect to available bandwidth.  If this control type is disabled, Feature Browser will display the initial value of the pollable features and not update them until the user clicks upon their corresponding display field in Feature Browser. Note that [`MdigInquire`](../../Reference/dig/MdigInquire.md) and [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) always return information that comes from the camera at the time of the inquire, regardless of whether feature polling is enabled or not.  Note that this control type does not enable execution-polling completion of executable commands. To enable execution-polling completion, use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_FEATURE_EXECUTE_POLLING_MODE`](../../Reference/sys/MsysControl.md).

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL | > **Note:** Note that, this control type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that automatically changing camera features will not be polled. |
| `M_ENABLE` | Specifies that automatically changing camera features will be polled. |

---

### `M_GC_FRAME_MAX_RETRIES`

**Board availability:** GigE Vision

Sets the maximum number of times packets should be re-sent before flagging their associated frame as corrupt. Each packet can be re-sent the number of times specified using [`M_GC_PACKET_MAX_RETRIES`](../../Reference/dig/MdigControl.md). When the total number of packet re-sends is equal to [`M_GC_FRAME_MAX_RETRIES`](../../Reference/dig/MdigControl.md), the frame is flagged as corrupt (for example, if each packet can be re-sent 3 times and the frame's maximum retries is set to 30, a total of 30 packets associated with the frame can be re-sent before the frame is flagged as corrupt).  To establish the number of frames corrupted, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_TOTAL_FRAMES_MISSED`](../../Reference/dig/MdigInquire.md), or Aurora Imaging Capture Works.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= M_GC_PACKET_MAX_RETRIES` *(default)* | Specifies the maximum number of times to retry sending the packets of a frame.  Note that this value should always be greater than or equal to [`M_GC_PACKET_MAX_RETRIES`](../../Reference/dig/MdigControl.md). |

---

### `M_GC_FRAME_TIMEOUT`

**Board availability:** GigE Vision

Sets the maximum amount of time to wait for the remaining packets of a frame, after receiving the trailer packet. If the missing packets are not returned within this time period, the corresponding frame is flagged as corrupt. To establish the number of corrupt frames, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_TOTAL_FRAMES_MISSED`](../../Reference/dig/MdigInquire.md), or Aurora Imaging Capture Works.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum time to wait, in msec. |

---

### `M_GC_HEARTBEAT_STATE`

**Board availability:** GevIQ, GigE Vision

Sets whether the heartbeat mechanism is used to keep the GigE Vision-compliant camera active. The heartbeat mechanism is used to inform your camera that communication between your Aurora Imaging Library application and the camera is still open.  If your application experiences a fatal error while the heartbeat mechanism is disabled, you will be unable to reconnect to your camera until you manually restart it. You should therefore typically disable the heartbeat mechanism only when required for debugging your application. If you experience timeouts when your application is running under normal circumstances, you should extend the heartbeat timeout period using [`M_GC_HEARTBEAT_TIMEOUT`](../../Reference/dig/MdigControl.md) instead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the heartbeat mechanism is disabled. The camera always assumes communication will continue and remains active indefinitely. |
| `M_ENABLE` *(default)* | Specifies that the heartbeat mechanism is enabled. If, after the heartbeat timeout value expires, the camera assumes there will be no further communication and shuts down. |

---

### `M_GC_HEARTBEAT_TIMEOUT`

**Board availability:** GevIQ, GigE Vision

Sets the amount of time that your GigE Vision-compliant camera will wait after the last communication before shutting down.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the amount of time, in msec. |

---

### `M_GC_INTER_PACKET_DELAY`

**Board availability:** GevIQ, GigE Vision

Sets the delay between packets sent by your camera when transmitting a stream of image packets.  The unit used is a timestamp tick. If you know the inter-packet delay in sec, use the following calculation to convert from timestamp ticks to sec: `(1/_m_)*_n_`, where _m_ is the frequency of the timestamp ticks (to inquire this value, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_COUNTER_TICK_FREQUENCY`](../../Reference/dig/MdigInquire.md)), and _n_ is your inter-packet delay value.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the delay, in timestamp ticks. |

---

### `M_GC_LOCAL_MESSAGE_PORT`

**Board availability:** GevIQ, GigE Vision

Sets the UDP port number used for the message channel on the computer connected to your GigE Vision-compliant camera. By default, this port number is set by Aurora Imaging Library. This control type should be changed only if there is a multicast conflict. Note that, with a multicast conflict, the multicast IP address and/or the UDP port assignment can be in conflict. If the problem persists after changing the message port, you can try changing the default multicast IP address, using [`M_GC_MESSAGE_CHANNEL_MULTICAST_ADDRESS_STRING`](../../Reference/dig/MdigInquire.md).

| Value | Description |
| --- | --- |
| `0 <= Value <=65535` | Specifies the port number. |

---

### `M_GC_LOCAL_STREAM_PORT`

**Board availability:** GevIQ, GigE Vision

Sets the UDP port number used for the GVSP (GigE Vision Stream Protocol) channel on the computer connected to your GigE Vision-compliant camera. By default, this port number is set by Aurora Imaging Library within the reserved range of multicast port numbers. This control type should be changed only if there is a multicast conflict. Note that, with a multicast conflict, the multicast IP address and/or the UDP port assignment can be in conflict. If the problem persists after changing the stream port, you can try changing the default multicast IP address, using [`M_GC_STREAM_CHANNEL_MULTICAST_ADDRESS_STRING`](../../Reference/dig/MdigInquire.md).

| Value | Description |
| --- | --- |
| `0 <= Value <=65535` | Specifies the port number. |

---

### `M_GC_MAX_NBR_PACKETS_OUT_OF_ORDER`

**Board availability:** GigE Vision

Sets the maximum number of packets that can be received out-of-order, before the associated frame is marked as corrupt. This is useful when using link-aggregated cameras, where packets are typically received out-of-order. To establish the number of packets currently received out-of-order, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_MAX_NBR_PACKETS_OUT_OF_ORDER`](../../Reference/dig/MdigInquire.md), or Aurora Imaging Capture Works.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number of out-of-order packets to receive. |

---

### `M_GC_MESSAGE_CHANNEL_MULTICAST_ADDRESS_STRING`

**Board availability:** GigE Vision

Sets the IP address used for the multicast message channel of your GigE Vision-compliant camera. This IP address is set by Aurora Imaging Library within the reserved range of multicast IP addresses (239.0.0.0 to 239.255.255.255). This control type should be changed only if there is a multicast conflict. Note that, with a multicast conflict, the multicast IP address and/or the UDP port assignment can be in conflict. If the problem persists after changing the multicast IP address, you can try changing the default UDP port assignment, using [`M_GC_LOCAL_MESSAGE_PORT`](../../Reference/dig/MdigInquire.md).

| Value | Description |
| --- | --- |
| `"239.n.n.n"` | Specifies the multicast IP address of the camera, in dotted decimal notation where each dotted decimal value (_n_) is a number between 0 and 255. |

---

### `M_GC_PACKET_MAX_RETRIES`

**Board availability:** GigE Vision

Sets the maximum number of times each packet can be re-sent. If the maximum number of times a packet can be re-sent is reached, the packet is flagged as corrupted.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= M_GC_FRAME_MAX_RETRIES` *(default)* | Specifies the maximum number of times to resend a given packet of a frame.  Note that this value should always be less than [`M_GC_FRAME_MAX_RETRIES`](../../Reference/dig/MdigInquire.md). |

---

### `M_GC_PACKET_RESEND`

**Board availability:** GigE Vision

Sets whether to request packets be re-sent from your GigE Vision-compliant camera, if the packets are not received properly (for example, when the packets are received out-of-order, or a packet timeout occurs). The request is issued up to a maximum number of times for each packet, set using [`M_GC_PACKET_MAX_RETRIES`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that packets should never be re-sent. |
| `M_ENABLE` *(default)* | Specifies that packets should be re-sent as required. |

---

### `M_GC_PACKET_SIZE`

**Board availability:** GevIQ, GigE Vision

Sets the packet size used by the GigE Vision-compliant camera when streaming data to the Host. By default, Zebra's GigE Vision driver negotiates the largest possible packet size with the GigE Vision-compliant camera. This control type should only be changed if the negotiated size is causing errors in acquisition (such as image corruption).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the packet size, in bytes. Typical packet sizes range from 512 bytes to 1.5 Kbytes. Jumbo packets, if enabled on your network and compatible with both your GigE Vision device and your firewall, are up to 9 Kbytes in size. |

---

### `M_GC_PACKET_TIMEOUT`

**Board availability:** GigE Vision

Sets the maximum amount of time to wait before flagging a packet as dropped.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum time to wait, in msec. |

---

### `M_GC_PIXEL_FORMAT`

**Board availability:** GenTL, GevIQ, GigE Vision, V4L2

Sets the pixel format that the digitizer should use to create internal buffers to receive images from the camera, if the digitizer is a multicast monitor digitizer. The pixel format cannot be inquired from the GigE Vision-compliant camera. It must therefore be set using this control type or a DCF. To set this value to the same pixel format as received image data, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_SOURCE_DATA_FORMAT`](../../Reference/dig/MdigInquire.md); in this case, you must first grab into an arbitrary image buffer and then issue the inquire.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as specified in the DCF. |
| `Value` | Specifies the data depth and raw color format to use to create the internal buffers. The GenICam predefined list of pixel formats are in the _PFNC.h_ header file in the installed aildyn folder (for example, _C:\Program Files\Aurora Imaging Library\11\include\classic\aildyn_). |

---

### `M_GC_PIXEL_FORMAT_SWITCHING`

**Board availability:** GenTL, GigE Vision, USB3 Vision, V4L2

Sets whether to allow the camera's pixel format to change automatically to match the current grab buffer, when supported by the GenICam-compliant camera.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable automatic pixel format switching. |
| `M_ENABLE` *(default)* | Specifies to enable automatic pixel format switching. |

---

### `M_GC_STATISTICS_RESET`

**Board availability:** GevIQ, GigE Vision, USB3 Vision

Resets the acquisition statistics. For a list of GenICam-compliant statistics available in Aurora Imaging Library, refer to [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_TOTAL_...`](../../Reference/dig/MdigInquire.md), or Aurora Imaging Capture Works.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_GC_STREAM_CHANNEL_MULTICAST_ADDRESS_STRING`

**Board availability:** GigE Vision

Sets the IP address used for the multicast stream channel of your GigE Vision-compliant camera. By default, this address is set by Aurora Imaging Library within a limited range of multicast addresses (239.0.0.0 to 239.255.255.255). This control type should be changed only if there is a multicast conflict. Note that, with a multicast conflict, the multicast IP address and/or the UDP port assignment can be in conflict. If the problem persists after changing the multicast IP address, you can try changing the default UDP port assignment, using [`M_GC_LOCAL_STREAM_PORT`](../../Reference/dig/MdigInquire.md).

| Value | Description |
| --- | --- |
| `"2nn.n.n.n"` | Specifies the multicast IP address of the camera, in dotted decimal notation where each dotted decimal value (_n_) is a number between 0 and 255.  While Aurora Imaging Library assigns an address within a limited range of multicast IP addresses (239.0.0.0 to 239.255.255.255), the full range is available (224.0.0.0 to 239.255.255.255). You can determine the size of this string by calling [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_STREAM_CHANNEL_MULTICAST_ADDRESS_STRING`](../../Reference/dig/MdigInquire.md)+[`M_STRING_SIZE`](../../Reference/dig/MdigInquire.md). |

---

### `M_GC_STREAMING_MODE`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets the camera's image stream activation mechanism. Typically, this control type is used when performing a monoshot grab in a loop. Since the camera cannot tell when the next grab might arrive, you can either manually control the image stream (starting it before the first grab and stopping it after the last one), or allow Aurora Imaging Library to handle it automatically (using [`M_GC_STREAMING_STOP_CHECK_PERIOD`](../../Reference/dig/MdigControl.md) and [`M_GC_STREAMING_STOP_DELAY`](../../Reference/dig/MdigControl.md) to control the delay between the grab and when the image stream is stopped). Reducing the time during which images are streamed to the Host can reduce bandwidth requirements.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTOMATIC` *(default)* | Specifies that the image stream is started and stopped automatically.  Use [`M_GC_STREAMING_STOP_CHECK_PERIOD`](../../Reference/dig/MdigControl.md) and [`M_GC_STREAMING_STOP_DELAY`](../../Reference/dig/MdigControl.md) to control the delay between the grab and when the image stream is stopped. |
| `M_MANUAL` | Specifies that the image stream is started and stopped manually. Use [`M_GC_STREAMING_START`](../../Reference/dig/MdigControl.md) and [`M_GC_STREAMING_STOP`](../../Reference/dig/MdigControl.md). |

---

### `M_GC_STREAMING_START`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Starts the camera's image stream. Note that this control type is only available when [`M_GC_STREAMING_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_MANUAL`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_GC_STREAMING_STOP`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Stops the camera's image stream. Note that this control type is only available when [`M_GC_STREAMING_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_MANUAL`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_GC_STREAMING_STOP_CHECK_PERIOD`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets the maximum amount of time to wait before Aurora Imaging Library checks to see whether a grab is pending. If there is no grab pending after this delay, a second check is made after a second delay (set using [`M_GC_STREAMING_STOP_DELAY`](../../Reference/dig/MdigControl.md)). If there is still no grab pending, then Aurora Imaging Library stops the digitizer from streaming images.  Note that this control type is only available when [`M_GC_STREAMING_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_AUTOMATIC`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the amount of time, in msec. |

---

### `M_GC_STREAMING_STOP_DELAY`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets the amount of time to wait before stopping the image stream, if no grab is pending. Note that this delay occurs after the first check for pending grabs (set using [`M_GC_STREAMING_STOP_CHECK_PERIOD`](../../Reference/dig/MdigControl.md)).  Note that this control type is only available when [`M_GC_STREAMING_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_AUTOMATIC`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the amount of time, in msec. |

---

### `M_GC_UPDATE_MULTICAST_INFO`

**Board availability:** GigE Vision

Updates the multicast information on the controller, worker, or monitor digitizer. The multicast information includes the IP address and port number used for the multicast stream and message channels of your GigE Vision-compliant camera.  When used with the multicast controller digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_MULTICAST_CONTROLLER`](../../Reference/dig/MdigAlloc.md)), this control type updates the controller digitizer's multicast information after you manually change it on the camera (using [`M_GC_STREAM_CHANNEL_MULTICAST_ADDRESS_STRING`](../../Reference/dig/MdigControl.md), [`M_GC_LOCAL_STREAM_PORT`](../../Reference/dig/MdigControl.md), [`M_GC_MESSAGE_CHANNEL_MULTICAST_ADDRESS_STRING`](../../Reference/dig/MdigControl.md), and/or [`M_GC_LOCAL_MESSAGE_PORT`](../../Reference/dig/MdigControl.md)).  When used with a multicast worker digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_MULTICAST_WORKER`](../../Reference/dig/MdigAlloc.md)), this control type is used to update the local copy of the worker digitizer's multicast information after the multicast controller digitizer disconnects with the camera and reconnects using a different multicast IP address and/or port number.  When used with a multicast monitor digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_MULTICAST_MONITOR`](../../Reference/dig/MdigAlloc.md)), this control type is used to update the monitor digitizer's multicast information after you change the multicast stream channel address and port address manually for the digitizer.  To verify that the multicast controller is connected, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_MULTICAST_CONTROLLER_CONNECTED`](../../Reference/dig/MdigInquire.md).  To detect when the image stream stops, check the number of images grabbed with [`MdigProcess`](../../Reference/dig/MdigProcess.md), using [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_PROCESS_FRAME_COUNT`](../../Reference/dig/MdigInquire.md) before and after a wait period.  For more information on multicast IP addresses, refer to Using IP multicast.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_GC_USER_NAME`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision

Sets the camera's name.

| Value | Description |
| --- | --- |
| `"CameraName"` | Specifies the name of the camera. Note that this string can be 16 characters long (including the null-terminating character) and must use the UTF-8 character set. |

---

### `M_GENTL_ANNOUNCE_BUFFER`

**Board availability:** GenTL

Announces the specified buffer to the GenTL producer, before starting the acquisition with [`MdigGrab`](../../Reference/dig/MdigGrab.md). Note that it is not necessary to announce buffers when using [`MdigGrab`](../../Reference/dig/MdigGrab.md),[`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md). If you manually announce the buffer with [`M_GENTL_ANNOUNCE_BUFFER`](../../Reference/dig/MdigControl.md), you need to revoke the buffer after acquisition has stopped with [`M_GENTL_REVOKE_BUFFER`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `Grab image buffer identifier` | Specifies the identifier of a destination image buffer with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute. |

---

### `M_GENTL_REVOKE_BUFFER`

**Board availability:** GenTL

Revokes a previously announced grab image buffer from the GenTL producer. Note that acquisition must be stopped before a buffer can be revoked.

| Value | Description |
| --- | --- |
| `Grab image buffer identifier` | Specifies the identifier of a destination image buffer with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute. |

---

### `M_GRAB_ABORT`

Immediately stops a grab in progress and aborts queued grabs. This is useful for canceling a triggered grab that is waiting for a trigger.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_GRAB_DIRECTION_X`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets the horizontal grab direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FORWARD` *(default)* | Specifies to grab from left to right, in the horizontal direction. |
| `M_REVERSE` | Specifies to grab from right to left, in the horizontal direction. Effectively, this flips the grabbed image horizontally. |

---

### `M_GRAB_DIRECTION_Y`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets the vertical grab direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FORWARD` *(default)* | Specifies to grab from top to bottom, in the vertical direction. |
| `M_REVERSE` | Specifies to grab from bottom to top, in the vertical direction. Effectively, this flips the grabbed image vertically. |

---

### `M_GRAB_FIELD_NUM`

**Board availability:** Clarity UHD

Sets the number of fields to grab when grabbing data with [`MdigGrab`](../../Reference/dig/MdigGrab.md).  This control type should only be used with interlaced cameras. When using progressive cameras, set this control type to 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  For interlaced cameras, the default is 2. For progressive cameras, the default is 1. |
| `1` | Specifies to grab one field. The grabbed field is written to sequential rows of the grab buffer. When set to 1, each field is treated like a frame and the following digitizer events occur relative to the grabbed field: [`M_GRAB_FRAME_START`](../../Reference/dig/MdigHookFunction.md), [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md), and [`M_GRAB_FRAME_END`](../../Reference/dig/MdigHookFunction.md).  To achieve 60 fps in NTSC or 50 fps in PAL, [`M_GRAB_START_MODE`](../../Reference/dig/MdigControl.md) must be set to [`M_FIELD_START`](../../Reference/dig/MdigControl.md). |
| `2` | Specifies to grab two fields. |

---

### `M_GRAB_FRAME_MISSED_COUNTER`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets whether to count the number of frames sent by the camera, but not received by the digitizer when performing a grab operation (that is, [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md)).  To inquire the current value of this counter, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GRAB_FRAME_MISSED`](../../Reference/dig/MdigInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable frames missed detection. |
| `M_ENABLE` | Specifies to enable frames missed detection. |

---

### `M_GRAB_FRAME_MISSED_RESET`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Resets the counter for the number of frames sent by the camera but not received by the digitizer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_GRAB_LINE_COUNTER`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets whether a function hooked to an [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md), [`M_ROTARY_ENCODER`](../../Reference/dig/MdigHookFunction.md), or [`M_GRAB_FRAME_END`](../../Reference/dig/MdigHookFunction.md) event can inquire the number of lines grabbed, performed using [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md); the function could have been hooked using either [`MdigProcess`](../../Reference/dig/MdigProcess.md) or [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md). To inquire the number of lines grabbed at any other time, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_GRAB_LINE_COUNT`](../../Reference/dig/MdigGetHookInfo.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the hooked function cannot inquire the number of lines grabbed. |
| `M_ENABLE` | Specifies that the hooked function can inquire the number of lines grabbed. |

---

### `M_GRAB_LUT_PALETTE`

**Board availability:** Iris GTX

Sets which LUT to use.  Note that this control value is only supported on color cameras.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use a 3-band RGB LUT. |
| `M_GAMMA` | Specifies to use the native Bayer LUT that is applied to the Bayer image before it is converted to an RGB image. |

---

### `M_GRAB_MODE`

Sets how the grab should be synchronized with the Host when grabbing data with [`MdigGrab`](../../Reference/dig/MdigGrab.md). Note that this does not affect [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), which always runs asynchronously; otherwise, [`MdigHalt`](../../Reference/dig/MdigHalt.md) could not stop the grab. In addition, this control type does not affect [`MdigProcess`](../../Reference/dig/MdigProcess.md).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ASYNCHRONOUS` | Specifies that your application continues after one grab is queued, rather than waiting for the grab to finish. Note that only one [`MdigGrab`](../../Reference/dig/MdigGrab.md) function can be queued at a time. If one [`MdigGrab`](../../Reference/dig/MdigGrab.md) is already queued, subsequent grabs will cause your application to wait until the current grab is finished. This allows the currently queued grab to start. |
| `M_ASYNCHRONOUS_QUEUED` | Specifies that your application continues after each grab is queued, rather than waiting for the grab to finish. Note that more than one [`MdigGrab`](../../Reference/dig/MdigGrab.md) function can be queued. This causes the grabs to be truly asynchronous. |
| `M_SYNCHRONOUS` *(default)* | Specifies that your application is synchronized with the end of a grab operation (that is, your application waits for the grab to finish before returning from the grab function). |

---

### `M_GRAB_SCALE`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets the vertical and horizontal scaling factor when grabbing data with [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md).  If the scale entered is not supported, the closest scaling factor will be used instead. If, however, the scale entered is larger than the maximum value supported, an error is generated.

#### System specific

| Board(s) | Note |
|---|---|
| GigE Vision, USB3 Vision | The camera must support the ability to scale grabbed data for this control type to be available on the camera. This control type is also available on the board. |
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |
| Iris GTX | Note that this control type will subsample 2 adjacent pixels and 2 adjacent lines. |

| Value | Description |
| --- | --- |
| `M_FILL_DESTINATION` | Specifies that the scaling factor is calculated and set to fill the destination buffer. **[Clarity UHD, GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `1` | Specifies that no scaling is applied. **[GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |
| `Value = 1/n` | Specifies to reduce the image size. To reduce the image size, the specified value must always be less than 1.0. |

---

### `M_GRAB_SCALE_INTERPOLATION_MODE`

**Board availability:** Host System

Sets the interpolation mode used when performing a vertical and/or horizontal scaling.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTOMATIC` | Specifies that the best interpolation mode is automatically selected, based on the input filter (set using [`M_INPUT_FILTER`](../../Reference/dig/MdigControl.md)). |
| `M_BICUBIC` | Specifies bicubic interpolation. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated according to the depth and data type of the destination buffer. This interpolation mode is software-based. |
| `M_BILINEAR` | Specifies bilinear interpolation. This interpolation mode is software-based. |
| `M_NEAREST_NEIGHBOR` *(default)* | Specifies nearest neighbor interpolation. By default, nearest neighbor interpolation is software-based. |

---

### `M_GRAB_SCALE_X`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets the horizontal scaling factor when grabbing data.  If the scale entered is not supported, the closest scaling factor will be used instead. If, however, the scale entered is larger than the maximum value supported, an error is generated.

#### System specific

| Board(s) | Note |
|---|---|
| GigE Vision, USB3 Vision | The camera must support the ability to scale grabbed data for this control type to be available on the camera. This control type is also available on the board. |
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| *(see [`M_GRAB_SCALE`](Reference/dig/MdigControl.md))* |  |

---

### `M_GRAB_SCALE_Y`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets the vertical scaling factor when grabbing data.  If the scale entered is not supported, the closest scaling factor will be used instead. If, however, the scale entered is larger than the maximum value supported, an error is generated.

#### System specific

| Board(s) | Note |
|---|---|
| GigE Vision, USB3 Vision | The camera must support the ability to scale grabbed data for this control type to be available on the camera. This control type is also available on the board. |
| Iris GTX | When down-scaling the image width, a smaller image will result but its scan rate will be proportionally higher than if the image is not scaled. |
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| *(see [`M_GRAB_SCALE`](Reference/dig/MdigControl.md))* |  |
| `Value = 1/n` | See the description of this setting in [`M_GRAB_SCALE`](../../Reference/dig/MdigControl.md). |

---

### `M_GRAB_START_MODE`

**Board availability:** Clarity UHD, Radient eV-CL, Rapixo CL

Sets the type of field on which to grab. Note that this value applies only when dealing with interlaced cameras.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIELD_START` | Specifies that the grab starts on any field. **[Radient eV-CL, Rapixo CL]** |
| `M_FIELD_START_EVEN` | Specifies that the grab starts on an even field. |
| `M_FIELD_START_ODD` *(default)* | Specifies that the grab starts on an odd field. |

---

### `M_GRAB_TIMEOUT`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets the maximum time to wait for a frame before generating an error.  Note that when used with [`MdigProcess`](../../Reference/dig/MdigProcess.md), [`M_GRAB_TIMEOUT`](../../Reference/dig/MdigControl.md) sets the time to wait before checking whether a grab occurred. If no grab occurred within this time period, a second time period of the same duration might elapse before an error is generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default amount of time to wait. Note that this is the same as [`M_INFINITE`](../../Reference/dig/MdigControl.md) when performing a triggered grab. |
| `M_INFINITE` | Specifies to wait indefinitely. This is recommended only for triggered grabs. |
| `Value > 0` | Specifies the time to wait, in msec. |

---

### `M_HARDWARE_DEINTERLACING`

**Board availability:** Clarity UHD

Sets whether hardware deinterlacing is used when the video source is interlaced.  Note that this control type is only available when your DCF uses an interlaced video signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BOB_METHOD` | Specifies to use the BOB algorithm for hardware deinterlacing. |
| `M_DISABLE` *(default)* | Specifies that hardware deinterlacing is disabled. |
| `M_MADI_METHOD` | Specifies to use the MADI (motion adaptive deinterlacing) algorithm for hardware deinterlacing. |

---

### `M_INPUT_FILTER`

**Board availability:** Clarity UHD

Sets the low-pass filter applied to incoming data.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTOMATIC` | Specifies that the best hardware filter is automatically selected or the filter is bypassed when the video input is interlaced and hardware deinterlacing is disabled. |
| `M_BYPASS` *(default)* | Specifies to not use a filter. |
| `M_LOW_PASS_0` | Specifies to use the first low-pass filter.  Uses a Kaiser filter. |
| `M_LOW_PASS_1` | Specifies to use the second low-pass filter.  Uses a Gaussian filter. |

---

### `M_LAST_GRAB_IN_TRUE_BUFFER`

Sets whether a monoshot grab is performed when [`MdigHalt`](../../Reference/dig/MdigHalt.md) is called after performing a continuous grab operation. This control type only has an effect if the target image buffer of the continuous grab is selected on a display. When the target buffer is selected on the display, the continuous grab will typically grab into a temporary internal buffer and only update the target image buffer when [`MdigHalt`](../../Reference/dig/MdigHalt.md) is called. This control type establishes how to update to the target image buffer.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the last frame grabbed during the continuous grab operation is copied to the target image buffer, instead of performing one last grab. This setting might reduce the time it takes for the continuous grab to terminate once [`MdigHalt`](../../Reference/dig/MdigHalt.md) is called (since copying a frame tends to take less time than grabbing a frame). |
| `M_ENABLE` *(default)* | Specifies that a monoshot grab is performed. Note that after performing a monoshot grab in the target image buffer selected to the display, the display is updated. |

---

### `M_LIGHTING_BRIGHT_FIELD`

**Board availability:** Iris GTX

Sets the intensity of light to emit parallel to the optical axis, making the flat parts of an object stand out in stark contrast in the image.  Increasing the value of this control type increases the intensity of the light transmitted by your lighting controller. Note that this control type is only for controlling the intensity of an LED (as a signal passed to a lighting controller). To control the timing of when the strobe light goes on or off, use [`M_GRAB_TRIGGER...`](../../Reference/dig/MdigControl.md) + [`M_TIMER_STROBE`](../../Reference/dig/MdigControl.md) or [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) with [`M_TIMER_STROBE`](../../Reference/dig/MdigControl.md). For more information, refer to your board's Aurora Imaging Library Hardware-specific Notes.  Note that some consecutive bright field intensity level settings might produce the same result due to the fact that there are only 250 distinct adjustments (adjustments of 0.4% each).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 255` *(default)* | Specifies the intensity of light to be emitted on the flat parts of the object. |

---

### `M_LUT_ID`

**Board availability:** Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets whether to map the input data through the physical LUT of the specified digitizer and the values with which to initialize the LUT. Aurora Imaging Library uses the data format (DCF) of the digitizer to determine whether a physical LUT is supported. If it is not, and you try to initialize it, an error is generated.  To learn how the digitizer's physical LUT is configured, refer to [Mapping grabbed data through a LUT](../../UserGuide/C27_Grabbing_with_your_digitizer/S10_Reference_levels_lookup_tables_and_scaling.md).  If the destination grab buffer's depth is larger than that of the digitizer's physical LUT, the bits from the physical LUT are copied over and the remaining upper bits in the destination grab buffer are set to 0. If the digitizer's physical LUT depth is greater than that of the destination host grab buffer, the most-significant bits of the data (the non-zero values) are used when the data is grabbed.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | Note that if 14-, or 16-bit data is grabbed, the digitizer's physical LUT is not used. |
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the digitizer's physical LUT is not used. |
| `LUT buffer identifier` | Specifies the identifier of the LUT buffer (allocated with [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_LUT`](../../Reference/buf/MbufAlloc1d.md)) with which to initialize the digitizer's physical LUT.  To copy the data from a LUT buffer to the digitizer's physical LUT, the number of entries in the LUT buffer must match those of the digitizer's physical LUT. Note that if the digitizer's physical LUT cannot support the depth of the specified LUT buffer, an error will occur.  LUT buffer data is loaded into the digitizer's physical LUT as follows. In the case where a 1-band LUT buffer is loaded into a 3-band digitizer's physical LUT, the LUT buffer is copied into each component of the digitizer's physical LUT. When a 3-band LUT buffer is loaded into a 1-band digitizer's physical LUT, only the first band of the LUT buffer is copied into the digitizer's physical LUT. In all other cases, each band of the LUT buffer is copied into the associated band of the digitizer's physical LUT. |

---

### `M_POWER_OVER_CABLE`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP

Sets whether to enable PoCL (power over Camera Link) and PoCXP (power over CoaXPress).

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL | Note that the camera must be PoCL-capable. Once enabled, your PoCL-capable camera receives power over its Camera Link connection. |
| Rapixo CXP | Note that the camera must be PoCXP-capable. Once enabled, your PoCXP-capable camera receives power over its CoaXPress connection. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OFF` | Specifies to disable PoCL/PoCXP. |
| `M_ON` *(default)* | Specifies to enable PoCL/PoCXP. |

---

### `M_PROCESS_GRAB_MONITOR`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets whether to create an internal grab monitoring thread when using [`MdigProcess`](../../Reference/dig/MdigProcess.md) with either [`M_START`](../../Reference/dig/MdigProcess.md) or [`M_SEQUENCE`](../../Reference/dig/MdigProcess.md). The internal grab monitoring thread will produce an error if there is no longer any grab activity (for example, when a camera is unplugged) after [`M_PROCESS_TIMEOUT`](../../Reference/dig/MdigControl.md) expires.  A typical use of this control type would be to disable the grab monitoring thread before a camera is unplugged, so that an error is not generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to create a grab monitoring thread. |
| `M_ENABLE` *(default)* | Specifies to create a grab monitoring thread. |

---

### `M_PROCESS_TIMEOUT`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets the maximum amount of time to wait for [`MdigProcess`](../../Reference/dig/MdigProcess.md) to complete the current grab ([`M_STOP`](../../Reference/dig/MdigProcess.md)) or all the queued grabs ([`M_STOP`](../../Reference/dig/MdigProcess.md) + [`M_WAIT`](../../Reference/dig/MdigProcess.md)), before generating an error. In an ideal situation, [`MdigProcess`](../../Reference/dig/MdigProcess.md) would complete all the queued grabs before this timeout expires. If the timeout does expire either the specified time was too short, or a grab error occurred.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies to wait indefinitely. |
| `Value >= 0` | Specifies the time to wait, in msec. |

---

### `M_SOURCE_OFFSET_X`

Sets the X-offset of the input signal capture window.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |
| Iris GTX | Note that this value must be multiple of 32. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset, in pixels. |

---

### `M_SOURCE_OFFSET_Y`

Sets the Y-offset of the input signal capture window.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |
| Iris GTX | Note that this value must be multiple of 2. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset, in pixels. |

---

### `M_SOURCE_SIZE_X`

Sets the width of the input signal capture window.

#### System specific

| Board(s) | Note |
|---|---|
| GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2 | On some cameras, you might not be able to set the width (that is, this information might be read-only). |
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |
| Iris GTX | Note that this value must be multiple of 32. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the width, in pixels. |

---

### `M_SOURCE_SIZE_Y`

Sets the height of the input signal capture window.

#### System specific

| Board(s) | Note |
|---|---|
| GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2 | On some cameras, you might not be able to set the height (that is, this information might be read-only). |
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |
| Iris GTX | Note that this value must be multiple of 2. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the height, in pixels. |

---

### `M_TL_TRIGGER_ACTIVATION`

**Board availability:** Rapixo CXP, Rapixo CoF

Sets the signal variation upon which to generate a trigger signal to the camera through the transport layer interface.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the camera's default value or, if supported, to enable the camera's auto control state for this feature(that is, the camera adjusts this value automatically). |
| `M_ANY_EDGE` | Specifies that a trigger will be generated both upon a high-to-low and a low-to-high signal transition. |
| `M_EDGE_RISING` | Specifies that a trigger will be generated upon a low-to-high signal transition. |

---

### `M_WHITE_BALANCE`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets whether to perform white balancing.  The white balance coefficients used for white balancing are taken from an internal buffer, which is created and updated when you specify coefficients (using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_BAYER_COEFFICIENTS_ID`](../../Reference/dig/MdigControl.md)) or when you let the driver calculate them (using this control type with [`M_CALCULATE`](../../Reference/dig/MdigControl.md)).  Note that to use this control type, [`M_BAYER_CONVERSION`](../../Reference/dig/MdigControl.md) must be enabled.

#### System specific

| Board(s) | Note |
|---|---|
| GenTL, GigE Vision, USB3 Vision, V4L2 | Note that this control type is used to perform Bayer conversion if performed by the Host. To control the Bayer conversion on the camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md); refer to your camera's documentation for details regarding the features to set. |
| Iris GTX | Note that this control type is only available when using color versions of Zebra Iris GTX (such as 2000C). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | **[Rapixo CXP, Rapixo CoF]** |
| `M_CALCULATE` | Calculates new white balance coefficients, overwriting old coefficients.  Calling [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_CALCULATE`](../../Reference/dig/MdigControl.md) grabs an image. The image is assumed to be a uniform shade of gray that is not saturated. It is used to calculate the new white balance coefficients.  Use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_BAYER_COEFFICIENTS_ID`](../../Reference/dig/MdigInquire.md) to determine the calculated values. |
| `M_DISABLE` *(default)* | Specifies that white balancing is disabled. |
| `M_ENABLE` | Specifies that white balancing is enabled.  If you have not called [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_BAYER_COEFFICIENTS_ID`](../../Reference/dig/MdigControl.md) or with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_CALCULATE`](../../Reference/dig/MdigControl.md) before enabling white balancing, it will automatically calculate the white balance coefficients and then use those coefficients for subsequent images. Note that if [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) is disabled and then re-enabled, the previously calculated coefficients are used. |

### Combination Constants — For specifying the configuration file associated with the feature

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify which GenTL configuration file (XML file) is associated with the feature.

> **Board availability:** GenTL

| Value | Description |
| --- | --- |
| `M_GENTL_STREAM_NUMBER` | Specifies the feature is associated with an instance of the GenTL stream configuration information. |
| `M_GENTL_DEVICE` | Specifies that the feature is associated with the GenTL device configuration information. |
| `M_GENTL_REMOTE_DEVICE` *(default)* | Specifies that the feature is associated with the GenTL remote device configuration information. |

### Combination Constants — For specifying to wait for the camera to perform its focusing operation.

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify to wait for the camera to focus.

> **Board availability:** Iris GTX

| Value | Description |
| --- | --- |
| `M_WAIT` | Waits for the amount of time it takes the camera's lens motor to move the lens of the camera to a position that produces optimum focus. When used, this combination constant delays the callback function (hook-handler function) of [`MdigFocus`](../../Reference/dig/MdigFocus.md) a predetermined amount of time. |

### Combination Constants — For specifying whether Feature Browser should be synchronous or asynchronous

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set whether the Feature Browser should be synchronous or asynchronous.

Specifying whether Feature Browser should be synchronous or asynchronous.

> **Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

| Value | Description |
| --- | --- |
| `M_ASYNCHRONOUS` | Specifies that this function returns immediately once a Feature Browser window opens. |
| `M_SYNCHRONOUS` *(default)* | Specifies that this function is blocked until a Feature Browser window closes. |

### For the general reference settings

The following control types and control values set (if available) the reference levels used to digitize the analog signal received from a camera. These control types are specific to analog input devices. Depending on the type of digitizer and input signal, some reference types are not applicable. The smallest voltage increment supported by your board can differ such that consecutive reference-level settings might produce the same result. Note, some digitizers might take a few milliseconds before the reference level stabilizes.  To inquire the maximum and minimum levels for your camera, set the reference level to [`M_MIN_LEVEL`](../../Reference/dig/MdigControl.md) or [`M_MAX_LEVEL`](../../Reference/dig/MdigControl.md), and then inquire the reference level (for example, set [`M_BRIGHTNESS_REF`](../../Reference/dig/MdigControl.md) to [`M_MIN_LEVEL`](../../Reference/dig/MdigControl.md), and then use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_BRIGHTNESS_REF`](../../Reference/dig/MdigInquire.md) to return the minimum level for the black reference level).

---

### `M_BLACK_OFFSET`

**Board availability:** Iris GTX

Sets the offset to the black level of the image in hardware before the image reaches the digitization module of the sensor.

| Value | Description |
| --- | --- |
| `0<= Value <= 255` | Specifies the value of the black offset. |

---

### `M_BLACK_REF`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision

Sets the input signal's digitization black reference level. Note that when setting black and white reference levels, always set black reference levels first.

| Value | Description |
| --- | --- |
| `M_AUTOMATIC` | Sets the reference level automatically. **[GenTL, GevIQ, GigE Vision, USB3 Vision]** |
| `Min. black level <= Value <= Max. black level` | Specifies the black reference level.  Note that to determine the camera-specific minimum and maximum black reference level, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_BLACK_REF`](../../Reference/dig/MdigInquire.md)+[`M_MAX_VALUE`](../../Reference/dig/MdigInquire.md) or +[`M_MIN_VALUE`](../../Reference/dig/MdigInquire.md), respectively. |

---

### `M_BRIGHTNESS_REF`

**Board availability:** Clarity UHD

Sets the brightness level for composite input signals.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default reference level for the specified digitizer data format. **[Clarity UHD]** |
| `M_MAX_LEVEL` | Specifies the maximum value. **[Clarity UHD]** |
| `M_MIN_LEVEL` | Specifies the minimum value. **[Clarity UHD]** |
| `M_MIN_LEVEL <= Value <= M_MAX_LEVEL` | Specifies the brightness reference level. |

---

### `M_CONTRAST_REF`

**Board availability:** Clarity UHD

Sets the contrast level for composite input signals.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default reference level for the specified digitizer data format. |
| `M_MAX_LEVEL` | Specifies the maximum value. |
| `M_MIN_LEVEL` | Specifies the minimum value. |
| `M_MIN_LEVEL <= Value <= M_MAX_LEVEL` | Specifies the contrast reference level. |

---

### `M_HUE_REF`

**Board availability:** Clarity UHD

Sets the hue level for composite input signals.  [`M_HUE_REF`](../../Reference/dig/MdigControl.md) is available when grabbing color data (not monochrome data).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default reference level for the specified digitizer data format. |
| `M_MAX_LEVEL` | Specifies the maximum value. **[Clarity UHD]** |
| `M_MIN_LEVEL` | Specifies the minimum value. **[Clarity UHD]** |
| `M_MIN_LEVEL <= Value <= M_MAX_LEVEL` | Specifies the hue level. |

---

### `M_SATURATION_REF`

**Board availability:** Clarity UHD

Sets the saturation level for composite input signals.  [`M_SATURATION_REF`](../../Reference/dig/MdigControl.md) is available when grabbing color data (not monochrome data).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default reference level for the specified digitizer data format. |
| `M_MAX_LEVEL` | Specifies the maximum value. |
| `M_MIN_LEVEL` | Specifies the minimum value. |
| `M_MIN_LEVEL <= Value <= M_MAX_LEVEL` | Specifies the saturation level. |

---

### `M_WHITE_REF`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision

Sets the input signal's digitization white reference level. Note that when setting black and white reference levels, always set black reference levels first.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default reference level for the specified digitizer data format. |
| `M_AUTOMATIC` | Sets the reference level automatically. **[GenTL, GevIQ, GigE Vision, USB3 Vision]** |
| `Min. white level <= Value <= Max. white level` | Specifies the white reference level.  Note that to determine the camera-specific minimum and maximum white reference level, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_WHITE_REF`](../../Reference/dig/MdigInquire.md) + [`M_MAX_VALUE`](../../Reference/dig/MdigInquire.md) or + [`M_MIN_VALUE`](../../Reference/dig/MdigInquire.md), respectively. |

### Combination Constants — For controlling the reference level of a specific acquisition path

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the reference level of a specific acquisition path when using RGB or multi-tap input.

> **Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision

| Value | Description |
| --- | --- |
| `M_ALL_REF` | Sets the reference level on all available acquisition paths. |
| `M_CH0_REF` | Sets the reference level on acquisition path 0. |
| `M_CH1_REF` | Sets the reference level on acquisition path 1. |
| `M_CH2_REF` | Sets the reference level on acquisition path 2. |

### For controlling the input gain and shading correction

The following control types allow you to control the input gain and shading correction.

---

### `M_GAIN`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2

Sets the input gain with which to amplify the input signal. Note that gain affects the acquisition path before the data is digitized, and is applied to all the color bands of the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the camera's default value or, if supported, to enable the camera's auto control state for this feature (that is, the camera adjusts this value automatically). |
| `Min. gain <= Value <= Max. gain` | Specifies a value between the minimum and maximum values supported by the camera. Use [`MdigInquire`](../../Reference/dig/MdigInquire.md) to determine these values. |

---

### `M_GRAB_AUTOMATIC_INPUT_GAIN`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, USB3 Vision

Sets whether the input gain should be automatically set.

#### System specific

| Board(s) | Note |
|---|---|
| Clarity UHD | This control type is only supported when the device number of the specified digitizer is [`M_DEV2`](../../Reference/dig/MdigInquire.md) or [`M_DEV3`](../../Reference/dig/MdigInquire.md). Note that this control type is available when the DCF specifies to grab either an analog or digital input signal. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the input gain is not automatically set. |
| `M_ENABLE` *(default)* | Specifies that the input gain is automatically set. |

---

### `M_GRAB_INPUT_GAIN`

**Board availability:** Clarity UHD

Sets the input gain with which to amplify the input signal. This allows you to optimize the amplitude of the analog video input signal so that the full dynamic range of the component digitizing the video is used.  To automatically set your input gain, use [`M_GRAB_AUTOMATIC_INPUT_GAIN`](../../Reference/dig/MdigControl.md).  To manually set your input gain, [`M_GRAB_AUTOMATIC_INPUT_GAIN`](../../Reference/dig/MdigControl.md) must first be disabled.  This control type is only supported when the device number of the specified digitizer is [`M_DEV2`](../../Reference/dig/MdigInquire.md) or [`M_DEV3`](../../Reference/dig/MdigInquire.md). Note that this control type is available when the DCF specifies to grab either an analog or digital input signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default input gain factor.  When dealing with a composite or RGB analog input signal, the input gain factor is 128. |
| `M_MAX_LEVEL` | Specifies the maximum input gain factor. |
| `M_MIN_LEVEL` | Specifies the minimum input gain factor. |
| `0 <= Value <= 255` | Specifies an integer value which is used to determine the input gain factor. With this range, 0 is mapped to the minimum gain factor supported by the camera, while 255 is mapped to the maximum gain factor supported by the camera.  This value is supported only when using a DCF that uses either a composite or RGB analog video signal. |

---

### `M_SHADING_CORRECTION`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets whether to perform a gain and offset correction per pixel. This shading correction operation is performed by the digitizer when transferring grabbed data to the Host, using the following equation:  `(_Grab Image_ x [`M_SHADING_CORRECTION_GAIN_ID`](../../Reference/dig/MdigControl.md)) + [`M_SHADING_CORRECTION_OFFSET_ID`](../../Reference/dig/MdigControl.md)`  Each row of gain and offset values is applied to the equivalent row of the grabbed image. To set the gain and offset values for shading correction, use [`M_SHADING_CORRECTION_GAIN_ID`](../../Reference/dig/MdigControl.md) and [`M_SHADING_CORRECTION_OFFSET_ID`](../../Reference/dig/MdigControl.md), respectively.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | > **Note:** Note that this control type is only available on Zebra Rapixo CXP Pro and Zebra Rapixo CoF 100, and only if the loaded FPGA configuration contains the Zebra PU that supports this functionality. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Disables shading correction. |
| `M_ENABLE` | Enables shading correction. |

---

### `M_SHADING_CORRECTION_GAIN_FIXED_POINT`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the number of bits in the fractional part of the gain values specified for performing shading correction, where values are in fixed point format. The image resulting from the shading correction will be rounded to the nearest whole number.  To enable on-board shading correction and provide gain values, use [`M_SHADING_CORRECTION`](../../Reference/dig/MdigControl.md) and [`M_SHADING_CORRECTION_GAIN_ID`](../../Reference/dig/MdigControl.md), respectively.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | > **Note:** Note that this control type is only available on Zebra Rapixo CXP Pro and Zebra Rapixo CoF 100, and only if the loaded FPGA configuration contains the Zebra PU that supports this functionality. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For an 8-bit buffer, the default value is 8. For a 16-bit buffer, the default value is 14. |
| `0 < Value <= 16` | Specifies the number of bits in the fractional part of the fixed-point gain values. |

---

### `M_SHADING_CORRECTION_GAIN_ID`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the buffer containing the gain values that the digitizer should use when it performs shading correction. This is interpreted as a fixed point buffer.  To enable on-board shading correction and provide an offset, use [`M_SHADING_CORRECTION`](../../Reference/dig/MdigControl.md) and[`M_SHADING_CORRECTION_OFFSET_ID`](../../Reference/dig/MdigControl.md), respectively.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | > **Note:** Note that this control type is only available on Zebra Rapixo CXP Pro and Zebra Rapixo CoF 100, and only if the loaded FPGA configuration contains the Zebra PU that supports this functionality. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies not to apply a gain correction when performing shading correction. |
| `ShadingCorrectionGainID` | Specifies the identifier of an [`M_IMAGE`](../../Reference/buf/MbufAlloc1d.md) buffer containing the gain values. The buffer must be a single-band, unsigned 8-bit or 16-bit buffer. It is interpreted as a fixed point buffer with 2-bits representing the mantissa (significant bits). Use [`M_SHADING_CORRECTION_GAIN_FIXED_POINT`](../../Reference/dig/MdigControl.md) to set the number of bits for the fractional part. The buffer can have a maximum X-size of 16384 pixels and a Y-size equal to the size of the DCF. |

---

### `M_SHADING_CORRECTION_OFFSET_ID`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the buffer containing the offset values that the digitizer should use when it performs shading correction.  To enable on-board shading correction and provide a gain, use [`M_SHADING_CORRECTION`](../../Reference/dig/MdigControl.md) and [`M_SHADING_CORRECTION_GAIN_ID`](../../Reference/dig/MdigControl.md), respectively.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | > **Note:** Note that this control type is only available on Zebra Rapixo CXP Pro and Zebra Rapixo CoF 100, and only if the loaded FPGA configuration contains the Zebra PU that supports this functionality. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies not to apply an offset correction when performing shading correction. |
| `ShadingCorrectionOffsetID` | Specifies the identifier of an [`M_IMAGE`](../../Reference/buf/MbufAlloc1d.md) buffer containing the offset values. The buffer must be a single-band, unsigned 8-bit or 16-bit buffer. It can have a maximum X-size of 16384 pixels. |

### For routing I/O signals and setting their mode

The following control types allow you to set the mode and the purpose of your Zebra imaging board's I/O signals (such as, auxiliary, Camera Link control, and transport layer trigger signal). Once the format, routing, and mode are determined for an I/O signal, you can further control the I/O signal using the control types described in the following tables: [`GroupUserBits`](../../Reference/GroupUserBits.md), [`GroupTimerSignals`](../../Reference/GroupTimerSignals.md), [`GroupExposureSignals`](../../Reference/GroupExposureSignals.md), and [`GroupTriggerSignals`](../../Reference/GroupTriggerSignals.md).  > **Note:** Note that for all Aurora Imaging Library supported hardware that have I/O signals, but are not supported with the constants below, see [`MsysControl`](../../Reference/sys/MsysControl.md).  Each Zebra imaging board and Aurora Imaging Library driver has its own list of limitations regarding the signals you can control with this function. While general limitations are listed in the table below, for a complete list of the available signals and their limitations, see the _Connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board or Aurora Imaging Library driver.

> **Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

---

### `M_IO_DEBOUNCE_TIME`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the amount of time that the specified auxiliary input signal is debounced. A maximum of 4 inputs can have a debounce set.

| Value | Description |
| --- | --- |
| `0 <= Value <= 8300000` | Specifies the minimum amount of time to ignore any additional signal transitions after accepting a signal transition, in nsec. |

---

### `M_IO_FORMAT`

**Board availability:** GenTL, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets whether to enable a specific transmitter/receiver for an I/O signal, on systems whose transmitters/receivers are enabled through software and where the option of two or more signal formats are possible. Note that some signals cannot be affected individually. In this case, if you affect any one of the signals in the group, they will all be affected. For more information, refer to the Technical information appendix in the _Installation and Hardware Reference_ manual for your Zebra imaging board.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the specified I/O signal is not available for use. **[GenTL, GigE Vision, USB3 Vision]** |
| `M_LINK_SIGNAL` | Specifies to use the transport layer link trigger for the specified I/O signal. **[Rapixo CXP, Rapixo CoF]** |
| `M_LVDS` | Specifies to use the LVDS transmitter/receiver for the specified I/O signal. |
| `M_OPEN_DRAIN` | Specifies to use the open collector (open drain) transmitter/receiver for the specified I/O signal. **[GenTL, GigE Vision, USB3 Vision]** |
| `M_OPTO` | Specifies to use the opto-coupled transmitter/receiver for the specified I/O signal. **[GenTL, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_RS422` | Specifies to use the RS-422 transmitter/receiver for the specified I/O signal. **[GenTL, GigE Vision, USB3 Vision]** |
| `M_TRI_STATE` | Specifies to use the tri-state transmitter/receiver for the specified I/O signal. **[GenTL, GigE Vision, USB3 Vision]** |
| `M_TTL` | Specifies to use the TTL transmitter/receiver for the specified I/O signal. |

---

### `M_IO_INTERRUPT_ACTIVATION`

Sets the signal transition upon which to generate an interrupt, if interrupt generation has been enabled for the specified I/O signal. Use [`M_IO_INTERRUPT_STATE`](../../Reference/dig/MdigControl.md) to enable interrupt generation.  Note that this only applies to input signals.  Note that this control type only has an effect when [`M_IO_INTERRUPT_STATE`](../../Reference/dig/MdigControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the signal transition specified by the DCF. If this value is not specified in the DCF, the default is the same as [`M_EDGE_RISING`](../../Reference/dig/MdigControl.md). |
| `M_ANY_EDGE` | Specifies to generate an interrupt upon both a low-to-high and a high-to-low signal transition. |
| `M_EDGE_FALLING` | Specifies that an interrupt will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` | Specifies that an interrupt will be generated upon a low-to-high signal transition. |

---

### `M_IO_INTERRUPT_STATE`

Sets whether to generate an interrupt upon the specified transition of the I/O signal. Use [`M_IO_INTERRUPT_ACTIVATION`](../../Reference/dig/MdigControl.md) to specify the transition. Note that this only applies to input signals, or I/O signals set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the transition specified by the DCF. If this value is not specified in the DCF, the default is the same as [`M_DISABLE`](../../Reference/dig/MdigControl.md). |
| `M_DISABLE` | Specifies not to generate an interrupt. |
| `M_ENABLE` | Specifies to generate an interrupt. |

---

### `M_IO_MODE`

Sets the mode of the specified I/O signal. Note that you can only change the mode of a bidirectional (I/O) signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the mode specified by the DCF, or disabled when dealing with a tri-state I/O signal. |
| `M_INPUT` | Specifies that the signal is for input. |
| `M_OUTPUT` | Specifies that the signal is for output. |

---

### `M_IO_SOURCE`

Sets the type of signal to route to an output signal, or a bidirectional signal set to output mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the type of signal specified by the DCF.  If this value is not specified in the DCF and dealing with an auxiliary signal, the default is [`M_USER_BITn`](../../Reference/dig/MdigControl.md), where _n_ is the number of the user-bit that can drive the output signal.  For the number of the user-bit that can drive the specified output signal, see the _Connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board or Aurora Imaging Library driver. |
| `M_AUX_IOn` | Specifies to reroute auxiliary input signal _n_ to the output signal, where _n_ is the number of the auxiliary input signal. Note that the specified auxiliary signal can also be a bidirectional signal set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md)).  You can typically reroute an auxiliary input signal to an output signal that is hard-wired to the camera (for example, a Camera Control (CC) output signal or a transport layer (TL) trigger signal). Note that you cannot reroute auxiliary input signals to auxiliary output signals. |
| `M_DISABLE` | Specifies to not route any signal to the specified signal. |
| `M_LINK_TRIGGER_n_IN` | Specifies to route the transport layer link trigger input signal _n_ (from the camera) to the output signal, where _n_ is the number of the link trigger input signal. **[Rapixo CXP, Rapixo CoF]** |
| `M_TIMERn` | Specifies to route the output of timer _n_, where _n_ is the number of timers available. |
| `M_TL_TRIGGER` | Specifies to use the transport layer trigger signal (input mode only). The transport layer trigger signal is an embedded bidirectional signal from the physical transport layer connection of your camera. Typically, the TL Trigger is reserved for trigger information and is sent with other control and data signals along the same cable. **[Rapixo CXP, Rapixo CoF]** |
| `M_USER_BIT_CC_IOn` | Specifies to route the state of bit _n_ of the camera control static-user-output register, where _n_ is a value from 0 to 1. Note that you can route a bit from the camera control static-user-output register to any of the 4 Camera Link camera control output signals (such as routing bit 0 to the `M_CC_IO1` signal).  Note that there is one camera control static-user-output register per acquisition path. **[Radient eV-CL, Rapixo CL]** |
| `M_USER_BIT_TL_TRIGGER0` | Specifies to route the state of the bit of the TL trigger static-user-output register. **[Rapixo CXP, Rapixo CoF]** |
| `M_USER_BITn` | Specifies to route the state of bit _n_ of the main static-user-output register, where _n_ is the bit number.  Note that there is one main static-user-output register per acquisition path. However, if one of its bits can be routed to a signal that is shared across acquisition paths, changing that bit in one register affects the register of all the other acquisition paths.  For the number of the user-bit that can drive the specified output signal, see the _Connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board or Aurora Imaging Library driver. |

### Combination Constants — For specifying the type and number of the I/O signal to affect

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to set the type and number of the I/O signal to affect.

> **Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to affect auxiliary signal _n_, where _n_ is the signal number.  > **Note:** Note that, when using this value with [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md), the auxiliary signal _n_ must be an output signal, or an I/O signal set to output (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md)). |
| `M_CC_IOn` | Specifies to affect Camera Link camera control signal _n_, where _n_ is a value from 1 to 4.  > **Note:** Note that this combination value is only available for [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md).  For a list of the available camera control signals, see the _Connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board or Aurora Imaging Library driver. **[Radient eV-CL, Rapixo CL]** |
| `M_LINK_TRIGGER_n_IN` | Specifies to affect the link trigger input signal _n_ (from the camera), where _n_ is the number of the link trigger input signal.  > **Note:** Note that this combination value is only available for [`M_IO_FORMAT`](../../Reference/dig/MdigControl.md), [`M_IO_INTERRUPT_ACTIVATION`](../../Reference/dig/MdigControl.md), [`M_IO_MODE`](../../Reference/dig/MdigControl.md), and [`M_IO_STATUS`](../../Reference/dig/MdigInquire.md). **[Rapixo CXP, Rapixo CoF]** |
| `M_LINK_TRIGGER_n_OUT` | Specifies to affect the transport layer link trigger output signal _n_ (to the camera), where _n_ is the number of the link trigger output signal.  > **Note:** Note that this combination value is only available for [`M_IO_FORMAT`](../../Reference/dig/MdigControl.md), [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md), [`M_IO_MODE`](../../Reference/dig/MdigControl.md), and [`M_IO_STATUS`](../../Reference/dig/MdigInquire.md). **[Rapixo CXP, Rapixo CoF]** |
| `M_TL_TRIGGER` | Specifies to affect the transport layer trigger signal. The transport layer trigger signal is an embedded bidirectional signal from the physical transport layer connection of your camera. Typically, the TL trigger signal is reserved for trigger information and is sent with other control and data signals along the same cable.  > **Note:** Note that this combination value is only available for [`M_IO_FORMAT`](../../Reference/dig/MdigControl.md), [`M_IO_INTERRUPT_ACTIVATION`](../../Reference/dig/MdigControl.md), [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md), [`M_IO_MODE`](../../Reference/dig/MdigControl.md), and [`M_IO_STATUS`](../../Reference/dig/MdigInquire.md). **[Rapixo CXP, Rapixo CoF]** |

### For setting the state of specified user-bits in a static-user-output register

The following control types and control values allow you to set the bits in a static-user-output register. You can route the bits to output signals or I/O signals set to output; to do so, use [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md) with [`M_USER_BIT...`](../../Reference/dig/MdigControl.md)). To establish which user-bits can be routed to a specific signal, see the _connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board.  > **Note:** Note that for all Aurora Imaging Library supported hardware that have I/O signals, but are not supported with the constants below, see [`MsysControl`](../../Reference/sys/MsysControl.md).

> **Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

---

### `M_USER_BIT_STATE`

Sets the state of the specified bit of a static-user-output register.

| Value | Description |
| --- | --- |
| `M_OFF` | Specifies that the specified bit is set to off. |
| `M_ON` | Specifies that the specified bit is set to on. |

---

### `M_USER_BIT_STATE_ALL`

Sets the state of all the bits of the main static-user-output register or another specified static-user-output register.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | To affect a static-user-output register other than the main one, use one of the combination values documented below. |

| Value | Description |
| --- | --- |
| `Value` | Specifies a bit-encoded value that establishes the value of all the bits of the specified static-user-output register. It is recommended to specify the value in hexadecimal notation (0x), so that it is more legible to what you are setting each bit of the register. |

### Combination Constants — For specifying the bit in a static-user-output register to affect

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the static-user-output register and bit to affect.

> **Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

| Value | Description |
| --- | --- |
| `M_USER_BIT_CC_IOn` | Specifies which bit of the camera control static-user-output register to affect, where _n_ is a value from 0 to 1. In this case, _n_ represents the bit in the camera control static-user-output register.  Note that there is one camera control static-user-output register per acquisition path. **[Radient eV-CL, Rapixo CL]** |
| `M_USER_BIT_TL_TRIGGER0` | Specifies to affect the bit of the TL trigger static-user-output register. **[Rapixo CXP, Rapixo CoF]** |
| `M_USER_BITn` | Specifies to affect bit _n_ of the main static-user-output register.  Note that there is one main static-user-output register per acquisition path. However, if one of its bits can be routed to a signal that is shared across acquisition paths, changing that bit in one register affects the register of all the other acquisition paths. |

### Combination Constants — For specifying the static-user-output register

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the static-user-output register to affect, if you don't want to affect the main static-user-output register.

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_USER_BIT` *(default)* | Specifies to control the main static-user-output register. |
| `M_USER_BIT_CC_IO` | Specifies to control the static-user-output register associated with Camera Link camera control signals (the camera control static-user-output register). **[Radient eV-CL, Rapixo CL]** |
| `M_USER_BIT_TL_TRIGGER` | Specifies to control the static-user-output register associated with TL trigger signals (the TL trigger static-user-output register). **[Rapixo CXP, Rapixo CoF]** |

### For controlling the settings to grab using a trigger

The following control types and control values allow you to set the controls associated with triggered grabbing. For more information, see [Grabbing with triggers](../../UserGuide/C27_Grabbing_with_your_digitizer/S11_Grabbing_with_triggers_and_exposures.md).

---

### `M_GRAB_CONTINUOUS_END_TRIGGER`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets whether to automatically generate a trigger after [`MdigHalt`](../../Reference/dig/MdigHalt.md) is issued when performing a triggered continuous grab.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  [`M_ENABLE`](../../Reference/dig/MdigControl.md) is the default for a software triggered grab.  [`M_DISABLE`](../../Reference/dig/MdigControl.md) is the default for a hardware triggered grab. |
| `M_DISABLE` | Specifies not to generate the trigger automatically.  With a software triggered continuous grab operation, [`MdigHalt`](../../Reference/dig/MdigHalt.md) will wait indefinitely until a software trigger is issued on a separate thread.  With a hardware triggered continuous grab operation, [`MdigHalt`](../../Reference/dig/MdigHalt.md) will wait indefinitely until a hardware trigger is generated before invoking the last grab. |
| `M_ENABLE` | Specifies to generate the trigger automatically.  With a software or hardware triggered continuous grab operation, [`MdigHalt`](../../Reference/dig/MdigHalt.md) will generate a software trigger that invokes the last grab. |

---

### `M_GRAB_TRIGGER_ACTIVATION`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets the signal transition upon which to generate a grab trigger. Use [`M_GRAB_TRIGGER_STATE`](../../Reference/dig/MdigControl.md) to enable triggered grabbing.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the signal transition specified by the DCF. If the transition upon which to generate a grab trigger is not in the DCF, [`M_EDGE_RISING`](../../Reference/dig/MdigControl.md) is the default. **[Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_ANY_EDGE` | Specifies that a trigger will be generated upon both a low-to-high and a high-to-low signal transition. **[GenTL, GevIQ, GigE Vision, USB3 Vision]** |
| `M_EDGE_FALLING` | Specifies that a trigger will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` | Specifies that a trigger will be generated upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a trigger is continuously issued during a high signal polarity. **[GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_LEVEL_HIGH_END_WHEN_INACTIVE` | Specifies that triggered grabs are initiated by a high trigger signal polarity, and completed when the trigger signal goes low. **[GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_LEVEL_LOW` | Specifies that a trigger is continuously issued during a low signal polarity. **[GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_LEVEL_LOW_END_WHEN_INACTIVE` | Specifies that triggered grabs are initiated by a low trigger signal polarity, and completed when the trigger signal goes high. **[GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |

---

### `M_GRAB_TRIGGER_DELAY`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

Sets the delay between the trigger and the grab.  Note, an error is generated if the specified delay cannot be respected.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the delay, in nsec. |

---

### `M_GRAB_TRIGGER_MISSED`

**Board availability:** Iris GTX

Sets whether the number of grab triggers missed should be counted.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to not count the number of grab triggers missed. |
| `M_ENABLE` | Specifies to count the number of grab triggers missed. |

---

### `M_GRAB_TRIGGER_OVERLAP`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Sets how to deal with a new grab trigger that occurs when the current grab is in progress.

#### System specific

| Board(s) | Note |
|---|---|
| GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2 | On the camera, this control type allows the exposure phase of a triggered grab to occur while the previous image is still being transferred to the Host. If disabled, the exposure phase and the data transfer phase will occur sequentially.  If the exposure phase ends before the previously grabbed image is transferred to the Host, disabling this control type can lead to dropped frames. If, however, the exposure phase ends after the previously grabbed image is transferred to the Host, disabling this control type can increase the speed of your application. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that a triggered grab will not overlap the transfer of the previous image. Note that, when grab trigger overlap is disabled, the exposure phase and the data transfer phase will occur sequentially. **[GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2]** |
| `M_ENABLE` *(default)* | Specifies that a triggered grab can overlap the transfer of the previous image. **[GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2]** |
| `M_OFF` | Specifies that a new trigger is ignored. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_PREVIOUS_FRAME` | Specifies that a trigger received, while a grab is in progress, will be latched (stored). As soon as the current grab finishes, a new grab will be started. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_RESET` *(default)* | Specifies that the current grab will be immediately stopped and a new grab will be started. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |

---

### `M_GRAB_TRIGGER_SOFTWARE`

Issues a software trigger.  To use this control type, the trigger source must be set to software ([`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_SOFTWARE`](../../Reference/dig/MdigControl.md)).  The call to issue a software trigger should be made in a thread separate from the thread performing the continuous grab operation (or the series of asynchronous grabs) to be triggered. If using [`MdigProcess`](../../Reference/dig/MdigProcess.md), the grab operation is automatically performed in a separate thread from the software trigger (along with any other issued [`M_GRAB_TRIGGER...`](../../Reference/dig/MdigControl.md) controls); whereas, [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md) and [`MdigGrab`](../../Reference/dig/MdigGrab.md) are performed in the same thread as the software trigger, unless otherwise programmed.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_ACTIVATE` | Specifies the default behavior. |

---

### `M_GRAB_TRIGGER_SOURCE`

Sets the signal source of the grab trigger when there are multiple sources available.

#### System specific

| Board(s) | Note |
|---|---|
| Iris GTX | > **Note:** Note that, when using an external trigger source to control both your external lighting source and your Zebra Iris's grab, if one out of x grabbed images is significantly darker (insufficiently lit), your external trigger source might be triggering faster than your Zebra Iris can grab, transfer the image, and prepare to grab again. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the one specified by the DCF. **[Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, V4L2]** |
| `M_NULL` | Specifies that there is no trigger source. |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal. **[GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_HSYNC` | Specifies to use the horizontal synchronization signal of the camera as the trigger source. **[Radient eV-CL, Rapixo CL]** |
| `M_IO_COMMAND_LIST1` | Specifies to use a bit of the I/O command register of I/O command list 1 as a trigger source. **[Iris GTX]** |
| `M_ROTARY_ENCODER` | Specifies to use the output of the default rotary decoder (set with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md)) as the trigger source. **[Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the trigger source, where _n_ is a number between 1 and 4.  To use the output of the default rotary decoder, use [`M_ROTARY_ENCODER`](../../Reference/dig/MdigControl.md). **[Iris GTX, Rapixo CXP, Rapixo CoF]** |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. Use [`M_GRAB_TRIGGER_SOFTWARE`](../../Reference/dig/MdigControl.md) to issue the trigger. |
| `M_SOFTWAREn` | Specifies to use the software trigger being used for the grab of another digitizer (on the same board) as the trigger source. With this control value, _n_ corresponds to 1+ the device number of the digitizer that is (by default) associated with the specified software trigger, and can be from 1 to 4. **[Rapixo CXP, Rapixo CoF]** |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source. Use the [`M_TIMER_...`](../../Reference/dig/MdigControl.md) to set up the timer. **[GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_VSYNC` | Specifies to use the vertical synchronization signal of the camera as the trigger source. **[Radient eV-CL, Rapixo CL]** |

---

### `M_GRAB_TRIGGER_STATE`

Sets whether, when a grab command is issued (for example, [`MdigGrab`](../../Reference/dig/MdigGrab.md)) to wait for a trigger before grabbing. If your Zebra frame grabber supports trigger input, you can set up your digitizer to perform a triggered grab, that is, to grab a frame upon the occurrence of an event. In this case, nothing is grabbed when you call [`MdigGrab`](../../Reference/dig/MdigGrab.md) or [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md) or [`MdigProcess`](../../Reference/dig/MdigProcess.md), until a specified event occurs. When grabbing continuously, the digitizer waits for a trigger before grabbing each frame; you must still call [`MdigHalt`](../../Reference/dig/MdigHalt.md) after grabbing all required frames.  Note that, you can specify the number of frames to grab per trigger received (using [`MdigProcess`](../../Reference/dig/MdigProcess.md) with [`M_FRAMES_PER_TRIGGER()`](../../Reference/dig/MdigProcess.md)) or only use a single trigger to start the grab (using [`MdigProcess`](../../Reference/dig/MdigProcess.md) with [`M_TRIGGER_FOR_FIRST_GRAB`](../../Reference/dig/MdigProcess.md)).

#### System specific

| Board(s) | Note |
|---|---|
| GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2 | Use [`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) to specify the trigger source. |
| Clarity UHD, Host System | The trigger source is automatically set to use a software trigger. |
| Host System | Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the state specified by the DCF or, if none specified, [`M_DISABLE`](../../Reference/dig/MdigControl.md). **[Clarity UHD, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_DISABLE` | Specifies that, when a grab command is issued, the grab occurs without waiting for a trigger.  Note that, this is the default behavior when using a software trigger. |
| `M_ENABLE` | Specifies that, when a grab command is issued, the grab waits for a trigger before occurring. |

### Combination Constants — For specifying which I/O command register bit to use

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which I/O command register bit to use.

> **Board availability:** Iris GTX

| Value | Description |
| --- | --- |
| `M_IO_COMMAND_BITn` | Specifies I/O command register bit _n_, where _n_ represents the bit number.  With Zebra Iris GTX, _n_ can be a value from 0 to 4. |

### For controlling frame bursts

The following control types allow you to control the settings for a grab of sequential frames into a multi-frame image buffer (frame burst).

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

---

### `M_GRAB_FRAME_BURST_END_TRIGGER_SOURCE`

Sets the signal from which a rising edge signals the end of a grab of sequential frames into a multi-frame image buffer (end-of-frame-burst trigger). This control type only has an effect if the [`M_GRAB_FRAME_BURST_END_TRIGGER_STATE`](../../Reference/dig/MdigControl.md) is set to [`M_ENABLE`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_AUX_IO0`](../../Reference/dig/MdigControl.md). |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal. Note that the specified auxiliary signal can also be a bidirectional signal set to input, using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md). |

---

### `M_GRAB_FRAME_BURST_END_TRIGGER_STATE`

Sets whether a grab into a multi-frame image buffer is ended upon an end-of-frame-burst trigger. Specify the trigger source using [`M_GRAB_FRAME_BURST_END_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md). Note that regardless of this setting, the grab also ends if the multi-frame image buffer is filled or the timeout is reached ([`M_GRAB_FRAME_BURST_MAX_TIME`](../../Reference/dig/MdigControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_DISABLE`](../../Reference/dig/MdigControl.md). |
| `M_DISABLE` | Specifies that an end-of frame-burst trigger is not used to end the grab. |
| `M_ENABLE` | Specifies that an end-of frame-burst trigger is used to end the grab. |

---

### `M_GRAB_FRAME_BURST_MAX_TIME`

Sets the maximum amount of time to wait for all the frames to be grabbed into the multi-frame image buffer. The timer starts when the first frame is grabbed. The number of frames in the buffer can be inquired using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_GRAB_FRAME_BURST_COUNT`](../../Reference/dig/MdigGetHookInfo.md). This is useful when the camera stops sending frames and the multi-frame buffer is only partially full.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies to wait indefinitely. |
| `0.000008 <= Value <= 1.000000` | Specifies the maximum amount of time to wait, in secs. |

---

### `M_GRAB_FRAME_BURST_SIZE`

Sets the number of sequential frames to grab into a multi-frame image buffer with one grab command ([`MdigGrab`](../../Reference/dig/MdigGrab.md), or one grab of [`MdigProcess`](../../Reference/dig/MdigProcess.md)); the defined number of frames are stored contiguously in the same buffer. The end-of-grab event only occurs once the entire group of frames has been grabbed, reducing the number of events that need to be handled. The Y-size of the grab buffer must be equal to (height of the frame * [`M_GRAB_FRAME_BURST_SIZE`](../../Reference/dig/MdigControl.md)). For more information about the grab buffer size, see [Specifying the dimensions of a multi-frame image buffer](../../UserGuide/C23_Data_buffers/S03_Specifying_the_dimensions_of_a_data_buffer.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 4095` *(default)* | Specifies the number of frames to grab. |

### For controlling the settings of a timer

The following control types and control values specify the settings for controlling a timer and the signal generated from the timer (timer output signals). For more information, see [Grabbing with triggers](../../UserGuide/C27_Grabbing_with_your_digitizer/S11_Grabbing_with_triggers_and_exposures.md).

> **Board availability:** GenTL, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

---

### `M_TIMER_ARM`

**Board availability:** Rapixo CXP, Rapixo CoF

Sets whether to enable the timer arming mechanism. If the timer arming mechanism is enabled, then the timer will ignore its trigger signal ([`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md)) until a signal transition specified using [`M_TIMER_ARM_ACTIVATION`](../../Reference/dig/MdigControl.md) occurs on the signal specified using [`M_TIMER_ARM_SOURCE`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that timer arming is disabled. |
| `M_ENABLE` | Specifies that timer arming is enabled. |

---

### `M_TIMER_ARM_ACTIVATION`

**Board availability:** Rapixo CXP, Rapixo CoF

Sets the signal transition upon which to arm the timer, if the timer arming mechanism is enabled. Use [`M_TIMER_ARM`](../../Reference/dig/MdigControl.md) to enable timer arming. Use [`M_TIMER_ARM_SOURCE`](../../Reference/dig/MdigControl.md) to set which input signal will arm the timer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE_FALLING` | Specifies that the timer will be armed upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the timer will be armed upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a timer is continuously armed during a high signal polarity. |
| `M_LEVEL_LOW` | Specifies that a timer is continuously armed during a low signal polarity. |

---

### `M_TIMER_ARM_SOFTWARE`

**Board availability:** Rapixo CXP, Rapixo CoF

Issues a software trigger to arm the timer.  To use this setting, the timer's arm source must be set to software ([`M_TIMER_ARM_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_SOFTWARE`](../../Reference/dig/MdigControl.md)).

| Value | Description |
| --- | --- |
| `M_ACTIVATE` | Specifies the default behavior. |

---

### `M_TIMER_ARM_SOURCE`

**Board availability:** Rapixo CXP, Rapixo CoF

Sets which input signal will arm the timer, if the timer arming mechanism is enabled. Use [`M_TIMER_ARM`](../../Reference/dig/MdigControl.md) to enable timer arming.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the input source set in the DCF. |
| `M_NULL` | Specifies to use no trigger source. |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source for the specified timer, where _n_ is the number of the auxiliary signal. Note that the specified auxiliary signal can also be a bidirectional signal set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md)). |
| `M_CONTINUOUS` | Specifies to automatically arm the specified timer immediately after the timer's duration expires. |
| `M_EXPOSURE_END` | Specifies to use the exposure end signal as the trigger source. The exposure end signal is a quick pulse that occurs when the exposure ends. |
| `M_EXPOSURE_START` | Specifies to use the exposure start signal as the trigger source. The exposure start signal is a quick pulse that occurs when the exposure starts. |
| `M_GRAB_TRIGGER` | Specifies to use the grab trigger source signal as the trigger source. To set the grab trigger source signal, use [`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md). |
| `M_HSYNC` | Specifies to use the horizontal synchronization signal as the trigger source. |
| `M_ROTARY_ENCODER` | Specifies to use the output of the default rotary decoder (set with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md)) as the trigger source. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder n, where _n_ is a number between 1 and 4.  To use the output of the default rotary decoder, use [`M_ROTARY_ENCODER`](../../Reference/dig/MdigControl.md). |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. Use [`M_TIMER_TRIGGER_SOFTWARE`](../../Reference/dig/MdigControl.md) to issue the trigger. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the timer number.  > **Note:** Note that, _n_ must be a value from 1 to 4. |
| `M_TL_TRIGGER` | Specifies to use the transport layer trigger signal. The transport layer trigger signal is an embedded bidirectional signal from the physical transport layer connection of your camera. Typically, the TL Trigger is reserved for trigger information and is sent with other control and data signals along the same cable. |
| `M_VSYNC` | Specifies to use the vertical synchronization signal as the trigger source. |

---

### `M_TIMER_CLOCK_SOURCE`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the source of the clock that drives the specified timer.  The clock source must have a frequency greater than or equal to 1 Hz.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the most appropriate clock source (as determined by the driver). |
| `M_HSYNC` | Specifies to use the horizontal synchronization frequency of your camera. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_PIXCLK` | Specifies to use the pixel clock frequency of your camera. **[Radient eV-CL, Rapixo CL]** |
| `M_SYSCLK` | Specifies to use the frequency of the allocated system's clock source. |
| `M_TIMERn` | Specifies to use the frequency of the output of the specified timer, where _n_ is the timer number. Note that, in this case, the timer output pulse is used as a clock tick. The specified timer should be in continuous mode (that is, [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) is set to [`M_CONTINUOUS`](../../Reference/dig/MdigControl.md)).  Only a continuous timer can clock another timer. References where timer 1 points to timer 2, and timer 2 points back to timer 1, are not supported and will generate an error. |
| `M_VSYNC` | Specifies to use the vertical synchronization frequency of your camera. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |

---

### `M_TIMER_DELAY`

Sets the delay between the timer trigger and the active portion of the timer output signal.  Note, an error is generated if the specified delay cannot be respected.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. **[Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `0` | Specifies that there is no delay. **[GenTL, GigE Vision, Iris GTX, USB3 Vision]** |
| `Value > 0` | Specifies the delay, in nsec. |

---

### `M_TIMER_DELAY2`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the delay between the end of the first active portion of the timer output signal and the start of the second pulse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. This is the same as specified in the DCF or, if not specified in the DCF, 0. |
| `Value > 0` | Specifies the delay, in nsec. |

---

### `M_TIMER_DURATION`

Sets the duration for the active portion of the timer output signal.  Note, an error is generated if the specified duration cannot be respected.

#### System specific

| Board(s) | Note |
|---|---|
| Iris GTX | Unlike with other products, when using the grab trigger signal to trigger the strobe, the specified duration of the strobe's timer output signal specifies how long the timer should remain active during the exposure period. The active period of the strobe's timer output will actually start after the specified delay, but the count for how long it should remain active only starts when the camera starts exposing its image sensor. Once counting begins, the signal will typically be active for the specified duration. If the duration of the strobe's timer is greater than the exposure time, the strobe's timer's output signal will end when the exposure time ends (set using [`M_EXPOSURE_TIME`](../../Reference/dig/MdigControl.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the duration specified by the DCF. **[Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `Value > 0` | Specifies the duration of the active portion of the timer output signal, in nsec. |

---

### `M_TIMER_DURATION2`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the duration for the active portion of the second pulse of the timer output signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. This is the same as specified in the DFC or, if not specified in the DCF, 0. |
| `Value > 0` | Specifies the duration of the active portion of the second pulse of the timer output signal, in nsecs. |

---

### `M_TIMER_OUTPUT_INVERTER`

**Board availability:** Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets whether the output of the timer should be inverted. This causes the low portion of the signal (the delay period) to be high and the high portion of the signal (the active portion) to be low.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_DISABLE` | Specifies not to invert the output of the timer. |
| `M_ENABLE` | Specifies to invert the output of the timer. |

---

### `M_TIMER_RESET_SOURCE`

**Board availability:** Rapixo CXP, Rapixo CoF

Sets the signal source to use to reset the timer to 0.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the trigger source set in the DCF. |
| `M_NULL` | Specifies to use no trigger source. |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal. Note that the specified auxiliary signal can also be a bidirectional signal set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md)). |
| `M_EXPOSURE_END` | Specifies to use the exposure end signal as the trigger source. The exposure end signal is a quick pulse that occurs when the exposure ends. |
| `M_EXPOSURE_START` | Specifies to use the exposure start signal as the trigger source. The exposure start signal is a quick pulse that occurs when the exposure starts. |
| `M_GRAB_TRIGGER` | Specifies to use the grab trigger source signal as the trigger source. To set the grab trigger source signal, use [`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md). |
| `M_HSYNC` | Specifies to use the horizontal synchronization signal as the trigger source. |
| `M_ROTARY_ENCODER` | Specifies to use the output of the default rotary decoder (set with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md)) as the trigger source. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder n, where _n_ is a number between 1 and 4.  To use the output of the default rotary decoder, use [`M_ROTARY_ENCODER`](../../Reference/dig/MdigControl.md). |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. Use [`M_TIMER_TRIGGER_SOFTWARE`](../../Reference/dig/MdigControl.md) to issue the trigger. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the timer number. |
| `M_TL_TRIGGER` | Specifies to use the transport layer trigger signal. The transport layer trigger signal is an embedded bidirectional signal from the physical transport layer connection of your camera. Typically, the TL Trigger is reserved for trigger information and is sent with other control and data signals along the same cable. |
| `M_VSYNC` | Specifies to use the vertical synchronization signal as the trigger source. |

---

### `M_TIMER_STATE`

Sets the state of the specified timer.  When a timer is enabled, the timer waits for a trigger to be received. To set the source of the trigger, use [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md). Once the trigger is received, the timer starts by outputting a low signal. This lasts for the duration of the delay period (set using [`M_TIMER_DELAY`](../../Reference/dig/MdigControl.md)). The timer then changes to output a high signal for the duration of the active period (set using [`M_TIMER_DURATION`](../../Reference/dig/MdigControl.md)). To invert this signal, use [`M_TIMER_OUTPUT_INVERTER`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. **[Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_DISABLE` | Specifies that the timer is disabled. |
| `M_ENABLE` | Specifies that the timer is enabled. |

---

### `M_TIMER_TRIGGER_ACTIVATION`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Sets the signal variation upon which to generate a timer trigger, if the specified timer is enabled. Use [`M_TIMER_STATE`](../../Reference/dig/MdigControl.md) to enable the timer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value. **[GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_ANY_EDGE` | Specifies that a timer trigger will be generated both upon a high-to-low and a low-to-high signal transition. **[GenTL, GigE Vision, USB3 Vision]** |
| `M_EDGE_FALLING` | Specifies that a timer trigger will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` | Specifies that a timer trigger will be generated upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a timer trigger is continuously issued during a high signal polarity. **[GenTL, GigE Vision, USB3 Vision]** |
| `M_LEVEL_LOW` | Specifies that a timer trigger is continuously issued during a low signal polarity. **[GenTL, GigE Vision, USB3 Vision]** |

---

### `M_TIMER_TRIGGER_MISSED`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets whether to count the number of trigger pulses missed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to count the number of trigger pulses missed. |
| `M_ENABLE` | Specifies to count the number of trigger pulses missed. |

---

### `M_TIMER_TRIGGER_OVERLAP`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets how to deal with a new trigger that occurs while the associated timer has not yet expired (both its delay and duration). This is applied to both the delay and duration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LATCH` | Specifies that a trigger received, while the associated timer has not expired, will be latched (stored). As soon as the current timer expires, a new trigger is issued by software. |
| `M_OFF` | Specifies that a new trigger is ignored. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_RESET` *(default)* | Specifies that a new trigger automatically resets the timer (regardless of whether it is in its delay or active period) and then restarts the timer. This process will repeat for each new trigger received. |

---

### `M_TIMER_TRIGGER_RATE_DIVIDER`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the frequency with which to accept trigger pulses (for example, if set to 2, the first trigger pulse is ignored and the second is accepted).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 255` | Specifies the frequency with which to accept a trigger out of a series of trigger pulses. Note that, if set to 1, all trigger pulses are accepted. |

---

### `M_TIMER_TRIGGER_SOFTWARE`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Issues a software trigger for the specified timer.  To use this setting, the timer's trigger source must be set to software ([`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_SOFTWARE`](../../Reference/dig/MdigControl.md)).

| Value | Description |
| --- | --- |
| `M_ACTIVATE` | Specifies the default behavior. |

---

### `M_TIMER_TRIGGER_SOURCE`

Selects the trigger source for the specified timer when there are multiple sources available.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the one specified by the DCF. **[GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_NULL` | Specifies that no trigger source is specified. **[GenTL, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source for the specified timer, where _n_ is the number of the auxiliary signal. Note that the specified auxiliary signal can also be a bidirectional signal set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md)). **[GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_CONTINUOUS` | Specifies to run the specified timer in periodic mode; no actual trigger signal is used. The timer is automatically reset after the timer's duration expires. The timer loops between a delay and an active period. **[GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_EXPOSURE_END` | Specifies to use the exposure end signal as the trigger source. The exposure end signal is a quick pulse that occurs when the exposure ends. **[GenTL, GigE Vision, USB3 Vision]** |
| `M_EXPOSURE_START` | Specifies to use the exposure start signal as the trigger source. The exposure start signal is a quick pulse that occurs when the exposure starts. **[GenTL, GigE Vision, Iris GTX, USB3 Vision]** |
| `M_GRAB_TRIGGER` | Specifies to use the grab trigger source signal as the trigger source. To set the grab trigger source signal, use [`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md). **[GenTL, GigE Vision, Iris GTX, USB3 Vision]** |
| `M_HSYNC` | Specifies to use the horizontal synchronization signal as the trigger source. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_ROTARY_ENCODER` | Specifies to use the output of the default rotary decoder (set with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md)) as the trigger source. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder n, where _n_ is a number between 1 and 4.  To use the output of the default rotary decoder, use [`M_ROTARY_ENCODER`](../../Reference/dig/MdigControl.md). **[Rapixo CXP, Rapixo CoF]** |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. Use [`M_TIMER_TRIGGER_SOFTWARE`](../../Reference/dig/MdigControl.md) to issue the trigger. **[GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the timer number. **[GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |
| `M_TL_TRIGGER` | Specifies to use the transport layer trigger signal. The transport layer trigger signal is an embedded bidirectional signal from the physical transport layer connection of your camera. Typically, the TL Trigger is reserved for trigger information and is sent with other control and data signals along the same cable. **[Rapixo CXP, Rapixo CoF]** |
| `M_VSYNC` | Specifies to use the vertical synchronization signal as the trigger source. **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |

### For connection testing

The following control types and control values relate to connection testing.

> **Board availability:** Rapixo CXP, Rapixo CoF

---

### `M_CONNECTION_TEST_ERROR_COUNT`

Reset the current connection error count for test packets.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to immediately reset the current connection error count for test packets. |

---

### `M_CONNECTION_TEST_MODE`

Sets whether to enable the CXP test mode.

| Value | Description |
| --- | --- |
| `M_MODE1` | Specifies that the CXP test mode is enabled. |
| `M_OFF` | Specifies that the CXP test mode is disabled. |

---

### `M_CONNECTION_TEST_PACKET_RECEIVED_COUNT`

Resets the count of test packets received.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to immediately reset the count of test packets received. |

---

### `M_CONNECTION_TEST_PACKET_TRANSMITTED_COUNT`

Resets the count of test packets transmitted.

| Value | Description |
| --- | --- |
| `Value = 0` | Specifies to immediately reset the count of test packets transmitted. |

### Combination Constants — For specifying the CXP input connector

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the CXP input connector.

> **Board availability:** Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_CONNECTIONn` | Specifies the connection at CXP input connector _n_, where _n_ is the CXP input connector number from 0 to 3. |

### For controlling the camera's exposure

The following control types and control values specify the settings for controlling the camera's exposure. For more information, see the Aurora Imaging Library Hardware-specific Notes for your Zebra imaging board.

> **Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2

---

### `M_AUTO_EXPOSURE`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

Sets the automatic exposure mode of the camera.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the camera's default values or, if supported, to enable the camera's auto control state for the gain, iris, and shutter features (that is, the camera adjusts these values automatically). |
| `M_AUTOMATIC` | Specifies that the camera's exposure duration is constantly adapted by the device to maximize its effect. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_DISABLE` | Specifies that the camera will not control the gain, iris, and shutter settings automatically. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_ENABLE` | Specifies that the camera controls the gain, iris, and shutter settings automatically. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |

---

### `M_EXPOSURE_DELAY`

**Board availability:** Iris GTX

Sets the required delay between the exposure trigger and the start of your camera's image sensor being exposed.  Note, an error is generated if the specified delay cannot be respected.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the delay, in nsec. |

---

### `M_EXPOSURE_MODE`

**Board availability:** Iris GTX

Sets whether the exposure duration is set using an exposure timer or using the width of the trigger signal.

| Value | Description |
| --- | --- |
| `M_TIMED` | Specifies to use [`M_EXPOSURE_TIME`](../../Reference/dig/MdigControl.md) to control the length of the exposure. |
| `M_TRIGGER_WIDTH` | Specifies to use duration of the active portion of the grab trigger's output signal to control the length of the exposure.  Use [`M_GRAB_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md)to specify the signal transition upon which to generate a grab trigger. Note that, [`M_TRIGGER_WIDTH`](../../Reference/dig/MdigControl.md) can only be used when [`M_GRAB_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md) is set to [`M_LEVEL_HIGH`](../../Reference/dig/MdigControl.md) or [`M_LEVEL_LOW`](../../Reference/dig/MdigControl.md). |

---

### `M_EXPOSURE_TIME`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2

Sets the amount of time to expose the camera's image sensor.  Note, an error is generated if the specified duration cannot be respected.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the camera's default exposure time, as specified by the DCF. **[Iris GTX]** |
| `Value >= 0` | Specifies the amount of time to expose the camera's image sensor, in nsec. |

### For controlling the settings of a quadrature decoder with inputs from a rotary or linear encoder

The following control types allow you to control the settings of a quadrature decoder with inputs from a rotary or linear encoder.

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

---

### `M_ROTARY_ENCODER_BIT0_SOURCE`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the auxiliary input signal on which to receive bit 0 of the 2-bit Gray code.  Note that if the auxiliary input signal for bit 0 is set using this constant, the corresponding auxiliary input signal for bit 1 will automatically be assigned; you do not need to specify it using [`M_ROTARY_ENCODER_BIT1_SOURCE`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as DCF. |
| `M_AUX_IOn` | Specifies the auxiliary input signal to use. The auxiliary input signal must support bit 0 of quadrature input. |

---

### `M_ROTARY_ENCODER_BIT1_SOURCE`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the auxiliary input signal on which to receive bit 1 of the 2-bit Gray code.  Note that if the auxiliary input signal for bit 1 is set using this constant, the corresponding auxiliary input signal for bit 0 will automatically be assigned; you do not need to specify it using [`M_ROTARY_ENCODER_BIT0_SOURCE`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as DCF. |
| `M_AUX_IOn` | Specifies the auxiliary input signal to use. The auxiliary input signal must support bit 1 of quadrature input. |

---

### `M_ROTARY_ENCODER_DIRECTION`

Sets the direction of movement occurring when the Gray code sequence received by the rotary decoder is 00 - 01 - 11 - 10; this essentially establishes whether to increment or decrement the counter when receiving this sequence. For more information on how to set the direction, refer to [Interpreting the direction of movement from the Gray code](../../UserGuide/C56_IO_signals/S07_Using_quadrature_input_from_a_rotary_encoder.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as DCF. |
| `M_BACKWARD` | Specifies a backward direction of movement and to decrement the counter when the Gray code sequence is 00 - 01 - 11 - 10. |
| `M_FORWARD` | Specifies a forward direction of movement and to increment the counter when the Gray code sequence is 00 - 01 - 11 - 10. |

---

### `M_ROTARY_ENCODER_FORCE_VALUE_SOURCE`

Sets the signal source to use to set the rotary decoder's counter to 0xFFFFFFFF. The rotary decoder will set the counter to this value on the rising edge of a signal transition of the selected source.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the source set in the DCF. |
| `M_NULL` | Specifies not to set the counter to 0xFFFFFFFF upon a signal. |
| `M_AUX_IOn` | Specifies to use an auxiliary input signal or an auxiliary bidirectional signal set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md)). |
| `M_COUNTER_OVERFLOW` | Specifies to set the counter to 0xFFFFFFFF and keep it at this value after the counter increments past 0x7FFFFFFF. To reset the counter to 0, use[`M_ROTARY_ENCODER_POSITION`](../../Reference/dig/MdigControl.md). |
| `M_POSITION_TRIGGER` | Specifies to use the trigger signal generated by the rotary decoder when the counter reaches the value specified with [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md). |
| `M_STEP_BACKWARD_WHILE_POSITIVE` | Specifies to set the counter to 0xFFFFFFFF upon a decrement, only if the counter value is in the range of 0x0 to 0x7FFFFFFF before the decrement occurs; when interpreting the counter value as signed, this would be the positive counter value range.  A typical use of this control value is to identify the number of steps taken backwards by the rotary encoder, so that the corresponding lines of data are not regrabbed. For a complete example, refer to [Using the rotary decoder's output to trigger a timer or a grab](../../UserGuide/C56_IO_signals/S07_Using_quadrature_input_from_a_rotary_encoder.md). |
| `M_TL_TRIGGER` | Specifies to use a transport layer trigger input signal. **[Rapixo CXP, Rapixo CoF]** |

---

### `M_ROTARY_ENCODER_FRAME_END_READ`

Sets whether to enable the rotary decoder to store the counter value at the end of the last grab or frame interrupt. If enabled, the value of the counter can be inquired at anytime using [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_ROTARY_ENCODER_FRAME_END_POSITION`](../../Reference/dig/MdigInquire.md), or retrieved at the end of the last grab from a hooked function, using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_ROTARY_ENCODER_FRAME_END_POSITION`](../../Reference/dig/MdigGetHookInfo.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the counter value will not be available to be inquired at the end of the last frame grabbed. |
| `M_ENABLE` | Specifies to enable the rotary decoder to store the counter value at the end of the last grabbed frame, so that it can be inquired. |

---

### `M_ROTARY_ENCODER_MULTIPLIER`

**Board availability:** Radient eV-CL, Rapixo CL

Sets the multiplying factor to apply to each increment/decrement of the rotary decoder's counter for every rotary encoder step (change in position); this in turn applies a multiplying factor to the number of pulses that the rotary decoder outputs per step. For example, if this control value is set to 4, and [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_STEP_ANY`](../../Reference/dig/MdigControl.md), the rotary decoder will increment its counter by 4 and output 4 pulses for every rotary encoder step.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 < Value <= 4096` *(default)* | Specifies the multiplying factor to use. |

---

### `M_ROTARY_ENCODER_OUTPUT_MODE`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the rotary decoder's counter value and/or the direction of movement upon which the rotary decoder should output a pulse. The pulse can be used to trigger a timer or a grab. To trigger a timer or a grab, set [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) or [`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md)(respectively) to [`M_ROTARY_ENCODER`](../../Reference/dig/MdigControl.md).  To decimate (subsample) the rotary decoder output signal before sending it to a timer or a grab controller, set this control type to [`M_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md) and set [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md) to the required decimation value. For more information, refer to [Pixel aspect ratio](../../UserGuide/C56_IO_signals/S07_Using_quadrature_input_from_a_rotary_encoder.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as DCF. |
| `M_POSITION_TRIGGER` | Specifies to output a pulse upon the trigger generated by [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md). |
| `M_POSITION_TRIGGER_MULTIPLE` | Specifies to start triggering only when the counter value set with [`M_ROTARY_ENCODER_POSITION_START_TRIGGER`](../../Reference/dig/MdigControl.md) is reached and then to generate a trigger at a set interval (change) in counter value. Set the interval with [`M_ROTARY_ENCODER_POSITION_START_TRIGGER`](../../Reference/dig/MdigControl.md). **[Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |
| `M_STEP_ANY` | Specifies to output a pulse upon any change in the rotary decoder's counter value (position change in any direction). |
| `M_STEP_ANY_WHILE_POSITIVE` | Specifies to output a pulse upon any change in the rotary decoder's counter value (position change in any direction), only if the counter value is in the range of 0x0 to 0x7FFFFFFF before the increment or decrement occurs; when interpreting the counter value as signed, this would be the positive counter value range.  Note that, if the counter value is treated as a signed integer and the counter is 0x7FFFFFFF, the next incremented value is 0x80000000 (falling into the negative counter value range). If you want to remain in the positive range, reset the counter to 0 using either [`M_ROTARY_ENCODER_POSITION`](../../Reference/dig/MdigControl.md) or [`M_ROTARY_ENCODER_RESET_SOURCE`](../../Reference/dig/MdigControl.md). |
| `M_STEP_FORWARD` | Specifies to output a pulse upon a rotary decoder counter increment only. |
| `M_STEP_FORWARD_WHILE_POSITIVE` | Specifies to output a pulse upon a rotary decoder counter increment, only if the counter value is in the range of 0x0 to 0x7FFFFFFF before the increment occurs; when interpreting the counter value as signed, this would be the positive counter value range.  Note that, if the counter value is treated as a signed integer and the counter is 0x7FFFFFFF, the next incremented value is 0x80000000 (falling into the negative counter value range). If you want to remain in the positive range, reset the counter to 0 using either [`M_ROTARY_ENCODER_POSITION`](../../Reference/dig/MdigControl.md) or [`M_ROTARY_ENCODER_RESET_SOURCE`](../../Reference/dig/MdigControl.md). |

---

### `M_ROTARY_ENCODER_POSITION`

Resets the rotary decoder's counter to 0 immediately.  To reset the counter to 0 upon a signal, use [`M_ROTARY_ENCODER_RESET_SOURCE`](../../Reference/dig/MdigControl.md).  Note that a call to [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_ROTARY_ENCODER_POSITION`](../../Reference/dig/MdigInquire.md) inquires the current value of the rotary decoder counter.

| Value | Description |
| --- | --- |
| `Value = 0` | Implements the default behavior. Note that, if a non-zero value is specified, an error is generated. |

---

### `M_ROTARY_ENCODER_POSITION_START_TRIGGER`

Sets the rotary decoder's counter value upon which to start generating triggers, when [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_POSITION_TRIGGER_MULTIPLE`](../../Reference/dig/MdigControl.md). If it is not set to this value, then [`M_ROTARY_ENCODER_POSITION_START_TRIGGER`](../../Reference/dig/MdigControl.md) is ignored.

| Value | Description |
| --- | --- |
| `0 <= Value <= 0xFFFFFFFF` | Specified the value of the counter upon which to start generating triggers. |

---

### `M_ROTARY_ENCODER_POSITION_TRIGGER`

Sets the rotary decoder's counter value upon which a trigger is generated.  You can hook a function to the trigger generated, using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with [`M_ROTARY_ENCODER`](../../Reference/dig/MdigHookFunction.md). You can also output this trigger to a timer or a grab controller using [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) set to [`M_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `0 <= Value <= 0xFFFFFFFF` | Specifies the value of the counter upon which a trigger is generated. If a value beyond the supported range is specified, an error is generated.  If you are treating the counter values as a signed range of values (for example, forcing the counter to reset to 0 at 0x80000000) and you want to generate a trigger upon a negative value, specify the equivalent value in the range of 0x80000000 to 0xFFFFFFFF.  If [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_POSITION_TRIGGER_MULTIPLE`](../../Reference/dig/MdigControl.md), this will specify the change in counter value at which to generate a trigger, beginning at the counter value specified by [`M_ROTARY_ENCODER_POSITION_START_TRIGGER`](../../Reference/dig/MdigControl.md).  If [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md), this will specify the counter value at which to generate a trigger each time this value is reached.  If [`M_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md) is set to any other control value, this value will be ignored. |

---

### `M_ROTARY_ENCODER_RESET_SOURCE`

Sets the signal source to use to reset the rotary decoder's counter to 0. The rotary decoder will reset the counter on the rising edge of a signal transition of the selected source.  Alternatively, you can immediately reset the counter to 0 (without using a signal to trigger the reset), using [`M_ROTARY_ENCODER_POSITION`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the source set in the DCF. |
| `M_NULL` | Specifies not to reset using a hardware signal source. |
| `M_AUX_IOn` | Specifies to use an auxiliary input signal or an auxiliary bidirectional signal set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md)). |
| `M_POSITION_TRIGGER` | Specifies to use the trigger signal generated by the rotary decoder when the counter reaches the value specified with [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md). |
| `M_TL_TRIGGER` | Specifies to use a transport layer trigger input signal. **[Rapixo CXP, Rapixo CoF]** |

### Combination Constants — For specifying which rotary decoder to set

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify which rotary decoder to set.

> **Board availability:** Radient eV-CL, Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_ROTARY_ENCODERn` | Specifies to set rotary decoder n, where _n_ is a number between 1 and 4.  Note that if you do not use this combination constant to specify a rotary decoder, the rotary decoder corresponding to 1+ the device number of the specified digitizer ([`DigId`](../../Reference/dig/MdigControl.md)) will be used. For example, when the device number of the specified digitizer is [`M_DEV2`](../../Reference/dig/MdigAlloc.md), rotary decoder [`M_ROTARY_ENCODER3`](../../Reference/dig/MdigControl.md) is used. |

### For controlling a data latch

The following control types allow you to control the settings for one of the data latches of your installed Aurora Imaging Library board. Data latches store information specific to a grabbed frame when the latch is triggered; the data latch is reset at the end of every grabbed frame. To retrieve information from a data latch, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_DATA_LATCH...`](../../Reference/dig/MdigGetHookInfo.md). Note that data latch information is only available when [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) is called from a function hooked to an end of grabbed frame event using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with [`M_GRAB_FRAME_END`](../../Reference/dig/MdigHookFunction.md) or from the hook-handler function (callback function) of [`MdigProcess`](../../Reference/dig/MdigProcess.md).  > **Note:** Note that you can only use data latches in association with the Aurora Imaging Library digitizer allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEV0`](../../Reference/dig/MdigAlloc.md).

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

---

### `M_DATA_LATCH_MODE`

Configures whether to latch data from the point when the grab is queued until the start of the first frame of a series of queued grabs or grab sequence.  If calls to [`MdigGrab`](../../Reference/dig/MdigGrab.md)occur while no other grab is currently being performed and [`M_DATA_LATCH_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_PREFETCH`](../../Reference/dig/MdigControl.md), events that occur, from the moment the grab command is issued until the start of the grabbed frame, can also trigger a data latch and be retrieved at the end of the grabbed frame, for each [`MdigGrab`](../../Reference/dig/MdigGrab.md) call.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NORMAL` *(default)* | Specifies not to latch before the start of the first grabbed frame. |
| `M_PREFETCH` | Specifies to latch data before the start of the first grabbed frame. |

---

### `M_DATA_LATCH_STATE`

Sets the state of the specified data latch.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Disables the data latch. |
| `M_ENABLE` | Enables the data latch. |

---

### `M_DATA_LATCH_TRIGGER_ACTIVATION`

Sets the trigger signal transition upon which to store the specified information to the specified data latch. Note that this is only useful when [`M_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) is set to [`M_AUX_IOn`](../../Reference/dig/MdigControl.md)or [`M_TIMER_ACTIVE`](../../Reference/dig/MdigControl.md).  To set the signal with which to trigger the data latch, use [`M_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE_FALLING` | Specifies to store the specified information to the data latch upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies to store the specified information to the data latch upon a low-to-high signal transition. |

---

### `M_DATA_LATCH_TRIGGER_SOURCE`

Sets what triggers storing the specified information to the specified data latch. Use [`M_DATA_LATCH_TYPE`](../../Reference/dig/MdigControl.md) to specify the type of information to store.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUX_IOn` | Specifies to use auxiliary signal _n_ as the trigger source, where _n_ is the auxiliary signal number. Note that the specified auxiliary signal can also be a bidirectional signal set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md)).  To specify the signal transition, use [`M_DATA_LATCH_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md). |
| `M_GRAB_FRAME_END` *(default)* | Specifies to trigger the data latch on the event that occurs at the end of each grabbed frame; this event occurs when the frame grabber receives the last pixels (or packet) of a frame from the camera. Note that, in this case, the grabbed data might not be completely transferred to the Host. |
| `M_GRAB_FRAME_START` | Specifies to trigger the data latch on the event that occurs at the start of each grabbed frame. |
| `M_GRAB_LINE` | Specifies to trigger the data latch on the event that occurs at the end of each grabbed line; this event occurs when the frame grabber receives the last pixels (or packet) of a line from the camera. Note that, in this case, the grabbed data might not be completely transferred to the Host. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the trigger source, where _n_ is the rotary decoder number.  To configure the rotary decoder, use the [`M_ROTARY_ENCODER...`](../../Reference/dig/MdigControl.md) control types. To establish when the rotary decoder issues a trigger, use [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md). |
| `M_TIMER_ACTIVE` | Specifies to trigger the data latch on the event that occurs when the specified timer is active. This can be at the start of the active period or the end of the active period, depending on how you set [`M_DATA_LATCH_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md). |

---

### `M_DATA_LATCH_TYPE`

Sets the type of information to store in the specified data latch. Note that each type of information can only be associated with one data latch, except for timestamp, which can be associated with up to four data latches. Aurora Imaging Library returns an error when this limitation is not respected.

| Value | Description |
| --- | --- |
| `M_IO_STATUS_ALL` | Stores the status of all the auxiliary I/O signals. |
| `M_ROTARY_ENCODERn` | Stores the value of the counter of rotary decoder _n_, where _n_ is a number between 1 and 4. To configure the rotary decoder, use the [`M_ROTARY_ENCODER...`](../../Reference/dig/MdigControl.md) control types. |
| `M_TIME_STAMP` | Stores the timestamp upon which the data latch is triggered, in ticks. Up to four separate data latches can store a timestamp.  To convert a timestamp from clock ticks to seconds, use the following equation: `_Timestamp_ * (` _TimestampFrequencyInHz_). To inquire the clock frequency, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_DATA_LATCH_CLOCK_FREQUENCY`](../../Reference/dig/MdigInquire.md). |

### Combination Constants — For specifying the data latch to affect

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which data latch to affect.

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_LATCHn` | Specifies which data latch to affect, where n is a value from 0 to 15. |

### Combination Constants — For specifying which on-board timer to control

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which on-board timer to control.

> **Board availability:** GenTL, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

| Value | Description |
| --- | --- |
| `M_TIMER_STROBE` | Specifies the strobe timer. **[Iris GTX]** |
| `M_TIMERn` | Specifies on-board timer _n_, where _n_ is one of the timers available on your Zebra imaging board or the hardware associated with your Aurora Imaging Library driver.  > **Note:** Note that, when dealing with timers (excluding [`M_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md)with [`M_TIMER_ACTIVE`](../../Reference/dig/MdigControl.md)) only a continuous timer can clock another timer. In addition, references where timer 1 points to timer 2, and timer 2 points back to timer 1, are not supported and will generate an error. **[GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision]** |

> **Note:** Note that, this control type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer.

Note that, for this control type to function on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)).

Note that this control type is used to perform Bayer conversion if performed by the Host. To control the Bayer conversion on the camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md); refer to your camera's documentation for details regarding the features to set.

Note that this reference type is only available when using a DCF that uses a composite video signal.

This control type is only available when dealing with a multicast controller or monitor (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_MULTICAST_CONTROLLER`](../../Reference/dig/MdigAlloc.md) or [`M_GC_MULTICAST_MONITOR`](../../Reference/dig/MdigAlloc.md)). For more information on using a multicast IP address, refer to Using IP multicast.

Note that this control type is only available when using color versions of Zebra Iris GTX (such as 2000C).

Note that certain source sizes, offsets, and destination buffer sizes can affect a grab. Refer to [`MdigGrab`](../../Reference/dig/MdigGrab.md) and [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md) for more information.

For a list of the available auxiliary I/O signals, see the _Connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board or Aurora Imaging Library driver.
