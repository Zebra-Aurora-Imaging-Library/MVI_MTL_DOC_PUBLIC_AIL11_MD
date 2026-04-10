---
doctype: Reference
module: reg
function: MregRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregRestore"
---

# MregRestore

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

> Restore a registration context or result buffer from disk.

## Syntax

```c
AIL_ID MregRestore(
    AIL_CONST_TEXT_PTR FileName,             //in
    AIL_ID             SystemId,             //in
    AIL_INT64          ControlFlag,          //in
    AIL_ID *           ContextOrResultIdPtr  //out
)
```

## Description

This function restores a registration context or result buffer that was previously saved to a file, using [`MregSave`](../../Reference/reg/MregSave.md) or [`MregStream`](../../Reference/reg/MregStream.md). This function restores all of the registration context's or result buffer's settings that were in effect when it was saved. Note that you can only restore an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer.

After restoring the registration context, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the registration context identifier returned is not [`M_NULL`](../../Reference/reg/MregRestore.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/reg/MregRestore.md) was specified).

When the restored registration context is no longer required, release it using[`MregFree`](../../Reference/reg/MregFree.md)unless [`M_UNIQUE_ID`](../../Reference/reg/MregRestore.md) was specified during restoration; if [`M_UNIQUE_ID`](../../Reference/reg/MregRestore.md) was specified, the smart identifier manages the restored registration context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the registration context or result buffer. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, registration context files have an MREG extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `SystemId` *(in, AIL_ID)*

Specifies the system on which to restore the registration context.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextOrResultIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the registration context identifier or specifies the data type that the function should use to return the registration context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated registration context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated registration context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_REG_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the registration context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the EDOF registration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored EDOF registration context.

If restoration fails, [`M_NULL`](../../Reference/reg/MregRestore.md) is written as the identifier. |
| `Address in which to write the EDOF result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored EDOF result buffer.

If restoration fails, [`M_NULL`](../../Reference/reg/MregRestore.md) is written as the identifier. |
| `Address in which to write the HDR registration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored HDR context.

If restoration fails, [`M_NULL`](../../Reference/reg/MregRestore.md) is written as the identifier. |
| `Address in which to write the stitching registration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored stitching context.

If restoration fails, [`M_NULL`](../../Reference/reg/MregRestore.md) is written as the identifier. |
| `Address in which to write the stitching result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored stitching result buffer.

If restoration fails, [`M_NULL`](../../Reference/reg/MregRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the registration context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_REG_ID_). If restoration fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/reg/MregRestore.md) was specified).
