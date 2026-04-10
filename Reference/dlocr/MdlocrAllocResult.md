---
doctype: Reference
module: dlocr
function: MdlocrAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrAllocResult"
---

# MdlocrAllocResult

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

> Allocate a Deep Learning OCR result buffer.

## Syntax

```c
AIL_ID MdlocrAllocResult(
    AIL_ID    SysId,            //in
    AIL_INT64 ResultType,       //in
    AIL_INT64 ControlFlag,      //in
    AIL_ID *  DlocrResultIdPtr  //out
)
```

## Description

This function allocates a Deep Learning OCR read result buffer, or a Deep Learning OCR fine-tuning result buffer on the specified system, to store results obtained from an [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) operation, or an [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md) operation, respectively.

When the Deep Learning OCR result buffer is no longer required, release it using[`MdlocrFree`](../../Reference/dlocr/MdlocrFree.md)unless [`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrAllocResult.md) was specified, the smart identifier manages the Deep Learning OCR result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the Deep Learning OCR result buffer. Set this parameter to one of the values below:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result buffer to allocate.

*For specifying the type of read result buffer to allocate*

| Value | Description |
| --- | --- |
| `M_OCR_NET1_READ_RESULT` | Specifies to allocate a Deep Learning OCR result buffer to hold results from a read context using the NET1 architecture. |
| `M_OCR_NET2_READ_RESULT` | Specifies to allocate a Deep Learning OCR result buffer to hold results from a read context using the NET2 architecture. |

*For specifying the type of fine-tuning result buffer to allocate*

| Value | Description |
| --- | --- |
| `M_OCR_NET2_FINETUNE_RESULT` | Specifies to allocate a Deep Learning OCR result buffer to hold results from a fine-tuning context using the NET2 architecture. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DlocrResultIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Deep Learning OCR result buffer identifier or specifies the data type that the function should use to return the Deep Learning OCR result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Deep Learning OCR result
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Deep Learning OCR result
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_DLOCR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Deep Learning OCR result
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the fine-tuning result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated Deep Learning OCR fine-tuning result buffer.

If allocation fails, [`M_NULL`](../../Reference/dlocr/MdlocrAllocResult.md) is written as the identifier. |
| `Address in which to write the read result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated Deep Learning OCR read result buffer.

If allocation fails, [`M_NULL`](../../Reference/dlocr/MdlocrAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Deep Learning OCR result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_DLOCR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/dlocr/MdlocrAllocResult.md) was specified).
