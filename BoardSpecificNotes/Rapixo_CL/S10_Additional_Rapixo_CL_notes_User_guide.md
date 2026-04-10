---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo_CL
section: Additional_Rapixo_CL_notes_User_guide
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cl-pro / Additional Rapixo CL notes User guide"
---

# Miscellaneous user guide information for Zebra Rapixo CL

This section includes additional user guide information for Zebra Rapixo CL. The information found in this section might be a reiteration of content previously documented.

## Supported standards and development kit

Zebra Rapixo CL supports the following standards:

- Camera Link version 2.1.
- GenICam GenApi version 3.4.2.
- GenICam GenCP Camera Link Module version 1.3.
- GenICam for Camera Link (CLProtocol) version 1.2.
  Note that this requires a third-party CLProtocol communication library (DLL) which the camera vendor supplies.

Zebra Rapixo CL supports Zebra FPGA Development Kit (FDK) version 22.2 and later.

## Additional features

The following are additional features supported for Zebra Rapixo CL.

- Firmware updates without power-cycling.
- 2 taps 10-bit RGB and 6 taps 10-bit mono.
- RGB 10/12 bits Gain and Offset. For more information, use [`MdigControl`](../../Reference/dig/MdigControl.md) with **M_SHADING_CORRECTION**.
- Data latches on all acquisition paths.
- Changing the timer duration during a grab.
- Scatter-gather DMA transfers.

## Aurora Imaging Configurator

The following are two tools integrated in Aurora Imaging Configurator (_AILConfig.exe_) for Zebra Rapixo CL. To access these tools, launch the Aurora Imaging Configurator utility (_AILConfig.exe_). Expand the **Boards** item in the tree structure and then click on the **Rapixo CL** subitem from the tree structure in the presented interface.

- Board Information: This tool is used to display details for each processing FPGA IP present in the current firmware.
- Deserializer Setup tool: This tool is used to calibrate the deserializer when using cables longer than 7 meters or when acquired images are corrupt.

In the same page, you can change the board type from Single-Full to Dual-Base, or vice versa.

## Particularities

When using a JAI camera, the Camera Link Camera Control bits on the Zebra Rapixo CL must be activated for stable communication with the camera (either with JAI or Zebra software). The bits can be activated using Aurora Imaging Intellicam in the **Other** tab of the **DCF** dialog.

*[Image: sc_Radient_eV-CL_JAIcamera.png]*

When editing a DCF for Camera Link using Aurora Imaging Intellicam, Feature Browser will close and reopen. Reopening a Feature Browser window might be subject to a noticeable delay due to camera initialization. To avoid this, uncheck "Automatically compute registers" in the **Preferences Register Compute** menu tab. However, you will then need to manually compute the DCF registers using the **DCF Compute Registers** menu tab.

DCFs with manually edited register values for OPTO_AUX_IN0, OPTO_AUX_IN1, LVDS_AUX_IN2, LVDS_AUX_IN3, TTL_AUX_IO_4, TTL_AUX_IO_5, TTL_AUX_IO_6, OPTO_AUX_IN8, OPTO_AUX_IN9, LVDS_AUX_IN10, LVDS_AUX_IN11, TTL_AUX_IO_12, TTL_AUX_IO_13, TTL_AUX_IO_14 will need to have these values adjusted by adding 0x1000 (for example, 0x64 becomes 0x1064).
