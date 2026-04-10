---
doctype: Reference
module: buf
function: MbufCopyComponent
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufCopyComponent"
---

# MbufCopyComponent

> Copy a data buffer, or components of a container, to components of a destination container.

## Syntax

```c
void MbufCopyComponent(
    AIL_ID    SrcContainerOrBufId,  //in
    AIL_ID    DstContainerBufId,    //out
    AIL_INT64 SrcComponent,         //in
    AIL_INT64 Operation,            //in
    AIL_INT64 ControlFlag           //in
)
```

## Description

This function copies the specified data buffer, or components of the specified source container, to the specified destination container. If the source is a container, you must specify which components to copy using[`SrcComponent`](../../Reference/buf/MbufCopyComponent.md).

By default, this function allocates one or more buffers and adds them as components of the destination container. The allocated buffers have the same buffer type, size, number of bands, component type, and some settings (such as 3D settings) as the source buffers. All data and other settings are also copied, unless you set the [`ControlFlag`](../../Reference/buf/MbufCopyComponent.md)parameter to[`M_NO_COPY_SOURCE_DATA`](../../Reference/buf/MbufCopyComponent.md). Some attributes (such as [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)) are not copied by default. You can specify that these attributes are also copied by setting the [`ControlFlag`](../../Reference/buf/MbufCopyComponent.md)parameter to [`M_IDENTICAL`](../../Reference/buf/MbufCopyComponent.md).

Optionally, you can specify to replace matching components in the destination container (in some cases, reusing the existing buffers) by specifying the [`M_REPLACE`](../../Reference/buf/MbufCopyComponent.md)operation.

When copying components from a container, you can force Aurora Imaging Library to use the destination components as-is and not free or reallocate them. To do so, set the [`ControlFlag`](../../Reference/buf/MbufCopyComponent.md)parameter to [`M_USE_DESTINATION`](../../Reference/buf/MbufCopyComponent.md). If there is a mismatch between a component in the source and destination container (for example, the destination component is smaller than the source component), Aurora Imaging Library will attempt to compensate (for example, by only copying some of the data). If there are not the same number of components that meet the specified criteria in the source and destination containers, an error is generated.

## Parameters

### `SrcContainerOrBufId` *(in, AIL_ID)*

Specifies the identifier of the source data buffer or container. When copying a buffer to a container, you should first set the buffer's component type using [`MbufControl`](../../Reference/buf/MbufControl.md)with [`M_COMPONENT_TYPE`](../../Reference/buf/MbufControl.md).

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container.

### `SrcComponent` *(in, AIL_INT64)*

Specifies the criterion which will be used to determine which components of the source container to copy. If the source is a data buffer, this parameter must be set to `M_DEFAULT`.

*For specifying which component to copy using a uniquely identifying criterion*

| Value | Description |
| --- | --- |
| `M_COMPONENT_BY_ID` | Specifies the component with the specified Aurora Imaging Library identifier. |
| `M_COMPONENT_BY_INDEX` | Specifies the component with the specified index in the container. |

*For specifying which component(s) to copy using a generalized criterion*

| Value | Description |
| --- | --- |
| `M_COMPONENT_BY_GROUP_ID` | Specifies that the component(s) have the specified group ID. |
| `M_COMPONENT_BY_REGION_ID` | Specifies that the component(s) have the specified region ID. |
| `M_COMPONENT_BY_SOURCE_ID` | Specifies that the component(s) have the specified source ID. |
| `M_COMPONENT_ALL` | Specifies all components of the container. When used with the 
                                 
                                    M_REPLACE
                                 
                              operation, this frees all existing components in the destination container and copies all components of the source container (unless the  
                                 
                                    ControlFlag
                                 
                              parameter is set to 
                                 
                                    M_USE_DESTINATION
                                 
                              ). |

*For specifying which component(s) to copy by component type*

| Value | Description |
| --- | --- |
| `M_COMPONENT_CONFIDENCE` | Specifies the components that store confidence information for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufCopyComponent.md)or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufCopyComponent.md)component of the container. |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies the components that store the A coordinate map provided by the camera. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies the components that store the B coordinate map provided by the camera. |
| `M_COMPONENT_CUSTOM + n` | Specifies the components that have the specified custom component type, identified by _n_, where n can be a value between 0 and 254. |
| `M_COMPONENT_DISPARITY` | Specifies the components that store a disparity map. |
| `M_COMPONENT_INFRARED` | Specifies the components that store an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies the components that store an intensity image of visible light. |
| `M_COMPONENT_MATRIX_AIL` | Specifies the components that store a 4x4 matrix that transforms depth map coordinates. |
| `M_COMPONENT_MESH_AIL` | Specifies the components that storemesh information for an [`M_COMPONENT_RANGE`](../../Reference/buf/MbufCopyComponent.md)component of the container. |
| `M_COMPONENT_METADATA` | Specifies the components that store metadata information. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies the components that store an intensity image where each band represents the intensity of a specific wavelength of light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies the components that store normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufCopyComponent.md)component of the container. |
| `M_COMPONENT_RANGE` | Specifies the components that store 3D distance/position information. |
| `M_COMPONENT_REFLECTANCE` | Specifies the components that store a reflectance map. |
| `M_COMPONENT_REGION_AIL` | Specifies the components that store a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufCopyComponent.md) component of the container. |
| `M_COMPONENT_SCATTER` | Specifies the components that store a scatter map. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies the components that store an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies the components that store information of an unknown type. |

### `Operation` *(in, AIL_INT64)*

Specifies how to copy the source buffer or component(s) to the destination container.

*For specifying the operation to perform*

| Value | Description |
| --- | --- |
| `M_APPEND` | Specifies that a new buffer is allocated and added as a component of the destination container for each buffer to be copied. |
| `M_REPLACE` | Specifies that existing component(s) in the destination container are replaced with the copied buffer(s).

> **Note:** This operation is not available when[`SrcComponent`](../../Reference/buf/MbufCopyComponent.md)is set to a setting from the table [`For_specifying_the_component_to_control_by_a_unique_identifier`](../../Reference/For_specifying_the_component_to_control_by_a_unique_identifier.md).

For each buffer to be copied, Aurora Imaging Library determines if an existing component meets the same specified criteria and has the same buffer attributes (such as [`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md)) as the source component; if so, it reuses the existing component. If no matching component is found, a new buffer is allocated with the correct attributes and added to the destination container. Any components in the destination container that meet the same specified criterion, but are not used during the copy operation, are freed.

> **Note:** If the source is a buffer, its [`M_COMPONENT_TYPE`](../../Reference/buf/MbufControl.md) setting is used to determine which component in the destination to replace. An error will be generated if there is more than one component with this component type in the destination container. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to copy the data, settings, and attributes. This parameter can be set to one of the following:

*For specifying how the buffer or container is copied*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_BASIC_ATTRIBUTE`](../../Reference/buf/MbufCopyComponent.md). |
| `M_BASIC_ATTRIBUTE` | Specifies to copy the specified buffer (or component(s) of the specified container) to the destination container, including all data and settings. Optional attributes of buffers are not copied. For example, if the source buffer was allocated with the [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)attribute, the destination buffer will be uncompressed. |
| `M_IDENTICAL` | Specifies to copy the specified buffer (or component(s) of the specified container) to the destination container including all data, settings, and attributes (except [`M_PROC`](../../Reference/buf/MbufAllocColor.md),[`M_GRAB`](../../Reference/buf/MbufAllocColor.md)and [`M_DISP`](../../Reference/buf/MbufAllocColor.md)which are always the same as the attributes of the destination container). |
| `M_USE_DESTINATION` | Specifies that all components in the source container that meet the criteria (specified in the [`SrcComponent`](../../Reference/buf/MbufCopyComponent.md) parameter) are copied by rank to existing components in the destination container that meet the criteria. For example, if [`SrcComponent`](../../Reference/buf/MbufCopyComponent.md) is set to**M_COMPONENT_BY_GROUP_ID()**, the first component with the specified group ID in the source container is copied to the first component with that group ID in the destination container, the second component with the specified group ID in the source container is copied to the second component with that group ID in the destination container, and so on.

If the number of components that meet the specified criteria differs between the source container and destination container, an error is generated.

Aurora Imaging Library attempts to compensate for any mismatch in the size and attributes of the source and destination components. For example, if the destination component has the [`M_COMPRESS`](../../Reference/buf/MbufAllocComponent.md) attribute but the source component does not, Aurora Imaging Library automatically compresses the data. If Aurora Imaging Library cannot compensate, an error is generated.

> **Note:** This setting is only available for the [`M_REPLACE`](../../Reference/buf/MbufCopyComponent.md)operation, with a container as a source. |

*For specifying how the source is used*

| Value | Description |
| --- | --- |
| `M_NO_COPY_SOURCE_DATA` | Specifies to allocate component(s) in the destination container with the same component type and attributes as the source buffer or component(s), without copying data from the source. In addition, some settings (such as 3D settings, from the table [`Component3DSettings`](../../Reference/buf/MbufControlContainer.md)) are copied. |
| `M_SOURCE_MUST_EXIST` | Specifies to generate an error if there is not at least one component in the source container that fits the specified criterion.

> **Note:** This setting is only available when the source is a container. |
