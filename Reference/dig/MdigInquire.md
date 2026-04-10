---
doctype: Reference
module: dig
function: MdigInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dig / MdigInquire"
---

# MdigInquire

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

> Inquire about a digitizer setting.

## Syntax

```c
AIL_INT MdigInquire(
    AIL_ID    DigId,        //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function allows you to inquire about various digitizer settings.

Note that you can use [`MdigControl`](../../Reference/dig/MdigControl.md) to control specific digitizer settings.

You can also interactively inquire most of the digitizer settings in real-time, using Feature Browser; to access this interface, use Aurora Imaging Intellicam or [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md).

Note that there are several inquire types that are available on a Host system, even without any additional Zebra imaging boards; these are to inquire a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). To see the inquire types available to inquire a simulated digitizer, select the Host system as your board type in this Help file. For more information on simulated digitizers, refer to [Simulated digitizer](../../UserGuide/C27_Grabbing_with_your_digitizer/S13_Simulated_Digitizer.md).

### System specific

| Board(s) | Note |
|---|---|
| GenTL, GigE Vision, USB3 Vision, V4L2 | Typically, the supported inquire types inquire the GenICam-compliant camera (or device) associated with the digitizer. These inquire types assume your camera supports the associated GenICam standard feature naming convention (SFNC) feature. If the device does not support the associated GenICam SFNC feature, an error is generated. In case of error, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) with the device-specific feature names and values specified by the device manufacturer. |

## Parameters

### `DigId` *(in, AIL_ID)*

Specifies the identifier of the digitizer.

### `InquireType` *(in, AIL_INT64)*

Specifies the digitizer setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MdigInquire`](../../Reference/dig/MdigInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For the general digitizer settings

The following inquire types allow you to inquire various digitizer settings.

---

### `M_BAYER_COEFFICIENTS_ID`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires an Aurora Imaging Library identifier of the internal buffer containing the white balance coefficients used when grabbing from a camera with a Bayer color filter. Note that you cannot free the retrieved buffer.

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF | Refer to your Zebra imaging board's hardware manual for more details. |
| GenTL, GigE Vision, USB3 Vision, V4L2 | Note that this inquire type is used to perform Bayer conversion if performed by the Host. To inquire the Bayer conversion on the camera, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md); refer to your camera's documentation for details regarding the features to inquire. |
| Iris GTX | Note that this inquire type is only available when using color versions of Zebra Iris GTX (such as 2000C). |

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you have set [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) to _M_DISABLE_. |
| `Array buffer identifier` | Specifies the identifier of an internally allocated [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer containing the white balance coefficients. |

---

### `M_BAYER_CONVERSION`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires whether a Bayer color conversion is performed by your digitizer.

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF | Refer to your Zebra imaging board's hardware manual for more details. |
| GenTL, GigE Vision, USB3 Vision, V4L2 | Note that this inquire type is used to perform Bayer conversion if performed by the Host. To inquire the Bayer conversion on the camera, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md); refer to your camera's documentation for details regarding the features to inquire. |
| Iris GTX | Note that this inquire type is only available when using color versions of Zebra Iris GTX (such as 2000C). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to not perform Bayer conversion on the grabbed image. |
| `M_ENABLE` *(default)* | Specifies to perform Bayer conversion on the grabbed image. |

---

### `M_BAYER_PATTERN`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the Bayer pattern used by the camera.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | Refer to your Zebra imaging board's hardware manual for more details. |
| GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2 | Refer to your camera's hardware manual for more details. |
| Iris GTX | Note that this inquire type is only available when using color versions of Zebra Iris GTX (such as 2000C). |

| Value | Description |
| --- | --- |
| `M_BAYER_BG` | Specifies the Bayer pattern where the top-left pixel is band and the next pixel is green. |
| `M_BAYER_GB` | Specifies the Bayer pattern where the top-left pixel is green and the next pixel is blue. |
| `M_BAYER_GR` | Specifies the Bayer pattern where the top-left pixel is green and the next pixel is red. |
| `M_BAYER_RG` | Specifies the Bayer pattern where the top-left pixel is red and the next pixel is green. |
| `M_WHITE_BALANCE_CALCULATE` | Determines the 3 coefficients required to white balance the source image, and writes these coefficients into the [`CoefOrExpId`](../../Reference/buf/MbufBayer.md) array. |
| `M_BAYER_BIT_SHIFT` | Indicates to shift the result by the specified number of bits. |
| `0 <= Value < Depth of source buffer` | Specifies the number of bits by which to shift the result. |
| `M_ADAPTIVE` | Specifies to use the adaptive demosaicing algorithm for the conversion process. |
| `M_ADAPTIVE_FAST` | Specifies to use a fast version of the adaptive demosaicing algorithm for the conversion process; using this algorithm gives you a good compromise between speed of the default bilinear algorithm and [`M_ADAPTIVE`](../../Reference/dig/MdigInquire.md) quality. |
| `M_AVERAGE_2X2` | Specifies to use a 2x2 neighborhood kernel to perform the demosaicing. |
| `M_COLOR_CORRECTION` | Specifies that a color correction is performed to further eliminate false colors. |
| `M_NULL` | Specifies that no Bayer pattern is used by the camera. |

---

### `M_BOARD_REVISION`

**Board availability:** Iris GTX

Inquires the revision number of your digitizer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the revision number. |

---

### `M_CAMERA_LOCKED`

**Board availability:** Clarity UHD, Host System, Rapixo CXP, Rapixo CoF

Inquires whether the digitizer is locked to the camera.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that a camera lock failed. |
| `M_YES` | Specifies that a camera lock succeeded. |

---

### `M_CAMERA_MODEL`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the name of the camera model.

| Value | Description |
| --- | --- |
| `Value` | Specifies the name of the camera model. |

---

### `M_CAMERA_PRESENT`

**Board availability:** Clarity UHD, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires whether a camera is present. Note that this does not indicate that a camera is locked.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that no camera is present. **[Clarity UHD, GevIQ, GigE Vision, Host System, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |
| `M_YES` | Specifies that a camera is present. |

---

### `M_CAMERA_VENDOR`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the name of the camera vendor.

| Value | Description |
| --- | --- |
| `"String"` | Specifies the name of the camera vendor. |

---

### `M_CHANNEL`

**Board availability:** Clarity UHD

Inquires the channel on which the digitizer is to acquire data.  Note that typically, this inquire type is only available when the specified digitizer uses acquisition paths with several multiplexed data inputs.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CH0` *(default)* | Specifies channel 0 as the channel on which the digitizer is to receive input data. |
| `M_CH1` | Specifies channel 1 as the channel on which the digitizer is to receive input data. |
| `M_CH2` | Specifies channel 2 as the channel on which the digitizer is to receive input data. |

---

### `M_CHANNEL_NUM`

Inquires the number of available channels on the digitizer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of channels. |

---

### `M_CHANNEL_SYNC`

**Board availability:** Clarity UHD, USB3 Vision

Inquires the channel currently being used as a synchronization channel instead of a data channel. Note that using a digitizer as an external synchronization signal renders the digitizer of that signal "occupied" and not available for allocating as a digitizer.

| Value | Description |
| --- | --- |
| `M_CHn` | Specifies the channel used for synchronization, where _n_ is the channel number. |

---

### `M_COMMAND_QUEUE_MODE`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires whether changes to digitizer settings affect the grab digitizer immediately, even if it is currently being used in a grab, or the change will wait until the current grab is complete.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_IMMEDIATE` | Specifies that, if you change the setting of a control type of a digitizer currently being used in a grab, the change affects the current grab. |
| `M_QUEUED` *(default)* | Specifies that, if you change the setting of a control type of a digitizer currently being used in a grab, the change does not affect the current grab. |

---

### `M_CORRUPTED_FRAME_ERROR`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2

Inquires whether an error is generated when a corrupted or incomplete frame is grabbed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to generate an error when grabbing a corrupted frame. |
| `M_ENABLE` *(default)* | Specifies to generate an error when grabbing a corrupted frame. |

---

### `M_DIG_PROCESS_IN_PROGRESS`

Inquires whether [`MdigProcess`](../../Reference/dig/MdigProcess.md) is currently running.

| Value | Description |
| --- | --- |
| `M_FALSE` | [`MdigProcess`](../../Reference/dig/MdigProcess.md) is not running. |
| `M_TRUE` | [`MdigProcess`](../../Reference/dig/MdigProcess.md) is running. |

---

### `M_DIGITIZER_INTERNAL_BUFFERS_NUM`

**Board availability:** GenTL, GigE Vision, USB3 Vision, V4L2

Inquires the number of internal grab buffers allocated and used when Aurora Imaging Library cannot grab directly into the specified buffers.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of internal grab buffers. |

---

### `M_DIGITIZER_TYPE`

**Board availability:** Iris GTX

Inquires the type of frame grabber(s) available to allocate a digitizer on the system.  The type of frame grabber is established by the type of sensor, so this inquire type returns the type of sensor.

| Value | Description |
| --- | --- |
| `M_GTX2000` | Specifies a Zebra Iris GTX 2000. |
| `M_GTX2000C` | Specifies a Zebra Iris GTX 2000C. |
| `M_GTX3000` | Specifies a Zebra Iris GTX 3000. |
| `M_GTX3000C` | Specifies a Zebra Iris GTX 3000C. |
| `M_GTX5000` | Specifies a Zebra Iris GTX 5000. |
| `M_GTX5000C` | Specifies a Zebra Iris GTX 5000C. |
| `M_GTX8000` | Specifies a Zebra Iris GTX 8000. |
| `M_GTX8000C` | Specifies a Zebra Iris GTX 8000C. |
| `M_GTX12000` | Specifies a Zebra Iris GTX 12000. |
| `M_GTX12000C` | Specifies a Zebra Iris GTX 12000C. |
| `M_GTX16000` | Specifies a Zebra Iris GTX 16000. |
| `M_GTX16000C` | Specifies a Zebra Iris GTX 16000C. |

---

### `M_EXTENDED_INIT_FLAG`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the digitizer initialization flag.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_DEV_NUMBER` | Specifies that the digitizer was allocated for the camera with the specified device number. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_EMULATED` | Specifies that the digitizer was allocated as a simulated digitizer. **[Host System]** |
| `M_GC_DEVICE_IP_ADDRESS` | Specifies that the digitizer was allocated for the camera with the specified IP address. **[GenTL, GevIQ, GigE Vision]** |
| `M_GC_DEVICE_NAME` | Specifies that the digitizer was allocated for the camera with the specified device's user name. **[GenTL, GevIQ, GigE Vision, USB3 Vision]** |
| `M_MINIMAL` | Specifies that grabbing is not permitted with the allocated digitizer. **[Clarity UHD]** |

---

### `M_FIX_PATTERN_NOISE_CORRECTION`

**Board availability:** Iris GTX

Inquires the type of fixed pattern noise correction to apply when acquiring images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PREPROCESSING` *(default)* | Specifies a correction for dark signal non-uniformity and for photo response non-uniformity, applied to the grabbed images. |
| `M_SENSOR` | Specifies a correction for dark signal non-uniformity, applied to the grabbed images. |

---

### `M_FOCUS`

**Board availability:** Iris GTX

Inquires the lens focus capabilities.  > **Note:** Note that this inquire type requires that your Zebra Iris GTX use an I<sup>2</sup>C (inter-integrated circuit) controlled lens.

| Value | Description |
| --- | --- |
| `Min. focus <= Value <= Max. focus` | Specifies a value between the minimum and maximum values supported by the camera. |

---

### `M_FOCUS_PERSISTENCE`

**Board availability:** Iris GTX

Inquires whether to store a focus position for the I<sub>2</sub>C (inter-integrated circuit) controlled lens. If there is no I<sub>2</sub>C (inter-integrated circuit) controlled lens attached, the value is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not store the focus position. |
| `M_ENABLE` | Specifies to store the focus position. |

---

### `M_FOCUS_PERSISTENT_VALUE`

**Board availability:** Iris GTX

Inquires the focus position to store for the I<sub>2</sub>C (inter-integrated circuit) controlled lens.

| Value | Description |
| --- | --- |
| `0 <= Value <= 1023` | Specifies to store the focus position. |

---

### `M_FORMAT`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the digitizer data format.

| Value | Description |
| --- | --- |
| `"M_DEFAULT"` | Specifies the default digitizer format. |
| `M_NULL` | Specifies the data format is set by the digitizer allocated as the multicast controller. |
| `"DCF File name"` | Specifies the path and file name of the DCF (for example: _"C:\mydirectory\myfile"_). |

---

### `M_FORMAT_DETECTED`

**Board availability:** Clarity UHD

Inquires the detected DCF name.

| Value | Description |
| --- | --- |
| `Value` | Specifies the detected DCF name. |

---

### `M_GC_CAMERA_TIME_STAMP`

**Board availability:** GenTL, GevIQ, GigE Vision, V4L2

Inquires the timestamp from the camera.  > **Note:** To inquire the timestamp of an event, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_GC_CAMERA_TIME_STAMP`](../../Reference/dig/MdigGetHookInfo.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the timestamp from the camera, in seconds. |

---

### `M_GC_CLPROTOCOL`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires whether the GenICam CLProtocol module is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the GenICam CLProtocol module is disabled. |
| `M_ENABLE` | Specifies that the GenICam CLProtocol module is enabled. |

---

### `M_GC_CLPROTOCOL_DEVICE_ID`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the identifier of the GenICam CLProtocol library currently associated with the Camera Link camera.

---

### `M_GC_CLPROTOCOL_DEVICE_ID_DEFAULT`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the identifier of the GenICam CLProtocol library currently set as the default in the Aurora Imaging Configurator utility. If no default has been set in the Aurora Imaging Configurator utility, this will cause an error.

---

### `M_GC_CLPROTOCOL_DEVICE_ID_NUM`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the number of GenICam CLProtocol camera identifiers available, given the list of the GenICam CLProtocol libraries installed on your computer.  Note that this inquire type can be used to enumerate the complete list of camera identifiers installed on your computer, using [`M_GC_CLPROTOCOL_DEVICE_ID`](../../Reference/dig/MdigInquire.md). For more information, refer to [Using Aurora Imaging Library with GenICam](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of the GenICam CLProtocol libraries. |

---

### `M_GC_CLPROTOCOL_DEVICE_ID_SIZE_MAX`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the maximum number of characters required to store the GenICam CLProtocol camera identification string.  Note that this inquire value can be used to enumerate the complete list of camera identifiers installed on your computer, using [`M_GC_CLPROTOCOL_DEVICE_ID`](../../Reference/dig/MdigInquire.md). For more information, refer to [Using Aurora Imaging Library with GenICam](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum number of characters required. |

---

### `M_GC_COUNTER_TICK_FREQUENCY`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

Inquires the camera's counter tick frequency. Note that a value of 0 Hz indicates that the camera does not support time stamps.

| Value | Description |
| --- | --- |
| `Value` | Specifies the camera's counter tick frequency, in Hz. |

---

### `M_GC_FEATURE_POLLING`

**Board availability:** GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires whether specific camera features will be periodically polled for updates when Feature Browser, accessed through Aurora Imaging Intellicam or [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md), is open.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that automatically changing camera features will not be polled. |
| `M_ENABLE` | Specifies that automatically changing camera features will be polled. |

---

### `M_GC_FRAME_MAX_RETRIES`

**Board availability:** GigE Vision

Inquires the maximum number of times packets should be re-sent before flagging their associated frame as corrupt.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= M_GC_PACKET_MAX_RETRIES` *(default)* | Specifies the maximum number of times to retry sending the packets of a frame. |

---

### `M_GC_FRAME_STATUS_CODE`

**Board availability:** GevIQ, GigE Vision, USB3 Vision

Inquires the status code from the last grabbed image.

| Value | Description |
| --- | --- |
| `Value` | Specifies the status code. |

---

### `M_GC_FRAME_TIMEOUT`

**Board availability:** GigE Vision

Inquires the maximum amount of time to wait for the remaining packets of a frame, after receiving the trailer packet.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum time to wait, in msec. |

---

### `M_GC_FRAME_TIMESTAMP`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision

Inquires the timestamp of the last frame grabbed.

| Value | Description |
| --- | --- |
| `Value` | Specifies the timestamp of the last frame grabbed, in sec. |

---

### `M_GC_HEARTBEAT_STATE`

**Board availability:** GevIQ, GigE Vision

Inquires whether the heartbeat mechanism is used to keep the GigE Vision-compliant camera active.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the heartbeat mechanism is disabled. |
| `M_ENABLE` *(default)* | Specifies that the heartbeat mechanism is enabled. |

---

### `M_GC_HEARTBEAT_TIMEOUT`

**Board availability:** GevIQ, GigE Vision

Inquires the amount of time that your GigE Vision-compliant camera will wait after the last communication before shutting down.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the amount of time, in msec. |

---

### `M_GC_INTER_PACKET_DELAY`

**Board availability:** GevIQ, GigE Vision

Inquires the delay between packets sent by your camera when transmitting a stream of image packets.  If you require the inter-packet delay in sec, use the following calculation to convert from timestamp ticks to sec: `(1/_m_)*_n_`, where m is the frequency of the timestamp ticks (to inquire this value, use [`M_GC_COUNTER_TICK_FREQUENCY`](../../Reference/dig/MdigInquire.md)), and _n_ is your inter-packet delay value.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the delay, in timestamp ticks. |

---

### `M_GC_INTERFACE_NAME`

**Board availability:** GenTL, GevIQ, GigE Vision

Inquires the name of the Host interface, such as the Ethernet controller or frame grabber, to which your camera is connected.

---

### `M_GC_LOCAL_CONTROL_PORT`

**Board availability:** GevIQ, GigE Vision

Inquires the UDP port number used for the GVCP (GigE Vision Control Protocol) channel on the computer connected to your GigE Vision-compliant camera.

| Value | Description |
| --- | --- |
| `0 <= Value <=66535` | Specifies the port number. |

---

### `M_GC_LOCAL_IP_ADDRESS`

**Board availability:** GevIQ, GigE Vision

Inquires the IP address (IPv4) of the Host's network adapter, connected to the GigE Vision-compliant camera.

| Value | Description |
| --- | --- |
| `Value` | Specifies the IP address (IPv4) of the Host's network adapter, connected to the GigE Vision-compliant camera.  The IP address is stored as a 32-bit integer in an _AIL_INT64_. Typically, IP addresses are written as 4 separate 8-bit numbers. To retrieve these values, either inquire the IP address as a string instead using[`M_GC_LOCAL_IP_ADDRESS_STRING`](../../Reference/dig/MdigInquire.md), or map the 32-bit IP address to 4 _AIL_UINT8_variables.  For example, the hexadecimal representation of the IP address `192.168.109.72`is `0x00000000486da8c0` (equivalent to the decimal 1215146176). The least-significant byte (`c0`) is the first number of the IP address (192), the second least-significant byte (`a8`) is the second number of the IP address (168) and so on. The first 8 digits (4 most-significant bytes) are not part of the IP address. |

---

### `M_GC_LOCAL_IP_ADDRESS_STRING`

**Board availability:** GevIQ, GigE Vision

Inquires the IP address of the Host's network adapter, connected to the GigE Vision-compliant camera, as a string.

| Value | Description |
| --- | --- |
| `"nnn.nnn.nnn.nnn"` | Specifies the IP address of the Host's network adapter, connected to the GigE Vision-compliant camera, as a string. The address string is expressed in dotted decimal notation, where each dotted decimal (_nnn_) is a number between 000 and 255. |

---

### `M_GC_LOCAL_MAC_ADDRESS`

**Board availability:** GevIQ, GigE Vision

Inquires the MAC address of the Host's network adapter, connected to the GigE Vision-compliant camera.

| Value | Description |
| --- | --- |
| `Value` | Specifies the MAC address of the Host's network adapter, connected to the GigE Vision-compliant camera.  The MAC address is stored as a 48-bit integer in an _AIL_INT64_. Typically, MAC addresses are written as 6 separate 8-bit numbers (hexadecimal number pairs). To retrieve these numbers, either inquire the MAC address as a string instead using[`M_GC_LOCAL_MAC_ADDRESS_STRING`](../../Reference/dig/MdigInquire.md), or map the 48-bit MAC address to 6 _AIL_UINT8_variables.  For example, the stored representation of the MAC address `84:20:fc:33:0e:e9`is `0x0000e90e33fc2084`. The least-significant byte (`84`) is the first number of the MAC address, the second least-significant byte (`0e`) is the second number of the MAC address and so on. The first 4 hexadecimal digits (2 most significant bytes) are not part of the MAC address. |

---

### `M_GC_LOCAL_MAC_ADDRESS_STRING`

**Board availability:** GevIQ, GigE Vision

Inquires the MAC address of the Host's network adapter, connected to the GigE Vision-compliant camera, as a string.

| Value | Description |
| --- | --- |
| `"nn-nn-nn-nn-nn-nn"` | Specifies the MAC address of the Host's network adapter, connected to the GigE Vision-compliant camera, as a string. The address string is expressed in hexadecimal pairs, where each pair (_nn_) is a hexadecimal number between 00 and FF. |

---

### `M_GC_LOCAL_MESSAGE_PORT`

**Board availability:** GevIQ, GigE Vision

Inquires the UDP port number used for the message channel on the computer connected to your GigE Vision-compliant camera.  This inquire type is only available when dealing with a multicast controller, or multicast monitor (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_MULTICAST_CONTROLLER`](../../Reference/dig/MdigAlloc.md) or [`M_GC_MULTICAST_MONITOR`](../../Reference/dig/MdigAlloc.md), respectively).

| Value | Description |
| --- | --- |
| `0 <= Value <=65535` | Specifies the port number. |

---

### `M_GC_LOCAL_STREAM_PORT`

**Board availability:** GevIQ, GigE Vision

Inquires the UDP port number used for the GVSP (GigE Vision Stream Protocol) channel on the computer connected to your GigE Vision-compliant camera.  This inquire type is only available when dealing with a multicast controller, or monitor (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_MULTICAST_CONTROLLER`](../../Reference/dig/MdigAlloc.md) or [`M_GC_MULTICAST_MONITOR`](../../Reference/dig/MdigAlloc.md), respectively).

| Value | Description |
| --- | --- |
| `0 <= Value <=65535` | Specifies the port number. |

---

### `M_GC_MAX_NBR_PACKETS_OUT_OF_ORDER`

**Board availability:** GigE Vision

Inquires the maximum number of packets that can be received out-of-order, before the associated frame is marked as corrupt.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number of out-of-order packets to receive. |

---

### `M_GC_MESSAGE_CHANNEL_MULTICAST_ADDRESS_STRING`

**Board availability:** GigE Vision

Inquires the IP address used for the multicast message channel of your GigE Vision-compliant camera.  This inquire type is only available when dealing with a multicast controller, or multicast monitor (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_MULTICAST_CONTROLLER`](../../Reference/dig/MdigAlloc.md) or [`M_GC_MULTICAST_MONITOR`](../../Reference/dig/MdigAlloc.md), respectively).

| Value | Description |
| --- | --- |
| `"2nn.nnn.nnn.nnn"` | Specifies the IP address in dotted decimal notation. |

---

### `M_GC_MULTICAST_CONTROLLER_CONNECTED`

**Board availability:** GigE Vision

Inquires whether a multicast controller digitizer is connected to the GigE Vision-compliant camera. Note that this can only be inquired from a digitizer allocated as a worker (using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_MULTICAST_WORKER`](../../Reference/dig/MdigAlloc.md)).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that a multicast controller is not connected to the GigE Vision-compliant camera. |
| `M_TRUE` | Specifies that a multicast controller is connected to the GigE Vision-compliant camera. |

---

### `M_GC_NUMBER_OF_STREAM_CHANNELS`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision

Inquires the number of image streams available.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of image streams available. |

---

### `M_GC_PACKET_MAX_RETRIES`

**Board availability:** GigE Vision

Inquires the maximum number of times a packet can be re-sent.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= M_GC_FRAME_MAX_RETRIES` *(default)* | Specifies the maximum number of times to resend a given packet of a frame. |

---

### `M_GC_PACKET_RESEND`

**Board availability:** GigE Vision

Inquires whether to request packets be re-sent from your GigE Vision-compliant camera, if the packets are not receive properly (for example, when the packets are received out-of-order, or a packet timeout occurs).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that packets should never be re-sent. |
| `M_ENABLE` *(default)* | Specifies that packets should be re-sent as required. |

---

### `M_GC_PACKET_SIZE`

**Board availability:** GevIQ, GigE Vision

Inquires the packet size that is used by the GigE Vision-compliant camera when streaming data to the Host.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the packet size, in bytes. |

---

### `M_GC_PACKET_TIMEOUT`

**Board availability:** GigE Vision

Inquires the maximum amount of time to wait before flagging a packet as dropped.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum time to wait, in msec. |

---

### `M_GC_PAYLOAD_SIZE`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

Inquires the number of bytes for each image transferred using the stream channel of your GenICam SFNC-compliant camera. Note that, if GenICam "chunk mode" is enabled on the camera, then this inquire returns the number of bytes in each chunk as well as the image transferred.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of bytes transferred per image or chunk. |

---

### `M_GC_PIXEL_FORMAT`

**Board availability:** GenTL, GevIQ, GigE Vision, V4L2

Inquires the pixel format that the digitizer should use to create internal buffers to receive images from the camera, if the digitizer is a multicast monitor digitizer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as specified in the DCF. |
| `Value` | Specifies the data depth and raw color format to use to create the internal buffers. |

---

### `M_GC_PIXEL_FORMAT_STRING`

**Board availability:** GenTL, GevIQ, GigE Vision

Inquires the pixel format that the digitizer should use to create internal buffers to receive images from the camera, if the digitizer is a multicast monitor digitizer, expressed in a human readable format.

---

### `M_GC_PIXEL_FORMAT_SWITCHING`

**Board availability:** GenTL, GigE Vision, USB3 Vision, V4L2

Inquires whether to allow the camera's pixel format to change to automatically match the current grab buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable automatic pixel format switching. |
| `M_ENABLE` *(default)* | Specifies to enable automatic pixel format switching. |

---

### `M_GC_REMOTE_CONTROL_PORT`

**Board availability:** GevIQ, GigE Vision

Inquires the UDP port number used for the GVCP (GigE Vision Control Protocol) channel on your GigE Vision-compliant camera.

| Value | Description |
| --- | --- |
| `0 <= Value <=66535` | Specifies the port number. |

---

### `M_GC_REMOTE_IP_ADDRESS`

**Board availability:** GevIQ, GigE Vision

Inquires the IP address (IPv4) of the GigE Vision-compliant grabberless interface camera.

| Value | Description |
| --- | --- |
| `Value` | Specifies the IP address (IPv4) of the GigE Vision-compliant grabberless interface camera.  The IP address is stored as a 32-bit integer in an _AIL_INT64_. Typically, IP addresses are written as 4 separate 8-bit numbers. To retrieve these values, either inquire the IP address as a string instead using[`M_GC_REMOTE_IP_ADDRESS_STRING`](../../Reference/dig/MdigInquire.md), or map the 32-bit IP address to 4 _AIL_UINT8_variables.  For example, the hexadecimal representation of the IP address `192.168.109.72`is `0x00000000486da8c0` (equivalent to the decimal 1215146176). The least-significant byte (`c0`) is the first number of the IP address (192), the second least-significant byte (`a8`) is the second number of the IP address (168) and so on. The first 8 digits (4 most-significant bytes) are not part of the IP address. |

---

### `M_GC_REMOTE_IP_ADDRESS_STRING`

**Board availability:** GevIQ, GigE Vision

Inquires the IP address (IPv4) of the GigE Vision-compliant camera, as a string.

| Value | Description |
| --- | --- |
| `"nnn.nnn.nnn.nnn"` | Specifies the IP address of the GigE Vision-compliant camera as a string. The address string is expressed in dotted decimal notation, where each dotted decimal value (_nnn_) is a number between 0 and 255. |

---

### `M_GC_REMOTE_MAC_ADDRESS`

**Board availability:** GevIQ, GigE Vision

Inquires the MAC address of the GigE Vision-compliant camera.

| Value | Description |
| --- | --- |
| `Value` | Specifies the MAC address of the GigE Vision-compliant camera.  The MAC address is stored as a 48-bit integer in an _AIL_INT64_. Typically, MAC addresses are written as 6 separate 8-bit numbers (hexadecimal number pairs). To retrieve these numbers, either inquire the MAC address as a string instead using[`M_GC_REMOTE_MAC_ADDRESS_STRING`](../../Reference/dig/MdigInquire.md), or map the 48-bit MAC address to 6 _AIL_UINT8_variables.  For example, the stored representation of the MAC address `84:20:fc:33:0e:e9`is `0x0000e90e33fc2084`. The least-significant byte (`84`) is the first number of the MAC address, the second least-significant byte (`0e`) is the second number of the MAC address and so on. The first 4 hexadecimal digits (2 most significant bytes) are not part of the MAC address. |

---

### `M_GC_REMOTE_MAC_ADDRESS_STRING`

**Board availability:** GevIQ, GigE Vision

Inquires the MAC address of the GigE Vision-compliant camera, as a string.

| Value | Description |
| --- | --- |
| `"nn-nn-nn-nn-nn-nn"` | Specifies the MAC address of the GigE Vision-compliant camera, as a string. The address string is expressed in hexadecimal pairs, where each pair (_nn_) is a hexadecimal number between 00 and FF. |

---

### `M_GC_REMOTE_MESSAGE_PORT`

**Board availability:** GevIQ, GigE Vision

Inquires the UDP port number used for the message channel on your GigE Vision-compliant camera.

| Value | Description |
| --- | --- |
| `0 <= Value <=66535` | Specifies the port number. |

---

### `M_GC_REMOTE_STREAM_PORT`

**Board availability:** GevIQ, GigE Vision

Inquires the UDP port number used for the GVSP (GigE Vision Stream Protocol) channel on your GigE Vision-compliant camera.

| Value | Description |
| --- | --- |
| `0 <= Value <=66535` | Specifies the port number. |

---

### `M_GC_SCHEMA_MAJOR`

**Board availability:** GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the major version number of the schema used by the camera. The version number of the schema is structured as [`M_GC_SCHEMA_MAJOR`](../../Reference/dig/MdigInquire.md).[`M_GC_SCHEMA_MINOR`](../../Reference/dig/MdigInquire.md).

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL | > **Note:** Note that this inquire type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the major number of the schema version. |

---

### `M_GC_SCHEMA_MINOR`

**Board availability:** GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the minor version number of the schema used by the camera. The version number of the schema is structured as [`M_GC_SCHEMA_MAJOR`](../../Reference/dig/MdigInquire.md).[`M_GC_SCHEMA_MINOR`](../../Reference/dig/MdigInquire.md).

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL | > **Note:** Note that this inquire type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the minor number of the schema version. |

---

### `M_GC_SERIAL_NUMBER`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the serial number for the camera.

| Value | Description |
| --- | --- |
| `"String"` | Specifies the serial number for the camera. |

---

### `M_GC_SPECIFIC_INFO`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

Inquires the specific information string for the camera.

| Value | Description |
| --- | --- |
| `"String"` | Specifies the specific information string for the camera. |

---

### `M_GC_STREAM_CHANNEL_MULTICAST_ADDRESS_STRING`

**Board availability:** GigE Vision

Inquires the IP address used for the multicast stream channel of your GigE Vision-compliant camera.  This inquire type is only available when dealing with a multicast controller, or multicast monitor (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_MULTICAST_CONTROLLER`](../../Reference/dig/MdigAlloc.md) or [`M_GC_MULTICAST_MONITOR`](../../Reference/dig/MdigAlloc.md), respectively).

| Value | Description |
| --- | --- |
| `"2nn.nnn.nnn.nnn"` | Specifies the IP address in dotted decimal notation. |

---

### `M_GC_STREAMING_MODE`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the camera's image stream activation mechanism.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTOMATIC` *(default)* | Specifies that the image stream is started and stopped automatically. |
| `M_MANUAL` | Specifies that the image stream is started and stopped manually. |

---

### `M_GC_STREAMING_STOP_CHECK_PERIOD`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the maximum amount of time to wait before Aurora Imaging Library checks to see whether a grab is pending.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the amount of time, in msec. |

---

### `M_GC_STREAMING_STOP_DELAY`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the amount of time to wait before stopping the image stream, if no grab is pending.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the amount of time, in msec. |

---

### `M_GC_THEORETICAL_INTER_PACKET_DELAY`

**Board availability:** GevIQ, GigE Vision

Inquires the theoretical delay between packets sent by your camera when transmitting a stream of image packets. This theoretical delay is determined by the GigE Vision-compliant camera's current state. This inquire type is provided as a starting point to calculate a usable inter-packet delay.

| Value | Description |
| --- | --- |
| `Value` | Specifies the theoretical delay between packets, in sec. |

---

### `M_GC_TOTAL_FRAME_CACHE_HITS`

**Board availability:** GigE Vision

Inquires the number of partial or complete frames that pass through the frame cache.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of frames. |

---

### `M_GC_TOTAL_FRAMES_CORRUPTED`

**Board availability:** GevIQ, GigE Vision, USB3 Vision

Inquires the number of corrupted frames.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of corrupted frames. |

---

### `M_GC_TOTAL_FRAMES_GRABBED`

**Board availability:** GevIQ, GigE Vision, USB3 Vision

Inquires the number of frames grabbed. Note that this number includes the number of corrupt frames.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of frames grabbed. |

---

### `M_GC_TOTAL_FRAMES_MISSED`

**Board availability:** GevIQ, GigE Vision, USB3 Vision

Inquires the number of frames missed.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of frames missed. |

---

### `M_GC_TOTAL_PACKET_CACHE_HITS`

**Board availability:** GigE Vision

Inquires the number of packets that pass through the packet cache.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of packets. |

---

### `M_GC_TOTAL_PACKETS_MISSED`

**Board availability:** GigE Vision

Inquires the number of packets missed.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of packets missed. |

---

### `M_GC_TOTAL_PACKETS_RECEIVED`

**Board availability:** GevIQ, GigE Vision

Inquires the number of packets received.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of packets received. |

---

### `M_GC_TOTAL_PACKETS_RECEIVED_OUT_OF_ORDER`

**Board availability:** GigE Vision

Inquires the number of packets received out of order.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of packets received out of order. |

---

### `M_GC_TOTAL_PACKETS_RECOVERED`

**Board availability:** GigE Vision

Inquires the number of packets recovered.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of packets recovered. |

---

### `M_GC_TOTAL_PACKETS_RESENDS_NUM`

**Board availability:** GigE Vision

Inquires the total number of packet re-send requests sent to the GigE Vision-compliant camera. Note that, one re-send request can cause multiple consecutive packets to be re-sent.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of packets re-sent. |

---

### `M_GC_TOTAL_PACKETS_TIMEOUT`

**Board availability:** GigE Vision

Inquires the total number of packets that have timed-out.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of packets timeouts. |

---

### `M_GC_UNIQUE_ID_STRING`

**Board availability:** GevIQ, GigE Vision, USB3 Vision

Inquires the unique identifier for your camera.

#### System specific

| Board(s) | Note |
|---|---|
| GigE Vision | This is the camera's MAC address. |
| USB3 Vision | This is the camera's global unique identifier (GUID). |

| Value | Description |
| --- | --- |
| `"nnnnnnnnnnnn"` | Specifies the unique identifier, as a string. The string is expressed in hexadecimal. |

---

### `M_GC_USER_NAME`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the camera's name.

| Value | Description |
| --- | --- |
| `"CameraName"` | Specifies the name of the camera. |

---

### `M_GC_VERSION`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the version number string for the camera.

| Value | Description |
| --- | --- |
| `"String"` | Specifies the version number string for the camera. |

---

### `M_GC_XML_MAJOR`

**Board availability:** GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the major version number of the GenICam-compliant camera's device description file (XML). The version number of the XML is structured as [`M_GC_XML_MAJOR`](../../Reference/dig/MdigInquire.md).[`M_GC_XML_MINOR`](../../Reference/dig/MdigInquire.md).[`M_GC_XML_SUBMINOR`](../../Reference/dig/MdigInquire.md).

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL | > **Note:** Note that this inquire type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the major version number of the xml. |

---

### `M_GC_XML_MINOR`

**Board availability:** GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the minor version number of the GenICam-compliant camera's device description file (XML). The version number of the XML is structured as [`M_GC_XML_MAJOR`](../../Reference/dig/MdigInquire.md).[`M_GC_XML_MINOR`](../../Reference/dig/MdigInquire.md).[`M_GC_XML_SUBMINOR`](../../Reference/dig/MdigInquire.md).

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL | > **Note:** Note that this inquire type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the minor version number of the xml. |

---

### `M_GC_XML_PATH`

**Board availability:** GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the path and file name of the GenICam-compliant camera's device description file (XML).

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL | > **Note:** Note that this inquire type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer. |

| Value | Description |
| --- | --- |
| `"PathAndFilename"` | Specifies the path and file name of the XML configuration file. |

---

### `M_GC_XML_SUBMINOR`

**Board availability:** GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the subminor version number of the GenICam-compliant camera's device description file (XML). The version number of the XML is structured as [`M_GC_XML_MAJOR`](../../Reference/dig/MdigInquire.md).[`M_GC_XML_MINOR`](../../Reference/dig/MdigInquire.md).[`M_GC_XML_SUBMINOR`](../../Reference/dig/MdigInquire.md).

#### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL | > **Note:** Note that this inquire type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the subminor version number of the xml. |

---

### `M_GENTL_INTERFACE_INDEX`

**Board availability:** GenTL

Inquires the number of the GenTL interfaces associated to this digitizer.

| Value | Description |
| --- | --- |
| `0 <= Value <=33` | Specifies the total number of the GenICam GenTL camera interfaces. |

---

### `M_GENTL_STREAM_COUNT`

**Board availability:** GenTL

Inquires the number of GenTL streams associated with the digitizer.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of GenTL streams. |

---

### `M_GRAB_DIRECTION_X`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the horizontal grab direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REVERSE` | Specifies to grab from right to left, in the horizontal direction. |
| `M_FORWARD` | Specifies to grab from left to right, in the horizontal direction. |

---

### `M_GRAB_DIRECTION_Y`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the vertical grab direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REVERSE` | Specifies to grab from bottom to top, in the vertical direction. |
| `M_FORWARD` | Specifies to grab from top to bottom, in the vertical direction. |

---

### `M_GRAB_FIELD_NUM`

**Board availability:** Clarity UHD, GenTL, GigE Vision, USB3 Vision, V4L2

Inquires the number of fields grabbed with [`MdigGrab`](../../Reference/dig/MdigGrab.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `2` | Specifies to grab two fields. |
| `1` | Specifies to grab one field. |

---

### `M_GRAB_FRAME_MISSED`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the number of frames sent by the camera, but not received by the digitizer when using [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md) or [`MdigProcess`](../../Reference/dig/MdigProcess.md). Note that this counter resets after calling this function with this inquire, and after calling [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_FRAME_MISSED_RESET`](../../Reference/dig/MdigControl.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of frames missed. |

---

### `M_GRAB_FRAME_MISSED_COUNTER`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires whether to count the number of frames sent by the camera, but not received by the digitizer when performing a grab operation (that is, [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ENABLE` | Specifies to enable frames missed detection. |
| `M_DISABLE` | Specifies to disable frames missed detection. |

---

### `M_GRAB_IN_PROGRESS`

Inquires the current grab state, when either [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md) or [`MdigProcess`](../../Reference/dig/MdigProcess.md)is called.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that there is no grab in progress. Note that a grab might be queued. |
| `M_YES` | Specifies that there is a grab in progress. |

---

### `M_GRAB_LINE_COUNTER`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires whether a function hooked to an [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md), [`M_ROTARY_ENCODER`](../../Reference/dig/MdigHookFunction.md), or [`M_GRAB_FRAME_END`](../../Reference/dig/MdigHookFunction.md) event can inquire the number of lines grabbed, performed using [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the hooked function cannot inquire the number of lines grabbed. |
| `M_ENABLE` | Specifies that the hooked function can inquire the number of lines grabbed. |

---

### `M_GRAB_LUT_PALETTE`

**Board availability:** Iris GTX

Inquires which LUT to use.  Note that this inquire value is only supported on color cameras.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use a 3-band RGB LUT. |
| `M_GAMMA` | Specifies to use the native Bayer LUT that is applied to the Bayer image before it is converted to an RGB image. |

---

### `M_GRAB_MODE`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires how the grab should be synchronized with the Host when grabbing data with [`MdigGrab`](../../Reference/dig/MdigGrab.md).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ASYNCHRONOUS` | Specifies that your application continues after one grab is queued, rather than waiting for the grab to finish. |
| `M_ASYNCHRONOUS_QUEUED` | Specifies that your application continues after each grab is queued, rather than waiting for the grab to finish. |
| `M_SYNCHRONOUS` *(default)* | Specifies that your application is synchronized with the end of a grab operation (that is, your application waits for the grab to finish before returning from the grab function). |

---

### `M_GRAB_PERIOD`

Inquires the duration of a frame (as specified in the DCF).

| Value | Description |
| --- | --- |
| `0` | Specifies that the inquiry of the duration of a frame is not supported. **[GenTL, GigE Vision, USB3 Vision, V4L2]** |
| `Value > 0` | Specifies the duration of a frame, in msec. |

---

### `M_GRAB_SCALE_INTERPOLATION_MODE`

**Board availability:** Host System

Inquires the interpolation mode, when performing a vertical and/or horizontal scaling.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTOMATIC` | Specifies that the best interpolation mode is automatically selected, based on the input filter (set using [`M_INPUT_FILTER`](../../Reference/dig/MdigInquire.md)). |
| `M_BICUBIC` | Specifies bicubic interpolation. |
| `M_BILINEAR` | Specifies bilinear interpolation. |
| `M_NEAREST_NEIGHBOR` *(default)* | Specifies nearest neighbor interpolation. |

---

### `M_GRAB_SCALE_X`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the horizontal scaling factor when grabbing data.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_FILL_DESTINATION` | Specifies that the scaling factor is calculated and set to fill the destination buffer. |
| `1` | Specifies that no scaling is applied. |
| `Value = 1/n` | Specifies to reduce the image size. |

---

### `M_GRAB_SCALE_Y`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the vertical scaling factor when grabbing data.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_FILL_DESTINATION` | Specifies that the scaling factor is calculated and set to fill the destination buffer. |
| `1` | Specifies that no scaling is applied. |
| `Value = 1/n` | Specifies to reduce the image size. |

---

### `M_GRAB_START_MODE`

**Board availability:** Clarity UHD, Radient eV-CL, Rapixo CL, V4L2

Inquires the type of field on which to grab. Note that this value only applies when dealing with interlaced cameras.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIELD_START` | Specifies that the grab starts on any field. |
| `M_FIELD_START_ODD` *(default)* | Specifies that the grab starts on an odd field. |
| `M_FIELD_START_EVEN` | Specifies that the grab starts on an even field. |

---

### `M_GRAB_TIMEOUT`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the maximum time to wait for a frame before generating an error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default amount of time to wait. |
| `M_INFINITE` | Specifies to wait indefinitely. |
| `Value > 0` | Specifies the time to wait, in msec. |

---

### `M_HARDWARE_DEINTERLACING`

**Board availability:** Clarity UHD

Inquires whether hardware deinterlacing is used when the video source is interlaced.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BOB_METHOD` | Specifies to use the BOB algorithm for hardware deinterlacing. |
| `M_DISABLE` *(default)* | Specifies that hardware deinterlacing is disabled. |
| `M_MADI_METHOD` | Specifies to use the MADI (motion adaptive deinterlacing) algorithm for hardware deinterlacing. |

---

### `M_INPUT_FILTER`

**Board availability:** Clarity UHD

Inquires the low-pass filter applied to incoming data.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTOMATIC` | Specifies that the best hardware filter is automatically selected or the filter is bypassed when the video input is interlaced and hardware deinterlacing is disabled. |
| `M_BYPASS` *(default)* | Specifies to not use a filter. |
| `M_LOW_PASS_0` | Specifies to use the first low-pass filter. |
| `M_LOW_PASS_1` | Specifies to use the second low-pass filter. |

---

### `M_INPUT_MODE`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires whether the analog or digital input mode is used, (as specified in the DCF).

| Value | Description |
| --- | --- |
| `M_ANALOG` | Specifies analog input mode. **[Clarity UHD, Iris GTX]** |
| `M_DIGITAL` | Specifies digital input mode. **[Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |

---

### `M_LAST_GRAB_IN_TRUE_BUFFER`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires whether a monoshot grab is performed when [`MdigHalt`](../../Reference/dig/MdigHalt.md) is called after performing a continuous grab operation.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the last frame grabbed during the continuous grab operation is copied to the target image buffer, instead of performing one last grab. |
| `M_ENABLE` *(default)* | Specifies that a monoshot grab is performed. |

---

### `M_LIGHTING_BRIGHT_FIELD`

**Board availability:** Iris GTX

Inquires the intensity of light to emit parallel to the optical axis, making the flat parts of an object stand out in stark contrast in the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 255` *(default)* | Specifies the intensity of light to be emitted on the flat parts of the object. |

---

### `M_LUT_ID`

**Board availability:** Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires whether to map the input data through the physical LUT of the specified digitizer and the values with which to initialize the LUT.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the digitizer's physical LUT is not used. |
| `LUT buffer identifier` | Specifies the identifier of the LUT buffer (allocated with [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_LUT`](../../Reference/buf/MbufAlloc1d.md)) with which to initialize the digitizer's physical LUT. |

---

### `M_NUMBER`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the device number of the digitizer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default device number. |
| `M_GC_CAMERA_ID` | Specifies the camera (or device) to allocate for the digitizer. |
| `M_DEVn` | Specifies which acquisition path on your frame grabber, or which camera by rank when dealing with a grabberless interface camera (for example, a GigE Vision or USB3 Vision camera), to allocate for the digitizer. |

---

### `M_OWNER_SYSTEM`

Inquires the Aurora Imaging Library identifier of the system on which the digitizer has been allocated.

| Value | Description |
| --- | --- |
| `System identifier` | Specifies the Aurora Imaging Library identifier of the system. |

---

### `M_POWER_OVER_CABLE`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP

Inquires whether PoCL (power over Camera Link) or PoCXP (power over CoaXPress) are enabled.

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

### `M_PROCESS_FRAME_CORRUPTED`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, USB3 Vision, V4L2

Inquires the number of corrupted frames grabbed in the sequence with [`MdigProcess`](../../Reference/dig/MdigProcess.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of corrupted frames. |

---

### `M_PROCESS_FRAME_COUNT`

Inquires the number of frames grabbed in the sequence with [`MdigProcess`](../../Reference/dig/MdigProcess.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of grabbed frames. |

---

### `M_PROCESS_FRAME_MISSED`

Inquires the number of frames believed to be missed when grabbing with [`MdigProcess`](../../Reference/dig/MdigProcess.md).

#### System specific

| Board(s) | Note |
|---|---|
| Clarity UHD, Host System, Iris GTX, USB3 Vision | This value is based on a comparison between the frame rate ([`M_PROCESS_FRAME_RATE`](../../Reference/dig/MdigInquire.md)) and the frame count ([`M_PROCESS_FRAME_COUNT`](../../Reference/dig/MdigInquire.md)). If a grab is triggered from a variable trigger or if the hooked function is slow, this value will be affected. This value is most trustworthy with a camera in continuous mode. |
| GenTL, GigE Vision, V4L2 | This value is based on a comparison between the previous frame's block identifier and the current frame's block identifier. The identifier is incremented by 1 for each frame transmitted. Using the block identifier allows Aurora Imaging Library to detect missed frames even when the camera is triggered from a variable trigger or if the hooked function is slow. |
| Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF | This value is returned from your imaging board. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of missed frames. |

---

### `M_PROCESS_FRAME_RATE`

Inquires the rate at which the frames are grabbed using [`MdigProcess`](../../Reference/dig/MdigProcess.md). Note that this inquire type is determined based on the average of the camera's frame rate, and the total amount of time to process the user-defined hooked function(s).

| Value | Description |
| --- | --- |
| `Value` | Specifies the rate at which the frames are grabbed, in frames/sec. |

---

### `M_PROCESS_GRAB_MONITOR`

Inquires whether to create an internal grab monitoring thread when using [`MdigProcess`](../../Reference/dig/MdigProcess.md) with either [`M_START`](../../Reference/dig/MdigProcess.md) or [`M_SEQUENCE`](../../Reference/dig/MdigProcess.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Does not create a grab activity thread. |
| `M_ENABLE` | Creates a grab activity thread. **[Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |

---

### `M_PROCESS_PENDING_GRAB_NUM`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the number of buffers remaining in the list of buffers used by [`MdigProcess`](../../Reference/dig/MdigProcess.md). Note that this number includes the buffer used by the grab in progress. When dealing with round-robin grabbing, this number will typically be relatively close to the total number of allocated grab buffers.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of buffers remaining in the list of buffers. |

---

### `M_PROCESS_TIMEOUT`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the maximum amount of time to wait for [`MdigProcess`](../../Reference/dig/MdigProcess.md) to complete the current grab ([`M_STOP`](../../Reference/dig/MdigProcess.md)) or all the queued grabs ([`M_STOP`](../../Reference/dig/MdigProcess.md) + [`M_WAIT`](../../Reference/dig/MdigProcess.md)), before generating an error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies to wait indefinitely. |
| `Value >= 0` | Specifies the time to wait, in msec. |

---

### `M_PROCESS_TOTAL_BUFFER_NUM`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the total number of allocated grab buffers in the list of buffers before [`MdigProcess`](../../Reference/dig/MdigProcess.md) begins.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of allocated grab buffers. |

---

### `M_SCAN_MODE`

Inquires the scan mode.

| Value | Description |
| --- | --- |
| `M_INTERLACE` | Specifies interlace mode. **[Clarity UHD, GenTL, GigE Vision, USB3 Vision, V4L2]** |
| `M_LINESCAN` | Specifies line-scan mode. **[GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |
| `M_PROGRESSIVE` | Specifies progressive mode. |

---

### `M_SELECTED_FRAME_RATE`

Inquires the frame rate of the camera when grabbing in synchronous mode.

| Value | Description |
| --- | --- |
| `Value` | Specifies the frame rate to be used by the camera, in frames/sec. |

---

### `M_SERIAL_NUMBER`

**Board availability:** Iris GTX

Inquires the serial number of the connected device.

| Value | Description |
| --- | --- |
| `"String"` | Specifies the serial number of the connected device. |

---

### `M_SIGN`

Inquires whether the data is signed or unsigned.

| Value | Description |
| --- | --- |
| `M_SIGNED` | Specifies that the data is signed. **[GenTL, GevIQ, GigE Vision, Host System, USB3 Vision, V4L2]** |
| `M_UNSIGNED` | Specifies that the data is unsigned. |

---

### `M_SIZE_BAND`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the number of input color bands of the digitizer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of input color bands of the digitizer. |

---

### `M_SIZE_BAND_LUT`

**Board availability:** GenTL, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the number of input color bands of the LUT buffer associated with the digitizer.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of input color bands. |

---

### `M_SIZE_BIT`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the depth per band.

| Value | Description |
| --- | --- |
| `Value` | Specifies the depth per band, in bits. |

---

### `M_SIZE_X`

Inquires the width of the image.  When you specify the digitizer configuration format (DCF) appropriate for your camera, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`DataFormat`](../../Reference/dig/MdigAlloc.md), you are indirectly setting the size of the image that can be grabbed.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the image, in pixels. |

---

### `M_SIZE_Y`

Inquires the height of the image.  When you specify the digitizer configuration format (DCF) appropriate for your camera, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`DataFormat`](../../Reference/dig/MdigAlloc.md), you are indirectly setting the size of the image that can be grabbed.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the image, in pixels. |

---

### `M_SOURCE_DATA_FORMAT`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

Inquires the Aurora Imaging Library buffer format that is compatible with the camera's pixel format.

#### System specific

| Board(s) | Note |
|---|---|
| GenTL, GigE Vision, USB3 Vision, V4L2 | Note that this value returns the Aurora Imaging Library equivalent of the camera's current pixel format. The camera's pixel format typically varies from camera to camera. To change the camera's pixel format, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md). |
| Clarity UHD | Note that this value returns the default Aurora Imaging Library buffer format, set using the DCF. |

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format (color buffer only); that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes (RRR... GGG... BBB...). **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |

---

### `M_SOURCE_NUMBER_OF_FRAMES`

Inquires the number of frames that can be grabbed.

| Value | Description |
| --- | --- |
| `M_INFINITE` | Specifies an infinite number of frames. **[Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |
| `Value > 0` | Specifies the number of frames in the AVI file or folder specified when you allocated the digitizer. |

---

### `M_SOURCE_OFFSET_X`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the X-offset of the input-signal capture window.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset, in pixels. |

---

### `M_SOURCE_OFFSET_Y`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the Y-offset of the input-signal capture window.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset, in pixels. |

---

### `M_SOURCE_SIZE_X`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the width of the input signal capture window.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `Value` | Specifies the width, in pixels. |

---

### `M_SOURCE_SIZE_Y`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the height of the input signal capture window.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `Value` | Specifies the height, in pixels. |

---

### `M_TARGET_BUFFER_OBJECT`

Inquires whether the digitizer outputs data in a format suitable for grabbing into a buffer or a container.  > **Note:** Note that you can grab from any digitizer into either a buffer or a container. However, grabbing data suitable for a container into a buffer will result in only one of the components transmitted from the camera being grabbed. Grabbing data suitable for a buffer into a container is supported, but not typically necessary.  > **Note:** You can automatically allocate the Aurora Imaging Library object most suitable for grabbing from a specified digitizer using [`MbufAllocDefault`](../../Reference/buf/MbufAllocDefault.md).

| Value | Description |
| --- | --- |
| `M_CONTAINER` | Specifies that the digitizer outputs data in a format suitable for grabbing into a container. |
| `M_IMAGE` | Specifies that the digitizer outputs data in a format suitable for grabbing grabbing into a buffer. |

---

### `M_TL_TRIGGER_ACTIVATION`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the signal variation upon which to generate a trigger signal to the camera through the transport layer interface.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the camera's default value or, if supported, to enable the camera's auto control state for this feature(that is, the camera adjusts this value automatically). |
| `M_ANY_EDGE` | Specifies that a trigger will be generated both upon a high-to-low and a low-to-high signal transition. |
| `M_EDGE_RISING` | Specifies that a trigger will be generated upon a low-to-high signal transition. |

---

### `M_TYPE`

Inquires the data type and depth of the digitizer's input. Depth is returned in bits.

| Value | Description |
| --- | --- |
| `depth value + M_SIGNED` | Specifies the data depth and that the data is signed. **[Clarity UHD, GenTL, GigE Vision, Iris GTX, USB3 Vision, V4L2]** |
| `depth value + M_UNSIGNED` | Specifies the data depth and that the data is unsigned. |

---

### `M_WHITE_BALANCE`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires whether to perform white balancing.

#### System specific

| Board(s) | Note |
|---|---|
| GenTL, GigE Vision, USB3 Vision, V4L2 | Note that this inquire type is used to perform Bayer conversion if performed by the Host. To inquire the Bayer conversion on the camera, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md); refer to your camera's documentation for details regarding the features to inquire. |
| Iris GTX | Note that this inquire type is only available when using color versions of Zebra Iris GTX (such as 2000C). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CALCULATE` | Calculates new white balance coefficients, overwriting old coefficients. |
| `M_DISABLE` *(default)* | Specifies that white balancing is disabled. |
| `M_ENABLE` | Specifies that white balancing is enabled. |

### Combination Constants — For specifying further details

> *Optional, can be used alone.*

> **Usage:** You can use one or more of the following values on its own, or add it to the above-mentioned values, to specify further details of the allocation of your camera.

| Value | Description |
| --- | --- |
| `M_GC_PACKET_SIZE_NEGOTIATION_SKIP` | Specifies that the packet size negotiation task is skipped. **[GevIQ, GigE Vision]** |
| `M_GC_XML_DOWNLOAD_SKIP` | Specifies that the camera's device description file (XML) should not be downloaded from the camera associated with the digitizer. **[GevIQ, GigE Vision, USB3 Vision]** |
| `M_GC_XML_FORCE_DOWNLOAD` | Specifies that the camera's device description file (XML) should be downloaded from the camera associated with the digitizer. **[GevIQ, GigE Vision, USB3 Vision]** |

### Combination Constants — For specifying the multicasting controller-worker relationship

> *Optional, can be used alone.*

> **Usage:** You can use one of the following values on its own, or add it to the above-mentioned values, to specify whether the allocated digitizer will be a controller, monitor, or worker digitizer in a multicast controller-worker relationship.

For information about using IP multicast and configuring your Zebra GigE Vision digitizer, refer to Using IP multicast.

> **Board availability:** GigE Vision

| Value | Description |
| --- | --- |
| `M_GC_MULTICAST_CONTROLLER` | Specifies that the allocated digitizer will be the controller digitizer in a multicasting controller-worker or monitor relationship. |
| `M_GC_MULTICAST_MONITOR` | Specifies that the allocated digitizer will be a special type of worker digitizer (a monitor digitizer) in a multicast controller-worker relationship. |
| `M_GC_MULTICAST_WORKER` | Specifies that the allocated digitizer will be a worker digitizer in a multicast controller-worker relationship. |

### Combination Constants — Returns the format of the planar color buffer

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the format of the planar color buffer.

> **Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

| Value | Description |
| --- | --- |
| `M_MONO8` | Specifies 8-bit monochrome depth planar pixels. |
| `M_MONO16` | Specifies 16-bit monochrome depth planar pixels. |
| `M_MONO32` | Specifies 32-bit monochrome depth planar pixels. |
| `M_RGB24` | Specifies 24-bit color depth planar (RGB 8:8:8) pixels. |
| `M_RGB48` | Specifies 48-bit color depth planar (RGB 16:16:16) pixels. |
| `M_RGB96` | Specifies 96-bit color depth planar (RGB 32:32:32) pixels. |
| `M_YUV9` | Specifies YUV9 planar pixels. |
| `M_YUV16` | Specifies YUV16 planar (4:2:2) pixels. |
| `M_YUV16_UYVY` | Specifies YUV16 planar (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). |
| `M_YUV16_YUYV` | Specifies YUV16 planar (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). |
| `M_YUV411_1394` | Specifies YUV411 planar (6:6:6) pixels. |
| `M_YUV444` | Specifies YUV444 planar (3:3:3) pixels. |

### Combination Constants — Returns the format of the packed color buffer

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the format of the packed color buffer.

> **Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

| Value | Description |
| --- | --- |
| `M_BGR24` | Specifies 24-bit color depth packed (BGRBGR) pixels. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_BGR32` | Specifies 32-bit color depth packed (BGRXBGRX) pixels. The most-significant byte is a "don't care" byte. |
| `M_RGB15` | Specifies 16-bit color depth packed (XRGB 1:5:5:5) pixels. Note that when accessing this packed buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_RGB16` | Specifies 16-bit color depth packed (RGB 5:6:5) pixels. Note that when accessing this packed buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. **[Clarity UHD, GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_RGB24` | Specifies 24-bit color depth packed (RGB 8:8:8) pixels. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_RGB32` | Specifies 32-bit color depth packed (RGB 10:12:10) pixels. |
| `M_RGB48` | Specifies 48-bit color depth packed (RGB 16:16:16) pixels. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_RGB96` | Specifies 96-bit color depth packed (RGB 32:32:32) pixels. |
| `M_YUV16` | Specifies YUV16 packed (4:2:2) pixels. **[Clarity UHD, GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). |
| `M_YUV411_1394` | Specifies YUV411 packed (6:6:6) pixels. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |
| `M_YUV444` | Specifies YUV444 packed (3:3:3) pixels. **[GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2]** |

### Combination Constants — For specifying which Camera Link camera to inquire

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which Camera Link camera to inquire.

> **Board availability:** Radient eV-CL, Rapixo CL

| Value | Description |
| --- | --- |
| `0 <= Value <= 127` | Specifies which Camera Link camera to inquire. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

> **Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### For the connection testing

The following inquire types and inquire values relate to connection testing.

> **Board availability:** Rapixo CXP, Rapixo CoF

---

### `M_CONNECTION_COUNT`

Inquires the number of connections between the system and the camera. The number of connections corresponds to the number of CXP input connectors used to connect with the camera.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of connections between the system and the camera. |

---

### `M_CONNECTION_ID`

Inquires which CXP connector on the camera is connected to the CXP input connector _n_ on the board.

| Value | Description |
| --- | --- |
| `Value` | Specifies the ID. |

---

### `M_CONNECTION_STATE`

Inquires the state of the connection.

| Value | Description |
| --- | --- |
| `M_DETECTED` | Specifies that the connection is detected. |
| `M_UNDETECTED` | Specifies that the connection is undetected. |
| `M_UNKNOWN` | Specifies that the state of the connection is unknown. |

---

### `M_CONNECTION_TEST_ERROR_COUNT`

Inquires the current connection error count for test packets.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current connection error count for test packets. |

---

### `M_CONNECTION_TEST_MODE`

Inquires the state of CXP test mode.

| Value | Description |
| --- | --- |
| `M_MODE1` | Specifies that the CXP test mode is enabled. |
| `M_OFF` | Specifies that the CXP test mode is disabled. |

---

### `M_CONNECTION_TEST_PACKET_RECEIVED_COUNT`

Inquires the number of test packets received.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of test packets received. |

---

### `M_CONNECTION_TEST_PACKET_TRANSMITTED_COUNT`

Inquires the number of test packets transmitted.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of test packets transmitted. |

---

### `M_CONNECTION_TYPE`

Inquires the CXP connection type.  > **Note:** A camera typically has only one main connection.

| Value | Description |
| --- | --- |
| `M_EXTENSION` | Specifies that the CXP connection type is an extension connection. |
| `M_MAIN` | Specifies that the CXP connection type is a main connection. |

### Combination Constants — For specifying the CXP input connector

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the CXP input connector.

> **Board availability:** Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_CONNECTIONn` | Specifies the connection at CXP input connector _n_, where _n_ is the CXP input connector number from 0 to 3. |

### For the general reference settings

The following inquire types and inquire values relate to (if available) the reference levels used to digitize the analog signal received from a camera. These inquire types are specific to analog input devices. Depending on the type of digitizer and input signal, some reference types are not applicable. The smallest voltage increment supported by your board can differ such that consecutive reference-level settings might produce the same result. Note, some digitizers might take a few milliseconds before the reference level stabilizes.

---

### `M_BLACK_OFFSET`

**Board availability:** Iris GTX

Inquires the offset to the black level of the image in hardware.

| Value | Description |
| --- | --- |
| `0<= Value <= 255` | Specifies the value of the black offset. |

---

### `M_BLACK_REF`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision

Inquires the input signal's digitization black reference level.

| Value | Description |
| --- | --- |
| `Value` | Specifies the black reference level. |

---

### `M_BRIGHTNESS_REF`

**Board availability:** Clarity UHD

Inquires the brightness level for composite input signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the brightness reference value. |

---

### `M_CONTRAST_REF`

**Board availability:** Clarity UHD

Inquires the contrast level for composite input signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the contrast reference value. |

---

### `M_HUE_REF`

**Board availability:** Clarity UHD

Inquires the hue level for composite input signals. Note that this inquire type is not supported for monochrome cameras.

| Value | Description |
| --- | --- |
| `Value` | Specifies the digitizer hue reference level. |

---

### `M_SATURATION_REF`

**Board availability:** Clarity UHD

Inquires the saturation level for composite input signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the digitizer saturation reference level. |

---

### `M_WHITE_REF`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision

Inquires the input signal's digitization white reference level.

| Value | Description |
| --- | --- |
| `Value` | Specifies the digitizer's white reference value. |

### For inquiring the input gain and shading correction

The following inquire types and inquire values allow you to inquire the input gain and shading correction.

---

### `M_GAIN`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2

Inquires the input gain with which to amplify the input signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the camera's default value or, if supported, to enable the camera's auto control state for this feature (that is, the camera adjusts this value automatically). |
| `Min. gain <= Value <= Max. gain` | Specifies a value between the minimum and maximum values supported by the camera. |

---

### `M_GRAB_AUTOMATIC_INPUT_GAIN`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, USB3 Vision

Inquires whether the input gain should be automatically set.

#### System specific

| Board(s) | Note |
|---|---|
| Clarity UHD | This inquire type is only supported when the device number of the specified digitizer is [`M_DEV2`](../../Reference/dig/MdigInquire.md) or [`M_DEV3`](../../Reference/dig/MdigInquire.md). Note that this inquire type is available when the DCF specifies to grab either an analog or digital input signal. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the input gain is not automatically set. |
| `M_ENABLE` *(default)* | Specifies that the input gain is automatically set. |

---

### `M_GRAB_INPUT_GAIN`

**Board availability:** Clarity UHD

Inquires the input gain with which to amplify the input signal.  This inquire type is only supported when the device number of the digitizer is [`M_DEV2`](../../Reference/dig/MdigInquire.md) or [`M_DEV3`](../../Reference/dig/MdigInquire.md). Note that this inquire type is available when the DCF specifies to grab either an analog or digital input signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default input gain factor. |
| `M_MAX_LEVEL` | Specifies the maximum input gain factor. |
| `M_MIN_LEVEL` | Specifies the minimum input gain factor. |
| `0 <= Value <= 255` | Specifies an integer value which is used to determine the input gain factor. |

---

### `M_SHADING_CORRECTION`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires whether to perform a gain and offset correction per pixel (shading correction).

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | > **Note:** Note that this inquire type is only available on Zebra Rapixo CXP Pro and Zebra Rapixo CoF 100, and only if the loaded FPGA configuration contains the Zebra PU that supports this functionality. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Disables shading correction. |
| `M_ENABLE` | Enables shading correction. |

---

### `M_SHADING_CORRECTION_GAIN_FIXED_POINT`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the number of bits in the fractional part of the gain values specified for performing shading correction, where values are in fixed point format. The image resulting from the shading correction will be rounded to the nearest whole number.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | > **Note:** Note that this inquire type is only available on Zebra Rapixo CXP Pro and Zebra Rapixo CoF 100, and only if the loaded FPGA configuration contains the Zebra PU that supports this functionality. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `0 < Value <= 16` | Specifies the number of bits in the fractional part of the fixed-point gain values. |

---

### `M_SHADING_CORRECTION_GAIN_ID`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the buffer containing the gain values that your digitizer should use when it performs shading correction.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | > **Note:** Note that this inquire type is only available on Zebra Rapixo CXP Pro and Zebra Rapixo CoF 100, and only if the loaded FPGA configuration contains the Zebra PU that supports this functionality. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies not to apply a gain correction when performing shading correction. |
| `ShadingCorrectionGainID` | Specifies the identifier of an [`M_IMAGE`](../../Reference/buf/MbufAlloc1d.md) buffer containing the gain values. |

---

### `M_SHADING_CORRECTION_OFFSET_ID`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the buffer containing the offset values that your digitizer should use when it performs a shading correction.

#### System specific

| Board(s) | Note |
|---|---|
| Rapixo CXP, Rapixo CoF | > **Note:** Note that this inquire type is only available on Zebra Rapixo CXP Pro and Zebra Rapixo CoF 100, and only if the loaded FPGA configuration contains the Zebra PU that supports this functionality. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies not to apply an offset correction when performing shading correction. |
| `ShadingCorrectionOffsetID` | Specifies the identifier of an [`M_IMAGE`](../../Reference/buf/MbufAlloc1d.md) buffer containing the offset values. |

### For inquiring I/O signals and their mode

The following inquire types and inquire values allow you to inquire the mode and the purpose of your Zebra imaging board's I/O signals (such as, auxiliary, Camera Link control, and transport layer trigger signal). Once the format, routing, and mode are determined for an I/O signal, you can further inquire the I/O signal using the inquire types described in the following tables: [`GroupUserBits`](../../Reference/GroupUserBits.md), [`GroupTimerSignals`](../../Reference/GroupTimerSignals.md), [`GroupExposureSignals`](../../Reference/GroupExposureSignals.md), and [`GroupTriggerSignals`](../../Reference/GroupTriggerSignals.md).  > **Note:** Note that for all Aurora Imaging Library supported hardware that have I/O signals, but are not supported with the constants below, see [`MsysInquire`](../../Reference/sys/MsysInquire.md).

> **Board availability:** GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

---

### `M_AUX_IO_COUNT`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, USB3 Vision

Inquires the total number of auxiliary signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of auxiliary signals. |

---

### `M_AUX_IO_COUNT_IN`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, USB3 Vision

Inquires the total number of auxiliary input and bidirectional signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of auxiliary input and bidirectional signals. |

---

### `M_AUX_IO_COUNT_OUT`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, USB3 Vision

Inquires the total number of auxiliary output and bidirectional signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of auxiliary output and bidirectional signals. |

---

### `M_CC_IO_COUNT_OUT`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, USB3 Vision

Inquires the total number of camera control output signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of camera control output and bidirectional signals. |

---

### `M_IO_DEBOUNCE_TIME`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the amount of time that the specified auxiliary input signal is debounced. Note that a maximum of 4 inputs can have a debounce set.

| Value | Description |
| --- | --- |
| `0 <= Value <= 8300000` | Specifies the minimum amount of time to ignore any additional signal transitions after accepting a signal transition, in nsec. |

---

### `M_IO_FORMAT`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the type of transmitter/receiver enabled for the specified I/O signal.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the specified I/O signal is not available for use. |
| `M_LINK_SIGNAL` | Specifies to use the transport layer link trigger for the specified I/O signal. |
| `M_OPEN_DRAIN` | Specifies to use the open collector (open drain) transmitter/receiver for the specified I/O signal. |
| `M_RS422` | Specifies to use the RS-422 transmitter/receiver for the specified I/O signal. |
| `M_TRI_STATE` | Specifies to use the tri-state transmitter/receiver for the specified I/O signal. |
| `M_LVDS` | Specifies to use the LVDS transmitter/receiver for the I/O signal. |
| `M_OPTO` | Specifies to use the opto-coupled transmitter/receiver for the specified I/O signal. |
| `M_TTL` | Specifies to use the TTL transmitter/receiver for the specified I/O signal. |

---

### `M_IO_INTERRUPT_ACTIVATION`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the signal transition upon which to generate an interrupt, if interrupt generation has been enabled for the specified I/O signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the signal transition specified by the DCF. |
| `M_ANY_EDGE` | Specifies to generate an interrupt upon both a low-to-high and a high-to-low signal transition. |
| `M_EDGE_FALLING` | Specifies that an interrupt will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` | Specifies that an interrupt will be generated upon a low-to-high signal transition. |

---

### `M_IO_INTERRUPT_STATE`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires whether to generate an interrupt upon the specified transition of the I/O signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the transition specified by the DCF. |
| `M_DISABLE` | Specifies not to generate an interrupt. |
| `M_ENABLE` | Specifies to generate an interrupt. |

---

### `M_IO_MODE`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the mode of the specified I/O signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the mode specified by the DCF, or disabled when dealing with a tri-state I/O signal. |
| `M_INPUT` | Specifies that the signal is for input. |
| `M_OUTPUT` | Specifies that the signal is for output. |

---

### `M_IO_SOURCE`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the type of signal routed onto an output signal or onto a bidirectional signal set to output mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the type of signal specified by the DCF. |
| `M_AUX_IOn` | Specifies to reroute auxiliary input signal _n_ to the output signal, where _n_ is the number of the auxiliary input signal. |
| `M_DISABLE` | Specifies to not route any signal to the specified signal. |
| `M_LINK_TRIGGER_n_IN` | Specifies to route the transport layer link trigger input signal _n_ (from the camera) to the output signal, where _n_ is the number of the link trigger input signal. |
| `M_TIMERn` | Specifies to route the output of timer _n_, where _n_ is the number of timers available. |
| `M_TL_TRIGGER` | Specifies to use the transport layer trigger signal (input mode only). |
| `M_USER_BIT_CC_IOn` | Specifies to route the state of bit _n_ of the camera control static-user-output register, where _n_ is a value from 0 to 1. |
| `M_USER_BIT_TL_TRIGGER0` | Specifies to route the state of the bit of the TL trigger static-user-output register. |
| `M_USER_BITn` | Specifies to route the state of bit _n_ of the main static-user-output register, where _n_ is the bit number. |

---

### `M_IO_STATUS`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the status of the input signal or bidirectional signal set to input.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the input signal is disabled. |
| `M_OFF` | Specifies that the input signal is off. |
| `M_ON` | Specifies that the input signal is on. |
| `M_UNKNOWN` | Specifies that the input signal cannot be inquired with its current configuration. |

---

### `M_IO_STATUS_ALL`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the status of all available I/O signals. Note that if there are I/O signals that cannot be inquired, the bits representing those signals, in the bit-encoded value returned, are not necessarily valid; these bits should be ignored.

| Value | Description |
| --- | --- |
| `Value` | Specifies the bit-encoded value representing the status of all available and inquirable I/O signals. |

---

### `M_TL_TRIGGER_COUNT`

Inquires the total number of TL trigger signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of TL trigger signals. |

---

### `M_TL_TRIGGER_COUNT_IN`

Inquires the total number of input and I/O TL trigger signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of input and I/O TL trigger signals. |

---

### `M_TL_TRIGGER_COUNT_OUT`

Inquires the total number of output and I/O TL trigger signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of output and I/O TL trigger signals. |

### Combination Constants — For specifying the type and number of the I/O signal to inquire

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the type and number of the I/O signal to inquire.

These combination constants have certain restrictions; for more information, refer to the [`SignalSource`](../../Reference/dig/MdigControl.md) combination table in [`MdigControl`](../../Reference/dig/MdigControl.md).

> **Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to inquire auxiliary signal _n_, where _n_ is the signal number.      With GigE Vision, USB3 Vision, and GenTL,_n_ is a number from 0 to 31.

 Note that, when using this value with [`M_IO_SOURCE`](../../Reference/dig/MdigInquire.md), the auxiliary signal _n_ must be an output signal, or an I/O signal set to output (using [`M_IO_MODE`](../../Reference/dig/MdigInquire.md)). |
| `M_CC_IOn` | Specifies to inquire Camera Link camera control signal _n_, where _n_ is a value from 1 to 4. Note that this combination value is only available for [`M_IO_SOURCE`](../../Reference/dig/MdigInquire.md). For a list of the available camera control signals, see the _Connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board or Aurora Imaging Library driver. |
| `M_LINK_TRIGGER_n_IN` | Specifies to inquire the link trigger input signal _n_ (from the camera), where _n_ is the number of the link trigger input signal. Note that this combination value is only available for [`M_IO_FORMAT`](../../Reference/dig/MdigInquire.md), [`M_IO_INTERRUPT_ACTIVATION`](../../Reference/dig/MdigInquire.md), [`M_IO_MODE`](../../Reference/dig/MdigInquire.md), and [`M_IO_STATUS`](../../Reference/dig/MdigInquire.md). |
| `M_LINK_TRIGGER_n_OUT` | Specifies to inquire the transport layer link trigger output signal _n_ (to the camera), where _n_ is the number of the link trigger output signal.Note that this combination value is only available for [`M_IO_FORMAT`](../../Reference/dig/MdigInquire.md), [`M_IO_SOURCE`](../../Reference/dig/MdigInquire.md), [`M_IO_MODE`](../../Reference/dig/MdigInquire.md), and [`M_IO_STATUS`](../../Reference/dig/MdigInquire.md). |
| `M_TL_TRIGGER` | Specifies to inquire the transport layer trigger signal. The transport layer trigger signal is an embedded bidirectional signal from the physical transport layer connection of your camera. Typically, the TL trigger signal is reserved for trigger information and is sent with other control and data signals along the same cable. Note that this combination value is only available for [`M_IO_FORMAT`](../../Reference/dig/MdigInquire.md), [`M_IO_INTERRUPT_ACTIVATION`](../../Reference/dig/MdigInquire.md), [`M_IO_SOURCE`](../../Reference/dig/MdigInquire.md), [`M_IO_MODE`](../../Reference/dig/MdigInquire.md), and [`M_IO_STATUS`](../../Reference/dig/MdigInquire.md). |

### Combination Constants — For specifying the type of I/O signal to inquire

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the type of I/O signal to inquire.

> **Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

| Value | Description |
| --- | --- |
| `M_AUX_IO` | Specifies to inquire all auxiliary signals. |
| `M_CC_IO` | Specifies to inquire all Camera Link camera control signals. **[Radient eV-CL, Rapixo CL]** |

### For inquiring the state of specified user-bits in a static-user-output register

The following inquire types and inquire values allow you to inquire the bits in a static-user-output register. The user-bits are the bits associated with I/O signals set to output or output signals. For the correspondence between your Aurora Imaging Library user-bits and the signal names, see the _connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board.  > **Note:** Note that for other Zebra imaging boards that have user-bits, but are not supported with the constants below, see [`MsysInquire`](../../Reference/sys/MsysInquire.md).

> **Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

---

### `M_USER_BIT_COUNT`

Inquires the total number of bits in the static-user-output register.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of bits in the default static-user-output register. |

---

### `M_USER_BIT_STATE`

Inquires the state of the specified bit in a static-user-output register.

| Value | Description |
| --- | --- |
| `M_OFF` | Specifies that the specified bit is set to off. |
| `M_ON` | Specifies that the specified bit is set to on. |

---

### `M_USER_BIT_STATE_ALL`

Inquires the state of all the bits in a static-user-output register. To change the type of static-user-output register (that is, from the main static-user-output register to a the camera control static-user-output register), use one of the combination values documented below.

| Value | Description |
| --- | --- |
| `Value` | Specifies a bit-encoded value that establishes the value of all the bits of the specified static-user-output register. |

### Combination Constants — For inquiring the bit in the static-user-output register

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to determine the bit in the static-user-output register.

> **Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

| Value | Description |
| --- | --- |
| `M_USER_BIT_CC_IOn` | Specifies which bit of the camera control static-user-output register to inquire, where _n_ is a value from 0 to 1. |
| `M_USER_BIT_TL_TRIGGER0` | Specifies to inquire the bit of the TL trigger static-user-output register. |
| `M_USER_BITn` | Specifies to inquire bit _n_ of the main static-user-output register. |

### Combination Constants — For inquiring the static-user-output register type to inquire

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the static-user-output register to inquire, if you don't want to inquire the main static-user-output register.

| Value | Description |
| --- | --- |
| `M_USER_BIT` *(default)* | Specifies to inquire all the bits in the main static-user-output register. |
| `M_USER_BIT_CC_IO` | Specifies to inquire all the bits in the static-user-output register associated with Camera Link camera control signals (the camera control static-user-output register). |
| `M_USER_BIT_TL_TRIGGER` | Specifies to inquire all the bits in the static-user-output register associated with TL trigger signals (the TL trigger static-user-output register). |

### For inquiring the settings to grab using a trigger

The following inquire types and inquire values specify the settings for inquiring triggers. For more information, see [Grabbing with triggers](../../UserGuide/C27_Grabbing_with_your_digitizer/S11_Grabbing_with_triggers_and_exposures.md).

---

### `M_GRAB_CONTINUOUS_END_TRIGGER`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires whether an automatic trigger is generated after [`MdigHalt`](../../Reference/dig/MdigHalt.md) is issued when performing a triggered continuous grab.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_DISABLE` | Specifies not to generate the trigger automatically. |
| `M_ENABLE` | Specifies to generate the trigger automatically. |

---

### `M_GRAB_TRIGGER_ACTIVATION`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the signal transition upon which to generate a grab trigger.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the signal transition specified by the DCF. |
| `M_ANY_EDGE` | Specifies that a trigger will be generated upon both a low-to-high and a high-to-low signal transition. |
| `M_EDGE_FALLING` | Specifies that a trigger will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` | Specifies that a trigger will be generated upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a trigger is continuously issued during a high signal polarity. |
| `M_LEVEL_HIGH_END_WHEN_INACTIVE` | Specifies that triggered grabs are initiated by a high trigger signal polarity, and completed when the trigger signal goes low. |
| `M_LEVEL_LOW` | Specifies that a trigger is continuously issued during a low signal polarity. |
| `M_LEVEL_LOW_END_WHEN_INACTIVE` | Specifies that triggered grabs are initiated by a low trigger signal polarity, and completed when the trigger signal goes high. |

---

### `M_GRAB_TRIGGER_DELAY`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

Inquires the delay between the trigger and the grab.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the delay, in nsec. |

---

### `M_GRAB_TRIGGER_MISSED`

**Board availability:** Iris GTX

Inquires whether the number of grab triggers missed should be counted.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to not count the number of grab triggers missed. |
| `M_ENABLE` | Specifies to count the number of grab triggers missed. |

---

### `M_GRAB_TRIGGER_OVERLAP`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires how a new grab trigger will be dealt with when the current grab is in progress.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that a triggered grab will not overlap the transfer of the previous image. |
| `M_ENABLE` *(default)* | Specifies that a triggered grab can overlap the transfer of the previous image. |
| `M_OFF` | Specifies that a new trigger is ignored. |
| `M_PREVIOUS_FRAME` | Specifies that a trigger received, while a grab is in progress, will be latched (stored). |
| `M_RESET` *(default)* | Specifies that the current grab will be immediately stopped and a new grab will be started. |

---

### `M_GRAB_TRIGGER_SOURCE`

Inquires the source for the grab trigger.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the one specified by the DCF. |
| `M_NULL` | Specifies that there is no trigger source. |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal. |
| `M_HSYNC` | Specifies to use the horizontal synchronization signal of the camera as the trigger source. |
| `M_IO_COMMAND_LIST1` | Specifies to use a bit of the I/O command register of I/O command list 1 as a trigger source. |
| `M_ROTARY_ENCODER` | Specifies to use the output of the default rotary decoder (set with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigInquire.md)) as the trigger source. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the trigger source, where _n_ is a number between 1 and 4. |
| `M_SOFTWAREn` | Specifies to use the software trigger being used for the grab of another digitizer (on the same board) as the trigger source. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source. |
| `M_VSYNC` | Specifies to use the vertical synchronization signal of the camera as the trigger source. |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. Use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_TRIGGER_SOFTWARE`](../../Reference/dig/MdigControl.md) to issue the trigger. |

---

### `M_GRAB_TRIGGER_STATE`

Inquires whether, when a grab command is issued (for example, [`MdigGrab`](../../Reference/dig/MdigGrab.md)), to wait for a trigger before grabbing.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)). |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the state specified by the DCF or, if none specified, [`M_DISABLE`](../../Reference/dig/MdigInquire.md). |
| `M_ENABLE` | Specifies that, when a grab command is issued, the grab waits for a trigger before occurring. |
| `M_DISABLE` | Specifies that, when a grab command is issued, the grab occurs without waiting for a trigger. |

### Combination Constants — For specifying which I/O command register bit to use

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which I/O command register bit to use.

> **Board availability:** Iris GTX

| Value | Description |
| --- | --- |
| `M_IO_COMMAND_BITn` | Specifies I/O command register bit _n_, where _n_ represents the bit number. |

### For inquiring frame burst settings

The following inquire types allow you to inquire the settings for a grab of sequential frames into a multi-frame image buffer (frame burst).

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

---

### `M_GRAB_FRAME_BURST_END_TRIGGER_SOURCE`

Inquires which signal causes the end of a grab of sequential frames into a multi-frame image buffer (end-of-frame-burst trigger).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_AUX_IO0`](../../Reference/dig/MdigInquire.md). |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal. |

---

### `M_GRAB_FRAME_BURST_END_TRIGGER_STATE`

Inquires whether a grab into a multi-frame image buffer is ended upon an end-of-frame-burst trigger.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_DISABLE`](../../Reference/dig/MdigInquire.md). |
| `M_DISABLE` | Specifies that an end-of frame-burst trigger is not used to end the grab. |
| `M_ENABLE` | Specifies that an end-of frame-burst trigger is used to end the grab. |

---

### `M_GRAB_FRAME_BURST_MAX_TIME`

Inquires the maximum amount of time to wait for all the frames to be grabbed into the multi-frame buffer. The timer starts when the first frame is grabbed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies to wait indefinitely. |
| `0.000008 <= Value <= 1.000000` | Specifies the maximum amount of time to wait, in secs. |

---

### `M_GRAB_FRAME_BURST_SIZE`

Inquires the number of sequential frames to grab into a multi-frame buffer with one grab command ([`MdigGrab`](../../Reference/dig/MdigGrab.md), or one grab of [`MdigProcess`](../../Reference/dig/MdigProcess.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 4095` *(default)* | Specifies the number of frames to grab. |

### For inquiring the settings of a timer

The following inquire types and inquire values specify the settings for inquiring timers and the signals generated from a timer (timer output signals). For more information, see [Grabbing with triggers](../../UserGuide/C27_Grabbing_with_your_digitizer/S11_Grabbing_with_triggers_and_exposures.md).  To specify which timer to inquire, see the [`TimerCombination`](../../Reference/TimerCombination.md) combination value below.

> **Board availability:** GenTL, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

---

### `M_TIMER_ARM`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the state of the timer arming mechanism.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that timer arming is disabled. |
| `M_ENABLE` | Specifies that timer arming is enabled. |
| `M_ENABLE` | Specifies to enable the timer arming mechanism. |

---

### `M_TIMER_ARM_ACTIVATION`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the signal transition upon which to arm the timer, if the timer arming mechanism is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE_FALLING` | Specifies that the timer will be armed upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the timer will be armed upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a timer is continuously armed during a high signal polarity. |
| `M_LEVEL_LOW` | Specifies that a timer is continuously armed during a low signal polarity. |

---

### `M_TIMER_ARM_SOURCE`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires which input signal will arm the timer, if the timer arming mechanism is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the input source set in the DCF. |
| `M_NULL` | Specifies to use no trigger source. |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source for the specified timer, where _n_ is the number of the auxiliary signal. |
| `M_CONTINUOUS` | Specifies to automatically arm the specified timer immediately after the timer's duration expires. |
| `M_EXPOSURE_END` | Specifies to use the exposure end signal as the trigger source. |
| `M_EXPOSURE_START` | Specifies to use the exposure start signal as the trigger source. |
| `M_GRAB_TRIGGER` | Specifies to use the grab trigger source signal as the trigger source. |
| `M_HSYNC` | Specifies to use the horizontal synchronization signal as the trigger source. |
| `M_ROTARY_ENCODER` | Specifies to use the output of the default rotary decoder (set with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigInquire.md)) as the trigger source. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder n, where _n_ is a number between 1 and 4. |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the timer number. |
| `M_TL_TRIGGER` | Specifies to use the transport layer trigger signal. |
| `M_VSYNC` | Specifies to use the vertical synchronization signal as the trigger source. |
| `M_NULL` | Specifies to use no trigger source. |

---

### `M_TIMER_CLOCK_FREQUENCY`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the frequency of the clock that drives the specified timer.

| Value | Description |
| --- | --- |
| `0` | Specifies that the value cannot be inquired. |
| `Value` | Specifies the frequency of the clock source, in Hz. |

---

### `M_TIMER_CLOCK_SOURCE`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the source of the clock that drives the specified timer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the most appropriate clock source (as determined by the driver). |
| `M_HSYNC` | Specifies to use the horizontal synchronization frequency of your camera. |
| `M_PIXCLK` | Specifies to use the pixel clock frequency of your camera. |
| `M_SYSCLK` | Specifies to use the frequency of the allocated system's clock source. |
| `M_TIMERn` | Specifies to use the frequency of the output of the specified timer, where _n_ is the timer number. |
| `M_VSYNC` | Specifies to use the vertical synchronization frequency of your camera. |

---

### `M_TIMER_DELAY`

Inquires the delay between the timer trigger and the active portion of the timer output signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `0` | Specifies that there is no delay. |
| `Value > 0` | Specifies the delay, in nsec. |

---

### `M_TIMER_DELAY2`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the delay between the end of the first active portion of the timer output signal and the start of the second pulse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `Value > 0` | Specifies the delay, in nsec. |

---

### `M_TIMER_DURATION`

Inquires the duration for the active portion of the timer output signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the duration specified by the DCF. |
| `Value > 0` | Specifies the duration of the active portion of the timer output signal, in nsec. |

---

### `M_TIMER_DURATION2`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the duration for the active portion of the second pulse of the timer output signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `Value > 0` | Specifies the duration of the active portion of the second pulse of the timer output signal, in nsecs. |

---

### `M_TIMER_OUTPUT_INVERTER`

**Board availability:** Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires whether the output of the timer is inverted.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value. |
| `M_DISABLE` | Specifies not to invert the output of the timer. |
| `M_ENABLE` | Specifies to invert the output of the timer. |

---

### `M_TIMER_RESET_SOURCE`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the signal source to use to reset the timer to 0.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the trigger source set in the DCF. |
| `M_NULL` | Specifies to use no trigger source. |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal. |
| `M_EXPOSURE_END` | Specifies to use the exposure end signal as the trigger source. |
| `M_EXPOSURE_START` | Specifies to use the exposure start signal as the trigger source. |
| `M_GRAB_TRIGGER` | Specifies to use the grab trigger source signal as the trigger source. |
| `M_HSYNC` | Specifies to use the horizontal synchronization signal as the trigger source. |
| `M_ROTARY_ENCODER` | Specifies to use the output of the default rotary decoder (set with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigInquire.md)) as the trigger source. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder n, where _n_ is a number between 1 and 4. |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the timer number. |
| `M_TL_TRIGGER` | Specifies to use the transport layer trigger signal. |
| `M_VSYNC` | Specifies to use the vertical synchronization signal as the trigger source. |
| `M_NULL` | Specifies to use no trigger source. |

---

### `M_TIMER_STATE`

Inquires the state of the specified timer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_DISABLE` | Specifies that the timer is disabled. |
| `M_ENABLE` | Specifies that the timer is enabled. |

---

### `M_TIMER_TRIGGER_ACTIVATION`

**Board availability:** GenTL, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

Inquires the signal variation upon which to generate a timer trigger, when the specified timer is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value. |
| `M_ANY_EDGE` | Specifies that a timer trigger will be generated both upon a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that a timer trigger will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` | Specifies that a timer trigger will be generated upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a timer trigger is continuously issued during a high signal polarity. |
| `M_LEVEL_LOW` | Specifies that a timer trigger is continuously issued during a low signal polarity. |

---

### `M_TIMER_TRIGGER_MISSED`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires whether to count the number of trigger pulses missed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to count the number of trigger pulses missed. |
| `M_ENABLE` | Specifies to count the number of trigger pulses missed. |

---

### `M_TIMER_TRIGGER_OVERLAP`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires how to deal with a new trigger that occurs while the associated timer has not yet expired (both its delay and duration). This is applied to both the delay and duration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LATCH` | Specifies that a trigger received, while the associated timer has not expired, will be latched (stored). |
| `M_OFF` | Specifies that a new trigger is ignored. |
| `M_RESET` *(default)* | Specifies that a new trigger automatically resets the timer (regardless of whether it is in its delay or active period) and then restarts the timer. |

---

### `M_TIMER_TRIGGER_RATE_DIVIDER`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the frequency with which to accept trigger pulses.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 255` | Specifies the frequency with which to accept a trigger out of a series of trigger pulses. |

---

### `M_TIMER_TRIGGER_SOURCE`

Inquires the trigger source for the specified timer output signal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as the one specified by the DCF. |
| `M_NULL` | Specifies that no trigger source is specified. |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source for the specified timer, where _n_ is the number of the auxiliary signal. |
| `M_CONTINUOUS` | Specifies to run the specified timer in periodic mode; no actual trigger signal is used. |
| `M_EXPOSURE_END` | Specifies to use the exposure end signal as the trigger source. |
| `M_EXPOSURE_START` | Specifies to use the exposure start signal as the trigger source. |
| `M_GRAB_TRIGGER` | Specifies to use the grab trigger source signal as the trigger source. |
| `M_HSYNC` | Specifies to use the horizontal synchronization signal as the trigger source. |
| `M_ROTARY_ENCODER` | Specifies to use the output of the default rotary decoder (set with [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigInquire.md)) as the trigger source. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder n, where _n_ is a number between 1 and 4. |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the timer number. |
| `M_TL_TRIGGER` | Specifies to use the transport layer trigger signal. |
| `M_VSYNC` | Specifies to use the vertical synchronization signal as the trigger source. |
| `M_NULL` | Specifies that there is no timer trigger source. |

### For inquiring the camera's exposure

The following inquire types and inquire values specify the settings for inquiring the camera's exposure. For more information, see the Hardware-specific Notes for your Zebra Imaging board.

> **Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2

---

### `M_AUTO_EXPOSURE`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

Inquires the automatic exposure mode of the camera.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the camera's default values or, if supported, to enable the camera's auto control state for the gain, iris, and shutter features (that is, the camera adjusts these values automatically). |
| `M_AUTOMATIC` | Specifies that the camera's exposure duration is constantly adapted by the device to maximize its effect. |
| `M_DISABLE` | Specifies that the camera will not control the gain, iris, and shutter settings automatically. |
| `M_ENABLE` | Specifies that the camera controls the gain, iris, and shutter settings automatically. |

---

### `M_EXPOSURE_DELAY`

**Board availability:** Iris GTX

Inquires the required delay between the exposure trigger and the start of your camera's image sensor being exposed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the delay, in nsec. |

---

### `M_EXPOSURE_MODE`

**Board availability:** Iris GTX

Inquires whether the exposure duration is set using an exposure timer or using the width of the trigger signal.

| Value | Description |
| --- | --- |
| `M_TIMED` | Specifies to use [`M_EXPOSURE_TIME`](../../Reference/dig/MdigInquire.md) to control the length of the exposure. |
| `M_TRIGGER_WIDTH` | Specifies to use duration of the active portion of the grab trigger's output signal to control the length of the exposure. |

---

### `M_EXPOSURE_TIME`

**Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2

Inquires the amount of time to expose the camera's image sensor.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the camera's default exposure time, as specified by the DCF. |
| `Value >= 0` | Specifies the amount of time to expose the camera's image sensor, in nsec. |

### Combination Constants — For determining the largest or smallest values supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the largest or smallest value supported.

> **Board availability:** GenTL, GevIQ, GigE Vision, Iris GTX, USB3 Vision, V4L2

| Value | Description |
| --- | --- |
| `M_MAX_VALUE` | Inquires the largest value supported. |
| `M_MIN_VALUE` | Inquires the smallest value supported. |

### For inquiring the settings of a rotary decoder

The following inquire types and inquire values allow you to inquire the settings of a rotary decoder.

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

---

### `M_ROTARY_ENCODER_BIT0_SOURCE`

Inquires the auxiliary input signal on which to receive bit 0 of the 2-bit Gray code.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as DCF. |
| `M_AUX_IOn` | Specifies the auxiliary input signal to use. |

---

### `M_ROTARY_ENCODER_BIT1_SOURCE`

Inquires the auxiliary input signal on which to receive bit 1 of the 2-bit Gray code.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as DCF. |
| `M_AUX_IOn` | Specifies the auxiliary input signal to use. |

---

### `M_ROTARY_ENCODER_DIRECTION`

Inquires the direction of movement occurring when the Gray code sequence received by the rotary decoder is 00 - 01 - 11 - 10; this essentially establishes if the counter increments or decrements when receiving this sequence. A forward direction increments; while a backward direction decrements.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as DCF. |
| `M_BACKWARD` | Specifies a backward direction of movement and to decrement the counter when the Gray code sequence is 00 - 01 - 11 - 10. |
| `M_FORWARD` | Specifies a forward direction of movement and to increment the counter when the Gray code sequence is 00 - 01 - 11 - 10. |

---

### `M_ROTARY_ENCODER_FORCE_VALUE_SOURCE`

Inquires the signal source to use to set the rotary decoder's counter to 0xFFFFFFFF.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the source set in the DCF. |
| `M_NULL` | Specifies not to set the counter to 0xFFFFFFFF upon a signal. |
| `M_AUX_IOn` | Specifies to use an auxiliary input signal or an auxiliary bidirectional signal set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md)). |
| `M_COUNTER_OVERFLOW` | Specifies to set the counter to 0xFFFFFFFF and keep it at this value after the counter increments past 0x7FFFFFFF. |
| `M_POSITION_TRIGGER` | Specifies to use the trigger signal generated by the rotary decoder when the counter reaches the value specified with [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md). |
| `M_STEP_BACKWARD_WHILE_POSITIVE` | Specifies to set the counter to 0xFFFFFFFF upon a decrement, only if the counter value is in the range of 0x0 to 0x7FFFFFFF before the decrement occurs; when interpreting the counter value as signed, this would be the positive counter value range. |
| `M_TL_TRIGGER` | Specifies to use a transport layer trigger input signal. |

---

### `M_ROTARY_ENCODER_FRAME_END_POSITION`

Inquires the value of the rotary decoder's counter at the end of the last grab. To retrieve this value, you must have enabled the rotary decoder to store this value (before the grab), using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_FRAME_END_READ`](../../Reference/dig/MdigControl.md). To retrieve the counter value at the end of the last grab from a hooked function, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_ROTARY_ENCODER_FRAME_END_POSITION`](../../Reference/dig/MdigGetHookInfo.md).

| Value | Description |
| --- | --- |
| `0 <= Value <= 429496295` | Specifies the value of the rotary decoder's counter at the end of the last grab. |

---

### `M_ROTARY_ENCODER_FRAME_END_READ`

Inquires if the rotary decoder is enabled to store the counter value at the end of the last grab or frame interrupt. If enabled, the value of the counter can be inquired at anytime using [`M_ROTARY_ENCODER_FRAME_END_POSITION`](../../Reference/dig/MdigInquire.md), or retrieved at the end of the last grab from a hooked function, using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_ROTARY_ENCODER_FRAME_END_POSITION`](../../Reference/dig/MdigGetHookInfo.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the counter value will not be available to be inquired at the end of the last frame grabbed. |
| `M_ENABLE` | Specifies that the rotary decoder is enabled to store the counter value at the end of the last grabbed frame, so that it can be inquired. |

---

### `M_ROTARY_ENCODER_MULTIPLIER`

Inquires the multiplying factor applied to each increment/decrement of the rotary decoder's counter for every rotary encoder step (change in position); this in turn applies a multiplying factor to the number of pulses that the rotary decoder outputs per step.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 < Value <= 4096` *(default)* | Specifies the multiplying factor to use. |

---

### `M_ROTARY_ENCODER_OUTPUT_MODE`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the rotary decoder's counter value and/or the direction of movement upon which the rotary decoder should output a pulse. The pulse can be used to trigger a timer or a grab.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as DCF. |
| `M_POSITION_TRIGGER` | Specifies to output a pulse upon the trigger generated by [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md). |
| `M_POSITION_TRIGGER_MULTIPLE` | Specifies to start triggering only when the counter value set with [`M_ROTARY_ENCODER_POSITION_START_TRIGGER`](../../Reference/dig/MdigControl.md) is reached and then to generate a trigger at a set interval (change) in counter value. |
| `M_STEP_ANY` | Specifies to output a pulse upon any change in the rotary decoder's counter value (position change in any direction). |
| `M_STEP_ANY_WHILE_POSITIVE` | Specifies to output a pulse upon any change in the rotary decoder's counter value (position change in any direction), only if the counter value is in the range of 0x0 to 0x7FFFFFFF before the increment or decrement occurs; when interpreting the counter value as signed, this would be the positive counter value range. |
| `M_STEP_FORWARD` | Specifies to output a pulse upon a rotary decoder counter increment only. |
| `M_STEP_FORWARD_WHILE_POSITIVE` | Specifies to output a pulse upon a rotary decoder counter increment, only if the counter value is in the range of 0x0 to 0x7FFFFFFF before the increment occurs; when interpreting the counter value as signed, this would be the positive counter value range. |

---

### `M_ROTARY_ENCODER_POSITION`

Inquires the current value of the rotary decoder's counter.

| Value | Description |
| --- | --- |
| `0 <= Value <= 429497295` | Specifies the current value of the counter. |

---

### `M_ROTARY_ENCODER_POSITION_START_TRIGGER`

Inquires the rotary decoder's counter value upon which to start generating triggers, when [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigInquire.md) is set to [`M_POSITION_TRIGGER_MULTIPLE`](../../Reference/dig/MdigInquire.md). If it is not set to this value, then [`M_ROTARY_ENCODER_POSITION_START_TRIGGER`](../../Reference/dig/MdigInquire.md) is ignored.

| Value | Description |
| --- | --- |
| `0 <= Value <= 0xFFFFFFFF` | Specified the value of the counter upon which to start generating triggers. |

---

### `M_ROTARY_ENCODER_POSITION_TRIGGER`

Inquires the rotary decoder's counter value upon which a trigger is generated.

| Value | Description |
| --- | --- |
| `0 <= Value <= 0xFFFFFFFF` | Specifies the value of the counter upon which a trigger is generated. If a value beyond the supported range is specified, an error is generated. If you are treating the counter values as a signed range of values (for example, forcing the counter to reset to 0 at 0x80000000) and you want to generate a trigger upon a negative value, specify the equivalent value in the range of 0x80000000 to 0xFFFFFFFF. If [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_POSITION_TRIGGER_MULTIPLE`](../../Reference/dig/MdigInquire.md), this will specify the change in counter value at which to generate a trigger, beginning at the counter value specified by [`M_ROTARY_ENCODER_POSITION_START_TRIGGER`](../../Reference/dig/MdigControl.md).If [`M_ROTARY_ENCODER_OUTPUT_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_POSITION_TRIGGER`](../../Reference/dig/MdigInquire.md), this will specify the counter value at which to generate a trigger each time this value is reached.If [`M_POSITION_TRIGGER`](../../Reference/dig/MdigInquire.md) is set to any other control value, this value will be ignored. |

---

### `M_ROTARY_ENCODER_RESET_SOURCE`

Inquires the signal source to use to reset the rotary decoder's counter to 0.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the source set in the DCF. |
| `M_NULL` | Specifies not to reset using a hardware signal source. |
| `M_AUX_IOn` | Specifies to use an auxiliary input signal or an auxiliary bidirectional signal set to input (using [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to [`M_INPUT`](../../Reference/dig/MdigControl.md)). |
| `M_POSITION_TRIGGER` | Specifies to use the trigger signal generated by the rotary decoder when the counter reaches the value specified with [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md). |
| `M_TL_TRIGGER` | Specifies to use a transport layer trigger input signal. |

### Combination Constants — For specifying which rotary decoder to inquire

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify which rotary decoder to inquire.

> **Board availability:** Radient eV-CL, Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_ROTARY_ENCODERn` | Specifies to inquire rotary decoder n, where _n_ is a number between 1 and 4. |

### For inquiring a data latch

The following inquire types allow you to inquire the settings for one of the data latches of your installed Aurora Imaging Library board. Data latches store information specific to a grabbed frame when the latch is triggered. To retrieve information from a data latch, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_DATA_LATCH...`](../../Reference/dig/MdigGetHookInfo.md). Note that data latch information is only available when [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) is called from a function hooked to an end-of-frame event using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with [`M_GRAB_FRAME_END`](../../Reference/dig/MdigHookFunction.md) or from the callback function (hooked using [`MdigProcess`](../../Reference/dig/MdigProcess.md)).

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

---

### `M_DATA_LATCH_CLOCK_FREQUENCY`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the frequency of the clock when retrieving a timestamp in a data latch.

| Value | Description |
| --- | --- |
| `Value` | Specifies the frequency of the clock when retrieving a timestamp in a data latch, in Hz. |

---

### `M_DATA_LATCH_MODE`

Inquires whether to latch data from the point when the first grab is queued until the start of the first frame of a series of queued grabs or grab sequence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NORMAL` *(default)* | Specifies not to latch before the start of the first grabbed frame. |
| `M_PREFETCH` | Specifies to latch data before the start of the first grabbed frame. |

---

### `M_DATA_LATCH_STATE`

Inquires the state of the specified data latch.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the data latch is disabled. |
| `M_ENABLE` | Specifies that the data latch is enabled. |

---

### `M_DATA_LATCH_TRIGGER_ACTIVATION`

Inquires the trigger signal transition upon which to store the specified information to the specified data latch.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE_FALLING` | Specifies to store the specified information to the data latch upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies to store the specified information to the data latch upon a low-to-high signal transition. |

---

### `M_DATA_LATCH_TRIGGER_SOURCE`

Inquires what triggers storing the specified information to the specified data latch.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUX_IOn` | Specifies to use auxiliary signal _n_ as the trigger source, where _n_ is the auxiliary signal number. |
| `M_GRAB_FRAME_END` *(default)* | Specifies to trigger the data latch on the event that occurs at the end of each grabbed frame; this event occurs when the frame grabber receives the last pixels (or packet) of a frame from the camera. |
| `M_GRAB_FRAME_START` | Specifies to trigger the data latch on the event that occurs at the start of each grabbed frame. |
| `M_GRAB_LINE` | Specifies to trigger the data latch on the event that occurs at the end of each grabbed line; this event occurs when the frame grabber receives the last pixels (or packet) of a line from the camera. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the trigger source, where _n_ is the rotary decoder number. |
| `M_TIMER_ACTIVE` | Specifies to trigger the data latch on the event that occurs when the specified timer is active. |

---

### `M_DATA_LATCH_TYPE`

Inquires the type of information to store to the specified data latch.

| Value | Description |
| --- | --- |
| `M_IO_STATUS_ALL` | Stores the status of all the auxiliary I/O signals. |
| `M_ROTARY_ENCODERn` | Stores the value of the counter of rotary decoder _n_, where _n_ is a number between 1 and 4. |
| `M_TIME_STAMP` | Stores the timestamp upon which the data latch is triggered, in ticks. |

### Combination Constants — For specifying the data latch to inquire

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which data latch to inquire about.

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_LATCHn` | Specifies which data latch to affect, where n is a value from 0 to 15. |

### Combination Constants — For specifying which on-board timer to inquire

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify which on-board timer to inquire.

> **Board availability:** GenTL, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision

| Value | Description |
| --- | --- |
| `M_TIMER_STROBE` | Specifies the strobe timer. |
| `M_TIMERn` | Specifies on-board timer _n_, where _n_ is one of the timers available on your Zebra imaging board or the hardware associated with your Aurora Imaging Library driver. |

### For inquiring your GigE Vision camera's optional settings

The following inquire types allow you to inquire the settings of your GigE Vision camera. These settings are part of the GigE Vision Control Protocol (GVCP), described in the GigE Vision Specification version 2.0. The GVCP details GigE Vision specific settings to send and receive information across your network, as well as store information on your GigE Vision camera. Each inquire type specifies a family of capabilities within your GigE Vision device, where your camera is capable of using one or more features (although, each of these features is not necessarily enabled or configured). Note that, for these inquire types to be available, your GigE Vision camera must support one or more GigE Vision Control Protocol (GVCP) commands. Each of these inquire types returns a bitwise string, whose potential values are presented as inquire values. The inquire values are listed alphabetically; the bitwise order might differ.

> **Board availability:** GevIQ, GigE Vision

---

### `M_GC_CONTROL_PROTOCOL_CAPABILITY`

Inquires the presence of one or more optional GigE Vision features on your camera. Note that a combination of the values below can be returned. Bitwise operators must be used to verify the presence of a specific capability.

| Value | Description |
| --- | --- |
| `M_GC_ACTION_SUPPORT` | Specifies that your camera supports action commands. To configure an action command, use[`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_ACTION...`](../../Reference/sys/MsysControl.md)+[`M_GC_ACTIONn`](../../Reference/sys/MsysControl.md), where _n_ is the action number. Configure your camera, using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) or Feature Browser (accessible through Aurora Imaging Intellicam or [`MdigControl`](../../Reference/dig/MdigControl.md) with[`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md)) with the appropriate device features. |
| `M_GC_CONCATENATION_SUPPORT` | Specifies that your camera supports concatenating multiple messages of a certain type into a single packet. |
| `M_GC_DISCOVERY_ACK_DELAY_SUPPORT` | Specifies that your camera supports reading the discovery acknowledgment delay field. The discovery acknowledgment delay is the maximum delay permitted before the GenICam-compliant device sends a discovery acknowledgment. On some GenICam-compliant devices, this period is static; on others, it is an automatically established amount of time when the device boots. To inquire whether the discovery acknowledgment delay field on your GenICam-compliant device is a static amount, or changes when the camera boots, use [`M_GC_WRITABLE_DISCOVERY_ACK_DELAY_SUPPORT`](../../Reference/dig/MdigInquire.md). |
| `M_GC_EVENT_DATA_SUPPORT` | Specifies that your camera supports event-data messages. This means that when an event occurs on the camera, the camera can notify the application of the events and attach camera-specific data to the message. |
| `M_GC_EVENT_SUPPORT` | Specifies that your camera supports sending event messages. Event messages notify the application of events occurring on the camera. |
| `M_GC_EXTENDED_STATUS_CODES_1_SUPPORT` | Specifies that your camera supports the extended status codes introduced in the GigE Vision specification, version 1.1. |
| `M_GC_EXTENDED_STATUS_CODES_2_SUPPORT` | Specifies that your camera supports the extended status codes introduced in the GigE Vision specification, version 2.0. |
| `M_GC_HEARTBEAT_DISABLE_SUPPORT` | Specifies that your camera supports disabling the heartbeat mechanism. The heartbeat mechanism is used to inform your GigE Vision camera that communication between your Aurora Imaging Library application and the camera is still likely even though the camera has not contacted your application recently. |
| `M_GC_IEEE_1588_SUPPORT` | Specifies that your camera supports the IEEE 1588 standard for the precision time protocol. To configure an action command to use a scheduled time, use[`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_ACTION_TIME`](../../Reference/sys/MsysControl.md)+[`M_GC_ACTIONn`](../../Reference/sys/MsysControl.md), where _n_ is the action number. Enable the 1588 feature(s) on your camera, using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) or Feature Browser (accessible through Aurora Imaging Intellicam or [`MdigControl`](../../Reference/dig/MdigControl.md) with[`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md)) with the appropriate device features (for example, using the`GevIEEE1588`feature). |
| `M_GC_LINK_SPEED_REGISTER_SUPPORT` | Specifies that your camera supports storing the Ethernet link speed of the camera's network interface, in Mbits/sec. |
| `M_GC_MANIFEST_TABLE_SUPPORT` | Specifies that your camera supports a manifest table, which stores the camera's device description file (XML). |
| `M_GC_PACKET_RESEND_SUPPORT` | Specifies that your camera supports resending lost packets. |
| `M_GC_PENDING_ACK_SUPPORT` | Specifies that your camera supports a pending acknowledgment packet. A pending acknowledged packet indicates that the processing of a command will take a longer period than expected to execute. If your camera is capable of using such a feature, it will then increase the amount of time your application will wait before causing a timeout. |
| `M_GC_PORT_AND_IP_REGISTER_SUPPORT` | Specifies that your camera supports the control channel's primary port and IP address. |
| `M_GC_PRIMARY_APP_SWITCHOVER_SUPPORT` | Specifies that your camera supports primary application switchover. A primary application is an application granted full access to your camera. A primary application switchover occurs when two applications, both with the same level of access, trade control over your camera. |
| `M_GC_SCHEDULED_ACTION_SUPPORT` | Specifies that your camera supports scheduling commands at a specific time in the future. To configure an action command to use a scheduled time, use[`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_ACTION_TIME`](../../Reference/sys/MsysControl.md)+[`M_GC_ACTIONn`](../../Reference/sys/MsysControl.md), where _n_ is the action command number. Enable the IEEE 1588 features on your camera, using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) or Feature Browser (accessible through Aurora Imaging Intellicam or [`MdigControl`](../../Reference/dig/MdigControl.md) with[`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md)) with the appropriate device feature (for example, using the`GevIEEE1588`feature). |
| `M_GC_SERIAL_NUMBER_SUPPORT` | Specifies that your camera supports storing the camera's serial number. |
| `M_GC_TEST_DATA_SUPPORT` | Specifies that your camera supports creating and using a test packet to determine the size of the largest packet that your camera can transmit without error. |
| `M_GC_UNCONDITIONAL_ACTION_SUPPORT` | Specifies that your camera supports action messages, even when the primary control channel is closed. |
| `M_GC_USER_DEFINED_NAME_SUPPORT` | Specifies that your camera supports storing a user-defined name. |
| `M_GC_WRITABLE_DISCOVERY_ACK_DELAY_SUPPORT` | Specifies that your camera supports changing the discovery acknowledgment delay value when the camera boots. The discovery acknowledgment delay is the maximum delay permitted before the GenICam sends a discovery acknowledgment. On some GenICam devices, this period is static, while on others it is a random amount of time. To inquire whether the discovery acknowledgment delay field is supported on your GenICam device, use [`M_GC_DISCOVERY_ACK_DELAY_SUPPORT`](../../Reference/dig/MdigInquire.md). |
| `M_GC_WRITE_MEM_SUPPORT` | Specifies that your camera supports having the primary application write to the camera's memory. |

---

### `M_GC_MESSAGE_PROTOCOL_CAPABILITY`

Inquires whether your camera has one or more message channels available and their settings.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no options are supported for the message channel. |
| `M_GC_FIREWALL_TRAVERSAL_SUPPORT` | Specifies that the firewall will not block message packets. |

---

### `M_GC_NETWORK_INTERFACE_CAPABILITY`

Inquires whether your camera supports a series of networking interface options. Note that a combination of the values below can be returned. Bitwise operators must be used to verify the presence of a specific capability.

| Value | Description |
| --- | --- |
| `M_GC_DHCP_SUPPORT` | Specifies that your camera supports DHCP. |
| `M_GC_LINK_LOCAL_ADDRESS_SUPPORT` | Specifies that your camera supports link-local addresses (LLA). A LLA is an IPv4 address in the 169.254.0.0/16 range. |
| `M_GC_PAUSE_GENERATION_SUPPORT` | Specifies that your camera supports generating Ethernet pause frames that halt the transmissions from the Host to the camera for a period of time. Pause frames are a common way of controlling Ethernet flow control. |
| `M_GC_PAUSE_RECEPTION_SUPPORT` | Specifies that your camera supports receiving Ethernet pause frames that halt the transmissions from the camera for a period of time. Pause frames are a common method of Ethernet flow control. |
| `M_GC_PERSISTENT_IP_SUPPORT` | Specifies that your camera supports storing a persistent (static) IP address. |

---

### `M_GC_NETWORK_INTERFACE_CONFIGURATION`

Inquires which network and IP configuration schemes are enabled and in use on your current network. Note that a combination of the values below can be returned (for example, [`M_GC_PAUSE_RECEPTION_SUPPORT`](../../Reference/dig/MdigInquire.md) + [`M_GC_PAUSE_GENERATION_SUPPORT`](../../Reference/dig/MdigInquire.md) + [`M_GC_LINK_LOCAL_ADDRESS_SUPPORT`](../../Reference/dig/MdigInquire.md)). Bitwise operators must be used to verify the presence of a specific capability.

| Value | Description |
| --- | --- |
| `M_GC_DHCP_SUPPORT` | Specifies that your camera is using DHCP. |
| `M_GC_LINK_LOCAL_ADDRESS_SUPPORT` | Specifies that your camera is using link-local addresses (LLA). A LLA is an IPv4 address in the 169.254.0.0/16 range. |
| `M_GC_PAUSE_GENERATION_SUPPORT` | Specifies that your camera is using generating Ethernet pause frames that halt the transmissions from the Host to the camera for a period of time. Pause frames are a common way of controlling Ethernet flow control. |
| `M_GC_PAUSE_RECEPTION_SUPPORT` | Specifies that your camera is using receiving Ethernet pause frames that halt the transmissions from the camera for a period of time. Pause frames are a common method of Ethernet flow control. |
| `M_GC_PERSISTENT_IP_SUPPORT` | Specifies that your camera is using storing a persistent (static) IP address. |

---

### `M_GC_PHYSICAL_LINK_CONFIGURATION_CAPABILITY`

Inquires whether your camera supports a dynamic or static link aggregation group (LAG) in either a multiple or single link configuration. Note that a combination of the values below can be returned. Bitwise operators must be used to verify the presence of a specific capability.

| Value | Description |
| --- | --- |
| `M_GC_DYNAMIC_LINK_AGGREGATION_SUPPORT` | Specifies that your camera supports a dynamically configured LAG. |
| `M_GC_MULTIPLE_LINK_SUPPORT` | Specifies that your camera supports multiple links. |
| `M_GC_SINGLE_LINK_SUPPORT` | Specifies that your camera supports a single link. |
| `M_GC_STATIC_LINK_AGGREGATION_SUPPORT` | Specifies that your camera supports a static LAG. |

---

### `M_GC_STREAM_CHANNEL_CAPABILITY`

Inquires the list of capabilities specific to the stream channels of your camera. Note that a combination of the values below can be returned. Bitwise operators must be used to verify the presence of a specific capability.

| Value | Description |
| --- | --- |
| `M_GC_ALL_IN_SUPPORT` | Specifies that the stream channel supports the All-in transmission mode when transmitting data. |
| `M_GC_BIG_AND_LITTLE_ENDIAN_SUPPORT` | Specifies that your camera supports both big and little-endian on the stream channel. Note that all stream channels must support little-endian. |
| `M_GC_EXTENDED_CHUNK_DATA_SUPPORT` | Specifies that the stream channel supports an older formatting for chunk data. |
| `M_GC_IP_REASSEMBLY_SUPPORT` | Specifies that the stream channel(s) supports the reassembly of fragmented IP packets when receiving data. |
| `M_GC_MULTI_ZONE_SUPPORT` | Specifies that the stream channel supports transmission of uncompressed image data that is sliced into horizontal strips. |
| `M_GC_PACKET_RESEND_OPTION_SUPPORT` | Specifies that the stream channel supports unconditional streaming capabilities when transmitting. |
| `M_GC_UNCONDITIONAL_STREAMING_SUPPORT` | Specifies that the stream channel supports alternate destinations for packet resend requests. |

---

### `M_GC_STREAM_PROTOCOL_CAPABILITY`

Inquires how GVSP traffic (typically, unidirectional in nature) supports bypassing the firewall or other network restrictions. Note that a combination of the values below can be returned. Bitwise operators must be used to verify the presence of a specific capability.

| Value | Description |
| --- | --- |
| `M_GC_FIREWALL_TRAVERSAL_SUPPORT` | Specifies that the firewall will not block message packets. |
| `M_GC_LEGACY_16BIT_BLOCK_SUPPORT` | Specifies that your camera supports legacy GEV 1.x stream mode. |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

> **Note:** Note that this inquire type is only available if using a GenCP-compatible camera, or if you first install the third-party, vendor-supplied, standard compliant CLProtocol library for the Camera Link camera connected to your digitizer.

Note that, to use this inquire type, you must use a DCF that uses a composite video signal.

Note that, to use this inquire type on the Host, you must pass a simulated digitizer (allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md)).

Note that this inquire type is only available when [`M_GC_STREAMING_MODE`](../../Reference/dig/MdigControl.md) is set to [`M_AUTOMATIC`](../../Reference/dig/MdigControl.md).

Note that this inquire type is used to perform Bayer conversion if performed by the Host. To inquire the Bayer conversion on the camera, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md); refer to your camera's documentation for details regarding the features to inquire.

Note that this inquire type is only available when using color versions of Zebra Iris GTX (such as 2000C).
