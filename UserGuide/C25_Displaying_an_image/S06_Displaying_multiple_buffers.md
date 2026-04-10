---
doctype: UserGuide
part: "2D related information"
chapter: Displaying_an_image
section: Displaying_multiple_buffers
module_tag: disp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / display / Displaying multiple buffers"
---

# Displaying multiple buffers

You can view one image buffer at a time in a display, but you can view multiple image buffers using multiple displays. Select the image buffers to different displays, using [`MdispSelect`](../../Reference/disp/MdispSelect.md).

Using multiple [windowed displays](S02_Types_of_displays.md), you can view more than one buffer at the same time on the Windows desktop screen(s).

For [exclusive displays](S02_Types_of_displays.md), you can have as many displays as you have appropriate screens; however, you can have only one display at a time on a given screen. To view more than one image at a time in one display, use [child buffers](../C23_Data_buffers/S06_Using_child_buffers_ROIs_or_a_copy_to_manipulate_specific_data_areas.md). For example, you can display the source and destination images of an operation, using the following steps:

1. Allocate a large displayable image buffer (big enough to contain the source and destination images) using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) or [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md). This buffer will be known as the parent buffer.
2. Allocate two non-overlapping child buffers within it, using [`MbufChild2d`](../../Reference/buf/MbufChild2d.md) or [`MbufChildColor`](../../Reference/buf/MbufChildColor.md).
3. Select the parent buffer for display using [`MdispSelect`](../../Reference/disp/MdispSelect.md).
4. Use one of the child buffers as the source image buffer and the other as a destination image buffer of the operation.

The following example shows how to display multiple image buffers in a single display. The source image, _Bird.mim_, is loaded into a child of a displayable image buffer and then used as the source of an image processing operation (increasing the image luminance). The result is stored in the other child of the same image displayable buffer.

> **Code example:** [MBufColor.cpp](MBufColor.cpp)

> **Note:** For Aurora Imaging Library Lite, the luminance operation is not performed; the image is merely copied from the left child buffer to the right.
