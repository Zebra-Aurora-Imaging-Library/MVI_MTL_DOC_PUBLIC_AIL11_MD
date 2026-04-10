---
doctype: Reference
module: mod
function: MmodAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodAllocResult"
---

# MmodAllocResult

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

> Allocate a Model Finder result buffer.

## Syntax

```c
AIL_ID MmodAllocResult(
    AIL_ID    SysId,          //in
    AIL_INT64 ControlFlag,    //in
    AIL_ID *  ModResultIdPtr  //out
)
```

## Description

This function allocates a result buffer, on the specified system, to store results obtained from an [`MmodFind`](../../Reference/mod/MmodFind.md) operation.

When the Model Finder result buffer is no longer required, release it using[`MmodFree`](../../Reference/mod/MmodFree.md)unless [`M_UNIQUE_ID`](../../Reference/mod/MmodAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/mod/MmodAllocResult.md) was specified, the smart identifier manages the ModelFinder result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the result buffer.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the type of result buffer to allocate. This parameter should be set to one of the following values:

*For specifying the type of Model Finder result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of context. |
| `M_SHAPE_CIRCLE` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md)type of context. |
| `M_SHAPE_ELLIPSE` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md)type of context. |
| `M_SHAPE_RECTANGLE` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md)type of context. |
| `M_SHAPE_SEGMENT` | Specifies that the Model Finder result buffer can hold results from a search with an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md)type of context. |

### `ModResultIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Model Finder result buffer identifier or specifies the data type that the function should use to return the Model Finder result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Model Finder context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Model Finder context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_MOD_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Model Finder context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the geometric search Model Finder result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated geometric search Model Finder result buffer.

If allocation fails, [`M_NULL`](../../Reference/mod/MmodAllocResult.md) is written as the identifier. |
| `Address in which to write the shape search Model Finder result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated shape search Model Finder result buffer.

If allocation fails, [`M_NULL`](../../Reference/mod/MmodAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Model Finder result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_MOD_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/mod/MmodAllocResult.md) was specified).
