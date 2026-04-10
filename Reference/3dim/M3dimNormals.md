---
doctype: Reference
module: 3dim
function: M3dimNormals
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimNormals"
---

# M3dimNormals

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

> Compute a point cloud's unit normal vectors.

## Syntax

```c
void M3dimNormals(
    AIL_ID    NormalsContext3dimId,  //in
    AIL_ID    SrcContainerBufId,     //in
    AIL_ID    DstContainerBufId,     //out
    AIL_INT64 ControlFlag            //in
)
```

## Description

This function computes the unit normal vector for each point in a point cloud, and stores the values in an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component, which is added to the destination container. Any previously existing [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component in the destination container is overwritten.

All other source components are copied to the destination container, and replace any previously existing components.

For some functions that use normal values, how those values were calculated can affect results. For example, when using [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md) with a bilateral smoothing filter ([`M_SMOOTH_BILATERAL`](../../Reference/3dim/M3dimControl.md)), the filter operation works best with normals that were calculated using the [`M_MESH`](../../Reference/3dim/M3dimControl.md) or [`M_TREE`](../../Reference/3dim/M3dimControl.md) neighbor search mode ([`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md)). However, normals created using the [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md) mode can give faster filtering results.

## Parameters

### `NormalsContext3dimId` *(in, AIL_ID)*

Specifies a normals 3D image processing context.

*For specifying the normals 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `M_NORMALS_CONTEXT_MESH` | Specifies a predefined normals 3D image processing context with all normals context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_MESH`](../../Reference/3dim/M3dimControl.md). Use this predefined context to determine the neighbors of a point using the point cloud's [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component. |
| `M_NORMALS_CONTEXT_ORGANIZED` | Specifies a predefined normals 3D image processing context with all normals context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md). Use this predefined context to determine the neighbors of a point using the point cloud's organizational structure. |
| `M_NORMALS_CONTEXT_TREE` | Specifies a predefined normals 3D image processing context with all normals context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, including [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dim/M3dimControl.md) which is set to [`M_TREE`](../../Reference/3dim/M3dimControl.md). Use this predefined context to determine the neighbors of a point using a KD tree search mode. |
| `Normals 3D image processing context identifier` | Specifies the identifier of a normals 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_NORMALS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** If a previously allocated context is specified, the function applies the normals control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |

### `SrcContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the source container containing a 3D-processable point cloud. The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)).

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container, previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). The destination container must not be a child container.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
