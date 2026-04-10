---
doctype: UserGuide
part: "2D related information"
chapter: JPEG_and_JPEG2000_compression
section: General_steps
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / jpeg / General steps"
---

# General steps

This section discusses general steps in compressing and decompressing images and sequences, as well as other aspects of compression.

> **Note:** Note that during development and at runtime, compression support is reliant upon the presence of a compression/decompression package license. This license is only included by default with the development dongle for the full version of Aurora Imaging Library. In other cases, this license must be purchased separately.

## Compression

To compress an image and save it in an image buffer:

1. Allocate a buffer in which to hold the compressed image. Use [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) combined with a compression attribute value.
   > **Note:** Compressed buffers that are created using the [`MbufCreate...`](../../Reference/buf/MbufCreate2d.md) functions should not be used as the destination buffer of an Aurora Imaging Library function. However, if a buffer with an [`M_COMPRESS`](../../Reference/buf/MbufCreate2d.md) attribute is used as a source buffer for an operation, the data will be decompressed depending on the attributes of the destination buffer.
2. If necessary, change the control settings of the buffer, using [`MbufControl`](../../Reference/buf/MbufControl.md).
   For example, for a JPEG or JPEG2000 lossy compression, you might want to change the quantization factor ([`M_Q_FACTOR`](../../Reference/buf/MbufControl.md)), which is one of the factors that determine the amount of compression. The default value of the quantization factor is 50; setting a lower value will produce marginal improvement in image quality and will result in a larger file size; setting a higher value will produce a smaller file, and therefore a poorer quality image.
3. If the image to compress is stored in a buffer, use [`MbufCopy`](../../Reference/buf/MbufCopy.md) to compress it into the buffer allocated in step 1. If it is stored in a file, use [`MbufImport`](../../Reference/buf/MbufImport.md).
   You can also automatically compress your grabbed images. To do so, use [`MdigGrab`](../../Reference/dig/MdigGrab.md) with a destination buffer that has an [`M_GRAB`](../../Reference/buf/MbufAllocColor.md)+[`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) + _CompressionType_ attribute. Note that due to the computational complexity of JPEG2000 compression, grabbing into such a buffer presents a risk of missing frames.
   > **Note:** Compression operations are optimized when the uncompressed source buffer and the compressed destination buffer are in the same format. Typically, buffers in YUV16 format produce the best compromise for quality and speed.

Note that, if you want the compressed image stored on file rather than in a buffer, use [`MbufExport`](../../Reference/buf/MbufExport.md) instead of [`MbufCopy`](../../Reference/buf/MbufCopy.md). In this case, there is no need to allocate a destination buffer.

## Decompression

To decompress an image, use [`MbufCopy`](../../Reference/buf/MbufCopy.md), [`MbufImport`](../../Reference/buf/MbufImport.md), or [`MbufExport`](../../Reference/buf/MbufExport.md), depending on where the source image is stored (in a buffer or on file) and where you want results written (to a buffer or file).

Before the decompression, you should not change any control settings in the source image; the same controls must be used for decompression, otherwise the image data will be lost. The only exception to this rule is for JPEG2000 lossy compression, where you can change the target size of the image (the [`M_TARGET_SIZE`](../../Reference/buf/MbufControl.md) control type).

> **Note:** Decompression operations are optimized when the compressed source and uncompressed destination buffers are in the same format. Typically, buffers in YUV16 format produce the best compromise for quality and speed.

> **Note:** Decompressing a JPEG buffer into a YUV16 packed (YUYV) buffer might accelerate transfer to the display.

## Sequences

When compressing sequences, you can use [`MbufImportSequence`](../../Reference/buf/MbufImportSequence.md) to import a sequence of images from an audio video interleave (AVI) file into separate compressed buffers. You can use [`MbufExportSequence`](../../Reference/buf/MbufExportSequence.md) to export a sequence of compressed image buffers to an AVI file.

## Multi-band buffers, color formats, and control settings - JPEG

When you allocate a multi-band buffer for a JPEG lossy compression, you can specify that the compressed image be stored in an RGB or YUV format. YUV is convenient because most image-viewing software support compressed color images in YUV16 format.

If you are performing a JPEG lossy compression on a YUV image, you can use the control types that specify luminance (such as [`M_QUANTIZATION_LUMINANCE`](../../Reference/buf/MbufControl.md)) to control the Y band; to control the U and V bands, use the control types that specify chrominance (such as [`M_HUFFMAN_DC_CHROMINANCE`](../../Reference/buf/MbufControl.md)). The control types without these suffixes control all bands. See the _Aurora Imaging Library Reference_ for the list of YUV-specific control types.

When the specified compressed buffer format differs from that of the source image, Aurora Imaging Library will internally convert the source image to the specified format before performing the compression.

## Multi-band buffers, color formats, and control settings - JPEG2000

When you allocate a multi-band buffer for a JPEG2000 compression, you can specify that the compressed image be stored in an RGB or YUV format. If you are compressing a multi-band buffer in JPEG2000, you can specify different control settings for each band. To do so, add [`M_RED`](../../Reference/buf/MbufControl.md), [`M_BLUE`](../../Reference/buf/MbufControl.md), or [`M_GREEN`](../../Reference/buf/MbufControl.md) to your control type (for a YUV buffer, add [`M_Y`](../../Reference/buf/MbufControl.md), [`M_U`](../../Reference/buf/MbufControl.md), or [`M_V`](../../Reference/buf/MbufControl.md)). Using the control type alone or with [`M_ALL_BANDS`](../../Reference/buf/MbufControl.md) will control all bands.

## Application-specific markers

During a compression, Aurora Imaging Library adds some application-specific markers to the resulting image. Most other packages will ignore these markers and therefore be able to decompress the file. Aurora Imaging Library itself ignores unrecognized markers when it decompresses files.
