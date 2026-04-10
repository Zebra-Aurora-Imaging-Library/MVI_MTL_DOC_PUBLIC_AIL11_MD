---
doctype: Reference
module: 3dim
function: M3dimMatrixTransform
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimMatrixTransform"
---

# M3dimMatrixTransform

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

> Apply the specified transformation matrix to the 3D points of a specified point cloud or to a 3D geometry.

## Syntax

```c
void M3dimMatrixTransform(
    AIL_ID    SrcContainerBufOrGeometry3dgeoId,  //in
    AIL_ID    DstContainerBufOrGeometry3dgeoId,  //out
    AIL_ID    Matrix3dgeoId,                     //in
    AIL_INT64 ControlFlag                        //in
)
```

## Description

This function applies the specified transformation matrix to the 3D points of a specified point cloud or to a 3D geometry.

> **Note:** Note that, when transforming a point cloud, this function affects the coordinates of the points and not their location in the container.

If an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source container, the component is recalculated then added to the destination container. All other source components are copied to the destination container. Any previously existing components in the destination container are removed.

## Parameters

### `SrcContainerBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies the source point cloud container or 3D geometry object.

*For specifying the source point cloud container or 3D geometry object*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `Source 3D geometry object identifier` | Specifies the identifier of the source 3D geometry object.

The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined.

> **Note:** Note that sphere, box, and cylinder 3D geometry objects do not accept non-uniform scaling. |
| `Source point cloud container identifier` | Specifies the identifier of the source point cloud container.

The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). |

### `DstContainerBufOrGeometry3dgeoId` *(out, AIL_ID)*

Specifies the destination container or 3D geometry object.

*For specifying the destination container or 3D geometry object*

| Value | Description |
| --- | --- |
| `Destination 3D geometry object identifier` | Specifies the identifier of the destination 3D geometry object.

The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |
| `Destination container identifier` | Specifies the identifier of the destination container.

The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container. |

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). The transformation matrix must be any valid affine transformation matrix, where the last row is (0,0,0,1). That is, if you call [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_AFFINE`](../../Reference/3dgeo/M3dgeoInquire.md), the function returns [`M_TRUE`](../../Reference/3dgeo/M3dgeoInquire.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
