---
doctype: Reference
module: 3dmeas
function: M3dmeasAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasAllocResult"
---

# M3dmeasAllocResult

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

> Allocates a 3D measurement result buffer.

## Syntax

```c
AIL_ID M3dmeasAllocResult(
    AIL_ID    SysId,             //in
    AIL_INT64 ResultType,        //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID *  Result3dmeasIdPtr  //out
)
```

## Description

This function allocates a 3D measurement result buffer on the specified system, to store results obtained from an [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) operation.

When the 3D measurement result buffer is no longer required, release it using[`M3dmeasFree`](../../Reference/3dmeas/M3dmeasFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dmeas/M3dmeasAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dmeas/M3dmeasAllocResult.md) was specified, the smart identifier manages the 3D measurement result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the 3D measurement result buffer.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of 3D measurement result buffer to allocate.

*For specifying the type of result to allocate*

| Value | Description |
| --- | --- |
| `M_FIND_MARKER_PATH_RESULT` | Specifies to allocate a path 3D measurement result buffer, used to store [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) results. |
| `M_FIND_MARKER_PROFILE_RESULT` | Specifies to allocate a profile 3D measurement result buffer, used to store [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) results. |
| `M_FIND_MARKER_TEMPLATE_RESULT` | Specifies to allocate a template 3D measurement result buffer, used to store [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) results. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Result3dmeasIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D measurement result buffer identifier or specifies the data type that the function should use to return the 3D measurement result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D measurement result
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D measurement result
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DMEAS_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D measurement result
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the path 3D measurement result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated path 3D measurement result buffer.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAllocResult.md) is written as the identifier. |
| `Address in which to write the profile 3D measurement result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated profile 3D measurement result buffer.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAllocResult.md) is written as the identifier. |
| `Address in which to write the template 3D measurement result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated template 3D measurement result buffer.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D measurement result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DMEAS_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dmeas/M3dmeasAllocResult.md) was specified).
