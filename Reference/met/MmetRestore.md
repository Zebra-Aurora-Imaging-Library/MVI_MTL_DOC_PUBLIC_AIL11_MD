---
doctype: Reference
module: met
function: MmetRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetRestore"
---

# MmetRestore

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

> Restore a metrology context from disk.

## Syntax

```c
AIL_ID MmetRestore(
    AIL_CONST_TEXT_PTR FileName,     //in
    AIL_ID             SysId,        //in
    AIL_INT64          ControlFlag,  //in
    AIL_ID *           ContextIdPtr  //out
)
```

## Description

This function restores a metrology context that was previously saved to a file, using [`MmetSave`](../../Reference/met/MmetSave.md) or [`MmetStream`](../../Reference/met/MmetStream.md). This function restores all the metrology context's settings and names that were in effect when the metrology context was saved.

If you had associated a camera calibration context with the template reference in your metrology context and you did not save it with the context, you must re-associate the camera calibration context, using [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/met/MmetControl.md). Moreover, if you had previously set drawing control types, using [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_...`](../../Reference/gra/MgraControl.md), you must reset them since they were not saved with the context.

When the restored metrology context is no longer required, release it using[`MmetFree`](../../Reference/met/MmetFree.md)unless [`M_UNIQUE_ID`](../../Reference/met/MmetRestore.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/met/MmetRestore.md) was specified, the smart identifier manages the restored metrology context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the metrology context. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, metrology contexts have an MET extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the metrology context.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to restore, from the metrology context, the camera calibration context associated with the template reference.

*For specifying whether to restore the camera calibration context associated with the template reference*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the camera calibration context is not restored. |
| `M_WITH_CALIBRATION` | Specifies that the camera calibration context is restored. In this case, the camera calibration context must have been previously saved with the context, using [`MmetSave`](../../Reference/met/MmetSave.md) or [`MmetStream`](../../Reference/met/MmetStream.md) with [`M_WITH_CALIBRATION`](../../Reference/met/MmetSave.md).

The camera calibration information is restored from the same file as the metrology context. The camera calibration cannot be managed independently from the metrology context. When the metrology context is freed, the camera calibration is automatically freed as well. |

### `ContextIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the metrology context identifier or specifies the data type that the function should use to return the metrology context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated metrology context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated metrology context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_MET_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the metrology context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored metrology context.

If restoration fails, [`M_NULL`](../../Reference/met/MmetRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the metrology context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_MET_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/met/MmetRestore.md) was specified).
