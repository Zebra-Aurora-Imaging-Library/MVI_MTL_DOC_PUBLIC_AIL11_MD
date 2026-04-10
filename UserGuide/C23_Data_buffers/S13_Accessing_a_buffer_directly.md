---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Accessing_a_buffer_directly
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Accessing a buffer directly"
---

# Accessing an Aurora Imaging Library buffer directly

If needed, an Aurora Imaging Library buffer's contents can be accessed directly. For instance, if you want to calculate the average value of the pixels in your image, you could create a custom algorithm. The algorithm could be applied directly to the buffer without having to copy the contents of the Aurora Imaging Library buffer into a user-allocated array using [`MbufGet`](../../Reference/buf/MbufGet.md). This would be more efficient and might improve the performance of the custom algorithm.

To access the Aurora Imaging Library buffer directly, you need to know the buffer's address, pitch, depth, size, and format. When dealing with image buffers, the format includes whether the image buffer is packed or planar, as well as its color format. The pitch is the number of pixels between the beginning of any two adjacent lines of the buffer data. The pitch can differ from the width of the buffer, especially when dealing with child buffers, where the pitch of the child is the pitch of the parent buffer. For this reason, we recommend you always inquire the pitch (using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_PITCH`](../../Reference/buf/MbufInquire.md)). To inquire the pitch in bytes, use [`M_PITCH_BYTE`](../../Reference/buf/MbufInquire.md).

*[Image: Accessbuffer.png]*

You can inquire the address of a parent or child buffer using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_HOST_ADDRESS`](../../Reference/buf/MbufInquire.md) or [`M_PHYSICAL_ADDRESS`](../../Reference/buf/MbufInquire.md). For an image buffer, the returned address will be the address of the top left-most pixel in the parent or child image buffer, respectively.

The following example shows how to inquire and use the Host address when accessing a monochrome buffer, a color packed buffer, and a color planar buffer.

> **Code example:** [mbufpointeraccess.cpp](mbufpointeraccess.cpp)
