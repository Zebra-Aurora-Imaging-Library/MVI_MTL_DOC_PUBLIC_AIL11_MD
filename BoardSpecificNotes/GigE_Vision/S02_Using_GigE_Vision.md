---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: Using_GigE_Vision
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / Using GigE Vision"
---

# Using GigE Vision with Aurora Imaging Library

To use your Zebra GigE Vision driver with Aurora Imaging Library, you must allocate an Aurora Imaging Library GigE Vision system ([`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md)), using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). This allocation opens general communication with (allows you to discover) all GigE Vision-compliant cameras (or other devices) found on your network(s) (through all Gigabit Ethernet, or faster, network adapters in your computer). Note that GigE Vision cameras connected to a Zebra GevIQ frame grabber are not accessible (discoverable) by the GigE Vision system and vice versa.

You must then allocate a digitizer for each camera that you want to use to grab images and/or access directly, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). In the case where you have only one GigE Vision-compliant interface camera, use [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEV0`](../../Reference/dig/MdigAlloc.md). If, however, you have multiple GigE Vision-compliant interface cameras, you should use the [`M_GC_CAMERA_ID()`](../../Reference/dig/MdigAlloc.md) macro and specify the device's name; this will make for more portable code. You can use the Aurora Imaging Capture Works utility to view a list of available GigE Vision-compliant cameras and their user-defined name. For information on this utility, refer to [Zebra GigE Vision tools](S03_GigE_Vision_utilities.md). For information regarding working with multiple GigE Vision-compliant devices, refer to [Working with one or more GigE Vision-compliant cameras](S02_Using_GigE_Vision.md).

Each process (executable) can allocate only one Aurora Imaging Library GigE Vision system; an error will occur if you try to allocate more than one. Multiple processes can allocate an Aurora Imaging Library GigE Vision system.

Typically, multiple processes cannot allocate a digitizer for the same GigE vision-compliant camera simultaneously. However, this is not a restriction when using IP multicasting, as long as only one digitizer is allocated as a multicast controller digitizer (by using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md)with [`M_GC_MULTICAST_CONTROLLER`](../../Reference/dig/MdigAlloc.md)). For information regarding IP multicasting, refer to [Using IP multicast](S07_IP_multicast.md).

All GigE Vision-compliant devices support configuration using the GenICam standard. To learn the list of GenICam-standard features available, refer to GenICam's standard feature naming convention (SFNC), found online at: [emva.org/standards-technology/genicam/](https://www.emva.org/standards-technology/genicam/). Your camera might support additional GenICam features that do not appear in the GenICam SFNC; check your camera's manual and Aurora Imaging Capture Works' Feature Browser for details. Aurora Imaging Capture Works' Feature Browser accesses the GigE Vision-compliant camera's device description file (XML), providing an interface to view and change the camera's information.

In most cases you should control and inquire GigE Vision devices features using the conventional Aurora Imaging Library functions [`MdigInquire`](../../Reference/dig/MdigInquire.md) and [`MdigControl`](../../Reference/dig/MdigControl.md). However, some GigE Vision-compliant device have features that can only be accessed using [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)and [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) functions. For more information about accessing the features of your GigE Vision-compliant device, refer to [Using Aurora Imaging Library with GenICam](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

GigE Vision information in the Aurora Imaging Library Reference can be found in the paragraphs and values marked as being supported by the Zebra GigE Vision driver.

## Working with more than one Gigabit Ethernet network adapter

When your computer has more than one Gigabit Ethernet network adapter installed, you can use each network adapter to access cameras on different networks. Alternatively, multiple network adapters can simply be used to increase the number of cameras to which you have access. Unless otherwise specified, this documentation uses the model of one network adapter connected to a local network on which one or more GigE Vision-compliant cameras exist.

When dealing with more than one Gigabit Ethernet network adapter, the list of your GigE Vision-compliant devices in the Aurora Imaging Capture Works is subdivided by the network adapter to which they are visible. Both the network adapters and their associated GigE Vision-compliant devices are listed in the order of their MAC addresses.

For more detailed information regarding network configurations and GigE Vision-compliant camera setup, refer to the Aurora Imaging Capture Works white paper (located in your _Aurora Imaging Library\Tools_ folder).

## Working with one or more GigE Vision-compliant cameras

When there is more than one GigE Vision-compliant network camera available, the network discovery process (in the Zebra GigE Vision discovery service) ranks the GigE Vision-compliant cameras according to their MAC address. For example, the camera having the lowest MAC address is ranked as device zero; the rank increments by one for each camera having a subsequently greater MAC address. Aurora Imaging Library uses the camera's rank when you allocate a digitizer with a device number (using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEVn`](../../Reference/dig/MdigAlloc.md)); so the rank 0 camera becomes [`M_DEV0`](../../Reference/dig/MdigAlloc.md). When you replace a GigE Vision-compliant camera on your network, its rank might not be the same as the camera you are replacing. This means that the rank of all the GigE Vision-compliant cameras could shift to allow the new camera into the sequence the next time you stop and restart your application. When you try to allocate a digitizer for the new camera (to replace the old camera), if the rank is different, the allocation might fail or worse, it might succeed, but the images captured by the camera actually allocated will not be of what you expect.

To avoid this problem, assign the camera a user-defined name, using Aurora Imaging Capture Works (on the **Device information** tab), and then use this name when you allocate a digitizer for this camera.

To allocate a digitizer for a camera with a user-defined name, use [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_GC_DEVICE_NAME`](../../Reference/dig/MdigAlloc.md) and specify your camera's name. Then, when a camera needs to be replaced, set the user-defined name of the new camera to match the name of the camera that it replaced. Allocating your GigE Vision-compliant camera using the camera's name allows your application to work with the new camera without modifying either your application or your application's configuration. Alternatively, if your camera uses a persistent (static) IP address, you can use that IP address to identify your camera (using [`M_GC_DEVICE_IP_ADDRESS`](../../Reference/dig/MdigAlloc.md)).

A new GigE Vision-compliant camera (added to the network as a new camera or a replacement) is not allocated by default and, although it is present, it is not available to your application until you allocate it using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). If an allocated camera is removed from your network, free it using [`MdigFree`](../../Reference/dig/MdigFree.md).

To watch for new cameras on your network, use [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_CAMERA_PRESENT`](../../Reference/sys/MsysHookFunction.md). Note that verifying the camera's state (through the hooked function) should be done before your application relies on information from the camera.

Use [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md) to determine the number of cameras available. This number is updated when you allocate your system, refresh the list of discovered devices, using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_DISCOVER_DEVICE`](../../Reference/sys/MsysControl.md), and again if called by a function hooked to an [`M_CAMERA_PRESENT`](../../Reference/sys/MsysHookFunction.md) event. Note that this number is not affected by the number of digitizers allocated for the cameras.

## Working without a Zebra board as your Gigabit Ethernet network adapter

To successfully allocate an Aurora Imaging Library GigE Vision system without a Zebra board as your Gigabit Ethernet network adapter, you must already have purchased the Zebra GigE Vision driver license (regardless if you have a license for Aurora Imaging Library development, Aurora Imaging Library runtime, or Aurora Imaging Library Lite). To determine whether you have a Zebra GigE Vision driver license, use [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_LICENSE_MODULES`](../../Reference/app/MappInquire.md); it should return [`M_LICENSE_INTERFACE`](../../Reference/app/MappInquire.md).
