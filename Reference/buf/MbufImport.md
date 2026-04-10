---
doctype: Reference
module: buf
function: MbufImport
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufImport"
---

# MbufImport

> Import data from a file into a data buffer or container.

## Syntax

```c
AIL_ID MbufImport(
    AIL_CONST_TEXT_PTR FileName,            //in
    AIL_INT64          FileFormat,          //in
    AIL_INT64          Operation,           //in
    AIL_ID             SysId,               //in
    AIL_ID *           ContainerOrBufIdPtr  //in-out
)
```

## Description

This function imports data from a file into an existing or automatically allocated Aurora Imaging Library data buffer or container. The function assumes that the data in the file is in the specified format.

Note, you can also import data using [`MbufLoad`](../../Reference/buf/MbufLoad.md) or [`MbufRestore`](../../Reference/buf/MbufRestore.md); however, these functions try to determine the format from the data rather than allowing you to specify the format.

For an [`M_RESTORE`](../../Reference/buf/MbufImport.md) operation of an image, this function tries to allocate the buffer so that it can be used for acquisition ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)), display ([`M_DISP`](../../Reference/buf/MbufAllocColor.md)), and processing ([`M_PROC`](../../Reference/buf/MbufAllocColor.md)) operations. If the buffer cannot be allocated with the [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute, Aurora Imaging Library allocates one that can be used in all of the above operations except for acquisition ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)). You can specify that the restored buffer is not allocated with the acquisition or compression attributes ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md) or [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) respectively), by combining [`M_RESTORE`](../../Reference/buf/MbufImport.md) with [`M_NO_GRAB`](../../Reference/buf/MbufImport.md) or [`M_NO_COMPRESS`](../../Reference/buf/MbufImport.md) respectively.

For an [`M_RESTORE`](../../Reference/buf/MbufImport.md) operation of a container, this function allocates a container that can be used for display ([`M_DISP`](../../Reference/buf/MbufAllocColor.md)) and processing ([`M_PROC`](../../Reference/buf/MbufAllocColor.md)) operations.

> **Note:** For an [`M_RESTORE`](../../Reference/buf/MbufImport.md)operation with an[`M_AIL_TIFF`](../../Reference/buf/MbufImport.md)or [`M_AIL_NATIVE`](../../Reference/buf/MbufImport.md) file format that stores a non-image buffer, this function allocates the destination buffer with the same attributes as the original buffer.

If importing a CAD file, the data must be saved in the PLY or STL file format. If the CAD file is in another format, you must convert it to the PLY or STL file format (ASCII encoding or binary encoding) before calling this function. For binary PLY file format, it must be in little-endian format.

If importing an image, all the pixel information in the file is imported and loaded into the buffer. Additionally, if the file was saved in [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md) file format and includes any region of interest (ROI) information, the ROI information is also imported and loaded into the buffer when the [`FileFormat`](../../Reference/buf/MbufImport.md) parameter is set to [`M_AIL_TIFF`](../../Reference/buf/MbufImport.md). To load the [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md) file without its ROI information, set [`FileFormat`](../../Reference/buf/MbufExport.md) to [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md)+[`M_NO_REGION`](../../Reference/buf/MbufExport.md). If the file was saved with calibration information that provides a constant pixel size, the calibration information can be loaded and associated with the imported image with [`M_WITH_CALIBRATION`](../../Reference/buf/MbufImport.md). For more information see [Camera calibration propagation](../../UserGuide/C28_Calibration/S08_Calibration_propagation.md).

> **Note:** If the vectorial information of an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI was input in [`M_WORLD`](../../Reference/gra/MgraControl.md) units, the ROI will be imported as an[`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI.

When importing an image file (not stored in the [`M_AIL_NATIVE`](../../Reference/buf/MbufImport.md) file format) that has been saved with an associated LUT (color palette) into a 3-band 8-bit image buffer, the LUT is automatically applied to the data to generate 3-band image data. In this case, a LUT buffer is not created and, therefore, is not associated with the 3-band 8-bit buffer. When importing an image file that has been saved with an associated LUT (color palette) into any other type of image buffer, the LUT is also imported and associated with the resulting image buffer. You can obtain the identifier of the associated LUT, using [`MbufInquire`](../../Reference/buf/MbufInquire.md).

Note that the associated LUT will be automatically selected on the display ([`MdispLut`](../../Reference/disp/MdispLut.md)) if the image buffer is selected on a display and the default LUT has not been overridden by a former call to [`MdispLut`](../../Reference/disp/MdispLut.md).

Using [`MbufDiskInquire`](../../Reference/buf/MbufDiskInquire.md), you can inquire about the dimensions of the data saved in a file (except for raw files, [`M_GENDC`](../../Reference/buf/MbufImport.md)files, and [`M_AIL_NATIVE`](../../Reference/buf/MbufImport.md) files that store a container) without importing it.

After importing or restoring a buffer or container, it is recommended that you check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md), or by verifying that the returned buffer or container identifier is not [`M_NULL`](../../Reference/buf/MbufImport.md).

When the restored buffer or container is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufImport.md) was specified during restoration; if [`M_UNIQUE_ID`](../../Reference/buf/MbufImport.md) was specified, the smart identifier manages the buffer or container's lifetime and you must not manually free it.

### System specific

| Board(s) | Note |
|---|---|
| Product-specific | When performing an [`M_RESTORE`](../../Reference/buf/MbufImport.md) operation on a file containing compressed data, the buffer will have an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) attribute. |
| Product-specific | When importing compressed data, the following applies. When performing an [`M_LOAD`](../../Reference/buf/MbufImport.md) operation and the destination buffer has an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) attribute (but not an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) attribute), this function will automatically decompress it. If necessary, the data in the file will be transformed to the format of the buffer. If unsure of the compression type of the data, use [`M_DEFAULT`](../../Reference/buf/MbufImport.md) as the file format rather than [`M_JPEG_...`](../../Reference/buf/MbufImport.md) or [`M_JPEG2000_...`](../../Reference/buf/MbufImport.md); the data will be read correctly. |
| Product-specific | When importing uncompressed data into a buffer with an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) attribute, this function will automatically compress it, according to the compression settings found in the buffer. |

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to import the data. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file, for example, _"C:\mydirectory\myfile"_.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `"ftp://user:password@server/pathtofile"` | Import data from a remote File Transfer Protocol (FTP) server into a data buffer or container.

To access a remote FTP server anonymously, omit the user name and password.

> **Note:** FTP-SSL (FTPS) is not supported.

> **Note:** Note that GenDC, STL, and PLY files cannot be imported using an FTP file path. |
| `"http://user:password@server/pathtofile"` | Import data from a remote Hypertext Transfer Protocol (HTTP) server into a data buffer or container.

HTTPS is not supported.

> **Note:** Note that importing PLY and STL files from a remote HTTP server is not supported. |
| `"sftp://user:password@server/pathtofile"` | Import data from a remote SSH File Transfer Protocol (SFTP) server into a data buffer or container.

> **Note:** FTP-SSL (FTPS) is not supported.

> **Note:** Note that GenDC, STL, and PLY files cannot be imported using an SFTP file path. |

### `FileFormat` *(in, AIL_INT64)*

Specifies the file format. This parameter can be set to one of the following values. Note that except for the [`M_AIL_NATIVE`](../../Reference/buf/MbufImport.md), [`M_AIL_TIFF`](../../Reference/buf/MbufImport.md),[`M_GENDC`](../../Reference/buf/MbufImport.md) and [`M_RAW`](../../Reference/buf/MbufImport.md) file formats, data is treated as image data. When performing an [`M_LOAD`](../../Reference/buf/MbufImport.md) operation, the data is internally converted appropriately.

*For specifying a file format that might need to be imported to a buffer or container*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library automatically determines the file format. If the file format is not supported, its data will be treated as raw data. |
| `M_AIL_NATIVE` | Imports a container or buffer saved in the Aurora Imaging Library native file format (typically with the mbuf or mbufc file extension). This format stores the entire content of an Aurora Imaging Library container or Aurora Imaging Library buffer, including settings. |

*For specifying a file format that must be imported to a buffer*

| Value | Description |
| --- | --- |
| `M_AIL_TIFF` | Imports data that is in the [`M_AIL_TIFF`](../../Reference/buf/MbufImport.md) file format (typically with the mim file extension). The baseline TIFF 6.0 specification is used. |
| `M_BMP` | Imports image data that is in BMP file format. The standard Windows BMP format is used.

If the specified BMP file contains an image whose image format, data type, and/or depth is not supported by Aurora Imaging Library for image buffers, the image will not be imported. |
| `M_JPEG2000_LOSSLESS` | Imports a JPEG2000 lossless image. |
| `M_JPEG2000_LOSSLESS_JP2` | Imports a JPEG2000 lossless image, as well as the additional JP2 information. |
| `M_JPEG2000_LOSSY` | Imports a JPEG2000 lossy image. |
| `M_JPEG2000_LOSSY_JP2` | Imports a JPEG2000 lossy image, as well as the additional JP2 information. |
| `M_JPEG_LOSSLESS` | Imports a JPEG lossless image. |
| `M_JPEG_LOSSLESS_INTERLACED` | Imports a JPEG lossless image stored in two separate fields. |
| `M_JPEG_LOSSY` | Imports a JPEG lossy image. |
| `M_JPEG_LOSSY_INTERLACED` | Imports a JPEG lossy image stored in two separate fields. |
| `M_JPEG_LOSSY_RGB` | Imports a 3-band JPEG lossy image that is in an RGB format. |
| `M_PNG` | Imports image data that is in a PNG file format. |
| `M_RAW` | Imports raw data. |
| `M_TIFF` | Imports image data that is in a TIFF file format. The baseline TIFF 6.0 specification is used.

If the specified TIFF file contains an image whose image format, data type, and/or depth is not supported by Aurora Imaging Library for image buffers, the image will not be imported.

Multi-page TIFF files can be imported into Aurora Imaging Library buffers but only the first page is saved in the buffer. |

*For specifying a file format that must be imported to a container*

| Value | Description |
| --- | --- |
| `M_GENDC` | Imports a container saved in the GenDC file format. |
| `M_PLY_ASCII` | Imports point cloud data saved in the PLY file format using ASCII encoding.

> **Note:** ail3d&lt;version number>.dll must be present to import this format. |
| `M_PLY_BINARY_LITTLE_ENDIAN` | Imports point cloud data saved in the PLY file format using binary encoding in little-endian order.

> **Note:** ail3d&lt;version number>.dll must be present to import this format. |
| `M_STL_ASCII` | Imports point cloud data, saved in the STL file format using ASCII encoding. STL files always include mesh data.

> **Note:** ail3d&lt;version number>.dll must be present to import this format. |
| `M_STL_BINARY` | Imports point cloud data, saved in the STL file format using binary encoding. STL files always include mesh data.

> **Note:** ail3d&lt;version number>.dll must be present to import this format. |

*For importing camera calibration or ROI information*

| Value | Description |
| --- | --- |
| `M_NO_REGION` | Imports the image, excluding any saved ROI information. |
| `M_WITH_CALIBRATION` | Imports the image, including its camera calibration information. Note that, this image must have been saved using [`MbufExport`](../../Reference/buf/MbufExport.md) and it must have a constant pixel size. If the image does not have a constant pixel size, the image will be imported without its camera calibration information; the target buffer will be uncalibrated. If the image is calibrated and it has a constant pixel size, it is associated with a default uniform camera calibration context upon being imported into the buffer (as set using [`McalUniform`](../../Reference/cal/McalUniform.md)). If the image is imported into an existing buffer that is associated with a camera calibration context, the initial camera calibration context is replaced by that of the imported image. |

### `Operation` *(in, AIL_INT64)*

Specifies the import operation. This parameter can be set to one of the following values.

*For specifying the import operation*

| Value | Description |
| --- | --- |
| `M_LOAD` | Imports data from the specified file into a previously allocated Aurora Imaging Library data buffer or container. If the destination is a container, all of its existing components will be freed.

By default, the contents of the file are loaded, including all data and settings. Optional attributes (such as [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)) of data stored in the file are not copied. You can use [`M_IDENTICAL`](../../Reference/buf/MbufImport.md)to specify that the buffer size and optional attributes of the specified file must be loaded from the file.

When loading a file to a buffer, the attributes of the destination buffer are used when loading the data (for example, if the destination does not have the [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)attribute, the file will be decompressed during the load operation).When loading a file to a container, components in the file are loaded to components allocated without optional attributes (for example, components that store compressed data are loaded to components allocated without the [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)attribute). |
| `M_RESTORE` | Imports data from the specified file into an automatically allocated Aurora Imaging Library data buffer or container. Note, you cannot restore ([`M_RESTORE`](../../Reference/buf/MbufImport.md)) a RAW data file ([`M_RAW`](../../Reference/buf/MbufImport.md)) because its dimensions are unknown. |

*For specifying how the buffer or container is loaded*

| Value | Description |
| --- | --- |
| `M_BASIC_ATTRIBUTE` | Specifies to load the contents of the file (that stores data suitable for loading into a container) to automatically allocated components that might have different attributes than the components stored in the file.

> **Note:** Note that this setting is only available for a container, not a buffer. |
| `M_IDENTICAL` | Specifies to load the contents of the file, including all data, settings, and attributes. This setting is only available for [`M_AIL_NATIVE`](../../Reference/buf/MbufImport.md)and [`M_GENDC`](../../Reference/buf/MbufImport.md)files.

An error will be generated if there is a mismatch between the size, number of bands, or attributes (such as[`M_RGB24`](../../Reference/buf/MbufAllocColor.md)) of the destination buffer and the contents of the file. |
| `M_USE_DESTINATION` | Specifies to load the contents of the file (that stores data suitable for loading into a container) into the existing components of the destination container. An error will be generated if the file has a different number of components than the destination container, or if at least one of the components in the file cannot be loaded into the component with the same index in the container (for example, because the components of the destination container are of the wrong buffer type to store the imported data).

> **Note:** Note that this setting is only available for a container, not a buffer. |

*For specifying that the buffer is not restored with a certain attribute*

| Value | Description |
| --- | --- |
| `M_NO_COMPRESS` | Specifies that Aurora Imaging Library will not allocate the restored data buffer with an [`M_COMPRESS`](../../Reference/buf/MbufAlloc2d.md) attribute. |
| `M_NO_GRAB` | Specifies that Aurora Imaging Library will not allocate the restored data buffer with an [`M_GRAB`](../../Reference/buf/MbufAlloc2d.md) attribute. |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the buffer or container for an [`M_RESTORE`](../../Reference/buf/MbufImport.md) operation. For an [`M_LOAD`](../../Reference/buf/MbufImport.md) operation, this parameter should be set to `M_NULL`.

*For specifying the system on which to allocate the buffer (with*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContainerOrBufIdPtr` *(in-out, *AIL_ID)*

Specifies the address of the variable containing the buffer or container identifier, for an [`M_LOAD`](../../Reference/buf/MbufImport.md) operation, or the address of the variable in which to store the new buffer or container identifier (or specifies the data type that the function should use to return the buffer or container identifier), for an [`M_RESTORE`](../../Reference/buf/MbufImport.md) operation.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the restored buffer or container; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the restored buffer or container; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.

> **Note:** This setting is only available when using C++11 (or later).

An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the buffer or container (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored buffer.

If restoration fails, [`M_NULL`](../../Reference/buf/MbufImport.md) is written as the identifier. |
| `Address in which to write the container identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored container.

If restoration fails, [`M_NULL`](../../Reference/buf/MbufImport.md) is written as the identifier. |
| `Address of the buffer identifier` | Specifies the address of the Aurora Imaging Library identifier of the buffer in which to load the contents of the file. |
| `Address of the container identifier` | Specifies the address of the Aurora Imaging Library identifier of the container in which to load the contents of the file. |

## Return Value

**Type:** `AIL_ID`

The returned value is the buffer or container identifier (for an [`M_RESTORE`](../../Reference/buf/MbufImport.md) operation only) either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If restoration fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufImport.md) was specified).

## Remarks

> Note that during development and at runtime, compression support is reliant upon the presence of a compression/decompression package license. This license is only included by default with the development dongle for the full version of Aurora Imaging Library. In other cases, this license must be purchased separately.
