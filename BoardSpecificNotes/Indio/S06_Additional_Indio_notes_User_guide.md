---
doctype: BoardSpecificNotes
part: ""
chapter: Indio
section: Additional_Indio_notes_User_guide
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / indio-u53 / Additional Indio notes User guide"
---

# Miscellaneous user guide information for Zebra Indio

This section includes additional user guide information for Zebra Indio. The information found in this section might be a reiteration of content previously documented.

Zebra Indio has sixteen discrete signals (8 inputs and 8 outputs) on the auxiliary I/O interface. All 8 auxiliary input signals support user input or quadrature input and have interrupt-generation capabilities. All 8 auxiliary output signals support user output, timer output, or outputting the state of an I/O command register bit.

Zebra Indio also integrates a network port. The port features an Intel I210 Ethernet controller with support for Power-over-Ethernet (PoE). This port is intended for communicating through Aurora Imaging Library using either the EtherNet/IP, Modbus over TCP/IP, or PROFINET protocols. PROFINET communication is hardware-assisted and does not require the installation of the Intel Network Adaptor Driver. EtherNet/IP and Modbus over TCP/IP communication requires the Intel Network Adaptor Driver. It is available from [downloadcenter.intel.com](https://downloadcenter.intel.com).

Zebra Indio's network port can also be used for interfacing with a GigE Vision camera. This requires an Aurora Imaging Library installation with the Aurora Imaging Library System for GigE Vision selected. The use of the port for GigE Vision also requires installing the Intel Network Adaptor Driver, which is available from [downloadcenter.intel.com](https://downloadcenter.intel.com). Its optimum use for GigE Vision requires additional configuration, which is explained in [Configuring your GigE Vision camera and Gigabit Ethernet network adapter](../GigE_Vision/S04_Configuring_your_GigE_Vision_device.md).

Hardware-assisted PROFINET LED is supported on Zebra Indio. It is off if hardware-assisted PROFINET is disabled in Aurora Imaging Configurator. The LED is red if hardware-assisted PROFINET is enabled, but no connection is established with PLC. If the connection is established, it is off.

To use Zebra Indio and have access to all its auxiliary I/O signals, you must allocate it as an Aurora Imaging Library Indio system (**M_SYSTEM_INDIO**) using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). This allocation opens communication with Zebra Indio and allows Aurora Imaging Library to use its resources. More than one process (executable) on Zebra Indio can allocate an Aurora Imaging Library Indio system.

The Aurora Imaging Library industrial communication module allows only one PROFINET device to be allocated. Therefore, if you have more than one Zebra Indio board installed in a system, you can only enable one for hardware-assisted PROFINET communication through the Communication section of the Aurora Imaging Configurator utility.

When selecting Zebra Indio as the default system in Aurora Imaging Configurator, the Aurora Imaging Library examples that use [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) will produce errors since the Zebra Indio system does not directly support video capture. If you need to grab using the Gigabit Ethernet port on Zebra Indio, you must select GigE Vision as the default system.

Note that **M_IO_FORMAT** in [`MsysInquire`](../../Reference/sys/MsysInquire.md) is obsolete.
