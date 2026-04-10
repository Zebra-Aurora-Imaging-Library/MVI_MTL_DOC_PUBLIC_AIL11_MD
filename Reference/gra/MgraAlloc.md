---
doctype: Reference
module: gra
function: MgraAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / gra / MgraAlloc"
---

# MgraAlloc

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

> Allocate a 2D graphics context.

## Syntax

```c
AIL_ID MgraAlloc(
    AIL_ID   SysId,           //in
    AIL_ID * ContextGraIdPtr  //out
)
```

## Description

This function allocates a 2D graphics context on the specified system. A 2D graphics context is required to use Aurora Imaging Library graphics to perform annotations. Whenever an annotation is done, 2D graphics context settings are inherited by the graphics. Different contexts can coexist; use their identifier to specify which to use or change.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) and [`MgraInquire`](../../Reference/gra/MgraInquire.md).

Note that, upon allocation of an application, a default 2D graphics context is automatically allocated. Rather than using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) to allocate a 2D graphics context, you can use this default 2D graphics context, by specifying [`M_DEFAULT`](../../Reference/gra/MgraInquire.md) wherever a 2D graphics context identifier is required. Note that there is a different default 2D graphics context for each thread.

When the 2D graphics context is no longer required, release it using[`MgraFree`](../../Reference/gra/MgraFree.md)unless [`M_UNIQUE_ID`](../../Reference/gra/MgraAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/gra/MgraAlloc.md) was specified, the smart identifier manages the 2D graphics context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the 2D graphics context.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextGraIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the 2D graphics context identifier or specifies the data type that the function should use to return the 2D graphics context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated 2D graphics context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated 2D graphics context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_GRA_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the 2D graphics context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 2D graphics context.

If allocation fails, [`M_NULL`](../../Reference/gra/MgraAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the 2D graphics context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_GRA_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/gra/MgraAlloc.md) was specified).
