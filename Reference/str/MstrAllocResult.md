---
doctype: Reference
module: str
function: MstrAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrAllocResult"
---

# MstrAllocResult

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

> Allocate a String Reader result buffer.

## Syntax

```c
AIL_ID MstrAllocResult(
    AIL_ID    SysId,        //in
    AIL_INT64 ControlFlag,  //in
    AIL_ID *  ObjectIdPtr   //out
)
```

## Description

This function allocates a String Reader result buffer, on the specified system, to store results obtained from an [`MstrRead`](../../Reference/str/MstrRead.md) operation.

When the String Reader result buffer is no longer required, release it using[`MstrFree`](../../Reference/str/MstrFree.md)unless [`M_UNIQUE_ID`](../../Reference/str/MstrAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/str/MstrAllocResult.md) was specified, the smart identifier manages the String Reader result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the String Reader result buffer.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ObjectIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the String Reader result buffer identifier or specifies the data type that the function should use to return the String Reader result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated String Reader result
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated String Reader result
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_STR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the String Reader result
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated String Reader result buffer.

If allocation fails, [`M_NULL`](../../Reference/str/MstrAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the String Reader result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_STR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/str/MstrAllocResult.md) was specified).
