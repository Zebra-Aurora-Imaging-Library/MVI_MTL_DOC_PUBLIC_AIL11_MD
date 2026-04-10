---
doctype: Reference
module: 3dmod
function: M3dmodAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodAlloc"
---

# M3dmodAlloc

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

> Allocate a 3D model finder context.

## Syntax

```c
AIL_ID M3dmodAlloc(
    AIL_ID    SysId,             //in
    AIL_INT64 ContextType,       //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID *  Context3dmodIdPtr  //out
)
```

## Description

This function allocates a 3D model finder context on the specified system. A 3D model finder context contains information needed to perform a [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) search. You can also use [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) to allocate a draw 3D model finder context for drawing results using [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md).

Define and add a model to the model finder context using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md). You can define one model per model finder context.

When the 3D model finder context is no longer required, release it using [`M3dmodFree`](../../Reference/3dmod/M3dmodFree.md) unless [`M_UNIQUE_ID`](../../Reference/3dmod/M3dmodAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dmod/M3dmodAlloc.md) was specified, the smart identifier manages the 3D model finder context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the 3D model finder context. Set this parameter to one of the values below:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of 3D model finder context to allocate. Set this parameter to one of the values below:

*For specifying the type of 3D model context*

| Value | Description |
| --- | --- |
| `M_DRAW_3D_GEOMETRIC_CONTEXT` | Allocates a draw geometric 3D model finder context for use with [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md). |
| `M_DRAW_3D_SURFACE_CONTEXT` | Allocates a draw surface 3D model finder context for use with [`M3dmodDraw3d`](../../Reference/3dmod/M3dmodDraw3d.md). |
| `M_FIND_BOX_CONTEXT` | Allocates a find box 3D model finder context for use with [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). |
| `M_FIND_CYLINDER_CONTEXT` | Allocates a find cylinder 3D model finder context for use with [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). |
| `M_FIND_PLANAR_SURFACE_CONTEXT` | Allocates a find planar surface 3D model finder context for use with [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). |
| `M_FIND_RECTANGULAR_PLANE_CONTEXT` | Allocates a find rectangular plane 3D model finder context for use with [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). |
| `M_FIND_SPHERE_CONTEXT` | Allocates a find sphere 3D model finder context for use with [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). |
| `M_FIND_SURFACE_CONTEXT` | Allocates a find surface 3D model finder context for use with [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Context3dmodIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D model finder context identifier or specifies the data type that the function should use to return the 3D model finder context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D model finder context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D model finder context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DMOD_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D model finder context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the draw geometric 3D model finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated draw geometric 3D model finder context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dmod/M3dmodAlloc.md) is written as the identifier. |
| `Address in which to write the draw surface 3D model finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated draw surface 3D model finder context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dmod/M3dmodAlloc.md) is written as the identifier. |
| `Address in which to write the find box 3D model finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated box 3D model finder context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dmod/M3dmodAlloc.md) is written as the identifier. |
| `Address in which to write the find cylinder 3D model finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated cylinder 3D model finder context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dmod/M3dmodAlloc.md) is written as the identifier. |
| `Address in which to write the find planar surface 3D model finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated planar surface 3D model finder context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dmod/M3dmodAlloc.md) is written as the identifier. |
| `Address in which to write the find rectangular plane 3D model finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated rectangular plane 3D model finder context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dmod/M3dmodAlloc.md) is written as the identifier. |
| `Address in which to write the find sphere 3D model finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated sphere 3D model finder context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dmod/M3dmodAlloc.md) is written as the identifier. |
| `Address in which to write the find surface 3D model finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated surface 3D model finder context identifier.

If allocation fails, [`M_NULL`](../../Reference/3dmod/M3dmodAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D model finder context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DMOD_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dmod/M3dmodAlloc.md) was specified).
