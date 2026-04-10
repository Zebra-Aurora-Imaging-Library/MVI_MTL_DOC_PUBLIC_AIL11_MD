---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Grabbing_images
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Grabbing images"
---

# Grabbing images

Many applications depend on the ability to grab an image for later analysis or inspection. With Aurora Imaging Library, you use an allocated digitizer to grab from a video source (typically a video camera). To allocate a digitizer, use [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) and specify a digitizer configuration format (DCF) that is compatible with the output format of your camera. This configures the required acquisition paths (camera interface) on the frame grabber so that they can accept input from the camera. With a call to [`MdigGrab`](../../Reference/dig/MdigGrab.md), you can then grab into a grab image buffer (an image buffer with an [`M_GRAB`](../../Reference/buf/MbufAlloc2d.md) attribute).

Allocate the grab image buffer on the same system, and of the same data format, as the digitizer. For color cameras, use color image buffers.

By default, when [`MdigGrab`](../../Reference/dig/MdigGrab.md) is issued, it grabs a complete frame of data, as defined by the DCF used to allocate the digitizer and not by the size of the image buffer. For more details on how to grab with the digitizer, see [Grabbing with your digitizer](../C27_Grabbing_with_your_digitizer/ChapterInformation.md).

## Continuous grabbing and adjusting your camera

When adjusting and focusing your camera, grabbing a single frame at a time can be tedious. Aurora Imaging Library features a continuous grab function, [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), that grabs image frames into a specified buffer until you issue [`MdigHalt`](../../Reference/dig/MdigHalt.md). Since you are grabbing continuously into the same buffer, this function is only really useful if you select the buffer to a display, using [`MdispSelect`](../../Reference/disp/MdispSelect.md), prior to starting the continuous grab, so that you can view what is being grabbed.

If your camera supports remote lens adjustment, you can use [`MdigFocus`](../../Reference/dig/MdigFocus.md) to automatically adjust the lens motor of the camera to achieve optimum focus in your images. For more information, see [Auto-focusing](../C27_Grabbing_with_your_digitizer/S12_Autofocusing.md).

## Sequential grabbing

Typically, you need to grab and process images continuously. To ensure that no frames are missed, the [`MdigProcess`](../../Reference/dig/MdigProcess.md) function allows you to grab images sequentially, store them into a list of image buffers, and process them as they are being grabbed.

[`MdigProcess`](../../Reference/dig/MdigProcess.md) has the same requirements as [`MdigGrab`](../../Reference/dig/MdigGrab.md), except [`MdigProcess`](../../Reference/dig/MdigProcess.md) accepts an array of grab image buffers and can cause a user-defined function to be called after an image has been grabbed into any of the image buffers. [`MdigProcess`](../../Reference/dig/MdigProcess.md) can round-robin through the buffers; in this case, buffers are filled with the grabbed data typically in the order they are stored in the list, wrapping around to the first buffer in the list once the last buffer in the list is filled.

For more information and examples, see [Multiple buffering](../C27_Grabbing_with_your_digitizer/S08_Grabbing_and_processing.md).

## An example

The following example grabs a single image from the camera.

> **Code example:** [userguide.building_an_application.grabbing_images](userguide.building_an_application.grabbing_images)
