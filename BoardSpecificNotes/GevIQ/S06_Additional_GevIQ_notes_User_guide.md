---
doctype: BoardSpecificNotes
part: ""
chapter: GevIQ
section: Additional_GevIQ_notes_User_guide
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / geviq / Additional GevIQ notes User guide"
---

# Miscellaneous user guide information for Zebra GevIQ

This section includes additional user guide information for Zebra GevIQ. The information found in this section might be a reiteration of content previously documented.

## GigE Vision Stream Channel

The following sections describe GigE Vision Stream Channel support and features for the Zebra GevIQ Smart GigE Vision Adapter:

### Stream channel support

Only the first stream is supported when interfacing with a camera that supports multiple stream channels.

### Link-Aggregation

You can offload Link-Aggregated devices. Link-aggregation is a networking technique that combines multiple network connections to increase the total data transfer capacity and throughput. Link-Aggregation can be enabled by defining a Link-Aggregation Group (LAG) where physical network connections are grouped and appear to the user as a single network connection having higher transfer capacity and throughput.

To define a LAG, open Aurora Imaging Configurator and navigate to **GevIQ** under the **Boards** item and click on **Link Aggregation**. The **GevIQ Link Aggregation** dialog appears.

*[Image: sc_Configurator_LinkAggregation.png]*

In the dialog, specify a group name and select the ports to include. Click **OK**. A reboot of the PC is typically required for the configuration change to take effect. In the example above, Ethernet 10 and Ethernet 11 will be replaced with a new connection that aggregates both ports.

Once defined, the link-aggregation configuration is stored in the FLASH memory of the Zebra GevIQ Smart GigE Vision Adapter and remains active until deleted.

To disable link aggregation, deselect **Enable Link Aggregation** in the above dialog and reboot.

The round-robin distribution algorithm is supported for static Link-Aggregation (as defined in the GigE Vision Specification).

## GenICam GenTL Producer support

The Zebra GevIQ Smart GigE Vision Adapter software stack provides a GenICam GenTL producer. It can be used with third party libraries that implement a GenTL consumer.

## Aurora Imaging Capture Works

The following are additional features that are available to Zebra GevIQ in Aurora Imaging Capture Works:

### Configuration file editor

The following steps provide a basic methodology for using the device configuration file (DCF) editor:

1. In Aurora Imaging Capture Works, allocate the device. This will grant you access to the configuration file editor.
2. Navigate to the **Configuration File** tab, which is beside the feature browser.
3. Click the **Camera and Digitizer Configuration** button.
4. In the popup dialog, configure the configuration file. For example. you can drag and drop the camera and/or digitizer features into the Camera and Digitizer Configuration section to include them in the file.
5. Save your changes.
6. Use your configuration file when allocating a device. This file can be used to configure the device's settings.

### Ethernet adapter configuration tool

Capture Works will highlight network adapters that are not correctly configured for GigE Vision acquisition with a yellow icon. A tooltip will offer the user recommended modifications. The user can easily rectify configuration issues by right-clicking on the network connection and choosing the **Optimize** option from the dropdown menu. Note that administrator privileges are necessary for these changes to be implemented.

In addition, an Ethernet Adapter Configuration tool is accessible from the Tools menu. This tool enables the optimization of multiple Ethernet adapters simultaneously and the ability to modify the IP configuration settings (including the IP address and subnet mask of the network adapters).

Alternatively, the Ethernet Adapter Configuration tool is also accessible from the command-line interface. Type `CaptureWorks.exe --help` on the command line for more details.

Zebra GevIQ Smart GigE Vision Adapter is optimized for GigE Vision acquisition. As such, the Capture Works Ethernet adapter configuration tool applies to network connections found in consumer end products when used with the software-based Aurora Imaging Library GigE Vision system and not the GevIQ network connections.

### Saving components

You can save an individual component (such as an image) from a multi-component grab container. This allows you to isolate and save a single component from a collection. To use this feature, follow these steps:

1. In Aurora Imaging Capture Works, select the component to save via the display window.
2. Click the **Save** button in the top-right corner of the display window.

### Renaming devices

Compatible devices can be re-named using the `F2` key or by right-clicking on the device and selecting the **Rename** option.

### Device window display mode

You can change the display mode of the device window. The conventional Vendor-Model display scheme can be replaced with user-assigned names instead. To activate this feature, follow these steps:

1. Access the **Tools** menu and select **Options**.
2. In the Options dialog, select the **View** tab.
3. Change the Device Window Display Format to User-defined name.

## Power conservation

Microsoft Windows power management can turn off Ethernet devices to save power. If you are using Zebra GevIQ Ethernet Adapters, ensure this option is turned off to maintain network connectivity. To do this, follow these steps:

1. Open Windows Device Manager.
2. Expand the Network Adapters section.
3. Right-click on a Zebra GevIQ Ethernet Adapter and select **Properties**.
4. In the Properties window, select the **Power Management** tab.
5. Uncheck the **Allow the computer to turn off this device to save power** option.

> **Note:** These steps must be repeated for all Zebra GevIQ Ethernet Adapters listed under Network Adapters in the Device Manager so that Ethernet adapters remain powered on and connected to the network, even when the system is trying to conserve power.

To achieve maximum performance, the following Windows power options should be configured:

- High Performance power plan should be selected.
- Under advanced power options, ensure **PCI Express Link State Power Management** is turned off.

## Particularities

The following are some particularities when using Zebra GevIQ:

**M_GRAB** buffers are allocated in paged memory by default.

CPU usage will be higher due to post-processing when:

- The camera PixelFormat is in YUV format, and a color-space conversion is required.
- A camera streams in legacy Chunk Data payload type.

The following digitizer controls are disabled when the payload transmitted by the camera is not an image payload type (as defined in the GigE Vision specification):

- **M_GRAB_SCALE_X**.
- **M_GRAB_SCALE_Y**.
- **M_SOURCE_SIZE_X**.
- **M_SOURCE_SIZE_Y**.
- **M_SOURCE_OFFSET_X**.
- **M_SOURCE_OFFSET_Y**.

If available, **M_GRAB_DIRECTION_X** and **M_GRAB_DIRECTION_Y** digitizer controls are mapped to ReverseX and ReverseY camera features.

Microsoft Kernel DMA protection is not available. See [Kernel DMA Protection | Microsoft Learn](https://learn.microsoft.com/en-us/windows/security/hardware-security/kernel-dma-protection-for-thunderbolt) for more details. If the Zebra GevIQ driver fails to start with code 55, you will need to disable kernel DMA protection in the UEFI/BIOS.

Packet resends are not available.
