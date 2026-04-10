---
doctype: Reference
module: agm
function: MagmSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmSave"
---

# MagmSave

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

> Save an AGM context to a file.

## Syntax

```c
void MagmSave(
    AIL_CONST_TEXT_PTR FileName,    //in
    AIL_ID             ContextId,   //in
    AIL_INT64          ControlFlag  //in
)
```

## Description

This function saves all the information about the previously allocated AGM context to disk, including the model's current settings. This information can be reloaded, using [`MagmRestore`](../../Reference/agm/MagmRestore.md) or [`MagmStream`](../../Reference/agm/MagmStream.md). However, preprocessing changes are not saved.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the AGM context; it is recommended that you use the MAGM file extension for easier use with other Aurora Imaging software products. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, AGM context files have an MAGM extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the AGM context to save.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
