---
doctype: Reference
module: 3dblob
function: M3dblobRestore
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobRestore"
---

# M3dblobRestore

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

> Restores a 3D blob analysis context from disk.

## Syntax

```c
AIL_ID M3dblobRestore(
    AIL_CONST_TEXT_PTR FileName,           //in
    AIL_ID             SysId,              //in
    AIL_INT64          ControlFlag,        //in
    AIL_ID *           Context3dblobIdPtr  //out
)
```

## Description

This function restores a 3D blob analysis context that was previously saved to a file, using [`M3dblobSave`](../../Reference/3dblob/M3dblobSave.md) or [`M3dblobStream`](../../Reference/3dblob/M3dblobStream.md). This function restores all of the 3D blob analysis context's settings that were in effect when it was saved.

When the restored 3D blob analysis context is no longer required, release it using[`M3dblobFree`](../../Reference/3dblob/M3dblobFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobRestore.md) was specified during restoration; if [`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobRestore.md) was specified, the smart identifier manages the 3D blob analysis context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the 3D blob analysis context. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, 3D blob analysis context files have an M3DBLOB extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the 3D blob analysis context.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Context3dblobIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D blob analysis context identifier or specifies the data type that the function should use to return the 3D blob analysis context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D blob analysis context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D blob analysis context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DBLOB_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D blob analysis context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the calculate 3D blob analysis context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated calculate 3D blob analysis context.

If allocation fails, [`M_NULL`](../../Reference/3dblob/M3dblobRestore.md) is written as the identifier. |
| `Address in which to write the draw 3D blob analysis context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated draw 3D blob analysis context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dblob/M3dblobRestore.md) is written as the identifier. |
| `Address in which to write the segmentation 3D blob analysis context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated segmentation 3D blob analysis context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dblob/M3dblobRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D blob analysis context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DBLOB_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobRestore.md) was specified).
