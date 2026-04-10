---
doctype: BoardSpecificNotes
part: ""
chapter: V4L2
section: Using_V4L2
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / v4l2 / Using V4L2"
---

# Using Zebra V4L2 Consumer (library) with Aurora Imaging Library

To use the Zebra Video4Linux2 Consumer with Aurora Imaging Library, you need to allocate an Aurora Imaging Library V4L2 system ([`M_SYSTEM_V4L2`](../../Reference/sys/MsysAlloc.md)), using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). This allocation opens general communication with all devices capable of video capture found on your computer (discovers all character device special files _ /dev/video*_‏‏‎‎, where * is a number between 0 to 63). You must then allocate a digitizer for each hardware device that you want to use to grab images and/or access directly, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEVn`](../../Reference/dig/MdigAlloc.md). For more information, refer to [Device number](../../UserGuide/C27_Grabbing_with_your_digitizer/S04_The_digitizer_number.md).

> **Note:** Note that, you can only allocate one Aurora Imaging Library V4L2 system per Linux process but multiple processes can allocate a V4L2 system. Each device can only be used by one process at a time.

A conversion between Video4Linux2 and GenICam has been implemented which allows V4L2 controls to be accessible through [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) and [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md). You can also use Aurora Imaging Capture Works' Feature Browser to access the Video4Linux2 controls, which provide an interface to view and change the camera's configuration. For more information, refer to [Using Aurora Imaging Library with GenICam](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

Zebra Video4Linux2 information in the _Aurora Imaging Library Reference_ can be found in the paragraphs and values marked as being supported by the Video4Linux2 system.
