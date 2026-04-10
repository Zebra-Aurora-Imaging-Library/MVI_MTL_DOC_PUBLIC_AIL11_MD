---
doctype: Reference
module: 3dim
function: M3dimRotate
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimRotate"
---

# M3dimRotate

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

> Rotate a specified point cloud or 3D geometry.

## Syntax

```c
void M3dimRotate(
    AIL_ID     SrcContainerBufOrGeometry3dgeoId,  //in
    AIL_ID     DstContainerBufOrGeometry3dgeoId,  //out
    AIL_INT64  RotationType,                      //in
    AIL_DOUBLE Param1,                            //in
    AIL_DOUBLE Param2,                            //in
    AIL_DOUBLE Param3,                            //in
    AIL_DOUBLE Param4,                            //in
    AIL_DOUBLE CenterX,                           //in
    AIL_DOUBLE CenterY,                           //in
    AIL_DOUBLE CenterZ,                           //in
    AIL_INT64  ControlFlag                        //in
)
```

## Description

This function applies the specified rotation to the 3D points in a point cloud container or to the geometry in a 3D geometry object.

If the source is a point cloud container, [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md) applies the specified rotation to the coordinates in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component. If an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source container, it is recalculated and added to the destination container, along with the modified [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component. The source container's [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) component is copied to the destination container. If [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) and [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) components exist in the source, they are also copied to the destination. Any previously existing [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md), [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md), or [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) components are removed from the destination container.

Note that all angles should be specified in degrees. However, unlike most other Aurora Imaging Library functions, angles are interpreted using the right-hand grip rule around the axis of rotation; if you wrap your right hand around the axis of rotation, pointing your thumb in the positive direction of the axis, your fingers wrap in the direction of rotation. For example, a positive rotation around the Z-axis corresponds to a rotation that turns the positive X-axis toward the positive Y-axis.

All coordinates are expressed in world units in the working coordinate system.

> **Note:** Note that, when rotating a point cloud, this function affects the coordinates of the points and not their storage location in the container.

## Parameters

### `SrcContainerBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies the source point cloud container or 3D geometry object.

*For specifying the source container or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `Source 3D geometry object identifier` | Specifies the identifier of the source 3D geometry object.

The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |
| `Source container identifier` | Specifies the identifier of the source point cloud container.

The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). |

### `DstContainerBufOrGeometry3dgeoId` *(out, AIL_ID)*

Specifies the destination container or 3D geometry object.

*For specifying the destination container or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `Destination 3D geometry object identifier` | Specifies the identifier of the destination 3D geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |
| `Destination container identifier` | Specifies the identifier of the destination container, previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). The destination container must not be a child container. |

### `RotationType` *(in, AIL_INT64)*

Specifies the type of rotation to apply to the point cloud or 3D geometry.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the rotation. Its definition is dependent on the type of rotation selected. Set this parameter to `M_DEFAULT` if not used.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the rotation. Its definition is dependent on the type of rotation selected. Set this parameter to `M_DEFAULT` if not used.

### `Param3` *(in, AIL_DOUBLE)*

Specifies an attribute of the rotation. Its definition is dependent on the type of rotation selected. Set this parameter to `M_DEFAULT` if not used.

### `Param4` *(in, AIL_DOUBLE)*

Specifies an attribute of the rotation. Its definition is dependent on the type of rotation selected. Set this parameter to `M_DEFAULT` if not used.

### `CenterX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the point around which the rotation is performed.

*For specifying the X-coordinate value*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GEOMETRY_CENTER` | Specifies the 3D geometry's center point. This setting is only applicable if the geometry is finite (for example, a box, sphere, or finite cylinder).

> **Note:** Note that, if [`M_GEOMETRY_CENTER`](../../Reference/3dim/M3dimRotate.md) is specified, you must set [`CenterY`](../../Reference/3dim/M3dimRotate.md) and [`CenterZ`](../../Reference/3dim/M3dimRotate.md) to [`M_DEFAULT`](../../Reference/3dim/M3dimRotate.md). |
| `Value` *(default)* | Specifies the X-coordinate value. |

### `CenterY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the point around which the rotation is performed. If the [`CenterX`](../../Reference/3dim/M3dimRotate.md) parameter is set to [`M_GEOMETRY_CENTER`](../../Reference/3dim/M3dimRotate.md), set this parameter to [`M_DEFAULT`](../../Reference/3dim/M3dimRotate.md).

*For specifying the Y-coordinate value*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate value. |

### `CenterZ` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the point around which the rotation is performed. If the [`CenterX`](../../Reference/3dim/M3dimRotate.md) parameter is set to [`M_GEOMETRY_CENTER`](../../Reference/3dim/M3dimRotate.md), set this parameter to [`M_DEFAULT`](../../Reference/3dim/M3dimRotate.md).

*For specifying the Z-coordinate value*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Z-coordinate value. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the rotation type
