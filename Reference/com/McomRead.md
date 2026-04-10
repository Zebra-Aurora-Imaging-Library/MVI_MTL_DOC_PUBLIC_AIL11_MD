---
doctype: Reference
module: com
function: McomRead
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / com / McomRead"
---

# McomRead

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

> Read data from a device that uses the specified communication protocol.

## Syntax

```c
void McomRead(
    AIL_ID             ComId,                //in
    AIL_CONST_TEXT_PTR DataObjectEntryName,  //in
    AIL_INT            Offset,               //in
    AIL_INT            Size,                 //in
    void *             UserArrayPtr          //out
)
```

## Description

This function reads data from a device that uses the specified communication protocol. In most cases, this function actually reads the local computer's memory for data received from a controller (e.g. a PLC) during the controller's last write cycle. However, when communicating with the device using the Modbus protocol in client configuration, or reading configuration data from an EtherNet/IP device explicitly, this function can read the data directly from the device's memory. This function can also read the data last written to local memory using [`McomWrite`](../../Reference/com/McomWrite.md).

A controller that uses the PROFINET protocol writes data to the local computer in modules in local memory. As such, you must specify the module's index from which to read the received data.

A client that uses the Modbus protocol writes data to the local computer in data tables in local memory, when the local computer is in a server configuration. To read from a Modbus device (either in a server or client configuration), you must specify the data table from which to read.

A client that uses the EtherNet/IP protocol writes data to the local computer, typically, in the consumer assembly in local memory. As such, to read data received from an EtherNet/IP client, specify to read from the consumer assembly in local memory. You can also read from the producer or configuration assembly in local memory. When reading from a producer assembly, you are reading your own published data in local memory. When reading from a configuration assembly, you are reading EtherNet/IP configuration data in local memory about the local computer. Note that you can also specify another device on the network from which to read data explicitly, although this should only be used to read configuration data.

A controller that uses the CC-Link IE Field Basic protocol writes data to the local computer, in register and flag data tables in local memory. As such, to read data received from a CC-Link controller, you must specify whether to read the data in the flags or registers data table. The local computer can only be configured as a worker device.

For PROFINET and EtherNet/IP, use [`McomInquire`](../../Reference/com/McomInquire.md) with [`M_COM_PROFINET_GET_MODULE_OUTSIZE`](../../Reference/com/McomInquire.md) or [`M_COM_ETHERNETIP_CONSUMER_SIZE`](../../Reference/com/McomInquire.md) to establish the size of the output module, or assembly, respectively, to which the controller has written. You must know how the module/assembly is divided into fields.

For CC-Link IE Field Basic, use [`McomInquire`](../../Reference/com/McomInquire.md) with [`M_COM_CCLINK_TOTAL_OCCUPIED_STATIONS`](../../Reference/com/McomInquire.md) to learn the number of stations used by the industrial protocol instance. To establish the number of registers (RWw) or flags (RY) to which the controller can write, multiply the number of stations by 32 or 64 respectively.

If the time to execute this function is greater than the value set using [`McomControl`](../../Reference/com/McomControl.md) with [`M_COM_TIMEOUT`](../../Reference/com/McomControl.md), an error occurs.

> **Note:** This function is only available if you have the Aurora Imaging Library Industrial & Robot Communications package, or another relevant update installed.

## Parameters

### `ComId` *(in, AIL_ID)*

Specifies the identifier of the Industrial Communication context that specifies the protocol of the device with which to communicate.

### `DataObjectEntryName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the location from which to read the data. This location is protocol specific. This parameter must be set to one of the following.

*For specifying the path of the data to read in PROFINET*

| Value | Description |
| --- | --- |
| `"mcom://ModuleIndex"` | Specifies the location in local memory from which to read the data received from a controller. Replace _ModuleIndex_ with the index of the module in local memory from which to read the data. _ModuleIndex_ can be from 0 to 255. |

*For specifying the path of the data to read in Modbus*

| Value | Description |
| --- | --- |
| `M_COILS` | Specifies to read the coil data table in local memory for data received from a Modbus client, or written by the last call to [`McomWrite`](../../Reference/com/McomWrite.md) with [`M_COILS`](../../Reference/com/McomWrite.md). |
| `M_DISCRETE_INPUT` | Specifies to read the discrete input table in local memory for data written by the last call to [`McomWrite`](../../Reference/com/McomWrite.md) with [`M_DISCRETE_INPUT`](../../Reference/com/McomWrite.md). |
| `M_HOLDING_REGISTER` | Specifies to read the holding register table in local memory for data received from a Modbus client, or written by the last call to [`McomWrite`](../../Reference/com/McomWrite.md) with [`M_HOLDING_REGISTER`](../../Reference/com/McomWrite.md). |
| `M_INPUT_REGISTER` | Specifies to read the input register table in local memory for data written by the last call to [`McomWrite`](../../Reference/com/McomWrite.md) with [`M_INPUT_REGISTER`](../../Reference/com/McomWrite.md). |
| `"mcom://ServerId[@ServerIP]/M_COILS"` | Specifies to read the coil data table of a server, when the local computer is communicating using the Modbus protocol in client configuration. |
| `"mcom://ServerId[@ServerIP]/M_DISCRETE_INPUT"` | Specifies to read the discrete input data table of a server, when the local computer is communicating using the Modbus protocol in client configuration. |
| `"mcom://ServerId[@ServerIP]/M_HOLDING_REGISTER"` | Specifies to read the holding register data table of a server, when the local computer is communicating using the Modbus protocol in client configuration. |
| `"mcom://ServerId[@ServerIP]/M_INPUT_REGISTER"` | Specifies to read the input register data table of a server, when the local computer is communicating using the Modbus protocol in client configuration. |

*For specifying the path of the data to read in EthernetIP*

| Value | Description |
| --- | --- |
| `M_COM_ETHERNETIP_CONFIG` | Specifies to read the configuration assembly in local memory for EtherNet/IP configuration data typically written by the last call to [`McomWrite`](../../Reference/com/McomWrite.md). |
| `M_COM_ETHERNETIP_CONSUMER` | Specifies to read the consumer assembly in local memory for data received from an EtherNet/IP client. |
| `M_COM_ETHERNETIP_PRODUCER` | Specifies to read the producer assembly in local memory for data written by the last call to [`McomWrite`](../../Reference/com/McomWrite.md). |
| `"mcom://[RemoteIP/]AssemblyID"` | Specifies another server on the network and the assembly from which to read the data when communicating using the Ethernet/IP protocol.

Replace _[RemoteIP]_ with the server's IP address; the Aurora Imaging Library Industrial Communication module supports IPv4 addresses. A typical IPv4 string has the format n.n.n.n, where n is a number between 0 and 255.

This method uses explicit messaging to read from a particular device's specified assembly. While this is supported in Aurora Imaging Library, this method will be slower than using implicit messaging. This should, in theory, only be used to read a configuration setting on the specified device. |

*For specifying the path of the data to read in CC-Link IE Field Basic*

| Value | Description |
| --- | --- |
| `M_COM_CCLINK_INPUT_FLAG` | Specifies to read the input flag (RX) data table in local memory for data written by the last call to [`McomWrite`](../../Reference/com/McomWrite.md). |
| `M_COM_CCLINK_INPUT_REGISTER` | Specifies to read the input flag (RWr) data table in local memory for data written by the last call to [`McomWrite`](../../Reference/com/McomWrite.md). |
| `M_COM_CCLINK_OUTPUT_FLAG` | Specifies to read the output flag (RY) data table in local memory for data received from a CC-Link IE Field Basic controller. |
| `M_COM_CCLINK_OUTPUT_REGISTER` | Specifies to read the output register (RWw) data table in local memory for data received from a CC-Link IE Field Basic controller. |

### `Offset` *(in, AIL_INT)*

Specifies the offset from which to start reading the data in the specified module/data table/assembly. Typically, the offset is specified in bytes. The following exceptions apply:

### `Size` *(in, AIL_INT)*

Specifies the amount of the data to read. Typically, the size is specified in bytes. The following exceptions apply:

### `UserArrayPtr` *(out, *void)*

Specifies the address of the array in which to write the data.

Replace _ServerId_ with the server's identifier number.

If connecting to a server over the Modbus TCP protocol, also enter _@_ and replace _[ServerIP]_ with the server's IP address; the Aurora Imaging Library Industrial Communication module supports IPv4 addresses. A typical IPv4 string has the format n.n.n.n, where n is a number between 0 and 255.

This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7.
