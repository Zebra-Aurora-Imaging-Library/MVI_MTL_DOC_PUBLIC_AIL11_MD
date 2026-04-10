---
doctype: Reference
module: cal
function: McalRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalRestore"
---

# McalRestore

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

> Restore a camera or 3D draw calibration context from disk.

## Syntax

```c
AIL_ID McalRestore(
    AIL_CONST_TEXT_PTR FileName,         //in
    AIL_ID             SysId,            //in
    AIL_INT64          ControlFlag,      //in
    AIL_ID *           CalibrationIdPtr  //out
)
```

## Description

This function restores a camera or 3D draw calibration context that was previously saved to a file, using [`McalSave`](../../Reference/cal/McalSave.md) or [`McalStream`](../../Reference/cal/McalStream.md), and assigns it an Aurora Imaging Library identifier.

This function can also restore the camera calibration context associated with an Aurora Imaging Library image buffer (MIM) that was previously saved to a file, using [`MbufExport`](../../Reference/buf/MbufExport.md) with [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md) + [`M_WITH_CALIBRATION`](../../Reference/buf/MbufExport.md), and assign the context an Aurora Imaging Library identifier. If you then associate this restored camera calibration context to the restored image buffer, the calibrated image will be in the same state as the previously saved image (at the moment it was saved). For more information, see [Saving and reloading a calibrated image](../../UserGuide/C28_Calibration/S08_Calibration_propagation.md).

When the restored camera or 3D draw calibration context, is no longer required, release it using[`McalFree`](../../Reference/cal/McalFree.md)unless [`M_UNIQUE_ID`](../../Reference/cal/McalRestore.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/cal/McalRestore.md) was specified, the smart identifier manages the camera or 3D draw calibration context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the camera or 3D draw calibration context. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Open** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, camera and 3D draw calibration context files have an MCA extension.

To specify a file on the remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `SysId` *(in, AIL_ID)*

Specifies the system on which to restore the camera or 3D draw calibration context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter must be set to the following value:

*For specifying the function's control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Sets the function's control flag to the default. |

### `CalibrationIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write a camera or 3D draw calibration context identifier or specifies the data type that the function should use to return the camera or 3D draw calibration context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated camera calibration context, or 3D draw
                              calibration context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated camera calibration context, or 3D draw
                              calibration context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_CAL_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the camera calibration context, or 3D draw
                              calibration context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the 3D draw calibration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored 3D draw calibration context.

If allocation fails, [`M_NULL`](../../Reference/cal/McalRestore.md) is written as the identifier. |
| `Address in which to write the camera calibration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the restored camera calibration context.

If allocation fails, [`M_NULL`](../../Reference/cal/McalRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the camera calibration context, or 3D draw calibration context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_CAL_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/cal/McalRestore.md) was specified).
