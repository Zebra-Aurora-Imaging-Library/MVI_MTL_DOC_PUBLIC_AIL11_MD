---
doctype: Reference
module: dmr
function: MdmrRestore
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrRestore"
---

# MdmrRestore

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

> Restore a SureDotOCR context from disk.

## Syntax

```c
AIL_ID MdmrRestore(
    AIL_CONST_TEXT_PTR FileName,        //in
    AIL_ID             SysId,           //in
    AIL_INT64          ControlFlag,     //in
    AIL_ID *           ContextDmrIdPtr  //out
)
```

## Description

This function restores a SureDotOCR context that was previously saved to a file, using [`MdmrSave`](../../Reference/dmr/MdmrSave.md) or [`MdmrStream`](../../Reference/dmr/MdmrStream.md). This function restores all of the context's settings (including font and string model settings) that were in effect when the context was saved. A restored context is not preprocessed; therefore, you must call [`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md) before performing a read with [`MdmrRead`](../../Reference/dmr/MdmrRead.md).

When the restored SureDotOCR context is no longer required, release it using[`MdmrFree`](../../Reference/dmr/MdmrFree.md)unless [`M_UNIQUE_ID`](../../Reference/dmr/MdmrRestore.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/dmr/MdmrRestore.md) was specified, the smart identifier manages the restored SureDotOCR context's lifetime and you must not manually free it.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to restore the SureDotOCR context. The function handles (internally) the opening and closing of the file. For easier use with other Aurora Imaging Library software products, files for SureDotOCR contexts should use the MDMR file extension. Predefined SureDotOCR context files are typically distributed with Aurora Imaging Library and can be found in the installed Contexts folder (for example, _C:\Program Files\Aurora Imaging Library\11\Contexts\"_).

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the SureDotOCR context file (for example,_"C:\mydirectory\MyContext.mdmr"_). Typically, SureDotOCR context files have an MDMR extension.

To specify a context file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MyContext.mdmr"`). |

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to restore the SureDotOCR context.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextDmrIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the SureDotOCR context identifier or specifies the data type that the function should use to return the SureDotOCR context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated SureDotOCR context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated SureDotOCR context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_DMR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the SureDotOCR context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated SureDot OCR context.

If restoration fails, [`M_NULL`](../../Reference/dmr/MdmrRestore.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the SureDotOCR context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_DMR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/dmr/MdmrRestore.md) was specified).
