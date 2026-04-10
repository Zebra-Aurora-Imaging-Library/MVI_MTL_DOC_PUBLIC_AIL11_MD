---
doctype: Reference
module: blob
function: MblobAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / blob / MblobAlloc"
---

# MblobAlloc

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

> Allocate a blob analysis context.

## Syntax

```c
AIL_ID MblobAlloc(
    AIL_ID    SysId,            //in
    AIL_INT64 ContextType,      //in
    AIL_INT64 ControlFlag,      //in
    AIL_ID *  ContextBlobIdPtr  //out
)
```

## Description

This function allocates a blob analysis context. A blob analysis context contains all the information necessary to perform an [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) operation, including global processing settings, and the blob features to calculate. Immediately after allocation, the context has only a few features enabled for calculation (for example, a blob's area). Use [`MblobControl`](../../Reference/blob/MblobControl.md) to enable addition features for calculation.

When the blob analysis context is no longer required, release it using[`MblobFree`](../../Reference/blob/MblobFree.md)unless [`M_UNIQUE_ID`](../../Reference/blob/MblobAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/blob/MblobAlloc.md) was specified, the smart identifier manages the blob analysis context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the context.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ContextBlobIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the blob analysis context identifier or specifies the data type that the function should use to return the blob analysis context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated blob analysis context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated blob analysis context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BLOB_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the blob analysis context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated blob analysis context.

If allocation fails, [`M_NULL`](../../Reference/blob/MblobAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the blob analysis context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BLOB_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/blob/MblobAlloc.md) was specified).
