---
doctype: Reference
module: buf
function: MbufChildContainer
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufChildContainer"
---

# MbufChildContainer

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

> Allocate a child container.

## Syntax

```c
AIL_ID MbufChildContainer(
    AIL_ID            ParentContainerBufId,       //in
    AIL_INT           ComponentCriteriaSize,      //in
    const AIL_INT64 * ComponentCriteriaArrayPtr,  //in
    AIL_INT64         ControlFlag,                //in
    AIL_ID *          ContainerBufIdPtr           //out
)
```

## Description

This function allocates a child container that behaves as though it contains components of the specified, previously allocated container. Which components are in the child container is determined dynamically by filtering the contents of the parent container using the specified criteria. Whenever the parent container is modified, Aurora Imaging Library determines the current contents of the child container.

For example, child containers are useful for working with 3D sensors that transmit multiple sets of components. If your 3D sensor transmits 3 range and confidence components with different group IDs in a single grab, you can create a child container of the grab container with the criteria **M_COMPONENT_BY_GROUP_ID()**; this allocates a child container that only has the range and confidence components of a single group. Each time you grab into the parent container, the child container will be updated with the components of that group (even if components were reallocated). You can pass this child container as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

> **Note:** You can create multiple child containers of a single parent container. You can also create a child container of a child container.

To allocate a child container, you must pass an array of criteria to [`ComponentCriteriaArrayPtr`](../../Reference/buf/MbufChildContainer.md). If you set[`ControlFlag`](../../Reference/buf/MbufChildContainer.md)to [`M_ANY_CRITERIA`](../../Reference/buf/MbufChildContainer.md), the child container will contain the components of the parent container which meet at least one of the criteria. If you set[`ControlFlag`](../../Reference/buf/MbufChildContainer.md)to [`M_ALL_CRITERIA`](../../Reference/buf/MbufChildContainer.md), the child container will only contain the components of the parent container that meet all of the specified criteria. Once you have allocated the child container, you cannot change these criteria. To remove a component from the child container, you can either free the component from the parent container (using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md)) or allocate a child container of the child container that has additional criteria. To add a component to the child container, you must allocate a new component for the parent container (using[`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md)) that meets the existing criteria.

A component of a child container is not allocated in its own memory space; it remains a component of the parent container. Therefore, any modification to a component in the child container affects that component in the parent and vice versa.

A child container is considered a container in its own right, and can be used in some of the same circumstances as its parent container. You cannot directly allocate or free components of a child container. A child container therefore cannot be passed as a destination for a grab, copy, or processing function; it can, however, be passed as a source to relevant functions.

A child container inherits its attributes (such as [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)) from the parent container.

After allocating the child container, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the child container identifier returned is not [`M_NULL`](../../Reference/buf/MbufChildContainer.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufChildContainer.md) was specified).

When the child container is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufChildContainer.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufChildContainer.md) was specified, the smart identifier manages the child container's lifetime and you must not manually free it.

## Parameters

### `ParentContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the parent container.

### `ComponentCriteriaSize` *(in, AIL_INT)*

Specifies the number of elements in the array of criteria that will be applied to the parent container.

### `ComponentCriteriaArrayPtr` *(in, *AIL_INT64)*

Specifies the address of an array of criteria that will be used to determine which components from the parent container to make part of the child container.

*For specifying which component to include in the child container using a uniquely identifying criterion*

| Value | Description |
| --- | --- |
| `M_COMPONENT_BY_ID` | Specifies the component with the specified Aurora Imaging Library identifier. |
| `M_COMPONENT_BY_INDEX` | Specifies the component with the specified index in the container. |

*For specifying which components to include in the child container using a generalized criterion*

| Value | Description |
| --- | --- |
| `M_COMPONENT_BY_GROUP_ID` | Specifies that the component(s) have the specified group ID. |
| `M_COMPONENT_BY_REGION_ID` | Specifies that the component(s) have the specified region ID. |
| `M_COMPONENT_BY_SOURCE_ID` | Specifies that the component(s) have the specified source ID. |

*For specifying the component type selection criteria*

| Value | Description |
| --- | --- |
| *(see [`ForSpecifyingComponentCriteria`](Reference/buf/MbufFreeComponent.md))* |  |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to have the child container contain the components that meet at least one of the criteria, or only the components that meet all of the criteria.

*For specifying whether the child container should contain components that meet any or all of the criteria*

| Value | Description |
| --- | --- |
| `M_ALL_CRITERIA` | Specifies that the child container will only contain the components of the parent container which meet all of the specified criteria. |
| `M_ANY_CRITERIA` | Specifies that the child container will contain the components of the parent container which meet at least one of the specified criteria. |

### `ContainerBufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the child container identifier or specifies the data type that the function should use to return the child container identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated child container; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated child container; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the child container (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated child container.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufChildContainer.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the child container identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufChildContainer.md) was specified).
