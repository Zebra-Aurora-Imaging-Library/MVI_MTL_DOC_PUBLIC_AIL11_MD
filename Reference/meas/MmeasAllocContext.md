---
doctype: Reference
module: meas
function: MmeasAllocContext
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasAllocContext"
---

# MmeasAllocContext

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

> Allocate a measurement context.

## Syntax

```c
AIL_ID MmeasAllocContext(
    AIL_ID    SysId,        //in
    AIL_INT64 ControlFlag,  //in
    AIL_ID *  ContextIdPtr  //out
)
```

## Description

This function allocates a measurement context on the specified system. Aurora Imaging Library uses a measurement context to control the behavior of measurement operations ([`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) and [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)). You can modify the settings of a measurement context with [`MmeasControl`](../../Reference/meas/MmeasControl.md).

Aurora Imaging Library allows you to specify **M_DEFAULT** as the identifier of the measurement context, when using [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md), [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md), and [`MmeasInquire`](../../Reference/meas/MmeasInquire.md); therefore, you need not call [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md). However, you cannot specify **M_DEFAULT** as the identifier of the measurement context if want to modify it using [`MmeasControl`](../../Reference/meas/MmeasControl.md); to modify a measurement context, you must actually allocate it using [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md).

When the measurement context is no longer required, release it using[`MmeasFree`](../../Reference/meas/MmeasFree.md)unless [`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocContext.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocContext.md) was specified, the smart identifier manages the measurement context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which the to allocate the measurement context.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the measurement context identifier or specifies the data type that the function should use to return the measurement context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated measurement context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated measurement context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_MEAS_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the measurement context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated measurement context.

If allocation fails, [`M_NULL`](../../Reference/meas/MmeasAllocContext.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the measurement context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_MEAS_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocContext.md) was specified).
