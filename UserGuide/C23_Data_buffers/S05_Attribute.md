---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Attribute
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Attribute"
---

# Attribute

The data buffer attribute indicates the buffer type and its intended usage. Aurora Imaging Library uses this information to determine the most appropriate location in physical memory in which to allocate the buffer, and how to handle the buffer. A data buffer can be one of the following types:

- [`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md). Image buffers are used to store grabbed images.
- [`M_LUT`](../../Reference/buf/MbufAlloc2d.md). Lookup table buffers are used to store lookup table data.
- [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md). Kernel buffers define the filters used by convolution functions.
- [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md). Structuring element buffers are used to store the structuring element data for morphology functions.
- [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md). Array buffers store array type data.

When allocating an image buffer ([`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md)), you must give more information about its intended usage. An image buffer can be any combination of the following:

- A buffer that can be displayed ([`M_DISP`](../../Reference/buf/MbufAlloc2d.md)).
- A buffer that can be processed ([`M_PROC`](../../Reference/buf/MbufAlloc2d.md)).
- A buffer in which data can be grabbed ([`M_GRAB`](../../Reference/buf/MbufAlloc2d.md)).
- A buffer in which data is stored in a compressed format ([`M_COMPRESS`](../../Reference/buf/MbufAlloc2d.md)).

For example, to allocate an image buffer that can be displayed and used for processing, its attribute should be given as:

[`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md)+[`M_DISP`](../../Reference/buf/MbufAlloc2d.md)+[`M_PROC`](../../Reference/buf/MbufAlloc2d.md)+[`M_GRAB`](../../Reference/buf/MbufAlloc2d.md).

Image buffers without an [`M_DISP`](../../Reference/buf/MbufAlloc2d.md), [`M_PROC`](../../Reference/buf/MbufAlloc2d.md), [`M_GRAB`](../../Reference/buf/MbufAlloc2d.md), or [`M_COMPRESS`](../../Reference/buf/MbufAlloc2d.md) attribute can still be used with the Buffer and Graphics modules.

> **Note:** The [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md) and [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) attributes are not required to work under Aurora Imaging Library Lite.

When a displayable image buffer is allocated and selected for display ([`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_DISP`](../../Reference/buf/MbufAlloc2d.md), and then [`MdispSelect`](../../Reference/disp/MdispSelect.md)), multiple buffers are maintained: one in Host memory for processing purposes, and other internal buffers in display or non-paged memory (maintained directly or using the normal Windows mechanisms) for display purposes (not necessarily the same size). When the Host buffer is modified, its associated internal buffers in display/non-paged memory are automatically updated. When displaying an image buffer, both the buffer and the display should be allocated on the same system.

When grabbing a single frame into a displayable image buffer, Aurora Imaging Library grabs into the Host memory version of the buffer and then updates the display of the buffer. When grabbing continuously, the grab will typically bypass the specified buffer, and grab into an intermediate temporary display buffer (in display or Host non-paged memory) to minimize CPU usage and improve performance. Only the last frame grabbed is written into the selected buffer.

It is possible to force the internal representation of a data buffer using internal storage format specifiers, such as [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) or [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md), which force the data buffer to be in a packed or planar format, respectively. Refer to [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) for a complete list of internal format specifiers.

If you try to use a data buffer in a situation that is not appropriate for its allocated attributes, an error message is generated and the operation is not performed. For example, if you try to display an image buffer without an [`M_DISP`](../../Reference/buf/MbufAllocColor.md) attribute with [`MdispSelect`](../../Reference/disp/MdispSelect.md), an error message will be generated.

## Memory locations

By default, all Zebra Imaging boards allocate buffers in shared Host memory instead of on-board memory. On these boards, on-board memory is typically limited in size and Host memory can be accessed much faster than on-board memory. Changing the default location for your buffer's allocation is typically done only when dealing with large buffers or limited amounts of memory. In most cases using the default memory storage locations will produce the best results. The following memory storage locations are available:

- [`M_HOST_MEMORY`](../../Reference/buf/MbufAlloc1d.md). Stores the buffer in Host memory. Note that in all cases, [`M_HOST_MEMORY`](../../Reference/buf/MbufAlloc1d.md) is the same as [`M_OFF_BOARD`](../../Reference/buf/MbufAlloc2d.md).
- [`M_ON_BOARD`](../../Reference/buf/MbufAlloc2d.md). Stores the buffer in memory located on your Zebra Imaging board.
- [`M_OFF_BOARD`](../../Reference/buf/MbufAlloc2d.md). Stores the buffer in memory located off your Zebra Imaging board.

### Using on-board memory

Using on-board memory can improve performance when all buffers are allocated and processed on-board. This is beneficial when using boards with fast-processing memory.

There are three types of on-board memory available for some Zebra Imaging boards:

- [`M_SHARED`](../../Reference/buf/MbufAlloc2d.md). Available in all boards with an FPGA, this forces the buffer in memory that is mapped on the PCI bus. Note that shared memory is only available on-board.
- **Unshared memory**. This is the default memory location for all boards with a FPGA. The buffer is stored in unmapped memory.

### Using paged or non-paged memory

There are two types of memory locations available to all Zebra Imaging boards:

- [`M_PAGED`](../../Reference/buf/MbufAlloc1d.md). Forces the buffer into paged memory.
- [`M_NON_PAGED`](../../Reference/buf/MbufAlloc1d.md). Forces the buffer into non-paged memory.

Paging memory at allocation divides the available computer memory into small portions where the page is the smallest building block. Paging memory helps reduce the amount of external fragmentation and thus little memory is wasted. Use the Aurora Imaging Configurator utility to set the amount of non-paged memory available to your system. The rest of the memory is available for paging memory. Whenever possible, use the default settings for paged and non-paged memory.

If there is insufficient memory of the appropriate type to allocate a buffer with the specified attributes, the function generates an error and does not allocate the buffer.
