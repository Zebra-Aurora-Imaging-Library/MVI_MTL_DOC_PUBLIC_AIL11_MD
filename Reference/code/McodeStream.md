---
doctype: Reference
module: code
function: McodeStream
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / code / McodeStream"
---

# McodeStream

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

> Load, restore, or save a code context from/to a file or a memory stream, save a grading report to a file, or save a training report to a file.

## Syntax

```c
void McodeStream(
    AIL_TEXT_PTR MemPtrOrFileName,          //in-out
    AIL_ID       SysId,                     //in
    AIL_INT64    Operation,                 //in
    AIL_INT64    StreamType,                //in
    AIL_DOUBLE   Version,                   //in
    AIL_INT64    ControlFlag,               //in
    AIL_ID *     ContextOrResultCodeIdPtr,  //in-out
    AIL_INT *    SizeByteVarPtr             //out
)
```

## Description

This function can load, restore, or save a code context from/to a file or memory stream. This function can also save a grading report to a file or save a training report to a file.

To inquire the number of bytes necessary to save a code context to memory stream, you should first call this function ([`McodeStream`](../../Reference/code/McodeStream.md)) with [`M_INQUIRE_SIZE_BYTE`](../../Reference/code/McodeStream.md).

The content saved to memory stream is equivalent to the content saved to file. In addition, any file saved using this function is equivalent to a file saved with [`McodeSave`](../../Reference/code/McodeSave.md).

You can use this and other Aurora Imaging Library stream functions, for example, to save all required Aurora Imaging Library objects, as well as any other custom data, for your application to a memory stream. Once in a memory stream, you can write the stream to a single file or transfer it over a network. You are responsible for concatenating the streams and for saving the stream to file.

Using [`McodeStream`](../../Reference/code/McodeStream.md), you can choose to save a backwards-compatible version of the code context, which will work using a specified version of Aurora Imaging Library that is up to one major release older than the current version. However, all settings and features unique to the higher version will be ignored when restored using the lower version. Besides saving backwards-compatible versions, you can also load or restore a code context saved using Aurora Imaging Library version 7.0 or above. Settings that do not exist in the lower version will be filled with default values when the code context is loaded or restored using the current version.

## Parameters

### `MemPtrOrFileName` *(in-out, AIL_TEXT_PTR)*

Specifies the file or memory stream.

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the code context.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform on the code context.

### `StreamType` *(in, AIL_INT64)*

Specifies the type of stream in which to store/from which to restore the code context. This parameter must be set to one of the following values:

*For specifying the type of stream*

| Value | Description |
| --- | --- |
| `M_FILE` | Specifies a file stream. Note that this is the only value available when performing an [`M_SAVE_REPORT`](../../Reference/code/McodeStream.md) operation. |
| `M_MEMORY` | Specifies a memory stream. You are responsible for allocating a block of memory for the stream. |

### `Version` *(in, AIL_DOUBLE)*

Specifies the Aurora Imaging Library version of the code context.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ContextOrResultCodeIdPtr` *(in-out, *AIL_ID)*

Specifies the address of the variable in which to write or from which to read the identifier of the code context. In the case of an [`M_SAVE_REPORT`](../../Reference/code/McodeStream.md) operation, this parameter specifies the address of the variable from which to read the identifier of the code result buffer.

### `SizeByteVarPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the size of the code context, in bytes. If the size is not required, or if performing an [`M_SAVE_REPORT`](../../Reference/code/McodeStream.md) operation, set this parameter to `M_NULL`.

## Parameter Associations

### For performing the stream operation.

---

### `M_INQUIRE_SIZE_BYTE`

Inquires the number of bytes required to save a code context to memory stream. This operation is not supported when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to [`M_FILE`](../../Reference/code/McodeStream.md).

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

Loads the content of a specified file or memory stream into a previously allocated code context.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to[`M_FILE`](../../Reference/code/McodeStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to [`M_FILE`](../../Reference/code/McodeStream.md). Code contexts typically have an MCO file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to [`M_MEMORY`](../../Reference/code/McodeStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |

---

### `M_RESTORE`

Restores a code context from a file or memory stream and assigns it an Aurora Imaging Library identifier.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to[`M_FILE`](../../Reference/code/McodeStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to [`M_FILE`](../../Reference/code/McodeStream.md). Code contexts typically have an MCO file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to [`M_MEMORY`](../../Reference/code/McodeStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_SAVE`

Saves a code context to a specified file or memory stream.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to[`M_FILE`](../../Reference/code/McodeStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to [`M_FILE`](../../Reference/code/McodeStream.md). For easier use with other Aurora Imaging software products, when saving a code context to a file, use the MCO file extension. The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to [`M_MEMORY`](../../Reference/code/McodeStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which to write, and the block of memory must be large enough to stream the entire object. To determine the required size, call this function with [`M_INQUIRE_SIZE_BYTE`](../../Reference/code/McodeStream.md).  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| *(see [`M_INQUIRE_SIZE_BYTE`](Reference/code/McodeStream.md))* |  |

---

### `M_SAVE_REPORT`

Saves a report containing most results from an [`McodeGrade`](../../Reference/code/McodeGrade.md) or [`McodeTrain`](../../Reference/code/McodeTrain.md)operation.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to[`M_FILE`](../../Reference/code/McodeStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/code/McodeStream.md) parameter is set to [`M_FILE`](../../Reference/code/McodeStream.md). The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
