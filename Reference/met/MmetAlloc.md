---
doctype: Reference
module: met
function: MmetAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetAlloc"
---

# MmetAlloc

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

> Allocate a metrology context or a derived metrology region object.

## Syntax

```c
AIL_ID MmetAlloc(
    AIL_ID    SysId,                   //in
    AIL_INT64 Type,                    //in
    AIL_ID *  ContextOrRegionMetIdPtr  //out
)
```

## Description

This function allocates a metrology context or a derived metrology region object on the specified system. A metrology context contains all the information necessary to perform an [`MmetCalculate`](../../Reference/met/MmetCalculate.md) operation, including global processing settings and a metrology template; the metrology template defines the set of features and geometric tolerances against which [`MmetCalculate`](../../Reference/met/MmetCalculate.md) validates and measures objects in an image. A derived metrology region object is an optional Aurora Imaging Library object that stores specialized information about metrology regions (ROIs).

When you allocate a metrology context, Aurora Imaging Library automatically creates a global frame, assigns it an index of 0, and labels it [`M_GLOBAL_FRAME`](../../Reference/met/MmetControl.md). The global frame is the first coordinate system used to define the features to add to the metrology template of a metrology context. The global frame always exists and you cannot delete it. If the target image is not calibrated, the global frame's default origin (0,0) is aligned with the center of the top-left corner pixel of the target image. If the target image is calibrated, the global frame's default origin is aligned with the origin of the relative (world) coordinate system.

To add features (either physically measured or constructed) or geometric tolerances to the metrology template of a metrology context, use [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) or [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md), respectively. To adjust metrology context, feature, and geometric tolerance settings, use [`MmetControl`](../../Reference/met/MmetControl.md).

For physically measured features, you must also use [`MmetSetRegion`](../../Reference/met/MmetSetRegion.md) to set the metrology region that delimits the area in the target image from which to establish the feature. You can set the metrology region using explicit values or a 2D graphics list; you can also derive a metrology region using other features. If you are using an explicitly-defined or a 2D graphics list metrology region, you just need to allocate a metrology context, and specify it when you call [`MmetSetRegion`](../../Reference/met/MmetSetRegion.md). If you are deriving a metrology region using other features, you must also allocate a derived metrology region object and specify it, as well as the metrology context, when you call [`MmetSetRegion`](../../Reference/met/MmetSetRegion.md).

When the metrology context or the derived metrology region object is no longer required, release it using[`MmetFree`](../../Reference/met/MmetFree.md)unless [`M_UNIQUE_ID`](../../Reference/met/MmetAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/met/MmetAlloc.md) was specified, the smart identifier manages the metrology context or the derived metrology region object's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the metrology context or the derived metrology region object.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `Type` *(in, AIL_INT64)*

Specifies whether to allocate a metrology context or a derived metrology region object. This parameter should be set to one of the following values:

*For specifying whether to allocate a metrology context or a derived metrology region object*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CONTEXT` *(default)* | Specifies a metrology context. |
| `M_DERIVED_GEOMETRY_REGION` | Specifies a derived metrology region object. This object is only required when a geometry setting of a metrology region must be derived from one or more features. This type of metrology region is referred to as a derived metrology region. Use [`MmetSetRegion`](../../Reference/met/MmetSetRegion.md) with [`M_FROM_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetSetRegion.md) to associate the derived metrology region with a measured feature. |

### `ContextOrRegionMetIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the metrology context or the derived metrology region object identifier or specifies the data type that the function should use to return the metrology context or the derived metrology region object identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated metrology context or the derived
                              metrology region object; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated metrology context or the derived
                              metrology region object; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_MET_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the metrology context or the derived
                              metrology region object (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the metrology context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated metrology context.

If allocation fails, [`M_NULL`](../../Reference/met/MmetAlloc.md) is written as the identifier. |
| `Address in which to write the metrology derived geometry region identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated metrology derived region object.

If allocation fails, [`M_NULL`](../../Reference/met/MmetAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the metrology context or the derived metrology region object identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_MET_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/met/MmetAlloc.md) was specified).
