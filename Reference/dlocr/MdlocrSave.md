---
doctype: Reference
module: dlocr
function: MdlocrSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrSave"
---

# MdlocrSave

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

> Save a Deep Learning OCR read or fine-tuning context to a file.

## Syntax

```c
void MdlocrSave(
    AIL_CONST_TEXT_PTR FileName,        //in
    AIL_ID             ContextDlocrId,  //in
    AIL_INT64          ControlFlag      //in
)
```

## Description

This function saves all the information about the previously allocated Deep Learning OCR read or fine-tuning context to disk, including all of its global settings and string model-specific positional constraints and settings. This information can be reloaded, using [`MdlocrRestore`](../../Reference/dlocr/MdlocrRestore.md) or [`MdlocrStream`](../../Reference/dlocr/MdlocrStream.md).

When you save the context, the preprocessing changes are not saved. Upon restoration, you must preprocess the context again.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the Deep Learning OCR read or fine-tuning context. The function internally handles the opening and closing of this file. For easier use with other Aurora Imaging Library software products, files for Deep Learning OCR read or fine-tuning contexts should use the MDLOCR file extension. If you specify a file that already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the Deep Learning OCR read or fine-tuning context file (for example,_"C:\mydirectory\MyContext.mdlocr"_). Typically, Deep Learning OCR context files have an MDLOCR extension.

To specify a context file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MyContext.mdlocr"`). |

### `ContextDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR read or fine-tuning context to save.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
