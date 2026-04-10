---
doctype: Reference
module: dmr
function: MdmrSave
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrSave"
---

# MdmrSave

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

> Save a SureDotOCR context to a file.

## Syntax

```c
void MdmrSave(
    AIL_CONST_TEXT_PTR FileName,      //in
    AIL_ID             ContextDmrId,  //in
    AIL_INT64          ControlFlag    //in
)
```

## Description

This function saves all the information about the previously allocated SureDotOCR context to disk, including all of the individual font and string model settings. This information can be reloaded, using [`MdmrRestore`](../../Reference/dmr/MdmrRestore.md) or [`MdmrStream`](../../Reference/dmr/MdmrStream.md).

When you save the context, the preprocessing changes are not saved. Upon restoration, you must preprocess the context again.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the SureDotOCR context. The function internally handles the opening and closing of this file. For easier use with other Aurora Imaging Library software products, files for SureDotOCR contexts should use the MDMR file extension. Predefined SureDotOCR context files are typically distributed with Aurora Imaging Library and can be found in the installed Contexts folder (for example, _"C:\Program Files\Aurora Imaging Library\11\Contexts\"_). If you specify a file that already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the SureDotOCR context file (for example,_"C:\mydirectory\MyContext.mdmr"_). Typically, SureDotOCR context files have an MDMR extension.

To specify a context file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MyContext.mdmr"`). |

### `ContextDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR context to save.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
