---
doctype: Reference
module: buf
function: MbufAllocDefault
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufAllocDefault"
---

# MbufAllocDefault

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

> Allocate an image buffer or a container (automatically determined by Aurora Imaging Library) with settings appropriate for grabbing using the specified digitizer.

## Syntax

```c
AIL_ID MbufAllocDefault(
    AIL_ID    SystemId,               //in
    AIL_ID    ReferenceDigId,         //in
    AIL_INT64 Attribute,              //in
    AIL_INT64 ControlFlag,            //in
    AIL_INT64 ControlValue,           //in
    AIL_ID *  VarContainerOrBufIdPtr  //out
)
```

## Description

This function allocates an image buffer or a container (automatically determined by Aurora Imaging Library) on the specified system with settings appropriate for grabbing using the specified digitizer. For example, when the digitizer is associated with a 2D camera, typically an image buffer is allocated. If the digitizer is associated with a 3D sensor, typically a container is allocated. When an image buffer is allocated, any required attributes (such as buffer size) are set automatically.

After allocating the image buffer or container, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the image buffer or container identifier returned is not [`M_NULL`](../../Reference/buf/MbufAllocDefault.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufAllocDefault.md) was specified).

When the image buffer or container is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufAllocDefault.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufAllocDefault.md) was specified, the smart identifier manages the image buffer or container's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library system on which to allocate the image buffer or the container. This parameter should be set to one of the following values:

*For specifying the Aurora Imaging Library system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ReferenceDigId` *(in, AIL_ID)*

Specifies the digitizer to use as a reference for determining the settings of the image buffer or container to allocate.

### `Attribute` *(in, AIL_INT64)*

Specifies the usage of the image buffer or container.

*For specifying the buffer or container usage*

| Value | Description |
| --- | --- |
| `M_DISP` | Specifies that the object is allocated with the display attribute.

If an image buffer is allocated, this attribute specifies that the image buffer can be displayed.

If a container is allocated, this attribute specifies that the container can become 3D-displayable. To learn how to make a container 3D-displayable, see [Preparing a container for display or processing](../../UserGuide/C41_3D_Containers/S04_Preparing_a_container_for_display_or_processing.md). |
| `M_GRAB` | Specifies that the object is allocated with the grab attribute.

This attribute specifies that the image buffer or container can be used to grab data (for example, using[`MdigGrab`](../../Reference/dig/MdigGrab.md). |
| `M_PROC` | Specifies that the object is allocated with the processable attribute.

If an image buffer is allocated, this attribute specifies that the image buffer can be processed. If you intend to use the image buffer as the source or destination buffer of a processing or analysis operation, you must allocate the image buffer with an [`M_PROC`](../../Reference/buf/MbufAllocDefault.md) attribute.

If a container is allocated, this attribute specifies that the container can become 3D-processable. To learn how to make a container 3D-processable, see [Preparing a container for display or processing](../../UserGuide/C41_3D_Containers/S04_Preparing_a_container_for_display_or_processing.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ControlValue` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `VarContainerOrBufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the image buffer or container identifier or specifies the data type that the function should use to return the image buffer or container identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated image buffer or
                              container; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated image buffer or
                              container; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the image buffer or
                              container (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the container identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated container.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufAllocDefault.md) is written as the identifier. |
| `Address in which to write the image buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated image buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufAllocDefault.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the image buffer or container identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufAllocDefault.md) was specified).

## Remarks

> If you are creating a DLL that includes a call to [`MbufAllocDefault`](../../Reference/buf/MbufAllocDefault.md), ensure that the call is not made from the `DllMain()` function, because [`MbufAllocDefault`](../../Reference/buf/MbufAllocDefault.md) might load a required DLL and you cannot load a DLL from `DllMain()`. If necessary, call [`MbufAllocDefault`](../../Reference/buf/MbufAllocDefault.md) from an initialization function in your DLL instead.
