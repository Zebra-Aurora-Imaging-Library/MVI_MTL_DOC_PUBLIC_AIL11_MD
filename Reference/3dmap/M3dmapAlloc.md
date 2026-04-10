---
doctype: Reference
module: 3dmap
function: M3dmapAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapAlloc"
---

# M3dmapAlloc

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

> Allocate a 3D reconstruction context, 3D draw context, or 3D alignment context.

## Syntax

```c
AIL_ID M3dmapAlloc(
    AIL_ID    SysId,                       //in
    AIL_INT64 ContextType,                 //in
    AIL_INT64 ControlFlag,                 //in
    AIL_ID *  ContextOrGeometry3dmapIdPtr  //out
)
```

## Description

This function allocates a 3D reconstruction context or 3D alignment context on the specified system. A 3D reconstruction context contains information needed to perform an [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) operation and a 3D alignment context contains information needed to perform an [`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) operation. You can also use [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) to allocate a 3D draw context for drawing results using [`M3dmapDraw3d`](../../Reference/3dmap/M3dmapDraw3d.md).

When the 3D reconstruction context, 3D draw context, or 3D alignment context is no longer required, release it using[`M3dmapFree`](../../Reference/3dmap/M3dmapFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dmap/M3dmapAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dmap/M3dmapAlloc.md) was specified, the smart identifier manages the 3D reconstruction context, 3D draw context, or 3D alignment context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the 3D reconstruction context, 3D draw context, or 3D alignment context.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies to allocate a 3D reconstruction context, 3D draw context, or 3D alignment context. This parameter must be set to one of the following:

*For specifying the type*

| Value | Description |
| --- | --- |
| `M_ALIGN_CONTEXT` | Specifies a 3D alignment context. |
| `M_DRAW_3D_CONTEXT` | Specifies a 3D draw context. |
| `M_LASER` | Specifies a 3D reconstruction context that will be used to perform laser line (sheet of light) profiling. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the 3D reconstruction mode of the context, which determines how data is represented. The data representation determines whether depth maps can be fully corrected and whether 3D point clouds can be returned.

*For specifying the 3D reconstruction mode*

| Value | Description |
| --- | --- |
| `M_CALIBRATED_CAMERA_LINEAR_MOTION` | Specifies that the 3D reconstruction context will include camera calibration information and depth correction information. This allows for the storage of 3D point clouds and the generation of fully corrected depth maps (shape and depth corrected).

> **Note:** Note that the camera calibration context must have been allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md)with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md). |
| `M_DEPTH_CORRECTION` | Specifies that the 3D reconstruction context will include depth correction information, but will not include camera calibration information. This allows for only partially corrected depth maps (depth, but not shape corrected). |

*For specifying the camera label or laser label for a given context*

| Value | Description |
| --- | --- |
| `M_CAMERA_LABEL` | Specifies the label for the camera used by this 3D reconstruction context.

Note that multiple 3D reconstruction contexts can share the same camera, and in those cases, they must also share the same camera label. |
| `M_LASER_LABEL` | Specifies the label for the laser used by this 3D reconstruction context.

Note that multiple 3D reconstruction contexts can share the same laser, and in those cases, they must also share the same laser label. |

### `ContextOrGeometry3dmapIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D reconstruction context, 3D draw context, or 3D alignment context identifier or specifies the data type that the function should use to return the 3D reconstruction context, 3D draw context, 3D alignment context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D reconstruction context, 3D draw
                              context, or 3D alignment context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D reconstruction context, 3D draw
                              context, or 3D alignment context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DMAP_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D reconstruction context, 3D draw
                              context, or 3D alignment context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the 3D alignment context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 3D alignment context.

If allocation fails, [`M_NULL`](../../Reference/3dmap/M3dmapAlloc.md) is written as the identifier. |
| `Address in which to write the 3D draw context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 3D draw context.

If allocation fails, [`M_NULL`](../../Reference/3dmap/M3dmapAlloc.md) is written as the identifier. |
| `Address in which to write the 3D reconstruction context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 3D reconstruction context.

If allocation fails, [`M_NULL`](../../Reference/3dmap/M3dmapAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D reconstruction context, 3D draw context, or 3D alignment context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DMAP_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dmap/M3dmapAlloc.md) was specified).
