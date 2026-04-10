---
doctype: Reference
module: 3dmet
function: M3dmetStat
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetStat"
---

# M3dmetStat

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

> Compute a variety of statistics on the distances between a point cloud or depth map, and a point cloud, depth map, or 3D geometry.

## Syntax

```c
void M3dmetStat(
    AIL_ID     StatContext3dmetId,        //in
    AIL_ID     SrcContainerOrImageBufId,  //in
    AIL_ID     RefAilObjectId,            //in
    AIL_ID     StatResult3dmetId,         //out
    AIL_INT64  DistanceType,              //in
    AIL_INT64  Condition,                 //in
    AIL_DOUBLE CondLow,                   //in
    AIL_DOUBLE CondHigh,                  //in
    AIL_INT64  ControlFlag                //in
)
```

## Description

This function calculates a variety of statistics on the distance measurements collected between a source and reference Aurora Imaging Library object.

The specified statistics 3D metrology context establishes which statistics to compute, and the [`DistanceType`](../../Reference/3dmet/M3dmetStat.md) and [`Condition`](../../Reference/3dmet/M3dmetStat.md) parameters establish the type of distance measurements to use in computing the statistics. Note that you can use a predefined context to calculate a single statistic. In this case, the statistic is calculated using its default settings.

You can use this function to, for example, calculate the mean distance between all points in a point cloud and the center of a 3D sphere geometry object.

## Parameters

### `StatContext3dmetId` *(in, AIL_ID)*

Specifies a previously allocated statistics 3D metrology context (used to evaluate multiple statistical calculations), or a predefined statistics 3D metrology context (used to evaluate a single statistical calculation).

*For specifying the statistics 3D metrology context, or specific statistic to calculate*

| Value | Description |
| --- | --- |
| `M_STAT_CONTEXT_MAX` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_MAX`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the maximum distance between the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_MAX_ABS` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_MAX_ABS`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the maximum absolute distance between the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_MEAN` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_MEAN`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the average distance between the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_MEAN_ABS` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_MEAN_ABS`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the average absolute distance between the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_MIN` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_MIN`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the minimum distance between the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_MIN_ABS` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_MIN_ABS`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the minimum absolute distance between the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_NUMBER` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_NUMBER`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the number of points that meet the condition specified using the [`Condition`](../../Reference/3dmet/M3dmetStat.md), [`CondLow`](../../Reference/3dmet/M3dmetStat.md) and [`CondHigh`](../../Reference/3dmet/M3dmetStat.md) parameters. |
| `M_STAT_CONTEXT_RMS` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_RMS`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the root-mean-square (RMS) error of the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_STANDARD_DEVIATION` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_STANDARD_DEVIATION`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the standard deviation of all the distances calculated between the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_SUM` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_SUM`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the sum of all the distances calculated between the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_SUM_ABS` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_SUM_ABS`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the sum of all the absolute distances calculated between the point cloud or depth map, and the specified reference object. |
| `M_STAT_CONTEXT_SUM_OF_SQUARES` | Specifies a predefined statistics 3D metrology context with all the control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) in the context set to their default, except for [`M_STAT_SUM_OF_SQUARES`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ENABLE`](../../Reference/3dmet/M3dmetControl.md).

Use this predefined context to calculate the sum of squared distances between the point cloud or depth map, and the specified reference object. |
| `Statistics 3D metrology context identifier` | Specifies a statistics 3D metrology context identifier, previously allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_STATISTICS_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md).

You can specify which statistics calculations are evaluated using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md). |

### `SrcContainerOrImageBufId` *(in, AIL_ID)*

Specifies the source point cloud container, depth map container, or depth map image buffer.

*For specifying the point cloud or depth map*

| Value | Description |
| --- | --- |
| `Depth map container identifier` | Specifies the identifier of a depth map container. The container must store data in a 3D-processable depth map format. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable depth map.

The container can have a region component; only the values inside the region component will be used in the statistics calculations. |
| `Depth map image buffer identifier` | Specifies the identifier of a depth map image buffer.

The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer. It must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

The image buffer can have region of interest (ROI) in raster format; only the values inside the ROI will be used in the statistics calculations. |
| `Point cloud container identifier` | Specifies the identifier of a point cloud container. The container must be 3D-processable. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable point cloud. |

### `RefAilObjectId` *(in, AIL_ID)*

Specifies the identifier of the reference Aurora Imaging Library object, with respect to which distances will be measured.

*For specifying the reference Aurora Imaging Library object identifier*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane.

> **Note:** This is equivalent to passing a 3D plane geometry object, and can therefore be used with the same [`DistanceType`](../../Reference/3dmet/M3dmetStat.md)parameter settings. |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object to use as the reference Aurora Imaging Library object. The geometry object must have been previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) and must have been successfully defined. Supported 3D geometries include box, cylinder, line, plane, point, and sphere. |
| `Depth map container identifier` | Specifies the identifier of a depth map container.

The container must store data in a 3D-processable depth map format. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable depth map.

The container must not have a region component. |
| `Depth map image buffer identifier` | Specifies the identifier of a depth map image buffer.

The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer. It must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

The image buffer is allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md), and must not have a region of interest (ROI) associated with it. |
| `Point cloud container identifier` | Specifies the identifier of a point cloud container.

The container must be 3D-processable. You can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with[`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable point cloud. |

### `StatResult3dmetId` *(out, AIL_ID)*

Specifies the identifier of the statistics 3D metrology result buffer in which to store the results. The result buffer must have been allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_STATISTICS_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md).

### `DistanceType` *(in, AIL_INT64)*

Specifies the type of distance to calculate.

### `Condition` *(in, AIL_INT64)*

Specifies the condition for which distance measurements will be used in statistics calculations.

*For specifying the condition*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to use all distance measurements. |
| `M_GREATER` | Specifies to use all distance measurements greater than [`CondLow`](../../Reference/3dmet/M3dmetStat.md). |
| `M_GREATER_OR_EQUAL` | Specifies to use all distance measurements greater than or equal to [`CondLow`](../../Reference/3dmet/M3dmetStat.md). |
| `M_IN_RANGE` | Specifies to use all distance measurements between [`CondLow`](../../Reference/3dmet/M3dmetStat.md) and [`CondHigh`](../../Reference/3dmet/M3dmetStat.md) (inclusive). |
| `M_LESS` | Specifies to use all distance measurements less than [`CondLow`](../../Reference/3dmet/M3dmetStat.md). |
| `M_LESS_OR_EQUAL` | Specifies to use all distance measurements less than or equal to [`CondLow`](../../Reference/3dmet/M3dmetStat.md). |

### `CondLow` *(in, AIL_DOUBLE)*

Specifies the lower limit of the selected condition.

*For the lower limit of the selected condition*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that this parameter is not applicable. |
| `Value` | Specifies the lower limit of the condition. |

### `CondHigh` *(in, AIL_DOUBLE)*

Specifies the upper limit of the selected condition.

*For the upper limit of the selected condition*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that this parameter is not applicable. |
| `Value` | Specifies the upper limit of the condition. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the type of distance calculation for the specified reference object

To specify the type of distance measurement, the [`DistanceType`](../../Reference/3dmet/M3dmetStat.md) parameter can be set to one of the following values, depending on the type of reference object ([`RefAilObjectId`](../../Reference/3dmet/M3dmetStat.md)) being measured against.

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
| `M_DISTANCE_TO_CENTER_AXIS` | Specifies to calculate the distance to the cylinder's central axis.  The axis extends infinitely in both directions, even if the cylinder has a finite size. |
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
| `M_ABSOLUTE_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the absolute distance to the position in the depth map that is directly above or below the source along the Z-axis.  The returned value is always positive.  If a point would be projected outside of the depth map, the point is not used in statistics calculations. |
| `M_SIGNED_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the signed distance using the Z-coordinate of the corresponding pixel of the reference depth map. That is, the distance is calculated along the Z-axis.  Distance measurements are positive for points above the depth map, and negative for points below the depth map.  If a point would be projected outside of the depth map, the point is not used in statistics calculations. |

---

### `Depth map image buffer identifier`

Specifies the identifier of a depth map image buffer.  The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer. It must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).  The image buffer must not have a region of interest (ROI) associated with it.  > **Note:** When the source and reference Aurora Imaging Library objects are depth maps, calculations are completed more quickly. The speed is further increased if both depth maps use the same calibration context.

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the absolute distance to the position in the depth map that is directly above or below the source along the Z-axis.  The returned value is always positive.  If a point would be projected outside of the depth map, the point is not used in statistics calculations. |
| `M_SIGNED_DISTANCE_Z_TO_SURFACE` | Specifies to calculate the signed distance using the Z-coordinate of the corresponding pixel of the reference depth map. That is, the distance is calculated along the Z-axis.  Distance measurements are positive for points above the depth map, and negative for points below the depth map.  If a point would be projected outside of the depth map, the point is not used in statistics calculations. |

---

### `Point cloud container identifier`

Specifies a point cloud container, allocated with [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md).  The function runs much slower when the reference Aurora Imaging Library object is a point cloud container. If speed is an issue, you can specify a range for calculating distance measurements using the[`Condition`](../../Reference/3dmet/M3dmetStat.md), [`CondHigh`](../../Reference/3dmet/M3dmetStat.md) and [`CondLow`](../../Reference/3dmet/M3dmetStat.md)parameters. Any distance measurements outside the specified range are not used in statistics calculations.  You can also project the reference point cloud into a fully corrected depth map first (using [`M3dimProject`](../../Reference/3dim/M3dimProject.md)), at the cost of precision.

| Value | Description |
| --- | --- |
| `M_DISTANCE_TO_MESH` | Specifies to calculate the shortest distance to the reference object's mesh.  This calculation requires that the reference object have a mesh component. |
| `M_DISTANCE_TO_NEAREST_NEIGHBOR` | Specifies to calculate the distance to the nearest point in the reference point cloud. |
