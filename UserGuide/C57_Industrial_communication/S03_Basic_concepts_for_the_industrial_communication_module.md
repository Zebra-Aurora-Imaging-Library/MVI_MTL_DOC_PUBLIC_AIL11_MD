---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Basic_concepts_for_the_industrial_communication_module
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Basic concepts for the industrial communication module"
---

# Basic concepts for the Aurora Imaging Library Industrial Communication module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Industrial Communication module are:

- **Controller**. A computer used for automation of industrial applications, such as controlling machinery on assembly lines.
- **Data tables**. A data table is the location in which data is stored from which the controller can read, or to which the controller can write data. For the PROFINET protocol, the data tables are known as modules. For the EtherNet/IP protocol, data tables are known as assemblies. For the Modbus protocol, the accepted term is data tables.
- **EtherNet/IP**. A communications protocol standard used in industrial applications which uses standard Ethernet technologies.
- **Zebra automation device**. The local computer running the Aurora Imaging Library application. The controller will communicate with the local computer by reading from and writing to data tables in the local computer's memory.
- **Modbus**. A serial communications protocol standard used in industrial applications.
- **PROFINET**. A communications protocol standard used in industrial applications with Siemens PLCs.
- **Quaternion**. A mathematical notation for representing orientations and rotations of objects in three dimensions.
- **Robot arm**. An electromechanical device that consists of a mechanical arm that can typically move along six axes and the electronics that control its movement.
- **Robot controller**. A computer connected to the robot arm that sends electrical signals to move the robot arm to the correct location.
- **Robot-side API files**. Robot-specific files that need to be included on a robot controller to program the robot-side code necessary to communicate with Aurora Imaging Library.
