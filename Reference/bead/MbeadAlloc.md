---
doctype: Reference
module: bead
function: MbeadAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadAlloc"
---

# MbeadAlloc

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

> Allocate a bead context.

## Syntax

```c
AIL_ID MbeadAlloc(
    AIL_ID    SysId,            //in
    AIL_INT64 Strategy,         //in
    AIL_INT64 ControlFlag,      //in
    AIL_ID *  ContextBeadIdPtr  //out
)
```

## Description

This function allocates a bead context on the specified system.

Any value that you provide to the Aurora Imaging Library Bead module (for example, position and width) must be specified in appropriate units (pixel or real-world), depending on whether your images are calibrated. For more information, see [Camera calibration - overview](../../UserGuide/C28_Calibration/S01_Calibration_overview.md). Note that, if you are specifying pixel units, make sure the pixel sizes are consistent across all camera calibrations (for example, pixel sizes should be the same in the training and verification images). When using different camera calibration contexts, it is recommended that you use world units.

When the bead context is no longer required, release it using[`MbeadFree`](../../Reference/bead/MbeadFree.md)unless [`M_UNIQUE_ID`](../../Reference/bead/MbeadAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/bead/MbeadAlloc.md) was specified, the smart identifier manages the bead context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the bead context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `Strategy` *(in, AIL_INT64)*

Specifies the strategy with which to extract and analyze beads. This parameter should be set to the following value:

*For specifying the strategy*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default strategy. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextBeadIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the bead context identifier or specifies the data type that the function should use to return the bead context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated bead context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated bead context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BEAD_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the bead context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated bead context.

If allocation fails, [`M_NULL`](../../Reference/bead/MbeadAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the bead context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BEAD_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/bead/MbeadAlloc.md) was specified).
