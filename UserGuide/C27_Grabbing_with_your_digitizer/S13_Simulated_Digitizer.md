---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Simulated_Digitizer
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Simulated Digitizer"
---

# Simulated digitizer

In some cases, you might need to develop or debug an application on a computer that does not have access to the hardware which will be used or is used to grab images. In these cases, you can use a simulated digitizer to grab the required images and to develop/test the rest of your application.

A simulated digitizer can grab a single image from a single file (stored in any file format supported by [`MbufImport`](../../Reference/buf/MbufImport.md) except for _*.raw_), multiple images from a single video file (_*.avi_), a series of images from all supported files in a directory, or multiple images from a pattern generator. When grabbing from a video or directory of images, once the last image is grabbed, grabbing will restart from the beginning of the video or from the first file in the directory.

To use a simulated digitizer, you must allocate a Host system, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_HOST`](../../Reference/sys/MsysAlloc.md). Then, allocate a simulated digitizer on this system, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_EMULATED`](../../Reference/dig/MdigAlloc.md). Note that these allocations can be done with a single call, using [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) if a Host system and a simulated digitizer are set as the defaults at installation or using the Aurora Imaging Configurator utility. This is actually the recommended way because it allows you to debug your code without significant modification.

Although you can import an image (or a sequence of images), using [`MbufImport`](../../Reference/buf/MbufImport.md) or [`MbufImportSequence`](../../Reference/buf/MbufImportSequence.md), using a simulated digitizer to grab images grants access to common digitizer controls, inquires, hooks, and LUTs. Note that, when using a simulated digitizer, the X- and Y-size of each image in the path should be the same; otherwise, the X- and Y-size is taken from the first image and all subsequent images are either cropped to these dimensions, or expanded without zooming or stretching the original image, accordingly. In the latter case, portions of the previous image(s) might still be visible at the borders if the new image is smaller than the previous image.

When allocating a simulated digitizer, you must specify to use either a pattern generator (a simulated DCF or SDCF) or the path to one or a series of static images or a video file. The video file or pattern generator is best used when testing the timing of your triggers and grabbing, whereas a series of static images is best used when testing specific problem cases.

To learn the various settings available for your simulated digitizer, refer to the Host system column of the Aurora Imaging Library command reference in the [`Mdig...`](../../Reference/dig/MdigAlloc.md) module. Note that, if you try to perform an operation using a simulated digitizer but the operation is not supported, an error or unpredictable behavior can occur.

In the case where a simulated digitizer is being used to debug an application intended for another Aurora Imaging Library system (for example, a Zebra Radient eV-CL or Zebra Clarity UHD), you should encapsulate any reference to hardware-specific control types or inquire types that are not supported by a simulated digitizer (such as camera lock sensitivity, timers, and I/Os) in an if statement. This allows your application to run without error even when using a simulated digitizer.

> **Code example:** [userguide.grabbing_with_your_digitizer_simulateddigitizer](userguide.grabbing_with_your_digitizer_simulateddigitizer)

Once you have finished using a simulated digitizer, you should free it, using [`MdigFree`](../../Reference/dig/MdigFree.md).
