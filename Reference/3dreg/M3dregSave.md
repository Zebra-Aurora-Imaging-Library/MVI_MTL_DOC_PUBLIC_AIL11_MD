---
doctype: Reference
module: 3dreg
function: M3dregSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregSave"
---

# M3dregSave

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

> Save a 3D registration context or 3D registration result buffer to a file.

## Syntax

```c
void M3dregSave(
    AIL_CONST_TEXT_PTR FileName,                //in
    AIL_ID             ContextOrResult3dregId,  //in
    AIL_INT64          ControlFlag              //in
)
```

## Description

This function saves all the information about a previously allocated 3D registration context or 3D registration result buffer to disk.

To load a saved context or result, use either [`M3dregRestore`](../../Reference/3dreg/M3dregRestore.md) or [`M3dregStream`](../../Reference/3dreg/M3dregStream.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the 3D registration context or 3D registration result buffer. It is recommended that you use the M3DREG file extension for easier use with other Aurora Imaging software products. The function handles (internally) the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). The recommended extension is M3DREG.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextOrResult3dregId` *(in, AIL_ID)*

Specifies the identifier of the 3D registration context or 3D registration result buffer to save. These must have been successfully allocated (using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) or [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md)) prior to calling this function.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
