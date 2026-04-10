---
doctype: Reference
module: 3dim
function: M3dimMesh
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimMesh"
---

# M3dimMesh

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

> Create a 3D mesh structure for a point cloud or depth map.

## Syntax

```c
void M3dimMesh(
    AIL_ID    MeshContext3dimId,  //in
    AIL_ID    SrcContainerBufId,  //in
    AIL_ID    DstContainerBufId,  //out
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function generates a triangular mesh structure for the source container's point cloud or depth map, using the specified surface reconstruction mode. The mesh data is stored in an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component, which is added to the destination container.

You can choose the surface reconstruction mode with which to create the mesh, either organized, local plane, or smoothed. Use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) to set the mode, or use this function to specify a predefined context that uses default settings to create the mesh.

The organized surface reconstruction mode ([`M_MESH_ORGANIZED`](../../Reference/3dim/M3dimControl.md)) requires an organized point cloud or depth map and connects neighboring points to create the mesh. With this mode, invalid data points in the point cloud or depth map result in a mesh with holes. No change is made to the source container's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components; they are simply copied over to the destination container. If the components [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md), [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md), and [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) exist in the source container, these are also copied to the destination container.

The local plane reconstruction mode ([`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md)) also connects neighboring points to create the mesh; however, it uses local planes computed from each point's neighborhood to establish which neighbors to connect. For each point, neighborhood points are projected to the local plane and the most suitable points are chosen to form a mesh triangle with the point in question. Note that the triangle's vertices are points from the original point cloud, which can be organized or unorganized. With this mode, the mesh can have holes. Components are handled in the same way as for the organized surface reconstruction mode. Note that normal vectors at each 3D point are required ([`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component). Note also that this mode is only available for point clouds.

The smoothed surface reconstruction mode ([`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimControl.md)) also requires a point cloud and normal vectors at each 3D point ([`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component), and generates a mesh by solving a Poisson equation. This approach generates a smooth surface even when the source point cloud is noisy; missing data is approximated when possible, resulting in significantly fewer holes. Before solving the Poisson equation, an octree is built, which partitions the 3D space for the mesh calculation. The octree's depth determines the mesh's resolution. Note that a higher octree depth gives a higher resolution, but a slower calculation time. Smoothed surface reconstruction generates a new point cloud; new [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) components are added to the destination container, along with the mesh component.

To remove points from the point cloud whose confidence is equal to zero, as well as points that are not part of the mesh, use [`M3dimRemovePoints`](../../Reference/3dim/M3dimRemovePoints.md) with [`M_NOT_MESH_POINTS`](../../Reference/3dim/M3dimRemovePoints.md).

> **Note:** Note that, when using the smoothed surface reconstruction mode, no [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component is added to the destination container.

To determine the number of triangular faces in the mesh, inquire the [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md).

Inquiring the [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md) value of the [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component returns 3 for an unorganized destination point cloud (3 vertices for each triangular face), and 6 for an organized destination point cloud or depth map (3 vertices with corresponding coordinates in both X and Y).

## Parameters

### `MeshContext3dimId` *(in, AIL_ID)*

Specifies a mesh 3D image processing context.

*For specifying the mesh 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `M_MESH_CONTEXT_LOCAL_PLANE` | Specifies a predefined mesh 3D image processing context with all mesh context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except for [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_MESH_LOCAL_PLANE`](../../Reference/3dim/M3dimControl.md). Use this predefined context to create the mesh from local planes that are fit to neighboring points.

With the local plane surface reconstruction mode, local planes help determine which points in the source point cloud to connect. With this mode, the mesh can have holes.

> **Note:** This value is not available for depth map containers. |
| `M_MESH_CONTEXT_ORGANIZED` | Specifies a predefined mesh 3D image processing context with all mesh context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except for [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_MESH_ORGANIZED`](../../Reference/3dim/M3dimControl.md). Use this predefined context to create the mesh using the organized surface reconstruction mode.

With surface reconstruction from an organized point cloud or depth map, neighboring points are connected. Any invalid points will result in a mesh with holes. |
| `M_MESH_CONTEXT_SMOOTHED` | Specifies a predefined mesh 3D image processing context with all mesh context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except for [`M_MESH_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_MESH_SMOOTHED`](../../Reference/3dim/M3dimControl.md). Use this predefined context to generate a mesh with a smoothed surface.

With the smoothed surface reconstruction mode, the mesh structure is extrapolated from existing points and smoothing is applied to reduce the noise in the point cloud; missing data is approximated when possible, resulting in significantly fewer holes. This mode generates a new point cloud with a smooth surface.

> **Note:** This value is not available for depth map containers. |
| `Mesh 3D image processing context identifier` | Specifies the identifier of a mesh 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_MESH_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** If a previously allocated context is specified, the function applies the mesh control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |

### `SrcContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the 3D-processable point cloud or depth map container to use as the source.

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container, previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). The destination container must not be a child container.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
