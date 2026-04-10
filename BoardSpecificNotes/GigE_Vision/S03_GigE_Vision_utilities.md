---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: GigE_Vision_utilities
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / GigE Vision utilities"
---

# Zebra GigE Vision tools

Besides general utilities (such as Aurora Imaging Intellicam and the Aurora Imaging Configurator), the Zebra GigE Vision driver comes with Aurora Imaging Capture Works (external application) and the Zebra GigE Vision discovery service.

## Aurora Imaging Capture Works

Aurora Imaging Capture Works is an interactive utility to configure and test devices that make use of a GenICam-based interface standard, and supports the GigE Vision transport layer standard (among other standards). Aurora Imaging Capture Works is compatible with GenICam GenDC containers and multi-part payload types. The Aurora Imaging Library GigE Vision system decodes and stores data into container ([`M_CONTAINER`](../../Reference/buf/MbufControlContainer.md)) buffers. Aurora Imaging Capture Works also adds support for 3D displays. Aurora Imaging Capture Works will list all detected GigE Vision-compliant devices connected to the host (or PC). It can start or stop capturing images, display acquired images, save the last grabbed image, send a software trigger, as well as browse and control the selected device's features. You can view and change acquisition properties, and view acquisition statistics.

Aurora Imaging Capture Works key features:

- Camera-centric user interface with support for camera enumeration and presence.
- Feature browser to display and control camera, digitizer, and system settings.
- DCF creation to save camera and digitizer settings.
- Efficient display of grabbed images with many options and view modes.
- Display of line and arc profiles.
- Display of image histogram.
- Memory viewer to display pixel values.
- Ethernet adapter configuration tool to optimize network adapters and modify IP configuration settings.
- GigE Vision device IP configuration tool.

More information is available from the Aurora Imaging Capture Works manual available from the Zebra website. To access this documentation, you can also search online for "Aurora Imaging Capture Works manual".

## Zebra GigE Vision discovery service

The Zebra GigE Vision discovery service allows your Aurora Imaging Library application and the Aurora Imaging Capture Works utility to automatically detect when GigE Vision-compliant cameras are added to or removed from your network. The service is enabled by default when you install the Zebra GigE Vision driver. To disable or re-enable the service, launch the Aurora Imaging Configurator utility. Then, select the **Boards GigE Vision** item from the tree structure in the presented interface, click on the** Use Camera Discovery Service** check box in the presented page, and click on the **Apply** button.

While the service is running, your Aurora Imaging Library application can execute a hook-handler function whenever a GigE Vision-compliant camera is added to or removed from your network (using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_CAMERA_PRESENT`](../../Reference/sys/MsysHookFunction.md)).

While the service is not running, your Aurora Imaging Library application will only detect which GigE Vision-compliant cameras are on your network at the moment you allocate an Aurora Imaging Library GigE Vision system.

You can manually refresh the list of discovered GigE Vision compliant cameras on your network using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_DISCOVER_DEVICE`](../../Reference/sys/MsysControl.md) or by clicking the **Perform Device Discovery** toolbar button in Aurora Imaging Capture Works. You can perform this without running the service.

> **Note:** Note that if you have disabled the discovery service, your firewall might block the discovery of GigE Vision devices.
