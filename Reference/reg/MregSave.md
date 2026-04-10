---
doctype: Reference
module: reg
function: MregSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregSave"
---

# MregSave

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

> Save a registration context or an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer to a file.

## Syntax

```c
void MregSave(
    AIL_CONST_TEXT_PTR FileName,           //in
    AIL_ID             ContextOrResultId,  //in
    AIL_INT64          ControlFlag         //in
)
```

## Description

This function saves all the information about a previously allocated registration context or an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. This information can be reloaded, using [`MregRestore`](../../Reference/reg/MregRestore.md) or [`MregStream`](../../Reference/reg/MregStream.md). Saving an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer is useful if you want to reuse the registration results (you cannot save other types of registration result buffers).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the registration context or the contents of the registration result buffer. It is recommended that you use the MREG file extension for easier use with other Aurora Imaging Library software products. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, registration context files have an MREG extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextOrResultId` *(in, AIL_ID)*

Specifies either the identifier of the registration context or the [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer to save.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
