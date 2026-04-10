---
doctype: Reference
module: pat
function: MpatSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / pat / MpatSave"
---

# MpatSave

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

> Save a Pattern Matching context to disk.

## Syntax

```c
void MpatSave(
    AIL_CONST_TEXT_PTR FileName,      //in
    AIL_ID             ContextPatId,  //in
    AIL_INT64          ControlFlag    //in
)
```

## Description

This function saves all the information about the previously allocated Pattern Matching context to disk, including all of the Pattern Matching context's current search parameters values. Later, this information can be reloaded, using [`MpatRestore`](../../Reference/pat/MpatRestore.md) or [`MpatStream`](../../Reference/pat/MpatStream.md).

All of the Pattern Matching context settings that are enabled or were in effect are saved. A loaded or restored Pattern Matching context is not preprocessed.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the model. It is recommended that you use the MPAT file extension for easier use with other Zebra Aurora software products. The function handles (internally) the opening and closing of the file. If the file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, Pattern Matching context files have an MPAT extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextPatId` *(in, AIL_ID)*

Specifies the identifier of the Pattern Matching context to save.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
