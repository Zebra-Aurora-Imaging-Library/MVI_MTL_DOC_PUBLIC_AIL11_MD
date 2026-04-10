---
doctype: Reference
module: bead
function: MbeadRestore
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadRestore"
---

# MbeadRestore

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

> Restore a bead context from disk.

## Syntax

```c
AIL_ID MbeadRestore(
    AIL_CONST_TEXT_PTR FileName,         //in
    AIL_ID             SysId,            //in
    AIL_INT64          ControlFlag,      //in
    AIL_ID *           ContextBeadIdPtr  //out
)
```

## Description

This function restores a bead context that was previously saved to a file, using [`MbeadSave`](../../Reference/bead/MbeadSave.md) or [`MbeadStream`](../../Reference/bead/MbeadStream.md). This function restores all of the bead context's settings that were in effect when it was saved. If the bead context was trained when it was saved, the training information will be restored. You can use [`MbeadInquire`](../../Reference/bead/MbeadInquire.md) with [`M_STATUS`](../../Reference/bead/MbeadInquire.md) to inquire the training status of the bead context after calling this function. If the training status is [`M_COMPLETE`](../../Reference/bead/MbeadInquire.md), the training information was restored; otherwise, you must call [`MbeadTrain`](../../Reference/bead/MbeadTrain.md) before performing a verification with [`MbeadVerify`](../../Reference/bead/MbeadVerify.md).

If you had associated a camera calibration context with the training image before saving, the camera calibration context is automatically restored from the same file as the bead context. The camera calibration cannot be managed independently from the bead context. When the bead context is freed, the camera calibration is automatically freed as well.

When the restored bead context is no longer required, release it using[`MbeadFree`](../../Reference/bead/MbeadFree.md)unless [`M_UNIQUE_ID`](../../Reference/bead/MbeadRestore.md) was specified during restoration; if [`M_UNIQUE_ID`](../../Reference/bead/MbeadRestore.md) was specified, the smart identifier manages the bead context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the bead context. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, bead context files have a MBEAD file extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to restore the bead context. This parameter should be set to one of the following values:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextBeadIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the bead context identifier or specifies the data type that the function should use to return the bead context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated bead context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated bead context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BEAD_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the bead context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored bead context.

If restoration fails, [`M_NULL`](../../Reference/bead/MbeadRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the bead context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BEAD_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/bead/MbeadRestore.md) was specified).
