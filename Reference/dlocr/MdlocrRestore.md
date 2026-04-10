---
doctype: Reference
module: dlocr
function: MdlocrRestore
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrRestore"
---

# MdlocrRestore

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

> Restore a Deep Learning OCR read or fine-tuning context from disk.

## Syntax

```c
AIL_ID MdlocrRestore(
    AIL_CONST_TEXT_PTR FileName,          //in
    AIL_ID             SysId,             //in
    AIL_INT64          ControlFlag,       //in
    AIL_ID *           ContextDlocrIdPtr  //out
)
```

## Description

This function restores a Deep Learning OCR read or fine-tuning context that was previously saved to a file, using [`MdlocrSave`](../../Reference/dlocr/MdlocrSave.md) or [`MdlocrStream`](../../Reference/dlocr/MdlocrStream.md). This function restores all of the context's settings (including string model-specific positional constraints and settings) that were in effect when the context was saved. A restored context is not preprocessed; therefore, you must call [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md) before performing a read using [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md).

When the restored Deep Learning OCR context is no longer required, release it using[`MdlocrFree`](../../Reference/dlocr/MdlocrFree.md)unless [`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrRestore.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrRestore.md) was specified, the smart identifier manages the restored Deep Learning OCR context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the Deep Learning OCR read or fine-tuning context. The function handles (internally) the opening and closing of the file. For easier use with other Aurora Imaging Library software products, files for Deep Learning OCR contexts should use the MDLOCR file extension.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the Deep Learning OCR context file (for example,_"C:\mydirectory\MyContext.mdlocr"_). Typically, Deep Learning OCR context files have an MDLOCR extension.

To specify a context file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MyContext.mdlocr"`). |

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to restore the Deep Learning OCR read or fine-tuning context.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextDlocrIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Deep Learning OCR context identifier or specifies the data type that the function should use to return the Deep Learning OCR context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Deep Learning OCR
                              context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Deep Learning OCR
                              context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_DLOCR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Deep Learning OCR
                              context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the fine-tuning context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated Deep Learning OCR fine-tuning context.

If restoration fails, [`M_NULL`](../../Reference/dlocr/MdlocrRestore.md) is written as the identifier. |
| `Address in which to write the read context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated Deep Learning OCR read context.

If restoration fails, [`M_NULL`](../../Reference/dlocr/MdlocrRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Deep Learning OCR context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_DLOCR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrRestore.md) was specified).
