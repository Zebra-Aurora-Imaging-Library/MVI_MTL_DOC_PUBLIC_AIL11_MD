---
doctype: BoardSpecificNotes
part: ""
chapter: Clarity_UHD
section: Automatic_detection_Clarity_UHD
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / clarity-UHD-u36 / Automatic detection Clarity UHD"
---

# Automatic camera detection with Zebra Clarity UHD

With Zebra Clarity UHD, Aurora Imaging Library provides autodetect DCFs and specific resolution DCFs. To use an autodetect DCF when allocating a digitizer, a camera must be connected to the specified acquisition path ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)). To use a specific resolution DCF, a camera does not necessarily need to be connected before the allocation.

You can hook a function to the event that occurs when a camera is connected or disconnected from the board using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with the [`M_CAMERA_PRESENT`](../../Reference/dig/MdigInquire.md)hook type. In the hook-handler function, you can inquire whether the event occurred upon the connection or disconnection of the camera using [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) with [`M_CAMERA_PRESENT`](../../Reference/sys/MsysGetHookInfo.md); you can establish to which acquisition path the camera was connected or disconnected from using [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) with[`M_NUMBER`](../../Reference/sys/MsysGetHookInfo.md). Based on this information, you can allocate or free the digitizer for the camera in the hook-handler function.

The following example demonstrates how to use automatic camera detection with Zebra Clarity UHD: _MultiCameraDisplay.cpp_. To run the example, you can use **Example Launcher**.
