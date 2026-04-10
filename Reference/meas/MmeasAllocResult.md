---
doctype: Reference
module: meas
function: MmeasAllocResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasAllocResult"
---

# MmeasAllocResult

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

> Allocate a measurement result buffer.

## Syntax

```c
AIL_ID MmeasAllocResult(
    AIL_ID    SysId,           //in
    AIL_INT64 ResultType,      //in
    AIL_ID *  MeasResultIdPtr  //out
)
```

## Description

This function allocates a measurement result buffer, on the specified system, to store results obtained from an [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) operation. Note that you do not need a result buffer for an [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) operation; results are stored with the marker.

When the measurement result buffer is no longer required, release it using[`MmeasFree`](../../Reference/meas/MmeasFree.md)unless [`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocResult.md) was specified, the smart identifier manages the measurement result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the result buffer.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result buffer to allocate.

*For specifying the type of result buffer to allocate*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CALCULATE`](../../Reference/meas/MmeasAllocResult.md). |
| `M_CALCULATE` | Allocates a result buffer for an [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) operation. |

### `MeasResultIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the measurement result buffer identifier or specifies the data type that the function should use to return the measurement result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated measurement result
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated measurement result
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_MEAS_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the measurement result
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated measurement result buffer.

If allocation fails, [`M_NULL`](../../Reference/meas/MmeasAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the measurement result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_MEAS_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocResult.md) was specified).
