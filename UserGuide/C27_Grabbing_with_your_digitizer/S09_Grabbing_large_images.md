---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Grabbing_large_images
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Grabbing large images"
---

# Grabbing large images

In some cases, you might need to grab an image whose size is in the order of gigabytes. Ideally, you would use a digitizer whose DCF specifies an appropriate frame size. In practice, this is not always possible because the on-board frame size is restricted by the memory space on-board. If you try to set a larger frame size using Aurora Imaging Intellicam, you will obtain an error when you call [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) due to insufficient memory space for the temporary on-board buffers required for the large frame.

A solution to the above problem is to do the following:

1. Allocate a large buffer on the Host using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_GRAB`](../../Reference/buf/MbufAllocColor.md).
2. Allocate child buffers in the large parent buffer using [`MbufChild...`](../../Reference/buf/MbufChild1d.md). The size of each child buffer will have to be equal to or smaller than the supported frame size specified by the DCF settings.
3. Put each child buffer into an array for easy access later.
4. Grab sectional frames of the large image into each child buffer in the array.

As mentioned in [Multiple buffering](S08_Grabbing_and_processing.md), to ensure that no frames are missed during a grab, you can use [`MdigProcess`](../../Reference/dig/MdigProcess.md). [`MdigProcess`](../../Reference/dig/MdigProcess.md) grabs a sequence of images into an array of image buffers and can cause a user-defined function to be called after an image has been grabbed into any of these buffers.
