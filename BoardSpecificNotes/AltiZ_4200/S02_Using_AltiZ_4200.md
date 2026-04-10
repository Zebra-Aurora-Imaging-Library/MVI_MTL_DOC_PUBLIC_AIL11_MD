---
doctype: BoardSpecificNotes
part: ""
chapter: AltiZ_4200
section: Using_AltiZ_4200
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / altiz_4200 / Using AltiZ 4200"
---

# Using Zebra AltiZ 4200

To use your Zebra AltiZ 4200 with Aurora Imaging Library, you must allocate an Aurora Imaging Library GigE Vision system ([`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md)), using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). This allocation opens general communication with all the GigE Vision-compliant cameras found on your subnet (through one or more Gigabit Ethernet network adapters in your computer). Then, allocate a digitizer for each Zebra AltiZ 4200 that you want to use to grab data and/or access directly, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). In the case where you have only one Zebra AltiZ 4200, use [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEV0`](../../Reference/dig/MdigAlloc.md). If, however, you have multiple grabberless interface cameras, you should use the [`M_GC_CAMERA_ID()`](../../Reference/dig/MdigAlloc.md) macro and specify the camera's name; this will make for more portable code.

Each process (executable) can allocate only one Aurora Imaging Library GigE Vision system; an error will occur if you try to allocate more than one. Multiple processes can allocate an Aurora Imaging Library GigE Vision system.

Since you interact with Zebra AltiZ 4200 as you would another GigE Vision camera, Zebra AltiZ 4200 information in the Aurora Imaging Library Reference can be found in the paragraphs and values marked as being supported by the GigE Vision driver.

To grab data from Zebra AltiZ 4200, use [`MdigGrab`](../../Reference/dig/MdigGrab.md) with an Aurora Imaging Library container. For information, refer to [Grabbing from 3D sensors](../../UserGuide/C42_Grabbing_from_3D_sensors/ChapterInformation.md).

If the grabbed data from your Zebra AltiZ 4200 is in the form of a point cloud and the direction of the Z-axis of your working coordinate system is not suitable for your application, you can invert or flip its direction. For more information, refer to [Aligning or flipping the coordinate system](../../UserGuide/C42_Grabbing_from_3D_sensors/S06_Aligning_or_flipping_the_coordinate_system.md).

To inquire the current value of the required GigE Vision camera feature, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md). If a GigE Vision feature is not already set to the required value, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) to change the value of the feature. Functionality is also supported with the conventional Aurora Imaging Library functions [`MdigControl`](../../Reference/dig/MdigControl.md) and [`MdigInquire`](../../Reference/dig/MdigInquire.md). Note to control/inquire about the auxiliary I/O lines (signals), rotary encoder interface, and timers, you need to use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) and [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md). For information on the available GenICam features, see the Zebra AltiZ 4200 Installation and Technical Reference.

## Using Zebra Altiz 4200 with Capture Works

Aurora Imaging Capture Works' Feature Browser accesses the device description file (XML) of any GigE Vision-compliant camera/3D sensor, including Zebra AltiZ 4200, and provides an interface to change or inquire the device's information. For information on how to change or inquire the features of your Zebra AltiZ 4200 using Aurora Imaging Library, refer to [Using Aurora Imaging Library with GenICam](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).
