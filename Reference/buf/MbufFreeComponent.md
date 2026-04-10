---
doctype: Reference
module: buf
function: MbufFreeComponent
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufFreeComponent"
---

# MbufFreeComponent

> Free one or more buffer components of a specified container.

## Syntax

```c
void MbufFreeComponent(
    AIL_ID    ContainerBufId,  //out
    AIL_INT64 Component,       //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function deallocates one or more previously allocated buffer components of the specified container. The memory reserved for the specified component is released.

Child buffers associated with the component must be deallocated, using [`MbufFree`](../../Reference/buf/MbufFree.md), prior to deallocating the component.

All buffers allocated on a particular system must be freed before the system can be freed.

## Parameters

### `ContainerBufId` *(out, AIL_ID)*

Specifies the Aurora Imaging Library identifier of the container which contains the component(s) to be freed.

### `Component` *(in, AIL_INT64)*

Specifies the criterion used to determine which component(s) will be freed from the container. For all settings except [`M_COMPONENT_ALL`](../../Reference/buf/MbufFreeComponent.md) or a component type, an error will be generated if no components of the container specified in [`ContainerBufId`](../../Reference/buf/MbufFreeComponent.md) meet the criterion.

*For specifying which component to free using a uniquely identifying criterion*

| Value | Description |
| --- | --- |
| `M_COMPONENT_BY_ID` | Specifies the component with the specified Aurora Imaging Library identifier. |
| `M_COMPONENT_BY_INDEX` | Specifies the component with the specified index in the container. |

*For specifying which components to free using a generalized criterion*

| Value | Description |
| --- | --- |
| `M_COMPONENT_BY_GROUP_ID` | Specifies that the component(s) have the specified group ID. The group ID of a component can be inquired using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_COMPONENT_GROUP_ID`](../../Reference/buf/MbufInquireContainer.md). |
| `M_COMPONENT_BY_REGION_ID` | Specifies that the component(s) have the specified region ID. The region ID of a component can be inquired using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_COMPONENT_REGION_ID`](../../Reference/buf/MbufInquireContainer.md). |
| `M_COMPONENT_BY_SOURCE_ID` | Specifies that the component(s) have the specified source ID. The source ID of a component can be inquired using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_COMPONENT_SOURCE_ID`](../../Reference/buf/MbufInquireContainer.md). |
| `M_COMPONENT_ALL` | Specifies all components of the container. |

*For specifying the component to free by component type*

| Value | Description |
| --- | --- |
| `M_COMPONENT_CONFIDENCE` | Specifies the components that store confidence information for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufFreeComponent.md)or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufFreeComponent.md)component of the container. |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies the components that store the A coordinate map provided by the camera. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies the components that store the B coordinate map provided by the camera. |
| `M_COMPONENT_CUSTOM + n` | Specifies the components that have a custom component type, identified by _n_, where n can be a value between 0 and 254. |
| `M_COMPONENT_DISPARITY` | Specifies the components that store a disparity map. |
| `M_COMPONENT_INFRARED` | Specifies the components that store an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies the components that store an intensity image of visible light. |
| `M_COMPONENT_MATRIX_AIL` | Specifies the components that store a 4x4 matrix that transforms depth map coordinates. |
| `M_COMPONENT_MESH_AIL` | Specifies the components that storemesh information for an [`M_COMPONENT_RANGE`](../../Reference/buf/MbufFreeComponent.md)component of the container. |
| `M_COMPONENT_METADATA` | Specifies the components that store metadata information. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies the components that store an intensity image where each band represents the intensity of a specific wavelength of light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies the components that store normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufFreeComponent.md)component of the container. |
| `M_COMPONENT_RANGE` | Specifies the components that store 3D distance/position information. |
| `M_COMPONENT_REFLECTANCE` | Specifies the components that store a reflectance map. |
| `M_COMPONENT_REGION_AIL` | Specifies the components that store a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufFreeComponent.md) component of the container. |
| `M_COMPONENT_SCATTER` | Specifies the components that store a scatter map. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies the components that store an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies the components that store information of an unknown type. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
