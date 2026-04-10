---
doctype: Reference
module: pat
function: MpatRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / pat / MpatRestore"
---

# MpatRestore

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

> Restore a Pattern Matching context from disk.

## Syntax

```c
AIL_ID MpatRestore(
    AIL_CONST_TEXT_PTR FileName,        //in
    AIL_ID             SysId,           //in
    AIL_INT64          ControlFlag,     //in
    AIL_ID *           ContextPatIdPtr  //out
)
```

## Description

This function restores a Pattern Matching context that was previously saved to a file, using [`MpatSave`](../../Reference/pat/MpatSave.md). A restored Pattern Matching context is not preprocessed, therefore you must call [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md) before performing a search with [`MpatFind`](../../Reference/pat/MpatFind.md).

When the restored Pattern Matching context is no longer required, release it using[`MpatFree`](../../Reference/pat/MpatFree.md)unless [`M_UNIQUE_ID`](../../Reference/pat/MpatRestore.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/pat/MpatRestore.md) was specified, the smart identifier manages the restored Pattern Matching context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the model. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, Pattern Matching context files have an MPAT extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the model.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, which you have allocated using the [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) function. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ContextPatIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Pattern Matching context identifier or specifies the data type that the function should use to return the Pattern Matching context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Pattern Matching context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Pattern Matching context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_PAT_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Pattern Matching context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored Pattern Matching context.

If restoration fails, [`M_NULL`](../../Reference/pat/MpatRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Pattern Matching context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_PAT_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/pat/MpatRestore.md) was specified).
