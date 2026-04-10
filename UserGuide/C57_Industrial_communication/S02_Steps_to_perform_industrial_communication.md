---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Steps_to_perform_industrial_communication
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Steps to perform industrial communication"
---

# Steps to perform industrial communication

The following steps provide a basic methodology for using the Aurora Imaging Library Industrial Communication module:

1. For applications communicating with a robot, perform the following steps:
   1. Allocate an industrial communication context, using [`McomAlloc`](../../Reference/com/McomAlloc.md). The industrial communication context allows you to specify with which type of robot you will be communicating.
   2. Wait to receive a request for a new position from the robot using [`McomWaitPositionRequest`](../../Reference/com/McomWaitPositionRequest.md).
   3. Perform some analysis and calculate a new position to send back to the robot.
   4. Send the new position to the robot using [`McomSendPosition`](../../Reference/com/McomSendPosition.md).
2. For applications communicating in worker configuration with a controller that uses a communications protocol standard (such as PROFINET), perform the following steps:
   1. If required, configure the controller's software so that the controller recognizes the local computer (computer running the Aurora Imaging Library application) with the appropriate number of data tables (such as modules, data tables, assemblies). Note that the local computer should have a static IP address.
   2. On the local computer, create, configure, and enable an instance of the industrial communication protocol service using the Aurora Imaging Configurator utility's **Communication** item. For more information visit [Creating, configuring, and enabling the communication protocol service](S05_Creating_configuring_and_enabling_the_communication_protocol_service.md).
   3. Allocate an industrial communication context, using [`McomAlloc`](../../Reference/com/McomAlloc.md). The industrial communication context allows you to specify with which type of protocol you will be communicating.
   4. Read data received from the controller during its last write cycle using the same communications protocol standard (such as PROFINET) as the context, using [`McomRead`](../../Reference/com/McomRead.md).
   5. Perform some analysis and calculate new data to write back to the controller.
   6. Write data to be read from the controller during its next read cycle, using [`McomWrite`](../../Reference/com/McomWrite.md).
3. Free your industrial communication context, using [`McomFree`](../../Reference/com/McomFree.md), unless [`M_UNIQUE_ID`](../../Reference/com/McomAlloc.md) was specified during allocation.

> **Note:** Note that if you are using the Aurora Imaging Library application in a Modbus client configuration, the steps required to perform industrial communication are similar to that of a server. To see the differences between the two, see the [Setting up the environment to use the local computer as a Modbus client](S07_Industrial_communication_with_Modbus.md) and [Reading and writing to servers as a Modbus client](S07_Industrial_communication_with_Modbus.md).

> **Note:** Note that industrial communication using PROFINET is only available when using an Aurora Imaging Library system that has a hardware PROFINET engine (for example, a Zebra Indio system, a Zebra Iris GTX system, or a Host system running on a Zebra 4Sight EV6 or EV7).
