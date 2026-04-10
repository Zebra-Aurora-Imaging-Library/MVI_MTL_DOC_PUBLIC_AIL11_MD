---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Specialized_image_processing
section: Correcting_dead_pixels
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Specialized-image / Correcting dead pixels"
---

# Correcting dead pixels

If one or more pixels remains unchanged in the same place in a series of grabbed images, irresepective of the image being grabbed, the cause could be dead pixels on your camera's CCD. To replace the dead pixels on newly grabbed images with an average pixel value, taken from the neighboring pixels, use [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md). This function uses a dead-pixel image processing context to store processing settings.

You can list the coordinates of the dead pixels, using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_DEAD_PIXELS`](../../Reference/im/MimPut.md) and two arrays (the first containing the x-coordinates, and the second containing the y-coordinates). Alternatively, create an Aurora Imaging Library image buffer to act as a mask, where all non-zero pixels are considered dead pixels. Once specified, the coordinates of the non-zero pixels can be inquired, using [`MimGet`](../../Reference/im/MimGet.md) with [`M_XY_DEAD_PIXELS`](../../Reference/im/MimGet.md).

*[Image: IM_DeadPixels_UG_02.png]*

[`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) replaces dead pixels with an average of their non-dead pixel neighbors. Other pixels are copied from the source image buffer into the destination image buffer unchanged. The source and destination image buffer must be as large (or larger than) the dead pixel mask or equal to or larger than the largest dead pixel position of the array containing the x- and y-coordinates of the dead pixels for the source and destination, respectively.

If the dead pixel mask is smaller than the source image, dead pixels on the edge of the intersection of the mask, the source image, and the destination image will only use neighboring pixels that are inside the intersection of the images when calculating the correction. Dead pixels outside the intersection are ignored.

*[Image: IM_DeadPixels_UG_01.png]*

Preprocess the image processing context by calling [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) with [`M_PREPROCESS`](../../Reference/im/MimDeadPixelCorrection.md). Both the source and destination image buffers can be set to [`M_NULL`](../../Reference/im/MimDeadPixelCorrection.md). If, however, the source or destination image is provided, it should be a typical source or destination buffer, respectively, and it will be used in the preprocess operation, to better optimize future calls to [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md). If the preprocess operation was not done explictly (using [`M_PREPROCESS`](../../Reference/im/MimDeadPixelCorrection.md)), it will be done when [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) is first called.
