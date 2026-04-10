---
doctype: Reference
module: 3dmeas
function: M3dmeasAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasAlloc"
---

# M3dmeasAlloc

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

> Allocate a 3D measurement context.

## Syntax

```c
AIL_ID M3dmeasAlloc(
    AIL_ID    SysId,              //in
    AIL_INT64 ContextType,        //in
    AIL_INT64 ControlFlag,        //in
    AIL_ID *  Context3dmeasIdPtr  //out
)
```

## Description

This function allocates a 3D measurement context on the specified system. A 3D measurement context contains information needed to perform an [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) operation, which allow you to find markers in a specified depth map or fit a geometry to found markers, respectively. Markers denote the location of edge transitions along a profile; the edge transition can go from a lower Z-value to a higher Z-value (or vice versa). You can also use [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) to allocate a draw 3D measurement context for drawing results using [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md).

You can find markers in a profile taken along a specified path, in profiles taken at regular intervals perpendicular to a specified template, or in each row of a depth map, where each row is interpreted as a single profile.

Finding markers along a path is useful when you want to find an object but you don't know its exact location. In this case, you can allocate an [`M_FIND_MARKER_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md) and use [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md) to set a path that crosses through the possible locations. The 3D measurement module will extract a profile along the path and find the markers within the profile.

Finding markers perpendicular to a template is useful when you want to verify the location or shape of an object. You can allocate an [`M_FIND_MARKER_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md) and specify a template that follows an expected edge of your object using [`M3dmeasDefine`](../../Reference/3dmeas/M3dmeasDefine.md). To find the markers that can form the template, the 3D measurement module will extract profiles at regular intervals perpendicular to the template and find a single marker in each profile where the height change occurs. Note that you can use [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) to fit a geometry to the found markers to compare their location to that of the template.

You can use a profile 3D measurement context ([`M_FIND_MARKER_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md)) to analyze an entire depth map. In this case, the profile extraction step is skipped and the find marker operation is performed on each row (profile) of the depth map. The 3D measurement module can find the specified number of markers along each profile. Note that a profile 3D measurement context contains a default template that is inherent to the supplied profiles and does not need to be defined.

After allocating the 3D measurement context, and if required, defining the path or template, use [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) to find and/or fit the markers.

When the 3D measurement context is no longer required, release it using[`M3dmeasFree`](../../Reference/3dmeas/M3dmeasFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dmeas/M3dmeasAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dmeas/M3dmeasAlloc.md) was specified, the smart identifier manages the 3D measurement context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the 3D measurement context.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of 3D measurement context to allocate. This parameter should be set to one of the following values:

*For specifying the context object*

| Value | Description |
| --- | --- |
| `M_DRAW_3D_PATH_CONTEXT` | Specifies to allocate a draw path 3D measurement context for use with [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md), to draw features of a path, a path's profile, or markers found in a path's profile. |
| `M_DRAW_3D_PROFILE_CONTEXT` | Specifies to allocate a draw profile 3D measurement context for use with [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md), to draw features of supplied profiles or markers found in supplied profiles. |
| `M_DRAW_3D_TEMPLATE_CONTEXT` | Specifies to allocate a draw template 3D measurement context for use with [`M3dmeasDraw3d`](../../Reference/3dmeas/M3dmeasDraw3d.md), to draw features of a template, a template's perpendicular profiles, markers found in a template's perpendicular profiles, or results from a fit operation. |
| `M_FIND_MARKER_PATH_CONTEXT` | Specifies to allocate a path 3D measurement context for use with [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md), to find markers in a path's profile. |
| `M_FIND_MARKER_PROFILE_CONTEXT` | Specifies to allocate a profile 3D measurement context for use with [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md), to find markers in supplied profiles. Note that supplied profiles have a corresponding default template that does not need to be defined. |
| `M_FIND_MARKER_TEMPLATE_CONTEXT` | Specifies to allocate a template 3D measurement context for use with [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md) or [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md), to find markers in a template's perpendicular profiles. |
| `M_FIT_CONTEXT` | Specifies to allocate a fit 3D measurement context for use with [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md), to fit a geometry to markers found in a template's perpendicular profiles. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Context3dmeasIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D measurement context identifier or specifies the data type that the function should use to return the 3D measurement context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D measurement context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D measurement context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DMEAS_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D measurement context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the draw path 3D measurement context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated draw path 3D measurement context.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAlloc.md) is written as the identifier. |
| `Address in which to write the draw profile 3D measurement context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated draw profile 3D measurement context.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAlloc.md) is written as the identifier. |
| `Address in which to write the draw template 3D measurement context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated draw template 3D measurement context.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAlloc.md) is written as the identifier. |
| `Address in which to write the fit 3D measurement context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated fit 3D measurement context.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAlloc.md) is written as the identifier. |
| `Address in which to write the path 3D measurement context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated path 3D measurement context.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAlloc.md) is written as the identifier. |
| `Address in which to write the profile 3D measurement context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated profile 3D measurement context.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAlloc.md) is written as the identifier. |
| `Address in which to write the template 3D measurement context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated template 3D measurement context.

If allocation fails, [`M_NULL`](../../Reference/3dmeas/M3dmeasAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D measurement context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DMEAS_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dmeas/M3dmeasAlloc.md) was specified).
