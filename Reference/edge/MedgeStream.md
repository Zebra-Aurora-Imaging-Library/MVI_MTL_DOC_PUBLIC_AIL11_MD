---
doctype: Reference
module: edge
function: MedgeStream
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeStream"
---

# MedgeStream

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

> Load, restore, or save an Edge Finder context from/to a file or memory stream, or save calculated edges from an Edge Finder result buffer to a file or memory stream in CAD DXF format.

## Syntax

```c
void MedgeStream(
    AIL_TEXT_PTR MemPtrOrFileName,          //in-out
    AIL_ID       SysId,                     //in
    AIL_INT64    Operation,                 //in
    AIL_INT64    StreamType,                //in
    AIL_DOUBLE   Version,                   //in
    AIL_INT64    ControlFlag,               //in
    AIL_ID *     ContextOrResultEdgeIdPtr,  //in-out
    AIL_INT *    SizeByteVarPtr             //out
)
```

## Description

This function can load, restore, or save an Edge Finder context from/to a file or memory stream. Moreover, this function can save previously calculated edges (edge chains and/or edge approximations), stored in an Edge Finder result buffer, to a file or memory stream in a standard CAD DXF format. Edge chains and edge approximations are saved in the DXF file or memory stream in separate layers.

> **Note:** Note that you cannot load or restore any CAD DXF file or memory stream using this function.

To inquire the number of bytes necessary to save an Edge Finder context or calculated edges to memory stream, you should first call this function ([`MedgeStream`](../../Reference/edge/MedgeStream.md)) with [`M_INQUIRE_SIZE_BYTE`](../../Reference/edge/MedgeStream.md).

The content saved to memory stream is equivalent to the content saved to file. This function is equivalent to a file saved using [`MedgeSave`](../../Reference/edge/MedgeSave.md).

You can use this and other Aurora Imaging Library stream functions, for example, to save all required Aurora Imaging Library objects, as well as any other custom data, for your application to a memory stream. Once in a memory stream, you can write the stream to a single file or transfer it over a network. You are responsible for concatenating the streams and for saving the stream to file.

Using [`MedgeStream`](../../Reference/edge/MedgeStream.md), you can choose to save a backwards-compatible version of the Edge Finder context, which will work using a version of Aurora Imaging Library that is up to one major release older than the current version (depending on which version is specified). However, all settings and features unique to the higher version will be ignored when restored using the lower version. Besides saving backwards-compatible versions, you can also load or restore Edge Finder contexts saved using Aurora Imaging Library version 7.5 or above. Settings that do not exist in the lower version will be filled with default values when the Edge Finder context is loaded or restored.

## Parameters

### `MemPtrOrFileName` *(in-out, AIL_TEXT_PTR)*

Specifies the file or memory stream.

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the Edge Finder context.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

### `StreamType` *(in, AIL_INT64)*

Specifies the type of stream in which to store/from which to restore the Edge Finder context or calculated edges. This parameter must be set to one of the following values:

*For the type of stream*

| Value | Description |
| --- | --- |
| `M_FILE` | Specifies a file stream. |
| `M_MEMORY` | Specifies a memory stream. You are responsible for allocating a block of memory for the stream. |

### `Version` *(in, AIL_DOUBLE)*

Specifies the Aurora Imaging Library version of the Edge Finder context. When performing an [`M_LOAD`](../../Reference/edge/MedgeStream.md) or [`M_RESTORE`](../../Reference/edge/MedgeStream.md) operation, this parameter must be set to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag.

### `ContextOrResultEdgeIdPtr` *(in-out, *AIL_ID)*

Specifies the address of the variable in which to write or from which to read the identifier of the Edge Finder context or the Edge Finder result buffer.

### `SizeByteVarPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the size of the Edge Finder context or calculated edges, in bytes. If the size is not required, you can set this parameter to `M_NULL`.

## Parameter Associations

### For performing the stream operation.

---

### `M_INQUIRE_SIZE_BYTE`

Inquires the number of bytes required to save an Edge Finder context or result buffer to memory stream. This operation is not supported when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to [`M_FILE`](../../Reference/edge/MedgeStream.md).

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

Loads the content of a specified file or memory stream into a previously allocated Edge Finder context.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to[`M_FILE`](../../Reference/edge/MedgeStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to [`M_FILE`](../../Reference/edge/MedgeStream.md). Edge Finder contexts typically have an MEF file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to [`M_MEMORY`](../../Reference/edge/MedgeStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |

---

### `M_RESTORE`

Restores an Edge Finder context from a file or memory stream and assigns it an Aurora Imaging Library identifier.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to[`M_FILE`](../../Reference/edge/MedgeStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to [`M_FILE`](../../Reference/edge/MedgeStream.md). Edge Finder contexts typically have an MEF file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to [`M_MEMORY`](../../Reference/edge/MedgeStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_SAVE`

Saves an Edge Finder context or result buffer to a specified file or memory stream; alternatively, if a result buffer is passed, this operation saves the calculated edges stored in the result buffer to the specified file or memory stream in a standard CAD DXF format.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to[`M_FILE`](../../Reference/edge/MedgeStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to [`M_FILE`](../../Reference/edge/MedgeStream.md). For easier use with other Aurora Imaging Library software products, when saving an Edge Finder context to a file, use the MEF file extension, and when saving calculated edges from an Edge Finder result buffer to a CAD file, use the DXF file extension. The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/edge/MedgeStream.md) parameter is set to [`M_MEMORY`](../../Reference/edge/MedgeStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which to write, and the block of memory must be large enough to stream the entire object. To determine the required size, call this function with [`M_INQUIRE_SIZE_BYTE`](../../Reference/edge/MedgeStream.md).  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| *(see [`M_INQUIRE_SIZE_BYTE`](Reference/edge/MedgeStream.md))* |  |
| `M_DEFAULT` | Specifies the default value. |
| `M_CHAIN` | Specifies that edge chains will be saved. |
| `M_CHAIN_APPROXIMATION` | Specifies that edge approximations will be saved. |
