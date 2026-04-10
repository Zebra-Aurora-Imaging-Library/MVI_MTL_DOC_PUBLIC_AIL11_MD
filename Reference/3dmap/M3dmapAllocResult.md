---
doctype: Reference
module: 3dmap
function: M3dmapAllocResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap / M3dmapAllocResult"
---

# M3dmapAllocResult

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

> Allocate a 3D reconstruction or alignment result buffer.

## Syntax

```c
AIL_ID M3dmapAllocResult(
    AIL_ID    SysId,            //in
    AIL_INT64 ResultType,       //in
    AIL_INT64 ControlFlag,      //in
    AIL_ID *  Result3dmapIdPtr  //out
)
```

## Description

This function allocates a 3D reconstruction or alignment result buffer on the specified system.

When the 3D reconstruction or alignment result buffer is no longer required, release it using[`M3dmapFree`](../../Reference/3dmap/M3dmapFree.md)unless [`M_UNIQUE_ID`](../../Reference/3dmap/M3dmapAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/3dmap/M3dmapAllocResult.md) was specified, the smart identifier manages the 3D reconstruction or alignment result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the result buffer.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result buffer to allocate. This parameter must be set to the following value:

*For specifying the type of result buffer to allocate*

| Value | Description |
| --- | --- |
| `M_ALIGN_RESULT` | Specifies to allocate a 3D alignment result buffer. This result buffer holds the results produced from calling[`M3dmapAlignScan`](../../Reference/3dmap/M3dmapAlignScan.md) with a 3D alignment context ([`M_ALIGN_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md)). |
| `M_DEPTH_CORRECTED_DATA` | Specifies to allocate a 3D reconstruction result buffer used to store results generated in [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) mode, after the 3D reconstruction context has been calibrated. |
| `M_LASER_CALIBRATION_DATA` | Specifies to allocate a 3D reconstruction result buffer used to store images of laser line displacement at specified heights during the 3D reconstruction calibration process. |
| `M_POINT_CLOUD_RESULT` | Specifies to allocate a 3D reconstruction result buffer used to store results generated in [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) mode, after the 3D reconstruction context has been calibrated. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Result3dmapIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 3D reconstruction or alignment result buffer identifier or specifies the data type that the function should use to return the 3D reconstruction or alignment result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D reconstruction or alignment result
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 3D reconstruction or alignment result
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_3DMAP_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 3D reconstruction or alignment result
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the 3D alignment result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 3D alignment result buffer.

If allocation fails, [`M_NULL`](../../Reference/3dmap/M3dmapAllocResult.md) is written as the identifier. |
| `Address in which to write the 3D reconstruction result buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 3D reconstruction result buffer.

If allocation fails, [`M_NULL`](../../Reference/3dmap/M3dmapAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 3D reconstruction or alignment result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_3DMAP_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/3dmap/M3dmapAllocResult.md) was specified).
