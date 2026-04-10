---
doctype: Reference
module: cal
function: McalAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalAllocResult"
---

# McalAllocResult

> Allocate a calculate hand-eye result buffer.

## Syntax

```c
AIL_ID McalAllocResult(
    AIL_ID    SysId,          //in
    AIL_INT64 ResultType,     //in
    AIL_INT64 ControlFlag,    //in
    AIL_ID *  ResultCalIdPtr  //out
)
```

## Description

This function allocates a calculate hand-eye result buffer, on the specified system, to store results from an [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) operation.

When the calculate hand-eye result buffer is no longer required, release it using[`McalFree`](../../Reference/cal/McalFree.md)unless [`M_UNIQUE_ID`](../../Reference/cal/McalAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/cal/McalAllocResult.md) was specified, the smart identifier manages the calculate hand-eye result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the result buffer. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result buffer to allocate.

*For specifying the type of result buffer to allocate*

| Value | Description |
| --- | --- |
| `M_CALCULATE_HAND_EYE_RESULT` | Specifies to allocate a calculate hand-eye result buffer used to store [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) results.

> **Note:** To retrieve the transformation matrices that are calculated using [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md), use [`McalCopyResult`](../../Reference/cal/McalCopyResult.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ResultCalIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the calculate hand-eye result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated calculate hand-eye result buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated calculate hand-eye result buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_CAL_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the calculate hand-eye result buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the calculate hand-eye result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated calculate hand-eye result buffer.

If allocation fails, [`M_NULL`](../../Reference/cal/McalAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the calculate hand-eye result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_CAL_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/cal/McalAllocResult.md) was specified).
