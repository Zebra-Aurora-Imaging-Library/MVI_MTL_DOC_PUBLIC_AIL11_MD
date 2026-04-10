---
doctype: Reference
module: 3dgeo
function: M3dgeoSphere
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoSphere"
---

# M3dgeoSphere

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

> Define a 3D sphere geometry object.

## Syntax

```c
void M3dgeoSphere(
    AIL_ID     Geometry3dgeoId,  //out
    AIL_DOUBLE CenterX,          //in
    AIL_DOUBLE CenterY,          //in
    AIL_DOUBLE CenterZ,          //in
    AIL_DOUBLE Radius,           //in
    AIL_INT64  ControlFlag       //in
)
```

## Description

This function defines a 3D sphere geometry object, using its center coordinates and radius. You can translate, scale, or transform the resulting sphere, using the 3D image processing module.

You can use the resulting sphere to, for example, crop a point cloud or perform an arithmetic operation on a depth map using the 3D image processing module, or calculate its distance from each point in a point cloud using the 3D metrology module.

If you want to define a sphere geometry object from results obtained in a different module, you can use the copy function of that module.

All coordinates are expressed in world units in the working coordinate system.

> **Note:** Note that if [`Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoSphere.md) specifies a previously-defined sphere, you can leave some of its attributes unchanged, even if that attribute was modified using the 3D image processing module. To do so, set the corresponding parameter to [`M_UNCHANGED`](../../Reference/3dgeo/M3dgeoSphere.md).

## Parameters

### `Geometry3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the 3D geometry object, previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `CenterX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the sphere's center.

*For specifying the X-coordinate of the sphere's center*

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the X-coordinate of the sphere's center. |

### `CenterY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the sphere's center.

*For specifying the Y-coordinate of the sphere's center*

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the Y-coordinate of the sphere's center. |

### `CenterZ` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the sphere's center.

*For specifying the Z-coordinate of the sphere's center*

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the Z-coordinate of the sphere's center. |

### `Radius` *(in, AIL_DOUBLE)*

Specifies the sphere's radius. This parameter cannot be set to a negative value.

*For specifying the sphere's radius*

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value >= 0.0` | Specifies the sphere's radius. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
