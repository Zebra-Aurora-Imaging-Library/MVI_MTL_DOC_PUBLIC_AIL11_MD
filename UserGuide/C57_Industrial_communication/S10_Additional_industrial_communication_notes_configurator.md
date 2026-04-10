---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Additional_industrial_communication_notes_configurator
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Additional industrial communication notes configurator"
---

# Miscellaneous user guide information for Industrial Communication: Reference support and creating and enabling industrial protocol instances

This section includes additional user guide information for the Industrial Communication module. The information found in this section might be a reiteration of content previously documented.

## Reference support

Note that the [`McomRead`](../../Reference/com/McomRead.md) and [`McomWrite`](../../Reference/com/McomWrite.md) does not support the use of reference to a std::vector&lt;AIL_UINT8> for the UserArrayPtr parameter.

## Creating and enabling industrial protocol instances

The Industrial Communication module supports multiple instances of the same protocol on different Ethernet interfaces. These protocol instances are created from the **Communication** section in Aurora Imaging Configurator. The Communication section has a dedicated page for each protocol. To create a new instance, select the appropriate protocol, click the Add button, and fill the dialog box with the values required for the chosen protocol.

Each protocol instance must have a unique name. Each Ethernet interface can only have one instance of a particular protocol type. The following are some particularities for the supported protocols:

### PROFINET

The **Aurora Imaging Configurator Communication PROFINET** page will only be enabled if the system offers the corresponding hardware engine. The PROFINET input and output module sizes must match the ones in the PLC project that will be linked to the PROFINET instance.

The hardware engine associated with the PROFINET instance will display its device name, MAC address, and IP address when it is available.

The PROFINET device name field is displayed when the Siemens TIA Portal application sets the device name.

Note that the device name displayed in Aurora Imaging Configurator can be slightly different when non alphanumeric characters are used in the device name definition in Siemens TIA Portal. The PROFINET IP address will display "Set by PLC" whenever the IP address is supplied directly by the PLC controlling the PROFINET instance; otherwise, the IP address will be reported whenever it is assigned statically by Siemens TIA Portal.

### EtherNet/IP

The EtherNet/IP producer, consumer, and configuration sizes must match the ones in the PLC project that will be linked to the EtherNet/IP instance.

### Modbus

The Modbus type, mode, and ID are assigned when adding a worker instance.

The Modbus type, mode, and associated worker list are assigned when adding a controller instance.

### CC-Link

The CC-Link number of stations is assigned when adding an instance.
