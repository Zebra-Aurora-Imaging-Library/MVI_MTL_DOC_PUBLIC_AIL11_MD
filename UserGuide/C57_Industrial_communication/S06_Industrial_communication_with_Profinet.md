---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Industrial_communication_with_Profinet
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Industrial communication with Profinet"
---

# Industrial communication with PROFINET

Aurora Imaging Library supports communication with the PROFINET industrial protocol. PROFINET is an industrial communication standard for connecting devices together, and it defines a communication protocol that allows interaction between devices used in industrial automation processes. Information is received or transmitted through the computer's (or smart camera's) Ethernet port that has access to a Zebra PROFINET Engine. When using this protocol with Aurora Imaging Library, your Zebra PROFINET Engine is considered to be a worker in the network, while the PROFINET controller (for example, a PLC) is considered the controller.

> **Note:** Note that industrial communication using PROFINET is only available when using an Aurora Imaging Library system that has a hardware PROFINET engine; for example, a Zebra Indio system, a Zebra Iris GTX system, or a Host system running on a Zebra 4Sight EV6 or EV7.

## Setting up the environment

To use the PROFINET industrial communication protocol with your Aurora Imaging Library application, you need to set up the environment on the PROFINET controller's side and on the local computer's side.

You must configure your PROFINET controller to recognize the local computer as a Zebra PROFINET automation device. To do so, use the **Siemens Totally Integrated Automation Portal** (TIA) and specify the local computer's IP address. The local computer's IP address should be static. In addition, use the provided **GSD** file, and specify the number and size of input and output modules (data tables) that the PROFINET controller should expect your computer to have. The **GSD** file can be found in the installed Profinet config directory (for example, _C:\Program Files\Aurora Imaging Library\11\Library\Config\Profinet_). The PROFINET controller sends or receives data to/from the local computer through output and input modules in the computer's local memory, respectively. The default module size for the PROFINET implementation in Aurora Imaging Library is 64 bytes. The required size is dependent on the information you intend to store in these modules. See [Memory modules and their data fields](S06_Industrial_communication_with_Profinet.md) for more information.

The designation of input and output modules is from the perspective of the PROFINET controller. The PROFINET protocol states that data which the PROFINET controller receives is stored in an input module, and data sent by a PROFINET controller is stored in an output module.

*[Image: Communication-overview-Profinet.png]*

Once the PROFINET controller has been configured to recognize the local computer as a Zebra PROFINET automation device, you must enable the PROFINET protocol service using the Aurora Imaging Configurator utility's **Communication** item. Then, if the local computer has multiple network adapters with access to a Zebra PROFINET Engine, use the **Interface** item to specify which should be used.

Now, when you reboot your PROFINET controller, it will recognize your local computer as a Zebra PROFINET automation device with the modules specified.

## Receiving and sending data to the controller

To communicate with the PROFINET controller, allocate an Industrial Communication context using [`McomAlloc`](../../Reference/com/McomAlloc.md) with [`M_COM_PROTOCOL_PROFINET`](../../Reference/com/McomAlloc.md). To send data to the PROFINET controller, use the [`McomWrite`](../../Reference/com/McomWrite.md) function and specify the [`M_COM_PROTOCOL_PROFINET`](../../Reference/com/McomAlloc.md) Industrial Communication context that you have just allocated. In this case, this function actually writes to the local computer's memory the data to send to the PROFINET controller during the controller's next read cycle. To read data received from the PROFINET controller, use the [`McomRead`](../../Reference/com/McomRead.md) function and specify the [`M_COM_PROTOCOL_PROFINET`](../../Reference/com/McomAlloc.md) Industrial Communication context. In this case, this function actually reads the local computer's memory for data received from a PROFINET controller during the PROFINET controller's last write cycle. So when you call [`McomWrite`](../../Reference/com/McomWrite.md)/[`McomRead`](../../Reference/com/McomRead.md), you must specify the module in which to write, or from which to read.

PROFINET is a polling protocol and as such, it should be noted that whatever is written to an input module might not be read by the PROFINET controller if another [`McomWrite`](../../Reference/com/McomWrite.md) operation occurs before the PROFINET controller's read cycle is completed.

You can, for example, use [`McomRead`](../../Reference/com/McomRead.md) to receive a request from a PROFINET controller to grab and analyze an image, and then return the analysis results, necessary for the automation process, back to the PROFINET controller using [`McomWrite`](../../Reference/com/McomWrite.md).

### Memory modules and their data fields

The Aurora Imaging Library Industrial Communication module is very flexible, knowing that different applications need to send/receive different information to/from the PROFINET controller. As such, it allows you to establish the number of input/output modules to make available to the PROFINET controller, and how to divide these into different data fields (register fields). For example, you could specify in the Siemens TIA portal that there are 2 (PROFINET controller) input modules, and 2 (PROFINET controller) output modules, and organize your data in these modules as follows:

- Controller status output module (PLC writes to this module):
  - PLC ready bit. The PLC should set this bit when it's ready for production.
  - PLC error bit. The PLC should set this bit when it is in an error state.
  - PLC reset bit. The PLC should set this bit when it is about to be reset.
- Application control output module (PLC writes to this module):
  - Trigger bit. The PLC should set this bit when it wants the Aurora Imaging Library application to initiate a grab and process operation.
  - Result acknowledge bit. The PLC should set this bit when it acknowledges that it has received a result from a grab and process operation.
- Status input module (PLC reads from this module):
  - Success bit. The Aurora Imaging Library application should set this bit when the grab and processing was successful.
  - Failure bit. The Aurora Imaging Library application should set this bit when the grab and processing was not successful.
  - Error bit. The Aurora Imaging Library application should set this bit when there was an error in the grab and processing operations.
  - Project running bit. The Aurora Imaging Library application should set this bit while a grab and process operation is in progress.
  - Success number bits. The Aurora Imaging Library application should set these bits to identify the number of successful operations.
  - Failure number bits. The Aurora Imaging Library application should set these bits to identify the number of failed operations.

Note that this is only an example of how you can organize your data in the modules. In addition, the software running on the PROFINET controller (for example, a ladder logic software) must be aware of how the data is organized in the modules.
