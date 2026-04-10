---
doctype: Reference
module: edge
function: MedgeAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeAlloc"
---

# MedgeAlloc

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

> Allocate an Edge Finder context.

## Syntax

```c
AIL_ID MedgeAlloc(
    AIL_ID    SysId,           //in
    AIL_INT64 EdgeFinderType,  //in
    AIL_INT64 ControlFlag,     //in
    AIL_ID *  ContextIdPtr     //out
)
```

## Description

This function allocates an Edge Finder context on the specified system. An Edge Finder context contains all the information necessary to perform an [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) operation, including global processing settings, and edge features to calculate.

Edge Finder context settings can be adjusted using [`MedgeControl`](../../Reference/edge/MedgeControl.md).

When the Edge Finder context is no longer required, release it using[`MedgeFree`](../../Reference/edge/MedgeFree.md)unless [`M_UNIQUE_ID`](../../Reference/edge/MedgeAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/edge/MedgeAlloc.md) was specified, the smart identifier manages the Edge Finder context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the Edge Finder context.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `EdgeFinderType` *(in, AIL_INT64)*

Specifies the type of Edge Finder context. This parameter must be set to one of the values below.

*For the Edge Finder context*

| Value | Description |
| --- | --- |
| `M_CONTOUR` | Specifies a contour context type, which is used to find object contours in images. |
| `M_CREST` | Specifies a crest context type, which is used to find thin line crests in images. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Edge Finder context identifier or specifies the data type that the function should use to return the Edge Finder context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Edge Finder context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Edge Finder context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_EDGE_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Edge Finder context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the contour Edge Finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated contour Edge Finder context.

If allocation fails, [`M_NULL`](../../Reference/edge/MedgeAlloc.md) is written as the identifier. |
| `Address in which to write the crest Edge Finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated crest Edge Finder context.

If allocation fails, [`M_NULL`](../../Reference/edge/MedgeAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Edge Finder context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_EDGE_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/edge/MedgeAlloc.md) was specified).
