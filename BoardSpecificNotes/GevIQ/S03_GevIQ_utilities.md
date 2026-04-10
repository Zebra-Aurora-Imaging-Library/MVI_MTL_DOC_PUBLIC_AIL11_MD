---
doctype: BoardSpecificNotes
part: ""
chapter: GevIQ
section: GevIQ_utilities
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / geviq / GevIQ utilities"
---

# Zebra GevIQ utilities and tools

The Zebra GevIQ can leverage the use of Aurora Imaging Capture Works (external application) and a series of tools integrated in the Aurora Imaging Configurator utility (such as, the PCIe Information tool and the System Monitor tool) to help setup, diagnose, and debug your system.

To access these utilities and tools, launch the Aurora Imaging Configurator utility. Expand the **Boards** item in the tree structure and then click on the**GevIQ** subitem from the tree structure in the presented interface. The utilities and tools are located in the **Launch external applications** and **Zebra GevIQ System Monitor** pages, respectively.

## Zebra GevIQ Bench utility

The Zebra GevIQ Bench utility calculates the real-time transfer speed of the PCIe slot (in Gbytes/sec) for Zebra GevIQ-to-Host:

- **Non-paged DMA**: A copy from a Zebra GevIQ on-board buffer to a buffer located in Host non-paged memory.
- **Scatter gather DMA**: A copy from a Zebra GevIQ on-board buffer to a buffer located in Host paged memory using scatter gather DMA.

Another key feature of the Zebra GevIQ Bench utility is the interrupt latency tool, which measures the system interrupt latency (the time required for the system to respond to an interrupt). While the Zebra GevIQ is under load (for example, grabbing images using the Aurora Imaging Capture Works utility), the tool will output the current, minimum, and maximum ISR (Interrupt Service Routine) latency in microseconds.

## Aurora Imaging Capture Works

Aurora Imaging Capture Works is an interactive utility to configure and test devices that make use of a GenICam-based interface standard, and supports the GigE Vision interface standard (besides other standards). Aurora Imaging Capture Works adds, in particular, support for GenICam GenDC containers and multi-part payload types. Aurora Imaging Capture Works also adds support for 3D displays. Aurora Imaging Capture Works will list all detected GigE Vision-compliant devices connected to each allocated board. It can start or stop capturing images, display acquired images, save the last grabbed image, send a software trigger, as well as browse and control the selected device's features. You can view and change acquisition properties, and view acquisition statistics.

Aurora Imaging Capture Works key features:

- Camera-centric user interface with support for camera enumeration and presence.
- Feature browser to display and control camera, digitizer, and system settings.
- DCF creation to save camera and digitizer settings.
- Efficient display of grabbed images with many options and view modes.
- Display of line and arc profiles.
- Display of image histogram.
- Memory viewer to display pixel values.

More information is available from the Aurora Imaging Capture Works manual available online.

## Aurora Imaging Gecho Viewer

[Aurora Imaging Gecho Viewer](../../UserGuide/C01_Tools_and_utilities/S06_Aurora_Imaging_Gecho_Viewer.md) is an event logging application that gives you the ability to monitor and troubleshoot acquisition performance.

## Zebra GigE Vision discovery service

The Zebra GigE Vision discovery service allows your Aurora Imaging Library application and the Aurora Imaging Capture Works utility to automatically detect when GigE Vision-compliant cameras are added to or removed from your network. For more information, refer to [Zebra GigE Vision discovery service](../GigE_Vision/S03_GigE_Vision_utilities.md).

## Zebra GevIQ PCIe Information tool

The Zebra GevIQ PCIe Information tool returns information about the board's PCIe connection.

Use the acquisition (device) selector to set the stream channel about which to inquire ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)). Note that this selector is shared with the Zebra GevIQ System Monitor tool.

This tool presents the following information inside the Aurora Imaging Configurator utility:

- Current number of PCIe lanes.
- Current speed of the PCIe lanes.
- Maximum number of PCIe lanes.
- Maximum speed of the PCIe lanes.

## Zebra GevIQ System Monitor tool

The Zebra GevIQ System Monitor tool displays board properties of the specified digitizer ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)).

Use the acquisition (device) selector to set the stream channel about which to inquire ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)). Note that this selector is shared with the Zebra GevIQ PCIe Information tool.

This tool presents the following information inside the Aurora Imaging Configurator utility:

- The current temperature of the camera connected to the specified digitizer ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)).
- The maximum recorded temperature of the camera connected to the specified digitizer ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)).

## Ethernet switches and Configuring your IP

Zebra GevIQ supports digital video sources compliant with the GigE Vision 2.2 standard. Depending on how your video sources are configured/connected to your Zebra GevIQ (for example, using a switch), a single port can offload multiple stream channels. For information regarding working with multiple GigE Vision-compliant devices using a switch, refer to[Using a Gbit Ethernet switch with GigE Vision-compliant cameras](../GigE_Vision/S09_Using_an_Ethernet_switch_with_GigE_vision_complaint_cameras.md).

Zebra GevIQ network adapter settings are already optimized for best performance and do not need to be adjusted (for example, in the event of corrupted frames). However, for generalized information regarding acquisition reliability and common causes (such as firewall settings, insufficient inter-packet delay, packet resend capabilities, and more), refer to [Troubleshooting acquisition reliability issues](../GigE_Vision/S10_Troubleshooting_acquisition_reliability_issues.md).

For information regarding IP configuration of your Zebra GevIQ and GigE Vision-complaint camera, refer to [Configuring IP addresses](../GigE_Vision/S05_GigE_Vision_network_adapters.md).
