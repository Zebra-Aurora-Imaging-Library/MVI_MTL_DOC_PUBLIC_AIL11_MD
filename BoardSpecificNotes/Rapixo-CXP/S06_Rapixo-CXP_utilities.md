---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo-CXP
section: Rapixo-CXP_utilities
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cxp / Rapixo-CXP utilities"
---

# Zebra Rapixo CXP utilities and tools

There are four Zebra Rapixo CXP external applications (utilities) available (Usage Meter, Rapixo CXP Bench, Rapixo CXP EyeScan, and Aurora Imaging Gecho Viewer) and a series of tools integrated in the Aurora Imaging Configurator utility (such as, the PCIe Information tool and the System Monitor tool).

To access these utilities and tools, launch the Aurora Imaging Configurator utility. Then, expand the **Boards** item in the tree structure and then click on the**Rapixo CXP** subitem from the tree structure in the presented interface. The utilities are located in the **Launch external applications** and **Zebra Rapixo CXP System Monitor** pages, respectively.

Zebra Rapixo CXP can also leverage the use of Aurora Imaging Capture Works (external application), which can be accessed using Aurora Imaging Control Center.

## Usage Meter utility

The Usage Meter utility calculates how much of the grab section and the transfer section of each DMA write engine is being used. This rate of utilization is expressed as a percentage. This utility also shows the usage of on-board memory.

## Rapixo CXP Bench utility

The Rapixo CXP Bench utility calculates the real-time transfer speed of the PCIe slot (in Mbytes/sec) from:

- Zebra Rapixo CXP to Host.
- Host to Zebra Rapixo CXP.

## Rapixo CXP EyeScan

The Rapixo CXP EyeScan utility displays an eye diagram of the high speed incoming CoaXPress video signal as it is seen inside the FPGA's transceiver. This tool could help physical error diagnosis (for example, jitter or signal amplitude). Generally, the wider the opening of the eye in the X and Y-axis the better the quality of the signal. Be aware that differences in FPGA transceivers and external PHY might produce channel differences between signals of the same quality.

> **Note:** For Zebra Rapixo Single, Dual, Quad CXP-12, and Pro Quad operating at speeds of CXP-12, CXP-6, or CXP-3, if you don't deactivate the re-timer (which smooths the data), then the signal that you see will always appear as a perfect eye. To see a non-retimed signal for these boards, call the EyeScan utility using the ByPassEq command line parameter.

## Aurora Imaging Gecho Viewer

[Aurora Imaging Gecho Viewer](../../UserGuide/C01_Tools_and_utilities/S06_Aurora_Imaging_Gecho_Viewer.md) is an event logging application that gives you the ability to monitor and troubleshoot acquisition performance.

## Aurora Imaging Capture Works

Aurora Imaging Capture Works is an interactive utility used to configure and test devices that make use of a GenICam-based interface standard, and supports the CoaXPress interface standard (besides other standards). Aurora Imaging Capture Works adds, in particular, support for GenICam GenDC containers and multi-part payload types. It also adds support for 3D displays. Aurora Imaging Capture Works will list all detected CoaXPress devices connected to each allocated Zebra Rapixo CXP board. It can start or stop capturing images, display acquired images, save the last grabbed image, send a software trigger, as well as browse and control the selected device's features. You can view and change acquisition properties, and view acquisition statistics.

Aurora Imaging Capture Works key features:

- Camera-centric user interface with support for camera enumeration and presence.
- Feature browser to display and control camera, digitizer, and system settings.
- DCF creation to save camera and digitizer settings.
- Efficient display of grabbed images with many options and view modes.
- Display of line and arc profiles.
- Display of image histogram.
- Memory viewer to display pixel values.

More information is available from the Aurora Imaging Capture Works manual available online.

## PCIe Information tool

The PCIe Information tool returns information about the board's PCIe connection.

Use the acquisition (device) selector to set the acquisition path about which to inquire ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)). Note that this selector is shared with the System Monitor tool.

This tool presents the following information inside the Aurora Imaging Configurator utility:

- Current number of PCIe lanes.
- Current speed of the PCIe lanes.
- Maximum number of PCIe lanes.
- Maximum speed of the PCIe lanes.

## System Monitor tool

The System Monitor tool displays board properties of the specified digitizer ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)).

Use the acquisition (device) selector to set the acquisition path about which to inquire ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)). Note that this selector is shared with the PCIe Information tool.

This tool presents the following information inside the Aurora Imaging Configurator utility:

- The grab FPGA's current temperature, shown on a sliding scale from 0 to 85 degrees Celsius (for Zebra Rapixo CXP Quad CXP-6), or 0 to 100 (for all other Zebra Rapixo CXP boards), in degrees Celsius.
- The speed of the fan on the board, in RPM (Zebra Rapixo CXP Pro only).
- Whether a power source is detected on the internal auxiliary 12 V power connection.

If your Zebra Rapixo CXP is operating in the normal range of temperatures, a check mark is presented at the bottom of the System Monitor tool. If your Zebra Rapixo CXP is operating above or below the normal range of temperatures, an X is presented.
