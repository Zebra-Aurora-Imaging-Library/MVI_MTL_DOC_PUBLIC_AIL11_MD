---
doctype: Reference
module: 3dmet
function: M3dmetAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetAlloc"
---

# M3dmetAlloc

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

> Allocate a 3D metrology context.

## Syntax

```c
AIL_ID M3dmetAlloc(
    AIL_ID    SysId,             //in
    AIL_INT64 ContextType,       //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID *  Context3dmetIdPtr  //out
)
```

## Description

This function allocates a 3D metrology context on the specified system. A 3D metrology context contains information needed to perform an [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md), [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md), [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md), or [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation. You can also use [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) to allocate a draw 3D metrology context for drawing results using [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md).

When the 3D metrology context is no longer required, release it using[`M3dmetFree`](../../Reference/3dmet/M3dmetFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dmet/M3dmetAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dmet/M3dmetAlloc.md) was specified, the smart identifier manages the 3D metrology context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the 3D metrology context.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of 3D metrology context to allocate. This parameter should be set to one of the following values:

*For specifying the context object*

| Value | Description |
| --- | --- |
| `M_DISTANCE_CONTEXT` | Specifies to allocate a distance 3D metrology context to use with [`M3dmetDistanceEx`](../../Reference/3dmet/M3dmetDistanceEx.md). |
| `M_DRAW_3D_CONTEXT` | Specifies to allocate a draw 3D metrology context to use with [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md). |
| `M_FIT_CONTEXT` | Specifies to allocate a fit 3D metrology context to use with [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md). |
| `M_STATISTICS_CONTEXT` | Allocates a statistics 3D metrology context to use with [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md). |
| `M_VOLUME_CONTEXT` | Specifies to allocate a volume 3D metrology context to use with [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Context3dmetIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D metrology context identifier or specifies the data type that the function should use to return the 3D metrology context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D metrology context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D metrology context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DMET_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D metrology context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the distance 3D metrology context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated distance 3D metrology context.

If allocation fails, [`M_NULL`](../../Reference/3dmet/M3dmetAlloc.md) is written as the identifier. |
| `Address in which to write the fit 3D metrology context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated fit 3D metrology context.

If allocation fails, [`M_NULL`](../../Reference/3dmet/M3dmetAlloc.md) is written as the identifier. |
| `Address in which to write the statistics 3D metrology context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated statistics 3D metrology context.

If allocation fails, [`M_NULL`](../../Reference/3dmet/M3dmetAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D metrology context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DMET_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dmet/M3dmetAlloc.md) was specified).
