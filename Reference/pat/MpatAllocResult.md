---
doctype: Reference
module: pat
function: MpatAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / pat / MpatAllocResult"
---

# MpatAllocResult

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

> Allocate a Pattern Matching result buffer.

## Syntax

```c
AIL_ID MpatAllocResult(
    AIL_ID    SysId,          //in
    AIL_INT64 ControlFlag,    //in
    AIL_ID *  ResultPatIdPtr  //out
)
```

## Description

This function allocates a Pattern Matching result buffer, on the specified system, to store results obtained from an [`MpatFind`](../../Reference/pat/MpatFind.md) operation.

When the Pattern Matching result buffer is no longer required, release it using[`MpatFree`](../../Reference/pat/MpatFree.md)unless [`M_UNIQUE_ID`](../../Reference/pat/MpatAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/pat/MpatAllocResult.md) was specified, the smart identifier manages the Pattern Matching result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the result buffer.

*For the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ResultPatIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Pattern Matching result buffer identifier or specifies the data type that the function should use to return the Pattern Matching result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Pattern Matching result
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Pattern Matching result
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_PAT_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Pattern Matching result
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated Pattern Matching result buffer.

If allocation fails, [`M_NULL`](../../Reference/pat/MpatAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Pattern Matching result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_PAT_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/pat/MpatAllocResult.md) was specified).
