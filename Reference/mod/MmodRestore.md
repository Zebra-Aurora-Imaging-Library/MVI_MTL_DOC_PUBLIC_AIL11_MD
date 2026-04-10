---
doctype: Reference
module: mod
function: MmodRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodRestore"
---

# MmodRestore

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

> Restore a Model Finder context from disk.

## Syntax

```c
AIL_ID MmodRestore(
    AIL_CONST_TEXT_PTR FileName,     //in
    AIL_ID             SysId,        //in
    AIL_INT64          ControlFlag,  //in
    AIL_ID *           ContextIdPtr  //out
)
```

## Description

This function restores a Model Finder context that was previously saved to a file, using [`MmodSave`](../../Reference/mod/MmodSave.md) or [`MmodStream`](../../Reference/mod/MmodStream.md). This function restores all of the Model Finder context's settings that were in effect when the Model Finder context was saved. A restored Model Finder context is not preprocessed, therefore you must call [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md) before performing a search with [`MmodFind`](../../Reference/mod/MmodFind.md).

If you had associated camera calibration contexts with the models in your Model Finder context and you did not save them with the context, you must re-associate the camera calibration contexts, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md). Moreover, if you had previously set drawing control types, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_...`](../../Reference/gra/MgraControl.md), then you must reset them since they were not saved with the context.

When the restored Model Finder context is no longer required, release it using[`MmodFree`](../../Reference/mod/MmodFree.md)unless [`M_UNIQUE_ID`](../../Reference/mod/MmodRestore.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/mod/MmodRestore.md) was specified, the smart identifier manages the restored Model Finder context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the Model Finder context. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, Model Finder context files have an MMF extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the Model Finder context.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to restore, from the Model Finder context, the camera calibration contexts associated with the models.

*For specifying whether to restore the camera calibration contexts associated with the models*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the camera calibration contexts are not restored. |
| `M_WITH_CALIBRATION` | Specifies that the camera calibration contexts are restored. In this case, the camera calibration contexts must have been previously saved with the context, using [`MmodSave`](../../Reference/mod/MmodSave.md) or [`MmodStream`](../../Reference/mod/MmodStream.md) with [`M_WITH_CALIBRATION`](../../Reference/mod/MmodSave.md). Note that the camera calibration contexts associated with synthetic models are not saved or restored.

The camera calibration information is restored from the same file as the Model Finder context. The camera calibration cannot be managed independently from the Model Finder context. When the Model Finder context is freed, the camera calibration is automatically freed as well. |

### `ContextIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Model Finder context identifier or specifies the data type that the function should use to return the Model Finder context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Model Finder context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Model Finder context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_MOD_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Model Finder context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored Model Finder context.

If restoration fails, [`M_NULL`](../../Reference/mod/MmodRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Model Finder context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_MOD_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/mod/MmodRestore.md) was specified).
