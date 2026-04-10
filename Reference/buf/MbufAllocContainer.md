---
doctype: Reference
module: buf
function: MbufAllocContainer
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufAllocContainer"
---

# MbufAllocContainer

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Allocate a container.

## Syntax

```c
AIL_ID MbufAllocContainer(
    AIL_ID    SystemId,          //in
    AIL_INT64 Attribute,         //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID *  ContainerBufIdPtr  //out
)
```

## Description

This function allocates a container on the specified system. A container is a data object that stores related buffers, referred to as the container's buffer components. By default, a container is empty. Components are automatically added to a container, when you grab into it or use it as a destination of a supported function. You can also add components to a container using [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md).

After allocating the container, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the container identifier returned is not [`M_NULL`](../../Reference/buf/MbufAllocContainer.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufAllocContainer.md) was specified).

When the container is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufAllocContainer.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufAllocContainer.md) was specified, the smart identifier manages the container's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library system on which to allocate the container. This parameter should be set to one of the following values:

*For specifying the Aurora Imaging Library system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `Attribute` *(in, AIL_INT64)*

Specifies the container usage.

*For specifying the container usage*

| Value | Description |
| --- | --- |
| `M_DISP` | Specifies that image buffer components of the container can be displayed, and that the container can become 3D-displayable. You can inquire whether a container is currently 3D-displayable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)with[`M_3D_DISPLAYABLE`](../../Reference/buf/MbufInquireContainer.md). |
| `M_GRAB` | Specifies that the image buffer components of the container can be used to grab data, and that the container itself can be used to grab data. |
| `M_PROC` | Specifies that image buffer components of the container can be processed, and that the container can become 3D-processable. You can inquire whether a container is currently 3D-processable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)with[`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) for a point cloud container or with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md) for a depth map container. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContainerBufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the container identifier or specifies the data type that the function should use to return the container identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated container; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated container; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the container (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated container.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufAllocContainer.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the container identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufAllocContainer.md) was specified).
