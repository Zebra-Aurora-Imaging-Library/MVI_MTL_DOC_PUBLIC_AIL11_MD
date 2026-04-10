---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: Additional_GigE_Vision_notes_user_guide
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / Additional GigE Vision notes user guide"
---

# Miscellaneous user guide information for Zebra GigE Vision driver

This section includes additional user guide information for Zebra GigE Vision driver. The information found in this section might be a reiteration of content previously documented.

## Working with a 3D device

The following includes useful information when working with a 3D device.

### Grabbing from a 3D device

Data from a 3D device is grabbed using functions from the Digitizer module such as [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), [`MdigGrab`](../../Reference/dig/MdigGrab.md), and [`MdigProcess`](../../Reference/dig/MdigProcess.md). In most respects, grabbing data from a 3D device is similar to grabbing images from a 2D camera. However, to work with 3D data in Aurora Imaging Library, you need to use a container. The reason is that 3D devices can transmit multiple components (range, confidence, etc.) during a grab.

If your 3D device transmits data using the GenICam GenDC or the GigE Vision multi-part payload types, you can grab directly into a container that was previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md).

### Grabbing into an Aurora Imaging Library container

When grabbing into a container for the first time, Aurora Imaging Library will free all existing components and allocate the components required to store the current frame of data. The library will typically reuse these automatically allocated components during subsequent grabs, unless you add or remove components from the container. If the latter is the case, all components will be freed and new components will again be allocated for the grabbed data.

Aurora Imaging Library will not grab into components of the container that you have allocated manually using [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md), [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md), or [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md). Rather, the library will create components, as required.

### Grabbing into an Aurora Imaging Library buffer

If you grab into a traditional buffer from a device that outputs data in a format suitable for a container, the first intensity component (i.e., a standard image) in the frame of data will be grabbed. If there is no intensity component, the first component that is not metadata will be grabbed.

### Grabbing using older utilities and certain examples

Limitations are to be expected when using older utilities (such as Aurora Imaging Intellicam), and certain examples (such as MdigGrab, MdigGrabSequence and MdigProcess), to grab from particular 3D devices. These utilities and examples do not support grabbing multiple components. That is, they are not able to grab all of the components at once. If present, the first intensity component is always used. Otherwise, the first component that can be stored into an image buffer is used.

## Capture Works

The Capture Works interactive utility has been updated with the following.

### Ethernet adapter configuration tool

Capture Works will highlight network adapters that are not correctly configured for GigE Vision acquisition with a yellow icon. A tooltip will offer the user recommended modifications. The user can easily rectify configuration issues by right-clicking on the network connection and choosing the "optimize" option from the dropdown menu. Please note that administrator privileges are necessary for these changes to be implemented.

In addition, an Ethernet Adapter Configuration tool is accessible from the Tools menu. This tool enables the optimization of multiple Ethernet adapters simultaneously and the ability to modify the IP configuration settings (including the IP address and subnet mask of the network adapters).

Alternatively, the Ethernet Adapter Configuration tool is also accessible from the command-line interface. Type CaptureWorks.exe --help on the command line for more details.

### GigE Vision device IP configuration tool

GigE Vision devices require an IP address for operation. The acquisition of an IP address by a device can be achieved through three methods: Static IP, DHCP, and Link-Local Address. A device can become inaccessible if the IP address in use is incompatible with its network segment. In many cases, Aurora Imaging Library identifies this issue and assigns a valid IP address, thereby resolving the configuration problem transparently.

However, there are situations where Aurora Imaging Library is unable to rectify the configuration problem, particularly when the network segment in question has a DHCP service. The IP address allocated by the library could eventually lead to problems as the DHCP server might assign the same IP address to a different device, resulting in an IP address conflict. A device that is misconfigured on such a network segment will become inaccessible.

In previous versions of Aurora Imaging Library, unreachable devices were not reported. Consequently, it was the user's responsibility to troubleshoot this type of problem. However, the GigE Vision driver now identifies such devices as "unreachable", and Capture Works will denote these devices with a red icon.

To resolve these issues, the GigE Vision device IP configuration tool included in Capture Works can be utilized. Simply right-click on the device and select the "IP resolve" option to automatically rectify the problem. The device will be temporarily assigned a valid IP address, making it addressable. At this point, Capture Works will modify the device's IP configuration to a mode that is compatible with its network segment and send a command requesting the device to initiate its IP configuration cycle using the new configuration mode. If a DHCP server is available, the device will request an IP address using the DHCP method, resulting in a properly configured device.

Alternatively, and for more control over the method used to resolve the conflict, users can access the GigE Vision device IP configuration option from the Tools menu.

## Known issues

There are known acquisition issues when Microsoft Teams is running resulting in stream data packet resends and corrupted frames. Shutting down Microsoft Teams resolves the issue; note that simply closing its window is not sufficient to resolve the problem. Microsoft Teams can be shutdown through the Windows system tray.

## Standards compliance

The following is a list of supported standards:

- Support for GigE Vision 2.2, including:
  - Support for GigE Vision control, stream, and message channels.
  - Support for multicasting on stream and message channels.
  - Support for GenICam chunk mode.
  - Support for peripheral devices (e.g. non-streaming devices).
- Support for GenICam 3.4.
- Support for Standard Feature Naming Convention (SFNC) version 2.7.
- Support for GenDC 1.1.
- Support for GigE Vision action and scheduled action command with precision time protocol (PTP IEEE 1588).
- Support for Microsoft Network Driver Interface Specification 6.20.
- Seamless support for link aggregated devices. Note that Intel has dropped support in their Windows 11 drivers but link aggregation is supported on the GevIQ frame grabber regardless of the OS.
