---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Additional_industrial_communication_notes_profinet
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Additional industrial communication notes profinet"
---

# Miscellaneous user guide information for Industrial Communication: PROFINET

This section includes additional user guide information for the PROFINET Industrial Communication protocol. The information found in this section might be a reiteration of content previously documented.

Enabling an instance of the PROFINET service activates the associated PROFINET engine.

The PROFINET engine supports a minimum I/O cycle time of 1 millisecond. The network connection associated with the PROFINET engine communication can be shared with other Ethernet traffic.

When activated, the PROFINET interface is able to act as a second Ethernet communication device with its own MAC and IP settings; these settings are distinct from those of the main Ethernet communication interface available to the operating system. Note that the operating system does not have access to the second Ethernet communication device. The two Ethernet communication channels can be represented as follows:

*[Image: comm_internal_connections.png]*

## Assigning a static IP address

To configure the PLC to recognize your runtime platform as a worker device when using the PROFINET Industrial Communication protocol, you must use the Siemens Totally Integrated Automation Portal (TIA) Portal software to assign the runtime platform's PROFINET interface a static IP address that is compatible with the automation network.

*[Image: comm_siemensTIA.png]*

The general-purpose NIC interface must also have a static IP address (set up using normal operating system settings); otherwise, the Auto-IP or DHCP modes might lead to the connection failing when the general-purpose NIC interface attempts to discover a DHCP server or renegotiate its IP address. To maximize the PROFINET engine's execution, you must install the Intel network driver for the general-purpose NIC interface.

Both the PROFINET interface and the general-purpose NIC interface must be on the same subnet to run the Siemens TIA Portal software. The IP addresses of both interfaces must be unique on your network; otherwise, they will interfere with each other. To verify that there is no conflict, access the ** Communication** page in Aurora Imaging Configurator and click on the ** PROFINET** page. If there is a conflict, change either the general purpose NIC interface's IP address in the operating system settings or change the PROFINET interface's IP address using the Siemens TIA portal software.

> **Note:** You should not enable the PROFINET service on a computer that has both the Siemens TIA Portal software and Aurora Design Assistant installed; this can cause network adapter addressing conflicts.

## Configuring the PLC

After assigning the IP address, load the provided _GSD_ file located in the installed Profinet folder (for example, _C:\Program Files\Aurora Imaging Library\11\Config\Profinet_) for your runtime platform. Then, add the runtime platform as a device in your PROFINET network, specify the address of the fields, and if required, change the size of the DataFromPLC and DataToPLC data tables (modules); acceptable sizes range between 16 and 256 bytes.

## Turning off the LLDP protocol for hardware-assisted PROFINET ports

The PROFINET certification process requires disabling Microsoft Link Layer Discovery Protocol (LLDP) on the device being tested to ensure all validation is successful. Subsequently, you should disable the LLDP protocol service on your system containing the Zebra hardware that incorporates the PROFINET engine. For example, if you are using a Zebra 4Sight system, the Microsoft LLDP can be disabled as follows:

1. Open your system's Control Panel.
2. Select Network and Internet settings.
3. Select Network and Sharing Center.
4. Select Change Adapter Settings from the left vertical pane.
5. Right-click the Ethernet Adapter that provides the hardware assistant PROFINET port and select Properties.
6. In the new Ethernet Properties dialog, select the Microsoft LLDP Protocol Driver.
7. Uncheck the Microsoft LLDP Protocol Driver. You need to disable the protocol only on ports using hardware-assisted PROFINET.
8. If the hardware-assisted PROFINET in Aurora Imaging Configurator is not associated with any PROFINET protocol instance, you do not need to disable the LLDP protocol of the hardware-assisted PROFINET port.
   *[Image: comm_LLDP_disable.png]*
