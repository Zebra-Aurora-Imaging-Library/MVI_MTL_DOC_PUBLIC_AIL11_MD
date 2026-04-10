---
doctype: UserGuide
part: "3D related information"
chapter: Grabbing_from_3D_sensors
section: Grabbing_from_3D_sensors_overview
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Grabbing_from_3D_sensors / Grabbing from 3D sensors overview"
---

# Grabbing from 3D sensors overview

You can grab 3D data from a 3D sensor using functions from the [`Mdig...`](../../Reference/dig/MdigGrab.md)module. In most respects, grabbing 3D data from a 3D sensor is similar to grabbing images from a standard camera (as described in [Grabbing with your digitizer](../C27_Grabbing_with_your_digitizer/ChapterInformation.md)). However, to work with 3D data in Aurora Imaging Library, you need a container.

Typically, if your 3D sensor transmits 3D data in a format defined by an industry standard (such as GigE Vision or GenICam), you can grab directly into a container (previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md)). For more information, see[Working with compliant cameras](S04_Working_with_compliant_cameras.md).

For 3D sensors that do not transmit data suitable for grabbing into a container, you must grab that data into a buffer (or grab the data using a third-party SDK and create a buffer on its memory), and manually prepare it for use with Aurora Imaging Library by putting the data in the components of a container. For more information, see[Working with non-compliant cameras](S05_Working_with_noncompliant_cameras.md).

In most cases, you will need to convert grabbed 3D data using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) before using it with other Aurora Imaging Library functions. For more information, see [Preparing a container for display or processing](../C41_3D_Containers/S04_Preparing_a_container_for_display_or_processing.md).
