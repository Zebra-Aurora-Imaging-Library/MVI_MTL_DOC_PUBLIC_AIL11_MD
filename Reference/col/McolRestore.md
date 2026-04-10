---
doctype: Reference
module: col
function: McolRestore
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolRestore"
---

# McolRestore

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

> Restore a color matching or relative color calibration context from disk.

## Syntax

```c
AIL_ID McolRestore(
    AIL_CONST_TEXT_PTR FileName,     //in
    AIL_ID             SysId,        //in
    AIL_INT64          ControlFlag,  //in
    AIL_ID *           ContextIdPtr  //out
)
```

## Description

This function restores a color matching or a relative color calibration context that was previously saved to a file, using [`McolSave`](../../Reference/col/McolSave.md) or [`McolStream`](../../Reference/col/McolStream.md). This function restores all of the context's settings (and color-samples) that were in effect when the context was saved. A restored context is not preprocessed; therefore, you must call [`McolPreprocess`](../../Reference/col/McolPreprocess.md) before calling [`McolMatch`](../../Reference/col/McolMatch.md) (color matching) or [`McolTransform`](../../Reference/col/McolTransform.md) (relative color calibration).

When the restored color matching or relative color calibration is no longer required, release it using[`McolFree`](../../Reference/col/McolFree.md)unless [`M_UNIQUE_ID`](../../Reference/col/McolRestore.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/col/McolRestore.md) was specified, the smart identifier manages the context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the context. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). For easier use with other Aurora Imaging software products, files for color matching contexts should use the MCOL file extension. Files for relative color calibration contexts should use the MCCC file extension.

To specify the file on the remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the context. This parameter should be set to one of the following values:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the color matching context or relative calibration context identifier or specifies the data type that the function should use to return the context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated color matching or relative color
                              calibration context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated color matching or relative color
                              calibration context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_COL_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the color matching or relative color
                              calibration context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the color matching context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored color matching context.

If allocation fails, [`M_NULL`](../../Reference/col/McolRestore.md) is written as the identifier. |
| `Address in which to write the relative color calibration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored relative color calibration context.

If allocation fails, [`M_NULL`](../../Reference/col/McolRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the color matching context or relative color calibration context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_COL_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/col/McolRestore.md) was specified).
