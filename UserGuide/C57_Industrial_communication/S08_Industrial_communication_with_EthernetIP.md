---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Industrial_communication_with_EthernetIP
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Industrial communication with EthernetIP"
---

# Industrial communication with EtherNet/IP

Aurora Imaging Library supports communication with the EtherNet/IP industrial communication protocol. EtherNet/IP is an industrial communication standard for connecting devices together, and it defines a communication protocol that allows interaction between devices used in industrial automation processes. This information is received or transmitted through the computer's (or smart camera's) 100 Mbit/1 Gbit Ethernet port. When using this protocol with Aurora Imaging Library, your local computer is considered to be an adapter (equivalent to server) in the network, and the EtherNet/IP client (typically a PLC) is typically considered to be the scanner.

## Setting up the environment

To use the EtherNet/IP industrial communication protocol with your Aurora Imaging Library application, you need to set up the environment on the EtherNet/IP client's side and on the local computer's side.

You must configure the EtherNet/IP client (using its appropriate software package) to recognize the local computer as a server. You must also configure the EtherNet/IP client with the local computer's IP address; the local computer's IP address should be static. An**EDS** file is provided and can be found in the installed Ethernet/IP config directory (for example, _C:\Program Files\Aurora Imaging Library\11\Library\Config\EthernetIP_).

*[Image: Communication-overview-EIP.png]*

Once the EtherNet/IP client has been configured to recognize the local computer as an Zebra EtherNet/IP automation device, you must enable the EtherNet/IP protocol service using the Aurora Imaging Configurator utility's **Communication** item. Then, if the local computer has multiple network adapters, use the **Interface** item to specify which should be used. The default assembly size on the local computer is 64 bytes, but can be changed in the Aurora Imaging Configurator utility.

## Receiving and sending data to the client

To communicate with the EtherNet/IP client, allocate an Industrial Communication context using [`McomAlloc`](../../Reference/com/McomAlloc.md) with [`M_COM_PROTOCOL_ETHERNETIP`](../../Reference/com/McomAlloc.md). To send data to the EtherNet/IP client, use the [`McomWrite`](../../Reference/com/McomWrite.md) function and specify the [`M_COM_PROTOCOL_ETHERNETIP`](../../Reference/com/McomAlloc.md) Industrial Communication context that you have just allocated. The [`McomWrite`](../../Reference/com/McomWrite.md) function actually writes to the local computer's assemblies the data to send to the EtherNet/IP client on its next read cycle. To read data received from the EtherNet/IP client, use the [`McomRead`](../../Reference/com/McomRead.md) function and specify the [`M_COM_PROTOCOL_ETHERNETIP`](../../Reference/com/McomAlloc.md) Industrial Communication context. The [`McomRead`](../../Reference/com/McomRead.md) function actually reads the local computer's assemblies for data received from the EtherNet/IP client during its last write cycle. So when you call [`McomWrite`](../../Reference/com/McomWrite.md)/[`McomRead`](../../Reference/com/McomRead.md), you must specify the assembly in which to write, or from which to read.

The EtherNet/IP assemblies are created in local memory when the EtherNet/IP service is enabled and configured using the Aurora Imaging Configurator utility. The supported assemblies on the local computer that can be accessed from the Aurora Imaging Library application are:

- [`M_COM_ETHERNETIP_CONSUMER`](../../Reference/com/McomRead.md).
- [`M_COM_ETHERNETIP_PRODUCER`](../../Reference/com/McomRead.md).
- [`M_COM_ETHERNETIP_CONFIG`](../../Reference/com/McomRead.md).

EtherNet/IP is a polling protocol (implicit producer) and as such, it should be noted that whatever is written to a local memory assembly (using [`McomWrite`](../../Reference/com/McomWrite.md) with [`M_COM_ETHERNETIP_PRODUCER`](../../Reference/com/McomWrite.md)) might not be read by the EtherNet/IP client if another [`McomWrite`](../../Reference/com/McomWrite.md) operation occurs before the EtherNet/IP client's read cycle is completed.

You can, for example, use [`McomRead`](../../Reference/com/McomRead.md) to receive a request from a EtherNet/IP client to grab and analyze an image, and then return the analysis results, necessary for the automation process, back to the EtherNet/IP client using [`McomWrite`](../../Reference/com/McomWrite.md).

### Assemblies and their data fields

The Aurora Imaging Library Industrial Communication module is very flexible, knowing that different applications need to send/receive different information to/from the EtherNet/IP client. As such, it allows you to establish how to divide the assemblies into different data fields (register fields). For example, you could organize data for your consumer and producer assemblies as follows:

- Consumer assembly:
  - PLC ready bit. The PLC should set this bit when its ready for production.
  - PLC error bit. The PLC should set this bit when it is in an error state.
  - PLC reset bit. The PLC should set this bit when it is about to be reset.
  - Trigger bit. The PLC should set this bit when it wants the Aurora Imaging Library application to initiate a grab and process operation.
  - Result acknowledge bit. The PLC should set this bit when it acknowledges that it has received a result from a grab and process operation.
- Producer assembly:
  - Success bit. The Aurora Imaging Library application should set this bit when the grab and processing was successful.
  - Failure bit. The Aurora Imaging Library application should set this bit when the grab and processing was not successful.
  - Error bit. The Aurora Imaging Library application should set this bit when there was an error in the grab and processing operations.
  - Project running bit. The Aurora Imaging Library application should set this bit while a grab and process operation is in progress.
  - Success number bits. The Aurora Imaging Library application should set these bits to identify the number of successful operations.
  - Failure number bits. The Aurora Imaging Library application should set these bits to identify the number of failed operations.

Note that this is only an example of how you can organize your data in the assemblies. In addition, the software running on the EtherNet/IP client (for example, a ladder logic software) must be aware of how the data is organized in the assemblies.

## Explicit communication with another device on an EtherNet/IP network

The only time when the local computer needs to know the IP address of another device is when connecting as an **Unconnected Message Manager** (UCMM) to configure a setting of another EtherNet/IP device on the network. While explicit communication using TCP is supported, the EtherNet/IP standard intends for this type of connection to be used to configure other devices, and not as a primary method of consuming data from a producing device.

*[Image: Communication-overview-EIP-explicit.png]*

To configure or read a setting of a device on the network, you must specify the following when calling [`McomWrite`](../../Reference/com/McomWrite.md)/[`McomRead`](../../Reference/com/McomRead.md):

- [`"mcom://[RemoteIP/]AssemblyID"`](../../Reference/com/McomRead.md).

In theory, you could explicitly consume data from another device by specifying its IP address and the device's relevant assembly ID, but this method will be slower than using implicit messaging. The EtherNet/IP protocol recommends to produce and consume data in an implicit manner. Implicit connections in EtherNet/IP are performed using the User Datagram Protocol (UDP).
