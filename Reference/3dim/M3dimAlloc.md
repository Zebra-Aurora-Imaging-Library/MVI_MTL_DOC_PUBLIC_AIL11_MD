---
doctype: Reference
module: 3dim
function: M3dimAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimAlloc"
---

# M3dimAlloc

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

> Allocate a 3D image processing context.

## Syntax

```c
AIL_ID M3dimAlloc(
    AIL_ID    SysId,            //in
    AIL_INT64 ContextType,      //in
    AIL_INT64 ControlFlag,      //in
    AIL_ID *  Context3dimIdPtr  //out
)
```

## Description

This function allocates a 3D image processing context on the specified system.

When the 3D image processing context is no longer required, release it using[`M3dimFree`](../../Reference/3dim/M3dimFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dim/M3dimAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dim/M3dimAlloc.md) was specified, the smart identifier manages the 3D image processing context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the 3D image processing context.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of 3D image processing context to allocate. This parameter must be set to one of the following:

*For specifying the context type*

| Value | Description |
| --- | --- |
| `M_CALCULATE_MAP_SIZE_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md). |
| `M_FILL_GAPS_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimFillGaps`](../../Reference/3dim/M3dimFillGaps.md). |
| `M_FILTER_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md). |
| `M_LATTICE_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md). |
| `M_MESH_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md). |
| `M_NORMALS_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md). |
| `M_OUTLIERS_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md). |
| `M_PROFILE_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md). |
| `M_PROJECT_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md). |
| `M_REMAP_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimRemapDepthMap`](../../Reference/3dim/M3dimRemapDepthMap.md). |
| `M_STATISTICS_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimStat`](../../Reference/3dim/M3dimStat.md). |
| `M_STITCH_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md). |
| `M_SUBSAMPLE_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimSample`](../../Reference/3dim/M3dimSample.md) to perform subsampling. |
| `M_SURFACE_SAMPLE_CONTEXT` | Specifies to allocate a 3D image processing context that can be used with [`M3dimSample`](../../Reference/3dim/M3dimSample.md) to perform surface sampling. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Context3dimIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D image processing context identifier or specifies the data type that the function should use to return the 3D image processing context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D image processing context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D image processing context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DIM_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D image processing context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the calculate map size 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated map size 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the fill gaps 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated fill gaps 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the filter 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated filter 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the lattice 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated lattice 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the mesh 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated mesh 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the normals 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated normals 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the profile 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated profile 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the project 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated project 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the remap 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated remap 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the statistics 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated statistics 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the stitch 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated stitch 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the subsample 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated subsample 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |
| `Address in which to write the surface sample 3D image processing context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated surface sample 3D image processing context.

If allocation fails, [`M_NULL`](../../Reference/3dim/M3dimAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D image processing context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DIM_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dim/M3dimAlloc.md) was specified).
