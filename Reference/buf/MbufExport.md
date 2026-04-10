---
doctype: Reference
module: buf
function: MbufExport
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufExport"
---

# MbufExport

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

> Export a data buffer or container to a file.

## Syntax

```c
void MbufExport(
    AIL_CONST_TEXT_PTR FileName,            //in
    AIL_INT64          FileFormat,          //in
    AIL_ID             SrcContainerOrBufId  //in
)
```

## Description

This function exports a data buffer or container to a file, in the specified output file format.

When exporting an image buffer, all the pixel information in the buffer is exported to the file. To export an image with a LUT (color palette), associate the LUT to the image, using [`MbufControl`](../../Reference/buf/MbufControl.md). Upon export, the image is saved with its associated color palette (MIM, TIFF, PNG, and BMP file formats).

When exporting an image buffer in the [`M_AIL_TIFF`](../../Reference/buf/MbufImport.md)file format, you can save the camera calibration information associated with the buffer (if applicable) using [`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md). Additionally, if the image buffer includes any region of interest (ROI) information, set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), the ROI information is also exported to the file (unless you specify[`M_NO_REGION`](../../Reference/buf/MbufExport.md)).

You can also save a buffer in the[`M_AIL_TIFF`](../../Reference/buf/MbufExport.md) file format, using [`MbufSave`](../../Reference/buf/MbufSave.md).

You can export a buffer or container with all of its data, attributes, and settings (excluding process-dependent settings, such as Aurora Imaging Library identifiers) using the [`M_AIL_NATIVE`](../../Reference/buf/MbufImport.md)file format. You can restore the buffer or container exactly as it was exported using[`MbufImport`](../../Reference/buf/MbufImport.md).

### System specific

| Board(s) | Note |
|---|---|
| Product-specific | When exporting uncompressed data to a file in an [`M_JPEG_...`](../../Reference/buf/MbufExport.md) or [`M_JPEG2000_...`](../../Reference/buf/MbufExport.md) file format, this function will automatically compress the data, according to the specified file format. The buffer does not need an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) attribute. If you are exporting compressed data to a file in an uncompressed file format, this function will automatically decompress the data. |

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the data buffer or container. It is recommended that you use the appropriate file extension for the specified format (for example, files in a JPEG format should usually be stored with the jpg file extension). The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_).

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `"ftp://user:password@server/pathtofile"` | Export a data buffer to a file on a remote File Transfer Protocol (FTP) server.

To access a remote FTP server anonymously, omit the user name and password.

> **Note:** FTP-SSL (FTPS) is not supported.

> **Note:** Note that GenDC, STL, and PLY files cannot be exported using an FTP file path. |
| `"sftp://user:password@server/pathtofile"` | Export a data buffer to a file on a remote SSH File Transfer Protocol (SFTP) server.

> **Note:** FTP-SSL (FTPS) is not supported.

> **Note:** Note that GenDC, STL, and PLY files cannot be exported using an SFTP file path. |

### `FileFormat` *(in, AIL_INT64)*

Specifies the file format. This parameter can be set to one of the following values.

*For determining the file format automatically*

| Value | Description |
| --- | --- |
| `M_USE_EXTENSION` | Specifies that the buffer or container is saved in a format automatically determined from the file extension specified in the [`FileName`](../../Reference/buf/MbufExport.md) parameter. An error is generated if the automatically determined file format cannot be used to store the specified buffer or container.

If the file extension specifies a jpeg or jpeg 2000 format, the buffer is saved as though it were exported using[`M_JPEG_LOSSY`](../../Reference/buf/MbufExport.md) and [`M_JPEG2000_LOSSY`](../../Reference/buf/MbufExport.md) respectively (except if the file extension is jp2, in which case the buffer is saved as though it were exported using[`M_JPEG2000_LOSSY_JP2`](../../Reference/buf/MbufExport.md)).

If the file extension specifies a stl or ply format, the buffer is saved as though it were exported using [`M_STL_BINARY`](../../Reference/buf/MbufExport.md)and[`M_PLY_BINARY_LITTLE_ENDIAN`](../../Reference/buf/MbufExport.md) respectively. |

*For specifying the file format that can be used with either a buffer or a container*

| Value | Description |
| --- | --- |
| `M_AIL_NATIVE` | Specifies that the buffer or container is saved in the Aurora Imaging Library native file format. This format stores all contents of the buffer or container, including all attributes and settings (excluding process-dependent settings, such as Aurora Imaging Library identifiers). The recommended file extension when exporting a buffer is mbuf. The recommended file extension when exporting a container is mbufc.

> **Note:** Note that the Aurora Imaging Library native file format is Aurora Imaging Library-specific. You can use the [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md)file format to export buffers to a file that complies with the TIFF standard. |

*For specifying the file format that can be used with a buffer*

| Value | Description |
| --- | --- |
| `M_AIL_TIFF` | Specifies that the buffer is saved in an Aurora Imaging Library file format that uses the baseline TIFF 6.0 file format with extran Aurora Imaging Library information included in the comment field. The buffer attributes and data type are also saved in the file.

This file format supports images that are binary, grayscale, and RGB, as well as palette-color images.

The Aurora Imaging Library file format is a single-page, uncompressed TIFF file format.

The recommended file extension is mim. |
| `M_RAW` | Specifies that the buffer is saved as raw data. The contents are dumped directly (byte stream) into the file and no header is added. If the buffer is multi-band, the buffer is saved in planar format (one band after another). |

*For specifying the file format for use with a buffer that stores image data*

| Value | Description |
| --- | --- |
| `M_BMP` | Specifies that the buffer is saved in a BMP file format. The standard Windows BMP format is used.

This file format supports images that are grayscale or RGB. Images in a different format are internally converted before being saved. |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer is saved in a JPEG2000 lossless file format. If the buffer is 3-band and does not have an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)+[`M_JPEG2000_LOSSLESS`](../../Reference/buf/MbufExport.md) attribute, the data will be stored in RGB format; otherwise, it will be stored in the same color format as the buffer. |
| `M_JPEG2000_LOSSLESS_JP2` | Specifies that the buffer is saved in a JPEG2000 lossless file format using the standard JP2 format. Only the color specification of the image buffer can be specified from the additional JP2 information. If the buffer is 3-band and does not have an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)+[`M_JPEG2000_LOSSLESS`](../../Reference/buf/MbufExport.md) attribute, the data will be stored in RGB format; otherwise, it will be stored in the same color format as the buffer. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer is saved in a JPEG2000 lossy file format. If the buffer is 3-band and does not have an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)+[`M_JPEG2000_LOSSY`](../../Reference/buf/MbufExport.md) attribute, the data will be stored in YUV16 planar format if the buffer is 8-bit or in RGB format if the buffer is 16-bit; otherwise, it will be stored in the same color format as the buffer. |
| `M_JPEG2000_LOSSY_JP2` | Specifies that the buffer is saved in a JP2000 lossy file format using the standard JP2 file format. Only the color specification of the image buffer can be specified from the additional JP2 information. If the buffer is 3-band and does not have an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)+[`M_JPEG2000_LOSSY`](../../Reference/buf/MbufExport.md) attribute, the data will be stored in YUV16 planar format if the buffer is 8-bit or in RGB format if the buffer is 16-bit; otherwise, it will be stored in the same color format as the buffer. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer is saved in a JPEG lossless file format. If the buffer is 3-band, the data will be stored in RGB format. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer is saved in an interlaced JPEG lossless file format. This file format is only applicable to 1-band image buffers. |
| `M_JPEG_LOSSY` | Specifies that the buffer is saved in a JPEG lossy file format. If the buffer is 3-band and does not have an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)+[`M_JPEG_LOSSY`](../../Reference/buf/MbufExport.md) attribute, the data will be stored in YUV16 packed format; otherwise, it will be stored in the same color format as the buffer. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer is saved in an interlaced JPEG lossy file format. If the buffer is 3-band, the data will be stored in YUV16 packed format. |
| `M_JPEG_LOSSY_RGB` | Specifies that the buffer is saved in a JPEG lossy file format. If the buffer is 3-band, the data will be stored in RGB format. |
| `M_PNG` | Specifies that the buffer is saved in a PNG file format.

This file format supports only 1-, 8- and 16-bit buffers. 32-bit buffers are saved as 16-bit buffers; only the 16 least-significant bits are saved. In the case of a 32-bit buffer, it is best to shift the buffer by 16 bits to the right before exporting the image to keep the most significant data. |
| `M_TIFF` | Specifies that the buffer is saved in a TIFF file format. The TIFF file format that is used respects the baseline TIFF 6.0 specification.

The TIFF file format that is used supports images that are binary, grayscale, and RGB, as well as palette-color images. Also, this TIFF format supports any data type and depth that Aurora Imaging Library supports for buffers.

An image buffer cannot be saved using a multi-page TIFF file format; it is always saved as a single-page TIFF file. |

*For specifying the file format that can be used with a container*

| Value | Description |
| --- | --- |
| `M_GENDC` | Specifies that the container is saved in the GenDC file format.

The recommended file extension is gendc.

An error is generated if the source container has one of the following:

- A component that is not an[`M_IMAGE`](../../Reference/buf/MbufInquireContainer.md) or [`M_ARRAY`](../../Reference/buf/MbufInquireContainer.md)buffer.
- A component with[`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md) set to [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufInquireContainer.md)or [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufInquireContainer.md).
- A component with[`M_COMPONENT_GROUP_ID`](../../Reference/buf/MbufInquireContainer.md),[`M_COMPONENT_REGION_ID`](../../Reference/buf/MbufInquireContainer.md), or [`M_COMPONENT_SOURCE_ID`](../../Reference/buf/MbufInquireContainer.md) set to a value larger than 65535.
- A component that is an image buffer compressed in a format other than JPEG or JPEG 2000. |

*For specifying the file format for use with a 3D-processable container*

| Value | Description |
| --- | --- |
| `M_PLY_ASCII` | Specifies that the point cloud container is saved in the PLY file format using ASCII encoding. |
| `M_PLY_ASCII_100_SP7` | Specifies that the point cloud container is saved in the PLY file format using ASCII encoding with the header following the Aurora Imaging Library 10.0 Service Pack 7 convention. |
| `M_PLY_BINARY_LITTLE_ENDIAN` | Specifies that the point cloud container is saved in the PLY file format using binary encoding in little-endian order. |
| `M_PLY_BINARY_LITTLE_ENDIAN_100_SP7` | Specifies that the point cloud container is saved in the PLY file format using binary encoding in little-endian order with the header following the Aurora Imaging Library 10.0 Service Pack 7 convention. |
| `M_STL_ASCII` | Specifies that the point cloud container is saved in the STL file format using ASCII encoding.

> **Note:** Note that exporting to an STL file requires the presence of a valid mesh component. |
| `M_STL_ASCII_100_SP7` | Specifies that the point cloud container is saved in the STL file format using ASCII encoding with the header following the Aurora Imaging Library 10.0 Service Pack 7 convention.

> **Note:** Note that exporting to an STL file requires the presence of a valid mesh component. |
| `M_STL_BINARY` | Specifies that the point cloud container is saved in the STL file format using binary encoding.

> **Note:** Note that exporting to an STL file requires the presence of a valid mesh component. |
| `M_STL_BINARY_100_SP7` | Specifies that the point cloud container is saved in the STL file format using binary encoding with the header following the Aurora Imaging Library 10.0 Service Pack 7 convention.

> **Note:** Note that exporting to an STL file requires the presence of a valid mesh component. |

*For specifying the compression level of a PNG*

| Value | Description |
| --- | --- |
| `M_PNG_COMPRESSION_LEVELn` | Specifies a compression level of _n_, where _n_ is a value from 0 to 9.

> **Note:** The highest compression level (that is, 9) will result in the smallest file, but will take longer to save. |

*For saving camera calibration information or ROI data*

| Value | Description |
| --- | --- |
| `M_NO_REGION` | Specifies that any ROI data associated with the buffer is not saved with the buffer contents. |
| `M_WITH_CALIBRATION` | Specifies that any camera calibration information associated with the buffer is saved. To restore the associated camera calibration, use [`McalRestore`](../../Reference/cal/McalRestore.md). |

*For saving a color image in a planar format*

| Value | Description |
| --- | --- |
| `M_PLANAR` | Specifies that the color image buffer is saved in a planar format. |

*For saving an image in big endian format*

| Value | Description |
| --- | --- |
| `M_BIG_ENDIAN` | Specifies that the image buffer is saved in big endian format. |

*For compressing a TIFF image file*

| Value | Description |
| --- | --- |
| `M_COMPRESSION_CCITTFAX3` | Specifies to use CCITT FAX3 compression.

> **Note:** This compression can only be used with monochrome 1-bit buffers. |
| `M_COMPRESSION_CCITTFAX4` | Specifies to use CCITT FAX4 compression.

> **Note:** This compression can only be used with monochrome 1-bit buffers. |
| `M_COMPRESSION_CCITTRLE` | Specifies to use CCITT RLE compression.

> **Note:** This compression can only be used with monochrome 1-bit buffers. |
| `M_COMPRESSION_CCITTRLEW` | Specifies to use CCITT RLEW compression.

> **Note:** This compression can only be used with monochrome 1-bit buffers. |
| `M_COMPRESSION_LZW` | Specifies to use LZW compression. |
| `M_COMPRESSION_NONE` | Specifies to use no compression. |
| `M_COMPRESSION_PACKBITS` | Specifies to use PackBits compression. |

*For saving the organization of a point cloud*

| Value | Description |
| --- | --- |
| `M_WITH_ORGANIZATION` | Specifies to include point cloud organization information in the exported file. If you later import the file using Aurora Imaging Library (for example, using [`MbufRestore`](../../Reference/buf/MbufRestore.md)), the imported point cloud will retain this organization.

> **Note:** Note that the organization information is stored using Aurora Imaging Library-specific data fields; third-party software will typically ignore the organization information.

Invalid points are not exported; they are re-added when you later import the file using Aurora Imaging Library. In this case, the values for invalid points in components other than the confidence component (such as the range and intensity components) will be undefined. |

### `SrcContainerOrBufId` *(in, AIL_ID)*

Specifies the identifier of the data buffer or container to save.

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)+[`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

This file format supports only 8-bit, or 8-bit/band if applicable, buffers. If you are saving a non 8-bit buffer in this file format, only the 8 least-significant bits are saved/band.

This file format supports only 8- and 16-bit buffers. 1-bit buffers are unpacked and saved as 8-bit buffers. 32-bit buffers are saved as 16-bit buffers; only the 16 least-significant bits are saved. In the case of a 32-bit buffer, it is best to shift the buffer by 16 bits to the right before exporting the image to keep the most significant data.

By default, most color image buffers are saved in a packed (chunky) format (in accordance with baseline TIFF 6.0 specifications). Color binary buffers are saved in a 1-bit per pixel format (data is stored in a 3-band, packed binary format).
