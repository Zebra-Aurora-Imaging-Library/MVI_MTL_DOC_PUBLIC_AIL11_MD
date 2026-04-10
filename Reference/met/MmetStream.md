---
doctype: Reference
module: met
function: MmetStream
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetStream"
---

# MmetStream

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

> Load, restore, or save an Aurora Imaging Library metrology context from/to a file or memory stream.

## Syntax

```c
void MmetStream(
    AIL_TEXT_PTR MemPtrOrFileName,  //in-out
    AIL_ID       SysId,             //in
    AIL_INT64    Operation,         //in
    AIL_INT64    StreamType,        //in
    AIL_DOUBLE   Version,           //in
    AIL_INT64    ControlFlag,       //in
    AIL_ID *     ContextMetIdPtr,   //in-out
    AIL_INT *    SizeByteVarPtr     //out
)
```

## Description

This function can load, restore, or save an Aurora Imaging Library metrology context from/to a file or memory stream.

To inquire the number of bytes necessary to save a Model Finder context to memory stream, you should first call this function ([`MmetStream`](../../Reference/met/MmetStream.md)) with [`M_INQUIRE_SIZE_BYTE`](../../Reference/met/MmetStream.md).

The content saved to memory stream is equivalent to the content saved to file. This function is equivalent to a file saved using [`MmetSave`](../../Reference/met/MmetSave.md).

You can use this and other Aurora Imaging Library stream functions, for example, to save all required Aurora Imaging Library objects, as well as any other custom data, for your application to a memory stream. Once in a memory stream, you can write the stream to a single file or transfer it over a network. You are responsible for concatenating the streams and for saving the stream to file. All information about the previously allocated metrology context is saved, including all of the feature settings and geometric tolerance settings. However, any associated camera calibration contexts and drawing control type settings are not saved.

All of the metrology context's settings and names that were in effect when the metrology context was saved will be restored. If you had associated a camera calibration context with the template reference in your metrology context and you did not save it with the context, you must re-associate the camera calibration context, using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/met/MmetControl.md). Also, if you had previously set drawing control types, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_...`](../../Reference/gra/MgraControl.md), you must reset them since they were not saved with the context.

Using [`MmetStream`](../../Reference/met/MmetStream.md), you can choose to save a backwards-compatible version of an Aurora Imaging Library metrology context, which will work using a version of Aurora Imaging Library that is up to one major release older than the current version (depending on which version is specified). However, all settings and features unique to the higher version will be ignored when restored using the lower version. Besides saving backwards-compatible versions, you can also load or restore Aurora Imaging Library metrology context saved using Aurora Imaging Library version 8.0 or above. Settings that do not exist in the lower version will be filled with default values when the metrology context is loaded or restored.

## Parameters

### `MemPtrOrFileName` *(in-out, AIL_TEXT_PTR)*

Specifies the file or memory stream.

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the metrology context.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

### `StreamType` *(in, AIL_INT64)*

Specifies the type of stream in which to store/from which to restore the metrology context.

*For the type of stream*

| Value | Description |
| --- | --- |
| `M_FILE` | Specifies a file stream. |
| `M_MEMORY` | Specifies a memory stream. You are responsible for allocating a block of memory for the stream. |

### `Version` *(in, AIL_DOUBLE)*

Specifies the Aurora Imaging Library version of the metrology context. When performing an [`M_LOAD`](../../Reference/met/MmetStream.md) or [`M_RESTORE`](../../Reference/met/MmetStream.md) operation, this parameter must be set to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to restore, from the file or memory stream, the camera calibration context associated with the template reference, or specifies to save, with the metrology context, the camera calibration context associated with the template reference.

*For specifying whether to restore or save the camera calibration contexts*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the camera calibration contexts are not saved or restored. |
| `M_WITH_CALIBRATION` | Specifies that the camera calibration context is saved or restored, depending on the [`Operation`](../../Reference/met/MmetStream.md) parameter. When restored, the camera calibration context must have been previously saved with the context, using [`MmetSave`](../../Reference/met/MmetSave.md) or [`MmetStream`](../../Reference/met/MmetStream.md) with [`M_WITH_CALIBRATION`](../../Reference/met/MmetSave.md).

The camera calibration information is restored from/ saved to the same file or memory stream as the metrology context. The camera calibration cannot be managed independently from the metrology context. When the metrology context is freed, the camera calibration is automatically freed as well. |

### `ContextMetIdPtr` *(in-out, *AIL_ID)*

Specifies the address of the variable in which to write or from which to read the identifier of the metrology context.

### `SizeByteVarPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the size of the metrology context, in bytes.

## Parameter Associations

### For performing the stream operation.

---

### `M_INQUIRE_SIZE_BYTE`

Inquires the number of bytes required to save a metrology context to memory stream. This operation is not supported when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to [`M_FILE`](../../Reference/met/MmetStream.md).

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

Loads the content of a specified file or memory stream into a metrology context.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to[`M_FILE`](../../Reference/met/MmetStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to [`M_FILE`](../../Reference/met/MmetStream.md). Metrology contexts typically have an MET file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to [`M_MEMORY`](../../Reference/met/MmetStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |

---

### `M_RESTORE`

Specifies the address of the variable in which to write the identifier of the metrology context. If the operation is not successful, [`M_NULL`](../../Reference/met/MmetStream.md) is returned.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to[`M_FILE`](../../Reference/met/MmetStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to [`M_FILE`](../../Reference/met/MmetStream.md). Metrology contexts typically have an MET file extension. The function handles (internally) the opening and closing of the file.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to [`M_MEMORY`](../../Reference/met/MmetStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which the object is located, and the block of memory must contain the entire object.  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_SAVE`

Saves a metrology context to a specified file or memory stream.

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file, when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to[`M_FILE`](../../Reference/met/MmetStream.md). |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_), when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to [`M_FILE`](../../Reference/met/MmetStream.md). For easier use with other Aurora Imaging Library software products, when saving a metrology context to a file, use the MET file extension. The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.  To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |
| `MemPtr` | Specifies the address of the block of memory, when the [`StreamType`](../../Reference/met/MmetStream.md) parameter is set to [`M_MEMORY`](../../Reference/met/MmetStream.md). The block of memory should be of type _AIL_UINT8_. The specified address must correspond to the first memory address in which to write, and the block of memory must be large enough to stream the entire object. To determine the required size, call this function with [`M_INQUIRE_SIZE_BYTE`](../../Reference/met/MmetStream.md).  Note that when using a C compiler (not a C++ or other compiler), you must cast the _AIL_UINT8_ pointer to an _AIL_TEXT_PTR_. |
| *(see [`M_INQUIRE_SIZE_BYTE`](Reference/met/MmetStream.md))* |  |
