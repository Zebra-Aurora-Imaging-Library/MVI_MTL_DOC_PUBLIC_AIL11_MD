---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Multi_band_buffers
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Multi band buffers"
---

# Multi-band buffers

While 1-band image buffers store a single value for each pixel in an image, multi-band image buffers store multiple values per pixel. Each band in a multi-band buffer represents different information. For example, RGB image buffers contain 3 bands to represent the red, green, and blue values of an image, which produce a full-color image when combined. In Aurora Imaging Library, image buffers can contain up to 1024 bands.

Note that 3-band image buffers are used to store color data. All other multi-band buffers are used for non-color data.

Note that only the Buffer, Graphics, Data Generation, and Image Processing modules support the full range of multi-band buffers. Other modules support only 1-band and 3-band image buffers. Note that support also varies within each module; see specific functions for details on the number of bands supported.

## Non-color buffers

3-band image buffers always have a color space and are sometimes referred to as color buffers. For more information about color spaces, see [Color spaces and converting between them](../C22_Color/S03_Color_spaces_and_converting_between_them.md). All other multi-band buffers (those with 2 bands or more than 3 bands) are non-color buffers. Note that 1-band image buffers are monochrome buffers since they only have a single band.

Since multi-band buffers store data using multiple bands, they can represent a wide range of information, not limited to color data. This allows for comprehensive analysis of various aspects of a scene, such as texture or changes over time. Non-color buffers are typically used to represent data across a range of wavelengths outside of the visible light spectrum, such as infrared or ultraviolet light.

Note that all non-color buffers are always stored in planar format.

### Loading data into a non-color multi-band buffer

Non-color multi-band buffers are typically initialized by loading data stored on disk. If your acquisition device saves one image per band, you can load the bands one at a time. For each band, use [`MbufChildColor`](../../Reference/buf/MbufChildColor.md) to allocate a child buffer within the multi-band parent buffer (previously allocated using [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md)), and load the band's corresponding image, using [`MbufImport`](../../Reference/buf/MbufImport.md) with [`M_LOAD`](../../Reference/buf/MbufImport.md), or using [`MbufLoad`](../../Reference/buf/MbufLoad.md). If your acquisition device saves all the bands in a single file, you cannot use [`MbufImport`](../../Reference/buf/MbufImport.md) or [`MbufLoad`](../../Reference/buf/MbufLoad.md). In this case, consult the technical specifications provided by your device manufacturer to determine how to load the data, and use [`MbufPutColor`](../../Reference/buf/MbufPutColor.md) to populate the bands of your multi-band buffer.

## Multiple multi-band buffers

When two or more multi-band buffers are used in a function, they must all have the same number of bands; otherwise, an error will occur. For example, if your source image buffer has 4 bands, all other source and destination image buffers must have either 4 bands or 1 band. You can't, for example, pass a 4-band image buffer and a 5-band image buffer to a function. This applies to any function that supports multi-band buffers, with the exception of [`MbufCopyColor`](../../Reference/buf/MbufCopyColor.md), [`MbufCopyColor2d`](../../Reference/buf/MbufCopyColor2d.md), and [`MbufTransfer`](../../Reference/buf/MbufTransfer.md). For [`MbufCopyColor`](../../Reference/buf/MbufCopyColor.md), this rule only applies if the [`Band`](../../Reference/buf/MbufCopyColor.md) parameter is set to [`M_ALL_BANDS`](../../Reference/buf/MbufCopyColor.md). For [`MbufCopyColor2d`](../../Reference/buf/MbufCopyColor2d.md), this rule only applies if the [`SrcBand`](../../Reference/buf/MbufCopyColor2d.md) and [`DstBand`](../../Reference/buf/MbufCopyColor2d.md) parameters are set to [`M_ALL_BANDS`](../../Reference/buf/MbufCopyColor2d.md). For [`MbufTransfer`](../../Reference/buf/MbufTransfer.md), this rule only applies if the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_COPY`](../../Reference/buf/MbufTransfer.md) and the [`SrcBand`](../../Reference/buf/MbufTransfer.md) and [`DestBand`](../../Reference/buf/MbufTransfer.md) parameters are set to [`M_ALL_BANDS`](../../Reference/buf/MbufTransfer.md).
