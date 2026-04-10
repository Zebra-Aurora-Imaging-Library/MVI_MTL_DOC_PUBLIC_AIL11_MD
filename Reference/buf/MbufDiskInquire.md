---
doctype: Reference
module: buf
function: MbufDiskInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufDiskInquire"
---

# MbufDiskInquire

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Inquire about the data in a file.

## Syntax

```c
AIL_INT MbufDiskInquire(
    AIL_CONST_TEXT_PTR FileName,     //in
    AIL_INT64          InquireType,  //in
    void *             UserVarPtr    //out
)
```

## Description

This function inquires about the data in the specified file on disk.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the file that contains the data.

*For specifying the file name or memory stream*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Note, an error occurs if the file does not have a known file format or the file format is not supported.

The supported file types include all the formats supported by the [`MbufExport`](../../Reference/buf/MbufExport.md) and [`MbufExportSequence`](../../Reference/buf/MbufExportSequence.md) functions. Since a raw data file does not have any information regarding size or type, you can only use [`MbufDiskInquire`](../../Reference/buf/MbufDiskInquire.md) to determine the file format of this type of file.

This function cannot access a file on the remote computer; the file must be located on the local computer. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of file setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MbufDiskInquire`](../../Reference/buf/MbufDiskInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about general types of file settings

The following inquire types allow you to inquire about general types of file settings.

---

### `M_BYTE_ORDER`

Inquires the file's byte order.

| Value | Description |
| --- | --- |
| `M_BIG_ENDIAN` | Specifies that the file is in big endian format. |
| `M_LITTLE_ENDIAN` | Specifies that the file is in little endian format. |

---

### `M_FILE_FORMAT`

Inquires the file format.

| Value | Description |
| --- | --- |
| `M_AVI_AIL` | Specifies an AVI format used to hold images in their Aurora Imaging Library format. |
| `M_AVI_DIB` | Specifies an AVI format used to hold non-compressed DIB images. |
| `M_AVI_MJPG` | Specifies an AVI format used to hold JPEG compressed sequences. |
| `M_AIL_NATIVE` | Specifies that the data is saved in the Aurora Imaging Library native file format. |
| `M_AIL_TIFF` | Specifies that the data is saved in an Aurora Imaging Library file format that uses the baseline TIFF 6.0 file format with extran Aurora Imaging Library information included in the comment field. |
| `M_RAW` | Specifies that the data is saved as raw data. |
| `M_BMP` | Specifies that the data is saved in a BMP file format. |
| `M_JPEG2000_LOSSLESS` | Specifies that the data is saved in a JPEG2000 lossless file format. |
| `M_JPEG2000_LOSSLESS_JP2` | Specifies that the data is saved in a JPEG2000 lossless file format using the standard JP2 format. |
| `M_JPEG2000_LOSSY` | Specifies that the data is saved in a JPEG2000 lossy file format. |
| `M_JPEG2000_LOSSY_JP2` | Specifies that the data is saved in a JP2000 lossy file format using the standard JP2 file format. |
| `M_JPEG_LOSSLESS` | Specifies that the data is saved in a JPEG lossless file format. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the data is saved in an interlaced JPEG lossless file format. |
| `M_JPEG_LOSSY` | Specifies that the data is saved in a JPEG lossy file format. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the data is saved in an interlaced JPEG lossy file format. |
| `M_JPEG_LOSSY_RGB` | Specifies that the data is saved in a JPEG lossy file format. |
| `M_PNG` | Specifies that the data is saved in a PNG file format. |
| `M_TIFF` | Specifies that the data is saved in a TIFF file format. |
| `M_PNG_COMPRESSION_LEVELn` | Specifies a compression level of _n_, where _n_ is a value from 0 to 9. |
| `M_NO_REGION` | Specifies that any ROI data associated with the data is not saved with the buffer contents. |
| `M_WITH_CALIBRATION` | Specifies that any camera calibration information associated with the data is saved. |
| `M_PLANAR` | Specifies that the color image buffer is saved in a planar format. |
| `M_BIG_ENDIAN` | Specifies that the image buffer is saved in big endian format. |
| `M_COMPRESSION_CCITTFAX3` | Specifies to use CCITT FAX3 compression. |
| `M_COMPRESSION_CCITTFAX4` | Specifies to use CCITT FAX4 compression. |
| `M_COMPRESSION_CCITTRLE` | Specifies to use CCITT RLE compression. |
| `M_COMPRESSION_CCITTRLEW` | Specifies to use CCITT RLEW compression. |
| `M_COMPRESSION_LZW` | Specifies to use LZW compression. |
| `M_COMPRESSION_NONE` | Specifies to use no compression. |
| `M_COMPRESSION_PACKBITS` | Specifies to use PackBits compression. |
| `M_GENDC` | Specifies that the data is saved in the GenDC file format. |
| `M_PLY_ASCII` | Specifies that the data is saved in the PLY file format using ASCII encoding. |
| `M_PLY_BINARY_LITTLE_ENDIAN` | Specifies that the data is saved in the PLY file format using binary encoding in little-endian order. |
| `M_STL_ASCII` | Specifies that the data is saved in the STL file format using ASCII encoding. |
| `M_STL_BINARY` | Specifies that the data is saved in the STL file format using binary encoding. |
| `M_WITH_ORGANIZATION` | Specifies to include point cloud organization information in the exported file. |
| `M_AVI_CODEC` | Specifies an AVI format used with a codec found on your computer, where the AVI is not an [`M_AVI_DIB`](../../Reference/buf/MbufExportSequence.md), [`M_AVI_AIL`](../../Reference/buf/MbufExportSequence.md) or[`M_AVI_MJPG`](../../Reference/buf/MbufExportSequence.md). |

---

### `M_OBJECT_TYPE`

Inquires the type of Aurora Imaging Library object stored in the file.  Note that AVI files return [`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md) as an object type.

| Value | Description |
| --- | --- |
| `M_ARRAY` | Specifies that the file stores an array buffer. |
| `M_CONTAINER` | Specifies that the file stores a container. |
| `M_IMAGE` | Specifies that the file stores an image buffer. |
| `M_KERNEL` | Specifies that the file stores a kernel buffer. |
| `M_LUT` | Specifies that the file stores a LUT buffer. |
| `M_STRUCT_ELEMENT` | Specifies that the file stores a struct element buffer. |

---

### `M_TYPE`

Inquires the file data type and depth. The depth is returned in bits.

| Value | Description |
| --- | --- |
| `depth value + M_FLOAT` | Specifies the data depth and that the data type is floating-point. |
| `depth value + M_SIGNED` | Specifies the data depth and that the data type is signed. |
| `depth value + M_UNSIGNED` | Specifies the data depth and that the data type is unsigned. |

### For

You can inquire the following values for [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) [`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md), [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md), [`M_LUT`](../../Reference/buf/MbufAlloc2d.md) and [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) files.

---

### `M_RESOLUTION_X`

Inquires the X-resolution of the file in pixels per inch (PPI).

| Value | Description |
| --- | --- |
| `0` | Specifies that the file has no resolution information. |
| `Value > 0` | Specifies the X resolution in PPI. |

---

### `M_RESOLUTION_Y`

Inquires the Y-resolution of the file in pixels per inch (PPI). If the file has no resolution information, 0 is returned.

| Value | Description |
| --- | --- |
| `0` | Specifies that the file has no resolution information. |
| `Value > 0` | Specifies the Y resolution in PPI. |

---

### `M_SIGN`

Inquires whether the file's data is signed or unsigned.

| Value | Description |
| --- | --- |
| `M_SIGNED` | Specifies that the data is signed. |
| `M_UNSIGNED` | Specifies that the data is unsigned. |

---

### `M_SIZE_BAND`

Inquires the number of bands in the file.

| Value | Description |
| --- | --- |
| `1 <= Value <= 1024` | Specifies the number of bands. |

---

### `M_SIZE_BIT`

Inquires the file data depth.

| Value | Description |
| --- | --- |
| `1 <= Value <= 64` | Specifies the data depth, in bits. |

---

### `M_SIZE_X`

Inquires the width of the data in the file.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the data, in pixels. |

---

### `M_SIZE_Y`

Inquires the height of the data in the file.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the data, in pixels. |

### Combination Constants — For obtaining information about the LUT associated with the image

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the width or the number of bands of the LUT associated with the image.

This option is available only if image data was saved to the file. This option is not available for AVI files.

#### `M_LUT`

Inquires either the width or the number of bands of the LUT associated with the image in the file.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that there is no LUT associated with the image. |
| `Value` | Specifies either the width of the LUT associated with the image in the file (for [`M_SIZE_X`](../../Reference/buf/MbufDiskInquire.md)) or the number of bands of the LUT associated with the image in the file (for [`M_SIZE_BAND`](../../Reference/buf/MbufDiskInquire.md)). |

### For

You can inquire the following values for [`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md) files (images or AVI sequences).

---

### `M_ASPECT_RATIO`

Inquires the aspect ratio of the images in the file.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the aspect ratio. |

---

### `M_CALIBRATION_PRESENT`

Inquires whether the calibration information associated with the buffer was saved, using [`MbufExport`](../../Reference/buf/MbufExport.md) with [`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the calibration was not saved. |
| `M_TRUE` | Specifies that the calibration was saved. |

---

### `M_COMPRESSION_TYPE`

Inquires the compression type of the images in the file.

| Value | Description |
| --- | --- |
| `M_JPEG2000_LOSSLESS` | Specifies that the file holds JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the file holds JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the file holds JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the file holds JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the file holds JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the file holds JPEG lossy data in separate fields. |
| `M_JPEG2000_LOSSLESS` | Specifies that the file holds JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the file holds JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the file holds JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the file holds JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the file holds JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the file holds JPEG lossy data in separate fields. |
| `M_COMPRESSION_CCITTFAX3` | Specifies that the file holds TIFF CCITT FAX3 data. |
| `M_COMPRESSION_CCITTFAX4` | Specifies that the file holds TIFF CCITT FAX4 data. |
| `M_COMPRESSION_CCITTRLE` | Specifies that the file holds TIFF CCITT RLE data. |
| `M_COMPRESSION_CCITTRLEW` | Specifies that the file holds TIFF CCITT RLEW data. |
| `M_COMPRESSION_LZW` | Specifies that the file holds TIFF LZW data. |
| `M_COMPRESSION_PACKBITS` | Specifies that the file holds TIFF PackBits data. |
| `M_NULL` | Specifies that the images are not compressed. |

---

### `M_FRAME_RATE`

Inquires the frame rate of an AVI file.  This value is only valid for sequences.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the frame rate, in number of images/second. |

---

### `M_LUT_PRESENT`

Inquires the presence of LUT data in the file.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that LUT data is not available. |
| `M_YES` | Specifies that LUT data is available. |

---

### `M_NUMBER_OF_IMAGES`

Inquires the number of images in an AVI file.  This value is only valid for sequences.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of images. |

### For

You can inquire the following values for [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md) and [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) files.

---

### `M_OFFSET_CENTER_X`

Inquires the X-coordinate of the center of the kernel or structuring element.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate. |

---

### `M_OFFSET_CENTER_Y`

Inquires the Y-coordinate of the center of the kernel or structuring element.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate. |

---

### `M_OVERSCAN`

Inquires the overscan type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that Aurora Imaging Library automatically selects the type of overscan to optimize speed and logic according to the specified operation and the target system. |
| `M_DISABLE` | Specifies that no overscan is used, unless processing the border pixels is faster than ignoring them; in the latter case, Aurora Imaging Library automatically selects the overscan to optimize speed according to the specified operation and the target system. |
| `M_FAST` | Specifies that Aurora Imaging Library automatically selects the overscan to optimize speed according to the specified neighborhood operation and the target system. |
| `M_MIRROR` | Specifies a type of overscan that processes the border pixels of a source image using overscan pixel values that mirror the source buffer pixel values. |
| `M_REPLACE` | Specifies a type of overscan that processes the border pixels of a source image using overscan pixel values set to the overscan replacement value ([`M_OVERSCAN_REPLACE_VALUE`](../../Reference/buf/MbufControl.md)). |
| `M_REPLICATE` | Specifies a type of overscan that processes the border pixels of a source image using overscan pixel values that replicate the border pixels. |
| `M_TRANSPARENT` | Specifies a type of overscan that processes the border pixels of a source image using transparent overscan pixel values. |

---

### `M_OVERSCAN_REPLACE_VALUE`

Inquires the replacement value for the overscan pixel values.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REPLACE_MAX` | Specifies that the overscan neighborhood pixel values will be set to the maximum value of the source buffer. |
| `M_REPLACE_MIN` | Specifies that the overscan neighborhood pixel values will be set to the minimum value of the source buffer. |
| `Value` *(default)* | Specifies the value of the overscan neighborhood pixels. |

### For

You can inquire the following values for [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md) files.

---

### `M_ABSOLUTE_VALUE`

Inquires the absolute value setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to take the absolute value of the result. |
| `M_ENABLE` | Specifies to take the absolute value of the result. |

---

### `M_NORMALIZATION_FACTOR`

Inquires the normalization factor.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the normalization factor. |

---

### `M_SATURATION`

Inquires the saturation flag.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to saturate results, except when Aurora Imaging Library can take advantage of optimization routines to accelerate the processing. |
| `M_ENABLE` | Specifies to saturate results. |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information. If the requested information is not available, `M_INVALID` is returned. If an AVI file is specified but its codec is not known, `M_AVI_CODEC_UNSUPPORTED` is returned.

## Remarks

> Note that Aurora Imaging Library Lite does not support the inquiries available for [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md) and [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) data buffers.

> Note that during development and at runtime, compression support requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.
