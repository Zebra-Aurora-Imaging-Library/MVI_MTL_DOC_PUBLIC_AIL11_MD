---
doctype: Reference
module: agm
function: MagmAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmAllocResult"
---

# MagmAllocResult

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

> Allocate a find or train AGM result buffer.

## Syntax

```c
AIL_ID MagmAllocResult(
    AIL_ID    SysId,          //in
    AIL_INT64 ResultType,     //in
    AIL_INT64 ControlFlag,    //in
    AIL_ID *  ResultAgmIdPtr  //out
)
```

## Description

This function allocates a find or train AGM result buffer on the specified system. A find AGM result buffer stores results obtained from an [`MagmFind`](../../Reference/agm/MagmFind.md) operation, while a train AGM result buffer stores results obtained from an [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation.

When the AGM result buffer is no longer required, release it using[`MagmFree`](../../Reference/agm/MagmFree.md)unless [`M_UNIQUE_ID`](../../Reference/agm/MagmAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/agm/MagmAllocResult.md) was specified, the smart identifier manages the AGM result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the result buffer.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of AGM result buffer. This parameter must be set to one of the following values:

*For the type of results buffer*

| Value | Description |
| --- | --- |
| `M_GLOBAL_EDGE_BASED_FIND_RESULT` | Specifies to allocate a find AGM result buffer. This result buffer holds the results produced from calling [`MagmFind`](../../Reference/agm/MagmFind.md). |
| `M_GLOBAL_EDGE_BASED_TRAIN_RESULT` | Specifies to allocate a train AGM result buffer. This result buffer holds the results produced from calling [`MagmTrain`](../../Reference/agm/MagmTrain.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ResultAgmIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the AGM result buffer identifier. Since the [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md)function also returns the AGM result buffer identifier, you can set this parameter to [`M_NULL`](../../Reference/agm/MagmAllocResult.md).

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated AGM result buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated AGM result buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_AGM_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the AGM result buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the find AGM result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated find AGM result buffer.

If allocation fails, [`M_NULL`](../../Reference/agm/MagmAllocResult.md) is written as the identifier. |
| `Address in which to write the train AGM result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated train AGM result buffer.

If allocation fails, [`M_NULL`](../../Reference/agm/MagmAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the AGM result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_AGM_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/agm/MagmAllocResult.md) was specified).
