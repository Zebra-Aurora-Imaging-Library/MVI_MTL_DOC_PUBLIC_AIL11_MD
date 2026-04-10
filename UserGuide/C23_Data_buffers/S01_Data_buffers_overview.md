---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Data_buffers_overview
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Data buffers overview"
---

# Data buffers overview

In this Help file, the term **data buffer** is used loosely to refer to the most general type of data buffer (storage area) that is allocated by the Aurora Imaging Library package and operated on by most Aurora Imaging Library functions. For example, a data buffer can be a buffer for image data or one for lookup table (LUT) data. Besides data buffers, there are also other buffers (for example, result buffers), which are specific to a particular group of functions. These types of buffers are discussed in the chapters describing their related functions.

All data buffers must be allocated before a function can access them. You can allocate a monochrome buffer using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md), [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md), or [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md). You allocate a color buffer using [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md).

When allocating a data buffer, you must specify its:

- Target Aurora Imaging Library system.
- Dimensions.
- Data type and depth.
- Attribute.

You can manipulate or control specific parts of data buffers by allocating and using child buffers. A child buffer is a subset of a parent buffer (a specific area of the parent buffer). Although any change made to the child buffer data affects the parent buffer, the buffer is considered a data buffer in its own right; wherever the parent buffer can be used, you can use the child buffer instead to affect only a part of the buffer. All results are returned relative to the child buffer coordinates rather than the parent buffer.

Another way to specify a part of an image buffer is to use a region of interest (ROI). The ROI identifies specific areas of the buffer for further processing or analysis. The ROI of a buffer can be any shape and can encompass non-contiguous areas; it can be defined using a 2D graphics list or an image mask. In addition, when defined using a 2D graphics list whose elements have been defined in world units, the region of interest will move according to the position of the relative coordinate system. Unlike a child buffer, which can be created for any type of buffer, an ROI can only be created for an image buffer. Furthermore, an ROI is not a buffer in its own right; it does not have its own Aurora Imaging Library identifier. When you call a function, you pass the identifier of the buffer in which the ROI is defined. If a processing or analysis function respects the ROI of an image buffer, the function will only operate on the areas dictated by the ROI; whether a function respects the ROI will be stated in the function's description. In addition, all results are returned relative to the buffer in which it is defined.
