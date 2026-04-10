---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Creating_configuring_and_enabling_the_communication_protocol_service
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Creating configuring and enabling the communication protocol service"
---

# Creating, configuring, and enabling the communication protocol service

The Aurora Imaging Library Industrial Communication module allows one or more controllers to communicate with Aurora Imaging Library applications running on a single computer. This is done by creating, configuring, and enabling instances of the industrial communication protocol service, along with creating and running the Aurora Imaging Library applications.

Typically, you have a single controller communicating with your local computer. In this case, you need to create, configure, and enable a single instance of the required industrial communication protocol service, using the **Communication** page of the Aurora Imaging Configurator utility on the runtime platform.

If you want multiple controllers communicating with Aurora Imaging Library applications running on a single computer, you need to create, configure, and enable as many instances of the protocol service as controllers. To enable multiple Industrial Communication instances, repeat the [steps to perform industrial communication](S02_Steps_to_perform_industrial_communication.md) as many times as required for each instance.

*[Image: Ethernet_Protocol_IP_Config.png]*

To specify the instance with which your Aurora Imaging Library application should communicate, pass the instance name to the [`DeviceDescription`](../../Reference/com/McomAlloc.md) parameter of [`McomAlloc`](../../Reference/com/McomAlloc.md) when allocating the industrial communication context.

> **Note:** Note that, industrial communication protocol instance names are case sensitive.

Each instance of the protocol service has its own data tables and an interface setting that indicates the NIC or serial port (depending on the protocol) on which to communicate. Typically, each instance should use its own separate and unique network interface (NIC) or serial port. Note that some NICs have several interfaces (for example, Zebra Concord PoE).

When using the EtherNet/IP and Modbus industrial protocols, it can be possible to use virtual NICs (virtual interfaces), with Microsoft Windows 10. In this case, you can create multiple virtual NICs on 1 physical NIC. You can then use a virtual NIC when specifying the interface for your instance. Note that, once you use the physical interface for an instance, you cannot then create virtual interfaces on that NIC.

You should typically use the following settings depending on the industrial protocol:

- For EtherNet/IP, set the sizes of the **Producer**and **Consumer**assemblies to values higher than the highest address in your data map plus the size of that data field (for example, if your project will write a byte of data to the PLC at the address 92, set the **Consumer**assembly size to 93). The maximum assembly size supported is 1502.
- For PROFINET, set the sizes of the **Input**and **Output**modules to the next available value higher than the highest address in your data map plus the size of that data field (for example, if your project will write a byte of data to the PLC at the address 92, the minimum size required is 93 so you should set the **Output**module size to 128).
- For CC-Link, set the **Number of stations** to a value between 1 and 16, depending on your project setup.

Once the protocol instance is added, the status of the protocol instance should be **Ready**. If the status is **Waiting NIC**, there is no cable connected to the NIC.

> **Note:** Note that to enable multiple instances of the PROFINET protocol service on a computer, the computer requires a PROFINET engine (for example, a Zebra Indio board) for each Industrial Communication instance you enable.
