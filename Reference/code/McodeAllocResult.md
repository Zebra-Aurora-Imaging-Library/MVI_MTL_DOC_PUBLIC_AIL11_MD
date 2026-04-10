---
doctype: Reference
module: code
function: McodeAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeAllocResult"
---

# McodeAllocResult

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

> Allocate a code result buffer.

## Syntax

```c
AIL_ID McodeAllocResult(
    AIL_ID    SysId,           //in
    AIL_INT64 ControlFlag,     //in
    AIL_ID *  ResultCodeIdPtr  //out
)
```

## Description

This function allocates a code result buffer on the specified system to store results obtained from an [`McodeTrain`](../../Reference/code/McodeTrain.md), [`McodeDetect`](../../Reference/code/McodeDetect.md) [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.

When the code result buffer is no longer required, release it using[`McodeFree`](../../Reference/code/McodeFree.md)unless [`M_UNIQUE_ID`](../../Reference/code/McodeAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/code/McodeAllocResult.md) was specified, the smart identifier manages the code result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the result buffer. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the type of result buffer to allocate. This parameter must be set to one of the following:

*For specifying the result buffer type*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CODE_DETECT_RESULT` | Specifies to allocate a code detect result buffer to be used with [`McodeDetect`](../../Reference/code/McodeDetect.md). |
| `M_CODE_GRADE_RESULT` *(default)* | Specifies to allocate a code grade result buffer to be used with [`McodeGrade`](../../Reference/code/McodeGrade.md). |
| `M_CODE_READ_RESULT` | Specifies to allocate a code read result buffer to be used with [`McodeRead`](../../Reference/code/McodeRead.md). |
| `M_CODE_TRAIN_RESULT` | Specifies to allocate a code train result buffer to be used with [`McodeTrain`](../../Reference/code/McodeTrain.md). |
| `M_CODE_WRITE_RESULT` | Specifies to allocate a code write result buffer to be used with [`McodeWrite`](../../Reference/code/McodeWrite.md). |

### `ResultCodeIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the code result buffer identifier or specifies the data type that the function should use to return the code result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated code result buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated code result buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_CODE_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the code result buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the code detect result identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated code detect result buffer.

If allocation fails, [`M_NULL`](../../Reference/code/McodeAllocResult.md) is written as the identifier. |
| `Address in which to write the code grade result identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated code grade result buffer.

If allocation fails, [`M_NULL`](../../Reference/code/McodeAllocResult.md) is written as the identifier. |
| `Address in which to write the code read result identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated code read result buffer.

If allocation fails, [`M_NULL`](../../Reference/code/McodeAllocResult.md) is written as the identifier. |
| `Address in which to write the code train result identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated code train result buffer.

If allocation fails, [`M_NULL`](../../Reference/code/McodeAllocResult.md) is written as the identifier. |
| `Address in which to write the code write result identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated code write result buffer.

If allocation fails, [`M_NULL`](../../Reference/code/McodeAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the code result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_CODE_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/code/McodeAllocResult.md) was specified).
