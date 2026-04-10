---
doctype: Reference
module: buf
function: MbufRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufRestore"
---

# MbufRestore

> Restore data from disk into an automatically allocated data buffer or container.

## Syntax

```c
AIL_ID MbufRestore(
    AIL_CONST_TEXT_PTR FileName,            //in
    AIL_ID             SystemId,            //in
    AIL_ID *           ContainerOrBufIdPtr  //out
)
```

## Description

This function restores the data that was previously saved to a file and loads it into an automatically allocated buffer or container. It tries to detect the file format from the data.

This function will allocate the destination buffer or container with the same attributes as the original buffer or container, with the exception of an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) buffer.

In the case of an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) buffer, the [`MbufRestore`](../../Reference/buf/MbufRestore.md) function tries to allocate the buffer so that it can be used for acquisition ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)), display ([`M_DISP`](../../Reference/buf/MbufAllocColor.md)), and processing ([`M_PROC`](../../Reference/buf/MbufAllocColor.md)) operations. If the buffer cannot be allocated with the [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute, Aurora Imaging Library allocates one that can be used in all of the above operations except for acquisition ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)).

When restoring a container, this function allocates a container that can be used for display ([`M_DISP`](../../Reference/buf/MbufAllocColor.md)) and processing ([`M_PROC`](../../Reference/buf/MbufAllocColor.md)) operations.

If restoring an image, all the pixel information in the file is restored and loaded into the buffer. Additionally, if the file was saved in [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md) file format and includes any region of interest (ROI) information, the ROI information is also restored and loaded into the buffer.

> **Note:** If the vectorial information of an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI was input in [`M_WORLD`](../../Reference/gra/MgraControl.md) units, the ROI will be restored as an[`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI.

When restoring an image file that was saved with an associated LUT (color palette), the LUT is also restored and associated with the restored image buffer. You can obtain the identifier of the associated LUT, using [`MbufInquire`](../../Reference/buf/MbufInquire.md).

Note, you can perform the same operation as [`MbufRestore`](../../Reference/buf/MbufRestore.md)using [`MbufImport`](../../Reference/buf/MbufImport.md), which uses the specified file format to restore the data instead of trying to determine the format from the data.

After restoring the buffer or container, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the buffer or container identifier returned is not [`M_NULL`](../../Reference/buf/MbufRestore.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufRestore.md) was specified).

When the restored buffer or container is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufRestore.md) was specified during restoration; if [`M_UNIQUE_ID`](../../Reference/buf/MbufRestore.md) was specified, the smart identifier manages the buffer or container's lifetime and you must not manually free it.

### System specific

| Board(s) | Note |
|---|---|
| Product-specific | When restoring compressed data, the buffer will have an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) attribute. |

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the data buffer or container. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example _"C:\mydirectory\myfile"_). Typically, data buffer files can contain any of the file extensions listed in [`MbufImport`](../../Reference/buf/MbufImport.md).

To specify the file on the remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `"ftp://user:password@server/pathtofile"` | Restore data from a remote File Transfer Protocol (FTP) server into an automatically allocated data buffer.

To access a remote FTP server anonymously, omit the user name and password.

> **Note:** FTP-SSL (FTPS) is not supported.

> **Note:** Note that GenDC, STL, and PLY files cannot be restored using an FTP file path. |
| `"http://user:password@server/pathtofile"` | Restore data from a remote HTTP server into an automatically allocated data buffer.

HTTPS is not supported. |
| `"sftp://user:password@server/pathtofile"` | Restore data from a remote SSH File Transfer Protocol (SFTP) server into an automatically allocated data buffer.

> **Note:** FTP-SSL (FTPS) is not supported.

> **Note:** Note that GenDC, STL, and PLY files cannot be restored using an SFTP file path. |

### `SystemId` *(in, AIL_ID)*

Specifies the system on which to allocate the buffer or container.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContainerOrBufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the buffer or container identifier or specifies the data type that the function should use to return the buffer or container identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer or container ; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer or container ; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the buffer or container  (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored buffer.

If restoration fails, [`M_NULL`](../../Reference/buf/MbufRestore.md) is written as the identifier. |
| `Address in which to write the container identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored container.

If restoration fails, [`M_NULL`](../../Reference/buf/MbufRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the buffer or container identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufRestore.md) was specified).

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)+[`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.
