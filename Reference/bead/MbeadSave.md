---
doctype: Reference
module: bead
function: MbeadSave
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadSave"
---

# MbeadSave

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

> Save a bead context to a file.

## Syntax

```c
void MbeadSave(
    AIL_CONST_TEXT_PTR FileName,       //in
    AIL_ID             ContextBeadId,  //in
    AIL_INT64          ControlFlag     //in
)
```

## Description

This function saves all the information about the previously allocated bead context to disk, including the current settings of each template, training information, and the camera calibration context associated with the training image, if any. This information can be reloaded, using [`MbeadRestore`](../../Reference/bead/MbeadRestore.md) or [`MbeadStream`](../../Reference/bead/MbeadStream.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the bead context. It is recommended that you use the MBEAD file extension for easier use with other Aurora Imaging software products. The function internally handles the opening and closing of the file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, bead context files have a MBEAD file extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextBeadId` *(in, AIL_ID)*

Specifies the identifier of the bead context to save.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
