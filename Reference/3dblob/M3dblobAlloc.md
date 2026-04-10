---
doctype: Reference
module: 3dblob
function: M3dblobAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobAlloc"
---

# M3dblobAlloc

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

> Allocate a 3D blob analysis context.

## Syntax

```c
AIL_ID M3dblobAlloc(
    AIL_ID    SysId,              //in
    AIL_INT64 ContextType,        //in
    AIL_INT64 ControlFlag,        //in
    AIL_ID *  Context3dblobIdPtr  //out
)
```

## Description

This function allocates a 3D blob analysis context on the specified system. A 3D blob analysis context contains information needed to perform a [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) or [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) operation. You can also use [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) to allocate a draw 3D blob analysis context for drawing results using [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md).

When the 3D blob analysis context is no longer required, release it using[`M3dblobFree`](../../Reference/3dblob/M3dblobFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobAlloc.md) was specified, the smart identifier manages the 3D blob analysis context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the 3D blob analysis context. Set this parameter to one of the values below:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of 3D blob analysis context to allocate. Set this parameter to one of the values below:

*For specifying the type of 3D blob analysis context*

| Value | Description |
| --- | --- |
| `M_CALCULATE_CONTEXT` | Allocates a calculate 3D blob analysis context for use with [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md). |
| `M_DRAW_3D_CONTEXT` | Allocates a draw 3D blob analysis context for use with [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md). |
| `M_SEGMENTATION_CONTEXT` | Allocates a segmentation 3D blob analysis context for use with [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Context3dblobIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D blob analysis context identifier or specifies the data type that the function should use to return the 3D blob analysis context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D blob analysis context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D blob analysis context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DBLOB_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D blob analysis context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the calculate 3D blob analysis context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated calculate 3D blob analysis context.

If allocation fails, [`M_NULL`](../../Reference/3dblob/M3dblobAlloc.md) is written as the identifier. |
| `Address in which to write the draw 3D blob analysis context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated draw 3D blob analysis context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dblob/M3dblobAlloc.md) is written as the identifier. |
| `Address in which to write the segmentation 3D blob analysis context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated segmentation 3D blob analysis context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dblob/M3dblobAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D blob analysis context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DBLOB_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dblob/M3dblobAlloc.md) was specified).
