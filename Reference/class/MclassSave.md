---
doctype: Reference
module: class
function: MclassSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassSave"
---

# MclassSave

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

> Save an Aurora Imaging Library Classification context (classifier, dataset, data preparation, or training) to a file.

## Syntax

```c
void MclassSave(
    AIL_CONST_TEXT_PTR FileName,        //in
    AIL_ID             ContextClassId,  //in
    AIL_INT64          ControlFlag      //in
)
```

## Description

This function saves all the information about the previously allocated Aurora Imaging Library Classification context (classifier, dataset, data preparation, or training) to disk. This information can be reloaded, using [`MclassRestore`](../../Reference/class/MclassRestore.md) or [`MclassStream`](../../Reference/class/MclassStream.md).

When you save the context, the preprocessing changes are not saved. Upon restoration, you must preprocess the context again.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the classification context (classifier, dataset, data preparation, or training). The function internally handles the opening and closing of this file. For easier use with other Aurora Imaging software products, classifier contexts, data preparation contexts, and training context files should have an MCLASS extension. Dataset context files should have an MCLASSD extension.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the classification context file (for example,_"C:\mydirectory\WePutTheArtInArtificialIntelligence.mclass"_). Classifier context, data preparation context, and training context files should have an MCLASS extension. Dataset context files should have an MCLASSD extension.

To specify a context file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\WePutTheArtInArtificialIntelligence.mclass"`). |

### `ContextClassId` *(in, AIL_ID)*

Specifies the identifier of the classification context to save (that is, a classifier context, a dataset context, a data preparation context, or a training context).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
