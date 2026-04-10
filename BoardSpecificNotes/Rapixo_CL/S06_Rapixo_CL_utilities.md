---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo_CL
section: Rapixo_CL_utilities
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cl-pro / Rapixo CL utilities"
---

# Zebra Rapixo Series CL tools

There are five Zebra Rapixo CL external applications (utilities) available (Usage Meter, Rapixo CL Bench, Board Information, Deserializer Setup tool, and Aurora Imaging Gecho Viewer) and a series of tools integrated in the Aurora Imaging Configurator utility (such as, the PCIe Information tool and the System Monitor tool).

To access these utilities and tools, launch the Aurora Imaging Configurator utility. Then, expand the **Boards** item in the tree structure, click on the**Rapixo series** subitem in the tree structure, and then select**CL**. The utilities are located in the **Launch External Applications** and the tools are located in the **PCIe Information** and **System Monitor** sub-entries, respectively.

## Usage Meter utility

The Usage Meter utility calculates how much is being used of the grab section and the transfer section of each DMA engine. This rate of utilization is expressed as a percentage. This utility also shows the usage of on-board memory.

## Rapixo CL Bench utility

The Rapixo CL Bench utility calculates the real-time transfer speed of the PCIe slot (in Mbytes/sec) from:

- Zebra Rapixo CL to Host.
- Host to Zebra Rapixo CL.

## Board Information

The Board Information tool displays a feature browser window with information of each board. This utility shows information, such as:

- Hardware identification: (such as, board revision, firmware, location, and serial number).
- Hardware resources: (such as, DCF support, digitizer number, free memory, PCIe lanes, and PCIe speed).

## Deserializer setup tool

The Deserializer setup tool finds the initial values needed for the deserializer alignment process when grabbing data. The values are determined for each Camera Link connection that is in-use. The resulting values are saved in the DCF, and help compensate for signal degradation.

> **Note:** Note that it is recommended to use this tool only when using cables longer than 7 meters and when acquired images are corrupted. For more information on using the Deserializer setup tool, refer to the Zebra Rapixo CL installation and hardware reference manual.

## Aurora Imaging Gecho Viewer

[Aurora Imaging Gecho Viewer](../../UserGuide/C01_Tools_and_utilities/S06_Aurora_Imaging_Gecho_Viewer.md) is an event logging application that gives you the ability to monitor and troubleshoot acquisition performance.

## PCIe Information tool

The PCIe Information tool returns information about the board's PCIe connector.

Use the acquisition (device) selector to set the acquisition path about which to inquire ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)).

> **Note:** Note that this selector is shared with the System Monitor tool.

This tool presents the following information inside the Aurora Imaging Configurator utility:

- Current number of PCIe lanes.
- Current speed of the PCIe lanes.
- Maximum number of PCIe lanes.
- Maximum speed of the PCIe lanes.

## System Monitor tool

The System Monitor tool calculates the temperatures of the specified device ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)), and presents the following information inside the Aurora Imaging Configurator utility:

- An acquisition (device) selector.
- The grab FPGA current temperature, in degrees Celsius.
- The grab FPGA current temperature, as shown on a sliding scale from 0 to 85 degrees Celsius.
- The fan speed, in RPM.
- The sensor for the auxiliary power connection.
- Either a check mark (if your Zebra Rapixo CL is operating in the normal range of temperatures) or an X (if your Zebra Rapixo CL is operating above the normal range of temperatures).
