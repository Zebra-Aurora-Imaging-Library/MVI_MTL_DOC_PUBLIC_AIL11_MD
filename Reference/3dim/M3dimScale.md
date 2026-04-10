---
doctype: Reference
module: 3dim
function: M3dimScale
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimScale"
---

# M3dimScale

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

> Scale a specified point cloud or 3D geometry.

## Syntax

```c
void M3dimScale(
    AIL_ID     SrcContainerBufOrGeometry3dgeoId,  //in
    AIL_ID     DstContainerBufOrGeometry3dgeoId,  //out
    AIL_DOUBLE ScaleX,                            //in
    AIL_DOUBLE ScaleY,                            //in
    AIL_DOUBLE ScaleZ,                            //in
    AIL_DOUBLE CenterX,                           //in
    AIL_DOUBLE CenterY,                           //in
    AIL_DOUBLE CenterZ,                           //in
    AIL_INT64  ControlFlag                        //in
)
```

## Description

This function applies the specified scale factors to the points in the source point cloud container or to the geometry in the source 3D geometry object. The scaling is applied to each point's distance from the origin or specified center point, along X, Y, or Z. Note that you can specify negative scale factors.

If the source is a point cloud container, [`M3dimScale`](../../Reference/3dim/M3dimScale.md) applies the specified scale factors to the coordinates in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component. If an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source container, it is recalculated and added to the destination container, along with the modified [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component. The source container's [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) component is copied to the destination container. If [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) components exist in the source container, they are also copied to the destination container. Any previously existing normals, reflectance, or mesh components are removed from the destination container.

> **Note:** Note that sphere, box, and cylinder 3D geometries do not accept non-uniform scaling. That is, you must apply the same scale factor in X, Y, and Z for a source sphere, box, or cylinder.

> **Note:** Note that, when scaling a point cloud, this function affects the coordinates of the points and not their storage location in the container.

## Parameters

### `SrcContainerBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies a source point cloud container or 3D geometry object.

*For specifying the source container or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `Source 3D geometry object identifier` | Specifies the identifier of the source 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |
| `Source container identifier` | Specifies the identifier of the source point cloud container.

The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). |

### `DstContainerBufOrGeometry3dgeoId` *(out, AIL_ID)*

Specifies the destination container or 3D geometry object.

*For specifying the destination container or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `Destination 3D geometry object identifier` | Specifies the identifier of the destination 3D geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |
| `Destination container identifier` | Specifies the identifier of the destination container, previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). The destination container must not be a child container. |

### `ScaleX` *(in, AIL_DOUBLE)*

Specifies the factor by which to scale in X.

### `ScaleY` *(in, AIL_DOUBLE)*

Specifies the factor by which to scale in Y.

### `ScaleZ` *(in, AIL_DOUBLE)*

Specifies the factor by which to scale in Z.

### `CenterX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the point that defines the center of the scaling operation.

*For specifying the X-coordinate value*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GEOMETRY_CENTER` | Specifies the 3D geometry object's center point for a finite 3D geometry (for example, a box, sphere, or finite cylinder).

> **Note:** Note that, if you specify [`M_GEOMETRY_CENTER`](../../Reference/3dim/M3dimScale.md), you must set [`CenterY`](../../Reference/3dim/M3dimScale.md) and [`CenterZ`](../../Reference/3dim/M3dimScale.md) to [`M_DEFAULT`](../../Reference/3dim/M3dimScale.md). |
| `Value` *(default)* | Specifies the X-coordinate value. |

### `CenterY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the point that defines the center of the scaling operation. If the [`CenterX`](../../Reference/3dim/M3dimScale.md) parameter is set to [`M_GEOMETRY_CENTER`](../../Reference/3dim/M3dimScale.md), set this parameter to [`M_DEFAULT`](../../Reference/3dim/M3dimScale.md).

*For specifying the Y-coordinate value*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate value. If scaling a sphere, box, or cylinder 3D geometry object, this parameter must be set to the same value as [`CenterX`](../../Reference/3dim/M3dimScale.md) and [`CenterZ`](../../Reference/3dim/M3dimScale.md). |

### `CenterZ` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the point that defines the center of the scaling operation. If the [`CenterX`](../../Reference/3dim/M3dimScale.md) parameter is set to [`M_GEOMETRY_CENTER`](../../Reference/3dim/M3dimScale.md), set this parameter to [`M_DEFAULT`](../../Reference/3dim/M3dimScale.md).

*For specifying the Z-coordinate value*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Z-coordinate value. If scaling a sphere, box, or cylinder 3D geometry object, this parameter must be set to the same value as [`CenterX`](../../Reference/3dim/M3dimScale.md) and [`CenterY`](../../Reference/3dim/M3dimScale.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
