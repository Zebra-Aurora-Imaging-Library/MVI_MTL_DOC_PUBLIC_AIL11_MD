---
doctype: Reference
module: 3dgeo
function: M3dgeoConstruct
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoConstruct"
---

# M3dgeoConstruct

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

> Construct a 3D geometry from other geometries.

## Syntax

```c
void M3dgeoConstruct(
    AIL_ID     Src1Geometry3dgeoId,  //in
    AIL_ID     Src2Geometry3dgeoId,  //in
    AIL_ID     DstGeometry3dgeoId,   //out
    AIL_INT    GeometryType,         //in
    AIL_INT64  Operation,            //in
    AIL_DOUBLE Param,                //in
    AIL_INT64  ControlFlag           //in
)
```

## Description

This function constructs a new 3D geometry using the geometry from one or two source 3D geometries. You can construct a box, a cylinder, a line, a plane, a point, or a sphere.

## Parameters

### `Src1Geometry3dgeoId` *(in, AIL_ID)*

Specifies the first source 3D geometry object.

*For specifying the first source 3D geometry*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |

### `Src2Geometry3dgeoId` *(in, AIL_ID)*

Specifies the second source 3D geometry object. If a second source is not required, set this parameter to`M_NULL`.

*For specifying the second source 3D geometry*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |

### `DstGeometry3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the destination 3D geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `GeometryType` *(in, AIL_INT)*

Specifies the type of 3D geometry to construct.

### `Operation` *(in, AIL_INT64)*

Specifies how to construct the 3D geometry.

### `Param` *(in, AIL_DOUBLE)*

Specifies a scalar value for certain operations, where required. Set this parameter to `M_DEFAULT` if not used.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the type of geometry to construct

Set unused parameters to [`M_DEFAULT`](../../Reference/3dgeo/M3dgeoConstruct.md).

---

### `M_BOX`

Specifies to construct a box.

| Value | Description |
| --- | --- |
| `M_BOTH_CORNERS` | Constructs the box using two opposite corners. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) and [`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as points. |

---

### `M_CYLINDER`

Specifies to construct a cylinder.

| Value | Description |
| --- | --- |
| `M_AXIS` | Constructs a cylinder using a central axis and a radius. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) determines the central axis and must be defined as a line. |
| `M_FLIP` | Constructs a cylinder by flipping the specified cylinder in on itself by swapping its start and end points. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a cylinder. |
| `M_TWO_POINTS` | Constructs a cylinder using two points and a radius. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) and [`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as points. |
| `M_DEFAULT` | Specifies that this parameter is not used. |
| `Value >= 0.0` | Sets the radius of the cylinder. |

---

### `M_LINE`

Specifies to construct a line.

| Value | Description |
| --- | --- |
| `M_AXIS` | Constructs a line on the specified cylinder's central axis; the line will have the same length as the cylinder. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a cylinder. |
| `M_EDGE` | Constructs a line on the specified edge of the source box geometry. The line will have the same length as the edge. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a box. |
| `M_FLIP` | Constructs a line by flipping the specified line in on itself by swapping its start and end points. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a line. |
| `M_NORMAL` | Constructs a unit line parallel to the plane's normal. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a plane, and [`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a point or [`M_NULL`](../../Reference/3dgeo/M3dgeoConstruct.md).  If [`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) is [`M_NULL`](../../Reference/3dgeo/M3dgeoConstruct.md), the line passes through the closest point to the origin; otherwise, it passes through[`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md). |
| `M_TWO_POINTS` | Constructs a finite line from two points. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) and [`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as points. |
| `M_DEFAULT` | Specifies that this parameter is not used. |
| `0 <= Value <= 11` | Sets the edge along which to construct the line.  The edges are assigned the following indices upon initially defining the box, based on the edge's position with respect to the axes of the working coordinate system. The index of an edge does not change based on the orientation of the box. *[Image: Edge_Construction.png]* |

---

### `M_PLANE`

Specifies to construct a plane.

| Value | Description |
| --- | --- |
| `M_FACE` | Constructs a plane passing through the face of a box or cylinder. The plane's normal always points outside the geometry. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a box or cylinder. Use [`Param`](../../Reference/3dgeo/M3dgeoConstruct.md) to specify the box's face or cylinder's base. For infinite cylinders, set [`Param`](../../Reference/3dgeo/M3dgeoConstruct.md) to 0, which constructs the plane at the cylinder's start point, perpendicular to its central axis. |
| `M_FLIP` | Constructs a plane by flipping the specified plane in on itself by inverting its normal vector. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a plane. |
| `M_LINE_AND_POINT` | Constructs a plane passing through the specified line and point. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a line, and [`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a point.  You can use [`M_LINE_AND_POINT`](../../Reference/3dgeo/M3dgeoConstruct.md) to construct a plane from 3 points if you first construct a line using two of the points ([`M_TWO_POINTS`](../../Reference/3dgeo/M3dgeoConstruct.md)). |
| `M_NORMAL` | Constructs a plane with a normal vector that coincides with the specified line. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a line, and [`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a point or [`M_NULL`](../../Reference/3dgeo/M3dgeoConstruct.md).  The plane passes through the specified point ([`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md)); otherwise, the plane passes through the line's start point. |
| `M_DEFAULT` | Specifies that this parameter is not used. |
| `0` | Specifies that the plane passes through the start point of the infinite cylinder. |
| `0 <= Value <= 1` | Specifies either the first (0) or the second (1) circular base of the cylinder. |
| `0 <= Value <= 5` | Specifies the face of the box.  The faces are assigned the following indices upon initially defining the box, based on the face's position with respect to the axes of the working coordinate system. The index of a face does not change based on the orientation of the box. *[Image: box_faces.png]*  To inquire the box's minimum and maximum coordinates, use [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_UNROTATED_...`](../../Reference/3dgeo/M3dgeoInquire.md). |

---

### `M_POINT`

Specifies to construct a point.

| Value | Description |
| --- | --- |
| `M_CENTER` | Constructs a point at the center of the specified 3D geometry. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a box, finite cylinder, finite line, point, or sphere. |
| `M_CLOSEST_TO_ORIGIN` | Constructs a point on the plane such that the point is closest to the origin. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a plane. |
| `M_CORNER` | Constructs a point at the specified corner of the box. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a box. |
| `M_END_POINT` | Constructs a point at the end point of a finite line or a finite cylinder. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a finite line or finite cylinder. |
| `M_START_POINT` | Constructs a point at the start point of a line or cylinder. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a line or cylinder. |
| `M_DEFAULT` | Specifies that this parameter is not used. |
| `0 <= Value <= 7` | Specifies the box corner.*[Image: 3dgeo_box.png]* |

---

### `M_SPHERE`

Specifies to construct a sphere.

| Value | Description |
| --- | --- |
| `M_CENTER_AND_RADIUS` | Constructs a sphere using a center point and a radius. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as a point. |
| `M_DIAMETER` | Constructs a sphere using two antipodal points. [`Src1Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) and [`Src2Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoConstruct.md) must be defined as points. |
| `M_DEFAULT` | Specifies that this parameter is not used. |
| `Value >= 0.0` | Sets the radius of the sphere. |
