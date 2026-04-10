---
doctype: Reference
module: str
function: MstrAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrAlloc"
---

# MstrAlloc

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

> Allocate a String Reader context.

## Syntax

```c
AIL_ID MstrAlloc(
    AIL_ID    SysId,        //in
    AIL_INT64 ContextType,  //in
    AIL_INT64 ControlFlag,  //in
    AIL_ID *  ObjectIdPtr   //out
)
```

## Description

This function allocates a String Reader context on the specified system. A String Reader context contains the read parameters, the list of fonts, and the list of string models.

> **Note:** Note that you cannot allocate a fontless context. You must restore one of the predefined fontless context that is distributed with Aurora Imaging Library using [`MstrRestore`](../../Reference/str/MstrRestore.md).

When the String Reader context is no longer required, release it using[`MstrFree`](../../Reference/str/MstrFree.md)unless [`M_UNIQUE_ID`](../../Reference/str/MstrAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/str/MstrAlloc.md) was specified, the smart identifier manages the String Reader context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the String Reader context.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of String Reader context. This parameter must be set to the value below.

*For specifying the context*

| Value | Description |
| --- | --- |
| `M_FONT_BASED` | Specifies a font-based context. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ObjectIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the String Reader context identifier or specifies the data type that the function should use to return the String Reader context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated String Reader context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated String Reader context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_STR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the String Reader context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated String Reader context.

If allocation fails, [`M_NULL`](../../Reference/str/MstrAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the String Reader context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_STR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/str/MstrAlloc.md) was specified).
