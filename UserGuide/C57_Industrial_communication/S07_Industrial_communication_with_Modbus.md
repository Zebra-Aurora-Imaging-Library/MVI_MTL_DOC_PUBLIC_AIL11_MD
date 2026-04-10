---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Industrial_communication_with_Modbus
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Industrial communication with Modbus"
---

# Industrial communication with Modbus

Aurora Imaging Library supports communication with the Modbus industrial communication protocol. Modbus is an industrial communication standard for connecting devices together, and it defines a communication protocol that allows interaction between devices used in industrial automation processes. This information is received or transmitted through the computer's (or smart camera's) 100 Mbit/1 Gbit Ethernet port or serial port (COM port). The Aurora Imaging Library implementation of the Modbus protocol allows you to designate the local computer (or smart camera) running Aurora Imaging Library as being either a client (Zebra Modbus automation client) or a server (Zebra Modbus automation server) in the network topology.

## Setting up the environment to use the local computer as a Modbus server

To use the Modbus industrial communication protocol with your Aurora Imaging Library application, you need to set up the environment on the Modbus client's side and on the local computer's side.

You must configure the Modbus client (using its appropriate software package) to recognize the local computer as a server. Configure the Modbus client with a user-defined identifier for the local computer. The identifier that you select must be unique on the network (that is, does not conflict with another device's ID). If using Modbus TCP, also configure the Modbus client with the local computer's IP address; the local computer's IP address should be static.

Once the Modbus client has been configured to recognize the local computer as a server, you must enable and configure an instance of the Modbus protocol service using the Aurora Imaging Configurator utility's **Communication** item. You must select the communication type for your network, and the mode in which the local computer will be running (in this case, server). The Modbus types supported in the Aurora Imaging Library implementation of the protocol are as follows, and should correspond to that used on the network:

- Modbus ASCII.
- Modbus Remote Terminal Unit (RTU).
- Modbus TCP.

You must set the **ID** item to the user-defined identifier that you associated with the local computer when configuring the Modbus client.

If you select the communication type to be either ASCII or RTU, you must also select the serial port on the local computer from which to communicate (COM port), baud rate, parity (odd or even), stop bits (1 or 2), and the serial communication standard used (RS232 or RS485).

If you select the communication type to be TCP, and you have multiple network adapters in your computer, you must also configure the **Interface** item, otherwise you can use the default selection.

*[Image: Communication-overview-Modbus-Worker.png]*

## Receiving and sending data to the client as a Modbus server

Once the instance of the Modbus communication protocol service has been enabled and configured using the Aurora Imaging Configurator utility, you can allocate an Industrial Communication context for use with Modbus ([`McomAlloc`](../../Reference/com/McomAlloc.md) with [`M_COM_PROTOCOL_MODBUS`](../../Reference/com/McomAlloc.md)) and use the [`McomWrite`](../../Reference/com/McomWrite.md) and [`McomRead`](../../Reference/com/McomRead.md) functions to communicate with the Modbus client.

Note that when you set the local computer as a server, [`McomWrite`](../../Reference/com/McomWrite.md) actually writes to data tables in local memory the data to send to the Modbus client on its next read cycle. Similarly, as a server, [`McomRead`](../../Reference/com/McomRead.md) actually reads the data tables in local memory for data received from the Modbus client during its last write cycle. So, when you call [`McomWrite`](../../Reference/com/McomWrite.md)/[`McomRead`](../../Reference/com/McomRead.md), you must specify the data table to write/read.

The Modbus data tables are created in local memory when the Modbus service is enabled and configured to be in server mode, using the Aurora Imaging Configurator utility. The supported Modbus data tables are:

- [`M_COILS`](../../Reference/com/McomRead.md).
- [`M_DISCRETE_INPUT`](../../Reference/com/McomRead.md).
- [`M_HOLDING_REGISTER`](../../Reference/com/McomRead.md).
- [`M_INPUT_REGISTER`](../../Reference/com/McomRead.md).

The coil and discrete input data tables support 1-bit data, while the input and holding register data tables support 16-bit data. The Modbus client can write to either the coil or the holding register data tables, and can read from all the data tables. As such, you should expect new data received from the Modbus client in the coil ([`M_COILS`](../../Reference/com/McomRead.md)) and holding register ([`M_HOLDING_REGISTER`](../../Reference/com/McomRead.md)) data tables. The local computer can write to any of the four data tables.

You can, for example, use [`McomRead`](../../Reference/com/McomRead.md) to receive a request from a Modbus client to grab and analyze an image, and then return the analysis results, necessary for the automation process, back to the Modbus client using [`McomWrite`](../../Reference/com/McomWrite.md).

### Data tables and their data fields

The Aurora Imaging Library Industrial Communication module is very flexible, knowing that different applications need to send/receive different information to/from the Modbus client. As such, it allows you to establish how to divide these data tables into different data fields (register fields). For example, you could organize your data as follows:

- Holding register data table (the client writes to this module):
  - PLC ready bit. The PLC should set this bit when it's ready for production.
  - PLC error bit. The PLC should set this bit when it is in an error state.
  - PLC reset bit. The PLC should set this bit when it is about to be reset.
  - Trigger bit. The PLC should set this bit when it wants the Aurora Imaging Library application to initiate a grab and process operation.
  - Result acknowledge bit. The PLC should set this bit when it acknowledges that it has received a result from a grab and process operation.
- Input register data table (the client reads from this module):
  - Success bit. The Aurora Imaging Library application should set this bit when the grab and processing was successful.
  - Failure bit. The Aurora Imaging Library application should set this bit when the grab and processing was not successful.
  - Error bit. The Aurora Imaging Library application should set this bit when there was an error in the grab and processing operations.
  - Project running bit. The Aurora Imaging Library application should set this bit while a grab and process operation is in progress.
  - Success number bits. The Aurora Imaging Library application should set these bits to identify the number of successful operations.
  - Failure number bits. The Aurora Imaging Library application should set these bits to identify the number of failed operations.

Note that this is only an example of how you can organize your data in the data tables. In addition, the software running on the Modbus client (for example, a ladder logic software) must be aware of how the data is organized in the data tables.

## Setting up the environment to use the local computer as a Modbus client

When the local computer running Aurora Imaging Library is using the Modbus protocol in a client configuration that uses TCP, you must configure the **Interface** item in the Aurora Imaging Configurator utility to the network adapter that should be used to communicate. For better performance, you can also specify all the devices on the Modbus network in the Aurora Imaging Configurator utility; in this case, for every server on the Modbus network, specify their Server IDs and their IP addresses in the Aurora Imaging Configurator utility.

When the local computer running Aurora Imaging Library in a client configuration is not using the TCP protocol (that is, ASCII or RTU), you must select the serial port on the local computer from which to communicate (COM port), baud rate, parity (odd or even), stop bits (1 or 2), and the serial communication standard used (RS232 or RS485).

*[Image: Communication-overview-Modbus-controller.png]*

## Reading and writing to servers as a Modbus client

If the local computer is configured as a client, it can only write to a server's coils or holding registers. The local computer can read from any of the server's four data tables listed in the previous section. When reading or writing data to a server on a Modbus TCP network, you must specify both the device's server ID and its IP address, along with the data table with which you intend to communicate. When reading or writing data to a server on a Modbus ASCII or RTU network, you must only specify the device's server ID, along with the data table with which you intend to communicate.

The function codes that Aurora Imaging Library uses to read and write to a server, when the local computer is a client, are as follows:

|   |   |   |
| --- | --- | --- |
| Data table | Read | Write |
| Coil | 01 | 15 |
| Discrete Input | 02 | N/A |
| Input Register | 04 | N/A |
| Holding Register | 03 | 16 |
