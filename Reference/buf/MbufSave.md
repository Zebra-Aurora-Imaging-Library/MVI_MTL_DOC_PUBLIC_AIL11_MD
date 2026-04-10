---
doctype: Reference
module: buf
function: MbufSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufSave"
---

# MbufSave

> Save a data buffer or container in a file, using an Aurora Imaging Library output file format.

## Syntax

```c
void MbufSave(
    AIL_CONST_TEXT_PTR FileName,         //in
    AIL_ID             ContainerOrBufId  //in
)
```

## Description

This function saves a previously allocated data buffer or container to a file, in an Aurora Imaging Library file format.

If a container is specified, it is saved in the Aurora Imaging Library native file format. This format stores all data and settings of the container and its components.

If a buffer is specified, it is saved in the Aurora Imaging Library TIFF file format. This is a baseline TIFF 6.0 file format with extra information included in the comment field. The extra information includes the buffer attributes and data type.

The Aurora Imaging Library TIFF file format supports images that are binary, grayscale, and RGB, as well as palette-color images. Images in a different format are internally converted before being saved. In addition, by default, most color image buffers are saved in a packed (chunky) format (in accordance with baseline TIFF 6.0 specifications). Color binary buffers are saved in a 1-bit per pixel format (data is stored in a 3-band, packed binary format).

When saving an image buffer ([`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md)), all the pixel information is stored in the file. In addition, if the image buffer includes any region of interest (ROI) information, set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), the ROI information is also stored in the file.

When saving an image buffer ([`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md)) that has an associated LUT buffer (color palette), the content of the LUT is also saved with the image.

Note, you can perform the same operation as [`MbufSave`](../../Reference/buf/MbufSave.md), using [`MbufExport`](../../Reference/buf/MbufExport.md) with its [`FileFormat`](../../Reference/buf/MbufExport.md) parameter set to [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md) or [`M_AIL_NATIVE`](../../Reference/buf/MbufExport.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the file. It is recommended that you use the MIM file extension for buffers, and the MBUFC file extension for containers. The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, data buffer files can contain any of the file extensions listed in MbufImport.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `"ftp://user:password@server/pathtofile"` | Save a data buffer to a file on a remote File Transfer Protocol (FTP) server.

To access a remote FTP server anonymously, omit the user name and password.

> **Note:** FTP-SSL (FTPS) is not supported.

> **Note:** Note that GenDC, STL, and PLY files cannot be saved using an FTP file path. |
| `"sftp://user:password@server/pathtofile"` | Save a data buffer to a file on a remote SSH File Transfer Protocol (SFTP) server.

> **Note:** FTP-SSL (FTPS) is not supported.

> **Note:** Note that GenDC, STL, and PLY files cannot be saved using an SFTP file path. |

### `ContainerOrBufId` *(in, AIL_ID)*

Specifies the identifier of the data buffer or container to save. This image buffer can have an ROI set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).
