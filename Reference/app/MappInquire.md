---
doctype: Reference
module: app
function: MappInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / app / MappInquire"
---

# MappInquire

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Yes |
| Clarity UHD | Partial |
| Concord PoE | Partial |
| GenTL | Yes |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | Partial |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Inquire about an application setting.

## Syntax

```c
AIL_INT MappInquire(
    AIL_ID    ContextAppId,  //in
    AIL_INT64 InquireType,   //in
    void *    UserVarPtr     //out
)
```

## Description

This function inquires about the specified application setting.

## Parameters

### `ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the application context of the current process.

Typically specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappInquire.md). |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of application setting about which to inquire. This parameter can be set to one of the values below.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MappInquire`](../../Reference/app/MappInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about general types of application settings

The following inquire types allow you to inquire about general types of application settings.

---

### `M_AIL_DIRECTORY_CONTEXTS`

Inquires the path to the local contexts directory. The returned value is equivalent to the string constant **M_CONTEXT_PATH**.  By default on a Windows platform, this path is _C:\Program Files\Aurora Imaging Library\&lt;version number>\Library\Contexts_.  By default on a Linux platform, this path is _/opt/aurora_imaging_library/&lt;version>/contexts_.

---

### `M_AIL_DIRECTORY_EXAMPLES`

Inquires the path to the local examples directory.  By default on a Windows platform, this path is _C:\Program Files\Aurora Imaging Library\&lt;version number>\Library\Examples_.  By default on a Linux platform, this path is _/opt/aurora_imaging_library/&lt;version>/examples_.

---

### `M_AIL_DIRECTORY_IMAGES`

Inquires the path to the local images directory. The returned value is equivalent to the string constant **M_IMAGE_PATH**.  By default on a Windows platform, this path is _C:\Program Files\Aurora Imaging Library\&lt;version number>\Library\Images_.  By default on a Linux platform, this path is _/opt/aurora_imaging_library/&lt;version>/images_.

---

### `M_AIL_DIRECTORY_INSTALL`

Inquires the path to the root directory of the local Aurora Imaging Library installation. The returned value is equivalent to the string constant **M_INSTALL_DIR**.  By default on a Windows platform, this path is _C:\Program Files\Aurora Imaging Library\&lt;version number>\Library_.  By default on a Linux platform, this path is _/opt/aurora_imaging_library/&lt;version>/library_.

---

### `M_COMPONENT_AUTO_RESELECT`

Inquires whether image buffer components are automatically reselected to 2D displays when they are reallocated.  > **Note:** This setting does not affect containers shown in 3D displays.  > **Note:** If [`M_THREAD_CURRENT`](../../Reference/app/MappInquire.md) was specified, this inquires the setting for the current thread. Otherwise, it inquires the setting of the application environment.

| Value | Description |
| --- | --- |
| `M_COMPONENT_AUTO_RESELECT_DISABLE` | Specifies not to automatically reselect image buffer components to 2D displays. |
| `M_COMPONENT_AUTO_RESELECT_ENABLE` *(default)* | Specifies to automatically reselect image buffer components to 2D displays when they are reallocated. |
| `M_DEFAULT` | Specifies that the global application setting is used for the current thread. Note that this value is only returned if [`M_THREAD_CURRENT`](../../Reference/app/MappInquire.md) was specified. |

---

### `M_COMPONENT_AUTO_RESELECT_ENABLED`

Inquires whether[`M_COMPONENT_AUTO_RESELECT`](../../Reference/app/MappInquire.md) is enabled for the current thread.  This always returns whether [`M_COMPONENT_AUTO_RESELECT`](../../Reference/app/MappInquire.md) is enabled for the current thread (regardless of whether it is enabled because it was set for the application environment, or because you enabled it explicitly for the thread using [`MappControl`](../../Reference/app/MappControl.md) with [`M_COMPONENT_AUTO_RESELECT`](../../Reference/app/MappControl.md) + [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md)).  > **Note:** To inquire the setting for the application environment (instead of the current thread), use [`M_COMPONENT_AUTO_RESELECT`](../../Reference/app/MappInquire.md) instead.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that [`M_COMPONENT_AUTO_RESELECT`](../../Reference/app/MappInquire.md) is not enabled in the current thread. |
| `M_YES` | Specifies that [`M_COMPONENT_AUTO_RESELECT`](../../Reference/app/MappInquire.md) is enabled in the current thread. |

---

### `M_CURRENT_APPLICATION`

Inquires the identifier of the current Aurora Imaging Library application, if any.

| Value | Description |
| --- | --- |
| `0` | Specifies that no application is allocated. Note that no error is generated. |
| `Application identifier` | Specifies the identifier of the current application. |

---

### `M_DAIL_CONNECTION`

Inquires the Distributed Aurora Imaging Library monitoring configuration permission level.

| Value | Description |
| --- | --- |
| `M_DAIL_CONTROL` | Specifies that objects in the current application can be accessed with either read only or read/write permission, depending on how that object was specified with [`MobjControl`](../../Reference/obj/MobjControl.md). |
| `M_DAIL_MONITOR` | Specifies that objects in the current application can be accessed only with read only permission, regardless of how that object was specified with [`MobjControl`](../../Reference/obj/MobjControl.md). |
| `M_DISABLE` | Specifies that Distributed Aurora Imaging Library monitoring configuration is disabled in the current application. |

---

### `M_DAIL_CONNECTION_PORT`

Inquires the listening port of a publishing application in the Distributed Aurora Imaging Library monitoring configuration.

| Value | Description |
| --- | --- |
| `Listening port` | Specifies the listening port (an integer between 0 and 65535) that the current application will use to listen for connections from a monitoring application. |

---

### `M_ERROR`

Inquires the error printing mode in effect for the current thread.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_PRINT_ENABLE`](../../Reference/app/MappInquire.md), unless [`M_THREAD_CURRENT`](../../Reference/app/MappInquire.md) is specified, in which case this setting specifies that the current thread uses the setting of the application environment. |
| `M_PRINT_DISABLE` | Specifies not to print error messages. |
| `M_PRINT_ENABLE` | Specifies to print error messages. |
| `M_THROW_EXCEPTION` | Specifies that when an error occurs, a .NET or Python exception will be thrown instead of a message box being displayed. |
| `M_THROW_EXCEPTION` | Specifies that when an error occurs, a .NET exception will be thrown instead of a message box being displayed.  > **Note:** [`M_THROW_EXCEPTION`](../../Reference/app/MappInquire.md) is only valid when working in a  .NET environment. |

---

### `M_ERROR_HOOKS`

Inquires whether errors generated in the current thread will trigger the user-defined function that you specified using [`MappHookFunction`](../../Reference/app/MappHookFunction.md) (with[`M_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md) or [`M_ERROR_FATAL`](../../Reference/app/MappHookFunction.md)).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies to not trigger the user-defined function (hooked using [`MappHookFunction`](../../Reference/app/MappHookFunction.md)) when an error event occurs. |
| `M_ENABLE` *(default)* | Specifies to trigger the user-defined function when an error event occurs, as specified in [`MappHookFunction`](../../Reference/app/MappHookFunction.md). |

---

### `M_GENTL_PRODUCER_COUNT`

**Board availability:** GenTL, V4L2

Inquires the number of different GenTL producers installed during Aurora Imaging Library installation.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of different GenTL producers installed. |

---

### `M_GENTL_PRODUCER_DESCRIPTOR + n`

**Board availability:** GenTL, V4L2

Inquires the path and file name of the_n_ <sup>th</sup> GenTL producer, where _n_ is a number between 0 and the value returned by [`M_GENTL_PRODUCER_COUNT`](../../Reference/app/MappInquire.md).

| Value | Description |
| --- | --- |
| `"PathAndFilename"` | Specifies the path and file name of the specified GenTL producer. |

---

### `M_INSTALLED_SYSTEM_COUNT`

Inquires the number of different system types (drivers) installed during Aurora Imaging Library installation and the number of Distributed Aurora Imaging Library systems that are registered.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of different system types installed. |

---

### `M_INSTALLED_SYSTEM_DESCRIPTOR + n`

Inquires the descriptor of system type _n_, where _n_ is a number between 0 and the value returned by [`M_INSTALLED_SYSTEM_COUNT`](../../Reference/app/MappInquire.md). The descriptor can be passed to [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) to allocate the system.

---

### `M_INSTALLED_SYSTEM_DEVICE_COUNT + n`

Inquires the number of installed devices of the same type as system type n, where n is a number between 0 and the value returned by [`M_INSTALLED_SYSTEM_COUNT`](../../Reference/app/MappInquire.md).

| Value | Description |
| --- | --- |
| `M_UNKNOWN` | Specifies that the number of devices is unknown. |
| `Value >= 0` | Specifies the number of devices of the specified type. Note that although the driver for a system type is installed, this inquire type returns 0 if a corresponding physical device is not installed. In addition, for a GigE system type and USB3 system type, this inquire type will return 1. |

---

### `M_INSTALLED_SYSTEM_PRINT_NAME + n`

Inquires the printable name of system type _n_ (for example, Zebra Radient), where _n_ is a number between 0 and the value returned by [`M_INSTALLED_SYSTEM_COUNT`](../../Reference/app/MappInquire.md).

| Value | Description |
| --- | --- |
| `String` | Specifies the printable name of system type _n_. |

---

### `M_INSTALLED_SYSTEM_TYPE + n`

Inquires the type of system type _n_, where _n_ is a number between 0 and the value returned by [`M_INSTALLED_SYSTEM_COUNT`](../../Reference/app/MappInquire.md).  If a Distributed Aurora Imaging Library remote system has been registered, you can use the following macros to further inquire about the system:  - To determine whether the system is a Distributed Aurora Imaging Library remote system, use the **M_IS_SYSTEM_DISTRIBUTED** macro. This macro returns a non-zero value if the system is a remote system and zero if the system is local. - To remove the bit flag for Distributed Aurora Imaging Library remote systems, use the**M_REAL_SYSTEM_TYPE** macro. This macro returns only the system type and not the system type and remote system bit flag.

| Value | Description |
| --- | --- |
| `M_SYSTEM_CLARITY_UHD_TYPE` | Specifies an Aurora Imaging Library Clarity UHD system. |
| `M_SYSTEM_CONCORD_POE_TYPE` | Specifies an Aurora Imaging Library Concord POE system. |
| `M_SYSTEM_GENTL_TYPE` | Specifies an Aurora Imaging Library GenTL system. |
| `M_SYSTEM_GEVIQ_TYPE` | Specifies an Aurora Imaging Library GevIQ system. |
| `M_SYSTEM_GIGE_VISION_TYPE` | Specifies an Aurora Imaging Library GigE Vision system. |
| `M_SYSTEM_HOST_TYPE` | Specifies the Host. |
| `M_SYSTEM_INDIO_TYPE` | Specifies an Aurora Imaging Library Indio system. |
| `M_SYSTEM_IRIS_GTX_TYPE` | Specifies an Aurora Imaging Library Iris GTX system. |
| `M_SYSTEM_RADIENTEVCL_TYPE` | Specifies an Aurora Imaging Library Radient eV-CL system. |
| `M_SYSTEM_RAPIXOCL_TYPE` | Specifies an Aurora Imaging Library Rapixo Pro CL system. |
| `M_SYSTEM_RAPIXOCOF_TYPE` | Specifies an Aurora Imaging Library Rapixo CoF system. |
| `M_SYSTEM_RAPIXOCXP_TYPE` | Specifies an Aurora Imaging Library Rapixo CXP system. |
| `M_SYSTEM_USB3_VISION_TYPE` | Specifies an Aurora Imaging Library USB3 Vision system. |
| `M_SYSTEM_V4L2_TYPE` | Specifies an Aurora Imaging Library Video4Linux2 system. |

---

### `M_KEY_SERIAL_NUMBER + n`

Inquires the serial number of the_n_ <sup>th</sup>Wibu-Systems dongle, where_n_ is a number between 0 and the value returned by [`M_NUMBER_OF_KEYS`](../../Reference/app/MappInquire.md).

| Value | Description |
| --- | --- |
| `String` | Specifies the inquired Wibu-Systems dongle serial number. |

---

### `M_LICENSE_MODULES`

Inquires the modules for which there is a valid license. The value returned will be a bitwise combination of the following.

| Value | Description |
| --- | --- |
| `M_LICENSE_3DCA` | Specifies a Camera Calibration module license that supports 3D-based camera calibration modes. |
| `M_LICENSE_3DSUP` | Specifies a 3D Supplement module license. |
| `M_LICENSE_BEAD` | Specifies a Bead module license. |
| `M_LICENSE_BLOB` | Specifies a Blob analysis module license. |
| `M_LICENSE_CAL` | Specifies a Camera Calibration module license that supports 2D-based camera calibration modes. |
| `M_LICENSE_CLASS` | Specifies a Classification module license. |
| `M_LICENSE_CODE` | Specifies a Code read/write module license. |
| `M_LICENSE_COL` | Specifies a Color Analysis module license. |
| `M_LICENSE_COM` | Specifies an industrial communication license. |
| `M_LICENSE_DAIL` | Specifies a Distributed Aurora Imaging Library license. |
| `M_LICENSE_DEBUG` | Specifies a development license. |
| `M_LICENSE_DMR` | Specifies a SureDotOCR module license. |
| `M_LICENSE_EDGE` | Specifies an Edge Finder module license. |
| `M_LICENSE_IM` | Specifies an Image Processing module license. |
| `M_LICENSE_INTERFACE` | Specifies a Zebra GigE Vision driver license. |
| `M_LICENSE_JPEG2000` | Specifies a JPEG2000 compression license. |
| `M_LICENSE_JPEGSTD` | Specifies a JPEG compression license. |
| `M_LICENSE_LITE` | Specifies an Aurora Imaging Library Lite module license. |
| `M_LICENSE_MEAS` | Specifies a Measurement module license. |
| `M_LICENSE_MET` | Specifies a Metrology module license. |
| `M_LICENSE_MOD` | Specifies a Geometric Model Finder module license. |
| `M_LICENSE_OCR` | Specifies an OCR module license. |
| `M_LICENSE_PAT` | Specifies a Pattern Matching module license. |
| `M_LICENSE_REG` | Specifies a Registration module license. |
| `M_LICENSE_STR` | Specifies a String Reader module license. |

---

### `M_MEMORY`

Inquires the memory compensation mode in effect for the current thread.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_COMPENSATION_ENABLE`](../../Reference/app/MappInquire.md), unless [`M_THREAD_CURRENT`](../../Reference/app/MappInquire.md) is specified, in which case this setting specifies that the current thread uses the setting of the application environment. |
| `M_COMPENSATION_DISABLE` | Specifies that on-board memory compensation is disabled. |
| `M_COMPENSATION_ENABLE` | Specifies that on-board memory compensation is enabled. |

---

### `M_NON_PAGED_MEMORY_FREE`

Inquires the amount of free non-paged memory reserved for Aurora Imaging Library (at installation time or using the Aurora Imaging Configurator utility).

| Value | Description |
| --- | --- |
| `Value` | Specifies the amount of free non-paged memory, in bytes. |

---

### `M_NON_PAGED_MEMORY_LARGEST_FREE`

Inquires the size of the largest contiguous block of free non-paged memory reserved for Aurora Imaging Library (at installation time or using the Aurora Imaging Configurator utility).  You cannot allocate a buffer larger than this size using an [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md)function, even if the total amount of free non-paged memory is sufficient. For more information, see [Large non-paged Aurora Imaging Library buffers](../../UserGuide/C23_Data_buffers/S18_Advanced_memory_management.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the size of the largest contiguous block of free non-paged memory reserved for Aurora Imaging Library. |

---

### `M_NON_PAGED_MEMORY_SIZE`

Inquires the amount of the non-paged memory (DMA) reserved for Aurora Imaging Library.

| Value | Description |
| --- | --- |
| `Value` | Specifies the amount of the non-paged memory (DMA) reserved for Aurora Imaging Library, in bytes. |

---

### `M_NON_PAGED_MEMORY_USED`

Inquires the amount of non-paged memory being used.

| Value | Description |
| --- | --- |
| `Value` | Specifies the amount of non-paged memory being used, in bytes. |

---

### `M_NUMBER_OF_KEYS`

Inquires the number of Wibu-Systems dongles detected.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of Wibu-Systems dongles detected. |

---

### `M_PARAMETER`

Inquires the parameter checking mode in effect for the current thread.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CHECK_ENABLE`](../../Reference/app/MappInquire.md), unless [`M_THREAD_CURRENT`](../../Reference/app/MappInquire.md) is specified, in which case this setting specifies that the current thread uses the setting of the application environment. |
| `M_CHECK_DISABLE` | Specifies that checking of parameters is disabled. |
| `M_CHECK_ENABLE` | Specifies that checking of parameters is enabled. |

---

### `M_PLATFORM_BITNESS`

Inquires the bitness for which the application has been compiled.

| Value | Description |
| --- | --- |
| `32` | Specifies that the specified application is 32-bit. |
| `64` | Specifies that the specified application is 64-bit. |

---

### `M_PLATFORM_OS_TYPE`

Inquires the operating system under which the specified application is running.

| Value | Description |
| --- | --- |
| `M_OS_LINUX` | Specifies that the specified application is running under a Linux operating system. |
| `M_OS_WINDOWS` | Specifies that the specified application is running under a Microsoft Windows operating system. |

---

### `M_PROCESSING`

Inquires the processing compensation mode in effect for the current thread.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_COMPENSATION_ENABLE`](../../Reference/app/MappInquire.md), unless [`M_THREAD_CURRENT`](../../Reference/app/MappInquire.md) is specified, in which case this setting specifies that the current thread uses the setting of the application environment. |
| `M_COMPENSATION_DISABLE` | Specifies that on-board processing compensation is disabled. |
| `M_COMPENSATION_ENABLE` | Specifies that on-board processing compensation is enabled. |

---

### `M_SELECTED_VERSION`

Inquires the selected version of Aurora Imaging Library.

| Value | Description |
| --- | --- |
| `Value` | Specifies the version number of Aurora Imaging Library. |

---

### `M_TRACE`

Inquires the trace setting, which is used to start or stop generating a trace log.  > **Note:** Note that this inquire type can return [`M_DEFAULT`](../../Reference/app/MappControl.md), a trace setting which can signify either to start or to stop generating a trace log. Use [`M_TRACE_ACTIVE`](../../Reference/app/MappInquire.md) to determine if a trace log is currently being generated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default trace setting, which either enables or disables generating a trace log, based on whether Aurora Imaging Profiler (or the Interactive log) was used to start generating the log. |
| `M_LOG_DISABLE` | Specifies the log disable setting, which stops generating the trace log. |
| `M_LOG_ENABLE` | Specifies the log enable setting, which starts generating the trace log. |

---

### `M_TRACE_ACTIVE`

Inquires whether a trace log is currently being generated in the current thread.  > **Note:** Note that adding the combination constant [`M_THREAD_CURRENT`](../../Reference/app/MappInquire.md) to this inquire will result in an error.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that a trace log is not currently being generated in the current thread. |
| `M_YES` | Specifies that a trace log is currently being generated in the current thread. |

---

### `M_VERSION`

Inquires the version of Aurora Imaging Library.

| Value | Description |
| --- | --- |
| `Value` | Specifies the version of Aurora Imaging Library. |

---

### `M_VERSION_STRING`

Inquires the version of Aurora Imaging Library, as a string.

| Value | Description |
| --- | --- |
| `"nn.nn.nnnn"` | Specifies the version of Aurora Imaging Library, as a string. |

---

### `M_WEB_CONNECTION`

Inquires whether Aurora Imaging Web Library server functionality is enabled.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Web Library server functionality is disabled in the current application. |
| `M_ENABLE` | Specifies that Aurora Imaging Web Library server functionality is enabled in the current application. |

---

### `M_WEB_CONNECTION_PORT`

Inquires the listening port used for Aurora Imaging Web Library server functionality.

| Value | Description |
| --- | --- |
| `0 <= Value <=65535` *(default)* | Specifies the port number. |

---

### `M_XORG_ACCELERATION`

Inquires whether Aurora Imaging Library can use X11 (Xorg) acceleration for display under Linux. This determines the hardware acceleration display mode.

| Value | Description |
| --- | --- |
| *(see `M_X11_ACCELERATION Status`)* |  |

### Combination Constants — For inquiring if a system is remote

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine if the inquired system type is remote.

By default, an installed systems that is remote will have a distributed flag appended to the returned system type.

| Value | Description |
| --- | --- |
| `M_SYSTEM_DISTRIBUTED_FLAG` | Specifies that the system is remote. |

### Combination Constants — For

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that the inquire should be limited to the current thread.

In multi-thread environments, by default, you can inquire about an application setting from any thread.

| Value | Description |
| --- | --- |
| `M_THREAD_CURRENT` | Specifies that data to be returned should relate to the current thread. |

### Combination Constants — For inquiring the size of a string

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Inquires the number of characters in the string. This number accounts for every character, including the terminating null character.

### For inquiring system fingerprints

When inquired and present, the requested system fingerprint is returned. If that specific fingerprint is not present, 0 is returned. For more information about system fingerprints, see [Protecting your own Aurora Imaging Library software application using a Zebra hardware fingerprint](../../UserGuide/C69_Distribution_and_licensing/S10_Protecting_your_own_software_application.md).

---

### `M_ANY_FINGERPRINT`

Inquires any hardware fingerprint.

| Value | Description |
| --- | --- |
| `0` | Specifies that no hardware that has been correctly installed has a fingerprint. |
| `Value` | Specifies the hardware fingerprint. |

---

### `M_CONCORD_GIGE_FINGERPRINT`

Inquires the hardware fingerprint of a Zebra Concord GigE Vision board.

| Value | Description |
| --- | --- |
| `0` |  |
| `Value` | Specifies the hardware fingerprint. |

---

### `M_CONCORDPOE_FINGERPRINT`

Inquires the hardware fingerprint of a Zebra Concord PoE board.

| Value | Description |
| --- | --- |
| `0` |  |
| `Value` | Specifies the hardware fingerprint. |

---

### `M_ID_KEY_FINGERPRINT`

Inquires the hardware fingerprint of a hardware identification key. The hardware identification key is the device, which when connected to your USB or parallel port, you can use as a hardware fingerprint to generate a software license.

| Value | Description |
| --- | --- |
| `0` | Specifies that the hardware identification key is not connected to the computer, or that it is not correctly installed. |
| `Value` | Specifies the hardware fingerprint. |

---

### `M_INDIO_FINGERPRINT`

Inquires the hardware fingerprint of a Zebra Indio board.

| Value | Description |
| --- | --- |
| `0` |  |
| `Value` | Specifies the hardware fingerprint. |

---

### `M_IRISGTX_FINGERPRINT`

Inquires the hardware fingerprint of a Zebra Iris GTX smart camera.

| Value | Description |
| --- | --- |
| `0` | Specifies that the current computer is not a Zebra Iris GTX smart camera. |
| `Value` | Specifies the hardware fingerprint. |

---

### `M_MSERIES_FINGERPRINT`

Inquires the hardware fingerprint of a Zebra M-Series graphics board.

| Value | Description |
| --- | --- |
| `0` |  |
| `Value` | Specifies the hardware fingerprint. |

---

### `M_RADIENT_FINGERPRINT`

Inquires the hardware fingerprint of a Zebra Radient board.

| Value | Description |
| --- | --- |
| `0` |  |
| `Value` | Specifies the hardware fingerprint. |

---

### `M_RAPIXOCXP_FINGERPRINT`

Inquires the hardware fingerprint of a Zebra Rapixo CXP board.

| Value | Description |
| --- | --- |
| `0` |  |
| `Value` | Specifies the hardware fingerprint. |

### Combination Constants — For specifying which Zebra product to take a fingerprint from

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify which Zebra product to take the fingerprint from when there are more than one available.

When multiple fingerprints of the same type are present, all similar fingerprints can be inquired using the following.

| Value | Description |
| --- | --- |
| `0 <= Value < 16` | Specifies the number of fingerprints of the same type that can be inquired about. |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

Specifies that the board is not present in the computer, or that the board is incorrectly installed.

Typically, you should not use this setting if [`ContextAppId`](../../Reference/app/MappInquire.md)specifies an application context allocated on a remote computer, accessed using Distributed Aurora Imaging Library. The result will return information about a thread on the remote computer, rather than the current thread.
