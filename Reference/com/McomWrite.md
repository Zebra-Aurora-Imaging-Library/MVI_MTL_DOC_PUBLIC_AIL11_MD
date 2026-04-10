---
doctype: Reference
module: com
function: McomWrite
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / com / McomWrite"
---

# McomWrite

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

> Write data to a device that uses the specified communication protocol.

## Syntax

```c
void McomWrite(
    AIL_ID             ComId,                //in
    AIL_CONST_TEXT_PTR DataObjectEntryName,  //in
    AIL_INT            Offset,               //in
    AIL_INT            Size,                 //in
    const void *       UserArrayPtr          //in
)
```

## Description

This function writes data to a device that uses the specified communication protocol. In most cases, this function actually writes to the local computer's memory, the data to send to a controller (e.g. a PLC) during the controller's next read cycle. However, when communicating with the device using the Modbus protocol in client configuration, or writing configuration data to an EtherNet/IP device explicitly, this function can write the data directly to the device's memory.

A controller that uses the PROFINET protocol reads data from the local computer, from modules in local memory. As such, you must specify the module's index in which to write the data to send to the controller.

A client that uses the Modbus protocol reads data from the local computer, from data tables in local memory, when the local computer is in a server configuration. To write to a Modbus device (either in a server or client configuration), you must specify the data table in which to write. Note that when the local computer is configured as a client, you can only write to the coil or holding register data tables of a server.

A client that uses the EtherNet/IP protocol reads data from the local computer, from a producer assembly in local memory. As such, to send data to an EtherNet/IP client, specify to write to the producer assembly in local memory. You can also write to a configuration assembly. When writing to a configuration assembly, you are writing EtherNet/IP configuration data in local memory about the local computer.

> **Note:** For EtherNet/IP, you can also specify to write explicitly to another device on the network. However, this method is slower than using implicit messaging. You should only use explicit messaging for data that is not time critical; typically, this means only writing data to the configuration assemblies of other devices.

A controller that uses the CC-Link IE Field Basic protocol reads data from the local computer, from register and flag data tables in local memory. As such, to send data to a CC-Link controller, you must specify whether to write the data in the flags or registers data table. The local computer can only be configured as a worker device.

For PROFINET and EtherNet/IP, use [`McomInquire`](../../Reference/com/McomInquire.md) with [`M_COM_PROFINET_GET_MODULE_INSIZE`](../../Reference/com/McomInquire.md), [`M_COM_ETHERNETIP_PRODUCER_SIZE`](../../Reference/com/McomInquire.md) to establish the size of the input module, or assembly, respectively, from which the controller can read. You must know how the module/assembly is divided into fields.

For CC-Link IE Field Basic, use [`McomInquire`](../../Reference/com/McomInquire.md) with [`M_COM_CCLINK_TOTAL_OCCUPIED_STATIONS`](../../Reference/com/McomInquire.md) to learn the number of stations used by the industrial protocol instance. To establish the number of registers (RWr) or flags (RX) from which the controller can read, multiply the number of stations by 32 or 64 respectively.

> **Note:** This function is only available if you have the Aurora Imaging Library Industrial & Robot Communications package, or another relevant update installed.

## Parameters

### `ComId` *(in, AIL_ID)*

Specifies the identifier of the Industrial Communication context that specifies the protocol of the device with which to communicate.

### `DataObjectEntryName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the location in which to write the data. This location is protocol specific. This parameter must be set to one of the following.

*For specifying the path of the data to write in PROFINET*

| Value | Description |
| --- | --- |
| `"mcom://ModuleIndex"` | Specifies the location in local memory in which to write the data to send to a controller. Replace _ModuleIndex_ with the index of the input module in local memory in which to write the data. _ModuleIndex_ can be from 0 to 255. |

*For specifying the path of the data to write in Modbus*

| Value | Description |
| --- | --- |
| `M_COILS` | Specifies to write to the coils data table in local memory the data to send to the Modbus client. |
| `M_DISCRETE_INPUT` | Specifies to write to the discrete input data table in local memory the data to send to the Modbus client. |
| `M_HOLDING_REGISTER` | Specifies to write to the holding register data table in local memory the data to send to the Modbus client. |
| `M_INPUT_REGISTER` | Specifies to write to the input register data table in local memory the data to send to the Modbus client. |
| `"[mcom://ServerId[@ServerIP]/]M_COILS"` | Specifies to write to the coils data table of a server, when the local computer is communicating using the Modbus protocol in client configuration. |
| `"[mcom://ServerId[@ServerIP]/]M_HOLDING_REGISTER"` | Specifies to write to the holding register data table of a server, when the local computer is communicating using the Modbus protocol in client configuration. |

*For specifying the path of the data to write in EtherNet/IP*

| Value | Description |
| --- | --- |
| `M_COM_ETHERNETIP_CONFIG` | Specifies to write EtherNet/IP configuration data about the local computer to the configuration assembly in local memory. |
| `M_COM_ETHERNETIP_PRODUCER` | Specifies to write data to the producer assembly in local memory to send the data to the Ethernet/IP client. |
| `"mcom://[RemoteIP]/AssemblyID"` | Specifies the server on the network and the assembly in which to write the data when communicating using the Ethernet/IP protocol.

Replace _[RemoteIP]_ with the server's IP address; the Aurora Imaging Library Industrial Communication module supports IPv4 addresses. A typical IPv4 string has the format n.n.n.n, where n is a number between 0 and 255.

This method uses explicit messaging to write to a particular device's specified assembly. While this is supported in Aurora Imaging Library, this method will be slower than using implicit messaging. Therefore, in theory, this method should only be used to write configuration data to another server on the network. |

*For specifying the path of the data to write in CC-Link IE Field Basic*

| Value | Description |
| --- | --- |
| `M_COM_CCLINK_INPUT_FLAG` | Specifies to write the data to the input flag (RX) data table in local memory to send the data to the CC-Link IE field basic controller. |
| `M_COM_CCLINK_INPUT_REGISTER` | Specifies to write the data to the input register (RWr) data table in local memory to send the data to the CC-Link IE field basic controller. |

### `Offset` *(in, AIL_INT)*

Specifies the offset from which to start writing the data in the specified module/data table/assembly. Typically, the offset is specified in bytes. The following exceptions apply:

### `Size` *(in, AIL_INT)*

Specifies the size of the data to be written. Typically, the size is specified in bytes. The following exceptions apply:

### `UserArrayPtr` *(in, *void)*

Specifies the address of the array containing the data.

Replace _ServerId_ with the server's identifier number.

If connecting to a server over the Modbus TCP protocol, also enter _@_ and replace _[ServerIP]_ with the server's IP address; the Aurora Imaging Library Industrial Communication module supports IPv4 addresses. A typical IPv4 string has the format n.n.n.n, where n is a number between 0 and 255.

This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7.
