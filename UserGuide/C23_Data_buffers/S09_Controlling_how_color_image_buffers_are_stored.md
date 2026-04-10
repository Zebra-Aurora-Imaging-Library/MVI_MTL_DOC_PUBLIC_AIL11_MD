---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Controlling_how_color_image_buffers_are_stored
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Controlling how color image buffers are stored"
---

# Controlling how color image buffers are stored

A color image buffer's internal representation can be either in a planar or packed format. When allocating the buffer, if you specify an [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md) attribute, the pixels are stored in planes (for example, RRR GGG BBB); if you specifiy an [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) attribute, each pixel is stored as one unit containing all its bands (for example, RGB RGB RGB).

Aurora Imaging Library automatically selects the most appropriate format, according to the specified intended purpose attribute. If an image buffer is allocated in one format, and a general processing function requiring another format is called, the function will automatically convert the data to the required format and re-convert it back to its original format upon completion. To change a buffer's default internal storage format, explicitly specify the required internal storage format when calling [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md). Note that it might be slower to process buffers with [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) attributes.

In general, packed formats are mostly used for display purposes; when setting a buffer's attribute to [`M_DISP`](../../Reference/buf/MbufAllocColor.md), the default internal representation is usually packed. This configuration allows for faster transfers to display sections.

Planar formats are generally preferred for processing. Processing is done on each of the band planes separately.

When allocating an image buffer with more than one attribute, for example, [`M_DISP`](../../Reference/buf/MbufAllocColor.md) and [`M_PROC`](../../Reference/buf/MbufAllocColor.md), the buffer's internal storage requirements for the display will take precedence over other attributes.

See [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) in the Aurora Imaging Library Reference to determine which formats are supported for your Aurora Imaging Library system.
