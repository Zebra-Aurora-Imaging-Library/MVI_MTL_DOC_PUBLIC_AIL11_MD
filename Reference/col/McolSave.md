---
doctype: Reference
module: col
function: McolSave
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolSave"
---

# McolSave

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

> Save a color matching or relative color calibration context to a file.

## Syntax

```c
void McolSave(
    AIL_CONST_TEXT_PTR FileName,    //in
    AIL_ID             ContextId,   //in
    AIL_INT64          ControlFlag  //in
)
```

## Description

This function saves to disk all the information about the previously allocated color matching or relative color calibration context (and the color-samples contained therein). However, preprocessing information is not saved. You can reload the saved information, using [`McolRestore`](../../Reference/col/McolRestore.md) or [`McolStream`](../../Reference/col/McolStream.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the context. Set this parameter to one of the following values:

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). For easier use with other Aurora Imaging software products, when saving a color matching context to a file, use the MCOL file extension. When saving a relative color calibration context to a file, use the MCCC file extension.

To save the file on the remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the context to save. The context must have been previously allocated on the required system using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md) or [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
