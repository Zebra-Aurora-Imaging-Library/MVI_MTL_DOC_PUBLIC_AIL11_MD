---
doctype: Reference
module: com
function: McomAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / com / McomAlloc"
---

# McomAlloc

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | No |
| Clarity UHD | No |
| Concord PoE | No |
| GenTL | No |
| GevIQ | No |
| GigE Vision | No |
| Indio | Yes |
| Iris GTX | Yes |
| Radient eV-CL | No |
| Rapixo CL | No |
| Rapixo CoF | No |
| Rapixo CXP | No |
| USB3 Vision | No |

> Allocate an Industrial Communication context.

## Syntax

```c
AIL_ID McomAlloc(
    AIL_ID             SysId,              //in
    AIL_INT64          ProtocolType,       //in
    AIL_CONST_TEXT_PTR DeviceDescription,  //in
    AIL_INT64          InitFlag,           //in
    AIL_INT64          InitValue,          //in
    AIL_ID *           ComIdPtr            //out
)
```

## Description

This function allocates an Industrial Communication context on the specified system. An Industrial Communication context allows you to communicate with a robot controller, with a generic controller that uses the specified industrial protocol (PROFINET, Modbus, EtherNet/IP, or CC-Link IE Field Basic), or with a server Modbus automation device.

For a context allocated to communicate using the PROFINET, Modbus, EtherNet/IP, or CC-Link IE Field Basic protocol, a connection with the controller (or device) is established upon allocation of the context. As such, the protocol service must have been enabled and configured in Aurora Imaging Configurator prior to allocating the context.

For a context allocated to communicate with a robot controller, you can choose to set the [`InitFlag`](../../Reference/com/McomAlloc.md) parameter to [`M_COM_NO_CONNECT`](../../Reference/com/McomAlloc.md) so that you do not connect automatically upon allocation. If this option is chosen, you must use [`McomControl`](../../Reference/com/McomControl.md) with [`M_COM_ROBOT_CONNECT`](../../Reference/com/McomControl.md) to manually establish the connection with the robot controller.

> **Note:** This function is only available if you have the Aurora Imaging Library Industrial & Robot Communications package, or another relevant update installed.

After allocating the Industrial Communication context, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the Industrial Communication context identifier returned is not [`M_NULL`](../../Reference/com/McomAlloc.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/com/McomAlloc.md) was specified).

When the Industrial Communication context is no longer required, release it using[`McomFree`](../../Reference/com/McomFree.md)unless [`M_UNIQUE_ID`](../../Reference/com/McomAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/com/McomAlloc.md) was specified, the smart identifier manages the Industrial Communication context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the Industrial Communication context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ProtocolType` *(in, AIL_INT64)*

Specifies the type of protocol to use.

*For specifying the type of protocol*

| Value | Description |
| --- | --- |
| `M_COM_PROTOCOL_ABB` | Specifies an ABB robot controller protocol. |
| `M_COM_PROTOCOL_CCLINK` | Specifies a CC-Link IE Field Basic protocol. |
| `M_COM_PROTOCOL_DENSO` | Specifies a DENSO robot controller protocol. |
| `M_COM_PROTOCOL_EPSON` | Specifies an Epson robot controller protocol. |
| `M_COM_PROTOCOL_ETHERNETIP` | Specifies an Ethernet/IP protocol. |
| `M_COM_PROTOCOL_FANUC` | Specifies a Fanuc robot controller protocol. |
| `M_COM_PROTOCOL_KUKA` | Specifies a KUKA robot controller protocol. |
| `M_COM_PROTOCOL_MODBUS` | Specifies a Modbus protocol. |
| `M_COM_PROTOCOL_PROFINET` | Specifies a PROFINET protocol. |
| `M_COM_PROTOCOL_STAUBLI` | Specifies a Staubli robot controller protocol. |

### `DeviceDescription` *(in, AIL_CONST_TEXT_PTR)*

Specifies the description of the device with which to communicate. This parameter must be set to the following:

*For describing the device*

| Value | Description |
| --- | --- |
| `"M_DEFAULT"` | Specifies to communicate with the default device described in Aurora Imaging Configurator that uses the specified protocol. If the protocol selected in the [`ProtocolType`](../../Reference/com/McomAlloc.md) parameter is not enabled in the Aurora Imaging Configurator utility (for the non-robot controller types of Industrial Communication contexts), an error is returned. |
| `"InstanceName"` | Specifies the instance name of a given Industrial Communication protocol. The name and other description/value pairs can be found on the**Communication** page of the Aurora Imaging Configurator utility on the runtime platform. Use the instance name when there is more than one instance of a given protocol listed in Aurora Imaging Configurator; otherwise, use [`"M_DEFAULT"`](../../Reference/com/McomAlloc.md). |
| `"IPOrName:Port"` | Specifies the IP address and port on the robot controller to use to communicate with it. The Aurora Imaging Library Industrial Communication module supports IPv4 addresses. A typical IPv4 string has the format n.n.n.n, where n is a number between 0 and 255. |

### `InitFlag` *(in, AIL_INT64)*

Specifies how to initialize the Industrial Communication context.

*For specifying the type of initialization*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior.

This is the only possible setting when allocating a context to communicate with a non-robot controller device. |
| `M_COM_NO_CONNECT` | Specifies not to automatically connect to the robot controller. |

### `InitValue` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ComIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Industrial Communication context identifier or specifies the data type that the function should use to return the Industrial Communication context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Industrial Communication
                              context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Industrial Communication
                              context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_COM_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Industrial Communication
                              context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated Industrial Communication context.

If allocation fails, [`M_NULL`](../../Reference/com/McomAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Industrial Communication context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_COM_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/com/McomAlloc.md) was specified).

This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7.
