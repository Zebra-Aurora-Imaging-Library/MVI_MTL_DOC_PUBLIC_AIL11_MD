---
doctype: Reference
module: meas
function: MmeasAllocMarker
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasAllocMarker"
---

# MmeasAllocMarker

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

> Allocate a measurement marker buffer.

## Syntax

```c
AIL_ID MmeasAllocMarker(
    AIL_ID    SysId,        //in
    AIL_INT64 MarkerType,   //in
    AIL_INT64 ControlFlag,  //in
    AIL_ID *  MarkerIdPtr   //out
)
```

## Description

This function allocates a measurement marker buffer on the specified system. You can allocate an edge, stripe, circle, or point marker buffer.

Once allocated, you can specify a marker's essential characteristics using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md); for an edge, stripe, or circle marker, you can also specify score characteristics using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md). For an edge, stripe, or circle marker, Aurora Imaging Library uses both essential and score characteristics to find the marker in a target image when you call [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md). You cannot search for a point marker; you must define it at a specific location with [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). A point marker is typically used as a reference position when taking measurements between two markers using [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md).

By default, when you allocate a marker buffer, it defines a single-occurrence marker. However, using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md), you can define an edge, stripe, or point marker as a multiple-occurrence marker. For edge and stripe markers, this allows you to search for multiple instances of the same image characteristics. For point markers, this allows you to define equidistant reference positions. You cannot define a multiple-occurrence circle marker.

When the measurement marker buffer is no longer required, release it using[`MmeasFree`](../../Reference/meas/MmeasFree.md)unless [`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocMarker.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocMarker.md) was specified, the smart identifier manages the measurement marker buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the marker buffer.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `MarkerType` *(in, AIL_INT64)*

Specifies the type of marker buffer to allocate.

*For specifying the type of marker buffer to allocate*

| Value | Description |
| --- | --- |
| `M_CIRCLE` | Specifies a circle marker buffer. |
| `M_EDGE` | Specifies an edge marker buffer. |
| `M_POINT` | Specifies a point marker buffer. |
| `M_STRIPE` | Specifies a stripe marker buffer. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `MarkerIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the measurement marker buffer identifier or specifies the data type that the function should use to return the measurement marker buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated measurement marker
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated measurement marker
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_MEAS_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the measurement marker
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated measurement marker buffer.

If allocation fails, [`M_NULL`](../../Reference/meas/MmeasAllocMarker.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the measurement marker buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_MEAS_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocMarker.md) was specified).
