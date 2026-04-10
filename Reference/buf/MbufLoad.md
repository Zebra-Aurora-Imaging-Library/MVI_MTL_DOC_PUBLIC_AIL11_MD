---
doctype: Reference
module: buf
function: MbufLoad
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufLoad"
---

# MbufLoad

> Load data from a file into a data buffer or container.

## Syntax

```c
void MbufLoad(
    AIL_CONST_TEXT_PTR FileName,         //in
    AIL_ID             ContainerOrBufId  //out
)
```

## Description

This function loads data from a file into a previously allocated data buffer or container. The function detects the file format from the data.

You can perform the same operation as [`MbufLoad`](../../Reference/buf/MbufLoad.md) using [`MbufImport`](../../Reference/buf/MbufImport.md), which uses the specified file format to open the file instead of trying to determine the format from the data.

When loading an image file, all the pixel information in the file is loaded in the buffer. Additionally, if the file was saved in [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md) file format and includes any region of interest (ROI) information, the ROI information is also loaded in the buffer. Note that if the vectorial information of an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI was input in [`M_WORLD`](../../Reference/gra/MgraControl.md) units, the ROI will be loaded as an[`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI.

When loading an image file that has been saved with an associated LUT (color palette) into a 3-band 8-bit image buffer, the LUT is automatically applied to the data to generate 3-band image data. In this case, a LUT buffer is not created and, therefore, is not associated with the 3-band 8-bit buffer. When loading an image file that has been saved with an associated LUT (color palette) into any other type of image buffer, the LUT is also imported and associated with the resulting image buffer. You can obtain the identifier of the associated LUT, using [`MbufInquire`](../../Reference/buf/MbufInquire.md). Note that the associated LUT will be automatically selected on the display ([`MdispLut`](../../Reference/disp/MdispLut.md)) if the image buffer is selected on a display and the default LUT has not been overidden by a former call to [`MdispLut`](../../Reference/disp/MdispLut.md).

When loading an image file into a container, the destination's container components are removed and a new component is allocated containing the image data from the specified source file.

When loading a container file, the destination's container components are removed and one or more components are allocated containing the component data from the specified source container file.

Using [`MbufDiskInquire`](../../Reference/buf/MbufDiskInquire.md), you can inquire about the dimensions of the data saved in a file (except for raw files, [`M_GENDC`](../../Reference/buf/MbufExport.md)files, and [`M_AIL_NATIVE`](../../Reference/buf/MbufExport.md) files that store a container) without loading it.

### System specific

| Board(s) | Note |
|---|---|
| Product-specific | When loading compressed data and the destination buffer has an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) attribute (but not an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) attribute), this function will automatically decompress it. If necessary, the data in the file will be transformed to the format of the buffer. When loading uncompressed data into a buffer with an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) attribute, this function will automatically compress it, according to the compression settings found in the buffer. |

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to load the data. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file, for example, _"C:\mydirectory\myfile"_. Typically, data buffers have a MIM file extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `"ftp://user:password@server/pathtofile"` | Load data from a remote File Transfer Protocol (FTP) server into a data buffer.

To access a remote FTP server anonymously, omit the user name and password.

> **Note:** FTP-SSL (FTPS) is not supported. |
| `"http://user:password@server/pathtofile"` | Load data from a remote HTTP server into a data buffer.

Import data from a remote HTTP server into a data buffer.

HTTPS is not supported. |
| `"sftp://user:password@server/pathtofile"` | Load data from a remote SSH File Transfer Protocol (SFTP) server into a data buffer.

> **Note:** FTP-SSL (FTPS) is not supported. |

### `ContainerOrBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer or container. For the data buffer, if the data is deeper than the buffer, the most-significant bits of the data are truncated when loaded into the buffer. If the buffer depth is greater than that of the data, the data is zero or sign-extended (depending on the data type) when loaded into the buffer. If the buffer is larger in size than the data, exceeding areas of the buffer are unaffected. If the buffer is smaller in size than the data, the data will be cropped. For containers, the component's size will always reflect what is in the file.

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)+[`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.
