---
doctype: Reference
module: 3dreg
function: M3dregRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregRestore"
---

# M3dregRestore

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

> Restore a 3D registration context or result buffer from disk.

## Syntax

```c
AIL_ID M3dregRestore(
    AIL_CONST_TEXT_PTR FileName,                  //in
    AIL_ID             SysId,                     //in
    AIL_INT64          ControlFlag,               //in
    AIL_ID *           ContextOrResult3dregIdPtr  //out
)
```

## Description

This function restores a 3D registration context or 3D registration result buffer that was previously saved to a file, using [`M3dregSave`](../../Reference/3dreg/M3dregSave.md) or [`M3dregStream`](../../Reference/3dreg/M3dregStream.md). This function restores all of the 3D registration context's or 3D registration result buffer's settings that were in effect when it was saved.

When the 3D registration context or 3D registration result buffer is no longer required, release it using[`M3dregFree`](../../Reference/3dreg/M3dregFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dreg/M3dregRestore.md) was specified during restoration; if [`M_UNIQUE_ID`](../../Reference/3dreg/M3dregRestore.md) was specified, the smart identifier manages the 3D registration context or 3D registration result buffer's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the 3D registration context or 3D registration result buffer. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, 3D registration context files have an M3DREG extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the 3D registration context or 3D registration result buffer.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextOrResult3dregIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D registration context or 3D registration result buffer or specifies the data type that the function should use to return the 3D registration context or 3D registration result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D registration context or 3D registration result buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D registration context or 3D registration result buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DREG_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D registration context or 3D registration result buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the 3D registration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored pairwise 3D registration context.

If allocation fails, [`M_NULL`](../../Reference/3dreg/M3dregRestore.md) is written as the identifier. |
| `Address in which to write the 3D registration result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored pairwise 3D result buffer.

If allocation fails, [`M_NULL`](../../Reference/3dreg/M3dregRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D registration context or 3D registration result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_APP_ID_). If restoration fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dreg/M3dregRestore.md) was specified).
