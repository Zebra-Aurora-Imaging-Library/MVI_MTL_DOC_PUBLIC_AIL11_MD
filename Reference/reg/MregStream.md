---
doctype: Reference
module: reg
function: MregStream
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregStream"
---

# MregStream

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

> Load, restore, or save a registration context or an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer from/to a file or memory stream.

## Syntax

```c
void MregStream(
    AIL_TEXT_PTR MemPtrOrFileName,         //in-out
    AIL_ID       SysId,                    //in
    AIL_INT64    Operation,                //in
    AIL_INT64    StreamType,               //in
    AIL_DOUBLE   Version,                  //in
    AIL_INT64    ControlFlag,              //in
    AIL_ID *     ContextOrResultRegIdPtr,  //in-out
    AIL_INT *    SizeByteVarPtr            //out
)
```

## Description

This function can load, restore, or save a registration context and/or an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer from/to a file or memory stream.

To inquire the number of bytes necessary to save a registration context or result buffer to a memory stream, you should first call this function ([`MregStream`](../../Reference/reg/MregStream.md)) with [`M_INQUIRE_SIZE_BYTE`](../../Reference/reg/MregStream.md).

The content saved to memory stream is equivalent to the content saved to file. This function is equivalent to a file saved using [`MregSave`](../../Reference/reg/MregSave.md).

You can use this and other Aurora Imaging Library stream functions, for example, to save all required Aurora Imaging Library objects, as well as any other custom data, for your application to a memory stream. Once in a memory stream, you can write the stream to a single file or transfer it over a network. You are responsible for concatenating the streams and for saving the stream to file.

Using [`MregStream`](../../Reference/reg/MregStream.md), you can choose to save a backwards-compatible version of the registration context or result buffer, which will work using a version of Aurora Imaging Library that is up to one major release older than the current version (depending on which version is specified). However, all settings and features unique to the higher version will be ignored when restored using the lower version. Besides saving backwards-compatible versions, you can also load or restore registration contexts or result buffers saved using Aurora Imaging Library version 9.0 or above. Settings that do not exist in the lower version will be filled with default values when the registration context is loaded or restored.

## Parameters

### `MemPtrOrFileName` *(in-out, AIL_TEXT_PTR)*

Specifies the file or memory stream.

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the registration context or [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

### `StreamType` *(in, AIL_INT64)*

Specifies the type of stream in which to store/from which to restore the registration context or the [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. This parameter must be set to one of the following values:

*For specifying the type of stream*

| Value | Description |
| --- | --- |
| `M_FILE` | Specifies a file stream. |
| `M_MEMORY` | Specifies a memory stream. You are responsible for allocating a block of memory for the stream. |

### `Version` *(in, AIL_DOUBLE)*

Specifies the Aurora Imaging Library version of the registration context or [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ContextOrResultRegIdPtr` *(in-out, *AIL_ID)*

Specifies the address of the variable in which to write or from which to read the identifier of the registration context or the [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. If the [`M_RESTORE`](../../Reference/reg/MregStream.md) operation is not successful, `M_NULL` is returned.

### `SizeByteVarPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the size of the registration context or [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, in bytes.

## Parameter Associations

### For performing the stream operation.

---

### `M_INQUIRE_SIZE_BYTE`

Inquires the number of bytes required to save a registration context or result buffer to memory stream. This operation is not supported when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to [`M_FILE`](../../Reference/reg/MregStream.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the current version of Aurora Imaging Library. |
| `M_PROC_VERSION_100_SP4` | Specifies the version as being Aurora Imaging Library 10.0 Service Pack 4. |
| `M_PROC_VERSION_100_SP5` | Specifies the version as being Aurora Imaging Library 10.0 Service Pack 5. |
| `M_PROC_VERSION_100_SP6` | Specifies the version as being Aurora Imaging Library 10.0 Service Pack 6. |
| `M_PROC_VERSION_100_SP7` | Specifies the version as being Aurora Imaging Library 10.0 Service Pack 7. |
| `M_PROC_VERSION_110` | Specifies the version as being Aurora Imaging Library 11.0. |

---

### `M_LOAD`

Loads the content of a specified file or memory stream into a previously allocated registration context or result buffer.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to[`M_FILE`](../../Reference/reg/MregStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to [`M_FILE`](../../Reference/reg/MregStream.md). Registration contexts or result buffers typically have an MREG file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to [`M_MEMORY`](../../Reference/reg/MregStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |

---

### `M_RESTORE`

Restores a registration context or result buffer from a file or memory stream and assigns it an Aurora Imaging Library identifier.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to[`M_FILE`](../../Reference/reg/MregStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to [`M_FILE`](../../Reference/reg/MregStream.md). Registration contexts or result buffers typically have an MREG file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to [`M_MEMORY`](../../Reference/reg/MregStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_SAVE`

Saves a registration context or an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) the contents of a registration result buffer to a specified file or memory stream.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to[`M_FILE`](../../Reference/reg/MregStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to [`M_FILE`](../../Reference/reg/MregStream.md). For easier use with other Aurora Imaging Library software products, when saving a registration context to a file, use the MREG file extension. The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/reg/MregStream.md) parameter is set to [`M_MEMORY`](../../Reference/reg/MregStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which to write, and the block of memory must be large enough to stream the entire object. To determine the required size, call this function with [`M_INQUIRE_SIZE_BYTE`](../../Reference/reg/MregStream.md).  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| *(see [`M_INQUIRE_SIZE_BYTE`](Reference/reg/MregStream.md))* |  |
