---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: RGB_buffers
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / RGB buffers"
---

# RGB buffers

By default, Aurora Imaging Library allocates color image buffers in an RGB color format. The pixels are internally stored in little-endian order, that is, they are stored in memory from their least-significant to the most significant bytes. The definitions of the RGB formats that are available are shown here. The corresponding Aurora Imaging Library constant is shown in brackets beside the common format name.

## RGB data formats

**BGR24 packed** ([`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+[`M_BGR24`](../../Reference/buf/MbufAllocColor.md)) and **RGB24 packed** ([`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+[`M_RGB24`](../../Reference/buf/MbufAllocColor.md)) are formats whereby each pixel is internally stored as three consecutive bytes. In BGR24 packed format, the blue band is stored in the first byte of the sequence; in RGB24 packed, the red byte is stored in the first byte of the sequence.

*[Image: bgr24.png]*

*[Image: rgb24.png]*

**BGR32 packed** ([`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+[`M_BGR32`](../../Reference/buf/MbufAllocColor.md)) is a format whereby each pixel is internally stored as four consecutive bytes. The first byte of the sequence stores the blue band of the image, and the last byte is a "don't care" byte, as shown below:

*[Image: rgb32.png]*

**RGB15 packed** ([`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+[`M_RGB15`](../../Reference/buf/MbufAllocColor.md)) is a format whereby each pixel is internally stored as a 16-bit word with a 5-bit blue value (least significant), a 5-bit green value, a 5-bit red value, and a "don't care" bit (most significant), in little-endian order, as shown below. Note that when accessing an [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) +[`M_RGB15`](../../Reference/buf/MbufAllocColor.md)buffer as a 3-band 8-bit buffer, the least significant bits of each band are set to 0.

*[Image: rgb15.png]*

**RGB16 packed** ([`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+[`M_RGB16`](../../Reference/buf/MbufAllocColor.md)) is a format whereby each pixel is internally stored as a 16-bit word with a 5-bit blue value (least significant), a 6-bit green value, and a 5-bit red value (most significant), in little-endian order, as shown below. Note that when accessing an [`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+[`M_RGB16`](../../Reference/buf/MbufAllocColor.md) buffer as a 3-band 8-bit buffer, the least significant bits of each band are set to 0.

*[Image: rgb16.png]*

**RGB planar** are formats whereby the color bands of all the pixels are stored contiguously: (RRR...., BBB..., GGG...).
