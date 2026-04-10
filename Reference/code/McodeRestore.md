---
doctype: Reference
module: code
function: McodeRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeRestore"
---

# McodeRestore

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

> Restore a code context from disk.

## Syntax

```c
AIL_ID McodeRestore(
    AIL_CONST_TEXT_PTR FileName,         //in
    AIL_ID             SysId,            //in
    AIL_INT64          ControlFlag,      //in
    AIL_ID *           ContextCodeIdPtr  //out
)
```

## Description

This function restores a code context that has been saved to a file, using [`McodeSave`](../../Reference/code/McodeSave.md) or [`McodeStream`](../../Reference/code/McodeStream.md). This function can also restore a code context from a predefined code context file distributed with Aurora Imaging Library (such as, _C:\Program Files\Aurora Imaging Library\&lt;version number>\Contexts\SEMI_T1-95r303.mco_). For more information, refer to [Predefined SEMI code contexts](../../UserGuide/C17_Codes/S05_Supported_code_types_and_noteworthy_characteristics.md).

This function restores all of the code context's controls and models that were in effect when the code context was saved.

When the code context is no longer required, release it using[`McodeFree`](../../Reference/code/McodeFree.md)unless [`M_UNIQUE_ID`](../../Reference/code/McodeRestore.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/code/McodeRestore.md) was specified, the smart identifier manages the code context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the path and name of the file from which to restore the code context (for example, _"C:\mydirectory\myfile"_). The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"Filename.mco"` | Specifies to restore a user-defined code context. |
| `"SEMI_T1-95r303.mco"` | Specifies to restore a predefined BC-412 1D code context that matches the *SEMI T1-95 (reapproved 0303) specification for back surface bar code marking of silicon wafers* (SEMI, 2003, SEMI). |
| `"SEMI_T2-0298E.mco"` | Specifies to restore a predefined Data Matrix 2D code context with an ECC_200 encoding scheme that matches the *SEMI T2-0298 (reapproved 1104) specification for marking of wavers with a two-dimensional matrix code symbol* (SEMI, 2004, SEMI). |
| `"SEMI_T7-0303.mco"` | Specifies to restore a predefined Data Matrix 2D code context that matches the *SEMI T7-0303 specification for back surface marking of double-side polished wafers with a two-dimensional matrix code symbol* (SEMI, 2003, SEMI). |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ContextCodeIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the code context identifier or specifies the data type that the function should use to return the code context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated code context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated code context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_CODE_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the code context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored code context.

If allocation fails, [`M_NULL`](../../Reference/code/McodeRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the code context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_CODE_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/code/McodeRestore.md) was specified).
