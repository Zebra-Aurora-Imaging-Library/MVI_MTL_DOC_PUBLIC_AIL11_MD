---
doctype: Reference
module: 3dmap
function: M3dmapSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapSave"
---

# M3dmapSave

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

> Save a 3D object to a file.

## Syntax

```c
void M3dmapSave(
    AIL_CONST_TEXT_PTR FileName,    //in
    AIL_ID             M3dmapId,    //in
    AIL_INT64          ControlFlag  //in
)
```

## Description

This function saves to disk all the information about the previously allocated 3D object. You can save a 3D reconstruction context or 3D alignment context of types [`M_LASER`](../../Reference/3dmap/M3dmapAlloc.md) or [`M_ALIGN_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md). You can also save a 3D reconstruction result buffer or 3D alignment result buffer of types [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) or [`M_ALIGN_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md).

For a 3D reconstruction context or 3D alignment context, all camera or 3D profile sensor calibration information is saved, and the context will be ready to use on reload. You can reload the saved information, using[`M3dmapRestore`](../../Reference/3dmap/M3dmapRestore.md) or [`M3dmapStream`](../../Reference/3dmap/M3dmapStream.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and directory of the file in which to save the 3D object. The recommended extension is M3D.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, 3D object files have an M3D file extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `M3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D object to save.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
