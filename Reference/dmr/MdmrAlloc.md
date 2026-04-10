---
doctype: Reference
module: dmr
function: MdmrAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrAlloc"
---

# MdmrAlloc

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

> Allocate a SureDotOCR context.

## Syntax

```c
AIL_ID MdmrAlloc(
    AIL_ID    SysId,           //in
    AIL_INT64 ContextType,     //in
    AIL_INT64 ControlFlag,     //in
    AIL_ID *  ContextDmrIdPtr  //out
)
```

## Description

This function allocates a SureDotOCR context on the specified system. A SureDotOCR context contains information required to use the Aurora Imaging Library SureDotOCR functions, which are prefixed with Mdmr; these functions allow you to read strings of dot-matrix text. Global settings, fonts, and string models are all held in a SureDotOCR context; use [`MdmrControl`](../../Reference/dmr/MdmrControl.md), [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md), and [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) to set these up, respectively. Use [`MdmrRead`](../../Reference/dmr/MdmrRead.md) to perform the actual read operation. To read successfully, a SureDotOCR context must have at least one font and one string model.

A SureDotOCR context internally handles characters in Unicode.

When the SureDotOCR context is no longer required, release it using[`MdmrFree`](../../Reference/dmr/MdmrFree.md)unless [`M_UNIQUE_ID`](../../Reference/dmr/MdmrAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/dmr/MdmrAlloc.md) was specified, the smart identifier manages the SureDotOCR context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the SureDotOCR context. Set this parameter to one of the values below:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `Aurora Imaging Library system identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of context to allocate. Set this parameter to the value below:

*For specifying the type of context to allocate*

| Value | Description |
| --- | --- |
| `M_DOT_MATRIX` | Specifies to allocate an Aurora Imaging Library SureDotOCR context. |

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

If allocation fails, [`M_NULL`](../../Reference/dmr/MdmrAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the SureDotOCR context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_DMR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/dmr/MdmrAlloc.md) was specified).
