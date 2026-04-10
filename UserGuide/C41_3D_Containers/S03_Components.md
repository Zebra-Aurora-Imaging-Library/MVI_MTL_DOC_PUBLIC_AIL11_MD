---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Containers
section: Components
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Containers / Components"
---

# Components

The buffers in a container are called its components. In most respects, components are no different from other buffers allocated using the [`MbufAlloc...`](../../Reference/buf/MbufAllocColor.md)functions. Components have their own Aurora Imaging Library buffer identifier and can be used with Aurora Imaging Library functions that take a buffer. However, components are allocated and freed differently from other buffers. Typically, when a container is used as a destination for a grabbing or 3D processing function, that function will allocate and free the components automatically as required. You can allocate components manually using [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md), [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md), or [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md). If you need to free components manually, use[`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md) or free the container using [`MbufFree`](../../Reference/buf/MbufFree.md).

Only containers with the appropriate components can be processed using Aurora Imaging Library's 3D functions. See [Preparing a container for display or processing](S04_Preparing_a_container_for_display_or_processing.md) for more information on 3D-processable point cloud ([`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)) and 3D-processable depth map ([`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md)) containers.

## Component type

Each component has a component type, which indicates the type of information stored in the component. For example, a component with the component type [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md) stores 3D coordinates when the container stores a point cloud. Aurora Imaging Library uses the component type to determine what type of information a component stores about the points in a point cloud container.

Typically, a component's component type is set automatically during acquisition. You can set a component's component type manually using[`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md)with[`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md).

## Group, region, and source ID

If you grab data from a 3D sensor that transmits data in a format suitable for grabbing into a container, it might assign each component a group, region, and/or source ID number. Typically, components which have the same group, region, or source ID are associated in some way; for example, if your 3D sensor transmits the range and confidence components for multiple 3D scenes in a single grab, each set of components might have a different group ID. These component ID numbers can be inquired using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)with [`M_COMPONENT_GROUP_ID`](../../Reference/buf/MbufInquireContainer.md),[`M_COMPONENT_REGION_ID`](../../Reference/buf/MbufInquireContainer.md) and [`M_COMPONENT_SOURCE_ID`](../../Reference/buf/MbufInquireContainer.md) respectively.

You can create a child container that contains only the components of a single group, using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md)with [`M_COMPONENT_BY_GROUP_ID()`](../../Reference/buf/MbufChildContainer.md). You can then call [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)with the child container to convert the 3D data to a 3D-processable point cloud container.

> **Note:** Your 3D sensor might transmit group, region, and source IDs that are not sequential. Refer to your 3D sensor manual to determine what ID numbers it will assign to components.

## Component criterion

Some functions, such as [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md), allow you to perform operations on the components of a specified container (such as freeing those components) by specifying a criterion. In some cases, these criteria (such as [`M_COMPONENT_BY_INDEX()`](../../Reference/buf/MbufInquireContainer.md)) can only refer to a single component in the container. In other cases they can potentially refer to several components in the container (such as[`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufInquireContainer.md), which specifies all intensity components in the container).

By default, 3D functions only apply the operation to core components of a 3D-processable point cloud or depth map container. These components include the range, confidence, reflectance, normals, intensity, mesh, matrix (depth map), and region (depth map); for more information, see [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) and [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md). Some functions (such as [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md)) allow you to apply the operation to all components, including custom components ([`M_COMPONENT_CUSTOM + n`](../../Reference/buf/MbufAllocComponent.md)), if you specify [`M_APPLY_TO_ALL_COMPONENTS`](../../Reference/3dim/M3dimMerge.md). The components must have the same dimensions as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md); otherwise, they will be copied to the destination container unmodified. Note that if a custom component has an associated calibration context, the component's information is modified/copied, but its calibration context is discarded.

> **Note:** Some functions, such as [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md), require that you specify a criterion that identifies only a single component in the container. For these functions, an error will be generated if there is more than one component in the container that meets the specified criterion.
