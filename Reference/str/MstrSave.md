---
doctype: Reference
module: str
function: MstrSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrSave"
---

# MstrSave

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

> Save a String Reader context to a file.

## Syntax

```c
void MstrSave(
    AIL_CONST_TEXT_PTR FileName,    //in
    AIL_ID             ContextId,   //in
    AIL_INT64          ControlFlag  //in
)
```

## Description

This function saves all the information about the previously allocated String Reader context to disk, including all of the individual font and string settings. This information can be reloaded, using [`MstrRestore`](../../Reference/str/MstrRestore.md) or [`MstrStream`](../../Reference/str/MstrStream.md).

> **Note:** When you save the String Reader context, the preprocessing changes are not saved. Upon restoration, you must preprocess the context again.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the String Reader context. It is recommended that you use the MSR file extension for easier use with other Aurora Imaging Library software products. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, String Reader context files have an MSR extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the String Reader context to save.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
