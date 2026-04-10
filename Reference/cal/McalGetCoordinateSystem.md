---
doctype: Reference
module: cal
function: McalGetCoordinateSystem
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalGetCoordinateSystem"
---

# McalGetCoordinateSystem

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

> Return the position and orientation of a coordinate system.

## Syntax

```c
void McalGetCoordinateSystem(
    AIL_ID       CalibratedAilObjectId,      //in
    AIL_INT64    TargetCoordinateSystem,     //in
    AIL_INT64    ReferenceCoordinateSystem,  //in
    AIL_INT64    InquireType,                //in
    AIL_ID       ArrayBufOrMatrix3dgeoId,    //out
    AIL_DOUBLE * Param1Ptr,                  //out
    AIL_DOUBLE * Param2Ptr,                  //out
    AIL_DOUBLE * Param3Ptr,                  //out
    AIL_DOUBLE * Param4Ptr                   //out
)
```

## Description

This function returns the position and orientation of one coordinate system as a transformation of another coordinate system. For example, you can use this function to inquire about the position of your camera in the absolute coordinate system; essentially, you can inquire about the origin and orientation of the camera coordinate system with respect to the absolute coordinate system. Note that to move a specified (target) coordinate system relative to a reference coordinate system, you can use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md).

Note that [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md) returns angles in degrees. However, unlike most other Aurora Imaging Library functions, this function returns angles following the right-hand grip rule around the axis of rotation; if you wrap your right hand around the axis of rotation, pointing your thumb in the positive direction of the axis, your fingers wrap in the direction of rotation. For example, a positive rotation around the Z-axis corresponds to a rotation that turns the positive X-axis toward the positive Y-axis.

## Parameters

### `CalibratedAilObjectId` *(in, AIL_ID)*

Specifies the identifier of a camera calibration context, a 3D reconstruction context of type [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md), a 3D reconstruction result buffer of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md), or any object that has camera calibration information, such as an image or a result buffer.

### `TargetCoordinateSystem` *(in, AIL_INT64)*

Specifies the coordinate system for which to inquire about its position and orientation. This parameter must be set to one of the following values.

*For specifying the target coordinate system*

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_COORDINATE_SYSTEM` | Specifies an implicit and unmovable coordinate system from which all other coordinate systems are defined. By default, it corresponds to the center of the top-left calibration point of the camera calibration grid if calibrated using [`McalGrid`](../../Reference/cal/McalGrid.md). |
| `M_CAMERA_COORDINATE_SYSTEM` | Specifies the coordinate system whose origin corresponds to the effective pinhole of the camera and whose Z-axis points in the direction that the camera is facing. This coordinate system is only defined after a successful camera calibration in [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) mode. Alternatively, you can define this coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md). |
| `M_LASER_LINE_COORDINATE_SYSTEM` | Specifies the coordinate system whose origin is on the first laser line at the point closest to the origin of the absolute coordinate system.

> **Note:** This value is only supported when [`CalibratedAilObjectId`](../../Reference/cal/McalGetCoordinateSystem.md) is set to a valid calibrated 3D reconstruction context. |
| `M_RELATIVE_COORDINATE_SYSTEM` | Specifies the coordinate system which determines the world plane used when working with world units which can be recentered and/or re-oriented using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) or [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md). By default, it corresponds to the absolute coordinate system. |
| `M_ROBOT_BASE_COORDINATE_SYSTEM` | Specifies the coordinate system whose origin is positioned at the base of the robot holding the camera. This coordinate system is only supported for [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration modes. It is defined using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md), or after a successful camera calibration with [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md). |
| `M_TOOL_COORDINATE_SYSTEM` | Specifies the coordinate system used to change the position of the camera coordinate system. Although you can use this coordinate system to move the camera coordinate system, it need not be associated with the real camera position. By default, its axes are parallel to the absolute coordinate system, and its origin is the same as that of the absolute coordinate system. |

### `ReferenceCoordinateSystem` *(in, AIL_INT64)*

Specifies the coordinate system that will be used as a reference for calculations. All returned coordinates or angles will be calculated relative to this coordinate system. This parameter must be set to one of the following values.

*For specifying the reference coordinate system*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE_COORDINATE_SYSTEM` *(default)* | Specifies an implicit and unmovable coordinate system from which all other world coordinate systems are defined. By default, the origin lies at the center of the top-left calibration point when using [`McalGrid`](../../Reference/cal/McalGrid.md). |
| `M_CAMERA_COORDINATE_SYSTEM` | Specifies the coordinate system whose origin corresponds to the effective pinhole of the camera and whose Z-axis points in the direction that the camera is facing. This coordinate system can only be used as a reference coordinate system after it has been defined, either by a successful camera calibration in [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) mode. Alternatively, you can define this coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md). |
| `M_LASER_LINE_COORDINATE_SYSTEM` | Specifies the coordinate system whose origin is on the first laser line at the point closest to the origin of the absolute coordinate system.

> **Note:** This value is only supported when [`CalibratedAilObjectId`](../../Reference/cal/McalGetCoordinateSystem.md) is set to a valid calibrated 3D reconstruction context. |
| `M_RELATIVE_COORDINATE_SYSTEM` | Specifies the coordinate system used when working in world units, which can be recentered and/or re-oriented using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) or [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md). By default, it is equivalent to the absolute coordinate system. |
| `M_ROBOT_BASE_COORDINATE_SYSTEM` | Specifies the coordinate system whose origin is positioned at the base of the robot holding the camera. This coordinate system is only supported for [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration modes. It is defined using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md), or after a successful camera calibration with [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md). |
| `M_TOOL_COORDINATE_SYSTEM` | Specifies the coordinate system used to change the position of the camera coordinate system. Although this is the coordinate system used to move the camera coordinate system, it need not be associated with the real camera position. By default, its axes are parallel to the absolute coordinate system, and its origin is the same as that of the absolute coordinate system. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of transformation to use to return the location of the target coordinate system with respect to the reference coordinate system.

### `ArrayBufOrMatrix3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the destination Aurora Imaging Library array buffer or transformation matrix object in which to return the matrix representation of the transformation required to cause the reference coordinate system to align with the target coordinate system.

### `Param1Ptr` *(out, *AIL_DOUBLE)*

Specifies the address in which to return information about the transformation required to cause the reference coordinate system to overlap or align with the target coordinate system, depending on which is requested.

### `Param2Ptr` *(out, *AIL_DOUBLE)*

Specifies the address in which to return information about the transformation required to cause the reference coordinate system to overlap or align with the target coordinate system, depending on which is requested.

### `Param3Ptr` *(out, *AIL_DOUBLE)*

Specifies the address in which to return information about the transformation required to cause the reference coordinate system to overlap or align with the target coordinate system, depending on which is requested.

### `Param4Ptr` *(out, *AIL_DOUBLE)*

Specifies the address in which to return information about the transformation required to cause the reference coordinate system to overlap or align with the target coordinate system, depending on which is requested.

## Parameter Associations

### For inquiring about the transformation parameters

Set unused parameters to [`M_NULL`](../../Reference/cal/McalGetCoordinateSystem.md).

---

### `M_HOMOGENEOUS_MATRIX`

Retrieves the transformation which makes the axes of the reference coordinate system parallel to those of the target coordinate system, and makes the origin of the reference coordinate system coincide with that of the target coordinate system. The transformation is returned as a `4x4` homogenous matrix.  Note that this transformation is also the transformation required to express target coordinates with respect to the reference coordinate system. So you can use this result to convert a coordinate in the target coordinate system to the reference coordinate system.  *[Image: Cal_homogeneous_transform.png]*

---

### `M_ROTATION_AXIS_ANGLE`

Retrieves the axis and angle of rotation which makes the axes of the reference coordinate system parallel to those of the target coordinate system. The axis of rotation is defined by a vector. The rotation angle is measured in the counter-clockwise direction around the axis of rotation, as per the right-hand rule.  *[Image: calAngleOfRotation.png]*  The axis of rotation is always normalized.  When the axes of the coordinate systems are already parallel, [`Param1Ptr`](../../Reference/cal/McalGetCoordinateSystem.md) to [`Param4Ptr`](../../Reference/cal/McalGetCoordinateSystem.md) will be set to (0, 0, 0, 0).

| Value | Description |
| --- | --- |
| `0 <= Value < 360` | Specifies the rotation angle, in degrees. |

---

### `M_ROTATION_AXIS_X`

Retrieves the normalized vector aligned with the X-axis of the reference coordinate system, expressed in the target coordinate system.

---

### `M_ROTATION_AXIS_Y`

Retrieves the normalized vector aligned with the Y-axis of the reference coordinate system, expressed in the target coordinate system.

---

### `M_ROTATION_AXIS_Z`

Retrieves the normalized vector aligned with the Z-axis of the reference coordinate system, expressed in the target coordinate system.

---

### `M_ROTATION_MATRIX`

Retrieves the rotation matrix that makes the axes of the reference coordinate system parallel to those of the target coordinate system. The transformation is returned as a `3x3` matrix.

---

### `M_ROTATION_QUATERNION`

Retrieves the components of the quaternion that defines the rotation which makes the axes of the reference coordinate system parallel to those of the target coordinate system.  The quaternion is always normalized.  When the axes of the coordinate systems are already parallel, [`Param1Ptr`](../../Reference/cal/McalGetCoordinateSystem.md) to [`Param4Ptr`](../../Reference/cal/McalGetCoordinateSystem.md) will be set to (1, 0, 0, 0).

---

### `M_ROTATION_XYZ`

Retrieves the angles of the three rotations that make the axes of the reference coordinate system parallel to those of the target coordinate system. The rotation is described by three distinct rotations about the axes of the reference coordinate system in the following order: the X-axis, Y-axis, and Z-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the Y-axis rotation, in degrees, when located between the positive Z-axis and the positive X-axis. |
| `270.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees, when located between the positive Z-axis and the negative X-axis. |
| `0.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees. |

---

### `M_ROTATION_XZY`

Retrieves the angles of the three rotations that make the axes of the reference coordinate system parallel to those of the target coordinate system. The rotation is described by three distinct rotations about the axes of the reference coordinate system in the following order: the X-axis, Z-axis, and Y-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the Z-axis rotation, in degrees, when located between the positive X-axis and the positive Y-axis. |
| `270.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees, when located between the positive X-axis and the negative Y-axis. |
| `0.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees. |

---

### `M_ROTATION_YXZ`

Retrieves the angles of the three rotations that make the axes of the reference coordinate system parallel to those of the target coordinate system. The rotation is described by three distinct rotations about the axes of the reference coordinate system in the following order: the Y-axis, X-axis, and Z-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the X-axis rotation, in degrees, when located between the positive Y-axis and the positive Z-axis. |
| `270.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees, when located between the positive Y-axis and the negative Z-axis. |
| `0.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees. |

---

### `M_ROTATION_YZX`

Retrieves the angles of the three rotations that make the axes of the reference coordinate system parallel to those of the target coordinate system. The rotation is described by three distinct rotations about the axes of the reference coordinate system in the following order: the Y-axis, Z-axis, and X-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the Z-axis rotation, in degrees, when located between the positive X-axis and the positive Y-axis. |
| `270.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees, when located between the positive X-axis and the negative Y-axis. |
| `0.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees. |

---

### `M_ROTATION_ZXY`

Retrieves the angles of the three rotations that make the axes of the reference coordinate system parallel to those of the target coordinate system. The rotation is described by three distinct rotations about the axes of the reference coordinate system in the following order: the Z-axis, X-axis, and Y-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the X-axis rotation, in degrees, when located between the positive Y-axis and the positive Z-axis. |
| `270.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees, when located between the positive Y-axis and the negative Z-axis. |
| `0.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees. |

---

### `M_ROTATION_ZYX`

Retrieves the angles of the three rotations that make the axes of the reference coordinate system parallel to those of the target coordinate system. The rotation is described by three distinct rotations about the axes of the reference coordinate system in the following order: Z-axis, Y-axis, and X-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the Y-axis rotation, in degrees, when located between the positive Z-axis and the positive X-axis. |
| `270.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees, when located between the positive Z-axis and the negative X-axis. |
| `0.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees. |

---

### `M_TRANSLATION`

Retrieves the components of the translation vector that makes the origin of the reference coordinate system coincide with the origin of the target coordinate system.
