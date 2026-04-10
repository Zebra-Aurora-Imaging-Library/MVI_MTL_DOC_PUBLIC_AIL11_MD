---
doctype: Reference
module: sys
function: MsysAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sys / MsysAlloc"
---

# MsysAlloc

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

> Allocate an Aurora Imaging Library system.

## Syntax

```c
AIL_ID MsysAlloc(
    AIL_ID             ContextAppId,      //in
    AIL_CONST_TEXT_PTR SystemDescriptor,  //in
    AIL_INT            SystemNum,         //in
    AIL_INT64          InitFlag,          //in
    AIL_ID *           SysIdPtr           //out
)
```

## Description

This function allocates an Aurora Imaging Library system so that it can be used by subsequent Aurora Imaging Library functions. This function can allocate an Aurora Imaging Library system, which consists of a Zebra imaging board (or third-party board), the Host CPU and memory, and any available graphics controller. Alternatively, this function can allocate a Host-type system, which consists of the Host CPU and memory, and any available graphics controller.

A system must be allocated before any buffers, displays, or digitizer can be allocated on it. Before allocating a system, an application must be allocated, using [`MappAlloc`](../../Reference/app/MappAlloc.md) or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md). To use the default system, you must allocate it using [`M_SYSTEM_DEFAULT`](../../Reference/sys/MsysAlloc.md).

Note, upon allocation of an application, a default Host system is automatically allocated. Rather than using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) to allocate a Host system, you can use this default Host system, by specifying [`M_DEFAULT_HOST`](../../Reference/disp/MdispAlloc.md) wherever a Host system identifier is required.

After allocating the system, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the system identifier returned is not [`M_NULL`](../../Reference/sys/MsysAlloc.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/sys/MsysAlloc.md) was specified).

When the system is no longer required, release it using[`MsysFree`](../../Reference/sys/MsysFree.md)unless [`M_UNIQUE_ID`](../../Reference/sys/MsysAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/sys/MsysAlloc.md) was specified, the smart identifier manages the system's lifetime and you must not manually free it.

## Parameters

### `ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the current application context. |
| `Application Context Identifier` | Specifies the application context identifier. |

### `SystemDescriptor` *(in, AIL_CONST_TEXT_PTR)*

Specifies the type of system to allocate. Set this parameter to one of the following values:

*For specifying the type of system to allocate*

| Value | Description |
| --- | --- |
| `M_SYSTEM_CLARITY_UHD` | Allocates an Aurora Imaging Library Clarity UHD system. |
| `M_SYSTEM_CONCORD_POE` | Allocates an Aurora Imaging Library Concord POE system.

> **Note:** Note that to use Zebra Concord PoE for acquisition, you must allocate and use an Aurora Imaging Library GigE Vision system ([`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md)) instead; refer to information denoted for a GigE Vision system. You only need to allocate and use an Aurora Imaging Library Concord PoE system ([`M_SYSTEM_CONCORD_POE`](../../Reference/sys/MsysAlloc.md)) to use the other functionality on the board and to inquire about the board itself. So information in this reference, for use with an Aurora Imaging Library Concord PoE system, is denoted Aurora Imaging Library Concord PoE with ToE since it is typically only applicable to this model of the board. If the information is applicable to the Zebra Concord PoE base model, it will be explicitly specified. For more information, see Using an Aurora Imaging Library Concord PoE system with the Zebra Concord PoE base model. |
| `M_SYSTEM_DEFAULT` | Specifies the default system. This value is set during Aurora Imaging Library installation and using the Aurora Imaging Configurator utility. Note that the end-user can always change this value. |
| `M_SYSTEM_GENTL` | Allocates an Aurora Imaging Library GenTL system. This allocation opens general communication with a GenTL Producer (library). |
| `M_SYSTEM_GEVIQ` | Allocates an Aurora Imaging Library GevIQ system. |
| `M_SYSTEM_GIGE_VISION` | Allocates an Aurora Imaging Library GigE Vision system. This allocation opens general communication with all the GigE Vision-compliant cameras (or devices) found on your subnet (through one or more Gigabit Ethernet network adapters in your computer). |
| `M_SYSTEM_HOST` | Specifies a Host system. Note that a Host system has no hardware-supported acquisition capabilities, however some acquisition capabilities are available when using a simulated digitizer, allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md). |
| `M_SYSTEM_INDIO` | Allocates an Aurora Imaging Library Indio system. If you need to capture images from a GigE Vision camera, you will need to allocate [`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md) as well. |
| `M_SYSTEM_IRIS_GTX` | Allocates an Aurora Imaging Library Iris GTX system. |
| `M_SYSTEM_RADIENTEVCL` | Allocates an Aurora Imaging Library Radient eV-CL system. |
| `M_SYSTEM_RAPIXOCL` | Allocates an Aurora Imaging Library Rapixo CL system. |
| `M_SYSTEM_RAPIXOCOF` | Allocates an Aurora Imaging Library Rapixo CoF system. |
| `M_SYSTEM_RAPIXOCXP` | Allocates an Aurora Imaging Library Rapixo CXP system. |
| `M_SYSTEM_USB3_VISION` | Allocates an Aurora Imaging Library USB3 Vision system. This allocation opens general communication with all the USB3 Vision-compliant cameras (or devices) connected to your computer. |
| `M_SYSTEM_V4L2` | Allocates a Video4Linux2 system. This allocation opens general communication with all the V4L2-compliant cameras (or devices) connected to your computer. |
| `"dailshm://[Passkey:]localhost[:Port]/AILSystemType"` | Allocates a DAIL remote system for a separate process on the local computer, using the DAIL SHM protocol. This protocol allows you to communicate between a client and server process on the same computer using shared memory. To allocate a DAIL remote system on a computer, that computer must have a valid DAIL installation.

When specifying the string that indicates the local computer:

1. If required, replace _[Passkey]_ with the passkey for the DAIL server on the local computer. The passkey is an alphanumeric string of up to 16 characters in length. This passkey is set by the Aurora Imaging Configurator utility in the server settings page. If a passkey was not set for the DAIL server, omit the passkey and the ":" that follows it.
2. Following "localhost:", replace _[Port]_ with the port that the local computer should access to initiate new connections with the DAIL server, unless the default connection port is the listening port of the DAIL server; in which case, omit the port number and the ":" that follows it.
3. Replace _AILSystemType_ with any valid Aurora Imaging Library system type listed in this table, such as [`M_SYSTEM_RAPIXOCXP`](../../Reference/sys/MsysAlloc.md). The "://" and "/" must be included.

> **Note:** The DAIL SHM protocol only supports connections to localhost.

Prior to allocating a DAIL remote system, certain conditions must be met. For more information, see [Distributed Aurora Imaging Library](../../UserGuide/C63_Distributed_AIL/ChapterInformation.md). |
| `"dailtcp://[Passkey:]RemoteComputerName[:Port]/AILSystemType"` | Allocates a DAIL remote system on a remote or local computer, using the TCP/IP protocol. To allocate a DAIL remote system on a computer, that computer must have a valid DAIL installation.

When specifying the string that indicates the remote computer (or local computer for a DAIL server also running locally):

1. If required, replace _[Passkey]_ with the passkey for the remote computer. The passkey is an alphanumeric string of up to 16 characters in length. This passkey is set by the Aurora Imaging Configurator utility in the server settings page. If a passkey was not set for the remote computer, omit the passkey and the ":" that follows it. If you are attempting to connect to a local computer, specify the passkey for the DAIL server on the local computer.
2. Replace _RemoteComputerName_ with the remote computer's name or IP address; Aurora Imaging Library supports both IPv4 and IPv6 addresses. A typical IPv4 string has the format n.n.n.n, where _n_ is a number between 0 and 255. A typical IPv6 string has the format x:x:x:x:x:x:x:x, where _x_ is a hexadecimal number between 0000 and FFFF. If you are supplying an IPv6 address, you must use square brackets to separate the address from the port. If you are attempting to connect to the local computer, set this to localhost.
3. If required, replace _[Port]_ with the port that the local computer should access on the remote computer to initiate new connections, unless the default connection port is the listening port of the DAIL server on the remote computer; in which case, omit the port number and the ":" that follows it. If you are attempting to connect to a local computer, specify the listening port of the DAIL server on the local computer.
4. Replace _AILSystemType_ with any valid Aurora Imaging Library system type listed in this table, such as [`M_SYSTEM_RAPIXOCXP`](../../Reference/sys/MsysAlloc.md). The "://" and "/" must be included.

Prior to allocating a DAIL remote system, certain conditions must be met. For more information, see [Distributed Aurora Imaging Library](../../UserGuide/C63_Distributed_AIL/ChapterInformation.md). |

### `SystemNum` *(in, AIL_INT)*

Specifies the number (rank) or user-defined name of the target board of the specified system type. This parameter can be set to one of the following:

*For specifying the number of the target board*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default board.

> **Note:** The default board is set in the Aurora Imaging Configurator utility. |
| `M_DEVn` | Specifies the device number (rank) of the board. You can set _n_ to one of the following values: 0 &lt;= _n_ &lt;=15. |
| `"BoardIdentifierString"` | Specifies the user-defined name of the board. When allocating the board with a name, the InitFlag must be set to[`M_DEVICE_NAME`](../../Reference/sys/MsysControl.md). The name must also have been previously assigned and written to the board. Typically, this is done from a different executable that allocates a system for the board using [`M_DEVn`](../../Reference/sys/MsysAlloc.md), and then assigns a name to the board using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_DEVICE_NAME`](../../Reference/sys/MsysControl.md). |

*For identifying the instance of the GenTL library to use*

| Value | Description |
| --- | --- |
| `M_GENTL_PRODUCER` | Specifies the GenTL Producer (library) for which to allocate this Aurora Imaging Library GenTL system. |

### `InitFlag` *(in, AIL_INT64)*

Specifies how to perform the allocation.

*For specifying the type of initialization setup*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_CL` | Specifies to initialize Camera Link transport layer technology. |
| `M_COMPLETE` | Specifies to initialize the system completely; the system is initialized to its default state and any required resident software is downloaded. At least one complete initialization is necessary after you power-up your system. |
| `M_CXP` | Specifies to initialize CoaXPress transport layer technology. |
| `M_GEV` | Specifies to initialize Ethernet transport layer technology. |
| `M_MIXED` | Specifies to initialize the transport layer technology specified by the GenTL Producer (library). |
| `M_U3V` | Specifies to initialize USB transport layer technology. |

*For specifying that the target board is identified using its user-defined name*

| Value | Description |
| --- | --- |
| `M_DEVICE_NAME` | Specifies that the[`SystemNum`](../../Reference/sys/MsysAlloc.md) parameter identifies the target board using its user-defined name. |

### `SysIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the system identifier or specifies the data type that the function should use to return the system identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated system; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated system; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_SYS_ID_is returned instead of a standard Aurora Imaging Library identifier.

> **Note:** This setting is only available when using C++11 (or later).

An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the system (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated system.

If allocation fails, [`M_NULL`](../../Reference/sys/MsysAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the system identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_SYS_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/sys/MsysAlloc.md) was specified).

## Remarks

> If you are creating a DLL that includes a call to [`MsysAlloc`](../../Reference/sys/MsysAlloc.md), ensure that the call is not made from the `DllMain()` function, because [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) might load a required DLL and you cannot load a DLL from `DllMain()`. If necessary, call [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) from an initialization function in your DLL instead.

Note that to allocate this system, the Distributed Aurora Imaging Library server cannot be running as a service on the remote computer; you must either set it to run at logon, or start it manually. To set it to run at logon, open the Aurora Imaging Configurator utility and select **Run at every logon with user credentials** in the **Server Settings** pane, found under the **Distributed Aurora Imaging Library** item.

To start it manually, you must logon to the remote computer and, from the Aurora Imaging Configurator utility, click on the **Start Server** button, found in the **Server Settings** pane under the **Distributed Aurora Imaging Library** item. For more information, see [Setting up the Distributed Aurora Imaging Library server on remote computers](../../UserGuide/C63_Distributed_AIL/S03_Preparing_computers_for_Distributed_AIL.md).
