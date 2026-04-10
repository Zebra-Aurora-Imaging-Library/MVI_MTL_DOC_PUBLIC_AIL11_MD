---
doctype: Reference
module: 3dim
function: M3dimRemovePoints
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimRemovePoints"
---

# M3dimRemovePoints

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

> Remove 3D points from a point cloud, based on a specified condition.

## Syntax

```c
void M3dimRemovePoints(
    AIL_ID    SrcContainerBufId,  //in
    AIL_ID    DstContainerBufId,  //out
    AIL_INT64 Option,             //in
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function removes 3D points from a point cloud, based on a specified condition (for example, all invalid points).

The specified points are removed from the point cloud, not just invalidated with the confidence component. The mesh component (if existing) is recalculated for the remaining points. If the point cloud has both intensity and reflectance components, the points' color or intensity data is assumed to be stored in the reflectance component. Consequently, the specified points' color or intensity data is removed from the reflectance component, while the intensity component is copied unchanged into the destination container. This is because, when both components exist in a point cloud, the intensity component often stores a photographic image of the source scene. A data correlation might or might not exist between the image and the container's 3D points, depending on the type of 3D sensor used to generate the data. If the intensity component or the reflectance component exists but not both, it is modified and copied to the destination container.

> **Note:** Note that, if an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source container, normal vectors of removed points are also removed; remaining point normals are not recalculated.

The destination point cloud is always unorganized, meaning its data is stored in the destination container with 1D components.

## Parameters

### `SrcContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the source point cloud container. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)).

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container in which to place the modified point cloud. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container.

### `Option` *(in, AIL_INT64)*

Specifies which points to remove.

*For specifying the option*

| Value | Description |
| --- | --- |
| `M_INVALID_NORMALS` | Specifies to remove points whose confidence is equal to zero, as well as points whose normal vector values are zero. Note that other points with non-unit normal vectors are not removed. This option is available only when the component [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) exists in the source point cloud container. |
| `M_INVALID_POINTS_ONLY` | Specifies to remove points whose confidence is equal to zero. |
| `M_NOT_MESH_POINTS` | Specifies to remove points whose confidence is equal to zero, as well as points that are not part of the mesh. This option is available only when the component [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) exists in the source point cloud container. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies which components will be affected by the operation.

*For specifying whether to remove points from all components*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the operation will only affect the core components of a 3D-processable container. These core components are listed in the description of [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md).

All other components are copied to the destination container unmodified. |
| `M_APPLY_TO_ALL_COMPONENTS` | Specifies that the operation will affect the core components of a 3D-processable container, as well as any other component (including custom components) that has the same dimension as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md). These components must not have a region of interest (ROI) associated with them. For example, using a component that is an image buffer with an ROI will cause an error.

> **Note:** Note that if a custom component has an associated calibration context, the component has points removed, but its context is discarded.

If the resulting operation yields an empty container with 0 valid points, the components with the same dimensions are not created and will not exist in the destination container. |
