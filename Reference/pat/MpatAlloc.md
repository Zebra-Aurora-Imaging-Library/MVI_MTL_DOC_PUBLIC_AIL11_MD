---
doctype: Reference
module: pat
function: MpatAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / pat / MpatAlloc"
---

# MpatAlloc

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

> Allocate a Pattern Matching context.

## Syntax

```c
AIL_ID MpatAlloc(
    AIL_ID    SysId,           //in
    AIL_INT64 ContextType,     //in
    AIL_INT64 ControlFlag,     //in
    AIL_ID *  ContextPatIdPtr  //out
)
```

## Description

This function allocates a Pattern Matching context on the specified system. A Pattern Matching context contains all the information necessary to perform an[`MpatFind`](../../Reference/pat/MpatFind.md) operation on a target image, including global search settings and one or more models.

Define search models and add to them to the Pattern Matching context using[`MpatDefine`](../../Reference/pat/MpatDefine.md). Note that you must add at least one model to the context.

You can adjust the Pattern Matching context settings using [`MpatControl`](../../Reference/pat/MpatControl.md). To inquire the Pattern Matching context settings, use [`MpatInquire`](../../Reference/pat/MpatInquire.md).

When the Pattern Matching context is no longer required, release it using[`MpatFree`](../../Reference/pat/MpatFree.md)unless [`M_UNIQUE_ID`](../../Reference/pat/MpatAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/pat/MpatAlloc.md) was specified, the smart identifier manages the Pattern Matching context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the Pattern Matching context.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, which you have allocated using the [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) function. |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of Pattern Matching context. The parameter must be set to the following:

*For the types of Pattern Matching context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NORMALIZED` *(default)* | Specifies a normalized grayscale correlation (NGC) Pattern Matching context. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ContextPatIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Pattern Matching context identifier or specifies the data type that the function should use to return the Pattern Matching context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Pattern Matching context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Pattern Matching context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_PAT_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Pattern Matching context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated Pattern Matching context.

If allocation fails, [`M_NULL`](../../Reference/pat/MpatAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the Pattern Matching context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_PAT_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/pat/MpatAlloc.md) was specified).
