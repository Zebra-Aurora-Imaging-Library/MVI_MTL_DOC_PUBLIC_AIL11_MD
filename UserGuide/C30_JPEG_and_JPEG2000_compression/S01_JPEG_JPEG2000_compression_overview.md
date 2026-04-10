---
doctype: UserGuide
part: "2D related information"
chapter: JPEG_and_JPEG2000_compression
section: JPEG_JPEG2000_compression_overview
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / jpeg / JPEG JPEG2000 compression overview"
---

# JPEG and JPEG2000 compression overview

Aurora Imaging Library allows you to compress and decompress images and sequences. Compression allows you to store more images in memory than would normally be possible. In addition, compression allows images to be transferred more quickly, since it reduces the amount of data that must be transferred. Aurora Imaging Library supports both lossy and lossless JPEG and JPEG2000 compression algorithms.

> **Note:** Note that during development and at runtime, compression support is reliant upon the presence of a compression/decompression package license. This license is only included by default with the development dongle for the full version of Aurora Imaging Library. In other cases, this license must be purchased separately.

Although processing operations support compressed source and/or destination image buffers, these operations take longer on a compressed image than on an uncompressed image. This is because the processing operation must decompress/compress the image before/after performing its operation. It is recommended that compressed images be used only for data transfer.

## JPEG lossless

The JPEG lossless algorithm compresses images without any loss of information. Typically, the algorithm compresses images by a factor of 2:1, although a factor of 4:1 can sometimes be achieved. The JPEG lossless algorithm can compress 8- or 16-bit buffers with 1 or 3 bands.

## JPEG lossy

The JPEG lossy algorithm compresses images by a variable factor but introduces some loss of information. The higher the compression factor, the more the compression, but the lower the image quality. The JPEG lossy algorithm can compress 8-bit buffers with 1 or 3 bands. To be compatible with most image-viewing software, Aurora Imaging Library allows you to store compressed color images in YUV format.

## Interlaced JPEG

Aurora Imaging Library can perform a JPEG compression such that the image data is stored in separate fields. This is referred to as an interlaced JPEG compression. Unless otherwise stated, everything that applies to a JPEG compression also applies to an interlaced JPEG compression.

## JPEG2000 lossless and lossy

JPEG2000 is another standard for image compression supported by Aurora Imaging Library. When compared with regular JPEG lossy compression at the same compression ratio, JPEG2000 lossy compression provides better image quality. JPEG2000 lossless compression permits a smaller buffer size while retaining the same image quality as regular JPEG lossless compression. The JPEG2000 lossy and lossless algorithms can compress 8- or 16-bit buffers with 1 or 3 bands (RGB or YUV). For a more detailed description of supported features, see [JPEG2000](S04_JPEG2000.md). Note, however, that JPEG2000 is best suited for archiving purposes because of the processing time required to compress and decompress images; therefore, when JPEG2000 is used for grabbing, there is a risk of missing frames.

### JP2 standard

In addition to saving files in the regular JPEG2000 lossless or lossy file format, it is also possible to save files in the JPEG2000 lossless format or lossy format using the JP2 standard. While the JPEG2000 file format is only a dump of the compressed image's data, the JP2 standard file format also contains an additional information header. Typically, this optional information contains intellectual property rights, color palette, color space definition, or application-specific data. The headers of JP2 files saved using Aurora Imaging Library only include color specifications. If the image was previously loaded into an Aurora Imaging Library buffer from a JP2 file, any information in the file header other than color specification will be lost.

Aurora Imaging Library adds the JP2 header when the buffer is exported to a file.

## Control options

Aurora Imaging Library allows you to control certain aspects of a compression. For example, you can use your own compression tables, although the default tables are suitable for most applications.
