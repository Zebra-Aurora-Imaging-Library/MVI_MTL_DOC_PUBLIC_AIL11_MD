---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: YUV_buffers
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / YUV buffers"
---

# YUV buffers

YUV is a format in which Y is the grayscale band (luminance) and U and V are the color bands. Aurora Imaging Library supports grabbing, loading, or saving images in a YUV color format.

Although some general processing operations can be performed on YUV buffers, allocating YUV buffers for processing purposes is not recommended because these operations can only process color data that is in RGB format. However, Aurora Imaging Library will automatically convert YUV buffer data to RGB for these operations (including conversion for display), and re-convert it to YUV upon completion.

All YUV formats are supported even on the Host system. However, only some systems support grabbing into YUV buffers. See [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) in the Aurora Imaging Library Reference to determine if grabbing into YUV buffers is supported on your system.

YUV buffers must be allocated as 3-band 8-bit buffers, however, the actual number of bits per pixel will differ depending on the YUV format selected.

The supported YUV formats are:

- YUV16 Packed.
- YUV9 Planar.
- YUV12 Planar.
- YUV16 Planar.
- YUV24 Planar.

## YUV16 Packed

YUV16 Packed or YUV 4:2:2 ([`M_YUV16`](../../Reference/buf/MbufAllocColor.md)+[`M_PACKED`](../../Reference/buf/MbufAllocColor.md)) is an interleaved data format. Although each pixel has a corresponding one byte Y (luminance band), each pair of pixels share the same one byte U (chrominance U) and the same one byte V (chrominance V). Since a pair (two pixels) is represented by 4 bytes, each pixel has an average of 16 bits per pixel.

The YUV16 packed data format has two available formats: YUYV and UYVY. The only difference between these two YUV formats is the ordering of data in the buffer. Certain digitizer boards grab data in exclusively YUYV or UYVY packed data format. Note that, for display, certain operations are optimized to handle the YUYV format; for example, displaying a decompressed buffer.

When you allocate an [`M_YUV16`](../../Reference/buf/MbufAllocColor.md)+[`M_PACKED`](../../Reference/buf/MbufAllocColor.md) buffer, Aurora Imaging Library allocates the buffer in the format that is most suitable for the selected system and the specified buffer attributes. You can, however, force a format using the [`M_YUV16_YUYV`](../../Reference/buf/MbufAllocColor.md) or [`M_YUV16_UYVY`](../../Reference/buf/MbufAllocColor.md) control types. When the buffer has an [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute, forcing an inappropriate format generates an error. When the buffer has an [`M_DISP`](../../Reference/buf/MbufAllocColor.md) attribute, if you force the buffer in the other YUV format, then CPU intervention is required to perform the automatic conversion. See the [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) reference for supported data formats.

The following image is an example of a YUYV buffer:

*[Image: yuv16pck.png]*

The following image is an example of a UYVY buffer:

*[Image: yuv16pck1.png]*

## YUV9 Planar

YUV9 Planar ([`M_YUV9`](../../Reference/buf/MbufAllocColor.md)+[`M_PLANAR`](../../Reference/buf/MbufAllocColor.md)) is a planar format whose bands have a depth of one byte but are not of the same size. Although each pixel has a corresponding 1 byte Y (luminance) band, each block of 16 pixels share the same one byte of U (chrominance U) and the same one byte of V (chrominance V). Since the 16 pixels are represented by 18 bytes, each pixel has an average 9 bits. For example, a block of 16 pixels has the following: 16 bytes Y and 1 byte each of U and V.

*[Image: yuv9pln.png]*

## YUV12 Planar

YUV12 Planar ([`M_YUV12`](../../Reference/buf/MbufAllocColor.md)+[`M_PLANAR`](../../Reference/buf/MbufAllocColor.md)) is a planar format whose bands have a depth of one byte but are not of the same size. Although each pixel has a corresponding 1 byte Y (luminance) band, each block of 4 pixels share the same one byte of U (chrominance U) and the same one byte of V (chrominance V). Since the 16 pixels are represented by 24 bytes, each pixel has an average of 12 bits. For example, a block of 16 pixels has the following: 16 bytes Y and 4 bytes each of U and V.

*[Image: yuv12pln.png]*

## YUV16 Planar

YUV16 Planar ([`M_YUV16`](../../Reference/buf/MbufAllocColor.md)+[`M_PLANAR`](../../Reference/buf/MbufAllocColor.md)) is a planar format whose bands have a depth of one byte but are not of the same size. Although each pixel has a corresponding 1 byte Y (luminance) band, each block of 2 pixels share the same 1 byte of U (chrominance U) and the same 1 byte of V (chrominance V). Since the 16 pixels are represented by 32 bytes, each pixel has an average 16 bits. For example, a block of 16 pixels has the following: 16 bytes Y and 8 bytes each of U and V.

*[Image: yuv16pln.png]*

## YUV24 Planar

YUV24 Planar ([`M_YUV24`](../../Reference/buf/MbufAllocColor.md)+[`M_PLANAR`](../../Reference/buf/MbufAllocColor.md)) is an uncompressed planar format whose bands have a depth of one byte and are of equal size. Each pixel has a corresponding 1 byte Y (luminance) band, 1 byte U band (chrominance U), and 1 byte V band (chrominance V). Since the 16 pixels are represented by 48 bytes, each pixel has an average 24 bits. For example, a block of 16 pixels has the following: 16 bytes Y and 16 bytes each of U and V.

*[Image: yuv24.png]*

## Child YUV buffers

You can create child buffers from YUV buffers in the same way as RGB child buffers. When creating YUV child buffers, Aurora Imaging Library will keep the proportions of the U and V bands with respect to the Y band. For example, if your YUV9 Planar Y band is a size of 256x256 pixels, the U and V bands will be 1/4 the size of the Y band in each dimension (width and height): 64x64 pixels, which is 1/16 the size of the Y band. If a child buffer is 16x16 pixels, then the U and V bands will be 4x4 pixels. In other words, the 4x4 U and V bands (16 pixels) is 1/16 the size of the Y band (256 pixels).
