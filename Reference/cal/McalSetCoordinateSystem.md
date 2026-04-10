---
doctype: Reference
module: cal
function: McalSetCoordinateSystem
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalSetCoordinateSystem"
---

# McalSetCoordinateSystem

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

> Change the position and orientation of a coordinate system.

## Syntax

```c
void McalSetCoordinateSystem(
    AIL_ID     CalibratedAilObjectId,      //out
    AIL_INT64  TargetCoordinateSystem,     //in
    AIL_INT64  ReferenceCoordinateSystem,  //in
    AIL_INT64  TransformType,              //in
    AIL_ID     ArrayBufOrMatrix3dgeoId,    //in
    AIL_DOUBLE Param1,                     //in
    AIL_DOUBLE Param2,                     //in
    AIL_DOUBLE Param3,                     //in
    AIL_DOUBLE Param4                      //in
)
```

## Description

This function moves a specified (target) coordinate system in relation to a specified (reference) coordinate system. For example, you can move the relative coordinate system by (0, 6, 0) in the absolute coordinate system. This will displace the origin of the relative coordinate system by 6 units along the Y-axis of the absolute coordinate system. You can call this function to move the relative coordinate system of a 3D-based camera calibration context, calibrated image, or 3D reconstruction result buffer of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md). Note that to return the position and orientation of one coordinate system as a transformation of another coordinate system, you can use [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md).

If you are moving the relative coordinate system of an image that has been corrected, the XY-plane of the relative coordinate system should not be moved in the Z-direction and should not be rotated around its X or Y-axis. In other words, the relative coordinate system of a corrected image can only be translated along its X or Y-axis or rotated around its Z-axis. Moving the relative coordinate system of an image is useful for analyzing an object using a temporary local coordinate system. You can also use [`McalFixture`](../../Reference/cal/McalFixture.md) to move the relative coordinate system with respect to a result. If you move the relative coordinate system, results returned in world units from other modules are returned with respect to the relative coordinate system's new position, and settings which accept input in world units will accept that input with respect to the relative coordinate system's new position.

If you transform the camera or tool coordinate system, Aurora Imaging Library assumes that the camera has been moved in your camera setup, so it adjusts the camera calibration mapping between the absolute coordinate system and the pixel coordinate system.

You can also use this function to transform the relative coordinate system of a 3D reconstruction result buffer of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md), which [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) uses to express world coordinates. To transform the relative coordinate system of a result buffer, [`TargetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) must be set to [`M_RELATIVE_COORDINATE_SYSTEM`](../../Reference/cal/McalSetCoordinateSystem.md) and [`ReferenceCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) must be set to [`M_RELATIVE_COORDINATE_SYSTEM`](../../Reference/cal/McalSetCoordinateSystem.md) or [`M_ABSOLUTE_COORDINATE_SYSTEM`](../../Reference/cal/McalSetCoordinateSystem.md).

Note that all angles should be given in degrees. However, unlike most other Aurora Imaging Library functions (including [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md)), angles are interpreted using the right-hand grip rule around the axis of rotation; if you wrap your right hand around the axis of rotation, pointing your thumb in the positive direction of the axis, your fingers wrap in the direction of rotation. For example, a positive rotation around the Z-axis corresponds to a rotation that turns the positive X-axis toward the positive Y-axis.

You can specify whether the transformation of the target coordinate system is applied from the current position ([`M_COMPOSE_WITH_CURRENT`](../../Reference/cal/McalSetCoordinateSystem.md)) of the target coordinate system or from the origin of the reference coordinate system ([`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md)).

*[Image: cal_CS_translation_assign.png]*

> **Note:** Note that you cannot move the pixel coordinate system nor the absolute coordinate system.

If you adjust the coordinate system of a calibrated image associated with an [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, the raster information will be discarded, causing the ROI to become an [`M_VECTOR`](../../Reference/buf/MbufInquire.md) ROI. See [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) for more information.

## Parameters

### `CalibratedAilObjectId` *(out, AIL_ID)*

Specifies the identifier of a 3D-based camera calibration context, 3D reconstruction result buffer of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md), or a calibrated image.

### `TargetCoordinateSystem` *(in, AIL_INT64)*

Specifies the coordinate system on which to apply the transformation. This parameter can be set to one of the following.

*For specifying the target coordinate system*

| Value | Description |
| --- | --- |
| `M_CAMERA_COORDINATE_SYSTEM` | Specifies to apply the transformation to the camera coordinate system. The origin of the camera coordinate system corresponds to the effective pinhole of the modeled camera and the Z-axis of the camera coordinate system points in the direction that the camera is facing.

By default, for an [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_MOVING_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), a rigid link exists between the tool and camera coordinate systems such that moving one automatically moves the other accordingly. This can be temporarily broken using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md). |
| `M_RELATIVE_COORDINATE_SYSTEM` | Specifies to apply the transformation to the relative coordinate system. The relative coordinate system defines the world plane in which results are measured. By default, it corresponds to the absolute coordinate system. |
| `M_ROBOT_BASE_COORDINATE_SYSTEM` | Specifies to apply the transformation to the robot base coordinate system. The origin of this coordinate system is positioned at the base of the robot holding the camera. This coordinate system is defined as the origin of the robot encoders. If the encoders of the robot all indicate 0, it means that the tool coordinate system is at the same position and orientation as the robot base coordinate system.

This coordinate system can only be moved when [`CalibratedAilObjectId`](../../Reference/cal/McalSetCoordinateSystem.md) is set to a camera calibration context of type [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) that you have successfully calibrated, or after using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md).

The only way to change the position and orientation of this coordinate system is when you reset the encoders on a mobile robot. |
| `M_TOOL_COORDINATE_SYSTEM` | Specifies to apply the transformation to the tool coordinate system. The tool coordinate system is used to position the camera. Though this coordinate system can be used to move the camera, it need not be associated with the real camera position. By default, its axes are parallel to the absolute coordinate system, and its origin is the same as that of the absolute coordinate system.

By default, for an [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_MOVING_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), a rigid link exists between the tool and camera coordinate systems such that moving one automatically moves the other accordingly. This can be temporarily broken using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md). |

### `ReferenceCoordinateSystem` *(in, AIL_INT64)*

Specifies the reference coordinate system. The reference coordinate system must be defined before calling this function. This parameter can be set to one of the following values.

*For specifying the reference coordinate system*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE_COORDINATE_SYSTEM` *(default)* | Specifies to use the absolute coordinate system as a reference coordinate system for the transformation. The absolute coordinate system is unmovable and is the coordinate system from which all other world coordinate systems are defined. By default, the origin of the absolute coordinate system corresponds to the center of the top-left calibration point when using [`McalGrid`](../../Reference/cal/McalGrid.md). |
| `M_CAMERA_COORDINATE_SYSTEM` | Specifies to use the camera coordinate system as a reference coordinate system for the transformation. The camera coordinate system's origin corresponds to the effective pinhole of the modeled camera and its Z-axis points in the direction that the camera is facing. |
| `M_RELATIVE_COORDINATE_SYSTEM` | Specifies to use the relative coordinate system as a reference coordinate system for the transformation. The relative coordinate system defines the world plane in which results are returned. By default, it corresponds to the absolute coordinate system. The relative coordinate system can be recentered and/or re-oriented using either [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) or [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md). |
| `M_ROBOT_BASE_COORDINATE_SYSTEM` | Specifies to use the robot base coordinate system as a reference coordinate system for the transformation. This coordinate system is defined as the origin of the robot encoders. If the encoders of the robot all indicate 0, it means that the tool coordinate system is at the same position and orientation as the robot base coordinate system.

This coordinate system can only used as a reference when [`CalibratedAilObjectId`](../../Reference/cal/McalSetCoordinateSystem.md) is set to an [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) that you have successfully calibrated, or after using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md).

The position and orientation of this coordinate system are only changed when resetting the encoders on a mobile robot. |
| `M_TOOL_COORDINATE_SYSTEM` | Specifies to use the tool coordinate system as a reference coordinate system for the transformation. The tool coordinate system is used to position the camera. By default, its axes are parallel to the absolute coordinate system, and its origin is the same as that of the absolute coordinate system. |

### `TransformType` *(in, AIL_INT64)*

Specifies the type of transformation to apply to the target coordinate system.

### `ArrayBufOrMatrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of an Aurora Imaging Library array buffer that contains the transformation matrix to use, or specifies the identifier of a transformation matrix object. The expected transformation matrix is dependent on the type of transformation selected.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the transformation. Its definition is dependent on the type of transformation selected.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the transformation. Its definition is dependent on the type of transformation selected.

### `Param3` *(in, AIL_DOUBLE)*

Specifies an attribute of the transformation. Its definition is dependent on the type of transformation selected.

### `Param4` *(in, AIL_DOUBLE)*

Specifies an attribute of the transformation. Its definition is dependent on the type of transformation selected.

## Parameter Associations

### For specifying the transformation type

---

### `M_HOMOGENEOUS_MATRIX`

Specifies to apply a translation, a rotation, or both to the target coordinate system. Specify the transformation using a `4x4` homogenous matrix.

---

### `M_IDENTITY`

Specifies to apply the identity transformation to the target coordinate system. When used with the [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md) combination constant, this operation transforms the target coordinate system so that it has the same position and orientation as the reference coordinate system.

---

### `M_ROTATION_AXIS_ANGLE`

Specifies to apply a rotation operation described by an axis and angle of rotation. The axis of rotation is defined by a vector. The angle of rotation is measured in the counter-clockwise direction around the axis of rotation.  *[Image: calAngleOfRotation.png]*

---

### `M_ROTATION_AXIS_X`

Specifies to apply a rotation operation on the target coordinate system which corresponds to the rotation that aligns the X-axis of the reference coordinate system with the specified vector.

---

### `M_ROTATION_AXIS_Y`

Specifies to apply a rotation operation on the target coordinate system which corresponds to the rotation that aligns the Y-axis of the reference coordinate system with the specified vector.

---

### `M_ROTATION_AXIS_Z`

Specifies to apply a rotation operation on the target coordinate system which corresponds to the rotation that aligns the Z-axis of the reference coordinate system with the specified vector.

---

### `M_ROTATION_MATRIX`

Specifies to apply a rotation operation that is described by a `3x3` rotation matrix.  > **Note:** Note that, if using a transformation matrix, the translation part of the matrix is ignored (no error is reported if it contains a translation).

---

### `M_ROTATION_QUATERNION`

Specifies to apply a rotation operation that is described by a rotation quaternion.

---

### `M_ROTATION_X`

Specifies to apply a rotation operation that is described by a rotation around the X-axis.

---

### `M_ROTATION_XYZ`

Specifies to apply a rotation operation that is described by three distinct rotations about the axes of the reference coordinate system in the following order: a rotation about the X-axis, a rotation about the Y-axis, and a rotation about Z-axis rotation.

---

### `M_ROTATION_XZY`

Specifies to apply a rotation operation that is described by three distinct rotations about the axes of the reference coordinate system in the following order: a rotation about the X-axis, a rotation about the Z-axis, and a rotation about the Y-axis rotation.

---

### `M_ROTATION_Y`

Specifies to apply a rotation operation that is described by a rotation about the Y-axis.

---

### `M_ROTATION_YXZ`

Specifies to apply a rotation operation that is described by three distinct rotations about the axes of the reference coordinate system in the following order: a rotation about the Y-axis, a rotation about the X-axis, and a rotation about Z-axis rotation. This is also known as roll-pitch-yaw rotation.

---

### `M_ROTATION_YZX`

Specifies to apply a rotation operation that is described by three distinct rotations about the axes of the reference coordinate system in the following order: a rotation about the Y-axis, a rotation about the Z-axis, and a rotation about the X-axis rotation.

---

### `M_ROTATION_Z`

Specifies to apply a rotation operation that is described by a rotation about the Z-axis.

---

### `M_ROTATION_ZXY`

Specifies to apply a rotation operation that is described by three distinct rotations about the axes of the reference coordinate system in the following order: a rotation about the Z-axis, a rotation about the X-axis, and a rotation about the Y-axis rotation.

---

### `M_ROTATION_ZYX`

Specifies to apply a rotation operation that is described by three distinct rotations about the axes of the reference coordinate system in the following order: a rotation about the Z-axis, a rotation about the Y-axis, and a rotation about the X-axis rotation.

---

### `M_TRANSLATION`

Specifies to apply a translation operation along each of the reference coordinate system's axes.

### Combination Constants — For specifying how to apply the transformation

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify how to apply the transformation.

| Value | Description |
| --- | --- |
| `M_ASSIGN` *(default)* | Specifies to assign the specified transformation to the target coordinate system from the origin and orientation of the reference coordinate system. |
| `M_COMPOSE_WITH_CURRENT` | Specifies to compose the specified transformation with the current position and orientation in the reference coordinate system. This effectively applies the transformation to the current position and orientation of the target coordinate system. Note that this constant cannot be used with an undefined coordinate system. |

This coordinate system can only be moved when [`CalibratedAilObjectId`](../../Reference/cal/McalSetCoordinateSystem.md) is set to a 3D-based camera calibration context ([`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), or [`M_MOVING_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)) that you have successfully calibrated, or after using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md).
