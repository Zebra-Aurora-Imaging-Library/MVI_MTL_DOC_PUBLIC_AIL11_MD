---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Multiple_systems
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Multiple systems"
---

# Multiple systems

There is no limit to the number of systems an application can access; it is limited only by the physical devices present in your computer or accessible from your computer. To use multiple Zebra imaging boards, you have to allocate an Aurora Imaging Library system for each board. If necessary, you can use [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_INSTALLED_SYSTEM_DESCRIPTOR + n`](../../Reference/app/MappInquire.md) to select the system to use at runtime.

To perform a processing operation, your source and destination buffers can be on different systems; Aurora Imaging Library will transparently copy buffers to the most efficient of these systems, if necessary.

To exchange data between systems, you can physically copy the data from one system to another. The copy is always performed by the most suitable system. If both systems are of the same type, the copy is always performed by the destination system.

Instead of performing a physical copy using [`MbufCopy`](../../Reference/buf/MbufCopy.md), you can allocate a buffer on one system and use [`MbufCreate...`](../../Reference/buf/MbufCreate2d.md) to access this buffer from another system. [`MbufCreate...`](../../Reference/buf/MbufCreate2d.md) creates a buffer that maps to allocated mappable memory, for example, on the Host or any Aurora Imaging Library system; no memory is actually allocated to this newly created buffer. This technique can be used, for example, to update a buffer (or part of it) with data grabbed from different systems. Note that after writing to the created buffer, you should notify the real buffer that its contents have been changed, by calling [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MODIFIED`](../../Reference/buf/MbufControl.md). For more information about creating data buffers, see [Data buffers](../C23_Data_buffers/ChapterInformation.md).

To grab, the digitizer and the destination buffer must be allocated on the same Aurora Imaging Library system. Similarly, to display a buffer, the display and the buffer must be allocated on the same Aurora Imaging Library system.

Windowed displays from different systems on the same computer will automatically display together on the same screen in different windows.
