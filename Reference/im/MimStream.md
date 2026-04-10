---
doctype: Reference
module: im
function: MimStream
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimStream"
---

# MimStream

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

> Load, restore, or save an image processing context from/to a file or a memory stream, or save an augmentation report to a file.

## Syntax

```c
void MimStream(
    AIL_TEXT_PTR MemPtrOrFileName,  //in-out
    AIL_ID       SysId,             //in
    AIL_INT64    Operation,         //in
    AIL_INT64    StreamType,        //in
    AIL_DOUBLE   Version,           //in
    AIL_INT64    ControlFlag,       //in
    AIL_ID *     ContextImIdPtr,    //in-out
    AIL_INT *    SizeByteVarPtr     //out
)
```

## Description

This function can load, restore, or save a image processing context from/to a file or memory stream, or save an augmentation report to a file.

To inquire the number of bytes necessary to save an image processing context to memory stream, you should first call this function ([`MimStream`](../../Reference/im/MimStream.md)) with [`M_INQUIRE_SIZE_BYTE`](../../Reference/im/MimStream.md).

The content saved to memory stream is equivalent to the content saved to file. This function is equivalent to a file saved using [`MimSave`](../../Reference/im/MimSave.md).

You can use this and other Aurora Imaging Library stream functions, for example, to save all required Aurora Imaging Library objects, as well as any other custom data, for your application to a memory stream. Once in a memory stream, you can write the stream to a single file or transfer it over a network. You are responsible for concatenating the streams and for saving the stream to file. When saving an image processing context, all information about the previously allocated image processing context is saved. The content saved to memory stream is equivalent to the content saved to file.

When loading or restoring an image processing context, all of the image processing context's settings that were in effect when the image processing context was saved will be restored. Note that, you can also restore an image processing context using [`MimRestore`](../../Reference/im/MimRestore.md).

Using [`MimStream`](../../Reference/im/MimStream.md), you can choose to save a backwards-compatible version of the image processing context, which will work using a version of Aurora Imaging Library that is up to one major release older than the current version (depending on which version is specified). However, all settings and features unique to the higher version will be ignored when restored using the lower version. Besides saving backwards-compatible versions, you can also load or restore image processing contexts saved using Aurora Imaging Library version 9.0 or above. Settings that do not exist in the lower version will be filled with default values when the image processing context is loaded or restored.

## Parameters

### `MemPtrOrFileName` *(in-out, AIL_TEXT_PTR)*

Specifies the file or memory stream.

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the image processing context.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform on the image processing context.

### `StreamType` *(in, AIL_INT64)*

Specifies the type of stream in which to store/from which to restore the image processing context. This parameter must be set to one of the following values:

*For specifying the type of stream*

| Value | Description |
| --- | --- |
| `M_FILE` | Specifies a file stream. |
| `M_MEMORY` | Specifies a memory stream. You are responsible for allocating a block of memory for the stream. |

### `Version` *(in, AIL_DOUBLE)*

Specifies the Aurora Imaging Library version of the image processing context.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ContextImIdPtr` *(in-out, *AIL_ID)*

Specifies the address of the variable in which to write or from which to read the identifier of the image processing context. In the case of an [`M_SAVE_REPORT`](../../Reference/im/MimStream.md) operation, this parameter specifies the address of the variable from which to read the identifier of the augmentation result buffer.

### `SizeByteVarPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the size of the image processing context, in bytes.

## Parameter Associations

### For performing the stream operation.

---

### `M_INQUIRE_SIZE_BYTE`

Inquires the number of bytes required to save an image processing context to memory stream. This operation is not supported when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to [`M_FILE`](../../Reference/im/MimStream.md).

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

Loads the content of a specified file or memory stream into a previously allocated image processing context.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to[`M_FILE`](../../Reference/im/MimStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to [`M_FILE`](../../Reference/im/MimStream.md). Image processing contexts typically have an MIC file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to [`M_MEMORY`](../../Reference/im/MimStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |

---

### `M_RESTORE`

Restores an image processing context from a file or memory stream and assigns it an Aurora Imaging Library identifier.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to[`M_FILE`](../../Reference/im/MimStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to [`M_FILE`](../../Reference/im/MimStream.md). Image processing contexts typically have an MIC file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to [`M_MEMORY`](../../Reference/im/MimStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_SAVE`

Saves an image processing context to a specified file or memory stream.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to[`M_FILE`](../../Reference/im/MimStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to [`M_FILE`](../../Reference/im/MimStream.md). For easier use with other Zebra Aurora software products, when saving a image processing context to a file, use the MIC file extension. The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to [`M_MEMORY`](../../Reference/im/MimStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which to write, and the block of memory must be large enough to stream the entire object. To determine the required size, call this function with [`M_INQUIRE_SIZE_BYTE`](../../Reference/im/MimStream.md).  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| *(see [`M_INQUIRE_SIZE_BYTE`](Reference/im/MimStream.md))* |  |

---

### `M_SAVE_REPORT`

Saves a report containing results from calling [`MimAugment`](../../Reference/im/MimAugment.md) with an augmentation result buffer.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to[`M_FILE`](../../Reference/im/MimStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\MyReportFile.txt"_), when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to [`M_FILE`](../../Reference/im/MimStream.md). You should use a TXT file extension. The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MyReportFile.txt"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/im/MimStream.md) parameter is set to [`M_MEMORY`](../../Reference/im/MimStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which to write, and the block of memory must be large enough to stream the entire object. To determine the required size, call this function with [`M_INQUIRE_SIZE_BYTE`](../../Reference/im/MimStream.md).  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
