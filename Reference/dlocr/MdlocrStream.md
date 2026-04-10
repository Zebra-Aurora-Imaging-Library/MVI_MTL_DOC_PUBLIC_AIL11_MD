---
doctype: Reference
module: dlocr
function: MdlocrStream
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrStream"
---

# MdlocrStream

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

> Load, restore, or save a Deep Learning OCR read or fine-tuning context from/to a file or a memory stream.

## Syntax

```c
void MdlocrStream(
    AIL_TEXT_PTR MemPtrOrFileName,   //in-out
    AIL_ID       SysId,              //in
    AIL_INT64    Operation,          //in
    AIL_INT64    StreamType,         //in
    AIL_DOUBLE   Version,            //in
    AIL_INT64    ControlFlag,        //in
    AIL_ID *     ContextDlocrIdPtr,  //in-out
    AIL_INT *    SizeByteVarPtr      //out
)
```

## Description

This function can load, restore, or save a Deep Learning OCR context from/to a file or memory stream. All information about the previously allocated context is saved, including all of the string model-specific settings and positional constraints. Preprocessing changes are not saved.

To inquire the number of bytes necessary to save a Deep Learning OCR read or fine-tuning context to memory stream, first call [`MdlocrStream`](../../Reference/dlocr/MdlocrStream.md) with [`M_INQUIRE_SIZE_BYTE`](../../Reference/dlocr/MdlocrStream.md).

The content saved to memory stream is equivalent to the content saved to file. In addition, any file saved using this function is equivalent to a file saved with [`MdlocrSave`](../../Reference/dlocr/MdlocrSave.md).

You can use this and other Aurora Imaging Library stream functions, for example, to save all required Aurora Imaging Library objects, as well as any other custom data, for your application to a memory stream. Once in a memory stream, you can write the stream to a single file or transfer it over a network. You are responsible for concatenating the streams and for saving the stream to file.

All of the context settings that were in effect when the context was saved will be restored. A loaded or restored context is not preprocessed. You must call [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md) before performing a read with [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md).

Using [`MdlocrStream`](../../Reference/dlocr/MdlocrStream.md), you can choose to save a backwards-compatible version of the Deep Learning OCR context, which will work using a version of Aurora Imaging Library that is up to one major release older than the current version (depending on which version is specified). However, all settings and features unique to the higher version will be ignored when restored using the lower version. Besides saving backwards-compatible versions, you can also load or restore Deep Learning OCR contexts saved using any Aurora Imaging Library version with which this function was available. Settings that do not exist in the lower version will be filled with default values when the Deep Learning OCR context is loaded or restored.

## Parameters

### `MemPtrOrFileName` *(in-out, AIL_TEXT_PTR)*

Specifies the file or memory stream.

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to restore the Deep Learning OCR read or fine-tuning context.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform on the Deep Learning OCR read or fine-tuning context.

### `StreamType` *(in, AIL_INT64)*

Specifies the type of stream in which to store/from which to restore the Deep Learning OCR read or fine-tuning context. Set this parameter to one of the values below:

*For specifying the type of stream*

| Value | Description |
| --- | --- |
| `M_FILE` | Specifies a file stream. |
| `M_MEMORY` | Specifies a memory stream. You are responsible for allocating a block of memory for the stream. |

### `Version` *(in, AIL_DOUBLE)*

Specifies the Aurora Imaging Library version of the Deep Learning OCR read or fine-tuning context.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextDlocrIdPtr` *(in-out, *AIL_ID)*

Specifies the address of the variable in which to write or from which to read the identifier of the Deep Learning OCR context.

### `SizeByteVarPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the size of the Deep Learning OCR context, in bytes. If size is not required, set this parameter to `M_NULL`.

## Parameter Associations

### For performing the stream operation.

---

### `M_INQUIRE_SIZE_BYTE`

Inquires the number of bytes required to save a Deep Learning OCR context to memory stream. This operation is not supported when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to [`M_FILE`](../../Reference/dlocr/MdlocrStream.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the current version of Aurora Imaging Library. |
| `M_PROC_VERSION_100_SP7` | Specifies the version as being Aurora Imaging Library 10.0 Service Pack 7. |
| `M_PROC_VERSION_110` | Specifies the version as being Aurora Imaging Library 11.0. |

---

### `M_LOAD`

Loads the content of a specified file or memory stream into a previously allocated Deep Learning OCR context.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the context file, when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to[`M_FILE`](../../Reference/dlocr/MdlocrStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the context file (for example, _"C:\mydirectory\MyContext.mdlocr"_), when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to [`M_FILE`](../../Reference/dlocr/MdlocrStream.md). Deep Learning OCR contexts typically have an MDLOCR file extension. The function handles (internally) the opening and closing of the file.  To specify a context file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MyContext.mdlocr"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to [`M_MEMORY`](../../Reference/dlocr/MdlocrStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |

---

### `M_RESTORE`

Restores a Deep Learning OCR context from a file or memory stream and assigns it an Aurora Imaging Library identifier.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the context file, when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to[`M_FILE`](../../Reference/dlocr/MdlocrStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the context file (for example, _"C:\mydirectory\MyContext.mdlocr"_), when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to [`M_FILE`](../../Reference/dlocr/MdlocrStream.md). Deep Learning OCR contexts typically have an MDLOCR file extension. The function handles (internally) the opening and closing of the file.  To specify a context file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MyContext.mdlocr"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to [`M_MEMORY`](../../Reference/dlocr/MdlocrStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_SAVE`

Saves a Deep Learning OCR context to a specified file or memory stream.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the context file, when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to[`M_FILE`](../../Reference/dlocr/MdlocrStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the context file (for example, _"C:\mydirectory\MyContext.mdlocr"_), when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to [`M_FILE`](../../Reference/dlocr/MdlocrStream.md). Deep Learning OCR contexts typically have an MDLOCR file extension. The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.  To specify a context file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MyContext.mdlocr"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/dlocr/MdlocrStream.md) parameter is set to [`M_MEMORY`](../../Reference/dlocr/MdlocrStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which to write, and the block of memory must be large enough to stream the entire object. To determine the required size, call this function with [`M_INQUIRE_SIZE_BYTE`](../../Reference/dlocr/MdlocrStream.md).  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| *(see [`M_INQUIRE_SIZE_BYTE`](Reference/dlocr/MdlocrStream.md))* |  |
