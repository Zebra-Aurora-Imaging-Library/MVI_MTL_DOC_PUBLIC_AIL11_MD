---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Industrial_communication_with_CC_Link_IE_Field_Basic
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Industrial communication with CC Link IE Field Basic"
---

# Industrial communication with CC-Link IE Field Basic

Aurora Imaging Library supports communication with the CC-Link IE Field Basic industrial communication protocol. CC-Link IE Field Basic is an industrial communication standard for connecting devices together, and it defines a communication protocol that allows interaction between devices used in industrial automation processes. This information is received or transmitted through the computer's (or smart camera's) 100 Mbit/1 Gbit Ethernet port. When using this protocol with Aurora Imaging Library, your local computer is considered to be a worker in the network, and the CC-Link IE Field Basic controller (typically a PLC) is considered to be the controller.

## Setting up the environment

To use the CC-Link IE Field Basic industrial communication protocol with your Aurora Imaging Library application, you need to set up the environment on the CC-Link IE Field Basic controller's side and on the local computer's side.

*[Image: Communication-overview-cc-link.png]*

You must configure your CC-Link IE Field Basic controller to recognize the local computer as a worker device. To do so, use appropriate configuration software such as **Mitsubishi GX Works** and specify the local computer's IP address. The local computer's IP address should be static. In addition, you can use the provided **CSPP** profile, and specify the number of stations that your computer will occupy (this determines the size of the data tables you can read from and write to). The **CSPP** profile can be found as a _ZIP_ file in the installed CC-Link config directory (for example, _C:\Program Files\Aurora Imaging Library\11\Library\Config\CCLink_).

The provided **CSPP**profiles are specific to your platform type, and the Aurora Imaging Library version installed on your computer; if you later update your computer to use a new version of Aurora Imaging Library, you must load the new **CSPP**profile for that version. The table below lists the _ZIP_ file that stores the **CSPP**profile required for each platform:

| System type/Host platform | ZIP file |
| --- | --- |
|  |
| Zebra Iris GTX | _0x0E0B_Iris_GTX_1.0.0_en.CSPP.zip_ |
| Zebra 4Sight EV6 | _0x0E0B_4Sight_EV6_1.0.0_en.CSPP.zip_ |
| Zebra 4Sight EV7 | _0x0E0B_4Sight_EV7_1.0.0_en.CSPP.zip_ |
| Other PC | _0x0E0B_Generic_PC_1.0.0_en.CSPP.zip_ |

> **Note:** Typically, you should not unzip the **CSPP**profile.

Once the CC-Link IE Field Basic controller has been configured to recognize the local computer as a worker, you must enable and configure an instance of the CC-Link IE Field Basic protocol service using the Aurora Imaging Configurator utility's **Communication** item. If the local computer has multiple network adapters, use the **Interface** item to specify which should be used. Use the**Number of stations**item to specify how many stations on the controller your application will use (if you are using the provided **CSPP** profile for your platform, you must set this to 4).

## Receiving and sending data to the controller

To communicate with the CC-Link IE Field Basic controller, allocate an Industrial Communication context using [`McomAlloc`](../../Reference/com/McomAlloc.md) with [`M_COM_PROTOCOL_CCLINK`](../../Reference/com/McomAlloc.md). To send data to the CC-Link IE Field Basic controller, use the [`McomWrite`](../../Reference/com/McomWrite.md) function and specify the [`M_COM_PROTOCOL_CCLINK`](../../Reference/com/McomAlloc.md) Industrial Communication context that you have just allocated. The [`McomWrite`](../../Reference/com/McomWrite.md) function actually writes the data to the local computer's data tables; the data stored in these tables is read by the CC-Link IE Field Basic controller on its next read cycle. To read data received from the CC-Link IE Field Basic controller, use the [`McomRead`](../../Reference/com/McomRead.md) function and specify the [`M_COM_PROTOCOL_CCLINK`](../../Reference/com/McomAlloc.md) Industrial Communication context. The [`McomRead`](../../Reference/com/McomRead.md) function actually reads the data from the local computer's data tables; the data stored in these data tables is written by the CC-Link IE Field Basic controller during its write cycle. When you call [`McomWrite`](../../Reference/com/McomWrite.md)/[`McomRead`](../../Reference/com/McomRead.md), you must specify the data table in which to write, or from which to read.

The CC-Link IE Field Basic data tables are created in local memory when an instance of the CC-Link IE Field Basic service is enabled using the Aurora Imaging Configurator utility. The supported CC-Link IE Field Basic data tables are:

- [`M_COM_CCLINK_INPUT_FLAG`](../../Reference/com/McomRead.md).
- [`M_COM_CCLINK_INPUT_REGISTER`](../../Reference/com/McomRead.md).
- [`M_COM_CCLINK_OUTPUT_FLAG`](../../Reference/com/McomRead.md).
- [`M_COM_CCLINK_OUTPUT_REGISTER`](../../Reference/com/McomRead.md).

The flag data tables support 1-bit data, while the register data tables support 16-bit data.

CC-Link IE Field Basic is a polling protocol and as such, it should be noted that whatever is written to an input data table might not be read by the CC-Link controller if another [`McomWrite`](../../Reference/com/McomWrite.md) operation occurs before the CC-Link IE Field Basic controller's read cycle is completed.

### Data tables and their data fields

The Aurora Imaging Library Industrial Communication module is very flexible, knowing that different applications need to send/receive different information to/from the CC-Link IE Field Basic controller. As such, it allows you to establish how to divide these data tables into different data fields. For example, you could organize your data as follows:

- Output flag data table (the controller writes to this data table):
  - PLC ready bit. The PLC should set this bit when it is ready for production.
  - PLC error bit. The PLC should set this bit when it is in an error state.
  - PLC reset bit. The PLC should set this bit when it is about to be reset.
  - Trigger bit. The PLC should set this bit when it wants the Aurora Imaging Library application to initiate a grab and process operation.
  - Result acknowledge bit. The PLC should set this bit when it acknowledges that it has received a result from a grab and process operation.
- Input flag data table (the controller reads from this module):
  - Success bit. The Aurora Imaging Library application should set this bit when the grab and processing was successful.
  - Failure bit. The Aurora Imaging Library application should set this bit when the grab and processing was not successful.
  - Error bit. The Aurora Imaging Library application should set this bit when there was an error in the grab and processing operations.
  - Project running bit. The Aurora Imaging Library application should set this bit while a grab and process operation is in progress.
  - Success number bits. The Aurora Imaging Library application should set these bits to identify the number of successful operations.
  - Failure number bits. The Aurora Imaging Library application should set these bits to identify the number of failed operations.

Note that this is only an example of how you can organize your data in the data tables. In addition, the software running on the CC-Link IE Field Basic controller (for example, a ladder logic software) must be aware of how the data is organized in the data tables.
