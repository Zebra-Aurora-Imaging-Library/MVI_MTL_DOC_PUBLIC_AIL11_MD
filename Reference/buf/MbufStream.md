---
doctype: Reference
module: buf
function: MbufStream
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufStream"
---

# MbufStream

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

> Load, restore, or save a buffer or container from/to a file or memory stream.

## Syntax

```c
void MbufStream(
    AIL_TEXT_PTR MemPtrOrFileName,     //in-out
    AIL_ID       SystemId,             //in
    AIL_INT64    Operation,            //in
    AIL_INT64    StreamType,           //in
    AIL_DOUBLE   Version,              //in
    AIL_INT64    ControlFlag,          //in
    AIL_ID *     ContainerOrBufIdPtr,  //in-out
    AIL_INT *    SizeByteVarPtr        //in-out
)
```

## Description

This function can load, restore, or save a buffer or container from/to a file or memory stream.

To inquire the number of bytes necessary to save a buffer or container, you should first call this function ([`MbufStream`](../../Reference/buf/MbufStream.md)) with [`M_INQUIRE_SIZE_BYTE`](../../Reference/buf/MbufStream.md).

The content saved to memory stream is equivalent to the content saved to file.

Typically the stream is saved in the Aurora Imaging Library native file format. Optionally, you can save a buffer to the PNG format or a container to a GenDC-compatible format by setting the [`ControlFlag`](../../Reference/buf/MbufStream.md) parameter to [`M_PNG`](../../Reference/buf/MbufStream.md) or [`M_GENDC`](../../Reference/buf/MbufStream.md), respectively. You must specify the same format when loading or restoring a stream saved in a PNG or GenDC-compatible format. When loading or restoring a PNG from memory, you must specify the size using the [`SizeByteVarPtr`](../../Reference/buf/MbufStream.md) parameter.

Optionally, when saving a container, you can specify to losslessly compress the data in the stream by setting the[`ControlFlag`](../../Reference/buf/MbufStream.md)parameter to [`M_COMPRESS`](../../Reference/buf/MbufStream.md). This is not available when saving a container in a format that is GenDC-compatible.

You can use this and other Aurora Imaging Library stream functions, for example, to save all required Aurora Imaging Library objects, as well as any other custom data, for your application to a memory stream. Once in a memory stream, you can write the stream to a single file or transfer it over a network. You are responsible for concatenating the streams and for saving the stream to file.

For an [`M_RESTORE`](../../Reference/buf/MbufStream.md) operation of an image, this function tries to allocate the buffer so that it can be used for acquisition ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)), display ([`M_DISP`](../../Reference/buf/MbufAllocColor.md)), and processing ([`M_PROC`](../../Reference/buf/MbufAllocColor.md)) operations. If the buffer cannot be allocated with the [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute, Aurora Imaging Library allocates one that can be used in all of the above operations except for acquisition ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)). You can specify that the restored buffer is not allocated with the acquisition or compression attributes ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md) or [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) respectively), by combining [`M_RESTORE`](../../Reference/buf/MbufStream.md) with [`M_NO_GRAB`](../../Reference/buf/MbufStream.md) or [`M_NO_COMPRESS`](../../Reference/buf/MbufStream.md) respectively.

For an [`M_RESTORE`](../../Reference/buf/MbufStream.md) operation of a container, this function allocates a container that can be used for display ([`M_DISP`](../../Reference/buf/MbufAllocColor.md)) and processing ([`M_PROC`](../../Reference/buf/MbufAllocColor.md)) operations.

Using [`MbufStream`](../../Reference/buf/MbufStream.md), you can choose to save a backwards-compatible version of the buffer or container, which will work using a version of Aurora Imaging Library that is up to one major release older than the current version (depending on which version is specified). However, all settings and features unique to the higher version will be ignored when restored using the lower version. Besides saving backwards-compatible versions, you can also load or restore buffers or containers saved using any Aurora Imaging Library version with which this function was available. Settings that do not exist in the lower version will be filled with default values when the buffer or container is loaded or restored.

## Parameters

### `MemPtrOrFileName` *(in-out, AIL_TEXT_PTR)*

Specifies the file or memory stream.

### `SystemId` *(in, AIL_ID)*

Specifies the system on which to restore the buffer or container.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

### `StreamType` *(in, AIL_INT64)*

Specifies the type of stream in which to store/from which to restore the buffer or container. This parameter should be set to one of the following values:

*For the type of stream*

| Value | Description |
| --- | --- |
| `M_FILE` | Specifies a file stream.

The file is saved in the Aurora Imaging Library native file format, unless [`ControlFlag`](../../Reference/buf/MbufStream.md) is set to [`M_GENDC`](../../Reference/buf/MbufStream.md) or [`M_PNG`](../../Reference/buf/MbufStream.md); in which case, the file is saved in the specified file format. The GenDC file format is only available when saving/loading a container. The PNG file format is only available when saving/loading buffers. |
| `M_MEMORY` | Specifies a memory stream. You are responsible for allocating a block of memory for the stream.

The memory stream is saved in the Aurora Imaging Library native file format, unless [`ControlFlag`](../../Reference/buf/MbufStream.md)is set to [`M_GENDC`](../../Reference/buf/MbufStream.md) or [`M_PNG`](../../Reference/buf/MbufStream.md); in which case, the memory stream is saved in the specified file format. The GenDC file format is only available when saving/loading a container. The PNG file format is only available when saving/loading a buffer.

> **Note:** When loading or restoring a PNG from memory, you must specify the size using the[`SizeByteVarPtr`](../../Reference/buf/MbufStream.md) parameter. |

### `Version` *(in, AIL_DOUBLE)*

Specifies the Aurora Imaging Library version of the buffer or container.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the control flag for the operation. Set this parameter to `M_DEFAULT` if not used.

### `ContainerOrBufIdPtr` *(in-out, *AIL_ID)*

Specifies the address of the variable in which to write, or from which to read, the buffer or container identifier.

### `SizeByteVarPtr` *(in-out, *AIL_INT)*

Specifies the address of the variable in which to write, or from which to read, the size of the buffer or container, in bytes.

## Parameter Associations

### For performing the stream operation.

---

### `M_INQUIRE_SIZE_BYTE`

Inquires the number of bytes required to save a buffer or container to a memory stream. This operation is not supported when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to [`M_FILE`](../../Reference/buf/MbufStream.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the current version of Aurora Imaging Library. |
| `M_PROC_VERSION_100_SP4` | Specifies the version as being Aurora Imaging Library 10.0 Service Pack 4. |
| `M_PROC_VERSION_100_SP5` | Specifies the version as being Aurora Imaging Library 10.0 Service Pack 5. |
| `M_PROC_VERSION_100_SP6` | Specifies the version as being Aurora Imaging Library 10.0 Service Pack 6. |
| `M_PROC_VERSION_100_SP7` | Specifies the version as being Aurora Imaging Library 10.0 Service Pack 7. |
| `M_PROC_VERSION_110` | Specifies the version as being Aurora Imaging Library 11.0. |
| `M_DEFAULT` | Specifies the default behavior. |
| `M_GENDC` | Specifies that the data is saved using a GenDC-compatible format.  > **Note:** This setting is only available for a container and for not a buffer. |
| `M_PNG` | Specifies that the data is saved using the PNG file format.  > **Note:** This setting is only available for a buffer and not a container. |

---

### `M_LOAD`

Loads the content of a specified file or memory stream into a previously allocated buffer or container.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to[`M_FILE`](../../Reference/buf/MbufStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), or URL when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to [`M_FILE`](../../Reference/buf/MbufStream.md). Files that store a buffer or container in the Aurora Imaging Library native format typically have the mbuf or mbufc extension respectively. Files that store a format in the GenDC format typically have the gendc extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to [`M_MEMORY`](../../Reference/buf/MbufStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| `M_DEFAULT` | Specifies the default behavior. When loading a container, this is equivalent to [`M_BASIC_ATTRIBUTE`](../../Reference/buf/MbufStream.md). |
| `M_GENDC` | Specifies that the stream is in a GenDC-compatible format.  > **Note:** This setting is only available for a container and not a buffer. |
| `M_PNG` | Specifies to load the contents of a file or memory stream that is in the PNG file format.  > **Note:** This setting is only available for a buffer and not a container.  > **Note:** When using this setting with [`M_MEMORY`](../../Reference/buf/MbufStream.md), you must specify the size using the[`SizeByteVarPtr`](../../Reference/buf/MbufStream.md) parameter. |
| `M_NULL` | Specifies that this parameter is unused. |
| `SizeByteVarPtr` | The address of the variable that contains the size of the buffer, in bytes. |

---

### `M_RESTORE`

Restores a buffer or container from a file or memory stream and assigns it an Aurora Imaging Library identifier.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to[`M_FILE`](../../Reference/buf/MbufStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), or URL when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to [`M_FILE`](../../Reference/buf/MbufStream.md). Files that store a buffer or container in the Aurora Imaging Library native format typically have the mbuf or mbufc extension respectively. Files that store a format in the GenDC format typically have the gendc extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to [`M_MEMORY`](../../Reference/buf/MbufStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |
| `M_DEFAULT` | Specifies the default behavior. |
| `M_GENDC` | Specifies that the data is saved using a GenDC-compatible format.  > **Note:** This setting is only available for a container and not a buffer. |
| `M_PNG` | Specifies that the data is saved using the PNG file format.  > **Note:** This setting is only available for a buffer and not a container.  > **Note:** When using this setting with [`M_MEMORY`](../../Reference/buf/MbufStream.md), you must specify the size using the[`SizeByteVarPtr`](../../Reference/buf/MbufStream.md) parameter. |
| `M_NULL` | Specifies that this parameter is unused. |
| `SizeByteVarPtr` | Specifies the address of the variable that contains the size of the buffer or container, in bytes. |

---

### `M_SAVE`

Saves a buffer or container to a specified file or memory stream.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to[`M_FILE`](../../Reference/buf/MbufStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), or URL when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to [`M_FILE`](../../Reference/buf/MbufStream.md). File streams that store a buffer or container in the Aurora Imaging Library native format typically have the mbuf or mbufc extension respectively. File streams that store a format in the GenDC format typically have the gendc extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/buf/MbufStream.md) parameter is set to [`M_MEMORY`](../../Reference/buf/MbufStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which to write, and the block of memory must be large enough to stream the entire object. To determine the required size, call this function with [`M_INQUIRE_SIZE_BYTE`](../../Reference/buf/MbufStream.md).  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| *(see [`M_INQUIRE_SIZE_BYTE`](Reference/buf/MbufStream.md))* |  |
| `M_DEFAULT` | Specifies the default behavior. |
| `M_COMPRESS` | Specifies to losslessly compress the data in the stream.  > **Note:** This setting is only available for a container and not a buffer. |
| `M_GENDC` | Specifies to save the data using a GenDC-compatible format. When saving using a GenDC-compatible format, some attributes and settings of the container and its components might be removed.  > **Note:** This setting is only available for a container and not a buffer.  An error is generated if the source container has one of the following:  - A component that is not an[`M_IMAGE`](../../Reference/buf/MbufInquireContainer.md) or [`M_ARRAY`](../../Reference/buf/MbufInquireContainer.md)buffer. - A component with[`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md) set to [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufInquireContainer.md)or [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufInquireContainer.md). - A component with[`M_COMPONENT_GROUP_ID`](../../Reference/buf/MbufInquireContainer.md),[`M_COMPONENT_REGION_ID`](../../Reference/buf/MbufInquireContainer.md), or [`M_COMPONENT_SOURCE_ID`](../../Reference/buf/MbufInquireContainer.md) set to a value larger than 65535. - A component that is an image buffer compressed in a format other than JPEG or JPEG 2000. |
| `M_PNG` | Specifies to save the data using the PNG file format.  > **Note:** This setting is only available for a buffer and not a container. |
| `M_NULL` | Specifies that this parameter is unused. |

### Combination Constants — For specifying what to load from the contents of a file or memory

> *Optional, can be used alone.*

> **Usage:** You can use one of the following values on its own, or add it to the above-mentioned values, to specify what to load from a file or stream.

| Value | Description |
| --- | --- |
| `M_BASIC_ATTRIBUTE` | Specifies to load the contents of the file or memory stream (that stores data suitable for loading into a container) to automatically allocated components that might have different attributes than the original stored components.  > **Note:** This setting is only available for a container and not a buffer. |
| `M_IDENTICAL` | Specifies to load all of the contents of the file or memory stream to the specified buffer (or to the specified container).  When loading a container, this will includes all data, settings, and attributes (except [`M_PROC`](../../Reference/buf/MbufAllocColor.md), [`M_GRAB`](../../Reference/buf/MbufAllocColor.md), and [`M_DISP`](../../Reference/buf/MbufAllocColor.md) which are always the same as the attributes of the destination container).  When loading a buffer, an error will be generated if there is a mismatch between the size, number of bands, or attributes (such as[`M_RGB24`](../../Reference/buf/MbufAllocColor.md)) of the destination buffer and the contents of the file. |
| `M_USE_DESTINATION` | Specifies to load the contents of the file or memory stream (that stores data suitable for loading into a container) into the existing components of the destination container. An error will be generated if the file or memory stream has a different number of components than the destination container.  > **Note:** This setting is only available for a container and not a buffer. |

### Combination Constants — For specifying that the buffer is not restored with a certain attribute

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to specify that the buffer is not restored with a certain attribute.

> **Note:** Note that these values are only available when restoring an image buffer, and not a container.

| Value | Description |
| --- | --- |
| `M_NO_COMPRESS` | Specifies that Aurora Imaging Library will not allocate the restored data buffer with an [`M_COMPRESS`](../../Reference/buf/MbufAlloc2d.md) attribute. |
| `M_NO_GRAB` | Specifies that Aurora Imaging Library will not allocate the restored data buffer with an [`M_GRAB`](../../Reference/buf/MbufAlloc2d.md) attribute. |

### Combination Constants — For specifying the compression level of a PNG

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the compression level of a PNG.

| Value | Description |
| --- | --- |
| `M_PNG_COMPRESSION_LEVELn` | Specifies a compression level of _n_, where _n_ is a value from 0 to 9.  > **Note:** The highest compression level (that is, 9) will result in the smallest file, but will take longer to save. |
