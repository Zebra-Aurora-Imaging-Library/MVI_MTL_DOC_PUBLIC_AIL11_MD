---
doctype: BoardSpecificNotes
part: ""
chapter: Radient_eV-CL
section: Radient_eV-CL_utilities
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / radient_ev-cl / Radient eV-CL utilities"
---

# Zebra Radient eV-CL utilities and tools

There are two Zebra Radient eV-CL external applications (utilities) available (Usage Meter and Radient eV-CL Bench) and a series of tools integrated in the Aurora Imaging Configurator utility (such as, the PCIe Information tool and the System Monitor tool).

To access these utilities and tools, launch the Aurora Imaging Configurator utility. Then, expand the **Boards** item in the tree structure, click on the**Radient eV-series** subitem in the tree structure, and then select your board (**eV-CL**). The utilities are located in the **Launch external applications** and **Zebra Radient eV System Monitor** pages, respectively. There is a version of each of these utilities and tools for each board-type, unless otherwise specified in the text below.

## Usage Meter utility

The Usage Meter utility calculates how much of the grab section and the transfer section of each DMA engine is being used. This rate of utilization is expressed as a percentage. This utility also shows the usage of on-board memory.

## Zebra Radient eV-CL Bench utility

The Zebra Radient eV-CL Bench utility calculates the real-time transfer speed of the PCIe slot (in Mbytes/sec) from:

- Zebra Radient eV-CL to Host.
- Host to Zebra Radient eV-CL.

## PCIe Information tool

The PCIe Information tool returns information about the board's PCIe connector.

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

- The grab FPGA's current temperature, shown on a sliding scale from 0 to 85 degrees Celsius.
- The speed of the fan on the board, in RPM.
- Whether a power source is detected on the internal auxiliary 12 V power connection, if available on the Zebra Radient eV-CL board.

If your Zebra Radient eV-CL is operating in the normal range of temperatures, a check mark is presented at the bottom of the System Monitor tool. If your Zebra Radient eV-CL is operating above or below the normal range of temperatures, an X is presented.

## Zebra Radient eV-CL Deserializer setup tool

The Zebra Radient eV-CL Deserializer setup tool finds the initial values needed for the deserializer alignment process when grabbing data. The values are determined for each Camera Link connection that is in-use. The resulting values are saved in the DCF, and help compensate for signal degradation. Note that we recommend using this tool only when using cables longer than 7 m and when acquired images are corrupted. For more information on using the Zebra Radient eV-CL Deserializer setup tool, refer to the Zebra Radient eV-CL installation and hardware reference manual.
