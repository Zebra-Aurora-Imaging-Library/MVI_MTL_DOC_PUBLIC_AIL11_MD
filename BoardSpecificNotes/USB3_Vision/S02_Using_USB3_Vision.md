---
doctype: BoardSpecificNotes
part: ""
chapter: USB3_Vision
section: Using_USB3_Vision
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / usb3vision / Using USB3 Vision"
---

# Using the Zebra USB3 Vision driver with Aurora Imaging Library

To use the Zebra USB3 Vision driver with Aurora Imaging Library, you must allocate an Aurora Imaging Library USB3 Vision system ([`M_SYSTEM_USB3_VISION`](../../Reference/sys/MsysAlloc.md)), using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). This allocation opens general communication with all the USB3 Vision-compliant cameras (or devices) connected to your computer. Then, allocate a digitizer for each camera (USB3.0 camera) that you want to use to grab images and/or access directly, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md).

Each process (executable) can allocate only one Aurora Imaging Library USB3 Vision system; an error will occur if you try to allocate more than one. Multiple processes can allocate an Aurora Imaging Library USB3 Vision system.

To inquire the current value of the required USB3 Vision camera feature, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md). If a USB3 Vision feature is not already set to the required value, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) to change the value of the feature. Functionality is also supported with the conventional Aurora Imaging Library functions [`MdigInquire`](../../Reference/dig/MdigInquire.md) and [`MdigControl`](../../Reference/dig/MdigControl.md). For more information on the GenICam-standard features available, refer to the GenICam standard feature naming convention (SFNC), found online at: [emva.org/standards-technology/genicam/](https://www.emva.org/standards-technology/genicam/). Additional GenICam-extension features might be available; check your camera's manual and Aurora Imaging Capture Works' Feature Browser for details.

You can also use Aurora Imaging Capture Works' Feature Browser to access the USB3 Vision-compliant camera's device description file (XML), providing an interface to view and change the camera's configuration. For more information, refer to [Using Aurora Imaging Library with GenICam](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

Zebra USB3 Vision information in the Aurora Imaging Library Reference can be found in the paragraphs and values marked as being supported by the USB Vision driver.

## Non-paged memory usage

With each call to [`MdigAlloc`](../../Reference/dig/MdigAlloc.md), a certain amount of memory is reserved so that Aurora Imaging Library can create internal buffers. To calculate the exact amount of memory reserved, inquire the amount of non-paged memory used before and again after you call [`MdigAlloc`](../../Reference/dig/MdigAlloc.md); to perform the inquire, use [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_NON_PAGED_MEMORY_USED`](../../Reference/app/MappInquire.md). Aurora Imaging Library uses this reserved non-paged memory when grabbing and when decoding image data received in "chunk mode".

If your application requires more non-paged memory than is currently available, increase the amount of non-paged memory accessible to Aurora Imaging Library using the Aurora Imaging Configurator utility.
