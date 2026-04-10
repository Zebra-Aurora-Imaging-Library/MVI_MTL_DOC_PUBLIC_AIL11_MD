---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Mapping_a_data_buffer_to_userallocated_memory
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Mapping a data buffer to userallocated memory"
---

# Mapping a data buffer to user-allocated memory

Instead of allocating new memory to a buffer using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md), you can create a buffer from the memory at a specified location, using [`MbufCreate2d`](../../Reference/buf/MbufCreate2d.md) to create a monochrome data buffer and [`MbufCreateColor`](../../Reference/buf/MbufCreateColor.md) to create a color data buffer. In these cases, Aurora Imaging Library does not allocate any memory; it uses the memory that you provide.

When creating a buffer with [`MbufCreateColor`](../../Reference/buf/MbufCreateColor.md), you can pass an array of data pointers. For packed color buffers, you can pass an array of one pointer; for planar buffers, you can pass an array with the same number of pointers as the number of bands in the buffer. When creating a buffer with [`MbufCreate2d`](../../Reference/buf/MbufCreate2d.md), you can pass the address of the data. The address(es) can be either logical or physical. If you want to use the buffer for grabbing, the address(es) must be physical (grab buffers must be allocated in physically contiguous and always present memory, that is, non-paged).

The [`MbufCreate...`](../../Reference/buf/MbufCreate2d.md) functions must be used with caution because, when using physical addresses, these functions provide direct access to any of your computer's memory mapped devices; when using logical addresses, memory protection errors could result.

You can use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with the [`M_HOST_ADDRESS`](../../Reference/buf/MbufInquire.md) or [`M_PHYSICAL_ADDRESS`](../../Reference/buf/MbufInquire.md) inquire type to determine the Host's logical address or the physical address of a buffer's data, respectively. Note that the physical address is not necessarily an address in Host memory. It could be an address in on-board memory. If an on-board buffer is mapped to the Host, you can use the [`MbufInquire`](../../Reference/buf/MbufInquire.md) function with the [`M_HOST_ADDRESS`](../../Reference/buf/MbufInquire.md) inquire type to determine the Host address to which it is mapped.

Both [`MbufCreate2d`](../../Reference/buf/MbufCreate2d.md) and [`MbufCreateColor`](../../Reference/buf/MbufCreateColor.md) can also be used to create a buffer that maps to an already existing buffer. When mapping to an existing buffer, pass the Aurora Imaging Library identifier of the buffer to [`MbufCreate2d`](../../Reference/buf/MbufCreate2d.md) or to [`MbufCreateColor`](../../Reference/buf/MbufCreateColor.md). Note that on some Zebra frame grabbers, on-board buffers are allocated in unshared memory by default. In this case, [`MbufCreate2d`](../../Reference/buf/MbufCreate2d.md) and [`MbufCreateColor`](../../Reference/buf/MbufCreateColor.md) are not supported, unless creating the buffer on the same system.

There are several instances when memory mapping is useful. A particularly useful instance is when processing and displaying an interlaced grab in a time critical application. In this case, you could use a displayable image buffer to store and display the grabbed data. Then, to process each field as it is grabbed, you could use a buffer that is mapped to the odd field of the displayable image buffer (Buffer 1) and a buffer that is mapped to the even field (Buffer 2).

Create Buffers 1 and 2 as follows:

*[Image: memory_mapping.png]*

Buffer 1 (Odd field):

- Size = 640x240 (that is, half height).
- Pitch = 1280 (that is, to skip to the next field).
- Address = Address A (that is, first pixel of the first row).

Buffer 2 (Even field):

- Size = 640x240 (that is, half height).
- Pitch = 1280 (that is, to skip to the next field).
- Address = Address B (that is, first pixel of the second row).

In general, Aurora Imaging Library automatically issues a display update after a displayed buffer has been modified. However, if a buffer selected on the display is modified using a mapped buffer, its display is not updated until you notify it of the change using the [`M_MODIFIED`](../../Reference/buf/MbufControl.md) setting in [`MbufControl`](../../Reference/buf/MbufControl.md).

*[Image: Notifyingofbufferchange.png]*

For another instance where creating buffers is useful, see [Multiple systems](../C02_Building_an_application/S15_Multiple_systems.md).
