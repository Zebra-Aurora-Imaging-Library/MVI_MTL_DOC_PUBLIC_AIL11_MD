---
doctype: UserGuide
part: "2D related information"
chapter: Lookup_tables
section: LUTs_and_data_buffers
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / lut / LUTs and data buffers"
---

# LUTs and data buffers

The Aurora Imaging Library package represents LUTs as LUT data buffers. As with any other data buffer, LUT buffers must be allocated before they are used. A LUT buffer can be loaded, stored, or copied to another buffer (not necessarily to another LUT buffer) or to disk. You can also allocate child LUT buffers. When a LUT buffer is no longer required, you should free its memory space, using [`MbufFree`](../../Reference/buf/MbufFree.md).

LUT buffers are typically one-dimensional data buffers created with [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) (single row). However, you can allocate a color RGB LUT, using [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md). In this case, set the number of bands to 3 (for RGB), the Y-dimension to 1, and the X-dimension to have enough entries to represent the full range of possible values of the image buffer.
