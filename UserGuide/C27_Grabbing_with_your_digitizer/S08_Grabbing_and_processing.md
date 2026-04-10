---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Grabbing_and_processing
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Grabbing and processing"
---

# Grabbing and processing

To optimize application performance when grabbing, you can:

- Set the grab mode.
- Perform multiple buffering.

## Grab mode

When grabbing data with [`MdigGrab`](../../Reference/dig/MdigGrab.md), you can control the synchronization by setting the [`M_GRAB_MODE`](../../Reference/dig/MdigControl.md) control type of [`MdigControl`](../../Reference/dig/MdigControl.md)   to [`M_SYNCHRONOUS`](../../Reference/dig/MdigControl.md), [`M_ASYNCHRONOUS`](../../Reference/dig/MdigControl.md), or [`M_ASYNCHRONOUS_QUEUED`](../../Reference/dig/MdigControl.md) (if supported).

- If the grab mode is set to [`M_SYNCHRONOUS`](../../Reference/dig/MdigControl.md), your application will be synchronized with the end of a grab operation. In other words, your application will wait until the grab has finished before executing the next function.
- If the grab mode is set to [`M_ASYNCHRONOUS`](../../Reference/dig/MdigControl.md), your application will not be synchronized with the end of a grab operation. This option allows other functions to execute while you are still grabbing. This is a useful option when performing multiple buffering, a technique whereby you can grab data into one buffer while processing previously grabbed buffers (discussed below). Note, another call to [`MdigGrab`](../../Reference/dig/MdigGrab.md) before the current grab has finished will cause your application to wait until the current grab has finished.
- If your frame grabber supports queuing, you can set the grab mode to [`M_ASYNCHRONOUS_QUEUED`](../../Reference/dig/MdigControl.md); if another grab is issued before the first one is finished, the grab will be queued on-board, allowing you to perform other processes while waiting for the next [`MdigGrab`](../../Reference/dig/MdigGrab.md) to be executed. Note, you can still force your application to wait until the end of a grab before executing an operation, by calling [`MdigGrabWait`](../../Reference/dig/MdigGrabWait.md).

Note that [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md) is by definition asynchronous since you must use [`MdigHalt`](../../Reference/dig/MdigHalt.md) to stop the grab.

## Multiple buffering

Multiple buffering involves grabbing into one image buffer while processing previously grabbed images. This technique allows you to grab and process images concurrently.

To perform multiple buffering in Aurora Imaging Library, you can use [`MdigProcess`](../../Reference/dig/MdigProcess.md). It allows you to use a list of previously-allocated buffers to hold a series of sequentially grabbed images and process them as they are being grabbed. These grabs can either continue until stopped (using [`MdigProcess`](../../Reference/dig/MdigProcess.md) with [`M_START`](../../Reference/dig/MdigProcess.md) to start and then [`M_STOP`](../../Reference/dig/MdigProcess.md) to stop) or continue until all buffers in the list are filled (using [`MdigProcess`](../../Reference/dig/MdigProcess.md) with [`M_SEQUENCE`](../../Reference/dig/MdigProcess.md)). In the former case, [`MdigProcess`](../../Reference/dig/MdigProcess.md) grabs round-robin through the list of buffers (buffers are filled with the grabbed data typically in the order they are stored in the list, wrapping around to the first buffer in the list once the last buffer in the list is filled); however, care must be taken to ensure that the average time it takes to process a frame is not greater than the frame rate of a camera, so that frames will not be missed.

Once a frame of data is grabbed into a buffer/container, Aurora Imaging Library will not grab into that buffer/container again until the hook function has finished processing it. Once the hook function finishes executing, the buffer/container becomes available for Aurora Imaging Library to grab into it. If all buffers/containers are filled with data and are waiting to be processed by the hook function, Aurora Imaging Library waits for the next available buffer/container (typically the next buffer/container in the list) before grabbing another frame of data.

Optionally, instead of grabbing data into a list of previously allocated buffers, [`MdigProcess`](../../Reference/dig/MdigProcess.md) can automatically allocate an appropriate image buffer to store each grabbed frame of data. This is particularly useful when grabbing from a camera that is configured to transmit images with different dimensions for each grab. To specify that the buffers should be automatically allocated, set [`DestContainerOrImageBufArrayPtr`](../../Reference/dig/MdigProcess.md)to[`M_NULL`](../../Reference/dig/MdigProcess.md).

[`MdigProcess`](../../Reference/dig/MdigProcess.md) hooks a user-defined function to the modification of any buffer in the specified list. So every time a new frame is grabbed in a buffer in the list, the user-defined function is called. If the hook-handler function modifies the buffer that called it, the hook-handler function for that buffer will not be called again until that buffer is modified by a new grab. This is to avoid infinite recursive calls being generated. Note that, if the buffer is modified by some function other than the one hooked to it, the hooked function will be called. In addition, if the hook-handler function modifies another buffer which has another function hooked, that function will be called.

Note, processing is generally faster if the buffer is not on the display.

The following example shows you how to perform multiple buffering.

> **Code example:** [MDigProcess.cpp](MDigProcess.cpp)
