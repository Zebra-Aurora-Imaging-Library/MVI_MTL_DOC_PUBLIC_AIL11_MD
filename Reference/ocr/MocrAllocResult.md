---
doctype: Reference
module: ocr
function: MocrAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrAllocResult"
---

# MocrAllocResult

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

> Allocate an OCR result buffer.

## Syntax

```c
AIL_ID MocrAllocResult(
    AIL_ID    SysId,          //in
    AIL_INT64 InitFlag,       //in
    AIL_ID *  ResultOcrIdPtr  //out
)
```

## Description

This function allocates a result buffer for use with other OCR functions. You should check if the operation was successful by verifying that the OCR result buffer identifier returned is not [`M_NULL`](../../Reference/ocr/MocrAllocResult.md).

When the result buffer is no longer required, you should release it using the [`MocrFree`](../../Reference/ocr/MocrFree.md) function.

When the OCR result buffer is no longer required, release it using[`MocrFree`](../../Reference/ocr/MocrFree.md)unless [`M_UNIQUE_ID`](../../Reference/ocr/MocrAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/ocr/MocrAllocResult.md) was specified, the smart identifier manages the OCR result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the OCR result buffer.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `InitFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ResultOcrIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the OCR result buffer identifier or specifies the data type that the function should use to return the OCR result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated OCR result buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated OCR result buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_OCR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the OCR result buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the OCR result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated OCR result buffer.

If allocation fails, [`M_NULL`](../../Reference/ocr/MocrAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the OCR result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_OCR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/ocr/MocrAllocResult.md) was specified).
