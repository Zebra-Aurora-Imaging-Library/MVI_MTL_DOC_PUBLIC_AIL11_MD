---
doctype: Reference
module: code
function: McodeSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeSave"
---

# McodeSave

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

> Save the specified code context in a file.

## Syntax

```c
void McodeSave(
    AIL_CONST_TEXT_PTR FileName,       //in
    AIL_ID             ContextCodeId,  //in
    AIL_INT64          ControlFlag     //in
)
```

## Description

This function saves a code context in a file so that it can be reloaded with [`McodeRestore`](../../Reference/code/McodeRestore.md) or [`McodeStream`](../../Reference/code/McodeStream.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the code context. It is recommended that you use the MCO file extension for easier use with other Aurora Imaging software products. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, code context files have an MCO extension.

To save the file on the remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextCodeId` *(in, AIL_ID)*

Specifies the identifier of the data buffer to save.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
