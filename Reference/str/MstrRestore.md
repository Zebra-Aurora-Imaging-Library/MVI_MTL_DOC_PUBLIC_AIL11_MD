---
doctype: Reference
module: str
function: MstrRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrRestore"
---

# MstrRestore

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

> Restore a String Reader context from disk.

## Syntax

```c
AIL_ID MstrRestore(
    AIL_CONST_TEXT_PTR FileName,     //in
    AIL_ID             SysId,        //in
    AIL_INT64          ControlFlag,  //in
    AIL_ID *           ContextIdPtr  //out
)
```

## Description

This function restores a String Reader context that was previously saved to a file, using [`MstrSave`](../../Reference/str/MstrSave.md) or [`MstrStream`](../../Reference/str/MstrStream.md). This function restores all of the String Reader context's settings (including font and string model settings) that were in effect when the String Reader context was saved. A restored String Reader context is not preprocessed; therefore, you must call [`MstrPreprocess`](../../Reference/str/MstrPreprocess.md) before performing a read with [`MstrRead`](../../Reference/str/MstrRead.md).

To use a fontless context, you must restore one of the predefined context files distributed with Aurora Imaging Library, which are located in the installed Contexts folder (for example, _"C:\Program Files\Aurora Imaging Library\Contexts\FONTLESS_ANPR.msr"_).

When the restored String Reader context is no longer required, release it using[`MstrFree`](../../Reference/str/MstrFree.md)unless [`M_UNIQUE_ID`](../../Reference/str/MstrRestore.md) was specified during restoration; if [`M_UNIQUE_ID`](../../Reference/str/MstrRestore.md) was specified, the smart identifier manages the restored String Reader context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the path and name of the file from which to restore the String Reader context (for example, _"C:\mydirectory\myfile"_). The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FONTLESS_ANPR.msr"` | Specifies to restore a generic String Reader context useful to read license plates. |
| `"FONTLESS_EUROPEAN_ANPR.msr"` | Specifies to restore a String Reader context useful to read European license plates. |
| `"FONTLESS_MACHINE_PRINT.msr"` | Specifies to restore a special String Reader context that reads machine printed characters in Arial, OCR-B, or other sans-serif fonts. |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the String Reader context.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the String Reader context identifier or specifies the data type that the function should use to return the String Reader context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated String Reader context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated String Reader context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_STR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the String Reader context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored String Reader context.

If restoration fails, [`M_NULL`](../../Reference/str/MstrRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the String Reader context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_STR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/str/MstrRestore.md) was specified).
