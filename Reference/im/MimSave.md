---
doctype: Reference
module: im
function: MimSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimSave"
---

# MimSave

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

> Save an image processing context to a file.

## Syntax

```c
void MimSave(
    AIL_CONST_TEXT_PTR FileName,     //in
    AIL_ID             ContextImId,  //in
    AIL_INT64          ControlFlag   //in
)
```

## Description

This function saves an image processing context to disk.

To load the saved image processing context, use either [`MimRestore`](../../Reference/im/MimRestore.md) or [`MimStream`](../../Reference/im/MimStream.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the image processing context; it is recommended that you use the MIC file extension for easier use with other Zebra Aurora software products. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, image processing context files have an MIC extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextImId` *(in, AIL_ID)*

Specifies the identifier of the image processing context to save. The image processing context must have been previously allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
