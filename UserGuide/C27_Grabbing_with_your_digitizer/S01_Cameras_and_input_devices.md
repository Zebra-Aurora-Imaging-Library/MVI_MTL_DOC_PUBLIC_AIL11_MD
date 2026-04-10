---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Cameras_and_input_devices
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Cameras and input devices"
---

# Cameras and video sources

Aurora Imaging Library supports input from any type of video source connected to the hardware associated with an [acquisition-type Aurora Imaging Library system](../C02_Building_an_application/S06_Mandatory_allocations_allocating_an_application_and_its_systems.md) (such as an Aurora Imaging Library Rapixo CXP system or Aurora Imaging Library GigE Vision system). Each acquisition-type system has one or more available acquisition paths. Aurora Imaging Library refers to the acquisition paths used to grab from one video source as a digitizer.

> **Note:** Note, since most video sources are cameras, they will hereafter be referred to as such.

For a digitizer to be recognized by Aurora Imaging Library, it must be allocated on the target system using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) (or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md)). The allocation sets up the digitizer to match your camera's data format. Once you have finished using a digitizer, you should free it using [`MdigFree`](../../Reference/dig/MdigFree.md).

To make a more portable application, you can allocate and use the default system and digitizer set up when installing Aurora Imaging Library or using the Aurora Imaging Configurator utility. To allocate the default system and digitizer, use [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md). Alternatively, you can use [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_DEFAULT`](../../Reference/sys/MsysAlloc.md) to allocate the default system. Use the returned system identifier with [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) and set the device number and data format both to [`M_DEFAULT`](../../Reference/dig/MdigAlloc.md) to allocate the default digitizer.

Use [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigProcess`](../../Reference/dig/MdigProcess.md), or [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md) to grab data from a camera through a digitizer. When grabbing from a 2D camera, pass an image buffer with an [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute as the location in which to store grabbed images. The image buffer should have as many bands as the grabbed data has color components. If you need to grab and process concurrently, you can use [`MdigProcess`](../../Reference/dig/MdigProcess.md). Both the image buffer and digitizer must be allocated on the same system.

> **Note:** If you have a 3D sensor, the basic procedure for grabbing is similar to grabbing from a 2D camera. However, you should typically pass a container with an [`M_GRAB`](../../Reference/buf/MbufAllocContainer.md) attribute. For more information, see [Grabbing from 3D sensors overview](../C42_Grabbing_from_3D_sensors/S01_Grabbing_from_3D_sensors_overview.md).

When your application requests a grab, either directly using [`MdigGrab`](../../Reference/dig/MdigGrab.md)or automatically using [`MdigProcess`](../../Reference/dig/MdigProcess.md), the grab is queued; Aurora Imaging Library grabs the next transmitted frame into the specified buffer. It is possible to configure some frame grabbers and cameras to wait for a hardware trigger or timer signal to grab/transmit a frame of data. When developing an application that uses triggers, it is recommended to begin with both your frame grabber and camera configured to grab/transmit frames of data continuously. Once you have implemented core functionality in your application, you can configure your frame grabber or camera to grab/transmit data using triggers. This approach can simplify debugging your application.
