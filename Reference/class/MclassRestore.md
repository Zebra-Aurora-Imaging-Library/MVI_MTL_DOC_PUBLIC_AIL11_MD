---
doctype: Reference
module: class
function: MclassRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassRestore"
---

# MclassRestore

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

> Restore an Aurora Imaging Library Classification context (classifier, dataset, data preparation, or training) from disk.

## Syntax

```c
AIL_ID MclassRestore(
    AIL_CONST_TEXT_PTR FileName,          //in
    AIL_ID             SysId,             //in
    AIL_INT64          ControlFlag,       //in
    AIL_ID *           ContextClassIdPtr  //out
)
```

## Description

This function restores an Aurora Imaging Library Classification context (classifier, dataset, data preparation, or training) that was previously saved to a file, using [`MclassSave`](../../Reference/class/MclassSave.md) or [`MclassStream`](../../Reference/class/MclassStream.md).

This function restores all of the context's settings that were in effect when it was saved. A restored context is not preprocessed; if the context requires preprocessing, you must call [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md) before calling [`MclassTrain`](../../Reference/class/MclassTrain.md) or [`MclassPredict`](../../Reference/class/MclassPredict.md).

Note, it is possible to restore a pretrained CNN, object detection, segmentation, or anomaly detection classifier context that Aurora Imaging has built and trained to address your specific requirements (as opposed to a predefined CNN, object detection, segmentation, anomaly detection classifier context that you have trained). For more information about having Zebra provide this service to you, [contact customer support](../../UserGuide/C00_AIL_Introduction/S06_Service_information.md).

When the restored classifier, dataset, data preparation, or training context is no longer required, release it using[`MclassFree`](../../Reference/class/MclassFree.md) unless [`M_UNIQUE_ID`](../../Reference/class/MclassRestore.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/class/MclassRestore.md) was specified, the smart identifier manages the context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the classification context (classifier, dataset, data preparation, or training). The function handles (internally) the opening and closing of the file. For easier use with other Aurora Imaging software products, classifier context, data preparation context, and training context files should have an MCLASS extension. Dataset context files should have an MCLASSD extension.

*For specifying the file name and path of the context to restore*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the classification context file (for example,_"C:\mydirectory\WePutTheArtInArtificialIntelligence.mclass"_). Classifier context, data preparation context, and training context files should have an MCLASS extension. Dataset context files should have an MCLASSD extension.

To specify a context file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\WePutTheArtInArtificialIntelligence.mclass"`). |

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to restore the classification context (classifier, dataset, data preparation, or training) to disk.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextClassIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the classifier, dataset, data preparation, or training context identifier or specifies the data type that the function should use to return the context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated classifier, dataset, data preparation,
                              or training data context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated classifier, dataset, data preparation,
                              or training data context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_CLASS_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the classifier, dataset, data preparation,
                              or training data context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the classifier context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated classifier context.

If restoration fails, [`M_NULL`](../../Reference/class/MclassRestore.md) is written as the identifier. |
| `Address in which to write the data preparation context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated data preparation context.

If restoration fails, [`M_NULL`](../../Reference/class/MclassRestore.md) is written as the identifier. |
| `Address in which to write the dataset context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated dataset context.

If restoration fails, [`M_NULL`](../../Reference/class/MclassRestore.md) is written as the identifier. |
| `Address in which to write the training context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated training context.

If restoration fails, [`M_NULL`](../../Reference/class/MclassRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the classifier context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_CLASS_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/class/MclassRestore.md) was specified).
