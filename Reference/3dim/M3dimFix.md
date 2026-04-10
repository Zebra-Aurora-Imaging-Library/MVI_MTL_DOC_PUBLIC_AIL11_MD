---
doctype: Reference
module: 3dim
function: M3dimFix
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimFix"
---

# M3dimFix

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

> Determine if a container satisfies certain conditions and optionally fixes them if they are not met.

## Syntax

```c
void M3dimFix(
    AIL_ID      SrcContainerBufId,  //in
    AIL_ID      DstContainerBufId,  //out
    AIL_INT64   Conditions,         //in
    AIL_INT64   ControlFlag,        //in
    AIL_INT64 * ResultPtr           //in
)
```

## Description

This function allows you to validate whether a container meets a certain condition. You can use [`M3dimFix`](../../Reference/3dim/M3dimFix.md) to verify that a container is suitable for use with another 3D function. You can optionally specify a destination container to obtain a modified copy of the source container that fixes the specified conditions.

## Parameters

### `SrcContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the source point cloud container, previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). The container must be 3D processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)).

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container in which to place the modified point cloud. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container.

### `Conditions` *(in, AIL_INT64)*

Specifies which conditions you would like to be validated and optionally fixed.

*For specifying the conditions to validate*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to verify that all the conditions listed in this table are met. Conditions are only validated if the specified source point cloud container has the component required to validate the condition.

If you specify a [`DstContainerBufId`](../../Reference/3dim/M3dimFix.md), you will retrieve a modified version of the source point cloud container that satisfies all the valid conditions for it. |
| `M_MESH_VALID_NEIGHBORS` | Specifies to check each edge in the mesh to verify that the edge appears in at most two triangular faces. Triangular faces are also verified to ensure that they are constructed from three different vertices to ensure that there are no degenerate triangular faces in the mesh.

This option is available when the source container has an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component. [`M_MESH_VALID_NEIGHBORS`](../../Reference/3dim/M3dimFix.md) must be satisfied for some functions which use a mesh component, such as [`M3dmetVolume`](../../Reference/3dmet/M3dmetVolume.md).

If you specify a destination container and more than two triangular faces are found sharing a common edge, the first two faces are kept, while the subsequent triangles are removed. |
| `M_MESH_VALID_POINTS` | Specifies to check that each point in the mesh is valid; a valid point has a non-zero confidence score.

This option is available when the source container has an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component. [`M_MESH_VALID_POINTS`](../../Reference/3dim/M3dimFix.md) must be satisfied for all functions that use a mesh component.

If you specify a destination container, any invalid point in the mesh that corresponds to the vertex of a triangular face results in that face's removal. |
| `M_NORMALS_FINITE` | Specifies to check that each point in the range component has a finite normal vector.

This option is available when the source container has an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component. [`M_NORMALS_FINITE`](../../Reference/3dim/M3dimFix.md) must be satisfied for all functions that use a normals component.

If you specify a [`DstContainerBufId`](../../Reference/3dim/M3dimFix.md) and points with infinite normal vectors are found, the point's confidence is set to 0 and the normals length is set to the range's invalid value. You can set this value using [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufControl.md) with [`MbufControl`](../../Reference/buf/MbufControl.md). |
| `M_NORMALS_NORMALIZED` | Specifies to check that each point with a non-zero confidence has a normal with a norm of exactly one or zero.

This option is available when the source container has an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component. [`M_NORMALS_NORMALIZED`](../../Reference/3dim/M3dimFix.md) must be satisfied for all functions that use a normals component.

If you specify a [`DstContainerBufId`](../../Reference/3dim/M3dimFix.md) and normals are found without a norm of one or zero, they will be normalized to have a norm of one. |
| `M_RANGE_FINITE` | Specifies to check that each point in the range component is finite.

[`M_RANGE_FINITE`](../../Reference/3dim/M3dimFix.md) is required for all 3D functions.

If you specify a [`DstContainerBufId`](../../Reference/3dim/M3dimFix.md) and invalid points are found, the point's confidence is set to 0 and the point is set to the range's invalid value. You can set this value using [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufControl.md) with [`MbufControl`](../../Reference/buf/MbufControl.md). |
| `M_UNREPLICATED_POINTS` | Specifies to check that each point in the range component with a non-zero confidence is unreplicated.

If you specify a [`DstContainerBufId`](../../Reference/3dim/M3dimFix.md) and replicated points are found, the point's confidence is set to 0. Note that when two points are replicates, either of the points could be removed. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ResultPtr` *(in, *AIL_INT64)*

Specifies the address of the variable in which to write the conditions that were detected. Alternatively, if you do not want to return the conditions, you can set this parameter to `M_NULL`.
