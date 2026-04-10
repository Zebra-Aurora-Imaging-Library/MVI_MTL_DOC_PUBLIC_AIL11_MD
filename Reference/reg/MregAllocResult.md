---
doctype: Reference
module: reg
function: MregAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregAllocResult"
---

# MregAllocResult

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

> Allocate a registration result buffer.

## Syntax

```c
AIL_ID MregAllocResult(
    AIL_ID    SystemId,     //in
    AIL_INT64 ControlFlag,  //in
    AIL_ID *  ResultIdPtr   //out
)
```

## Description

This function allocates a registration result buffer, on the specified system, to store results obtained from an [`MregCalculate`](../../Reference/reg/MregCalculate.md) operation.

When the registration result buffer is no longer required, release it using[`MregFree`](../../Reference/reg/MregFree.md)unless [`M_UNIQUE_ID`](../../Reference/reg/MregAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/reg/MregAllocResult.md) was specified, the smart identifier manages the registration result buffer's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the system on which to allocate the result buffer. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the type of result buffer to allocate. This parameter should be set to one of the following values.

*For specifying the type of result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DEPTH_FROM_FOCUS_RESULT` | Specifies that the result buffer can hold results from a depth-from-focus operation. |
| `M_EXTENDED_DEPTH_OF_FIELD_RESULT` | Specifies that the result buffer can hold results from an extended depth of field (EDoF) operation. |
| `M_HIGH_DYNAMIC_RANGE_RESULT` | Specifies that the result buffer can hold results from a high dynamic range operation. |
| `M_PHOTOMETRIC_STEREO_RESULT` | Specifies that the result buffer can hold results from a photometric stereo operation. |
| `M_STITCHING_RESULT` *(default)* | Specifies that the result buffer can hold results from a correlation-stitching registration operation. |

### `ResultIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the registration result buffer identifier or specifies the data type that the function should use to return the registration result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated registration result
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated registration result
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_REG_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the registration result
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the DFF result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated DFF result buffer.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAllocResult.md) is written as the identifier. |
| `Address in which to write the EDOF result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated EDOF result buffer.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAllocResult.md) is written as the identifier. |
| `Address in which to write the HDR result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated HDR result buffer.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAllocResult.md) is written as the identifier. |
| `Address in which to write the photometric stereo result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated photometric stereo result buffer.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAllocResult.md) is written as the identifier. |
| `Address in which to write the stitching result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated stitching result buffer.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the registration result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_REG_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/reg/MregAllocResult.md) was specified).
