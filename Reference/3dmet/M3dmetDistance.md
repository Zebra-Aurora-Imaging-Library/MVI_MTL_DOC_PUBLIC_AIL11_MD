---
doctype: Reference
module: 3dmet
function: M3dmetDistance
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetDistance"
---

# M3dmetDistance

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

> Calculates the distance between a point cloud or depth map, and a point cloud, depth map, or 3D geometry object.

## Syntax

```c
void M3dmetDistance(
    AIL_ID     SrcContainerOrImageBufId,  //in
    AIL_ID     RefAilObjectId,            //in
    AIL_ID     DstImageBufId,             //out
    AIL_INT64  DistanceType,              //in
    AIL_DOUBLE Param,                     //in
    AIL_INT64  ControlFlag                //in
)
```

## Description

This function calculates various distance measurements between a point cloud or depth map, and a point cloud, depth map, or 3D geometry.

This function is similar to [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md), but does not require allocating any 3D metrology contexts or result buffers, and does not calculate any statistical metrics based on the distance measurements.

## Parameters

### `SrcContainerOrImageBufId` *(in, AIL_ID)*

Specifies the source point cloud container, depth map container, or depth map image buffer.

*For specifying the point cloud or depth map*

| Value | Description |
| --- | --- |
| `Depth map container identifier` | Specifies the identifier of a depth map container.

The container must store data in a 3D-processable depth map format. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable depth map.

The container must not have a region component. |
| `Depth map image buffer identifier` | Specifies the identifier of a depth map image buffer.

The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer. It must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

The image buffer must not have a region of interest (ROI) associated with it. |
| `Point cloud container identifier` | Specifies the identifier of a point cloud container.

The container must be 3D-processable. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable point cloud. |

### `RefAilObjectId` *(in, AIL_ID)*

Specifies the reference object with respect to which distances will be measured.

*For specifying the reference Aurora Imaging Library object identifier*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane.

> **Note:** This is equivalent to passing a 3D plane geometry object, and can therefore be used with the same [`DistanceType`](../../Reference/3dmet/M3dmetDistance.md)parameter settings. |
| `3D geometry object identifier` | Specifies the identifier of a reference 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. Supported 3D geometries include box, cylinder, line, plane, point, and sphere. |
| `Depth map container identifier` | Specifies the identifier of a depth map container.

The container must store data in a 3D-processable depth map format. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable depth map.

The container must not have a region component. |
| `Depth map image buffer identifier` | Specifies the identifier of a depth map image buffer.

The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer. It must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

The image buffer is allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md), and must not have a region of interest (ROI) associated with it. |
| `Point cloud container identifier` | Specifies the identifier of a point cloud container.

The container must be 3D-processable. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable point cloud. |

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the buffer in which to store the calculated distances.

### `DistanceType` *(in, AIL_INT64)*

Specifies the type of distance to calculate.

### `Param` *(in, AIL_DOUBLE)*

Specifies an optional maximum distance for which to complete calculations when the reference Aurora Imaging Library object is a point cloud container. Specify a value if [`DistanceType`](../../Reference/3dmet/M3dmetDistance.md) is set to [`M_DISTANCE_TO_MESH`](../../Reference/3dmet/M3dmetDistance.md) or [`M_DISTANCE_TO_NEAREST_NEIGHBOR`](../../Reference/3dmet/M3dmetDistance.md); otherwise, set this parameter to [`M_DEFAULT`](../../Reference/3dmet/M3dmetDistance.md). Points determined to be further from the reference than this value are assigned a distance of **AIL_FLOAT_MAX**in the destination, instead of having their exact distances calculated.

*For specifying the maximum distance to calculate*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that there is no maximum distance for which to complete calculations. All distance measurements will be fully calculated. |
| `Value > 0.0` | Specifies an optional maximum distance. All distance measurements greater than this limit are returned as **AIL_FLOAT_MAX**. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the type of distance measurement for the specified reference object

To specify the type of distance measurement, the [`DistanceType`](../../Reference/3dmet/M3dmetDistance.md) parameter can be set to one of the following values, depending on the type of reference object ([`RefAilObjectId`](../../Reference/3dmet/M3dmetDistance.md)) being measured against.

---

### `3D box geometry object identfier`

Specifies a 3D box geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_DISTANCE_TO_SURFACE` | Specifies to calculate the absolute distance to the surface of the box.  The returned value is always positive. |
| `M_MANHATTAN_DISTANCE_TO_SURFACE` | Specifies to calculate the Manhattan distance to the surface of the box.  Distance measurements are positive for points outside the box, and negative for points inside the box. |
| `M_SIGNED_DISTANCE_TO_SURFACE` | Specifies to calculate the signed distance to the surface of the box.  Distance measurements are positive for points outside the box, and negative for points inside the box. |

---

### `3D cylinder geometry object identifier`

Specifies a 3D cylinder geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a cylinder.

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_DISTANCE_TO_SURFACE` | Specifies to calculate the absolute distance to the surface of the cylinder.  The returned value is always positive. |
| `M_DISTANCE_TO_CENTER_AXIS` | Specifies to calculate the absolute distance to the cylinder's central axis.  The axis extends infinitely in both directions, even if the cylinder has a finite size. |
| `M_DISTANCE_TO_CENTER_AXIS_SQUARED` | Specifies to calculate the square of the distance to the cylinder's central axis.  The axis extends infinitely in both directions, even if the cylinder has a finite size. |
| `M_SIGNED_DISTANCE_TO_SURFACE` | Specifies to calculate the signed distance to the surface of the cylinder.  Distance measurements are positive for points outside the cylinder, and negative for points inside the cylinder. |

---

### `3D line geometry object identifier`

Specifies a 3D line geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a line.

| Value | Description |
| --- | --- |
| `M_DISTANCE_TO_LINE` | Specifies to calculate the distance to the line.  The length of the line is respected; if the line is finite, it is not extended infinitely when calculating distances. |
| `M_DISTANCE_TO_LINE_SQUARED` | Specifies to calculate the square of the distance to the line.  The length of the line is respected; if the line is finite, it is not extended infinitely when calculating distances. |

---

### `3D plane geometry object identifier`

Specifies a 3D plane geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane.  When the reference object is a 3D plane geometry object, calculations are quicker if the source object is a depth map rather than a point cloud.

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_DISTANCE_TO_SURFACE` | Specifies to calculate the absolute distance, perpendicular to the plane.  The returned value is always positive. |
| `M_ABSOLUTE_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the absolute distance along the Z-axis to the plane.  The returned value is always positive. |
| `M_SIGNED_DISTANCE_TO_SURFACE` | Specifies to calculate the signed distance, perpendicular to the plane.  Distance measurements are positive for points on the same side as the plane's normal, and negative on the other side. |
| `M_SIGNED_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the signed distance along the Z-axis to the plane.  Distance measurements are positive if the point is above the plane and negative below. |

---

### `3D point geometry object identifier`

Specifies a 3D point geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a point.

| Value | Description |
| --- | --- |
| `M_DISTANCE_TO_POINT` | Specifies to calculate the distance to the point. |
| `M_DISTANCE_TO_POINT_SQUARED` | Specifies to calculate the square of the distance to the point. |

---

### `3D sphere geometry object identifier`

Specifies a 3D sphere geometry object, allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a sphere.

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_DISTANCE_TO_SURFACE` | Specifies to calculate the absolute distance to the surface of the sphere.  The returned value is always positive. |
| `M_DISTANCE_TO_CENTER` | Specifies to calculate the signed distance to the center of the sphere. |
| `M_DISTANCE_TO_CENTER_SQUARED` | Specifies to calculate the square of the distance to the center of the sphere. |
| `M_SIGNED_DISTANCE_TO_SURFACE` | Specifies to calculate the signed distance to the surface of the sphere.  Distance measurements are positive for points outside the sphere, and negative for points inside the sphere. |

---

### `Depth map container identifier`

Specifies a depth map container, allocated with [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md).  The container must not have a region component.  > **Note:** When the source and reference Aurora Imaging Library objects are depth maps, calculations are completed more quickly.

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the absolute distance to the position in the depth map that is directly above or below the source along the Z-axis.  The returned value is always positive.  If a point would be projected outside of the depth map, **AIL_FLOAT_MAX** is returned. |
| `M_SIGNED_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the signed distance using the Z-coordinate of the corresponding pixel of the reference depth map. That is, the distance is calculated along the Z-axis.  Distance measurements are positive for points above the depth map, and negative for points below the depth map.  If a point would be projected outside of the depth map, **AIL_FLOAT_MAX** is returned. |

---

### `Depth map image buffer identifier`

Specifies the identifier of a depth map image buffer.  The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer. It must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).  The image buffer must not have a region of interest (ROI) associated with it.  > **Note:** When the source and reference Aurora Imaging Library objects are depth maps, calculations are completed more quickly. The speed is further increased if both depth maps use the same calibration context.

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the absolute distance to the position in the depth map that is directly above or below the source along the Z-axis.  The returned value is always positive.  If a point would be projected outside of the depth map, **AIL_FLOAT_MAX** is returned. |
| `M_SIGNED_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the signed distance using the Z-coordinate of the corresponding pixel of the reference depth map. That is, the distance is calculated along the Z-axis.  Distance measurements are positive for points above the depth map, and negative for points below the depth map.  If a point would be projected outside of the depth map, **AIL_FLOAT_MAX** is returned. |

---

### `Point cloud container identifier`

Specifies a point cloud container, allocated with [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md).  The function runs much slower when the reference Aurora Imaging Library object is a point cloud container. If speed is an issue, you can specify a maximum for calculating distance measurements using the[`Param`](../../Reference/3dmet/M3dmetDistance.md) parameter. Any distance measurements greater than the maximum value are returned as **AIL_FLOAT_MAX**.  You can also project the reference point cloud into a fully corrected depth map first (using [`M3dimProject`](../../Reference/3dim/M3dimProject.md)), at the cost of precision.

| Value | Description |
| --- | --- |
| `M_DISTANCE_TO_MESH` | Specifies to calculate the shortest distance to the reference object's mesh.  This calculation requires that the reference object have a mesh component. |
| `M_DISTANCE_TO_NEAREST_NEIGHBOR` | Specifies to calculate the distance to the nearest point in the reference point cloud. |
