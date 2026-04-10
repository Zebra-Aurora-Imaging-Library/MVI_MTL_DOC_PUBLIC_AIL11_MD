---
doctype: Reference
module: reg
function: MregAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregAlloc"
---

# MregAlloc

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

> Allocate a registration context.

## Syntax

```c
AIL_ID MregAlloc(
    AIL_ID    SystemId,          //in
    AIL_INT64 RegistrationType,  //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID *  ContextIdPtr       //out
)
```

## Description

This function allocates a correlation-stitching, depth from focus, extended depth of field (EDoF), high dynamic range, or photometric stereo registration context on the specified system. A registration context contains all the information needed to perform the registration operation, using [`MregCalculate`](../../Reference/reg/MregCalculate.md).

Note that correlation-stitching and photometric stereo contexts contain registration elements and their settings, as well as the global registration settings. When you allocate a correlation-stitching or photometric stereo registration context, it is defined with a default number of registration elements (256 or 16, respectively). You can add or remove registration elements using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/reg/MregControl.md).

When the registration context is no longer required, release it using[`MregFree`](../../Reference/reg/MregFree.md)unless [`M_UNIQUE_ID`](../../Reference/reg/MregAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/reg/MregAlloc.md) was specified, the smart identifier manages the registration context's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the system on which to allocate the context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `RegistrationType` *(in, AIL_INT64)*

Specifies the type of registration context. This parameter defines the registration operation to perform. This parameter should be set to one of the following values:

*For specifying the type of registration context*

| Value | Description |
| --- | --- |
| `M_DEPTH_FROM_FOCUS` | Specifies a registration context for a depth-from-focus registration operation. |
| `M_EXTENDED_DEPTH_OF_FIELD` | Specifies a registration context for an extended depth of field registration operation. |
| `M_HIGH_DYNAMIC_RANGE` | Specifies a registration context for a high dynamic range registration operation. |
| `M_PHOTOMETRIC_STEREO` | Specifies a registration context for a photometric stereo registration operation. |
| `M_STITCHING` | Specifies a registration context for a correlation-stitching registration operation. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the registration context identifier or specifies the data type that the function should use to return the registration context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated registration context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated registration context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_REG_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the registration context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the DFF context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated DFF context.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAlloc.md) is written as the identifier. |
| `Address in which to write the EDOF context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated EDOF context.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAlloc.md) is written as the identifier. |
| `Address in which to write the HDR context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated HDR context.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAlloc.md) is written as the identifier. |
| `Address in which to write the photometric stereo context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated photometric stereo context.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAlloc.md) is written as the identifier. |
| `Address in which to write the stitching context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated stitching context.

If allocation fails, [`M_NULL`](../../Reference/reg/MregAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the registration context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_REG_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/reg/MregAlloc.md) was specified).
