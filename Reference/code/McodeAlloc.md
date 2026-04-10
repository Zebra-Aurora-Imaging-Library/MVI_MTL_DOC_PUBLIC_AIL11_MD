---
doctype: Reference
module: code
function: McodeAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeAlloc"
---

# McodeAlloc

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

> Allocate a code context.

## Syntax

```c
AIL_ID McodeAlloc(
    AIL_ID    SysId,            //in
    AIL_INT64 ContextType,      //in
    AIL_INT64 ControlFlag,      //in
    AIL_ID *  ContextCodeIdPtr  //out
)
```

## Description

This function allocates a code context. A code context is an Aurora Imaging Library object that stores one or more code models. The code model is used by the [`McodeRead`](../../Reference/code/McodeRead.md),[`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), and [`McodeWrite`](../../Reference/code/McodeWrite.md)operations.

To add code models to the context, use [`McodeModel`](../../Reference/code/McodeModel.md). A code context can contain multiple code models of 1D code types (excluding GS1 databar, Planet, Postnet, and 4-state); for other code types, a code context can contain at most one code model. To identify the types of code occurrences in an image, you can perform a [`McodeDetect`](../../Reference/code/McodeDetect.md) operation; note that a code context is not required for a [`McodeDetect`](../../Reference/code/McodeDetect.md) operation.

To adjust code model settings, use [`McodeControl`](../../Reference/code/McodeControl.md).

When allocating a code context, specify whether the context should be initialized for a robust or a typical [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) operation. The initial configuration of the code context establishes the default values for some code context and code model settings. To change the initial configuration at a later date, use [`McodeControl`](../../Reference/code/McodeControl.md) with[`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md). The explicit restrictions are indicated within the description of each code model setting; see [`McodeControl`](../../Reference/code/McodeControl.md) for details.

To read and grade code occurrences in an image, use [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md), respectively. The module only supports searching for multiple occurrences of 1D codes (excluding 4-State, GS1 Databar, Planet, and Postnet code types) and 2D Data Matrix codes. For other types of code occurrences, the module only supports searching for a single occurrence. When reading or grading multiple code occurrences, all results can be retrieved using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_ALL`](../../Reference/code/McodeGetResult.md). Each occurrence has a set of results.

To establish the best possible control type settings given your images, use [`McodeTrain`](../../Reference/code/McodeTrain.md).

When the code context is no longer required, release it using[`McodeFree`](../../Reference/code/McodeFree.md)unless [`M_UNIQUE_ID`](../../Reference/code/McodeAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/code/McodeAlloc.md) was specified, the smart identifier manages the code context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the context type. Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the initial configuration for the code context. The restrictions are indicated within each control, see [`McodeControl`](../../Reference/code/McodeControl.md) for details.

*For specifying the initial configuration of the code context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_IMPROVED_RECOGNITION` | Specifies a code context that might provide a more robust [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) operation. This initialization mode is recommended if your target images lack similarity (for example, some but not all include complex or noisy backgrounds), contain distorted code occurrences, or contain code occurrences that are at an angle or even flipped. It is also recommended when searching for 2D matrix codes that are composed of dots.

> **Note:** Note that when selecting this mode, the default of the affected code context and code model settings might change between Aurora Imaging Library releases to improve robustness, given new features or standard recommendations.

This initialization mode is not recommended with [`M_PHARMACODE`](../../Reference/code/McodeModel.md) code types. |
| `M_TYPICAL_RECOGNITION` *(default)* | Specifies a code context that might provide a quicker [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, but might potentially produce less robust results. |

### `ContextCodeIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the code context identifier or specifies the data type that the function should use to return the code context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated code context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated code context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_CODE_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the code context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated code context.

If allocation fails, [`M_NULL`](../../Reference/code/McodeAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the code context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_CODE_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/code/McodeAlloc.md) was specified).
