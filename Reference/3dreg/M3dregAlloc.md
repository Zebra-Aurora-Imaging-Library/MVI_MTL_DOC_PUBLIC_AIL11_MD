---
doctype: Reference
module: 3dreg
function: M3dregAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregAlloc"
---

# M3dregAlloc

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

> Allocate a 3D registration context.

## Syntax

```c
AIL_ID M3dregAlloc(
    AIL_ID    SysId,             //in
    AIL_INT64 ContextType,       //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID *  Context3dregIdPtr  //out
)
```

## Description

This function allocates a 3D registration context on the specified system. A 3D registration context contains information needed to perform an [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md)operation. You can also use [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) to allocate a draw 3D registration context for drawing results using [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md).

A pairwise 3D registration context contains global registration settings and registration elements; each registration element stores the registration information for a single point cloud. Specify how many registration elements are in the 3D registration context using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/3dreg/M3dregControl.md). This registration information is set using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) and [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md).

When the 3D registration context is no longer required, release it using[`M3dregFree`](../../Reference/3dreg/M3dregFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dreg/M3dregAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dreg/M3dregAlloc.md) was specified, the smart identifier manages the 3D registration context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the 3D registration context. Set this parameter to one of the values below:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of 3D registration context to allocate. Set this parameter to one of the values below:

*For specifying the type of 3D registration context*

| Value | Description |
| --- | --- |
| `M_DRAW_3D_CONTEXT` | Allocates a draw 3D registration context for use with [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md). |
| `M_PAIRWISE_REGISTRATION_CONTEXT` | Specifies a pairwise 3D registration context for use with [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Context3dregIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D registration context identifier or specifies the data type that the function should use to return the 3D registration context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D registration context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D registration context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DREG_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D registration context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the draw 3D registration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated draw 3D registration context.

If allocation fails, [`M_NULL`](../../Reference/3dreg/M3dregAlloc.md) is written as the identifier. |
| `Address in which to write the pairwise 3D registration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated pairwise 3D registration context.

If allocation fails, [`M_NULL`](../../Reference/3dreg/M3dregAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D registration context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DREG_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dreg/M3dregAlloc.md) was specified).
