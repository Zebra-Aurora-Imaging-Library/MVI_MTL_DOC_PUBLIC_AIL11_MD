---
doctype: Reference
module: mod
function: MmodAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodAlloc"
---

# MmodAlloc

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

> Allocate a Model Finder context.

## Syntax

```c
AIL_ID MmodAlloc(
    AIL_ID    SysId,            //in
    AIL_INT64 ModelFinderType,  //in
    AIL_INT64 ControlFlag,      //in
    AIL_ID *  ContextIdPtr      //out
)
```

## Description

This function allocates a Model Finder context on the specified system. A Model Finder context contains all the information necessary to perform an [`MmodFind`](../../Reference/mod/MmodFind.md) search, including global search settings, and the individual model(s) to locate.

Define and add models to the Model Finder context using [`MmodDefine`](../../Reference/mod/MmodDefine.md). You must add at least one model to the context regardless of the context type. Some context types accept only one model.

The Model Finder context and individual model search settings can be adjusted using [`MmodControl`](../../Reference/mod/MmodControl.md).

When the Model Finder context is no longer required, release it using[`MmodFree`](../../Reference/mod/MmodFree.md)unless [`M_UNIQUE_ID`](../../Reference/mod/MmodAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/mod/MmodAlloc.md) was specified, the smart identifier manages the Model Finder context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the context. This parameter should be set to one of the following values:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ModelFinderType` *(in, AIL_INT64)*

Specifies the type of Model Finder context. This parameter must be set to one of the following values:

*For the type of context*

| Value | Description |
| --- | --- |
| `M_GEOMETRIC` | Specifies that the Model Finder context uses a general geometric search algorithm. This algorithm uses geometric features to locate user-specified model(s). |
| `M_GEOMETRIC_CONTROLLED` | Specifies that the Model Finder context uses a controlled geometric search algorithm.

When there is a lot of geometric complexity, this search method is often faster and more robust than [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md). The search method is especially fast when the angle between the occurrence and the nominal angle of the model ([`M_ANGLE`](../../Reference/mod/MmodControl.md)) is small, although searching within an angular range is supported.

This search method is recommended when there are only small differences in scale between the occurrence and the nominal scale of the model ([`M_SCALE`](../../Reference/mod/MmodControl.md)); searching through a range of scales is not supported ([`M_SEARCH_SCALE_RANGE`](../../Reference/mod/MmodControl.md) is disabled by default and cannot be set to [`M_ENABLE`](../../Reference/mod/MmodControl.md)).

You cannot get or draw the target edges for the entire target ([`M_GENERAL`](../../Reference/mod/MmodGetResult.md)); you can only get or draw them in the region of an occurrence. |
| `M_SHAPE_CIRCLE` | Specifies that the Model Finder context uses a circular model search algorithm.

This context only supports [`M_CIRCLE`](../../Reference/mod/MmodDefine.md)type models, and only supports one model per context. The model cannot be calibrated. If the target is associated with a camera calibration context, the model will be interpreted in world units; conversely, if the target does not have a camera calibration context associated with it, the model will be interpreted in pixel units.

This search algorithm is recommended when the objects being sought are circular-type shapes, although the algorithm does have a tolerance for deformations of the circular shape. This controllable deformation tolerance ([`M_SAGITTA_TOLERANCE`](../../Reference/mod/MmodControl.md)) could include detecting certain elliptical-type shapes as well as noisy circles. This search algorithm will typically find circular shapes faster and in a more robust manner than the search algorithms used for [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) types of Model Finder contexts.

When using this type of Model Finder context, [`M_SEARCH_ANGLE_RANGE`](../../Reference/mod/MmodControl.md), [`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md), and [`M_SEARCH_SCALE_RANGE`](../../Reference/mod/MmodControl.md) are turned off by default and cannot be set to [`M_ENABLE`](../../Reference/mod/MmodControl.md).

When using this type of context, you must also allocate an [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAllocResult.md) Model Finder result buffer. |
| `M_SHAPE_ELLIPSE` | Specifies that the Model Finder context uses an elliptical model search algorithm.

This context only supports [`M_ELLIPSE`](../../Reference/mod/MmodDefine.md)type models, and only supports one model per context. The model cannot be calibrated. If the target is associated with a camera calibration context, the model will be interpreted in world units; conversely, if the target does not have a camera calibration context associated with it, the model will be interpreted in pixel units.

This search algorithm is recommended when the objects being sought are elliptical-type shapes (it can also find circles). Besides controllable deformation tolerance ([`M_SAGITTA_TOLERANCE`](../../Reference/mod/MmodControl.md)), it allows finding ellipses at varying aspect ratios ([`M_MODEL_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) and [`M_MODEL_ASPECT_RATIO_..._FACTOR`](../../Reference/mod/MmodControl.md)).

When using this type of Model Finder context, [`M_SEARCH_ANGLE_RANGE`](../../Reference/mod/MmodControl.md), [`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md), and [`M_SEARCH_SCALE_RANGE`](../../Reference/mod/MmodControl.md) are turned off by default and cannot be set to [`M_ENABLE`](../../Reference/mod/MmodControl.md).

When using this type of context, you must also allocate an [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAllocResult.md) Model Finder result buffer. |
| `M_SHAPE_RECTANGLE` | Specifies that the Model Finder context uses a rectangular model search algorithm.

This context only supports [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md)type models, and only supports one model per context. The model cannot be calibrated. If the target is associated with a camera calibration context, the model will be interpreted in world units; conversely, if the target does not have a camera calibration context associated with it, the model will be interpreted in pixel units.

This search algorithm is recommended when the objects being sought are rectangular-type shapes (it can also find squares). Besides controllable deformation tolerance ([`M_DEVIATION_TOLERANCE`](../../Reference/mod/MmodControl.md)), it allows finding rectangles at varying aspect ratios. However, at least a portion of each of their sides must be visible; that is, the model coverage on any side cannot be 0 ([`M_MIN_SIDE_COVERAGE`](../../Reference/mod/MmodControl.md)).

When using this type of context, you must also allocate an [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAllocResult.md) Model Finder result buffer. |
| `M_SHAPE_SEGMENT` | Specifies that the Model Finder context uses a segment model search algorithm.

This context only supports [`M_SEGMENT`](../../Reference/mod/MmodDefine.md)type models, and only supports one model per context. The model cannot be calibrated. If the target is associated with a camera calibration context, the model will be interpreted in world units; conversely, if the target does not have a camera calibration context associated with it, the model will be interpreted in pixel units.

This search algorithm is recommended when the objects being sought are segment-type shapes. Besides controllable deformation tolerance ([`M_DEVIATION_TOLERANCE`](../../Reference/mod/MmodControl.md)), it allows finding segments lying on contours.

When using this type of context, you must also allocate an [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAllocResult.md) Model Finder result buffer. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ContextIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Model Finder context identifier or specifies the data type that the function should use to return the Model Finder context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Model Finder context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Model Finder context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_MOD_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Model Finder context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the circular Model Finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated circular Model Finder context.

If allocation fails, [`M_NULL`](../../Reference/mod/MmodAlloc.md) is written as the identifier. |
| `Address in which to write the controlled geometric Model Finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated controlled geometric Model Finder context.

If allocation fails, [`M_NULL`](../../Reference/mod/MmodAlloc.md) is written as the identifier. |
| `Address in which to write the elliptical Model Finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated elliptical Model Finder context.

If allocation fails, [`M_NULL`](../../Reference/mod/MmodAlloc.md) is written as the identifier. |
| `Address in which to write the geometric Model Finder context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated geometric Model Finder context.

If allocation fails, [`M_NULL`](../../Reference/mod/MmodAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Model Finder context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_MOD_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/mod/MmodAlloc.md) was specified).
