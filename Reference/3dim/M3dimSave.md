---
doctype: Reference
module: 3dim
function: M3dimSave
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimSave"
---

# M3dimSave

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

> Save a 3D image processing context to a file.

## Syntax

```c
void M3dimSave(
    AIL_CONST_TEXT_PTR FileName,       //in
    AIL_ID             Context3dimId,  //in
    AIL_INT64          ControlFlag     //in
)
```

## Description

This function saves all the information about a previously allocated 3D image processing context to disk.

To load a saved context, use either [`M3dimRestore`](../../Reference/3dim/M3dimRestore.md) or [`M3dimStream`](../../Reference/3dim/M3dimStream.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the 3D image processing context; it is recommended that you use the M3DIM file extension for easier use with other Aurora Imaging software products. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). The recommended extension is M3DIM.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `Context3dimId` *(in, AIL_ID)*

Specifies the identifier of the 3D image processing context to save. The context must have been successfully allocated (using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md)) prior to calling this function.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
