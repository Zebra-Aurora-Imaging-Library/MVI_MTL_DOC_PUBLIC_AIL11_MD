---
doctype: Reference
module: 3dblob
function: M3dblobAllocResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobAllocResult"
---

# M3dblobAllocResult

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

> Allocate a 3D blob analysis result buffer.

## Syntax

```c
AIL_ID M3dblobAllocResult(
    AIL_ID    SysId,             //in
    AIL_INT64 ResultType,        //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID *  Result3dblobIdPtr  //out
)
```

## Description

This function allocates a 3D blob analysis result buffer on the specified system to store results obtained from an [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md)or [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) operation.

When the 3D blob analysis result buffer is no longer required, release it using[`M3dblobFree`](../../Reference/3dblob/M3dblobFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobAllocResult.md) was specified, the smart identifier manages the 3D blob analysis result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the result buffer. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result buffer to allocate. Set this parameter to the value below:

*For specifying the type of result buffer to allocate*

| Value | Description |
| --- | --- |
| `M_SEGMENTATION_RESULT` | Specifies to allocate a 3D blob analysis result buffer used to store [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) results. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `Result3dblobIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D blob analysis result buffer identifier or specifies the data type that the function should use to return the 3D blob analysis result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D blob analysis result buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D blob analysis result buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DBLOB_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D blob analysis result buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the segmentation 3D blob analysis result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated segmentation 3D blob analysis result buffer.

If allocation fails, [`M_NULL`](../../Reference/3dblob/M3dblobAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D blob analysis result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DBLOB_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobAllocResult.md) was specified).
