---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Specialized_image_processing
section: Rearranging_areas_in_your_grabbed_image
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Specialized-image / Rearranging areas in your grabbed image"
---

# Rearranging areas in your grabbed image

If one or more areas appear out of order in your grabbed image (for example, when using a camera that returns nonsequential data), you might need to rearrange one or more areas in your grabbed image. Rather than creating multiple child buffers and performing multiple copies, you can use [`MimRearrange`](../../Reference/im/MimRearrange.md). This function copies specified areas from a newly grabbed image to a specified destination image.[`MimRearrange`](../../Reference/im/MimRearrange.md) might save some processing time and overhead when compared to multiple child buffer copy operations.

The [`MimRearrange`](../../Reference/im/MimRearrange.md) function uses a rearrangement image processing context to store processing settings. You must set up the rearrangement image processing context with the offsets of the required areas in the source image buffer, the offsets of the target areas in the destination image buffer, and the size of each area. Only the specified areas of the source image are copied from the source image to the areas specified in the destination image. The rest of the source image is ignored and not copied. The source and destination image buffers need not be of the same size, but must be equal to or larger than the largest supplied source and destination coordinates.

You can copy areas or rows of the source image. In the following example, two areas are copied from the source image to the destination image.

*[Image: IM_Rearrange.png]*

In the above illustration, you would have to set up the rearrangement image processing context with the following:

| Area | Array with the X-offsets of the source areas | Array with the Y-offsets of the source areas | Array with the X-offsets of the target areas | Array with the Y-offsets of the target areas | Array with the X- size of the areas | Array with the Y- size of the areas |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 105 | 121 | 0 | 0 | 72 | 77 |
| 2 | 220 | 59 | 72 | 0 | 107 | 200 |

Before calling [`MimRearrange`](../../Reference/im/MimRearrange.md), you must first set up the rearrangement image processing context by performing the following:

1. Allocate a rearrangement image processing context, using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_REARRANGE_CONTEXT`](../../Reference/im/MimAlloc.md).
2. Set the processing mode, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODE`](../../Reference/im/MimControl.md). The processing mode indicates how to perform the operation. If the processing mode is set to lines mode, each area is a single horizontal line. If the mode is set to rectangles mode, each area is a single rectangle.
3. Allocate 4 or 6 arrays with the same number of entries. When copying lines, only 4 arrays are required. When copying rectangles, 6 arrays are required. There should be one entry for every area to be rearranged.
4. Set up 2 arrays with the offsets of the areas in the source image: one array for the X-offset and one array for the Y-offset. Then, load these arrays into the context, using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_SOURCE`](../../Reference/im/MimPut.md). If copying lines, the X-coordinate of all the (x, y) values must be set to 0.
5. Set up 2 arrays with the offsets of the areas in the destination image: one array for the X-offset and one array for the Y-offset. Then, load these arrays into the context, using [`MimPut`](../../Reference/im/MimPut.md) with[`M_XY_DESTINATION`](../../Reference/im/MimPut.md). If copying lines, the X-coordinate of all the (x, y) values must be set to 0. The destination offset arrays must contain the same number of elements as the source offset arrays.
6. If copying rectangles, set up 2 arrays with the sizes of the areas: one array for the dimension in X and one array for the dimension in Y. Then, load these arrays into the context, using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_SIZE`](../../Reference/im/MimPut.md). If copying lines, you should not specify the size of the areas, otherwise an error is reported. By default, all lines (rows) are 1 pixel high and as wide as the source image.
7. Preprocess the context by calling [`MimRearrange`](../../Reference/im/MimRearrange.md) with [`M_PREPROCESS`](../../Reference/im/MimRearrange.md). Both the source and/or destination image buffers can be set to [`M_NULL`](../../Reference/im/MimRearrange.md). If, however, the source or destination image buffer is provided, it should be a typical source or destination image buffer, respectively, and it will be used by the preprocess operation to better optimize future calls. If the preprocess operation is not done explicitly, it will be done when [`MimRearrange`](../../Reference/im/MimRearrange.md) is first called.
