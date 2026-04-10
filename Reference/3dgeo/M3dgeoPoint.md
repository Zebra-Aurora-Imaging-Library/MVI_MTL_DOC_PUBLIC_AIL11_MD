---
doctype: Reference
module: 3dgeo
function: M3dgeoPoint
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoPoint"
---

# M3dgeoPoint

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

> Define an arbitrary 3D point geometry object.

## Syntax

```c
void M3dgeoPoint(
    AIL_ID     Geometry3dgeoId,  //out
    AIL_DOUBLE PositionX,        //in
    AIL_DOUBLE PositionY,        //in
    AIL_DOUBLE PositionZ,        //in
    AIL_INT64  ControlFlag       //in
)
```

## Description

This function defines an arbitrary 3D point geometry object.

All coordinates are expressed in world units in the working coordinate system.

## Parameters

### `Geometry3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the 3D geometry object, previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `PositionX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the point.

*For specifying the X-coordinate of the point*

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the X-coordinate of the point. |

### `PositionY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the point.

*For specifying the Y-coordinate of the point*

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the Y-coordinate of the point. |

### `PositionZ` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the point.

*For specifying the Z-coordinate of the point*

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the Z-coordinate of the point. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
